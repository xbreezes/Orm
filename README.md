Orm [![Build Status](https://travis-ci.org/xbreezes/Orm.svg?branch=master)](https://travis-ci.org/xbreezes/Orm)    [ ![Download](https://api.bintray.com/packages/xbreezes/maven/droid-orm/images/download.svg) ](https://bintray.com/xbreezes/maven/droid-orm/_latestVersion)  [![Coverage Status](https://coveralls.io/repos/xbreezes/Orm/badge.svg)](https://coveralls.io/r/xbreezes/Orm)
-----

零配置android简单数据库操作框架

#### 功能简介

 * 支持CURD
 * 支持事务 CUD默认开启事务
 * 可通过注解自定义表名，列名，唯一性约束，NOT NULL约束
 * 支持链式表达查询，更直观的查询语义
 * 通过QueryAble查询获得的Cursor支持CursorAdapter的AutoReload

#### 开始使用 

1.模型定义

```java
    //model
    @Table(name="demo")
    class Demo{
        @Column(primaryKey=true,name="_id",autoincrement=true)
        public int id;
        @Column(name="name")
        public String name;
        @Column
        public int age;
    }
```

2.创建表

 * 一气呵成
 
```java
    SimpleOrmSQLiteHelper helper=new SimpleOrmSQLiteHelper(context,"demo.db",1,Demo.class);
```

 * 在onCreate中创建
 
```java
    class DemoHelper extends SimpleOrmSQLiteHelper{
     ...
        public void onCreate(SQLiteDatabase database){
            TableUtils.createTable(database,Demo.class);
        }
     ...
    }
```

3.CURD 操作

 * 插入

```java
    Demo demo=new Demo();
    demo.name="hello"
    demo.arg=12;
    helper.insert(demo);
```

 * 批量插入

 ```java
    ArrayList<Demo> demos=new ArrayList<>();
    for(int i=0;i<10;i++){
       Demo demo=new Demo();
       demo.name="hello"
       demo.arg=12;
       demos.add(demo);
    }
    helper.insertAll(demos.toArray());
 ```

 * 查询

    ```java
    //获取第一个
     Demo demo=helper.query(Demo.class).execute().first();
     
     //for循环遍历
     for(Demo demo:helper.query(Demo.class).execute()){
         //do somethings
     }
     
     //条件查询
     Demo demo=helper.query(Demo.class).where("age",12,"=").execute().first();
    ```

 * 删除

 ```java
 Demo demo=new Demo();
 demo.id=1;
 demo.name="hello";
 
 //按主键删除
 helper.delete(demo);
 
 //按自定义字段删除
 helper.deleteBy(demo,"name);
 ```

 * 修改

 ```java
 Demo demo=helper.query(Demo.class).execute().first();
 demo.name="hello1"
 demo.age=19;
 helper.insertOrUpdate(demo);
 ```

 * 其他

 ```java
 //关闭反馈URD影响的结果,提升效率
 OrmConfig.Response=false;
 //关闭反馈URD影响的结果,提升效率
 OrmConfig.Notify=false;
 ```

 * 升级

 ```java
 //1.创建SimpleOrmSQLiteHelper时将version增加,
 SimpleOrmSQLiteHelper helper=new SimpleOrmSQLiteHelper(context,"demo.db",2,Demo.class);
 //2.然后在Entity中增加所需要增加的字段
 ```

 >升级只支持增加字段,不支持删除字段

 >增加的字段必须有默认值


#### 版本更新

  * 2015-05-25

    >增加数据库升级功能

  * 2015-04-23

    >优化插入效率

    >增加notifyChange开关

    >增加是否返回URD影响结果的开关

  * 2014-10-22
  
    >提升ORM的查询和插入效率
  
    >Column中的排序属性number修改为order

    >insertAll拆分为insertAll和insertOrUpdateAll
    
