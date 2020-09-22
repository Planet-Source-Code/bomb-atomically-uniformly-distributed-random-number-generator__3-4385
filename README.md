<div align="center">

## Uniformly Distributed Random Number Generator


</div>

### Description

This is a class called urand I wrote to generate random numbers with a perfectly even distribution. If, for example, you want a sequence of 12 random numbers from 0 to 5, this random number generator might put out 3 4 1 0 5 2 5 0 1 4 2 3 whereas a traditional random number generator might output 5 5 0 1 0 4 2 5 3 0 1 3.
 
### More Info
 
A urand object can be constructed from a start and an end value (e.g. urand x(0, 4); would generate random numbers from the list 0,1,2,3,4), a start value, end value, and increment (e.g. urand x(1, 1.3, .1); would generate random numbers from the list 1,1.1,1.2,1.3), or a vector of integers or a vector of doubles (the random number generator would randomly select numbers from the vector).

Once a urand object is primed for a certain set of numbers, it will keep generating numbers from that list until it is primed for another list. You can generate a random number in one function, pass the urand object to another function, generate some more numbers, etc. and it will still spit out numbers with perfect distribution. Also, if you prime a urand object with a vector, that vector does not need to be in scope for the random number generator to function properly. If you have any questions, comments, suggestions, etc. please e-mail me at bomb_atomically@hotmail.com.

The Rand() member function of urand returns a single random value from the set of random numbers defined by the user.

The random number generator will always exhaust the entire set of numbers defined by the user before repeating itself. This means that the generator is slightly deterministic; the last number output by the random number generator gives information about the next number it will output. While this is good for some applications, it is bad for others. Do not use this random number generator to, for example, randomly generate a binary number.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Bomb Atomically](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/bomb-atomically.md)
**Level**          |Beginner
**User Rating**    |4.0 (16 globes from 4 users)
**Compatibility**  |C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+, UNIX C\+\+
**Category**       |[Palm OS](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/palm-os__3-35.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/bomb-atomically-uniformly-distributed-random-number-generator__3-4385/archive/master.zip)





### Source Code

```
//---------------------------------------------------------------------------
#ifndef urandH
#define urandH
#include <vector.h>
//---------------------------------------------------------------------------
class urand
{
  public:
    urand();
    urand(double start, double end, double increment = 1);
    explicit urand(vector<int> NumList);
    explicit urand(vector<double> NumList);
    double Rand();
    void prime(double start, double end, double increment = 1);
    void prime(vector<int> NumList);
    void prime(vector<double> NumList);
    void reset();
  private:
    vector<double> RandNums;
    int iter;
    void error();
    void shuffle();
};
#endif
//---------------------------------------------------------------------------
#include "urand.h"
#include <stdlib.h>
#include <vector.h>
#include <iostream.h>
#include <conio.h>
//---------------------------------------------------------------------------
urand::urand()
{
  prime(0,0);
}
/******************************************************************************/
urand::urand(double start, double end, double increment)
{
  prime(start, end, increment);
}
/******************************************************************************/
urand::urand(vector<int> NumList)
{
  prime(NumList);
}
/******************************************************************************/
urand::urand(vector<double> NumList)
{
  prime(NumList);
}
/******************************************************************************/
double urand::Rand()
{
  if(iter >= RandNums.size())
    reset();
  iter++;
  return(RandNums[iter-1]);
}
/******************************************************************************/
void urand::prime(double start, double end, double increment)
{
  if(end < start || increment <= 0)
  {
    error();
    return;
  }
  randomize();
  iter = 0;
  RandNums.clear();
  for(double i = start; i <= end; i+=increment)
    RandNums.push_back(i);
  shuffle();
}
/******************************************************************************/
void urand::prime(vector<int> NumList)
{
  if(NumList.size() == 0)
    prime(0,0);
  else
  {
    RandNums.clear();
    for(int i = 0; i < NumList.size(); i++)
      RandNums.push_back(NumList[i]);
  }
  shuffle();
}
/******************************************************************************/
void urand::prime(vector<double> NumList)
{
  if(NumList.size() == 0)
    prime(0,0);
  else
    RandNums = NumList;
  shuffle();
}
/******************************************************************************/
void urand::reset()
{
  iter = 0;
  shuffle();
}
/******************************************************************************/
void urand::error()
{
  cout << "Invalid use of urand object.\n";
  getch();
}
/******************************************************************************/
void urand::shuffle()
{
  randomize();
  int RandPos;
  double dummy;
  for(int i = 0; i < RandNums.size(); i++)
  {
    RandPos = rand()%RandNums.size();
    dummy = RandNums[i];
    RandNums[i] = RandNums[RandPos];
    RandNums[RandPos] = dummy;
  }
}
/*
//Example Code
int main(int argc, char* argv[])
{
  urand x(0, 15);
  for(int i = 0; i < 16; i++)
    cout << x.Rand() << " ";
  cout << endl << endl;
  x.prime(5, 9);
  for(int i = 0; i < 5; i++)
    cout << x.Rand() << " ";
  cout << endl << endl;
  cout << x.Rand() << " " << x.Rand() << " " << x.Rand() << " ";
  x.reset();
  cout << x.Rand() << " " << x.Rand() << " " << x.Rand() << " ";
  cout << endl << endl;
  x.prime(4, 5, .1);
  for(int i = 0; i < 11; i++)
    cout << x.Rand() << " ";
  cout << endl << endl;
  vector<int> nums(10);
  for(int i = 0; i < 10; i++)
    nums[i] = i*i;
  x.prime(nums);
  for(int i = 0; i < 10; i++)
    cout << x.Rand() << " ";
  cout << endl << endl;
  getch();
  return 0;
}
*/
```

