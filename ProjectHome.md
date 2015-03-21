RSDic is a C++ library for rank/select dictionary, and support rank/select operations efficiently, and space-efficient for both sparse and dense bit arrays.

The algorithm is based on the work by Navarro and Providel [link](#Bibliography.md).
RSDic combines the idea of on-the-fly decoding of Enumrative code, and utilization of rank/select samplings.
This idea is discussed as a future work in the paper.

## Install ##
```
./waf configure
./waf
./waf install (sudo)
```

## Test ##
```
./waf configure
./waf --check
```

## Usage ##
```
#include <rsdic/RSDicBuilder.hpp>
#include <rsdic/RSDic.hpp>

rsdic::RSDicBuilder rsdb;
rsdb.PushBack(1);
rsdb.PushBack(0);
...
rsdb.PushBack(1);

rsdic::RSDic rsd;
rsdb.Build(rsd);

// rsd conceptually stores the bit array B[0...] = 10...1

rsd.GetBit(12345); // return B[12345];
rsd.Rank(12345, 1); // return the number of 1's in B[0, 12345-1]
rsd.Rank(12345, 0); // return the number of 0's in B[0, 12345-1]
rsd.Select(12345, 0); // return the position of (12345+1)-th zero in B
rsd.Select(12345, 1); // return the position of (12345+1)-th one in B
```

## Quick Experience ##

  * OS: Mac OS X
  * CPU: 1.4GHz Intel Core 2 Duo
  * Mem: 2GB 1067MHz DDR3

bit length : 268435456 (2^28 = 256M)

|ones ratio | entropy | bits per orig bit	| GetBit qps  | Rank qps | Select qps  |
|:----------|:--------|:------------------|:------------|:---------|:------------|
|0.5 | 1.22133 | 1.21875 | 3.0377e+06 | 2.31205e+06 | 1.59855e+06|
|0.1 | 0.476411 | 0.646244 | 1.77234e+06 | 1.65947e+06 | 1.51413e+06|
|0.05 | 0.288248 | 0.467304 | 1.7825e+06 | 1.70708e+06 | 1.68093e+06|
|0.01 | 0.080852 | 0.276077 | 2.0757e+06 | 1.95777e+06 | 1.65173e+06|
|0.005 | 0.0453788 | 0.247993 | 2.26645e+06 | 2.04916e+06 | 1.50319e+06|
|0.001 | 0.01142 | 0.224728 | 2.42661e+06 | 2.28163e+06 | 679779|
|0.9 | 0.476411 | 0.64633 | 1.7846e+06 | 1.72547e+06 | 1.4951e+06|
|0.99 | 0.080852 | 0.276089 | 2.15698e+06 | 1.83817e+06 | 1.87332e+06|

You can reproduce this experience as follows
```
./waf configure
./waf
build/tool/PerformanceTest 268435456
```

## Bibliography ##

  * "Fast, Small, Simple Rank/Select on Bitmaps", Gonzalo Navarro and Eliana Providel, SEA 2012 [pdf](http://www.dcc.uchile.cl/~gnavarro/ps/sea12.1.pdf)