
- Date & time:  08/04/2024
- Tag: #personal
- Project:

---

## Context

These are the notes i took from the cuda workshop given by DIAM (DCSE?) on 8th and 9th of april.

## Note

### Day 1, morning: 
- Processors/ computers can be grouped into **Flynn categories**:
	- SISD (single instruction single data): CPU
	- SIMD (SImultiple data): GPU
	- MIMD: clusters, multicore, gpu clusters
- Send time is defined as time to send 1 real number (between whatever? Nodes, processors, clusters...?), 
	- **QUESTION:** but what if i send *n* real numbers, does the time to send scale proportionally? Or is it constant?
	- **ANSWER**: yes it scales proportionally, it'll take n flops to send n real numbers.
- Sending information is important to think about! 
	- GPU-CPU comms is a **slow** network ($10^5$ flops startup, $200$ flops send time), 
	- GPU-GPU, so GPU thread to GPU thread is a **fast** network ($100$ flops startup, $1$ flop send time).
- Parallel computing requires consideration of *granularity* of your algorithm:
	- **Fine grain**: high comms & high compute
	- **Coarse grain**: low comms & high compute
  And each granularity requires a different kind of network (fast or slow, gpu only vs cpu + gpu). 
- Use vectors...
- Gradient do district heating optimization, sound cool.
- Nvidia Jetson has GPU and CPU on same die/dye, so as opposed to normal computers which have super slow connection between GPU CPU, jetsons can send data a lot quicker and at higher bandwidths.
- On scientific vs gaming cards, what scientific offers which gaming doesn't:
	- ECC: error correcting checksum
	- fast double precision, gaming GPU's do it much slower and have at most good single precision.

### Day 1, afternoon


- went over the topic of mem transfer, and considerations that would affect speed of your programme (don't forget mem transfer btwn cpu gpu can be a bottleneck). The idea ofcd  latency vs bandwidth
	- for big messages, bandwidth of your mem transfer lane matters more
	- for small messages, latency matters more.
  The ideas of sending the entire message/data/array at once and splitting it into multiple messages, there is a breakeven to be found here (how small to split multiple messages). This BE point changes depending on how you "pack" the message too.
- BLAS is very cool, to perform operations using Blas usually means using some api/wrapper to access the actual subprograms (doing like numpy A@x for e.g.), but in the case of Cuda u have to access the subprograms (which actually do the linear algebra operation) manually, so call things like SGEMV for single precision generic *something* matrix vector product... All subprogrammes are named as (*upto*) [6 character functions]: XYYZZZ, where:
	- 1st character **X**, stands for number group or precision of the subprogramme, it can be:
		- S - Real
		- D - double precision
		- C - Complex
		- Z - Complex \*16 or double complex
	- 2nd & 3rd character **YY**, stands for the type of matrix(or most significant matrix), e.g.:
		- BD - bidiagonal
		- GE - general
		- HE - hermitian
		- ...
	- 4th, 5th, & 6th character **ZZZ**, stands for the computation performed.
		- MV - matrix vector product
		- MM - matrix matrix product
		- ...
### Day 2, morning

- dont forget to ask for link to slides n codebase.
### Day 2, afternoon



[6 character functions]: https://www.netlib.org/lapack/lug/node24.html