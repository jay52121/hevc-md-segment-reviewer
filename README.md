# HEVC · MD 视频片段审阅器

离线单文件工具，用于对照本地视频审阅模型 Markdown 中的分类片段、情绪分和判断依据。以用户上传的原文件为基础，作为项目辅助审阅工具独立维护。原始版本保留在 Git 历史中。

## 使用方法

在线使用：[打开审阅器](https://jay52121.github.io/hevc-md-segment-reviewer/)。视频和 MD 仍在浏览器本地读取。

1. 也可下载本目录的 `hevc_md_segment_reviewer.html`，用浏览器打开，无需安装依赖或启动服务。
2. 分别点击「选择视频」和「加载 MD」，载入对应的本地视频与 Markdown，先后顺序不限。两个文件独立选择，HTML 不内置视频或识别数据，也不上传文件。
3. 在右侧选择分类、点击时间段播放；用「上一段／下一段」或 ↑ / ↓ 切换当前分类中的片段。输入框或分类菜单获得焦点时不拦截方向键。
4. 视频尺寸可选 1×（播放器可用宽度）、0.5×、适应当前窗口高度。拖动视频进度或连续播放时，解析区跟随当前时间显示识别结果，不受右侧分类筛选限制；空档显示无对应记录。
5. 可调整倍速，并选择是否在片段结束时自动暂停。

## HEVC 播放限制

播放器依赖浏览器、操作系统和设备的原生 HEVC 解码能力，不包含解码器或转码功能。原页面建议 macOS 使用 Safari；能否播放仍取决于具体环境、视频封装及编码配置，选择成功不代表一定可解码。

## Markdown 标记约定

只读取下列两组标记之间的完整 `json` 围栏代码块，标记中的空格和名称应保持一致；不从正文标题推断片段，也不假设固定片段长度。

- `<!-- LABEL_DEFINITIONS_JSON_BEGIN -->` … `<!-- LABEL_DEFINITIONS_JSON_END -->`：包含 `labels` 对象和字符串 `headerPattern`。每个标签可定义 `name`、`group`；正则第 1 捕获组为标签代码，第 2 捕获组为情绪值（`-` 表示未知）。`requiresNonemptyBody` 默认要求判断依据非空。
- `<!-- PLAYER_DATA_JSON_BEGIN -->` … `<!-- PLAYER_DATA_JSON_END -->`：包含 `timeUnit: "seconds"` 和 `records` 数组，可提供 `durationSeconds`。每条记录包含唯一的零基整数 `index`、数值 `startSeconds` / `endSeconds`（开始非负、结束大于开始）、`rawAnswer` 和 `compactLabel: {code, emotion}`。

`rawAnswer` 首行必须匹配 `headerPattern`，其余正文为判断依据；`compactLabel` 必须与首行代码、情绪一致，未知情绪用 `null`。标签不硬编码，按 MD 定义显示；回复格式异常单列为「格式无效」，无效时间记录会跳过。

只将工具源码与说明纳入版本维护；视频和包含个人信息的实际分析记录留在本地。
