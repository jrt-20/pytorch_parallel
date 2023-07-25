# Pytorch跨节点运行（矩池云）

## 环境配置

矩池云https://matgo.cn/host-market/gpu

搭建集群，加入gpu服务器后，设置Master节点。

![image-20230725225139130](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20230725225139130.png)

添加机器成功后，系统会给每个节点分配集群 IP，当状态为已连接时，代表机器间可相互通信。这里我们还需要设置一个 master 节点，设置后会自动进行以下操作：

- 所有节点注册网卡的环境变量：NCCL_SOCKET_IFNAME、GLOO_IFACE
- 配置 master 节点到其它节点的SSH公钥登录

两台机器都可以ping通。

![image-20230725225010276](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20230725225010276.png)

![image-20230725225103993](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20230725225103993.png)

## 运行脚本

Master节点：

```
python -m torch.distributed.launch --nproc_per_node=2 --nnodes=2 --node_rank=0 --master_addr="192.168.1.2" --master_port=12345 mnmc_ddp_launch.py
```

另一个节点：

```
python -m torch.distributed.launch --nproc_per_node=2 --nnodes=2 --node_rank=1 --master_addr="192.168.1.2" --master_port=12345 mnmc_ddp_launch.py
```

运行结果

![image-20230725224712232](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20230725224712232.png)

![image-20230725225307244](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20230725225307244.png)
