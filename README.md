## Example:

```C++
// #define RELEASE // Ignores all timez functions
#include "timez.hpp"

int main()
{
    timez::init(); // Only needs to be called once

    int x = 0;
    int y = 0;
    for (int j = 0; j < 100; ++j)
    {
        {
            timez::perf A("Calculate: x");

            for (int i = 0; i < 1000; ++i)
            {
                timez::perf loop("loop");

                x += i;
            }
        }

        {
            timez::perf B("Calculate: y");

            {
                timez::perf B("Part 1");
                y = x * 2;
            }

            {
                timez::perf B("Part 2");
                y *= y;
            }

            {
                timez::perf B("Part 3(hidden)");
                ++y;
            }
        }
    }

    // recalcTotal: Removes inaccuracies from for example loops
    timez::print(true, std::vector<std::string>{"Part 3(hidden)"});
    timez::clean();

    return 0;
}
```

## Output from example:

```
       %       Time(us)       Total(us)       Used           ID
99.82          22             2236            100            Calculate: x
└100           0              2236            100000           loop
0.17           0              4               100            Calculate: y
└100           0              2               100              Part 2
└0             0              0               100              Part 1
```

## Output with "recalcTotal" disabled:

```
                       ---   Performance   ---
       %       Time(us)       Total(us)       Used           ID
99.62          82             8201            100            Calculate: x
└100           0              2211            100000           loop
0.37           0              31              100            Calculate: y
└100           0              2               100              Part 2
└0             0              0               100              Part 1
```
