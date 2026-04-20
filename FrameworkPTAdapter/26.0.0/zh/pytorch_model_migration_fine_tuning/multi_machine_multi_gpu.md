# 多机多卡场景

## 环境准备

准备集群环境时，集群中的各个单机需要按照[环境准备](standalone_gpu.md)部署好环境，确保单机可以正常运行。

此外还需要按照如下操作进行集群配置。

> [!CAUTION]
>
> - 分布式训练场景下，HCCL会使用Host服务器的部分端口进行集群信息收集，需要操作系统预留该部分端口。HCCL建议使用60000-60015端口，也可以通过环境变量HCCL\_IF\_BASE\_PORT指定Host网卡起始端口，此场景下需要预留以指定端口起始的16个端口。如果用户在物理机场景训练，则需要在物理机上配置以下命令；若是在容器场景训练，则需要在容器中配置以下命令。
>     - 若操作系统端口号预留仅需临时生效，可执行如下命令，临时生效的配置重启后会失效：
>
>         ```shell
>         sysctl -w net.ipv4.ip_local_reserved_ports=60000-60015
>         ```
>
>     - 若操作系统端口号预留需永久生效，可执行如下操作：
>         1. 以root用户登录服务器，编辑“/etc/sysctl.conf”文件。
>
>             ```shell
>             vim /etc/sysctl.conf
>             ```
>
>         2. 在`/etc/sysctl.conf`文件末尾加上`net.ipv4.ip_local_reserved_ports=60000-60015`，保存并退出。
>         3. 执行如下命令使配置生效。
>
>             ```shell
>             sysctl -p
>             ```
>
> - 集合通信仅支持1/2/4/8P粒度的分配。
> - 以下操作除模型修改外，只需配置执行一次。

1. 准备组网，以两台8卡服务器组网为例。

    通过交换机或光口直连的方式完成计算设备组网搭建。

2. 配置device IP。

    1. 在AI Server0上配置device IP，以下IP为示例。

        ```shell
        hccn_tool -i 0 -ip -s address 10.***.***.2 netmask 255.255.255.0
        hccn_tool -i 1 -ip -s address 10.***.***.3 netmask 255.255.255.0
        hccn_tool -i 2 -ip -s address 10.***.***.4 netmask 255.255.255.0
        hccn_tool -i 3 -ip -s address 10.***.***.5 netmask 255.255.255.0
        hccn_tool -i 4 -ip -s address 10.***.***.6 netmask 255.255.255.0
        hccn_tool -i 5 -ip -s address 10.***.***.7 netmask 255.255.255.0
        hccn_tool -i 6 -ip -s address 10.***.***.8 netmask 255.255.255.0
        hccn_tool -i 7 -ip -s address 10.***.***.9 netmask 255.255.255.0
        ```

    2. 在AI Server1上配置device IP，以下IP为示例。

        ```shell
        hccn_tool -i 0 -ip -s address 10.***.***.2 netmask 255.255.255.0
        hccn_tool -i 1 -ip -s address 10.***.***.3 netmask 255.255.255.0
        hccn_tool -i 2 -ip -s address 10.***.***.4 netmask 255.255.255.0
        hccn_tool -i 3 -ip -s address 10.***.***.5 netmask 255.255.255.0
        hccn_tool -i 4 -ip -s address 10.***.***.6 netmask 255.255.255.0
        hccn_tool -i 5 -ip -s address 10.***.***.7 netmask 255.255.255.0
        hccn_tool -i 6 -ip -s address 10.***.***.8 netmask 255.255.255.0
        hccn_tool -i 7 -ip -s address 10.***.***.9 netmask 255.255.255.0
        ```

        > [!NOTE]
        >
        > 配置device IP需遵守以下规则：
        > 1. 针对<term>Atlas 训练系列产品</term>，AI Server中的第0/4、1/5、2/6、3/7号device需处于同一网段，第0/1/2/3号device在不同网段，第4/5/6/7号device在不同网段；对于集群场景，各AI Server对应的位置的device需处于同一网段，AI Server0和AI Server1的0号网卡需处于同一网段、1号网卡需要在同一网段。
        > 2. 针对<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>，多台节点的NPU在同一网段即可。
        > 3. 每个IP都不能冲突，相同网段下的IP需在最后8位做区分。

    3. 使用hccn\_tool配置网络检测对象IP，从device0 - device7配置8次。

        ```shell
        hccn_tool -i 0 -netdetect -s address xx.xx.xx.xx
        ```

        **表 1** 参数说明

        |参数|说明|
        |--|--|
        |-i|设备ID，取值范围：0~7。|
        |-s address|NPU网络检测对象IP，主要用于检测NPU网口网络连接状态。该IP需确保能和NPU网口IP地址互通，通常将检测对象IP配置为与NPU网口IP地址相同网段的网关地址，训练节点会定时检测NPU网口和网关地址通信是否正常。|

    4. 执行如下命令查看网络健康状态，确保两台机器间所有卡都连通。

        ```shell
        for  i  in  {0..7}; do  hccn_tool -i $i -net_health -g; done 
        ```

        回显如下所示：

        ```shell
        net health status: Success
        ```

        **表 2** 回显说明

        |字段|说明|
        |--|--|
        |net health status|网络健康状态。状态信息：0：Success；1：Socket fail；2：Receive timeout；3：Unreachable；4：Time exceeded；5：Fault；6：Init；7：Thread error；8：Detect ip set；其它：Unknown。|

    5. 使用hccn\_tool工具验证device IP是否配置正确。
        
        1. 查询每个device的ip：

            ```shell
            hccn_tool -i 0 -ip -g
            ```

        2. 打印查询结果：

            ```shell
            ipaddr:10.***.***.2
            netmask:255.255.***.0
            ```

            返回上述打印则表示已经连通。

3. 关闭防火墙。
    - Debian系列防火墙关闭命令。

        ```shell
        ufw disable
        ```

    - openEuler系列防火墙关闭命令。

        ```shell
        systemctl stop firewalld
        ```

4. 确认交换机状态正常。

    执行以下命令，返回值不为空则正常。

    ```shell
    for i in {0..7}; do hccn_tool -i $i -lldp -g; done
    ```

5. 修改模型。可参考[多机多卡示例](#section993217526111)，将模型脚本上传至AI Server0和AI Server1任意路径下。
6. 由于主节点允许处理的并发建链数受Linux内核参数“somaxconn”与“tcp\_max\_syn\_backlog”的限制，所以，针对大规模集群组网，若“somaxconn”与“tcp\_max\_syn\_backlog”取值较小会导致部分客户端概率性提前异常退出，导致集群初始化失败。

    大规模集群组网场景下，建议用户根据集群数量适当调整“somaxconn”与“tcp\_max\_syn\_backlog”参数的值，例如：

    ```shell
    sysctl -w net.core.somaxconn=65535
    sysctl -w net.ipv4.tcp_max_syn_backlog=65535
    ```

    > [!NOTE]
    > 
    > 如果用户在物理机场景训练，则需要在物理机上配置以上命令；若是在容器场景训练，则需要在容器中配置以上命令。

7. 拉起多机多卡训练。需注意以下要点：
    - 指定参与计算节点总数（--nnodes）。
    - 指定当前节点序号（--node\_rank）。
    - 指定节点内参与计算的NPU卡数（--nproc\_per\_node）。
    - 指定主节点的ip地址（--master\_addr）和主节点端口号（--master\_port）。

8. 查看host日志。

    所有host日志统一保存在\~/ascend/log路径下，用户可以在该路径下查看每个host的device日志。

## 模型脚本配置

本节以[适配样例（DDP单NPU单进程场景）](ddp_single_npu_single_process_example.md)章节的代码为样例，为用户介绍将单卡训练脚本修改为多卡训练脚本的核心步骤。

1. 在主函数中添加如下代码。

    1. 添加分布式逻辑，即在环境变量中获取local\_rank参数。

        ```python
        local_rank = int(os.environ.get("LOCAL_RANK", 0)) 
        ```

    2. 用local\_rank自动获取device号。

        ```python
        device = torch.device(f'npu:{local_rank}')
        ```

    3. 初始化，将通信方式设置为HCCL。

        ```python
        torch.distributed.init_process_group(backend="hccl",rank=(args.node_rank)*(args.nproc_per_node) + local_rank)
        ```

2. 定义模型后，开启DDP模式。

    ```python
    model = torch.nn.parallel.DistributedDataParallel(model, device_ids=[local_rank], output_device=local_rank)
    ```

3. 在获取训练数据集后，设置train\_sampler。

    ```python
    train_sampler = torch.utils.data.distributed.DistributedSampler(train_data)
    ```

4. 将train\_sampler赋值至DataLoader中的sampler。

    ```python
    train_dataloader = DataLoader(dataset = train_data, batch_size=batch_size, sampler = train_sampler)
    ```

## 拉起训练

有5种脚本启动方式可拉起多卡训练：

- [shell脚本方式](launch_multi_gpu_train_demo.md#suctom-anchor06)
- [mp.spawn方式](launch_multi_gpu_train_demo.md#suctom-anchor07)
- [torch.distributed.launch方式](launch_multi_gpu_train_demo.md#suctom-anchor08)
- [torchrun方式](launch_multi_gpu_train_demo.md#suctom-anchor09)
- [torch\_npu\_run方式](launch_multi_gpu_train_demo.md#suctom-anchor11)**（集群场景推荐）**：此方式是torchrun在大集群场景的改进版，提升集群建链性能。

附录[拉起多卡训练脚本示例](launch_multi_gpu_train_demo.md)中，以一个简单模型脚本为样例，展示了每种拉起方式脚本代码的修改方法以及各种拉起方式的适配方法，用户可以参考学习。

> [!NOTE]
>
> - 集合通信存在如下约束：
>   - 数据并行模式中不同device上执行的计算图相同。
>   - 针对<term>Atlas 训练系列产品</term>：AllReduce和reduce\_scatter仅支持int8、int32、float16和float32数据类型。
>   - 针对<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>：AllReduce和reduce\_scatter仅支持int8、int32、float16、float32和bfloat16数据类型。
>   - 针对<term>Atlas 训练系列产品</term>，如果使用1台训练服务器（Server），要求实际参与集合通信的NPU数目只能为1/2/4/8，且0-3卡和4-7卡各为一个组网。使用2张卡或4张卡训练时，不支持跨组网创建设备集群。
>   - 针对<term>Atlas 训练系列产品</term>，在Server集群场景下（即由集群管理主节点和一组训练服务器组成训练服务器集群），要求参与集合通信的NPU数目只能为1\*n、2\*n、4\*n、8\*n（其中n为参与训练的Server个数，上限为512）。且n为2的指数倍情况下，集群性能最好，建议用户优先采用此种方式进行集群组网。
>   - 针对<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>，在Server集群场景（即由集群管理主节点和一组训练服务器组成训练服务器集群）下要求参与集合通信的NPU数目为（1\~8）\*n（其中n为参与训练的Server个数，上限为1152），其中，n为2的指数倍情况下，集群性能最好，建议用户优先采用此种方式进行集群组网。同时建议每个Server中参与集合通信的NPU数量保持一致，若不一致，会造成性能劣化。
> - 针对<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>，若用户准备进行2卡训练，可将8卡训练脚本进行改写，改为2卡训练脚本。可参见以下修改方法：
>   1. 若8卡脚本的batch\_size是单卡脚本的batch\_size的8倍，则将8卡训练时的batch\_size和learning\_rate同时除以4，作为2卡训练时的batch\_size和learning\_rate。
>   2. 将nproc\_per\_node修改为2。
> - 一个device对应执行一个训练进程，当前不支持多进程在同一个device上进行训练。

## 多机多卡示例<a id="section993217526111"></a>

以torchrun方式启动为例，通过一个简单的自定义模型，展示多机多卡的模型代码和启动脚本样例。

1. 模型脚本配置示例，以下示例以一个简单的自定义模型为例，在适配NPU时需要注意和修改的内容已加注释。<a id="li209299264310"></a>

    ```python
    import argparse
    import os
    from datetime import timedelta
    
    import torch
    import torch.distributed as dist
    import torch.nn as nn
    import torch.optim as optim
    # 导入DDP模块，用于分布式数据并行
    from torch.nn.parallel import DistributedDataParallel as DDP
    from torch.utils.data import DataLoader
    # 导入DistributedSampler模块，用于分布式训练数据采样
    from torch.utils.data.distributed import DistributedSampler
    import torch_npu
    from torchvision import datasets, transforms
    
    DATA_DIR = "./data"
    
    
    def cleanup():
        # 销毁进程组
        dist.destroy_process_group()
    
    
    class ToyModel(nn.Module):
        def __init__(self):
            super(ToyModel, self).__init__()
            self.layer1 = nn.Linear(784, 128)
            self.relu = nn.ReLU()
            self.layer2 = nn.Linear(128, 10)
    
        def forward(self, x):
            return self.layer2(self.relu(self.layer1(x)))
    
    
    def data_process(inputs, labels):
        inputs = inputs.view(-1, 784)
        labels = labels.view(-1)
        return inputs, labels
    
    
    def get_train_args():
        parser = argparse.ArgumentParser(description="command line arguments")
        parser.add_argument("--batch_size", type=int, default=64)
        parser.add_argument("--epochs", type=int, default=1)
        parser.add_argument("--learning_rate", type=float, default=0.0001)
        args = parser.parse_args()
        return args
    
    
    def train(args):
        # torchrun启动多机多卡训练时，通过环境变量获取参与训练的NPU总卡数world_size、当前进程所在NPU卡的全局序号global_rank_idx和当前进程所在NPU卡的本机序号local_rank_idx
        world_size = int(os.environ["WORLD_SIZE"])
        global_rank_idx = int(os.environ["RANK"])
        local_rank_idx = int(os.environ["LOCAL_RANK"])
    
        devices_per_node = torch.npu.device_count()
    
        # 初始化进程组，将通信方式设置为hccl
        dist.init_process_group("hccl", rank=global_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
    
        # 设定当前进程所在NPU卡序号
        torch_npu.npu.set_device(local_rank_idx)
    
        # 将模型转移到对应NPU卡上
        device_id = f"npu:{local_rank_idx}"
        model = ToyModel().to(device_id)
    
        transform = transforms.Compose([
            transforms.ToTensor(),
            transforms.Normalize((0.5,), (0.5,))
        ])
    
        # 为数据集准备分布式sampler和dataloader
        train_dataset = datasets.MNIST(root=DATA_DIR, train=True, download=True, transform=transform)
        train_sampler = DistributedSampler(train_dataset, num_replicas=devices_per_node, rank=local_rank_idx)
        train_dataloader = DataLoader(train_dataset, batch_size=args.batch_size, sampler=train_sampler)
    
        # 将交叉熵损失函数算子转移到对应NPU卡上
        criterion = nn.CrossEntropyLoss().to(device_id)
        optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)
    
         # 将模型封装为分布式数据并行训练状态
        model = DDP(model)
    
        for epoch in range(args.epochs):
            if local_rank_idx == 0:
                print(f"\nCurrent epoch: {epoch}")
    
            train_sampler.set_epoch(epoch)
    
            model.train()
    
            for i, (inputs, labels) in enumerate(train_dataloader):
                inputs, labels = data_process(inputs, labels)
               # 将数据转移到对应NPU卡上
               inputs, labels = inputs.to(device_id), labels.to(device_id)
    
                outputs = model(inputs)
                loss = criterion(outputs, labels)
    
                optimizer.zero_grad()
                loss.backward()
                optimizer.step()
    
                if local_rank_idx == 0:
                    print(f"Current step: {i}, loss: {loss.item()}")
    
        cleanup()
    
    
    def main():
        args = get_train_args()
        train(args)
    
    
    if __name__ == "__main__":
        main()
    ```

2. 启动脚本的配置样例，通过bash启动即可拉起训练，需要依次在每个节点执行：
    1. 新建启动脚本文件，例如“train\_16p\_torchrun.py”，将[1](#li209299264310)的示例代码放置其中。
    2. 在主节点的bash执行如下指令启动，其中--master\_addr为指定主节点IP地址：

        ```shell
        torchrun --nnodes 2 --node_rank 0 --nproc_per_node 8 --master_addr xxxx --master_port 12345 train_16p_torchrun.py
        ```

    3. 在从节点的bash执行如下指令启动，其中--master\_addr为指定主节点IP地址：

        ```shell
        torchrun --nnodes 2 --node_rank 1 --nproc_per_node 8 --master_addr xxxx --master_port 12345 train_16p_torchrun.py
        ```
