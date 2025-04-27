---
title: Linux IP Parsing
date: 2025-04-27T20:30:00+05:30
---

## Save time when working with IPs
There exists a shorthand notation for ip addresses containing `0`.

If I use curl command like this `curl 10.1`, linux will automatically interpret this as a full IPv4 address by adding zero for missing segments

This works for all abbreviated formats

```
"10" → "10.0.0.0"
"10.1" → "10.0.0.1"
"10.1.2" → "10.1.0.2"
```

So next time you're testing something on localhost, instead of wasting those precious seconds you can just do `curl 127.1`


## Waste time when working with IPs

Linux utilities can also interpret IPs passed as hex or 32 bit notation.
This is because Linux actually stores IPs as 32 bit notation

So `10.0.0.1` becomes `167772161`.

ChatGPT helps us with breaking it down.

```
10.0.0.1  =  (10 × 256³) + (0 × 256²) + (0 × 256¹) + (1 × 256⁰)
          =  (10 × 16777216) + (0 × 65536) + (0 × 256) + 1
          =  167772160 + 0 + 0 + 1
          =  167772161
```

Or we can also use hex for `10.0.0.1`, that is `0xA000001`

```
0xA000001 = 167772161 (in decimal)
```


Use those saved or wasted seconds wisely!