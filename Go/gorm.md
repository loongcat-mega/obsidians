# ORM

orm:对象-关系映射(Object-Relational Mapping) 

面向对象是企业级应用开发环境中的主流开发方法，关系数据库是企业级应用环境中永久存放数据的主流数据存储系统。
对象和关系数据是业务实体的两种表现形式，内存中对象之间存在关联和继承关系，而在关系数据库中，关系数据无法直接表达多对多关联和继承关系。因此orm系统一般以中间件的形式存在，主要实现**程序对象到关系数据库数据的映射**

ORM以最基本的形式建模数据，比如ORM会将MySQL的一张表映射成一个类，表的字段就是这个类的成员变量，使系统在代码层面保持准确统一

ORM包含对持久类对象进行CRUD操作的API，也就是sql查询全部封装成了编程语言中的函数，通过函数的链式组合方式生成最终的SQL语句，避免了不规范。冗余/风格不统一的SQL语句，方便编码风格的统一和后期维护



**数据库操纵工具，支持以面向对象的方式对数据库进行CRUD，而不必关心底层数据库细节**



| ORM  | DB  |     |     |
| ---- | --- | --- | --- |
| 类    | 数据表 |     |     |
| 实例对象 | 数据行 |     |     |
| 属性   | 字段  |     |     |
|      |     |     |     |

[image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cf9bf8b0c3294c70bb3b71c5b66de1ed~tplv-k3u1fbpfcp-jj-mark:1512:0:0:0:q75.avis#?w=1186&h=894&s=281349&e=pn)




# GORM

## 创建

### 创建记录

```go
user := User{Name: "Jinzhu", Age: 18, Birthday: time.Now()}  
  
result := db.Create(&user) // 通过数据的指针来创建  
  
user.ID             // 返回插入数据的主键  
result.Error        // 返回 error  
result.RowsAffected // 返回插入记录的条数
```
😴

创建多条记录
```go
users := []*User{  
    {Name: "Jinzhu", Age: 18, Birthday: time.Now()},  
    {Name: "Jackson", Age: 19, Birthday: time.Now()},  
}  
  
result := db.Create(users) // pass a slice to insert multiple row  
  
result.Error        // returns error  
result.RowsAffected // returns inserted records count
```




## 查询

### 检索单个对象

提供了First Take Last方法。当查询数据库时它添加了LIMIT 1 条件，且没有找到记录时，会返回ErrRecordNotFound错误

```go
//获取第一条记录（主键升序）
db.first(&user)
//select * from users order by id LIMIT 1

//获取一条记录，没有指定排序字段
db.Take(&user)
//select *from users LIMIT 1

//获取最后一条记录
db.Last(&user)
//select * from users order by id desc LIMIT1


result := db.First(&user)
result.RowsAffected//返回找到的记录数
result.Error

//检查ErrRecordNotFound错误
error.Is(result.Error,gorm.ErrReordNotFound)

```
#### 根据主键检索

如果主键是数字类型，可以使用内敛条件来检索对象。当使用字符串时，避免SQL注入

```go
db.First(&user,10)
db.First(&user,"id=?",10)
//select *from users where id = 10

db.Find(&user,[]int{1,2,3})
db.Find(&item, "id in ?", []int{1, 2, 3, 4})
//select * from users wherer id in (1,2,3)


```
当目标对象有一个主键值时，将使用主键构建查询条件

```go
item := Items{id:1}
db.First(&item)
//select *from items where id = 1
```
### 检索全部对象

```go
// Get all records
result := db.Find(&users)
// SELECT * FROM users;
```


### 条件

#### string条件


如果对象设置了主键，条件查询将不会覆盖主键的值，而是用AND连接条件
```go
// Get first matched record  
db.Where("name = ?", "jinzhu").First(&user)  
// SELECT * FROM users WHERE name = 'jinzhu' ORDER BY id LIMIT 1;

```

##### in
```go
// IN  
db.Where("name IN ?", []string{"jinzhu", "jinzhu 2"}).Find(&users)  
// SELECT * FROM users WHERE name IN ('jinzhu','jinzhu 2');
```

##### like

```go
// LIKE  
db.Where("name LIKE ?", "%jin%").Find(&users)  
// SELECT * FROM users WHERE name LIKE '%jin%';
```

##### and

```go
// AND  
db.Where("name = ? AND age >= ?", "jinzhu", "22").Find(&users)  
// SELECT * FROM users WHERE name = 'jinzhu' AND age >= 22;
```

##### bwtween

```go
// BETWEEN  
db.Where("created_at BETWEEN ? AND ?", lastWeek, today).Find(&users)  
// SELECT * FROM users WHERE created_at BETWEEN '2000-01-01 00:00:00' AND '2000-01-08 00:00:00';
```
#### struct & map 条件

```go

// Struct  
db.Where(&User{Name: "jinzhu", Age: 20}).First(&user)  
// SELECT * FROM users WHERE name = "jinzhu" AND age = 20 ORDER BY id LIMIT 1;  
  
// Map  
db.Where(map[string]interface{}{"name": "jinzhu", "age": 20}).Find(&users)  
// SELECT * FROM users WHERE name = "jinzhu" AND age = 20;  
  
// Slice of primary keys  
db.Where([]int64{20, 21, 22}).Find(&users)  
// SELECT * FROM users WHERE id IN (20, 21, 22);
```

```go
result := db.Where(&Items{ID: 1}).Or(&Items{ID: 2}).Or(map[string]interface{}{"ID": 4}).Find(&item)
```

当结构体字段为零值时，不会被构建进查询条件中

```go
db.Where(&User{Name: "jinzhu", Age: 0}).Find(&users)  
// SELECT * FROM users WHERE name = "jinzhu";
```


#### 内联查询

在查询条件处，字符串 结构体 和 map可以替换

#### Not条件

```go
db.Not("name = ?", "jinzhu").First(&user)  
// SELECT * FROM users WHERE NOT name = "jinzhu" ORDER BY id LIMIT 1;  
  
// Not In  
db.Not(map[string]interface{}{"name": []string{"jinzhu", "jinzhu 2"}}).Find(&users)  
// SELECT * FROM users WHERE name NOT IN ("jinzhu", "jinzhu 2");  
  
// Struct  
db.Not(User{Name: "jinzhu", Age: 18}).First(&user)  
// SELECT * FROM users WHERE name <> "jinzhu" AND age <> 18 ORDER BY id LIMIT 1;  
  
// Not In slice of primary keys  
db.Not([]int64{1,2,3}).First(&user)  
// SELECT * FROM users WHERE id NOT IN (1,2,3) ORDER BY id LIMIT 1;
```
#### Or条件


```go
db.Where("role = ?", "admin").Or("role = ?", "super_admin").Find(&users)  
// SELECT * FROM users WHERE role = 'admin' OR role = 'super_admin';  
  
// Struct  
db.Where("name = 'jinzhu'").Or(User{Name: "jinzhu 2", Age: 18}).Find(&users)  
// SELECT * FROM users WHERE name = 'jinzhu' OR (name = 'jinzhu 2' AND age = 18);  
  
// Map  
db.Where("name = 'jinzhu'").Or(map[string]interface{}{"name": "jinzhu 2", "age": 18}).Find(&users)  
// SELECT * FROM users WHERE name = 'jinzhu' OR (name = 'jinzhu 2' AND age = 18);
```

### select 特定字段

```go
db.Select("name", "age").Find(&users)  
// SELECT name, age FROM users;  
  
db.Select([]string{"name", "age"}).Find(&users)  
// SELECT name, age FROM users;  
  
db.Table("users").Select("COALESCE(age,?)", 42).Rows()  
// SELECT COALESCE(age,'42') FROM users;
```

### 排序 order

```go
db.Order("age desc, name").Find(&users)  
// SELECT * FROM users ORDER BY age desc, name;  
  
// Multiple orders  
db.Order("age desc").Order("name").Find(&users)  
// SELECT * FROM users ORDER BY age desc, name;
```


