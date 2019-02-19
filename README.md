# I2C-Log-File-Analyzer
This code analyzes I2C communication Log file
The lof file has data in the following format:
ITEM  SCL SDA
0   1   1
1   1   1
2   1   0
3   0   0
4   0   1
5   1   1
6   0   1
7   0   1
8   1   1
9   0   1
10   0   1
11   1   1
12   0   1
13   0   0
14   1   0
15   0   0
16   0   0
17   1   0
18   0   0
19   0   0
20   1   0
21   0   0
22   0   0
23   1   0
24   0   0
25   0   0
26   1   0
27   0   0
28   0   0
29   1   0
30   0   0
31   0   0
32   1   0
33   0   0
34   0   0
35   1   0
36   0   0
37   0   0
38   1   0
39   0   0
40   0   0
41   1   0
42   0   0
43   0   1
44   1   1
45   0   1
46   0   1
47   1   1
48   0   1
49   0   1
50   1   1
51   0   1
52   0   1
53   1   1
54   0   1
55   0   0
56   1   0
57   0   0
58   0   0
59   1   0
60   1   1

The Output file contains:
Total number of transactions(including NACK): 1
Toatal number of Master writes: 1
Toatal number of Master reads: 0
Toatal number of ACK transactions: 2
Toatal number of NACK transaction: 0
List of all read/write operations
    TYPE       ADDRESS     DATA
      W          0x70          0xF


