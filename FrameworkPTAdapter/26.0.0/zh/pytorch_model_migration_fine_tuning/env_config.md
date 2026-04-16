# 环境变量配置

在开始训练前，需要先配置训练相关环境变量，用于配置NPU上的PyTorch训练环境，一般使用shell脚本配置，具体配置步骤与示例如下：

1. <a id="custom-anchor"></a>配置环境变量的shell脚本，默认配置示例如下，用户可根据实际情况自行增加其他环境变量，具体可参考《[环境变量参考](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/environment_variable_reference/env_variable_list.md)》。

    ```shell
    #!/bin/bash
    CANN_INSTALL_PATH_CONF='/etc/Ascend/ascend_cann_install.info'  # CANN的配置文件路径
    
    if [ -f $CANN_INSTALL_PATH_CONF ]; then
        CANN_INSTALL_PATH=$(cat $CANN_INSTALL_PATH_CONF | grep Install_Path | cut -d "=" -f 2)
    else
        CANN_INSTALL_PATH="/usr/local/Ascend"  # CANN实际安装路径
    fi
    
    source ${CANN_INSTALL_PATH}/cann/set_env.sh
    
    #设置device侧日志等级为error
    msnpureport -g error -d 0
    msnpureport -g error -d 1
    msnpureport -g error -d 2
    msnpureport -g error -d 3
    msnpureport -g error -d 4
    msnpureport -g error -d 5
    msnpureport -g error -d 6
    msnpureport -g error -d 7
    #关闭device侧Event日志
    msnpureport -e disable
    #将Host日志输出到串口,0-关闭/1-开启
    export ASCEND_SLOG_PRINT_TO_STDOUT=0
    #设置默认日志级别,0-debug/1-info/2-warning/3-error
    export ASCEND_GLOBAL_LOG_LEVEL=3
    #设置Event日志开启标志,0-关闭/1-开启
    export ASCEND_GLOBAL_EVENT_ENABLE=0
    #设置是否开启taskqueue,0-关闭/1-开启/2-二级流水优化后的taskqueue
    export TASK_QUEUE_ENABLE=1
    #设置是否开启PTCopy,0-关闭/1-开启
    export PTCOPY_ENABLE=1
    #设置特殊场景是否需要重新编译,不需要修改
    export DYNAMIC_OP="ADD#MUL"
    #HCCL白名单开关,1-关闭/0-开启
    export HCCL_WHITELIST_DISABLE=1
    export HCCL_IF_IP=$(hostname -I |awk '{print $1}')
    ```

2. 在开始训练前，执行[1](#custom-anchor)配置的环境变量脚本。

    ```shell
    source env.sh # env.sh为环境变量脚本名称，用户可根据实际情况进行修改
    ```
