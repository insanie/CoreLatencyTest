# CoreLatencyTable

## What is it used for?
CoreLatencyTable is a tool designed to measure core-to-core latency of your CPU. It generates a .csv table to make measured data more comprehensive and visual.

## What's a core latency?
"Core-to-core latency" or simply "core latency" is a time needed for a certain core to pickup calculated data from another core via L3 cache (mostly). Thus, we measure latency between those two cores. Latency varies between certain cores and threads also. This depends on the chip design, its architecture, clock speed, and sometimes even RAM speed (Zen family CPUs)!

## Why does it matter?
Core latency can have a significant impact on performace of your workload depending on certain factors. For example, if you run a bunch of single-threaded apps that don't have to communicate with each other much, you will barely see any slowdown. On the other hand, if your workload consists of several threads computing a solid piece of data that needs to be checked on the way, you may see some unproportional gain in multithreading your workload (e.g. you get ~3.6 times more performance with 4 threads of the workload instead of exactly 4 times). Of course, this behavior can be caused by many factors and core latency is just one of them.

## Can I fix it?
Short answer is no, you can't. Core latency is an architectural implication of the chip design. There is overclocking and some fine tuning to compensate for that, but basically there is no software or hardware solution to make core latency lower.

## Then why should I care anyway?
To find out which CPU will satisfy your needs! Or if your current one does so. =)

# Usage

## Run the app
Close **everything** you can close in your OS to the level it's barely functional. You can even disable your network connection in order to prevent OS itself to do it's shady background stuff. Any background app will probably affect final numbers and you'll get wrong data. Or you'll have to run the test again.
Then simply run the app with the administrator rights and a console window will show up. It will show some specific data of your chip and then proceed to core-to-core tests. In the example below it says that it is a single-CPU configuration, the CPU has 8 cores and 16 threads (2 threads on each core), and 2 clusters of L3 cache - 4 cores to each cluster. As we need to provide a table of all cores to all cores latencies, the app will perform (threads count)^2 tests. In this example it is 16^2 = 256.
Do not panic if it takes some time! The more threads you have, the more time it will take... Exponentially!
![1_app_window](https://user-images.githubusercontent.com/43582428/122259007-5b9adc00-ceda-11eb-8507-f18cf07dc10c.png)

## Open latency table
After tests are completed you may find a file named **latency_table.csv** right in the directory that contains the app. Open it with a spreadsheet app of your choice. Most of the times the spreadsheet app will choose semicolon as a divider by default. If not so, you have to pick it manually.
![2_spreadsheet_table](https://user-images.githubusercontent.com/43582428/122259645-1034fd80-cedb-11eb-96ec-532d5bc4d61f.png)

## Apply conditional formatting
Actually, you can use the table right away - it shows all the measured data. But! We are here to make it more comprehencive, right? If your spreadsheet app supports conditional formatting, then do that! You need to select **only** cells with measured data, not the core number ones!
![3_conditional_formatting](https://user-images.githubusercontent.com/43582428/122260504-0790f700-cedc-11eb-906e-7111b62c1b25.png)

## Final result
![4_spreadsheet_conditional](https://user-images.githubusercontent.com/43582428/122260583-20011180-cedc-11eb-8977-46f67173b80a.png)

# How does it work
The app creates two cycles on two threads that are being measured. Those cycles share some data and perform simple math one after another. The app measures time needed for those calculations and then subtracts the time of calculations themselves. Thus, we're left only with a latency time that the core waited to pick up data manipulated by another core.
Those cycles go for **a lot** of iterations to prevent margin of error altering the result.

#### Credits to jedi95 and its initial CoreLatencyTest that made this app happen!
