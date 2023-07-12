# 2023-practical-training
# week 2 小组作业
以小组为单位，完成以下任务：
  从（企业）组织架构、社交网络、产品信息、明星关系等四个领域，任选一个完成。
  可以参考：图数据库 Neo4j.pptx
具体要求：
  1. 识别并抽取、设计该领域的实体和其间的关系，分别从：图数据库、关系型数据的角度，进行描述。
  2. 使用Neo4j落地上述的设计，要求编程实现实体及相应关系的创建和编辑等操作
  3. 使用MySQL落地上述的设计，要求编程实现实体及相应关系的创建和编辑等操作
  4. 自行拓展实现任意功能，包括但不限于：
    上述二者数据库中数据的复杂查询（多表查询、复杂关系查询）、
    模拟场景应用（比如：推荐、深层关联挖掘）实现功能、
    将上述前述功能封装到一个应用（如：web项目）中，整体展示、等等

## 数据来源
*[实习实训 Week2 小组作业 data](https://nankai.feishu.cn/sheets/AD5qsuCCohYG6atE5Zicnt7OnGc)*

## Section 1
### 图数据库描述
图数据库是一种专门用于处理图形数据的数据库系统。它通过使用图结构来存储和管理数据，并提供了高效的图遍历和查询能力。图数据库中的数据由节点和边组成，节点表示实体或对象，边表示节点之间的关系。图数据库的主要目标是快速查询和分析节点之间的关系，以及处理复杂的图形数据模式。它广泛应用于社交网络分析、知识图谱、推荐系统等领域。

图的基础：节点、关系与属性
节点：图中的对象，可带若干名-值属性，可带标签
关系：连接节点（有类型、带方向），可带若干名-值属性

创建节点：
创建疾病节点（Disease）：创建包含id、疾病名、又名、患病人群、传染性和是否在医保范围属性的节点。
创建症状节点（Symptom）：创建包含id、部位和症状属性的节点。
创建药物节点（Medication）：创建包含id、药物名、副作用、服用指导和费用属性的节点。
创建治疗节点（Treatment）：创建包含id和名字属性的节点。
创建科室节点（Department）：创建包含id和科室名属性的节点。

创建关系：
创建疾病和症状之间的关系（Disease_Symptom）：在疾病节点和症状节点之间创建关系，使用disease_id和symptom_id属性作为关系的属性。
创建疾病和药物之间的关系（Disease_Medication）：在疾病节点和药物节点之间创建关系，使用disease_id和medication_id属性作为关系的属性。
创建疾病和治疗之间的关系（Disease_Treatment）：在疾病节点和治疗节点之间创建关系，使用disease_id和treatment_id属性作为关系的属性。
创建疾病和科室之间的关系（Disease_Department）：在疾病节点和科室节点之间创建关系，使用disease_id和department_id属性作为关系的属性。

### 关系型数据库描述
关系型数据库是一种以关系模型为基础的数据库系统。它使用表格（关系）来组织和表示数据，并使用结构化查询语言（SQL）进行数据管理和查询。关系型数据库的核心思想是将数据分解为多个表格，每个表格包含行和列，其中行表示记录，列表示属性。关系型数据库通过定义表之间的关系（主键、外键等）来建立数据之间的联系。它具有事务处理能力和数据一致性保证，适用于大规模数据存储和复杂的数据操作需求。

