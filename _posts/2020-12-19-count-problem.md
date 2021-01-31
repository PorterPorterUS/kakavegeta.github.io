### Majority number

voting algorithm:

The order of each number's appearance does not matter, so we can assume that all majority appearances are placed in front, then at the last appearance, count of majority must be larger than n/2. Then we minus 1 until the end of array. Finally, count must be greater than 1.