### Riddler Express 9/13/2019

Original post [here](https://fivethirtyeight.com/features/can-you-help-dakota-jones-raid-the-lost-arc/) (fivethirtyeight.com).

At long last, Dakota Jones is close to finding the Lost Arc, a geometric antiquity buried deep in the sands of Egypt. Along the way, she discovered what she described as a “highly symmetric crystal” that’s needed to precisely locate the Arc. Dakota measured the crystal using her laser scanner and relayed the results to you. But nefarious agents have gotten wind of her plans, and Dakota and the crystal are nowhere to be found.

Locating the Arc is now up to you. To do that, you must recreate the crystal using the data from Dakota’s laser scanner. The scanner takes a 3D object, and records 2D cross-sectional slices along the third dimension. Here’s the looping animation file the scanner produced for the crystal:

![scanner](jones_538.gif)

What sort of three-dimensional shape is the crystal? No pressure — Dakota Jones, nay, the entire world, is counting on you to locate the Lost Arc and ensure its place in a museum!

### Answer:

At least a **trigonal trapezohedron**, and maybe one that is a **cube**. Since no information is given on the resolution of the slices (how "thick" they are), there is no way to tell whether the faces are actually identical (if they are, it's a cube). The camera moves along a vector from one corner to the opposite corner.

I started by decomposing the gif file into its constituent frames, for a total of 301 image slices.

| ![frame 30](frames/frame030.gif)  | ![frame 60](frames/frame060.gif)  | ![frame 90](frames/frame090.gif)  |
| ![frame 120](frames/frame120.gif)  | ![frame 150](frames/frame150.gif)  | ![frame 180](frames/frame180.gif)  |
| ![frame 210](frames/frame210.gif)  | ![frame 240](frames/frame240.gif)  | ![frame 270](frames/frame270.gif)  |

Then, I wrote a short program that:

 * Samples each image slice at regular intervals
 * Generates a point in 3D space for each sampled point that's inside the shape in each slice
 * Outputs those points into a text file

```C#
using System;
using System.Collections.Generic;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using MIConvexHull;

namespace Sampler
{
   class Program
   {
      static void Main(string[] args)
      {
         // get all images in directory, sorted by name
         DirectoryInfo directory = new DirectoryInfo("/slices/");
         List<FileInfo> files = directory.GetFiles("*.gif").OrderBy(p => p.Name).ToList();
         int current = 0;
         List<Point> points = new List<Point>();
         StringBuilder pointcloudBuilder = new StringBuilder();
         int overallNumSamples = 0;

         foreach (FileInfo file in files)
         {
            Bitmap bitmap = new Bitmap(Image.FromFile(file.FullName));
            double z = current++;

            int numSamples = 0;

            for (int x = 0; x < bitmap.Width; x += 10)
            {
               for (int y = 0; y < bitmap.Height; y += 10)
               {
                  // perform sample
                  Color color = bitmap.GetPixel(x, y);

                  // add point to list, if needed
                  if (color.R < 250 || color.G < 250 || color.B < 250)
                  {
                     numSamples++;
                     points.Add(new Point(){x=x, y=y, z=z});
                     pointcloudBuilder.AppendLine($"{x} {y} {z}");
                  }
               }
            }

            overallNumSamples += numSamples;

            Console.WriteLine($"file {file.Name} had {numSamples} samples written");
         }

         double[][] vertices = new double[overallNumSamples][];

         for (int i = 0; i < vertices.Length; i++)
         {
            vertices[i] = new[] {points[i].x, points[i].y, points[i].z};
         }

         var convexHull = ConvexHull.Create(vertices);

         StringBuilder objBuilder = new StringBuilder();

         int vertexIndex = 1;

         foreach (DefaultConvexFace<DefaultVertex> face in convexHull.Result.Faces)
         {
            objBuilder.AppendLine($"v {face.Vertices[0].Position[0]} {face.Vertices[0].Position[1]} {face.Vertices[0].Position[2]}");
            objBuilder.AppendLine($"v {face.Vertices[1].Position[0]} {face.Vertices[1].Position[1]} {face.Vertices[1].Position[2]}");
            objBuilder.AppendLine($"v {face.Vertices[2].Position[0]} {face.Vertices[2].Position[1]} {face.Vertices[2].Position[2]}");
            objBuilder.AppendLine($"f {vertexIndex++} {vertexIndex++} {vertexIndex++}");
         }

         // write point list to file
         File.WriteAllText("pointcloud.txt", pointcloudBuilder.ToString());
         File.WriteAllText("convexhull.obj", objBuilder.ToString());
      }
   }

   class Point
   {
      public double x, y, z;
   }
}
```

Each image is 388x355, so I sampled in a square grid every 10 pixels. This results in a [point cloud](pointcloud.txt) in which every point is inside whatever the shape is:

```
# x y z, where x and y are image coordinates and z is the depth coordinate, for which I just used the image sequence number
190 180 4
190 180 5
200 180 5
190 180 6
200 180 6
190 180 7
200 180 7
190 170 8
190 180 8
200 180 8
190 170 9
190 180 9
200 180 9
190 170 10
190 180 10
200 180 10
190 170 11
... etc
```

Visualizing the cloud (courtesy of [LidarView](http://lidarview.com/)) looks like this:

![Point Cloud 1](pointcloud1.png)

![Point Cloud 2](pointcloud2.png)

![Point Cloud 3](pointcloud3.png)

Hmm. Looks kind of like a crystalline structure, but honestly it's hard to tell.

Next, I used [MIConvexHull](https://designengrlab.github.io/MIConvexHull/) to generate the [convex hull](https://en.wikipedia.org/wiki/Convex_hull) of my point cloud. Not only does that library output the points that make up the hull, but it also outputs the triangular faces of the hull. I outputted the hull faces to an [OBJ file](convexhull.obj) ([a very simple 3D model format](https://en.wikipedia.org/wiki/Wavefront_.obj_file)):

```
v 200 180 5
v 200 150 23
v 190 140 26
f 1 2 3
v 330 250 106
v 330 210 132
v 330 100 200
f 4 5 6
v 200 180 5
v 250 210 41
v 310 240 86
f 7 8 9
v 190 330 201
v 200 330 197
v 200 330 191
f 10 11 12
... etc
```

...and then used an online OBJ viewer to visualize it:

{% include youtube.html id="_hb1UbvVmZc" %}
