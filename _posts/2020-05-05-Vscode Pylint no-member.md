---
layout: post
---

# Vscode Pylint no-member（本答案仅适用于解决Django报错）

## 错误信息
Class 'XXX' has no 'object' member pylint(no-member)

## 使用环境
Vscode
Pylint

## 错误分析
Pylint的报错原因主要有以下两种情况：
1. object确实不存在当前成员（Pylint报错且运行程序也报错）
2. object的成员是动态添加的，只有在程序运行时此成员才会出现（Pylint报错但是程序可正常执行）

## 解决方法
### 方法一（推荐）
使用pylint-django对Pylint进行增强，让Pylint理解Django web framework

**步骤**

1. 安装pylint-django
`pip install pylint-django`

2. 在vscode的setting.json对pylint-django进行使用
```json
"python.linting.pylintArgs": [
    "--load-plugins",
    "pylint_django"
]
```

### 方法二（不推荐）
直接禁用no-member错误

**步骤**
1. 在vscode的setting.json禁用no-member错误
```json
"python.linting.pylintArgs": [
    "--disable=E1101"
]
```

### 方法三（极不推荐）
规定动态生成的成员

**步骤**
```json
"python.linting.pylintArgs": [
    "--generated-members=pandas.*"
]
```

#### 解释
以下语句是使用`pylint  --long-help`对以上方法进行的查找
```
--generated-members=<members names>
    List of members which are set dynamically and missed
    by pylint inference system, and so shouldn't trigger
    E1101 when accessed. Python regular expressions are
    accepted. [current: none]
```

#### 吐槽
请问一下CSDN的大哥们能不能不要复制粘贴，一上来就说下面语句能解决问题，我？？？
```json
"python.linting.pylintArgs": [
    "--generate-members"
]
```
你们用了之后真的解决了问题吗？我怎么用了之后Pylint什么地方都不报错了呢？
上面的语句等同于下面的语句，也能同样达到你想要的效果
```json
"python.linting.pylintArgs": [
    "--瞎粘贴就完事了，能不能用和我没有关系，反正不是我的vscode，也不是我的人生"
]
```

**错误原因**
是`--generated-members`不是`--generate-members`，少了个d，所以上面两个语句都是一样的，pylint执行的语句错误，所以pylint直接就不能用了。
