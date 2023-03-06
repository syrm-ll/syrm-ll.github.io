# MyBatis 3 文档

> 边看官方文档边练习, 这边记录一点备忘.

# 一 安装

略

# 二 起步

准备配置文件, 实体类, 映射器 略

```java
SqlSessionFactory sf=new SqlSessionFactoryBuilder().build(Resources.getResourcesAsStream("ConfigFileUrl"),"环境别名");
        SqlSession session=sf.openSession();
```

# 三 XML配置文件

## 注意:

Mybatis 自己实现了事务隔离级别的枚举配置类: ` org.apache.ibatis.session.TransactionIsolationLevel`

其他默认的类型处理器, 枚举类型处理器, 脚本语言驱动 见官方文档.


## 属性

## 设置

## 类型别名

## 类型处理器

## 插件

## 环境配置

## 数据库厂商标识符

## 映射器


# 四 XML 映射文件

## select

## insert update delete

## 参数

## 结果映射

## 自动映射

## 缓存


# 五 注解配置(有限的)

> 设计初期的 MyBatis 是一个 XML 驱动的框架。配置信息是基于 XML 的，映射语句也是定义在 XML 中的。而在 MyBatis 3 中，我们提供了其它的配置方式。MyBatis 3
> 构建在全面且强大的基于 Java 语言的配置 API 之上。它是 XML 和注解配置的基础。注解提供了一种简单且低成本的方式来实现简单的映射语句。
>
> **提示** 不幸的是，Java 注解的表达能力和灵活性十分有限。尽管我们花了很多时间在调查、设计和试验上，但最强大的 MyBatis 映射并不能用注解来构建——我们真没开玩笑。而
> C# 属性就没有这些限制，因此 MyBatis.NET 的配置会比 XML 有更大的选择余地。虽说如此，基于 Java 注解的配置还是有它的好处的。

## 注解列表

https://mybatis.org/mybatis-3/zh/java-api.html

| 注解                                                                                     | 使用对象 | XML 等价形式                                       | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------|:-----|:-----------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `@CacheNamespace`                                                                      | `类`  | `<cache>`                                      | 为给定的命名空间（比如类）配置缓存。属性：`implemetation`、`eviction`、`flushInterval`、`size`、`readWrite`、`blocking`、`properties`。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `@Property`                                                                            | N/A  | `<property>`                                   | 指定参数值或占位符（placeholder）（该占位符能被 `mybatis-config.xml` 内的配置属性替换）。属性：`name`、`value`。（仅在 MyBatis 3.4.2 以上可用）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `@CacheNamespaceRef`                                                                   | `类`  | `<cacheRef>`                                   | 引用另外一个命名空间的缓存以供使用。注意，即使共享相同的全限定类名，在 XML 映射文件中声明的缓存仍被识别为一个独立的命名空间。属性：`value`、`name`。如果你使用了这个注解，你应设置 `value` 或者 `name` 属性的其中一个。`value` 属性用于指定能够表示该命名空间的 Java 类型（命名空间名就是该 Java 类型的全限定类名），`name` 属性（这个属性仅在 MyBatis 3.4.2 以上可用）则直接指定了命名空间的名字。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `@ConstructorArgs`                                                                     | `方法` | `<constructor>`                                | 收集一组结果以传递给一个结果对象的构造方法。属性：`value`，它是一个 `Arg` 数组。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `@Arg`                                                                                 | N/A  | `<arg>``<idArg>`                               | ConstructorArgs 集合的一部分，代表一个构造方法参数。属性：`id`、`column`、`javaType`、`jdbcType`、`typeHandler`、`select`、`resultMap`。id 属性和 XML 元素 `<idArg>` 相似，它是一个布尔值，表示该属性是否用于唯一标识和比较对象。从版本 3.5.4 开始，该注解变为可重复注解。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `@TypeDiscriminator`                                                                   | `方法` | `<discriminator>`                              | 决定使用何种结果映射的一组取值（case）。属性：`column`、`javaType`、`jdbcType`、`typeHandler`、`cases`。cases 属性是一个 `Case` 的数组。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `@Case`                                                                                | N/A  | `<case>`                                       | 表示某个值的一个取值以及该取值对应的映射。属性：`value`、`type`、`results`。results 属性是一个 `Results` 的数组，因此这个注解实际上和 `ResultMap` 很相似，由下面的 `Results` 注解指定。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `@Results`                                                                             | `方法` | `<resultMap>`                                  | 一组结果映射，指定了对某个特定结果列，映射到某个属性或字段的方式。属性：`value`、`id`。value 属性是一个 `Result` 注解的数组。而 id 属性则是结果映射的名称。从版本 3.5.4 开始，该注解变为可重复注解。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `@Result`                                                                              | N/A  | `<result>``<id>`                               | 在列和属性或字段之间的单个结果映射。属性：`id`、`column`、`javaType`、`jdbcType`、`typeHandler`、`one`、`many`。id 属性和 XML 元素 `<id>` 相似，它是一个布尔值，表示该属性是否用于唯一标识和比较对象。one 属性是一个关联，和 `<association>` 类似，而 many 属性则是集合关联，和 `<collection>` 类似。这样命名是为了避免产生名称冲突。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `@One`                                                                                 | N/A  | `<association>`                                | 复杂类型的单个属性映射。属性： `select`，指定可加载合适类型实例的映射语句（也就是映射器方法）全限定名； `fetchType`，指定在该映射中覆盖全局配置参数 `lazyLoadingEnabled`； `resultMap`(available since 3.5.5), which is the fully qualified name of a result map that map to a single container object from select result； `columnPrefix`(available since 3.5.5), which is column prefix for grouping select columns at nested result map. **提示** 注解 API 不支持联合映射。这是由于 Java 注解不允许产生循环引用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `@Many`                                                                                | N/A  | `<collection>`                                 | 复杂类型的集合属性映射。属性： `select`，指定可加载合适类型实例集合的映射语句（也就是映射器方法）全限定名； `fetchType`，指定在该映射中覆盖全局配置参数 `lazyLoadingEnabled` `resultMap`(available since 3.5.5), which is the fully qualified name of a result map that map to collection object from select result； `columnPrefix`(available since 3.5.5), which is column prefix for grouping select columns at nested result map. **提示** 注解 API 不支持联合映射。这是由于 Java 注解不允许产生循环引用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `@MapKey`                                                                              | `方法` |                                                | 供返回值为 Map 的方法使用的注解。它使用对象的某个属性作为 key，将对象 List 转化为 Map。属性：`value`，指定作为 Map 的 key 值的对象属性名。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `@Options`                                                                             | `方法` | 映射语句的属性                                        | 该注解允许你指定大部分开关和配置选项，它们通常在映射语句上作为属性出现。与在注解上提供大量的属性相比，`Options` 注解提供了一致、清晰的方式来指定选项。属性：`useCache=true`、`flushCache=FlushCachePolicy.DEFAULT`、`resultSetType=DEFAULT`、`statementType=PREPARED`、`fetchSize=-1`、`timeout=-1`、`useGeneratedKeys=false`、`keyProperty=""`、`keyColumn=""`、`resultSets=""`, `databaseId=""`。注意，Java 注解无法指定 `null` 值。因此，一旦你使用了 `Options` 注解，你的语句就会被上述属性的默认值所影响。要注意避免默认值带来的非预期行为。 The `databaseId`(Available since 3.5.5), in case there is a configured `DatabaseIdProvider`, the MyBatis use the `Options` with no `databaseId` attribute or with a `databaseId` that matches the current one. If found with and without the `databaseId` the latter will be discarded.      注意：`keyColumn` 属性只在某些数据库中有效（如 Oracle、PostgreSQL 等）。要了解更多关于 `keyColumn` 和 `keyProperty` 可选值信息，请查看“insert, update 和 delete”一节。                                                                                                                                                                                                                            |
| `@Insert``@Update`<br />`@Delete``@Select`                                             | `方法` | `<insert>``<update>`<br />`<delete>``<select>` | 每个注解分别代表将会被执行的 SQL 语句。它们用字符串数组（或单个字符串）作为参数。如果传递的是字符串数组，字符串数组会被连接成单个完整的字符串，每个字符串之间加入一个空格。这有效地避免了用 Java 代码构建 SQL 语句时产生的“丢失空格”问题。当然，你也可以提前手动连接好字符串。属性：`value`，指定用来组成单个 SQL 语句的字符串数组。 The `databaseId`(Available since 3.5.5), in case there is a configured `DatabaseIdProvider`, the MyBatis use a statement with no `databaseId` attribute or with a `databaseId` that matches the current one. If found with and without the `databaseId` the latter will be discarded.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `@InsertProvider`<br />`@UpdateProvider`<br />`@DeleteProvider`<br />`@SelectProvider` | `方法` | `<insert>``<update>`<br />`<delete>``<select>` | 允许构建动态 SQL。这些备选的 SQL 注解允许你指定返回 SQL 语句的类和方法，以供运行时执行。（从 MyBatis 3.4.6 开始，可以使用 `CharSequence` 代替 `String` 来作为返回类型）。当执行映射语句时，MyBatis 会实例化注解指定的类，并调用注解指定的方法。你可以通过 `ProviderContext` 传递映射方法接收到的参数、"Mapper interface type" 和 "Mapper method"（仅在 MyBatis 3.4.5 以上支持）作为参数。（MyBatis 3.4 以上支持传入多个参数） 属性：`value`、`type`、`method`、`databaseId`。 `value` and `type` 属性用于指定类名 (The `type` attribute is alias for `value`, you must be specify either one. But both attributes can be omit when specify the `defaultSqlProviderType` as global configuration)。 `method` 用于指定该类的方法名（从版本 3.5.1 开始，可以省略 `method` 属性，MyBatis 将会使用 `ProviderMethodResolver` 接口解析方法的具体实现。如果解析失败，MyBatis 将会使用名为 `provideSql` 的降级实现）。**提示** 接下来的“SQL 语句构建器”一章将会讨论该话题，以帮助你以更清晰、更便于阅读的方式构建动态 SQL。 The `databaseId`(Available since 3.5.5), in case there is a configured `DatabaseIdProvider`, the MyBatis will use a provider method with no `databaseId` attribute or with a `databaseId` that matches the current one. If found with and without the `databaseId` the latter will be discarded. |
| `@Param`                                                                               | `参数` | N/A                                            | 如果你的映射方法接受多个参数，就可以使用这个注解自定义每个参数的名字。否则在默认情况下，除 `RowBounds` 以外的参数会以 "param" 加参数位置被命名。例如 `#{param1}`, `#{param2}`。如果使用了 `@Param("person")`，参数就会被命名为 `#{person}`。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `@SelectKey`                                                                           | `方法` | `<selectKey>`                                  | 这个注解的功能与 `<selectKey>` 标签完全一致。该注解只能在 `@Insert` 或 `@InsertProvider` 或 `@Update` 或 `@UpdateProvider` 标注的方法上使用，否则将会被忽略。如果标注了 `@SelectKey` 注解，MyBatis 将会忽略掉由 `@Options` 注解所设置的生成主键或设置（configuration）属性。属性：`statement` 以字符串数组形式指定将会被执行的 SQL 语句，`keyProperty` 指定作为参数传入的对象对应属性的名称，该属性将会更新成新的值，`before` 可以指定为 `true` 或 `false` 以指明 SQL 语句应被在插入语句的之前还是之后执行。`resultType` 则指定 `keyProperty` 的 Java 类型。`statementType` 则用于选择语句类型，可以选择 `STATEMENT`、`PREPARED` 或 `CALLABLE` 之一，它们分别对应于 `Statement`、`PreparedStatement` 和 `CallableStatement`。默认值是 `PREPARED`。 The `databaseId`(Available since 3.5.5), in case there is a configured `DatabaseIdProvider`, the MyBatis will use a statement with no `databaseId` attribute or with a `databaseId` that matches the current one. If found with and without the `databaseId` the latter will be discarded.                                                                                                                                                                                                               |
| `@ResultMap`                                                                           | `方法` | N/A                                            | 这个注解为 `@Select` 或者 `@SelectProvider` 注解指定 XML 映射中 `<resultMap>` 元素的 id。这使得注解的 select 可以复用已在 XML 中定义的 ResultMap。如果标注的 select 注解中存在 `@Results` 或者 `@ConstructorArgs` 注解，这两个注解将被此注解覆盖。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `@ResultType`                                                                          | `方法` | N/A                                            | 在使用了结果处理器的情况下，需要使用此注解。由于此时的返回类型为 void，所以 Mybatis 需要有一种方法来判断每一行返回的对象类型。如果在 XML 有对应的结果映射，请使用 `@ResultMap` 注解。如果结果类型在 XML 的 `<select>` 元素中指定了，就不需要使用其它注解了。否则就需要使用此注解。比如，如果一个标注了 @Select 的方法想要使用结果处理器，那么它的返回类型必须是 void，并且必须使用这个注解（或者 @ResultMap）。这个注解仅在方法返回类型是 void 的情况下生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `@Flush`                                                                               | `方法` | N/A                                            | 如果使用了这个注解，定义在 Mapper 接口中的方法就能够调用 `SqlSession#flushStatements()` 方法。（Mybatis 3.3 以上可用）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

# 六 动态SQL

## if

语法:

```xml

<if test=" 表达式 ">
    ......
</if>

        例:
<if test="title != null">
AND title like #{title}
</if>
<if test="author != null and author.name != null">
AND author_name like #{author.name}
</if>
```

## choose when otherwise

意义: 类似于,Java中的 `switch`

语法:

```xml

<choose>
    <when test=""></when>
    <when test=""></when>
    ...
    <otherwise></otherwise>
</choose>

        例:
<select id="findActiveBlogLike"
        resultType="Blog">
SELECT * FROM BLOG WHERE state = ‘ACTIVE’
<choose>
    <when test="title != null">
        AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
        AND author_name like #{author.name}
    </when>
    <otherwise>
        AND featured = 1
    </otherwise>
</choose>
</select>
```

## trim where set

### where

SQL查询条件全部使用if动态SQl会出现问题: 空where子句, 或者以 and / or 开头的查询条件

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

```xml

<select id="findActiveBlogLike"
        resultType="Blog">
    SELECT * FROM BLOG
    <where>
        <if test="state != null">
            state = #{state}
        </if>
        <if test="title != null">
            AND title like #{title}
        </if>
        <if test="author != null and author.name != null">
            AND author_name like #{author.name}
        </if>
    </where>
</select>
```

### set

在 update 语句中, 使用 set 实现类似功能:

```xml

<update id="updateAuthorIfNecessary">
    update Author
    <set>
        <if test="username != null">username=#{username},</if>
        <if test="password != null">password=#{password},</if>
        <if test="email != null">email=#{email},</if>
        <if test="bio != null">bio=#{bio}</if>
    </set>
    where id=#{id}
</update>
```

### trim

如果 where 不能满足要求, 可以自定义 trim , 比如用trim实现默认的where:

```xml

<trim prefix="WHERE" prefixOverrides="AND |OR ">
    ...
</trim>
```

## foreach

动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）。比如：

```xml

<select id="selectPostIn" resultType="domain.blog.Post">
    SELECT *
    FROM POST P
    WHERE ID in
    <foreach item="item" index="index" collection="list" open="(" separator="," close=")">
        #{item}
    </foreach>
</select>
```

## script

在注解中使用动态SQL时, 使用 `<script>` 标签

新版本的JDK 13+, 可以使用多行字符串简化写法.

```java
 @Update({"<script>",
        "update Author",
        "  <set>",
        "    <if test='username != null'>username=#{username},</if>",
        "    <if test='password != null'>password=#{password},</if>",
        "    <if test='email != null'>email=#{email},</if>",
        "    <if test='bio != null'>bio=#{bio}</if>",
        "  </set>",
        "where id=#{id}",
        "</script>"})
    void updateAuthorValues(Author author);
```

## bind

> `bind` 元素允许你在 OGNL 表达式以外创建一个变量，并将其绑定到当前的上下文。比如：
>
> ```xml
> <select id="selectBlogsLike" resultType="Blog">
>   <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
>   SELECT * FROM BLOG
>   WHERE title LIKE #{pattern}
> </select>
> ```



类似参数预处理?

## 更换脚本语言

以上所有标签都由 `org.apache.ibatis.scripting.xmltags.XmlLanguageDrive` 提供, 如果需要更换其他脚本语言, 参考官方文档:

https://mybatis.org/mybatis-3/zh/dynamic-sql.html


# 七 SQL 语句构建器

主要支持类: `org.apache.ibatis.jdbc.SQL`

主要玩法:

```java
    // Builder / Fluent 风格
    public String insertPersonSql() {
        String sql = new SQL()
                .INSERT_INTO("PERSON")
                .VALUES("ID, FIRST_NAME", "#{id}, #{firstName}")
                .VALUES("LAST_NAME", "#{lastName}")
                .toString();
        return sql;
    }

    // 动态条件（注意参数需要使用 final 修饰，以便匿名内部类对它们进行访问）
    public String selectPersonLike(final String id, final String firstName, final String lastName) {
        return new SQL() {{
            SELECT("P.ID, P.USERNAME, P.PASSWORD, P.FIRST_NAME, P.LAST_NAME");
            FROM("PERSON P");
            if (id != null) {
                WHERE("P.ID like #{id}");
            }
            if (firstName != null) {
                WHERE("P.FIRST_NAME like #{firstName}");
            }
            if (lastName != null) {
                WHERE("P.LAST_NAME like #{lastName}");
            }
            ORDER_BY("P.LAST_NAME");
        }}.toString();
    }

    public String deletePersonSql() {
        return new SQL() {{
            DELETE_FROM("PERSON");
            WHERE("ID = #{id}");
        }}.toString();
    }

    public String insertPersonSql() {
        return new SQL() {{
            INSERT_INTO("PERSON");
            VALUES("ID, FIRST_NAME", "#{id}, #{firstName}");
            VALUES("LAST_NAME", "#{lastName}");
        }}.toString();
    }

    public String updatePersonSql() {
        return new SQL() {{
            UPDATE("PERSON");
            SET("FIRST_NAME = #{firstName}");
            WHERE("ID = #{id}");
        }}.toString();
    }
```

更多详细API见官方文档：<https://mybatis.org/mybatis-3/zh/statement-builders.html> 或者看源码 API文档


# 八 日志

> Mybatis 通过使用内置的日志工厂提供日志功能。内置日志工厂将会把日志工作委托给下面的实现之一：
>
> - SLF4J
> - Apache Commons Logging
> - Log4j 2
> - Log4j
> - JDK logging
>
> MyBatis 内置日志工厂会基于运行时检测信息选择日志委托实现。它会（按上面罗列的顺序）使用第一个查找到的实现。当没有找到这些实现时，将会禁用日志功能。







---

# -------------------------------

---

# Mybatis-Spring

# 入门

## 依赖

```xml

<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
</dependency>
```

**注意**

MyBatis-Spring 需要以下版本：

| MyBatis-Spring | MyBatis | Spring Framework | Spring Batch | Java    |
|:---------------|:--------|:-----------------|:-------------|:--------|
| **2.0**        | 3.5+    | 5.0+             | 4.0+         | Java 8+ |
| **1.3**        | 3.4+    | 3.2.2+           | 2.1+         | Java 6+ |

## 快速开始

核心目标, 在Spring内创建 `SqlSessionFactory` 和 需要的数据映射器Bean.

### 创建 SqlSessionFactory

```xml

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!-- 必须: 数据源 -->
    <property name="dataSource" ref="dataSource" />
    <!-- 配置文件位置 -->
    <property name="configLocation" value="classpath:mybatis.xml">
    <!-- 可选: 映射器 XML 文件位置 -->
    <property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
</bean>
```

等价于:

```java

@Configuration
public class MyBatisConfig {
    @Bean
    public SqlSessionFactory sqlSessionFactory() throws Exception {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource());
        return factoryBean.getObject();
    }
}
```

---

解释:

需要注意的是 `SqlSessionFactoryBean` 实现了 Spring 的 `FactoryBean` 接口（参见 Spring 官方文档 3.8
节 [通过工厂 bean 自定义实例化逻辑](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-extension-factorybean) ）。
这意味着由 Spring 最终创建的 bean **并不是** `SqlSessionFactoryBean` 本身，而是工厂类（`SqlSessionFactoryBean`）的 getObject() 方法的返回结果。这种情况下，Spring
将会在应用启动时为你创建 `SqlSessionFactory`，并使用 `sqlSessionFactory` 这个名字存储起来。

通常，在 MyBatis-Spring 中，你不需要直接使用 `SqlSessionFactoryBean` 或对应的 `SqlSessionFactory`。 相反，session 的工厂 bean
将会被注入到 `MapperFactoryBean` 或其它继承于 `SqlSessionDaoSupport` 的 DAO（Data Access Object，数据访问对象）中。

---

#### 属性列表

**`SqlSessionFactory`** 有一个唯一的必要属性：用于 JDBC 的 `DataSource`。这可以是任意的 `DataSource` 对象，它的配置方法和其它 Spring 数据库连接是一样的。

---

**`configLocation`**，它用来指定 MyBatis 的 XML 配置文件路径。它在需要修改 MyBatis 的基础配置非常有用。通常，基础配置指的是 `<settings>`
或 `<typeAliases>` 元素。这个配置文件**并不需要**是一个完整的 MyBatis 配置。确切地说，任何:

- 环境配置（`<environments>`）

- 数据源（`<DataSource>`）

- MyBatis 的事务管理器（`<transactionManager>`）

都会被**忽略**。 `SqlSessionFactoryBean` 会创建它自有的 MyBatis 环境配置（`Environment`），并按要求设置自定义环境的值。

---

**`mapperLocations`** 属性接受多个资源位置。这个属性可以用来指定 MyBatis 的映射器 XML 配置文件的位置。属性的值是一个 Ant
风格的字符串，可以指定加载一个目录中的所有文件，或者从一个目录开始递归搜索所有目录。

> 如果 MyBatis 在映射器类对应的路径下找不到与之相对应的映射器 XML 文件，那么也需要配置文件。这时有两种解决办法：第一种是手动在 MyBatis 的 XML
> 配置文件中的 `<mappers>` 部分中指定 XML 文件的类路径；第二种是设置工厂 bean 的 `mapperLocations` 属性。

---

管理事务时还需要一个属性: `transactionFactoryClass`


### 创建 DataBaseIdProvider

> 如果你使用了多个数据库，那么需要设置 `databaseIdProvider`

```xml

<bean id="databaseIdProvider" class="org.apache.ibatis.mapping.VendorDatabaseIdProvider">
    <property name="properties">
        <props>
            <prop key="SQL Server">sqlserver</prop>
            <prop key="DB2">db2</prop>
            <prop key="Oracle">oracle</prop>
            <prop key="MySQL">mysql</prop>
        </props>
    </property>
</bean>
        <!-- 注入到 sqlSessionFactory -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
<property name="dataSource" ref="dataSource" />
<property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
<property name="databaseIdProvider" ref="databaseIdProvider" />
</bean>
```

### 创建 Mapper 示例

```java

@Configuration
public class MyBatisConfig {

    @Bean
    public UserMapper userMapper() throws Exception {
        // 这个 sqlSessionFactory() 是指当前配置类中的方法
        SqlSessionTemplate sqlSessionTemplate = new SqlSessionTemplate(sqlSessionFactory());
        return sqlSessionTemplate.getMapper(UserMapper.class);
    }
}
```

# 事务

> 一个使用 MyBatis-Spring 的其中一个主要原因是它允许 MyBatis 参与到 Spring 的事务管理中。而不是给 MyBatis 创建一个新的专用事务管理器，MyBatis-Spring 借助了
> Spring 中的 `DataSourceTransactionManager` 来实现事务管理。
>
> 一旦配置好了 Spring 的事务管理器，你就可以在 Spring 中按你平时的方式来配置事务。并且支持 `@Transactional` 注解和 AOP
> 风格的配置。在事务处理期间，一个单独的 `SqlSession` 对象将会被创建和使用。当事务完成时，这个 session 会以合适的方式提交或回滚。
>
> 事务配置好了以后，MyBatis-Spring 将会透明地管理事务。这样在你的 DAO 类中就不需要额外的代码了。
>
> ---
>
> 来自官方文档


## 标准配置

要开启 Spring 的事务处理功能，在 Spring 的配置文件中创建一个 `DataSourceTransactionManager` 对象：

```xml

<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <constructor-arg ref="dataSource" />
</bean>
```

```java

@Configuration
public class DataSourceConfig {
    @Bean
    public DataSourceTransactionManager transactionManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}
```

传入的 `DataSource` 可以是任何能够与 Spring 兼容的 JDBC `DataSource`。包括连接池和通过 JNDI 查找获得的 `DataSource`。

注意：为事务管理器指定的 `DataSource` **必须**和用来创建 `SqlSessionFactoryBean` 的是同一个数据源，否则事务管理器就无法工作了。


## 容器管理事务

略

## 编程式事务

MyBatis 的 `SqlSession` 提供几个方法来在代码中处理事务。但是当使用 MyBatis-Spring 时，你的 bean 将会注入由 Spring 管理的 `SqlSession` 或映射器。也就是说，Spring
总是为你处理了事务。

你不能在 Spring 管理的 `SqlSession` 上调用 `SqlSession.commit()`，`SqlSession.rollback()` 或 `SqlSession.close()`
方法。如果这样做了，就会抛出 `UnsupportedOperationException` 异常。在使用注入的映射器时，这些方法也不会暴露出来。

无论 JDBC 连接是否设置为自动提交，调用 `SqlSession` 数据方法或在 Spring 事务之外调用任何在映射器中方法，事务都将会自动被提交。

如果你想编程式地控制事务，请参考 [the Spring reference document(Data Access -Programmatic transaction management-)](https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-programmatic)
。下面的代码展示了如何使用 `PlatformTransactionManager` 手工管理事务。

```java
public class UserService {
    private final PlatformTransactionManager transactionManager;

    public UserService(PlatformTransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public void createUser() {
        TransactionStatus txStatus =
                transactionManager.getTransaction(new DefaultTransactionDefinition());
        try {
            userMapper.insertUser(user);
        } catch (Exception e) {
            transactionManager.rollback(txStatus);
            throw e;
        }
        transactionManager.commit(txStatus);
    }
}
```

在使用 `TransactionTemplate` 的时候，可以省略对 `commit` 和 `rollback` 方法的调用。

```java
public class UserService {
    private final PlatformTransactionManager transactionManager;

    public UserService(PlatformTransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public void createUser() {
        TransactionTemplate transactionTemplate = new TransactionTemplate(transactionManager);
        transactionTemplate.execute(txStatus -> {
            userMapper.insertUser(user);
            return null;
        });
    }
}
```

注意：虽然这段代码使用的是一个映射器，但换成 SqlSession 也是可以工作的。

# SqlSession

在 MyBatis 中，你可以使用 `SqlSessionFactory` 来创建 `SqlSession`。 一旦你获得一个 session 之后，你可以使用它来执行映射了的语句，提交或回滚连接，最后，当不再需要它的时候，你可以关闭
session。 使用 MyBatis-Spring 之后，你不再需要直接使用 `SqlSessionFactory` 了，因为你的 bean 可以被注入一个线程安全的 `SqlSession`，它能基于 Spring
的事务配置来自动提交、回滚、关闭 session。

## SQLSessionTemplate

`SqlSessionTemplate` 是 MyBatis-Spring 的核心。作为 `SqlSession`
的一个实现，这意味着可以使用它无缝代替你代码中已经在使用的 `SqlSession`。 `SqlSessionTemplate` 是线程安全的，可以被多个 DAO 或映射器所共享使用。

可以使用 `SqlSessionFactory` 作为构造方法的参数来创建 `SqlSessionTemplate` 对象。

```xml

<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
```

```java

@Configuration
public class MyBatisConfig {
    @Bean
    public SqlSessionTemplate sqlSession() throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory());
    }
}
```

## SqlSessionDaoSupport

`SqlSessionDaoSupport` 是一个抽象的支持类，用来为你提供 `SqlSession`。调用 `getSqlSession()` 方法你会得到一个 `SqlSessionTemplate`，之后可以用于执行 SQL
方法，就像下面这样:

```java
public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao {
    public User getUser(String userId) {
        return getSqlSession().selectOne("org.mybatis.spring.sample.mapper.UserMapper.getUser", userId);
    }
}
```

在这个类里面，通常更倾向于使用 `MapperFactoryBean`，因为它不需要额外的代码。但是，如果你需要在 DAO 中做其它非 MyBatis 的工作或需要一个非抽象的实现类，那么这个类就很有用了。

`SqlSessionDaoSupport` 需要通过属性设置一个 `sqlSessionFactory` 或 `SqlSessionTemplate`。如果两个属性都被设置了，那么 `SqlSessionFactory` 将被忽略。

假设类 `UserMapperImpl` 是 `SqlSessionDaoSupport` 的子类，可以编写如下的 Spring 配置来执行设置：

```xml

<bean id="userDao" class="org.mybatis.spring.sample.dao.UserDaoImpl">
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```

# 映射器

## 主动注册映射器

### XML 配置

在你的 XML 中加入 `MapperFactoryBean` 以便将映射器注册到 Spring 中。就像下面一样：

```xml

<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="mapperInterface" value="org.mybatis.spring.sample.mapper.UserMapper" />
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```

如果映射器接口 UserMapper 在相同的类路径下有对应的 MyBatis XML 映射器配置文件，将会被 `MapperFactoryBean` 自动解析。不需要在 MyBatis
配置文件中显式配置映射器，除非映射器配置文件与接口类不在同一个类路径下。 参考 `SqlSessionFactoryBean`
的 [`configLocation`](http://mybatis.org/spring/zh/factorybean.html) 属性以获取更多信息。

注意 `MapperFactoryBean` 需要配置一个 `SqlSessionFactory` 或 `SqlSessionTemplate`。它们可以分别通过 `sqlSessionFactory` 和 `sqlSessionTemplate`
属性来进行设置。 如果两者都被设置，`SqlSessionFactory` 将被忽略。由于 `SqlSessionTemplate` 已经设置了一个 session 工厂，`MapperFactoryBean` 将使用那个工厂。

### Java 配置

```java

@Configuration
public class MyBatisConfig {
    @Bean
    public MapperFactoryBean<UserMapper> userMapper() throws Exception {
        MapperFactoryBean<UserMapper> factoryBean = new MapperFactoryBean<>(UserMapper.class);
        factoryBean.setSqlSessionFactory(sqlSessionFactory());
        return factoryBean;
    }
}
```

## 自动扫描映射器

不需要一个个地注册你的所有映射器。你可以让 MyBatis-Spring 对类路径进行扫描来发现它们。

有几种办法来发现映射器：

- 使用 `<mybatis:scan/>` 元素
- 使用 `@MapperScan` 注解
- 在经典 Spring XML 配置文件中注册一个 `MapperScannerConfigurer`

`<mybatis:scan/>` 和 `@MapperScan` 都在 MyBatis-Spring 1.2.0 中被引入。`@MapperScan` 需要你使用 Spring 3.1+。

### <mybatis:scan>

`<mybatis:scan/>` 元素会发现映射器，它发现映射器的方法与 Spring 内建的 `<context:component-scan/>` 发现 bean 的方法非常类似。

下面是一个 XML 配置样例：

```xml

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        <!-- 引入命名空间 -->
        xmlns:mybatis="http://mybatis.org/schema/mybatis-spring"
        xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring.xsd">

<mybatis:scan base-package="org.mybatis.spring.sample.mapper" />

        <!-- ... -->

        </beans>
```

`base-package` 属性允许你设置映射器接口文件的基础包。通过使用逗号或分号分隔，你可以设置多个包。并且会在你所指定的包中递归搜索映射器。

注意，不需要为 `<mybatis:scan/>` 指定 `SqlSessionFactory` 或 `SqlSessionTemplate`，这是因为它将使用能够被自动注入的 `MapperFactoryBean`
。但如果你正在使用多个数据源（`DataSource`），自动注入可能不适合你。 在这种情况下，你可以使用 `factory-ref` 或 `template-ref` 属性指定你想使用的 bean 名称。

`<mybatis:scan/>` 支持基于标记接口或注解的过滤操作。在 `annotation` 属性中，可以指定映射器应该具有的特定注解。而在 `marker-interface`
属性中，可以指定映射器应该继承的父接口。当这两个属性都被设置的时候，被发现的映射器会满足这两个条件。 默认情况下，这两个属性为空，因此在基础包中的所有接口都会被作为映射器被发现。

被发现的映射器会按照 Spring
对自动发现组件的默认命名策略进行命名（参考 [the Spring reference document(Core Technologies -Naming autodetected components-)](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-scanning-name-generator) ）。
也就是说，如果没有使用注解显式指定名称，将会使用映射器的首字母小写非全限定类名作为名称。但如果发现映射器具有 `@Component` 或 JSR-330 标准中 `@Named`
注解，会使用注解中的名称作为名称。 提醒一下，你可以设置 `annotation` 属性为你自定义的注解，然后在你的注解上设置 `org.springframework.stereotype.Component`
或 `javax.inject.Named`（需要使用 Java SE 6 以上）注解，这样你的注解既可以作为标记，也可以作为一个名字提供器来使用了。

> **提示** `<context:component-scan/>` 无法发现并注册映射器。映射器的本质是接口，为了将它们注册到 Spring
> 中，发现器必须知道如何为找到的每个接口创建一个 `MapperFactoryBean`。

### @MapperScan

当你正在使用 Spring 的基于 Java 的配置时（也就是 `@Configuration`），相比于使用 `<mybatis:scan/>`，你会更喜欢用 `@MapperScan`。

`@MapperScan` 注解的使用方法如下：

```java

@Configuration
@MapperScan("org.mybatis.spring.sample.mapper")
public class AppConfig {
    // ...
}
```

这个注解具有与之前见过的 `<mybatis:scan/>` 元素一样的工作方式。它也可以通过 `markerInterface` 和 `annotationClass` 属性设置标记接口或注解类。
通过配置 `sqlSessionFactory` 和 `sqlSessionTemplate` 属性，你还能指定一个 `SqlSessionFactory` 或 `SqlSessionTemplate`。

### MapperScannerConfigurer

`MapperScannerConfigurer` 是一个 `BeanDefinitionRegistryPostProcessor`，这样就可以作为一个 bean，包含在经典的 XML
应用上下文中。为了配置 `MapperScannerConfigurer`，使用下面的 Spring 配置：

```xml

<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="org.mybatis.spring.sample.mapper" />
</bean>
```

如果你需要指定 `sqlSessionFactory` 或 `sqlSessionTemplate`，那你应该要指定的是 **bean 名**而不是 bean 的引用，因此要使用 `value` 属性而不是通常的 `ref`
属性：

```xml

<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
```

**提示** 在 MyBatis-Spring 1.0.2 之前，`sqlSessionFactoryBean` 和 `sqlSessionTemplateBean` 属性是唯一可用的属性。 但由于 `MapperScannerConfigurer`
在启动过程中比 `PropertyPlaceholderConfigurer` 运行得更早，经常会产生错误。基于这个原因，上述的属性已被废弃，现在建议使用 `sqlSessionFactoryBeanName`
和 `sqlSessionTemplateBeanName` 属性。

---

# ------------------------------------

---

# Mybatis-plus

## 安装: 依赖

maven

```xml
<!-- Spring Boot  -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.2</version>
</dependency>
        <!-- Spring MVC -->
<dependency>
<groupId>com.baomidou</groupId>
<artifactId>mybatis-plus</artifactId>
<version>3.4.2</version>
</dependency>
```

gradle

```groovy
compile group: 'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.4.2'
compile group: 'com.baomidou', name: 'mybatis-plus', version: '3.4.2'
```

## 配置

添加 `@MapperScan` 注解, 其实就是Mybatis 的配置

更换 Mybatis 的 SqlSessionFactory 为 mybatis plus 的 `com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean`


## 注解配置

```java
@TableName
@TableId
// @TableField(exist = "False") 排除字段
@TableField
// 乐观锁
@Version
// 逻辑删除
@TableLogic
// 枚举类型字段
@EnumValue
// 主键策略
@KeySequence

```

## lambda 调用 Wrapper

### 获取 lambda 对象

```java
//创建lambda 条件构造器的方法
LambdaQueryWrapper<User> lambda1=Wrappers.<User>.lambdaQuery();
        LambdaQueryWrapper<User> lambda1=new LambdaQueryWrapper<>();
        LambdaQueryWrapper<User> lambda2=new QueryWrapper<User>().lambda();

        // MP 3.0.7 新增的方式
        new LambdaQueryChainWrapper<User>(userMapper)
        .eq(User::getNickName,"name")
        .list()
        .forEach(i->log.info(i.toString()));
```

### QueryWrapper 常用方法

```java
new LambdaQueryChainWrapper<User>(userMapper)
        .eq()    // 等于
        .ne()    // 不等于
        .lt()    // 小于
        .le()    // 小于等于
        .gt()    // 大于
        .ge()    // 大于等于
        .in()
        .isNUll()
        .between()
        .notBetween()
        .like()
        .notLike()
        .likeLeft()
        .likeRight()
        .isNull()
        .isNotNull()
        .in()
        .notIn()
        .and()
        .or()
        // 拼接语句, 有SQL注入的风险
        .first()
        .last()
// ...
```

### 分页查询

添加分页拦截器

```java

@Configuration
@EnableTransactionManagement
public class MybatisPlusConfig {
    // 旧版: 不推荐使用
    // @deprecated 3.4.0 please use {@link MybatisPlusInterceptor} {@link PaginationInnerInterceptor}    
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join
        paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
        return paginationInterceptor;
    }

    // 最新版
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
        return interceptor;
    }
}
```

正常查询即可, 使用注入的方法或者入参含 `IPage<>` 的自定义接口

使用注入的方法, 返回值是一个 `IPage<>` 对象

```java
new LambdaQueryChainWrapper<>(mapper)
        .page(new Page<>(1,2,false))
        .getRecords()
        .forEach(i->log.warn(i.toString()));

        mapper.selectMapsPage(new Page<>(1,3,false),
        new QueryWrapper<User>().orderByDesc("id"))
        .getRecords()
        .forEach(i->log.info(i.toString()));
```

自定义方法, 入参包含 `IPage<>` 即可

```java
public Collection<> selectPage(IPage<> page,@Param(Constants.WRAPPER) QueryWrapper<>);
```

`${ew.customSqlSegment}$`

```xml

<select id="selectPage" resultType="com.mp.entity.User">
    select * from user ${ew.customSqlSegment}
</select>
```

## 关于枚举

Mybatis Plus 需要配置注解类型处理器和注解包

注解属性字段上添加 `@EnumValue` 注解

Spring MVC 入参 ， 枚举类型使用枚举变量的变量名， 不是变量值。

类型转换不明。
