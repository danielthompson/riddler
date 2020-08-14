### Riddler Classic 4/14/2017

Original post [here](https://fivethirtyeight.com/features/how-many-bingo-cards-are-there-in-the-world/) (fivethirtyeight.com).

From Joe Vanderlans, a current constitutional problem:

> Imagine that U.S. Supreme Court nominees are only confirmed if the same party holds the presidency and the Senate. What is the expected number of vacancies on the bench in the long run?
> 
> You can assume the following:
> 
> - You start with an empty, nine-person bench.
> - There are two parties, and each has a 50 percent chance of winning the presidency and a 50 percent chance of winning the Senate in each election.
> - The outcomes of Senate elections and presidential elections are independent.
> - The length of time for which a justice serves is uniformly distributed between zero and 40 years.

### Answer

The problem is somewhat underconstrained, but with my model, in the long-term, there are approximately <strong>0.68</strong> vacancies per year.

Here's the code to simulate it:

```c++
#include <iostream>
#include <array>
#include <random>

constexpr int minTermLength = 0;
constexpr int maxTermLength = 40;

constexpr int benchSize = 9;
constexpr int years = 10000000;

constexpr int presidentElectionFrequency = 4;
constexpr int senateElectionFrequency = 2;

constexpr int numParties = 2;

std::random_device randomDevice;
std::mt19937 generator(randomDevice());

int getJusticeLength() {
   static std::uniform_int_distribution<unsigned int> distribution(minTermLength, maxTermLength);
   return distribution(generator);
}

int election() {
   static std::uniform_int_distribution<unsigned int> distribution(0, numParties - 1);
   return distribution(generator);
}

int main() {

   // the value is the number of years remaining
   // -1 means the seat is empty

   // initialize to 0
   std::array<int, benchSize> bench = { };

   int presidentParty = 0;
   int senateParty = 0;

   // start with 9 empty seats
   int totalEmptySeats = benchSize;

   for (int year = 0; year < years; year++) {

      // age the justices
      for (int seat = 0; seat < benchSize; seat++)
      {
         bench[seat]--;
      }

      // run the elections
      if (year % senateElectionFrequency == 0)
         senateParty = election();

      if (year % presidentElectionFrequency == 0)
         presidentParty = election();

      if (senateParty == presidentParty) {
         // appoint justices to any empty seats
         for (int seat = 0; seat < benchSize; seat++)
         {
            // reappoint seats if occupant retires this year
            while (bench[seat] < 0) {
               bench[seat] = getJusticeLength();
            }
         }
      };

      // count empty seats
      for (int seat = 0; seat < benchSize; seat++)
      {
         if (bench[seat] < 0) {
            totalEmptySeats++;
         }
      }
   }

   double average = (double)totalEmptySeats / (double)years;

   std::cout << "Longterm, " << average << " total empty seats per year" << std::endl;

   return 0;
}
```

