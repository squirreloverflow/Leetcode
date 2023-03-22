defaultdict 是 Python 标准库 collections 模块中的一个类。它是 dict 类的子类，提供了一个带有默认值的字典。当您尝试访问一个不存在的键时，defaultdict 会自动创建该键并将其值设置为指定的默认值。这使得 defaultdict 在某些情况下比普通的字典更方便，尤其是当您需要为不存在的键提供默认值时。

defaultdict 使用一个工厂函数作为参数，该函数在创建默认值时被调用。工厂函数可以是任何无参数的可调用对象，如内置类型（如 list、set 或 int）、自定义函数或类构造函数。  

```python
from collections import defaultdict

# 使用 list 作为默认值的工厂函数
d = defaultdict(list)

# 添加键值对
d["apple"].append("red")
d["banana"].append("yellow")
d["apple"].append("green")

print(d)
# 输出: defaultdict(<class 'list'>, {'apple': ['red', 'green'], 'banana': ['yellow']})

# 访问一个不存在的键，将自动创建一个具有默认值（空列表）的条目
print(d["orange"])
# 输出: []
```

在这个例子中，我们使用 list 作为默认值的工厂函数创建了一个 defaultdict。当我们访问不存在的键时，defaultdict 会自动创建一个具有空列表作为默认值的键。

这在处理计数、分组和邻接表等场景时特别有用，因为您不需要首先检查键是否存在于字典中，然后再手动设置默认值。
