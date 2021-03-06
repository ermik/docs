---
id: version-oryOS.9-performance-hydra
title: ORY Hydra
original_id: performance-hydra
---

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	0.5230 secs
  Slowest:	0.0231 secs
  Fastest:	0.0001 secs
  Average:	0.0050 secs
  Requests/sec:	19120.8095
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [3360]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1268]	|■■■■■■■■■■■■■■■
  0.007 [1744]	|■■■■■■■■■■■■■■■■■■■■■
  0.009 [2446]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.012 [833]	|■■■■■■■■■■
  0.014 [207]	|■■
  0.016 [47]	|■
  0.019 [17]	|
  0.021 [49]	|■
  0.023 [28]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0004 secs
  50% in 0.0052 secs
  75% in 0.0079 secs
  90% in 0.0095 secs
  95% in 0.0109 secs
  99% in 0.0157 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0231 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0050 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0064 secs
  resp wait:	0.0048 secs, 0.0001 secs, 0.0198 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0047 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	18.5632 secs
  Slowest:	0.7217 secs
  Fastest:	0.0165 secs
  Average:	0.1784 secs
  Requests/sec:	538.7007
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.017 [1]	|
  0.087 [1222]	|■■■■■■■■■■■■■
  0.158 [2952]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.228 [3698]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.299 [1092]	|■■■■■■■■■■■■
  0.369 [518]	|■■■■■■
  0.440 [380]	|■■■■
  0.510 [97]	|■
  0.581 [21]	|
  0.651 [11]	|
  0.722 [8]	|


Latency distribution:
  10% in 0.0827 secs
  25% in 0.1061 secs
  50% in 0.1816 secs
  75% in 0.2154 secs
  90% in 0.3000 secs
  95% in 0.3739 secs
  99% in 0.4835 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0165 secs, 0.7217 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0093 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0679 secs
  resp wait:	0.1778 secs, 0.0165 secs, 0.7200 secs
  resp read:	0.0003 secs, 0.0000 secs, 0.0680 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.3397 secs
  Slowest:	0.0279 secs
  Fastest:	0.0001 secs
  Average:	0.0031 secs
  Requests/sec:	29433.9275
  
  Total data:	4370000 bytes
  Size/request:	437 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5758]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.006 [1963]	|■■■■■■■■■■■■■■
  0.008 [1148]	|■■■■■■■■
  0.011 [689]	|■■■■■
  0.014 [279]	|■■
  0.017 [101]	|■
  0.020 [43]	|
  0.022 [4]	|
  0.025 [11]	|
  0.028 [3]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0011 secs
  75% in 0.0052 secs
  90% in 0.0088 secs
  95% in 0.0109 secs
  99% in 0.0156 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0279 secs
  DNS-lookup:	0.0001 secs, 0.0000 secs, 0.0132 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0132 secs
  resp wait:	0.0028 secs, 0.0000 secs, 0.0279 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0114 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.3468 secs
  Slowest:	0.0260 secs
  Fastest:	0.0001 secs
  Average:	0.0032 secs
  Requests/sec:	28832.4721
  
  Total data:	4350000 bytes
  Size/request:	435 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5676]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1684]	|■■■■■■■■■■■■
  0.008 [1224]	|■■■■■■■■■
  0.010 [821]	|■■■■■■
  0.013 [389]	|■■■
  0.016 [140]	|■
  0.018 [47]	|
  0.021 [14]	|
  0.023 [1]	|
  0.026 [3]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0011 secs
  75% in 0.0055 secs
  90% in 0.0091 secs
  95% in 0.0108 secs
  99% in 0.0146 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0260 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0091 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0080 secs
  resp wait:	0.0030 secs, 0.0000 secs, 0.0253 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0060 secs

Status code distribution:
  [200]	10000 responses



```
