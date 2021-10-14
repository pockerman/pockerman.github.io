---
title: 'PyTorch with C++: Develop a neural network'
date: 2021-10-08
permalink: /posts/2021/10/pytorch_cpp_2/
tags:
  - pytorch
  - deep-neural-networks
  - API
  - c++
  - numerics
---

Create a simple neural network using PyTorch C++ frontend.



 
PyTorch with C++: Develop a neural network
======

<a href="https://pytorch.org/">PyTorch</a> is one of the well established libraries for modeling deep neural networks. The exposed Python API is the most commonly used one. However, the library also exposes bindings for C++. In a <a href="#">previous post</a>, I discussed how to link with the PyTorch library and explore how to use the ```torch::Tensor``` class. In this post, I continue exploring the PyTorch C++ frontend by creating a neural network.

The code snippet below is taken from the official PyTorch documentation

```
#include <torch/torch.h>

#include <iostream>
#include <vector>

namespace example
{

typedef std::size_t uint_t;

class Net: public torch::nn::Module
{

public:

	//
	Net(uint_t n, uint_t m);
	
	// forward
	torch::Tensor forward(torch::Tensor input);
	
private:

	torch::Tensor W;
	torch::Tensor b;

};

Net::Net(uint_t n, uint_t m)
:
W(),
b()
{
 W = register_parameter("W", torch::randn({n, m}));
 b = register_parameter("b", torch::randn(m));
}

torch::Tensor 
Net::forward(torch::Tensor input){
	return torch::addmm(b, input, W);
}

}

int main() {


  using namespace  example;
  
  if(torch::cuda::is_available()){
  	std::cout<<"CUDA is available on this machine"<<std::endl;
  }
  else{
  	std::cout<<"CUDA is not available on this machine"<<std::endl;
  }

  
  
  Net net(4, 5);
  for (const auto& p : net.parameters()) {
    std::cout << p << std::endl;
  }
  
  
  
  return 0;
}
```

Here is the produced output

```
CUDA is not available on this machine
 1.3898  0.6982  0.7452  1.0598  0.6164
 0.0596 -1.0829 -1.5402  0.3675 -0.1448
-1.3196 -0.0802 -1.2496  1.7304  0.6239
-1.3721 -0.1861 -0.8428 -0.2359  0.4922
[ CPUFloatType{4,5} ]
 0.4445
-0.6764
-1.1953
-0.5633
 0.0561
[ CPUFloatType{5} ]
```


References
======

- <a href="https://pytorch.org/">PyTorch</a>
- <a href="https://pytorch.org/tutorials/advanced/cpp_frontend.html">Using the PyTorch C++ Frontend</a>
