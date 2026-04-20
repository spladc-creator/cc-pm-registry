# cc-pm-registry

Claude Code Agent & Skill 包仓库。由 [cc-pm](https://github.com/spladc-creator/cc-pm) 管理。

## 使用

```bash
# 安装 cc-pm 工具
curl -fsSL https://raw.githubusercontent.com/spladc-creator/cc-pm/main/bin/install.sh | bash

# 搜索远程包
cc-pm.sh remote-search <keyword>

# 拉取并安装
cc-pm.sh pull <name>
cc-pm.sh install <name> <project>
```

## 可用包

| 包名 | 类型 | 版本 | 说明 |
|------|------|------|------|
| deep-organizer | agent | 1.0 | 深度信息整理与结构化分析 |
| report-generator | agent | 1.0 | 多格式结构化报告生成 |
| literary-works-collector | agent | 1.0 | 文学作品信息收集 |
