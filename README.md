# ComfyUI If Branch Router - Matching Passthrough and Output Fork
This fork does two things. For the Router node it allows passthroughs to match the outputs. It also adds a selector node, so that you can place many inputs with a single output based on the int passed to the node. This fork allows you to send different output depending on the input. You will want to use the modified Router node if you intend to send a single consistent output to another node. You will use the Selector node if you have one destination and you want to send many potential outputs to it.
Though it uses "if" truthfully this node is more like a case statement. If i == 1 If i == 2  is really the same as Case i=1: Care i=2:. As far as I can see those node can't do something like "If i > 2 <= 6". 

Original description below (untranslated):

一个用于 ComfyUI 的条件分支节点：输入一个 `condition`，按多个 `if == value` 条件选择输出路径；所有条件都不匹配时，走最后的 `否则` 输出口。

## 安装

把整个 `ComfyUI-IfBranchRouter` 文件夹放到：

```text
ComfyUI/custom_nodes/
```

然后重启 ComfyUI。

## 使用方式

1. 添加节点：`logic/branch > If Branch Router`。
2. 把要判断的 INT 或 STRING 连接到 `condition`。
3. 如果你希望被选中的分支继续传递某个对象，例如 IMAGE、LATENT、MODEL、CONDITIONING，可以把它连接到可选输入 `passthrough`。如果不连接，选中的输出会输出 `condition` 本身。
4. 点击 `添加 if 条件` 或 `减少 if 条件` 来增减条件和输出口。
5. 每一行 `if ==` 对应一个输出口；最后一个输出口始终是 `否则`。

## 比较模式

- `AUTO`：两边都像整数时按 INT 比较，否则按 STRING 比较。
- `INT`：强制按整数比较，无法转成整数的条件不会命中。
- `STRING`：强制按字符串比较。

`string_trim` 可以在字符串比较前去掉首尾空格，`case_sensitive` 控制大小写是否敏感。

## 注意

- 最多支持 32 个 `if` 分支。
- 未命中的输出会返回 ComfyUI 的 `ExecutionBlocker`，用于阻断未选中分支的执行。
- 如果没有加载前端 JS，也可以手动编辑隐藏字段 `conditions_json`，格式为 JSON 数组，例如 `["0", "1", "cat"]`。
