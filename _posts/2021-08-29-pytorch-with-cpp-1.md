---
title: 'PyTorch with C++ 1'
date: 2021-08-29
permalink: /posts/2021/08/pytorch_cpp_1/
tags:
  - pytorch
  - deep-neural-networks
  - API
  - c++
  - numerics
---

<a href="https://pytorch.org/">PyTorch</a> is one of the well established libraries for modeling deep neural networks. The exposed Python API is the most commonly used one. However, the library also exposes bindings for C++. In this series of notebooks, I will try to demonstrate how to use the latter. I will be following to a large extent the documentation for the C++ frontend. 

PyTorch with C++ 1
======

I will start by performing some basic manipulations with the ```Tensor``` class. Instances of this class are used around the library to perform numerics. Whilst at this I will also show how to link against the C++ bindings.

Install and link against ```libtorch```
------

To start with, download the binaries from here: https://pytorch.org/get-started/locally/. I use the pre-build binaries. Also make sure that you download the C++11 ABI.   

Let's create now the first C++ PyTorch program. The following program is taken from the official documentation.  

```
#include <torch/torch.h>
#include <iostream>

int main() {
  torch::Tensor tensor = torch::eye(3);
  std::cout << tensor << std::endl;
  return 0;
}
```

I  use the following ```CMakeLists.txt``` file to generate the needed ```Makefile```

```
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

PROJECT(example_1 VERSION 1.0.0 LANGUAGES CXX)
SET(SOURCE example_1.cpp)
SET(EXECUTABLE  example_1)

# default optionsSET(BUILD_SHARED_LIBS ON)
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_COMPILER g++)
SET(CMAKE_CXX_STANDARD 20)
SET(CMAKE_CXX_STANDARD_REQUIRED True)
SET(CMAKE_C_COMPILER gcc)
SET(CMAKE_LINKER_FLAGS "-pthread")


LIST(APPEND CMAKE_PREFIX_PATH /home/alex/MySoftware/libtorch)
FIND_PACKAGE(Torch REQUIRED CONFIG)
MESSAGE(STATUS "TORCH Include directory ${TORCH_INCLUDE_DIRS}")

MESSAGE(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
MESSAGE(STATUS "C++ Compiler: ${CMAKE_CXX_COMPILER}")
MESSAGE(STATUS "C Compiler: ${CMAKE_C_COMPILER}")


INCLUDE_DIRECTORIES(${TORCH_INCLUDE_DIRS})

ADD_EXECUTABLE(${EXECUTABLE} ${SOURCE})
TARGET_LINK_LIBRARIES(${EXECUTABLE} ${TORCH_LIBRARIES})

```

The program above produces the following output when built and executed:

```
 1  0  0
 0  1  0
 0  0  1
[ CPUFloatType{3,3} ]

```

I will now extend the example above to check on the provided API. Still, this is very elementary. Here is the updated example

```
#include <torch/torch.h>

#include <iostream>
#include <vector>

int main() {


  if(torch::cuda::is_available()){
  	std::cout<<"CUDA is available on this machine"<<std::endl;
  }
  else{
  	std::cout<<"CUDA is not available on this machine"<<std::endl;
  }

  torch::Tensor tensor = torch::eye(3);
  std::cout << tensor << std::endl;
  
  std::vector<double> data(3, 2.0);
  auto tensor_from_data_1 = torch::tensor(data);
  std::cout << tensor_from_data_1 << std::endl; 
  
  data[0] = data[1] = data[2] = 1.0;
  auto tensor_from_data_2 = torch::tensor(data);
  std::cout << tensor_from_data_2 << std::endl; 
  
  auto sum = tensor_from_data_2 + tensor_from_data_1;
  std::cout << sum << std::endl;
    
    
  if(torch::cuda::is_available()){
  
   // create a tensor and send it to the GPU
   auto cuda_tensor = torch::tensor({1.0, 2.0, 3.0}).to("cuda");
   
  }
  
  // compute element-wise product
  auto tensor1 = torch::tensor({1.0, 2.0, 3.0});
  auto product = tensor1 * tensor1;
  std::cout << product << std::endl; 
  return 0;
}
```

The output of the program above is shown below

```
CUDA is not available on this machine
 1  0  0
 0  1  0
 0  0  1
[ CPUFloatType{3,3} ]
 2
 2
 2
[ CPUFloatType{3} ]
 1
 1
 1
[ CPUFloatType{3} ]
 3
 3
 3
[ CPUFloatType{3} ]
 1
 4
 9
[ CPUFloatType{3} ]

```

The driver code above can be found in this <a href="https://github.com/pockerman/pyttoch_cpp_examples">github repository</a>.

References
======

- <a href="https://pytorch.org/">PyTorch</a>
- <a href="https://pytorch.org/tutorials/advanced/cpp_frontend.html">Using the PyTorch C++ Frontend</a>
