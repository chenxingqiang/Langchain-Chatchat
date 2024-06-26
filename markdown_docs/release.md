## FunctionDef get_latest_tag

**get_latest_tag**: 此函数的功能是获取Git仓库中最新的标签。

**参数**: 此函数不接受任何参数。

**代码描述**: `get_latest_tag` 函数首先使用 `subprocess.check_output` 方法执行 `git tag` 命令，以获取当前Git仓库中所有的标签。然后，通过对输出结果进行解码（UTF-8）和分割，将其转换成一个标签列表。接下来，使用 `sorted` 函数和一个自定义的排序键，基于标签的版本号（假设遵循 `v主版本号.次版本号.修订号` 的格式）对标签列表进行排序。排序键通过正则表达式 `re.match` 匹配每个标签的版本号，并将其转换为整数元组，以便进行比较。最后，函数返回排序后的最后一个元素，即最新的标签。

在项目中，`get_latest_tag` 函数被 `main` 函数调用，用于获取当前Git仓库中的最新标签，并在终端中显示。此外，`main` 函数还根据用户的输入决定如何递增版本号，并创建新的标签推送到远程仓库。因此，`get_latest_tag` 函数在自动化版本控制和发布流程中起着关键作用，它确保了版本号的正确递增和新版本标签的生成。

**注意**: 使用此函数时，需要确保当前环境已安装Git，并且函数调用是在一个Git仓库的根目录下进行的。此外，此函数假定标签遵循 `v主版本号.次版本号.修订号` 的命名约定，如果标签不遵循此格式，可能无法正确排序和识别最新标签。

**输出示例**: 假设Git仓库中的最新标签为 `v1.2.3`，则函数调用 `get_latest_tag()` 将返回字符串 `"v1.2.3"`。

## FunctionDef update_version_number(latest_tag, increment)

**update_version_number**: 此函数用于根据最新的Git标签和用户指定的版本号递增规则来更新版本号。

**参数**:

- `latest_tag`: 最新的Git标签，字符串格式，预期为`vX.Y.Z`的形式，其中X、Y、Z分别代表主版本号、次版本号和修订号。
- `increment`: 用户指定的版本号递增规则，接受的值为`'X'`、`'Y'`或`'Z'`，分别代表递增主版本号、次版本号或修订号。

**代码描述**:
函数首先通过正则表达式从`latest_tag`中提取出当前的主版本号、次版本号和修订号，并将它们转换为整数。根据`increment`参数的值，函数将相应的版本号部分递增。如果`increment`为`'X'`，则主版本号加一，次版本号和修订号重置为0。如果`increment`为`'Y'`，则次版本号加一，修订号重置为0。如果`increment`为`'Z'`，则修订号加一。最后，函数将更新后的版本号拼接成`vX.Y.Z`的格式并返回。

此函数在项目中被`main`函数调用。在`main`函数中，首先获取当前最新的Git标签，然后询问用户希望递增哪部分版本号（主版本号、次版本号或修订号）。用户输入后，`update_version_number`函数被调用以生成新的版本号。根据用户的确认，新的版本号可能会被用来创建Git标签并推送到远程仓库。

**注意**:

- 输入的`latest_tag`必须严格遵循`vX.Y.Z`的格式，否则正则表达式匹配将失败，函数将无法正确执行。
- `increment`参数仅接受`'X'`、`'Y'`、`'Z'`三个值，任何其他输入都将导致函数无法按预期递增版本号。

**输出示例**:
如果`latest_tag`为`v1.2.3`且`increment`为`'Y'`，则函数将返回`v1.3.0`。

## FunctionDef main

**main**: 此函数的功能是自动化Git版本控制流程，包括获取最新Git标签，递增版本号，并根据用户确认将新版本号作为标签推送到远程仓库。

**参数**: 此函数不接受任何参数。

**代码描述**: `main` 函数首先通过调用 `get_latest_tag` 函数获取当前Git仓库中的最新标签，并将其打印出来。接着，函数提示用户选择要递增的版本号部分（主版本号X、次版本号Y或修订号Z）。用户的选择通过标准输入接收，并转换为大写字母以便后续处理。如果用户输入的不是X、Y或Z中的任何一个，系统会提示错误并要求用户重新输入，直到输入正确为止。

一旦获得有效输入，`main` 函数将调用 `update_version_number` 函数，传入最新的Git标签和用户选择的递增部分，以生成新的版本号。新版本号随后被打印出来，询问用户是否确认更新版本号并推送到远程仓库。用户的确认通过标准输入接收，并转换为小写字母进行判断。

如果用户确认（输入'y'），则使用 `subprocess.run` 方法执行Git命令，首先创建新的版本标签，然后将该标签推送到远程仓库。操作完成后，打印出相应的提示信息。如果用户不确认（输入'n'），则打印出操作已取消的信息。

**注意**:

- 在使用此函数之前，需要确保当前环境已安装Git，并且函数调用是在一个Git仓库的根目录下进行的。
- 用户输入的处理是大小写不敏感的，即输入'X'、'x'均被视为有效输入，并且都会被转换为大写进行处理。
- 在推送新标签到远程仓库之前，函数会要求用户进行确认。这是一个安全措施，以防止意外修改远程仓库。
- 此函数依赖于`get_latest_tag`和`update_version_number`两个函数。`get_latest_tag`用于获取最新的Git标签，而`update_version_number`根据用户指定的递增规则更新版本号。这两个函数的正确执行是`main`函数能够正确工作的基础。
