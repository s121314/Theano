# Intel Software Optimization for Theano*
---

This repo is dedicated to improving Theano performance on CPU, especially in Intel® Xeon® and Intel® Xeon Phi™ processors.

**Key Features**
  * New backend of Intel® MKL (version >= 2017.0 which includes neural network primitives)
  * Further graph optimizations
  * CPU friendly OPs
  * Switch to Intel® MKL backend automatically in Intel Architecture
  * Out-of-box performance improvements and good portability

**Benchmark**
  * Hardwares
    - Intel® Xeon® CPU E5-2699 v4 @ 2.20GHz, 128GB RAM
  * Softwares
    - Script: **[convnet-benchmarks](https://github.com/soumith/convnet-benchmarks/blob/master/theano/benchmark_imagenet.py)**
    - **[Stock Theano](https://github.com/theano/theano)**, commit ID: 2fa3cecb7572a5c884d3d3cb524ea7b7a741809a
    - **[Intel Theano](https://github.com/intel/theano)**, commit ID: 3abfbe0d1691ed953621485612c21542a3bfef95
  * Terminologies
    - FWD, forward for inference
    - FWD+BWD, forward and backward for training

   | seconds/batch | Stock/FWD | Intel/FWD | Stock/FWD+BWD | Intel/FWD+BWD |
   |---------------|-----------|-----------|---------------|---------------|
   | alexnet       | 1.045     | 0.081     | 2.333         | 0.250         |
   | googlenet     | 2.228     |           | 5.866         |               |
   | vgg           | 5.089     | 0.880     | 12.783        | 2.744         |
   | overfeat      | 6.105     | 0.268     | 13.202        | 0.857         |

**Performance Tips**
  * Add bias after convolution to archieve high performance since this sub-graph can be replaced with MKL OP 
  * Use group convolution OP, [AbstractConvGroup](https://github.com/intel/Theano/blob/master/theano/sandbox/mkl/mkl_conv.py)
  * Use New MKL OP: [LRN](https://github.com/intel/Theano/blob/dev/theano/tensor/nnet/lrn.py)

**Branch Information**
  * master, stable and fully tested version based on 0.9dev2 with Intel MKL backend
  * nomkl-optimized, based on 0.9.0dev1 with generic optimizations
  * others, experimental codes for different applications which may be merged into master and/or deleted soon

**Installation**

  * Quick Commands

    ```
    git clone https://github.com/intel/theano.git intel-theano
    cd intel-theano
    python setup.py build
    python setup.py install --user
    cp intel-theano/theanorc_icc_mkl ~/.theanorc
    # run benchmark
    democase/alexnet/benchmark.sh
    ```

  * Install Guide (recommend to go througth this document and set up optimized softwares)
    https://github.com/intel/theano/blob/master/Install_Guide.pdf


**Other Optimized Software**
  * Self-contained MKL in [here](https://github.com/01org/mkl-dnn/releases)
  * Optimized Numpy in [here](https://github.com/pcs-theano/numpy)

---
>\* Other names and trademarks may be claimed as the property of others.
