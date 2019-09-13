### Riddler Express 9/13/2019

Original post [here](https://fivethirtyeight.com/features/can-you-help-dakota-jones-raid-the-lost-arc/) (fivethirtyeight.com).

At long last, Dakota Jones is close to finding the Lost Arc, a geometric antiquity buried deep in the sands of Egypt. Along the way, she discovered what she described as a “highly symmetric crystal” that’s needed to precisely locate the Arc. Dakota measured the crystal using her laser scanner and relayed the results to you. But nefarious agents have gotten wind of her plans, and Dakota and the crystal are nowhere to be found.

Locating the Arc is now up to you. To do that, you must recreate the crystal using the data from Dakota’s laser scanner. The scanner takes a 3D object, and records 2D cross-sectional slices along the third dimension. Here’s the looping animation file the scanner produced for the crystal:

![scanner](jones_538.gif)

What sort of three-dimensional shape is the crystal? No pressure — Dakota Jones, nay, the entire world, is counting on you to locate the Lost Arc and ensure its place in a museum!

### Answer:

A **cube**. The camera moves along a vector from one corner to the opposite corner.

I started by decomposing the gif file into its constituent frames, for a total of 301 image slices.

Then, I wrote a short program that:

 * Samples each image slice at regular intervals
 * Generates a point in 3D space for each sampled point that's inside the shape in each slice
 * Outputs those points into a text file

Each image is 388x355, so I sampled in a square grid every 10 pixels. This results in a point cloud in which every point is inside whatever the shape is. Visualizing the cloud (courtesy of http://lidarview.com/) looks like this:

![Point Cloud 1](pointcloud1.png)

![Point Cloud 2](pointcloud2.png)

![Point Cloud 3](pointcloud3.png)

Hmm. Looks kind of like a crystalline structure, but honestly it's hard to tell.

Next, I used [MIConvexHull](https://designengrlab.github.io/MIConvexHull/) to generate the [convex hull](https://en.wikipedia.org/wiki/Convex_hull) of my point cloud. Not only does that library output the points that make up the hull, but it also outputs the triangular faces of the hull. I outputted the hull faces to an [OBJ file](https://en.wikipedia.org/wiki/Wavefront_.obj_file) (a very simple 3D model format), and then used an online OBJ viewer to see:

{% include youtube.html id=_hb1UbvVmZc %}
