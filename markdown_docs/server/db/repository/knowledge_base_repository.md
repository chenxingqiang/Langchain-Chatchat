## FunctionDef add_kb_to_db(session, kb_name, kb_info, vs_type, embed_model)

**add_kb_to_db**: 此函数的功能是向数据库中添加或更新知识库信息。

**参数**:

- `session`: 数据库会话实例，用于执行数据库操作。
- `kb_name`: 知识库的名称，用作知识库的唯一标识。
- `kb_info`: 知识库的简介信息，提供关于知识库的基本描述。
- `vs_type`: 向量库类型，指定知识库使用的向量库的类型。
- `embed_model`: 嵌入模型名称，指定用于知识库的嵌入模型。

**代码描述**:
此函数首先尝试查询数据库中是否存在与给定知识库名称相匹配的知识库实例。如果不存在，函数将创建一个新的`KnowledgeBaseModel`实例，并使用提供的参数填充其属性，然后将此新实例添加到数据库中。如果已存在具有相同名称的知识库，函数将更新该知识库的简介(`kb_info`)、向量库类型(`vs_type`)和嵌入模型(`embed_model`)信息。无论是添加新知识库还是更新现有知识库，此函数最终都会返回`True`，表示操作成功。

**注意**:

- 确保传入的`session`是一个有效的数据库会话实例，以允许函数执行数据库操作。
- 在调用此函数之前，应确保`kb_name`是唯一的，以避免不必要的知识库信息覆盖。
- 此函数不负责提交数据库会话，调用者需要在调用此函数后，根据自己的需求决定是否提交会话。

**输出示例**:
由于此函数的返回值是布尔类型，因此在成功执行添加或更新操作后，它将返回`True`。例如，无论是创建了一个新的知识库还是更新了现有的知识库信息，函数调用`add_kb_to_db(session, '技术文档库', '存储技术相关文档', 'ElasticSearch', 'BERT')`都将返回`True`。

## FunctionDef list_kbs_from_db(session, min_file_count)

**list_kbs_from_db**: 此函数的功能是从数据库中列出满足特定条件的知识库名称列表。

**参数**:

- `session`: 数据库会话对象，用于执行数据库查询。
- `min_file_count`: 文件数量的最小值，默认为-1，表示不对文件数量做限制。

**代码描述**:
`list_kbs_from_db` 函数通过传入的数据库会话对象 `session` 来查询 `KnowledgeBaseModel` 中的知识库名称。它使用了过滤条件，只有当知识库中的文件数量大于 `min_file_count` 参数指定的值时，该知识库才会被包含在结果列表中。查询结果首先是一个包含多个元组的列表，每个元组中的第一个元素是知识库名称。然后，通过列表推导式，将这些元组转换为仅包含知识库名称的列表。最终，函数返回这个知识库名称列表。

**注意**:

- 在使用此函数时，需要确保传入的 `session` 对象是有效的数据库会话对象，并且已经正确配置了数据库连接。
- `min_file_count` 参数允许调用者根据文件数量过滤知识库，可以根据实际需求调整其值。如果不需要基于文件数量过滤知识库，可以保留其默认值。

**输出示例**:
假设数据库中有三个知识库，文件数量分别为0、10、20，且 `min_file_count` 参数的值为5，那么函数的返回值可能如下：

```
['知识库B', '知识库C']
```

这表示只有文件数量大于5的知识库B和知识库C被包含在了结果列表中。

## FunctionDef kb_exists(session, kb_name)

**kb_exists**: 此函数的功能是检查数据库中是否存在指定名称的知识库。

**参数**:

- `session`: 数据库会话对象，用于执行数据库查询。
- `kb_name`: 要检查的知识库名称。

**代码描述**:
`kb_exists` 函数通过接收一个数据库会话对象和一个知识库名称作为参数，来检查数据库中是否存在具有该名称的知识库。函数内部首先使用传入的会话对象执行一个查询，该查询利用`KnowledgeBaseModel`模型对数据库中的`knowledge_base`表进行过滤，查找名称与`kb_name`参数相匹配的知识库记录。这里使用`ilike`方法进行不区分大小写的匹配，以提高查询的灵活性。如果查询结果中存在至少一个匹配的记录，则函数返回`True`，表示指定名称的知识库存在；如果没有找到匹配的记录，则返回`False`，表示知识库不存在。

**注意**:

- 在使用`kb_exists`函数时，需要确保传入的`session`对象是有效的数据库会话对象，并且已正确配置数据库连接。
- 传入的`kb_name`应为字符串类型，且在调用此函数前，最好进行必要的格式化或验证，以确保查询的准确性。
- 此函数的返回值是布尔类型，可以直接用于条件判断。

**输出示例**:
假设数据库中存在一个名为"技术文档库"的知识库，调用`kb_exists(session, "技术文档库")`将返回`True`。如果查询一个不存在的知识库名称，如`kb_exists(session, "不存在的库")`，则会返回`False`。

此函数在项目中的应用场景包括，但不限于，在添加新的知识库之前检查同名知识库是否已存在，或在执行知识库相关操作前验证知识库的存在性，以确保数据的一致性和操作的有效性。

## FunctionDef load_kb_from_db(session, kb_name)

**load_kb_from_db**: 此函数的功能是从数据库中加载指定名称的知识库信息。

**参数**:

- `session`: 数据库会话实例，用于执行数据库查询。
- `kb_name`: 要加载的知识库的名称。

**代码描述**: `load_kb_from_db` 函数通过接收一个数据库会话和一个知识库名称作为参数，利用这个会话来查询 `KnowledgeBaseModel` 表中名称与给定名称相匹配的第一个知识库记录。查询时忽略名称的大小写。如果找到相应的知识库记录，函数将从该记录中提取知识库的名称(`kb_name`)、向量库类型(`vs_type`)和嵌入模型名称(`embed_model`)。如果没有找到相应的记录，这三个变量将被设置为 `None`。最后，函数返回这三个值。

**注意**:

- 在调用此函数之前，确保传入的 `session` 是有效的数据库会话实例，并且已经正确配置。
- 传入的知识库名称 `kb_name` 应当是一个字符串，且在数据库中有对应的记录，否则函数将返回 `None` 值。
- 此函数对知识库名称大小写不敏感，即不区分大小写。

**输出示例**:
假设数据库中存在一个名为 "技术文档库" 的知识库，其向量库类型为 "ElasticSearch"，嵌入模型名称为 "BERT"，那么调用 `load_kb_from_db(session, "技术文档库")` 将返回：

```
("技术文档库", "ElasticSearch", "BERT")
```

如果数据库中不存在指定名称的知识库，调用 `load_kb_from_db(session, "不存在的库")` 将返回：

```
(None, None, None)
```

## FunctionDef delete_kb_from_db(session, kb_name)

**delete_kb_from_db**: 此函数的功能是从数据库中删除指定的知识库。

**参数**:

- `session`: 数据库会话实例，用于执行数据库操作。
- `kb_name`: 要删除的知识库的名称。

**代码描述**:
`delete_kb_from_db` 函数首先通过传入的 `session` 参数和知识库名称 `kb_name`，使用 `query` 方法查询 `KnowledgeBaseModel` 表中是否存在指定名称的知识库。查询时，使用了 `ilike` 方法来实现不区分大小写的匹配，以提高用户体验和容错性。如果查询到了目标知识库，那么使用 `session.delete` 方法将其从数据库中删除。无论是否找到并删除了知识库，函数最终都会返回 `True`，表示操作已完成。

**注意**:

- 在调用此函数时，需要确保传入的 `session` 是一个有效的数据库会话实例，且已正确配置数据库连接。
- 传入的知识库名称 `kb_name` 应当是一个字符串类型，且在数据库中唯一标识一个知识库。
- 函数执行后并不会自动提交数据库事务，调用方需要根据实际情况决定是否提交事务。

**输出示例**:
由于此函数的返回值是固定的 `True`，因此不提供具体的输出示例。调用此函数后，可以根据返回值确认操作已被执行，但需要通过其他方式（如查询数据库）来验证知识库是否真的被成功删除。

此函数在项目中的应用场景包括，但不限于，在知识库管理服务中，当用户或管理员请求删除一个知识库时，会通过调用此函数来执行删除操作。例如，在 `KBService` 类的 `drop_kb` 方法中，就通过调用 `delete_kb_from_db` 函数来实现知识库的删除功能。这样的设计使得知识库的删除操作既可以独立使用，也可以轻松集成到更复杂的服务流程中。

## FunctionDef get_kb_detail(session, kb_name)

**get_kb_detail**: 此函数的功能是获取指定知识库的详细信息。

**参数**:

- `session`: 数据库会话实例，用于执行数据库查询。
- `kb_name`: 字符串类型，指定要查询的知识库名称。

**代码描述**:
`get_kb_detail` 函数通过接收一个数据库会话实例和一个知识库名称作为参数，利用这个会话实例查询 `KnowledgeBaseModel` 表中名称与给定知识库名称相匹配的记录。查询时忽略大小写，以提高查询的灵活性。如果找到匹配的知识库记录，则函数将返回一个包含知识库名称 (`kb_name`)、知识库简介 (`kb_info`)、向量库类型 (`vs_type`)、嵌入模型名称 (`embed_model`)、文件数量 (`file_count`) 和创建时间 (`create_time`) 的字典。如果没有找到匹配的记录，则返回一个空字典。

此函数在项目中被多个位置调用，包括但不限于加载知识库嵌入向量和获取知识库详情列表。这些调用场景表明 `get_kb_detail` 函数是连接数据库知识库信息与项目其他部分的重要桥梁。

**注意**:

- 确保传入的 `session` 参数是一个有效的数据库会话实例，且在调用此函数前已正确配置和连接到数据库。
- 传入的 `kb_name` 应为字符串类型，且尽量确保其准确性，以便能够正确匹配数据库中的记录。

**输出示例**:
假设数据库中存在一个名为 "技术文档库" 的知识库，其详细信息如下所示：

```python
{
    "kb_name": "技术文档库",
    "kb_info": "存储技术相关文档的知识库",
    "vs_type": "ElasticSearch",
    "embed_model": "BERT",
    "file_count": 100,
    "create_time": "2023-04-01T12:00:00"
}
```

如果查询的知识库名称为 "技术文档库"，则 `get_kb_detail` 函数将返回上述字典。如果没有找到匹配的知识库，则函数返回 `{}`。
