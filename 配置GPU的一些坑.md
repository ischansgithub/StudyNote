删除NVIDIA驱动的一切：sudo apt-get autoremove --purge nvidia-*，如果删除不了，代表CUDA没有卸载。

CUDA 卸载不掉？

- 先暴力删除：sudo rm -rf cuda-×.0/
- sudo apt-get remove cuda*

