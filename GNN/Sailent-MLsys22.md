## ACCELERATING TRAINING AND INFERENCE OF GRAPH NEURAL NETWORKS WITH FAST SAMPLING AND PIPELINING
https://research.ibm.com/publications/accelerating-training-and-inference-of-graph-neural-networks-with-fast-sampling-and-pipelining

## Motivation & Contribution
1. Provide detail time breakdown of sampling, data transfer and training computation.
2. Replace the C++ STL hash map with a adavanced one (https://abseil.io/blog/20180927-swisstables)
3. Use shared memory parallelization instead of multi-process to reduce the communication overhead within a single machine.
4. Pipeline the data transfer with computation by using different CUDA streams and reduce the redundent CPU-GPU trips which are caused by validity check.

## Implementation
1. Link: https://github.com/MITIBMxGraph/SALIENT_artifact

## Further thinking
