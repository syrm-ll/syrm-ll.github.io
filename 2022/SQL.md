<a name="gF6qG"></a>

# 约束

<a name="tuc8R"></a>

## 1. 域完整性 (列)

> 域完整性是对数据表中字段属性的约束，通常指数据的有效性,它包括字段的值域、字段的类型及字段的有效规则等约束，它是由确定关系结构时所定义的字段的属性决定的。限制数据类型,缺省值,规则,约束,是否可以为空,域完整性可以确保不会输入无效的值.。

<a name="zg66v"></a>

### 1.1 非空约束 NOT NULL

创建时设置

```sql
CREATE TABLE name
(
    id    int  NOT NULL,
    name  char NOT NULL,
    other char NULL
)
```

修改

```sql
ALTER TABLE name modify other CHAR NOT NULL;
ALTER TABLE name modify NAME CHAR NULL;
```

<a name="eh4ec"></a>

### 1.2 检查 CHECK

略
<a name="sx29f"></a>

### 1.3 默认值 DEFAULT

略
<a name="UzGXR"></a>

## 2. 实体完整性 (行)

> 实体完整性是对关系中的记录唯一性，也就是主键的约束。准确地说，实体完整性是指关系中的主属性值不能为Null且不能有相同值。定义表中的所有行能唯一的标识,一般用主键,唯一索引
> unique关键字,及identity属性比如说我们的身份证号码,可以唯一标识一个人.

<a name="CQi60"></a>

### 2.1 唯一约束 UNIQUE

```sql
#
下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 UNIQUE 约束
：

# MySQL
：

CREATE TABLE Persons
(
    P_Id      int          NOT NULL,
    LastName  varchar(255) NOT NULL,
    FirstName varchar(255),
    Address   varchar(255),
    City      varchar(255),
    UNIQUE (P_Id)
) # SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id      int          NOT NULL UNIQUE,
    LastName  varchar(255) NOT NULL,
    FirstName varchar(255),
    Address   varchar(255),
    City      varchar(255)
) # 如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法：

# MySQL / SQL Server / Oracle / MS Access：

CREATE TABLE Persons
(
    P_Id      int          NOT NULL,
    LastName  varchar(255) NOT NULL,
    FirstName varchar(255),
    Address   varchar(255),
    City      varchar(255),
    CONSTRAINT uc_PersonID UNIQUE (P_Id, LastName)
)

```

修改

```sql
#
MySQL / SQL Server / Oracle / MS Access
：

ALTER TABLE Persons
    ADD UNIQUE (P_Id) # 如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法：
# MySQL / SQL Server / Oracle / MS Access：

ALTER TABLE Persons
    ADD CONSTRAINT uc_PersonID UNIQUE (P_Id, LastName)
```

删除

```sql
#
MySQL
：
ALTER TABLE Persons
DROP INDEX uc_PersonID # SQL Server / Oracle / MS Access：
ALTER TABLE Persons
DROP
CONSTRAINT uc_PersonID
```

<a name="mTISe"></a>

### 2.2 主键约束 PRIMARY KEY

略

<a name="ChS4W"></a>

## 3. 参照完整性

> 参照完整性是对关系数据库中建立关联关系的数据表间数据参照引用的约束，也就是对外键的约束。准确地说，参照完整性是指关系中的外键必须是另一个关系的主键有效值，或者是NULL。
> 参考完整性维护表间数据的有效性,完整性,通常通过建立外部键联系另一表的主键实现,还可以用触发器来维护参考完整性


<a name="GkP0o"></a>

## 外键约束 FOREIGN KEY

创建

```sql
#
下面的 SQL 在 "Orders" 表创建时在 "P_Id" 列上创建 FOREIGN KEY 约束
：
# MySQL
：
CREATE TABLE Orders
(
    O_Id    int NOT NULL,
    OrderNo int NOT NULL,
    P_Id    int,
    PRIMARY KEY (O_Id),
    FOREIGN KEY (P_Id) REFERENCES Persons (P_Id)
) # SQL Server / Oracle / MS Access：
CREATE TABLE Orders
(
    O_Id    int NOT NULL PRIMARY KEY,
    OrderNo int NOT NULL,
    P_Id    int FOREIGN KEY REFERENCES Persons(P_Id)
) # 如需命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束，请使用下面的 SQL 语法：
# MySQL / SQL Server / Oracle / MS Access：
CREATE TABLE Orders
(
    O_Id    int NOT NULL,
    OrderNo int NOT NULL,
    P_Id    int,
    PRIMARY KEY (O_Id),
    CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
        REFERENCES Persons (P_Id)
)
```

修改

```sql
#
MySQL / SQL Server / Oracle / MS Access
：
ALTER TABLE Orders
    ADD FOREIGN KEY (P_Id)
        REFERENCES Persons (P_Id) # 如需命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束，请使用下面的 SQL 语法：
# MySQL / SQL Server / Oracle / MS Access：
ALTER TABLE Orders
    ADD CONSTRAINT fk_PerOrders
        FOREIGN KEY (P_Id)
            REFERENCES Persons (P_Id)
```

删除

```sql
#
MySQL
：
ALTER TABLE Orders
DROP
FOREIGN KEY fk_PerOrders

# SQL Server / Oracle / MS Access
：
ALTER TABLE Orders
DROP
CONSTRAINT fk_PerOrders
```
