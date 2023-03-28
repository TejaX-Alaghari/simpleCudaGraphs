﻿# `SimpleCudaGraphs` Sample

The `simpleCudaGraphs` sample demonstrates the migration of CUDA Graph explicit API calls to SYCL using Taskflow programming model which manages a task dependency graph. This sample is implemented using SYCL* by migrating code from original CUDA source code and offloading computations to a GPU/CPU.

| Property               | Description
|:---                    |:---
| What you will learn    | How to begin migrating CUDA to SYCL
| Time to complete       | 15 minutes

>**Note**: This sample is based on the [simpleCudaGraphs](https://github.com/NVIDIA/cuda-samples/tree/v11.8/Samples/3_CUDA_Features/simpleCudaGraphs) sample in the NVIDIA/cuda-samples GitHub repository.


## Purpose

The sample shows the migration of simple explicit CUDA Graph API's such as cudaGraphCreate, cudaGraphAddMemcpyNode, cudaGraphClone etc, to SYCL equivalent API's using [Taskflow](https://github.com/taskflow/taskflow) programming Model. The parallel implementation demonstrates the use of CUDA Graph API's, CUDA streams, shared memory, cooperative groups and warp level primitives. 

This sample contains two versions in the following folders:

| Folder Name                   | Description
|:---                           |:---
| `01_dpct_output`              | Contains output of Intel® SYCLomatic Compatibility Tool used to migrate SYCL-compliant code from CUDA code. This SYCL code has some unmigrated code that has to be manually fixed to get full functionality. (The code does not functionally work as supplied.)
| `02_sycl_migrated`            | Contains manually migrated SYCL code from CUDA code.



## Prerequisites

| Property              | Description
|:---                   |:---
| OS                    | Ubuntu* 20.04
| Hardware              | Skylake with GEN9 or newer
| Software              | Intel® oneAPI DPC++/C++ Compiler

## Key Implementation Details

SYCL simpleCudaGraphs sample performs reduction operarion to obtain the sum value from 16777216 number of elements in two different computational kernels reduce and reduceFinal. These kernels are scheduled through taskflow which develops a simple and powerful task programming model to enable efficient implementations of heterogeneous decomposition strategies and leverages both static and dynamic task graph constructions to incorporate computational patterns.


## Set Environment Variables

When working with the command-line interface (CLI), you should configure the oneAPI toolkits using environment variables. Set up your CLI environment by sourcing the `setvars` script every time you open a new terminal window. This practice ensures that your compiler, libraries, and tools are ready for development.

## Build the `simpleCudaGraphs` Sample for CPU and GPU

> **Note**: If you have not already done so, set up your CLI
> environment by sourcing  the `setvars` script in the root of your oneAPI installation.
>
> Linux*:
> - For system wide installations: `. /opt/intel/oneapi/setvars.sh`
> - For private installations: ` . ~/intel/oneapi/setvars.sh`
> - For non-POSIX shells, like csh, use the following command: `bash -c 'source <install-dir>/setvars.sh ; exec csh'`
>
> For more information on configuring environment variables, see [Use the setvars Script with Linux* or macOS*](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-the-setvars-script-with-linux-or-macos.html).

### On Linux*

1. Change to the sample directory.
2. Build the program.
   ```
   $ mkdir build
   $ cd build
   $ cmake ..
   $ make
   ```

   By default, this command sequence will build the `02_sycl_migrated` versions of the program.

#### Troubleshooting

If an error occurs, you can get more details by running `make` with
the `VERBOSE=1` argument:
```
make VERBOSE=1
```
If you receive an error message, troubleshoot the problem using the **Diagnostics Utility for Intel® oneAPI Toolkits**. The diagnostic utility provides configuration and system checks to help find missing dependencies, permissions errors, and other issues. See the [Diagnostics Utility for Intel® oneAPI Toolkits User Guide](https://www.intel.com/content/www/us/en/develop/documentation/diagnostic-utility-user-guide/top.html) for more information on using the utility.


## Run the `simpleCudaGraphs` Sample

### On Linux

You can run the programs for CPU and GPU. The commands indicate the device target.

1. Run `02_sycl_migrated` for CPU and GPU.
    ```
    make run_cpu
    make run_gpu
    ```

### Build and Run the `simpleCudaGraphs` Sample on NVIDIA GPU

To run simpleCudaGraphs SYCL migrated sample with CUDA NVIDIA backend we use CLANG-LLVM compiler on Linux. Refer [DPC++ LLVM(CLang-LLVM)](https://www.intel.com/content/www/us/en/developer/articles/technical/compiling-sycl-with-different-gpus.html#:~:text=their%20short%20descriptions.-,DPC%2B%2B%2DLLVM%20(CLang%2DLLVM),-The%20Data%20Parallel) to build and set-up the compiler. 

#### Steps

1. Open a terminal on a Linux* system.
2. Download the samples.
    ```
    git clone https://github.com/oneapi-src/oneAPI-samples.git
    ```
3. Change to the sample directory.
4. To compile the sample following command is used,
    ```
    clang++ -fsycl -fsycl-targets=nvptx64-nvidia-cuda simpleCudaGraphs.cpp -I../../../Common/ -pthread -std=c++17 -o simplecg
    ```
5. Run the programs.
    ```
    ./simplecg
    ```

### Example Output

The following example is for `02_sycl_migrated` for GPU on **Tesla P100-PCIE-12GB**.
```
16777216 elements
threads per block  = 512
Graph Launch iterations = 3                                                                                                                                                                                                                                                                 
[syclTaskFlowManual] Host callback final reduced sum = 0.996214
[syclTaskFlowManual] Host callback final reduced sum = 0.996214
[syclTaskFlowManual] Host callback final reduced sum = 0.996214

Number of tasks(nodes) in the syclTaskFlow(graph) created manually = 7
Cloned Graph Output..
[syclTaskFlowManual] Host callback final reduced sum = 0.996214
[syclTaskFlowManual] Host callback final reduced sum = 0.996214
[syclTaskFlowManual] Host callback final reduced sum = 0.996214
Elapsed Time of SYCL TaskFlow Manual : 45.891258 (ms)
```

## License
Code samples are licensed under the MIT license. See
[License.txt](https://github.com/oneapi-src/oneAPI-samples/blob/master/License.txt) for details.

Third party program licenses are at [third-party-programs.txt](https://github.com/oneapi-src/oneAPI-samples/blob/master/third-party-programs.txt).

