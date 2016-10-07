# Python装饰器
## 1. 函数作用域LEGB原则
当引用一个变量时, Python按以下顺序依次进行查找: 本地作用域(Local) → 嵌套作用域(Enclosing)*(上一层结构中的def或lambda的本地作用域)* → 全局作用域(Global) → 内置作用域(Built-in).

## 2. 闭包(工厂函数)
子函数可以父函数(嵌套函数)的作用域中的变量值,这种行为叫做闭包或工厂函数.
 
例1:工厂函数
```python
>>> def maker(n):
        def action(x):
            return x**n
        return action
>>> maker(2)
<function maker.<locals>.action at 0x7ff7df58a2f0>
#可以看到,这里返回的是一个函数. 函数执行完毕, 变量b被销毁.

>>> maker(2)(3)
9
#子函数action内, 嵌套作用域中变量n的值依然被记住, 称作闭包行为.

>>> f = maker(2)
>>> f(3)
9
# f(3)等价于 maker(2)(3)

```

## 3. 函数装饰器
### 定义

1. 一个闭包，把外层函数(父函数)当做参数然后返回一个替代版函数。
* 通过在一个函数的def语句的末尾来运行另一个函数, 把最初的函数名重新绑定到结果.

### 用法
插入逻辑以拦截对函数的调用. 思路: 装饰器返回了一个包装器, 包装器把最初的函数保持到一个封闭的作用域中:

#### 一般形式
```python
def decorator(F):                 # 位置应该在@装饰器之前
    def wrapper(*args):           # 定义包装器,位置应该在调用包装器之前
        # 使用F和参数args
        # 函数F(*args)调用原始函数
    return wrapper                # 包装器调用
    
@decorator                        # 等价于 func = decorator(func)
def func(x, y):                   # func被传递给装饰器的F
    ...
  
func(6, 7)                        # 6, 7被传递给包装器参数*args,等价于 decorator(func)(6, 7)
    
```

#### 具体使用
例2
```python
def decorator(Func):      
    def new_Func(a, b):            # 定义包装器new_decorator
        print("input", a, b)     
        return Func(a, b)
    return new_Func                # 调用包装器

@decorator # 装饰器语法糖            # decorator(square_sum)
def square_sum(a,b):        
    return a**2 + b**2

# square_sum = decorator(square_sum) # 和装饰器语法糖的作用一样
print(square_sum(2, 3))              # 等价于 decorator(square_sum)(2, 3)
``` 


