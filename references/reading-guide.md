# 按需读取指南

## Windows / PowerShell

以下路径是示例，执行前换成用户明确指定或已确认的参考根目录。使用 `-LiteralPath` 处理空格、中文和方括号。`rg` 默认尊重忽略规则；已知需要的文件被忽略时，直接读取该文件，不进行无边界的隐藏文件扫描。

```powershell
Get-Item -LiteralPath 'E:\项目乙'
rg --files 'E:\项目乙' -g '!node_modules' -g '!dist' -g '!build' -g '!vendor' -g '!.git' -g '!.venv' -g '!venv' -g '!coverage' -g '!.env*' -g '!*.pem' -g '!*.key' -g '!credentials*' -g '!secrets*'
rg -n -e 'export' -e 'download' 'E:\项目乙\src' -g '*.ts' -g '*.tsx' -g '!*.min.*'
Get-Content -LiteralPath 'E:\项目乙\src\export.ts' -TotalCount 180
```

如需实际行号，对已筛选的单个文本文件编号，不对整个目录输出全文：

```powershell
$referenceLine = 0
Get-Content -LiteralPath 'E:\项目乙\src\export.ts' | ForEach-Object {
    $referenceLine++
    if ($referenceLine -ge 20 -and $referenceLine -le 90) {
        '{0}: {1}' -f $referenceLine, $_
    }
}
```

扩展名过滤只是减少无关文件，不能证明文件不含秘密。根据文件用途先筛选，避免读取认证存储及真实部署配置。超大文件先看搜索命中的局部；二进制文档使用相应读取工具。

## 来源与适配

可根据需求使用下面的列，不必每次填完整表格：

| 来源 | 已读证据 | 可借鉴内容 | 当前项目需调整 | 验证情况 |
| --- | --- | --- | --- | --- |
| 实际路径或 URL | 实际文件和行号 | 与需求相关的机制 | 接口、依赖、数据或运行方式差异 | 已执行的检查，或仅静态阅读 |

当用户要求实际复用时，先查清关联调用和依赖再移植。验证聚焦目标项目的行为；源项目是参考资料，不必为了读取它启动服务或执行安装流程。
