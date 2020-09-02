---
layout: post
---

# SQLAlchemy学习记录

## 一、关系定义

### （一）一对多关系

onetomany

表示**一对多**的关系时，在子表类中通过 foreign key (外键)引用父表类。

然后，在父表类中通过 **relationship()** 方法来引用子表的类：

```python
class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    username = Column(String(64), nullable=False, index=True)
    password = Column(String(64), nullable=False)
    email = Column(String(64), nullable=False, index=True)
    articles = relationship('Article')

    def __repr__(self):
        return '%s(%r)' % (self.__class__.__name__, self.username)


class Article(Base):
    __tablename__ = 'articles'

    id = Column(Integer, primary_key=True)
    title = Column(String(255), nullable=False, index=True)
    content = Column(Text)
    user_id = Column(Integer, ForeignKey('users.id'))

    def __repr__(self):
        return '%s(%r)' % (self.__class__.__name__, self.title)
```

每篇文章有一个外键指向 `users` 表中的主键 `id`， 而在 `User` 中使用 SQLAlchemy 提供的 `relationship` 描述 关系。而用户与文章的之间的这个关系是双向的，所以我们看到上面的两张表中都定义了 `relationship`。

SQLAlchemy 提供了 `backref` 让我们可以只需要定义一个关系：

```python
articles = relationship('Article', backref='author')
```

添加了这个就可以不用再在 `Article` 中定义 `relationship` 了

### （二）一对一关系

**一对一**是两张表之间本质上的双向关系

要做到这一点，只需要在**一对多**关系基础上的父表中使用 **uselist** 参数来表示

```python
class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    username = Column(String(64), nullable=False, index=True)
    password = Column(String(64), nullable=False)
    email = Column(String(64), nullable=False, index=True)
    # articles = relationship('Article', backref='author')
    userinfo = relationship('UserInfo', backref='user', uselist=False)

    def __repr__(self):
        return '%s(%r)' % (self.__class__.__name__, self.username)


class UserInfo(Base):
    __tablename__ = 'userinfos'

    id = Column(Integer, primary_key=True)
    name = Column(String(64))
    qq = Column(String(11))
    phone = Column(String(11))
    link = Column(String(64))
    user_id = Column(Integer, ForeignKey('users.id'))
```

定义方法和一对多相同，只是需要添加 `userlist=False` 

### （三）多对多关系

**多对多**关系会在两个类之间增加一个关联的表。

这个关联的表在 **relationship()** 方法中通过 **secondary** 参数来表示。

通常的，这个表会通过 **MetaData** 对象来与声明基类关联，所以这个 **ForeignKey** 指令会使用链接来定位到远程的表：

定义中间表可以定义中间关系表相关的类，也可以直接通过Base.metdata生成对应关系表对象，不过基于code first准则，还是推荐将中间关系写成类。

构建第三张关系类实现多对多。

```python
article_tag = Table(
    'article_tag', Base.metadata,
    Column('article_id', Integer, ForeignKey('articles.id')),
    Column('tag_id', Integer, ForeignKey('tags.id'))
)

class Tag(Base):
    __tablename__ = 'tags'

    id = Column(Integer, primary_key=True)
    name = Column(String(64), nullable=False, index=True)

    def __repr__(self):
        return '%s(%r)' % (self.__class__.__name__, self.name)
```



## 二、增删改查

```python
from sqlalchemy import Column, String
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 连接数据库
engine = create_engine('mysql+pymysql://root:123456@localhost:3306/test')  # 创建数据库引擎

# 建立会话
DBSession = sessionmaker(bind=engine)
session = DBSession() #类似cursor

# 创建基类
Base = declarative_base()


# 创价类映射到表
class User(Base):
    __tablename__ = 'user'

    id = Column(String(20), primary_key=True)
    name = Column(String(20))


# help(session.add)

# 增加记录add
new_user = User(id='1', name='lbq')
session.add(new_user)

# 查询记录query
user1 = session.query(User).filter(User.name == 'lbq').one()

# 更改记录
print('更改操作开始')
user1.name = 'nihao'
count_lbq = session.query(User).filter(User.name == 'lbq').count()
print('姓名为lbq的数量为', count_lbq)
count_nihao = session.query(User).filter(User.name == 'nihao').count()
print('姓名为nihao的数量为', count_nihao)
print('更改操作开始')

# 删除记录delete
print('删除操作开始')
user = session.query(User).filter(User.name == 'nihao').one()
session.delete(user)
user_nihao = session.query(User).filter(User.name == 'nihao').count()
print('姓名为nihao的数量为', count_nihao)
print('删除操作结束')

session.commit()
session.close()
```



## 三、类映射到数据库

方法一

```python
from sqlalchemy import Table, MetaData, Column, Integer, String, ForeignKey
from sqlalchemy.orm import mapper
 
metadata = MetaData()
 
user = Table('user', metadata,
            Column('id', Integer, primary_key=True),
            Column('name', String(50)),
            Column('fullname', String(50)),
            Column('password', String(12))
        )
 
class User(object):
    def __init__(self, name, fullname, password):
        self.name = name
        self.fullname = fullname
        self.password = password
 
mapper(User, user) 
```



方法二

```python
import sqlalchemy
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

engine = create_engine("mysql+pymysql://用户名:密码@IP/数据库名称?charset=utf8")

Base = declarative_base() #生成orm基类，执行SQL语句的类就继承Base

class User(Base):
    __tablename__ = 'user'
    
    id = Column(Integer, primary_key=True)
    name = Column(String(32))
    password = Column(String(64))
    
    def __repr__(self): #如果想让它变的可读，只需在定义表的类下面加上这样的代码
        return "<id:%s name:%s password:%s>\n"%(self.id,self.name,self.password)
      
Base.metadata.create_all(engine) #创建表结构
#1 Base是上面定义的orm父类，metadata.create_all是他的方法
#2 engine是连接数据库的引擎
#3 执行create_all(engine)将会执行所有继承Base的语句
```



## 四、对应数据库类型

| 数据类型 | python数据类型    | 说明       |
| -------- | ----------------- | ---------- |
| Integer  | int               | 整形       |
| String   | str               | 字符串     |
| Float    | float             | 浮点型     |
| DECIMAL  | decimal.Decimal   | 定点型     |
| Boolean  | bool              | 布尔型     |
| Date     | datetime.date     | 日期       |
| DateTime | datetime.datetime | 日期和时间 |
| Time     | datetime.time     | 时间       |
| Enum     | str               | 枚举类型   |
| Text     | str               | 文本类型   |
| LongText | str               | 长文本类型 |


## 五、基础指令
检查sqlalchemy版本
```
import sqlalchemy
sqlalchemy.__version__
```

初始化数据库连接
```
from sqlalchemy import create_engine
engine = create_engine('mysql+mysqlconnector://root:password@localhost:3306/test')
```

创建DBSession类型
```
DBSession = sessionmaker(bind=engine)
```

创建对象基类
```
Base = declarative_base()
```














