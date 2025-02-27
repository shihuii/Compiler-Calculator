# 实验五：前端编译器开发程序

## 实验环境

- Python 3.12
- pipenv

## 主要内容

- 完成词法分析、语法分析（自底向上语法分析）、语义分析、中间代码生成四部分的前端编译器开发任务；
- 方案一：按编译原理的思想设计实现一功能丰富的计算器。自定义操作数的类型、运算符的功能及优先级。

## 方法模块

1. **词法分析**
   - 任务：**将源代码分解成 `token`（标记序列）**
     - **检测和处理词法中的错误（非法字符）**
     - 检测到非法字符在终端输出错误具体信息并返回 `False`
   - 数字（十进制）：整数、小数、负数
   - 运算符：
     - 算术运算符：
       - `+` `-` `*` `/` `%`(取模) `**`(次方) `//`(整除)
     - 逻辑运算符：
       - `&`(与) `|`(或) `~`(非) `^`(异或)
       - `<<`(按位左移) `>>`(按位右移)等等
   - 函数：`sin(` `cos(` `tan(`  `ln(` `sqrt(`(根号)
   - 其它标识符：`(` `)` `.` (空格)
2. **语法分析**
   - 任务：验证输入的数学表达式是否符合语法规则。
     - 使用 **自底向上 LR 分析** 确定是否接受序列；
     - **弹窗表格展示 LR 分析过程**
     - **错误处理**：对于不合法的表达式在终端给出错误提示并返回 `False`
3. **语义分析**
   - **优化**：对输入的数学表达式进行优化；
   - **类型检查**：确保操作数和运算符之间的类型匹配
     - 以及进行必要的类型转换；
   - **语义错误**：例如除以零、未定义的操作等进行合理性检查
     - 错误处理：给出错误处理并返回 `False`
4. **中间代码生成**
   - **生成 RPN 后缀表达式**
   - 根据后缀表达式生成中间代码，返回计算结果。

## 数据格式

```python
# 用户输入：字符串（空格可有可无）
str = "sin((1<<2) + 3**4)"
```

- **词法分析**：生成元组标记列表
  - 输入：字符串
  - 输出：`return token` or `False`（错误处理）

```python
# token[0] = {'number','operator','left','right'}
#              数字     运算符     左括号  右括号
# left  - function 函数
#       - 空（普通左括号）
# right - 空（普通右括号）
# number - integer 整型
#        - float 浮点数
#        - unsigned 非负整数
# operator - arithmetic 算术运算符
#          - logical 逻辑运算符
token = [('left','function','sin('), ('left','','('),
    ('number','integer','1'), ('operator','logical','<<'),
    ('number','integer','2'), ('right','',')'),
    ('operator','arithmetic','+'), ('number','integer','3'),
    ('operator','arithmetic','**'), ('number','integer','4'),
    ('right','',')')]
```

- **语法分析（自底向上 - 算符优先分析法）**：判断是否接受序列
  - 输入：token
  - 输出：True or False

```python
# 根据算符优先关系表和文法G
# 弹窗生成 LR 分析表
# 分析表生成格式：
# https://github.com/CUCPTT/test3/blob/main/analyse.py
return True or False
```

- **语义分析 + 中间代码生成**：生成后缀表达式
  - 输入：token
  - 输出：RPN 逆波兰表达式列表

```python
RPN = ['7','5','2','+','*','3','/']
# 后缀表达式（也称为逆波兰表达式 Reverse Polish notation）
# 一种不使用括号来表示运算符优先级的数学表达式表示方法。
```

- **目标代码生成**：返回计算结果
  - 输入：RPN
  - 输出：数字数据类型 表示计算结果

## 图形界面

- ![](misc/GUI.png)

## 参考资料

- 《编译原理（第3版）》5.3 算符优先分析法
- [编译原理 简单计算器的编译器的设计与实现（附源码）](https://blog.csdn.net/hhypractise/article/details/107138566)
