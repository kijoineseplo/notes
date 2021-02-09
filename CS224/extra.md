---
title: CS 224 - Computer Networks
layout: post
---

## Some extra (course related) notes

**Suggested reading/watching**:
- [Video 1](https://youtu.be/X8jsijhllIA) and [ Video 2](https://youtu.be/b3NxrZOu_CE) on hamming codes and error correction by [3b1b](https://3b1b.co)
- An [interactive website](https://harryli0088.github.io/hamming-code/) about hamming code

### Hamming codes and error correction

In the [lecture about framing and crc](framing) we studied some techniques of error detection. And if we detect an error we discarded the whole data packet and it is a waste of data, it will be much better if somehow we could correct the error, that would save us from retransmitting the data.
