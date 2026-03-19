#  Visual Analytics for High-Dimensional Images via Dimensionality Reduction (ManiVault + plugins)

This repository contains the [ManiVault](https://github.com/ManiVaultStudio/core) core application and several plugins develop as part of or used during project of the dissertation [Visual Analytics for High-Dimensional Images via Dimensionality Reduction](https://doi.org/10.4233/uuid:0fdde46e-23cc-4984-b03b-7710a03e2b46) (2026) by Alexander Vieth.

```
git clone --recurse-submodule https://github.com/alxvth/ManiVaultPhdBundle.git
```

Plugins developed during PhD projects:
- [Spidr](https://github.com/ManiVaultStudio/SpidrPlugin) as part of [Incorporating Texture Information into Dimensionality Reduction for High-Dimensional Images](https://doi.org/10.1109/PacificVis53943.2022.00010) (2022)
- [Image & Embedding Coupling](https://github.com/ManiVaultStudio/ImageEmbeddingCoupling) as part of [Interactions for Seamlessly Coupled Exploration of High-Dimensional Images and Hierarchical Embeddings](https://doi.org/10.2312/vmv.20231227) (2023)
- [Superpixel Hierarchies and Embeddings](https://github.com/alxvth/SPH_Plugin) as part of [Manifold-Preserving Superpixel Hierarchies and Embeddings for the Exploration of High-Dimensional Images](https://doi.org/10.48550/arXiv.2602.24160) (2026)

## Building
This projects uses [vcpkg](https://github.com/microsoft/vcpkg/) to install dependencies. The [vcpkg.json](./vcpkg.json) manifest file can be used together with CMake to automate this process. 
After setting up vcpkg, you can incorporate vcpkg with CMake via the command line
```bash
> cmake -B [build directory] -S . "-DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake"
> cmake --build [build directory] 
```
or in the CMake GUI via "Specify toolchain for cross-compiling", or in your IDE's specific fashion.

When using presets, ensure to set the environment variable `VCPKG_ROOT` to the vcpkg install location, preferably a short path like `C:\vcpkg` (Windows) or `$HOME/vcpkg` (Linux).

Setup vcpkg, on Linux:
```bash
cd $HOME
sudo apt update
sudo apt install -y build-essential gfortran pkg-config zip unzip
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg && ./bootstrap-vcpkg.sh
echo -e '\n# Vcpk\nexport VCPKG_ROOT=$HOME/vcpkg' >> ~/.bashrc
source ~/.bashrc
```

You need at least [CMake](https://gitlab.kitware.com/cmake/cmake) 3.31. Your OS package manager might not provide this version yet.

Setup CMake, on Linux:
```bash
cd $HOME
sudo apt purge cmake
sudo apt install -y libssl-dev
mkdir cmake && cd cmake
mkdir cmake_install
git clone https://gitlab.kitware.com/cmake/cmake.git
mv cmake cmake_source && cd cmake_source
git checkout v3.31.11
./bootstrap --prefix=$HOME/cmake/cmake_install
make -j$(nproc) && make install
echo -e '\n# Custom cmake\nexport PATH=$HOME/cmake/cmake_install/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
