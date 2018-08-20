### Riddler Classic 11/18/2016

Original post [here](https://fivethirtyeight.com/features/the-puzzle-of-the-lonesome-king/) (fivethirtyeight.com).

A coronation probability puzzle from Charles Steinhardt:

The childless King of Solitaria lives alone in his castle. Overly lonely, the king one day offers one lucky subject the chance to be prince or princess for a day. The loyal subjects leap at the opportunity, having heard tales of the opulent castle and decadent meals that will be lavished upon them. The subjects assemble on the village green, hoping to be chosen.

The winner is chosen through the following game. In the first round, every subject simultaneously chooses a random other subject on the green. (It’s possible, of course, that some subjects will be chosen by more than one other subject.) Everybody chosen is eliminated. (Not killed or anything, just sent back to their hovels.) In each successive round, the subjects who are still in contention simultaneously choose a random remaining subject, and again everybody chosen is eliminated. If there is eventually exactly one subject remaining at the end of a round, he or she wins and heads straight to the castle for fêting. However, it’s also possible that everybody could be eliminated in the last round, in which case nobody wins and the king remains alone. If the kingdom has a population of 56,000 (not including the king), is it more likely that a prince or princess will be crowned or that nobody will win?

_Extra credit_: How does the answer change for a kingdom of arbitrary size?

### Answer:

I wrote a short program to simulate the results of this process for population sizes from 1,000 to 100,000:

```c++
#include <stdlib.h>
#include <iostream>
#include <iomanip>
#include <fstream>
#include <vector>
#include <random>

int main() {

   const std::string filePath = "/Users/daniel/Documents/solitaria2.txt";
   std::ofstream logfile;
   logfile.open(filePath, std::ios::trunc);

   std::cout << "Population\tWinners\tPercentage" << std::endl;
   logfile << "Population\tWinners\tPercentage" << std::endl;

   // minimum size of population
   constexpr unsigned int minPopulation = 1000;
   constexpr unsigned int maxPopulation = 100000;
   constexpr unsigned int populationStepSize = 1000;
   constexpr unsigned int iterationsPerStep = 100;

   std::random_device randomDevice;
   std::mt19937 generator(randomDevice());
   std::uniform_int_distribution<> distribution(0);

   // placeholders per iteration
   int winners = 0;
   int losers = 0;

   for (int populationSize = minPopulation; populationSize <= maxPopulation; populationSize += populationStepSize) {

      std::vector<bool> people(populationSize, true);
      for (int j = 0; j < iterationsPerStep; j++) {

         int peopleRemaining = populationSize;

         do {
            for (int i = 0; i < peopleRemaining; i++) {
               people[distribution(generator) % peopleRemaining] = false;
            }

            int newPeopleRemaining = 0;

            for (int i = 0; i < peopleRemaining; i++) {
               newPeopleRemaining = people[i] ? newPeopleRemaining + 1 : newPeopleRemaining;
               people[i] = true;
            }

            peopleRemaining = newPeopleRemaining;

         } while (peopleRemaining > 1);

         winners = (peopleRemaining == 1) ? winners + 1 : winners;
         losers = (peopleRemaining == 0) ? losers + 1 : losers;
      }
      float percentage = winners * populationStepSize / (float)(populationSize * iterationsPerStep);

      std::cout << std::fixed;
      logfile << std::fixed;

      std::cout << populationSize << "\t" << winners << "\t";
      logfile << populationSize << "\t" << winners << "\t";

      std::cout << std::fixed;
      logfile << std::fixed;

      std::cout << std::setprecision(4);
      logfile << std::setprecision(4);

      std::cout << percentage ;
      logfile << percentage ;

      std::cout << std::endl;
      logfile << std::endl;

      logfile << std::flush;
   }
   logfile.close();
}
```


