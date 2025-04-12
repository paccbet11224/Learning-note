# python学习笔记

#### 1、node._parameters.items()的意思是：

取出当前模块 node 中所有的参数（Parameter）及其名字，以键值对（(name, parameter)）的形式返回。

#### 2、**`.items()`**：

是 Python 字典的方法，返回所有键值对：

 → `[("weight", param1), ("bias", param2)]` 这样的形式

#### 3、`res["encoder.linear.weight"] = v`

就是给字典 `res` 添加或更新一对键值对：

```
key = "encoder.linear.weight"
value = v
```

> 如果这个 key 不存在，就新增一个键值对；
>  如果 key 已经存在，就更新它的值。

#### 4、self.named_parameters().values()

它的核心是：调用 values() 方法，从字典中取出所有的值。

##### 假设 `named_parameters()` 返回的是这样的字典：

```
{
    "encoder.linear.weight": <Parameter1>,
    "encoder.linear.bias": <Parameter2>,
    "decoder.linear.weight": <Parameter3>,
}
```

那么：

```
self.named_parameters().values()
```

的结果就是：

```
[<Parameter1>, <Parameter2>, <Parameter3>]
```

#### 5、cur.__dict__["_modules"].values()

cur.__dict__["_modules"].values()
的意思其实就是 —— 取出对象 cur 的 _modules 属性中的所有 value 值（即模块实例们）。

#####  1. `cur.__dict__`

这个是 Python 的内置魔法属性，返回的是对象 `cur` 的所有 **实例属性** 组成的字典：

```
cur.__dict__  ==  {
    "_modules": {...}, 
    "_parameters": {...},
    ...
}
```

##### 2. `cur.__dict__["_modules"]`

这一步就是直接从 `__dict__` 里拿出 `_modules` 这个字段，它是一个字典，结构大概是：

```
{
    "layer1": <Module1>,
    "layer2": <Module2>,
    ...
}
```

这是 PyTorch / MiniTorch 用来**记录子模块（层）**的地方。

##### 3. `.values()`

最后我们拿这个 `_modules` 字典的 `.values()`，得到的就是：

```
[<Module1>, <Module2>, ...]
```