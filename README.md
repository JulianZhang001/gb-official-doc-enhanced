# 国标公文增强校排 Skill

这是一个面向 Codex/Agent 使用的中文公文写作、审校与 Word 一键排版 Skill。它可以把普通 `.docx` 文档按 GB/T 9704-2012《党政机关公文格式》和 GB/T 15834-2011《标点符号用法》进行正文模式校排，并生成 Markdown 审校报告。

## 来源说明

本项目是在 GitHub 作者 **KaguraNanaga** 的开源项目基础上整理和增强的试用版，并结合本地已有公文校排 Skill 做了适配。

主要参考/整合来源：

- `KaguraNanaga/document-format-skills`：通用中文 Word 文档格式诊断、清理、预设排版和 Markdown/txt 转 DOCX 能力。
- `KaguraNanaga/docformat-gui`：公文格式 GUI 工具中的预设配置和模板思路。

上游项目采用 MIT License。相关许可证文本已保存在：

- `references/LICENSE.document-format-skills.txt`
- `references/LICENSE.docformat-gui.txt`

本项目保留原作者来源说明，并在此基础上做了 Skill 结构整理、入口脚本增强、冲突规则裁决和本地示例校排。

## 主要能力

- `.docx` 公文正文模式一键校排。
- 自动设置 A4 页面、页边距、标题、主送机关、正文、落款日期等格式。
- 按 GB/T 15834-2011 做高置信标点和日期规范处理。
- 生成 Markdown 审校报告。
- 保留 Kagura 的 `dfskills` 后备工具，可用于通用中文文档预设排版。
- 提供本地 HTML 上传下载页面。

## 默认规则

默认使用 `body` 模式，只处理普通正式材料的正文格式：

- 不自动添加红头、发文字号、签发人、印章、版记、抄送、印发机关、页码。
- 若需要完整红头/版记格式，请使用 `full` 模式并提供必要元数据。
- 行距采用“标准目标接近每页 22 行；Word 实现默认固定 31pt”的方案。

规则冲突说明见：

```text
references/conflict-resolution.md
```

## 快速使用

```bash
PY=/Users/zhangjun/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3
$PY scripts/one_click_enhanced.py examples/测试文件.docx --mode body --json
```

输出文件默认生成在输入文件同目录：

```text
测试文件_国标公文校排.docx
测试文件_国标公文校排_报告.md
```

## 本地网页

```bash
PY=/Users/zhangjun/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3
$PY scripts/web_app.py --no-open
```

macOS 也可以双击：

```text
scripts/run_official_doc_mac.command
```

## 目录说明

```text
SKILL.md                         Skill 入口说明
scripts/one_click_enhanced.py     国标公文一键校排入口
scripts/dfskills/                 Kagura document-format-skills CLI 适配
references/                       国标规则、冲突裁决、写作模板、许可证
assets/presets/                   docformat-gui 兼容预设
assets/web/                       本地 HTML 页面
examples/                         测试输入、校排输出和报告
```

## 注意事项

- 自动化校排不能替代业务事实、政策依据、主体权限和印章真实性审核。
- 正式发文前仍建议在 Word 或 PDF 中人工复核红线、印章、页码、打印装订等视觉细节。
- 如需上传公开仓库，请确认示例文件中不包含敏感信息。
