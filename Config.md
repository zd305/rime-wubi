# 配置说明

## `new_spelling`（拆分注释开关）

`new_spelling` 是方案里的 **Rime 开关项**（`switches` 中的 `name`），由 `lua_filter@new_spelling@spelling`（实现文件 `lua/new_spelling.lua`）读取，用来控制是否在候选上附加 **五笔拆分 / 字根 / 编码** 等注解信息。

### 界面状态（`states`）

在 `wubi.schema.yaml`、`wubi_pinyin.schema.yaml` 等处定义为：

- **隐**：关闭附加注解；正常组词时候选的注释保持上游结果，不通过本过滤器追加拆分类内容（拼音反查以 `z` 开头时也不再套一层拆分注释）。
- **显**：打开附加注解；根据当前输入上下文，为候选补充 `lua_reverse_db` 反查库中的拆分、多字词组拼等提示（与 `new_hide_pinyin` 等其它开关共同影响注释格式）。

默认由方案里的 `reset: 1` 决定初始落在第几个状态（本仓库中为第二项 **显**，即默认开启拆分注释）。

### 与 `rime.lua` 的关系

`rime.lua` 中 `spelling_keyword="new_spelling"` 把「拆分」这一能力在 **活动开关菜单**（如 `candidate_keywords` 里的「拆分」项）里与同一开关名对齐，便于在切换器里识别与切换。

### 数据依赖

过滤器初始化时会读取当前方案中的 `lua_reverse_db/spelling` 与 `lua_reverse_db/code`（例如纯五笔方案指向 `wb_spelling`），并加载对应的 `.reverse.bin`。若未正确部署依赖词典，拆分注释可能为空或异常。

### 快捷键（因方案而异）

- **纯五笔** `wubi.schema.yaml`：`Control+Shift+H` 用于 `toggle: new_spelling`。
- **五笔拼音混输** `wubi_pinyin.schema.yaml`：未绑定该快捷键时，可通过 **F4** 方案选单中的开关项切换「隐 / 显」。
