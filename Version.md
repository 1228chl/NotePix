# 更新日志

所有显著更改都将记录在此文件中。

格式基于[Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)，版本遵循[语义化版本](https://semver.org/lang/zh-CN/)。

## [1.4.6] - 2026-06-02

### 修复
- **重新整理图片序号命令**：修复执行后 `imageCounters` 未正确同步的问题。现在计数器会直接更新为当前图片的实际最大序号，避免下次上传时序号跳跃。
- **删除文件功能**：修复 `deleteFileFromGitHub` 中缺少 `Accept: application/vnd.github.v3+json` 头导致的解析错误（返回图片二进制而非 JSON）。
- **标题编号算法**：完全重写 `generateFileNameFromHeading`，基于文档标题树（`metadataCache`）生成绝对编号（如 `1.1.3`），不再依赖向上行扫描，解决编号过深、重复的问题。
- **粘贴图片扩展名**：修复剪贴板图片（`File` 对象）扩展名为 `.undefined` 的问题，现在正确从 `file.name` 提取。
- **缺失 `captureFilePlaceholder` 方法**：删除无效调用，避免控制台报错。

### 新增
- **公开图片链接格式切换**：支持 `GitHub Raw` 和 `jsDelivr CDN` 两种格式，可在设置中全局选择。
- **右键单张转换链接**：在编辑器或阅读视图中，右键点击图片可快速切换 Raw ↔ CDN 格式。
- **批量转换笔记链接**：设置面板添加“转换当前笔记链接”按钮，一键转换当前笔记中所有图片链接。
- **重新整理图片序号**：新增命令 `NotePix: 重新整理当前笔记的图片序号`，自动检测每个标题层级下的图片序号空缺并重新编号（从 1 开始连续），同时处理重命名、重新上传、链接更新及计数器同步。
- **标题层级最大深度设置**：可限制文件名中最多使用几级标题序号（1-6），超出部分截断。
- **自动上传监控图片开关**：设置界面添加开关，控制是否自动上传监控文件夹内的图片。

### 改进
- **计数器持久化**：`getNextImageCounter` 改为 `async` 并同步保存，避免并发覆盖。
- **文件创建监听优化**：增加 `alreadyConfirmed` 标记，防止粘贴/拖拽图片被文件事件重复处理。
- **占位符替换简化**：`replaceLinkInEditor` 改用简单正则匹配，提升兼容性。
- **代码结构**：添加 `parseImageUrl`, `buildImageUrl`, `extractUrlFromFullMatch`, `fileExistsOnGitHub`, `downloadImageFromGitHub`, `uploadImageData` 等辅助方法，模块化增强。

## [1.4.5] - 2026-05-20

### 修复
- 修复 `uploadPastedImage` 中条件错误导致粘贴图片不自动上传的问题。
- 修复 `deleteFileFromGitHub` 中缺少 `Accept` 头导致删除错误。

### 新增
- 公开图片链接格式选择框架（后端逻辑，界面待后续版本）。

## [1.4.4] - 2026-05-15

### 修复
- 修正 `autoUpload` 逻辑，粘贴图片立即上传，不再依赖文件监控事件。
- 修复 `generateFileNameFromHeading` 中计数器未正确使用 `await` 导致文件名出现 `[object Promise]`。

### 新增
- 设置界面补齐“自动上传监控图片”开关。

## [1.4.3] - 2026-05-10

### 初始复刻版本
- 基于原版 NotePix 复刻，包含基础功能：
  - 粘贴/拖拽图片上传到 GitHub 仓库。
  - 支持公开/私有仓库智能识别。
  - 加密存储 GitHub Token。
  - 移动端适配。
  - 按笔记路径存储图片（`imageStorageStrategy`）。
  - 基于光标向上扫描的标题层级文件名生成（初版，后续版本已重写）。

## [1.0.0] - 原始版本（Ayush Parkara）

### 原始功能
- 自动上传图片到 GitHub 仓库。
- 支持公开/私有模式切换。
- 基本的链接替换。