# SSM框架

## Mybatis

### 1. 快速入门

#### 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <!-- 配置事务管理器 -->
            <transactionManager type="JDBC"/>
            <!-- 配置数据源 -->
            <dataSource type="POOLED">
                <!-- 配置驱动 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <!-- 1. jdbc:mysql：协议
                     2. 127.0.0:1:3306 : 指定连接 mysql 的 ip + port
                     3. mybatis：连接的 DB
                     4. useSSL=true 表示使用安全连接
                     5. &amp; 表示 &，防止解析错误（转义）
                     6. useUnicode=true：使用 unicode 防止编码错误
                     6. characterEncoding=UTF-8：使用 UTF-8，防止中文乱码
                -->
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="352420kobe24llq"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

#### mybatis-config.xml 管理 MonsterMapper.xml

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202310140027543.png)

#### CRUD 模板

```java
<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  keyProperty=""
  keyColumn=""
  useGeneratedKeys=""
  timeout="20">

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
```

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `id`               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType`    | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以根据语句中实际传入的参数计算出应该使用的类型处理器（TypeHandler），默认值为未设置（unset）。 |
| `parameterMap`     | 用于引用外部 parameterMap 的属性，目前已被废弃。请使用行内参数映射和 parameterType 属性。 |
| `flushCache`       | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：（对 insert、update 和 delete 语句）true。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `statementType`    | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（`unset`）。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`        | （仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `databaseId`       | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    1. 这是一个 mapper.xml 文件
    2. 该文件可以去实现接口对应的方法
    3. namespace：指定该 xml 和哪个接口对应！！！
-->
<mapper namespace="com.llq.mapper.MonsterMapper">
    <!-- 配置 addMonster
        1. id="addMonster" 就是接口的方法
        2. parameterType="com.llq.entity.Monster" 放入的形参类型
        3. 注意："com.llq.entity.Monster" 可以简写
        4. 写入 sql 语句
        5. (#{age}, #{birthday}, #{email}, #{gender}, #{name}, #{salary}) 是从 monster 对象属性
        6. 这里 #{age} age 对应 monster 对象属性名，其它类推

    -->
    <insert id="addMonster" parameterType="com.llq.entity.Monster">
        INSERT INTO `monster`
        (`age`, `birthday`, `email`, `gender`, `name`, `salary`)
        VALUES(#{age}, #{birthday}, #{email}, #{gender}, #{name}, #{salary})
    </insert>
</mapper>
```

#### 创建工具类通过 SessionFactory（连接池） 获取到 SqlSeesion（连接）

```java
package com.llq.util;
/**
 * 工具类：得到 SqlSession
 */
public class MybatisUtils {
    //  属性
    private static SqlSessionFactory sqlSessionFactory;

    //  编写静态代码块，初始化 sqlSessionFactory
    static {
        try {
            //  指定资源文件【配置文件】
            String resourcePath = "mybatis-config.xml";
            //  获取到配置文件的 inputStream 【因为要读取它】
            InputStream resourceAsStream = Resources.getResourceAsStream(resourcePath);// 加载文件时，默认到 resources 目录 => 运行后的工作目录 classes
           sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //  编写方法，返回 SqlSession对象 -- 会话 【这个会话可以获取到 mapper 对象】
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

测试：

```java
package com.llq.mapper;
public class MonsterMapperTest {
    //  属性
    private SqlSession sqlSession;
    private MonsterMapper monsterMapper;
    //  编写方法，完成初始化
    /**
     * 1. 当方法 标注 @Before，表示在执行你的目标测试方法前，会先执行该方法
     */
    @Before
    public void init(){
        //  获取到 sqlSession
        sqlSession = MybatisUtils.getSqlSession();
        //  获取到 monsterMapper 对象 【class com.sun.proxy.$Proxy9 代理对象】
        //  底层使用了动态代理机制
        monsterMapper = sqlSession.getMapper(MonsterMapper.class);
        System.out.println("monsterMapper 运行类型 = " + monsterMapper.getClass());
    }

    @Test
    public void test1(){
        System.out.println("test1");
        monsterMapper.addMonster(new );
    }
}
```

##### 测试添加

```java
public class MonsterMapperTest {
    //  属性
    private SqlSession sqlSession;
    private MonsterMapper monsterMapper;

    //  编写方法，完成初始化

    /**
     * 1. 当方法 标注 @Before，表示在执行你的目标测试方法前，会先执行该方法
     */
    @Before
    public void init(){
        //  获取到 sqlSession
        sqlSession = MybatisUtils.getSqlSession();
        //  获取到 monsterMapper 对象 【class com.sun.proxy.$Proxy9 代理对象】
        //  底层使用了动态代理机制
        monsterMapper = sqlSession.getMapper(MonsterMapper.class);
        System.out.println("monsterMapper 运行类型 = " + monsterMapper.getClass());
    }


    @Test
    public void test1(){
        System.out.println("test1");
        for (int i = 0; i < 2; i++) {
            Monster monster = new Monster();
            monster.setAge(10 + i);
            monster.setBirthday(new Date());
            monster.setEmail("tn@sohu.com");
            monster.setGender(1);
            monster.setName("松鼠精" + i);
            monster.setSalary(1000 + i * 10);
            monsterMapper.addMonster(monster);
            System.out.println("刚刚添加的对象的id=" + monster.getId());
        }
        //  如果是增删改，需要提交事务
        if (sqlSession != null){
            sqlSession.commit();
            sqlSession.close();    //  将连接返回连接池
        }
        System.out.println("保存成功!!!");
    }
}
```

###### 获取主键【修改配置文件：userGenerateKeys="true"】

|                                     |                                                              |      |
| ----------------------------------- | ------------------------------------------------------------ | ---- |
| 属性                                | 描述                                                         |      |
| `useGeneratedKeys` 【可以拿到主键】 | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |      |
| `keyProperty` 【指定哪个是主键】    | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（`unset`）。如果生成列不止一个，可以用逗号分隔多个属性名称。 |      |

修改配置文件

```xml
<insert id="addMonster" parameterType="com.llq.entity.Monster" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO `monster`
    (`age`, `birthday`, `email`, `gender`, `name`, `salary`)
    VALUES(#{age}, #{birthday}, #{email}, #{gender}, #{name}, #{salary})
</insert>
```

添加完 monster后。主键 id 是可以通过 对象返回的

##### 测试删除

```java
package com.llq.mapper;

/**
 * 1. 这是一个接口
 * 2. 该接口用于声明操作 monster表的 方法
 * 3. 这些方法可以通过注解或者 XML 文件来实现【底层机制】
 */
public interface MonsterMapper {
    //  添加 monster
    public void addMonster(Monster monster);

    //  根据 id 删除一个 monster
    public void delMonster(Integer id);
}
```

```xml
<delete id="delMonster" parameterType="Integer">
    DELETE from `monster` where id = #{id}
</delete>
```

##### 测试修改

```xml
<update id="updateMonster" parameterType="com.llq.entity.Monster">
    UPDATE `monster` set `age` = #{age}, `birthday` = #{birthday}, `email` = #{email}, `gender` = #{gender}, `name` = #{name}, `salary` = #{salary} where `id` = #{id}
</update>
```

##### 测试查询

###### 单个

```xml
<select id="getMonsterById" resultType="com.llq.entity.Monster">
    select `id`, `age`, `birthday`, `email`, `gender`, `name`, `salary` from `monster` where `id` = #{id}
</select>
```

```java
@Test
public void getMonsterById(){
    Monster monster2 = monsterMapper.getMonsterById(2);
    System.out.println(monster2);
    if (sqlSession != null){
        sqlSession.close();    //  将连接返回连接池【查询虽然不用 commit，但也要 close】
    }
}
```

###### 全部查询

```xml
<select id="findAllMonster" resultType="com.llq.entity.Monster">
    select `id`, `age`, `birthday`, `email`, `gender`, `name`, `salary` from `monster`
</select>
```

```java
@Test
public void findAllMonster(){
    List<Monster> allMonster = monsterMapper.findAllMonster();
    for (Monster monster : allMonster) {
        System.out.println(monster);
    }
    if (sqlSession != null){
        sqlSession.close();    //  将连接返回连接池【查询虽然不用 commit，但也要 close】
    }
}
```

#### 类型别名

要在 mybatis-config.xml 中配置

```xml
<typeAliases>
    <typeAlias type="com.llq.entity.Monster" alias="Monster"/>
</typeAliases>
```

### 2. 日志【查看 Sql】

#### 2.1. 配置

```xml
<!-- 配置 MyBatis自带的日志输出：查看原生 sql -->
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

#### 2.2 . 查看 Log

![img](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202310181943714.png)





