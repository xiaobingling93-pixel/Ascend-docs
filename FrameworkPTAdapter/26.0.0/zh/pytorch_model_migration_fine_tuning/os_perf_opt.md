# OS性能优化

> [!NOTE]
>
> 在当前版本中，OS性能优化仅作为一项试用特性，此功能在后续版本中可能会有所调整或改进。请用户在使用过程中关注后续版本的迭代。

**表 1** OS性能优化点

|优化点|收益|易用性|
|--|--|--|
|tcmalloc|高|高|
|malloc使用大页|中|高|
|tmpfs使用大页|低|中|
|glibc动态库大页|高|中|

容器化部署使用说明：

1. 若容器中有权限部署，推荐在容器中部署更方便，无需关注下列说明。若容器中没有权限部署，则需要在对应物理机上部署，然后挂载到容器镜像中。（即docker run -it -v /path/to/yours:/path/to/yours /bin/bash中-v选项是指定的路径，下列说明中挂载均为此操作）
2. tcmalloc若在宿主机部署，在容器中需要将对应安装的libtcmalloc.so（可通过find /usr -name libtcmalloc.so\*搜索路径）和其依赖的动态库libunwind.so（可通过ldd /path/to/libtcmalloc.so查看当前tcmalloc所依赖libunwind的路径）都挂载到容器中。
3. malloc使用大页若在宿主机上部署，且宿主机glibc版本号低于2.34且需要使用标准大页，则需要将/dev/hugepages挂载到容器中。
4. tmpfs使用大页若在宿主机上设置，则需要将创建的tmpfs目录挂载到容器中。
5. glibc动态库大页本身对glibc版本有要求，无需额外挂载宿主机资源。

- **[高性能内存库替换](hi_perf_mem_pool_sub.md)**  

- **[大页内存相关优化](hp_opt.md)**  
