
```python3
AdaLoraConfig(target_r: int,
              init_r: int,
              tinit: int,
              tfinal: int,
              deltaT: int,
              beta1: float,
              beta2: float,
              orth_reg_weight: float,
              total_step: int,
              rank_pattern: list
)
```

## target_r

int, 默认值：2，最小目标秩（调整后的最低秩），保证基础效果，设 2~4（如 r=8 则 target_rank=2）

## init_r

int，None，兼容参数（同r）,初始 rank，可覆盖 r

## tinit

int，0，秩调整起始步数，通常设置为

## tfinal

int	100000	秩调整结束步数	设为训练总步数的 80%（如总步数 10 万则设 8 万）

## deltaT

int	1	秩调整间隔步数	每 N 步调整一次秩，推荐 1~10（小数据集设 1，大数据集设 10）

## beta1

float	0.85	移动平均系数（计算重要性分数）	控制重要性分数的平滑度，0.8~0.9 之间（默认 0.85）

## beta2

float	0.85	二阶移动平均系数	辅助重要性计算，与 beta1 保持一致即可

## orth_reg_weight

float	0.5	正交正则化权重	防止低秩矩阵退化，0.1~1.0（推荐 0.5）

## total_step



## rank_pattern

