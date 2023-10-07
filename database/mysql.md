



### 最左匹配原则

```
解释一下最左前缀原则：
2.当b+树的数据项是复合的数据结构，比如(name,age,sex)的时候，b+数是按照从左到右的顺序来建立搜索树的，比如当(张三,20,F)这样的数据来检索的时候，b+树会优先比较name来确定下一步的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当(20,F)这样的没有name的数据来的时候，b+树就不知道下一步该查哪个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。比如当(张三,F)这样的数据来检索时，b+树可以用name来指定搜索方向，但下一个字段age的缺失，所以只能把名字等于张三的数据都找到，然后再匹配性别是F的数据了， 这个是非常重要的性质，即索引的最左匹配特性
```



### 网络货运优化表实录

#### 数据量 17636893

##### 问题描述sql展示

1.等号左边中有时间函数DATE_FORMAT，可能导致索引失效

2.搜索条件中 != 情况，可能使索引失效

```sql
SELECT ROUND(SUM(yse)/10000,2) yse,ROUND(SUM(yl)/1000/10000,2) yl,DATE_FORMAT(sww.biz_time,'%Y-%m') ny
        FROM sowin_wljg_wlhy sww
        <where>
            <if test="areaName != '' and areaName != null">
                AND sww.mdsf = #{areaName}
                AND sww.qssf != #{areaName}
            </if>
            AND is_delete = 0
        </where>
        GROUP BY DATE_FORMAT(sww.biz_time,'%Y-%m')
        ORDER BY DATE_FORMAT(sww.biz_time,'%Y-%m') DESC
```



##### 解决方案

1.添加组合索引



2.限制数据搜索范围



3.剥离相关业务





### 索引

#### 1.索引操作

**创建** ：1.CREATE 2.ALTER 3.建表时创建

- 唯一索引创建 **UNIQUE** 

  CREATE UNIQUE INDEX index_age ON index_test(age)

  唯一索引表示表中该字段只能存在一个值，不能出现相同的值

- 主键索引 **PRIMARY**

​		**ALTER** **TABLE** tableName **ADD** **PRIMARY** KEY indexName(columnName);

- 全文索引 **FULLTEXT**

  CREATE FULLTEXT INDEX indexName **ON** tableName(columnName);

​		创建全文索引有三个条件 1.存储引擎必须为MyISAM 2.创建的字段必须为VARCHAR、TEXT等文本类型 3.





**查看**：SHOW INDEX FROM tableName

**删除**：DROP INDEX indexName ON tableName 

 FORCE INDEX 强制走索引



#### 2.索引分类

##### 2.1、数据结构层次

- B+Tree
- Hash
- R-Tree
- T-Tree

B+树和哈希索引使用最常见，Mysql中默认创建B+Tree

##### 2.2 、字段数量层次

**单列索引**

- 唯一索引：指索引中的索引节点值不允许重复，一般配合唯一约束使用
- 主键索引：主键索引是一种特殊的唯一索引，和普通索引的唯一区别就是不允许有空值
- 普通索引：通过KEY、INDEX关键字创建的索引
- 其他

**多列索引**

- 组合索引：由多个字段组合建立的索引

使用注意事项：当建立多列索引后，一条`SELECT`语句，只有当查询条件中了包含了多列索引的第一个字段时，才能使用多列索引。

如：建立了id,name,age的组合索引，只有在where条件中有id搜索条件时才会触发

##### 2.3 功能逻辑层次

- 普通索引、唯一索引、主键索引、全文索引、空间索引

全文索引：类似ES的分词器，只

空间索引：



##### 2.4 存储方式层次

- 聚簇索引：逻辑上连续且物理上连续
- 非聚簇索引：逻辑上连续但物理上不连续



一张表中只能存在一个聚簇索引
