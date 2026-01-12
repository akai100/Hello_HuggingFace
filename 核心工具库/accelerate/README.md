
accelerate 库是专门为**大模型训练 / 推理加速**设计的工具库，核心目标是让开发者无需深入掌握分布式训练、混合精度、硬件适配等底层细节，就能快速实现模型在单卡 / 多卡、CPU/GPU/TPU 上的高效运行。

## accelerator

### Accelerator

Accelerator 是 accelerate 库的核心，它会自动检测硬件环境（单卡 / 多卡 / TPU）、初始化分布式训练、管理混合精度上下文，所有加速逻辑都围绕这个类展开。


## big_modeling

### cpu_offload

## data_loader

### skip_first_batches

## inference

### prepare_pippy

## launchers

### debug_launcher

### notebook_launcher

## parallelism_config

### ParallelismConfig

## state

### PartialState

## utils

### AutocastKwargs

### DataLoaderConfiguration

### DDPCommunicationHookType

### DeepSpeedPlugin

### DistributedDataParallelKwargs

### DistributedType
