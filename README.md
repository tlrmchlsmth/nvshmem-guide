Install basic dependencies:
```
sudo apt install build-essential cmake librdmacm-dev libibverbs-dev openmpi-bin libopenmpi-dev
```

Build and install NVIDIA GDRCopy
```
git clone https://github.com/NVIDIA/gdrcopy
cd gdrcopy

# Build and install
make prefix=$HOME/.local all
make prefix=$HOME/.local install
```

Build and install NVSHMEM
```
wget https://developer.nvidia.com/downloads/assets/secure/nvshmem/nvshmem_src_3.2.5-1.txz
tar xvf nvshmem_src_3.2.5-1.txz

cd nvshmem-src
mkdir build
cd build

# May need to modify MPI_HOME
MPI_HOME=/usr/mpi/gcc/openmpi-4.1.5a1 \
LDFLAGS="-L$MPI_HOME/lib -lmpi" \
cmake \
    -DCMAKE_INSTALL_PREFIX=$HOME/.local \
    -DNVSHMEM_PREFIX=$HOME/.local \
    -DCMAKE_CUDA_ARCHITECTURES=80 \
    -DNVSHMEM_MPI_SUPPORT=1 \
    -DNVSHMEM_PMIX_SUPPORT=0 \
    -DNVSHMEM_LIBFABRIC_SUPPORT=0 \
    -DNVSHMEM_IBRC_SUPPORT=1 \
    -DNVSHMEM_IBGDA_SUPPORT=1 \
    -DNVSHMEM_BUILD_TESTS=1 \
    -DNVSHMEM_BUILD_EXAMPLES=1 \
    -DNVSHMEM_BUILD_HYDRA_LAUNCHER=1 \
    -DNVSHMEM_BUILD_TXZ_PACKAGE=1 \
    -DCMAKE_CXX_COMPILER=$MPI_HOME/bin/mpic++ \
    -DCMAKE_C_COMPILER=$MPI_HOME/bin/mpicc \
    -G Ninja \
    ..

ninja build
ninja install
```
