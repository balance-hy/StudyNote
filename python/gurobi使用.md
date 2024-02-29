# Gurobi

## 获得全部解

```py
from gurobipy import *
import numpy as np
import scipy.sparse as sp

import gurobipy as gp
from gurobipy import GRB

try:

    # Create a new model
    m = gp.Model("mip1")

    # Create variables
    x = m.addVar(vtype=GRB.BINARY, name="x")
    y = m.addVar(vtype=GRB.BINARY, name="y")
    z = m.addVar(vtype=GRB.BINARY, name="z")

    # Set objective

    # Add constraint: x + 2 y + 3 z <= 4
    m.addConstr(x + 2 * y + 3 * z <= 4, "c0")

    # Add constraint: x + y >= 1
    m.addConstr(x + y >= 1, "c1")

    # 限制收集的解决方案数量
    m.setParam(GRB.Param.PoolSolutions, 1024)
    # 通过为最差情况的解设置间隙来限制搜索空间。
    # m.setParam(GRB.Param.PoolGap, 0.10)
    # 系统地搜索 k 最佳解决方案
    m.setParam(GRB.Param.PoolSearchMode, 2) # 1会尝试找到更多解，但不保证是最好的解 2会进行更系统的搜索 实测1有问题

    # Optimize model
    m.optimize()

    for i in range(m.SolCount):
        m.setParam(GRB.Param.SolutionNumber, i) # 解的编号
        print(f"Solution {i}:") # 解的数量
        print("Obj_{} = {}".format(i, m.PoolObjVal)) # 输出目标函数取值
        print(m.Xn) # 输出变量取值
        print()

    print('找到解决方案数量: ' + str(m.SolCount))

except gp.GurobiError as e:
    print('Error code ' + str(e.errno) + ': ' + str(e))

except AttributeError:
    print('Encountered an attribute error')
```

