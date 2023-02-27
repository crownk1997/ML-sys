## Accelerating Dynamic Graph Analytics on GPUs
https://dl.acm.org/doi/10.14778/3151113.3151122

## Motivation & Contribution
1. Provide a data structure which can support envolving graph analysis. Existing works are required to rebuild the whole graph.
2. Provide GPU parallel implementation of GMPA and GMPA+ (Parallel version of packed memory array)

## GPU implementation
1. Link: https://github.com/desert0616/gpma_demo
2. Memory coalsced access. Sort the update kv pairs at the beginning and different threads can have more similar traversal paths. 
3. Serilization. Avoid using atomic operations and locks. Each warp can use single thread to access the data block and use shuffle primitives to broadcast the information to the rest. Avoid the race condition within single warp. 
4. Multi-scale implementation. Based on the segment length, the authors implement warp-based (register), block-based (shared memory) and device-based (global memory) kernel to optimize the communication. 

```
// func pointer for each template
    void (*func_arr[10])(SIZE_TYPE, SIZE_TYPE, KEY_TYPE*, VALUE_TYPE*, SIZE_TYPE*, KEY_TYPE*, VALUE_TYPE*,
            SIZE_TYPE*, SIZE_TYPE*, SIZE_TYPE, SIZE_TYPE, SIZE_TYPE*);
    func_arr[0] = block_rebalancing_kernel<2, 1>;
    func_arr[1] = block_rebalancing_kernel<4, 1>;
    func_arr[2] = block_rebalancing_kernel<8, 1>;
    func_arr[3] = block_rebalancing_kernel<16, 1>;
    func_arr[4] = block_rebalancing_kernel<32, 1>;
    func_arr[5] = block_rebalancing_kernel<32, 2>;
    func_arr[6] = block_rebalancing_kernel<32, 4>;
    func_arr[7] = block_rebalancing_kernel<32, 8>;
    func_arr[8] = block_rebalancing_kernel<32, 16>;
    func_arr[9] = block_rebalancing_kernel<32, 32>;

    // operate each tree node by cuda-block
    SIZE_TYPE THREADS_NUM = update_width > 32 ? 32 : update_width;
    SIZE_TYPE BLOCKS_NUM = unique_update_size;
```
5. Make the implementation easier with Cub and Thrust. 

## Further thinking
1. In the upper level, there is less segment. How to ensure high parallelism?
2. TODO: Check the bfs code details.
