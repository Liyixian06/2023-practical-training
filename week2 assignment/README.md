# week 2 小组作业

以小组为单位，完成以下任务：

从（企业）组织架构、社交网络、产品信息、明星关系等四个领域，任选一个完成。

具体要求：

1. 识别并抽取、设计该领域的实体和其间的关系，分别从：图数据库、关系型数据的角度，进行描述。

2. 使用Neo4j落地上述的设计，要求编程实现实体及相应关系的创建和编辑等操作

3. 使用MySQL落地上述的设计，要求编程实现实体及相应关系的创建和编辑等操作

4. 自行拓展实现任意功能，包括但不限于：

上述二者数据库中数据的复杂查询（多表查询、复杂关系查询）、

模拟场景应用（比如：推荐、深层关联挖掘）实现功能、

将上述前述功能封装到一个应用（如：web项目）中，整体展示、等等

  

## 数据来源

*[disease1.csv](https://github.com/Liyixian06/2023-practical-training/blob/main/neo4j/data/disease1.csv)*

  

## 环境配置

neo4j py2neo

MySQL mysqlclient

## 图数据库

### 图数据库描述

图数据库是一种专门用于处理图形数据的数据库系统。它通过使用图结构来存储和管理数据，并提供了高效的图遍历和查询能力。图数据库中的数据由节点和边组成，节点表示实体或对象，边表示节点之间的关系。图数据库的主要目标是快速查询和分析节点之间的关系，以及处理复杂的图形数据模式。它广泛应用于社交网络分析、知识图谱、推荐系统等领域。

  

图的基础：节点、关系与属性

节点：图中的对象，可带若干名-值属性，可带标签

关系：连接节点（有类型、带方向），可带若干名-值属性

  

函数`create_diseases_nodes`则专门用于创建疾病节点，并为每个节点设置属性。在函数中，使用`Node`对象创建具有相应属性的疾病节点，并通过调用`graph.create`将节点添加到图数据库中。

函数`create_node`用于创建一个特定标签和节点名称的节点。它接收两个参数：`label`表示节点的标签，`nodes`是一个节点名称的列表。代码使用`for`循环遍历`nodes`列表，对于每个节点名称，创建一个具有指定标签和名称的节点，并通过`self.graph.create(node)`将节点添加到图数据库中。

函数`create_diseases_nodes`用于创建疾病节点及其属性。它接收一个疾病信息的列表`disease_info`作为参数。代码使用`for`循环遍历`disease_info`列表中的每个疾病字典。对于每个疾病字典，根据字典中的键值对创建一个具有特定属性的疾病节点，并将节点添加到图数据库中。

函数`create_graphNodes`是整个过程的入口函数。它调用`read_file`函数来读取相关文件并获取所需的数据。然后，它依次调用`create_diseases_nodes`和`create_node`函数来创建疾病、症状、别名、部位、科室、并发症和药物等节点，并将它们添加到图数据库中。

函数`create_relationship`用于创建实体之间的关系边，首先通过将边的起始节点和结束节点连接为字符串，并将其添加到一个集合中来进行去重处理。然后，使用一个循环遍历集合中的每个边。对于每个边，它将其拆分为起始节点和结束节点，并构建一个Cypher查询语句。查询语句使用`MATCH`子句匹配起始节点和结束节点，然后使用`CREATE`子句创建起始节点与结束节点之间的关系，并指定关系类型和名称。最后，通过调用`self.graph.run(query)`来执行查询语句，将关系添加到图数据库中。

函数`create_relationship("Disease", "Alias", rel_alias, "ALIAS_IS", "别名") `表示创建疾病节点与别名节点之间的关系，关系类型为 "ALIAS_IS"，关系名称为 "别名"，症状、部位、科室、并发症和药物之间的关系仿照其实现。

函数`create_graphRels`调用`create_relationship`函数来创建各种实体之间的关系，`create_relationship`函数传递以下参数：起始节点类型（start_node）、结束节点类型（end_node）、边的列表（edges）、关系类型（rel_type）和关系名称（rel_name）。

  

### 图数据库编辑

- 查
  - 获取疾病节点
  - 查询与给定疾病相关的所有药品和症状
  
- 增
  - 更新疾病节点属性
    
- 删
  - 删除疾病 
- 改
  - 更新疾病
    

## 关系型数据库

### 关系型数据库描述

关系型数据库是一种以关系模型为基础的数据库系统。它使用表格（关系）来组织和表示数据，并使用结构化查询语言（SQL）进行数据管理和查询。关系型数据库的核心思想是将数据分解为多个表格，每个表格包含行和列，其中行表示记录，列表示属性。关系型数据库通过定义表之间的关系（主键、外键等）来建立数据之间的联系。它具有事务处理能力和数据一致性保证，适用于大规模数据存储和复杂的数据操作需求。

  
- `疾病（Disease）`
    - `名称（name）`：字符字段，最大长度为255
    - `别名（altername）`：字符字段，最大长度为255（可选）
    - `受影响人群（people）`：字符字段，最大长度为255（可选）
    - `传染性（infectivity）`：整数字段
    - `保险覆盖人数（under_insurance）`：整数字段

- `科室（Department）`
    - `科室名称（department）`：字符字段，最大长度为255

- `疾病科室（DiseaseDepartment）`
    - `疾病ID（disease）`：与`疾病（Disease）`表相关的外键
    - `科室ID（department）`：与`科室（Department）`表相关的外键

- `药物（Medicine）`
    - `名称（name）`：字符字段，最大长度为255
    - `不良反应（adverse_reaction）`：字符字段，最大长度为255（可选）
    - `用法指导（instruction）`：字符字段，最大长度为255（可选）

- `疾病药物（DiseaseMedicine）`
    - `疾病ID（disease）`：与`疾病（Disease）`表相关的外键
    - `药物ID（medicine）`：与`药物（Medicine）`表相关的外键

- `症状（Symptoms）`
    - `症状（symptom）`：字符字段，最大长度为255
    - `部位（part）`：字符字段，最大长度为255

- `疾病症状（DiseaseSymptom）`
    - `疾病ID（disease）`：与`疾病（Disease）`表相关的外键
    - `症状ID（symptom）`：与`症状（Symptoms）`表相关的外键

- `治疗方法（Treatments）`
    - `治疗方法（treatment）`：字符字段，最大长度为255

- `疾病治疗（DiseaseTreatment）`
    - `疾病ID（disease）`：与`疾病（Disease）`表相关的外键
    - `治疗方法ID（treatment）`：与`治疗方法（Treatments）`表相关的外键

其中每个表都有自增id为主键  

### 关系型数据库编辑
`/data `路径存储需要读入的csv文件

`insert_data.py`文件插入数据

`/index` 页面显示主要功能

- 增
  - 增加新的疾病及其属性

- 删
  - 删除整个疾病，删除特定属性

- 改
  - 修改疾病名称及其属性

- 查
  - 查找疾病

属性为：疾病名，又名，患病人群，有无传染性，是否在医保范围

可修改的属性不涉及相关操作
