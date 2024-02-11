

# why? 
[Rocm](https://rocm.docs.amd.com/en/latest/) is AMD's answer to cuda for scientific compute. Polaris support was officially dropped, but you are still able to get preliminary support via older packaged versions, specifically for Debian(+derivatives).

Polaris cards were decently popular, and some come with 8-16GB VRAM on the *very* cheap. 

Unfortunately, If you are trying to take advantage of rocBLAS to accelerate LLMs (for example, using [koboldcpp](https://github.com/YellowRoseCx/koboldcpp-rocm) compiled for hip support), its probably not worth your time. AMD stopped compiling Rocm against Polaris, plus Polaris GPUs  don't support parallelised fp16.

Just use openCL with cblas, or if you're on windows you can experiment with the Vulkan backend on kobold/llama.cpp. I find it a little faster than cblas, though it tends to toss out memory errors after a bit, especially on the initial token processing phase.


## I still want to try!

Install ubuntu 20.04 either in [bare metal](https://www.releases.ubuntu.com/focal/) or with [docker](https://hub.docker.com/_/ubuntu?uuid=dda056e8-a659-4ddc-8770-03da1ebf6d86%0A). Please note, there is a [kernel driver](https://github.com/ROCm/ROCK-Kernel-Driver) that gets loaded by rocm for AMDGPU. So keep that in mind when running through docker, the driver won't be loaded on your host device.


## What about arch?
I could not get it to function on arch, but maybe you will be able to. In the repo you can find a pacman pkg for rocBLAS I generated from ubuntu's debs using [debtap](https://aur.archlinux.org/packages/debtap), replacing its hip deps with arch's equivalent hip-runtime-amd which supports Polaris. 

```sudo pacman -U <your pkg.tar.zst>```

Also install hip-blas.

```sudo pacman -S hip-blas```

you can get amd's OpenCL from the [AUR](https://aur.archlinux.org/packages/opencl-amd), and as nho1ix writes make sure its the correct version, as later ones have dropped support for Polaris.

I assume its simply a version mismatch error, either with hip-runtime or hip-bias, but looking deeper doesn't seem very worthwhile to me.

thank you to [xuhuisheng](https://github.com/xuhuisheng) for the .deb
