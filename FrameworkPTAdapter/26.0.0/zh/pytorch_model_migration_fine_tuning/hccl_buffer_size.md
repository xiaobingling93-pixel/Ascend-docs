# hccl\_buffer\_size

## 功能描述

此配置用于控制两个NPU之间共享数据的缓存区大小。单位为M，取值最小为1，最大无上限，默认值是200M。HCCL\_BUFFSIZE环境变量只能配置全局HCCL通信域缓存区大小，相关内容请参考[HCCL\_BUFFSIZE](HCCL_BUFFSIZE.md)。hccl\_buffer\_size配置可用于分别设置每个通信域的hccl\_buffer\_size大小。

每个通信域buffer size设置的计算方法：

- 纯SDMA通信：按照较大通信数据量/rank size配置。
- 带有RDMA通信：按照较大通信数据量配置。
- SendRecv：按照实际收发数据量配置。
- Alltoallv：按照总通信量配置\(max \(input\[rank size\], output\[rank size\]\)\)。

## 配置示例

目前存在两个torch.distributed接口可以传入该配置参数，分别是torch.distributed.init\_process\_group和torch.distributed.new\_group，示例如下：

```python
options = torch_npu._C._distributed_c10d.ProcessGroupHCCL.Options()
options.hccl_config = {"hccl_buffer_size":200}
torch.distributed.init_process_group(backend='hccl', pg_options=options)
```

```python
options = torch_npu._C._distributed_c10d.ProcessGroupHCCL.Options()
options.hccl_config = {"hccl_buffer_size":200}
torch.distributed.new_group(backend="hccl", pg_options=options)
```

## 使用场景

相较于HCCL\_BUFFSIZE环境变量只能配置全局通信域，该配置允许配置单个通信域。当业务部署内存不足时，根据功能描述中的配置策略配置，小于配置策略会导致通信性能下降。
