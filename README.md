# 九域创意工作室（dsh-middle）

dsh（DeepSeek Harness）的项目侧栏 + 中窗插件：左侧项目抽屉、中间大窗渲染项目产出，1 个项目 = 1 个原生会话 + 1 块中窗。

> 兼容 **dsh Web / Windows 桌面 / macOS 桌面** 三端。自带 worktable 兼容层，存量 worktable 页面零改动可用。

## 功能

- **项目抽屉**：侧栏项目卡片（拖拽排序、右键菜单、重命名、删除记录）
- **状态灯（自动判断）**：🔴 待确认（产物有更新）· 🟢 已完成（呼吸灯，产物已确认）· 🔵 呼吸灯（会话输出中）· ⚪ 未绑定
- **图标自定义**：点卡片图标换图标/颜色（10 图标 × 8 色相）
- **中间大窗**：四档宽度（缩小/恢复/放大/全屏）+ 收起为悬浮圆钮（可拖动）
- **自动避让**：切到未绑定项目的会话时，中窗自动收起，不遮挡原生功能
- **浏览器打开**：经 Host 通道用系统浏览器打开产出（绕过 Electron 弹窗限制）
- **worktable 兼容层**：`/api/worktable/*` 全套（dshell.css 皮肤、site 静态资源、file/fs/write/mkdir/git），存量产物零改动
- **会话绑定**：新建专属会话 / 绑定当前会话 / 从最近会话挑选
- **从工作区导入**：一键把工作区根的 HTML 页面登记为项目
- **新建项目**：一句话需求 → 类型识别（资金/设计/开发/材料/终端/通用）→ 建会话 + 写入提示词草稿

## 安装（桌面版 macOS / Windows）

1. 下载 Release 里的 `dsh-middle-0.5.0.tgz`，放到 dsh 桌面 profile 的 `local-plugins/` 目录：

   | 平台 | profile 目录 |
   |---|---|
   | macOS | `~/.dsh/profiles/desktop/` |
   | Windows | `%USERPROFILE%\.dsh\profiles\desktop\` |

   （没有 `local-plugins` 目录就新建一个）

2. 编辑 profile 下的 `package.json`：

   ```json
   {
     "dependencies": {
       "dsh-middle": "file:./local-plugins/dsh-middle-0.5.0.tgz"
     },
     "dsh": {
       "profile": {
         "bundles": [
           "dsh-middle"
         ]
       }
     }
   }
   ```

   （`dependencies` 与 `dsh.profile.bundles` 都加一行 `dsh-middle`，其余已有内容不动）

3. 解包插件到 node_modules：

   ```bash
   # macOS / Linux
   cd ~/.dsh/profiles/desktop
   mkdir -p node_modules/dsh-middle
   tar -xzf local-plugins/dsh-middle-0.5.0.tgz -C node_modules/dsh-middle --strip-components=1
   ```

   ```powershell
   # Windows PowerShell
   cd $env:USERPROFILE\.dsh\profiles\desktop
   New-Item -ItemType Directory -Force node_modules\dsh-middle | Out-Null
   tar -xzf local-plugins\dsh-middle-0.5.0.tgz -C node_modules\dsh-middle --strip-components=1
   ```

4. 重启 dsh。左侧边栏底部出现「**九域创意工作室**」区块即安装成功。

## 安装（Web 版）

Web 部署的 dsh 同样按上述步骤配置 profile（`$DSH_HOME/profiles/<profile 名>/`），重启 dsh 服务后生效。

## 使用示例

Release 的 `examples/` 里有 4 个示例页面（**已去除个人路径**，工作区引用统一走 `WSROOT` 占位符，指向你自己的工作区根）：

- `原型工作台.html` — 原型登记/检索/放映工作台
- `应用工作台.html` — 从想法到应用的六步生成向导
- `数据工作台.html` — 数据源登记与看板工作台
- `功能工作台.html` — 应用工作台的跳板页

安装步骤：

1. 把 `examples/*.html` 拷到你的**工作区根目录**（dsh 中打开的工作区文件夹）；
2. dsh 左侧「九域创意工作室」→ 点「＋」→ 点「**从工作区导入…**」→ 选中示例页面；
3. 导入后中窗直接渲染。右键卡片可「绑定会话」让 Agent 继续开发它。

> 提示：数据工作台的可选数据文件（`数据源目录.json` 等）放在工作区根即可被识别；没有数据文件时空态运行。

## 常见问题

- **macOS 浏览「桌面/文稿/下载」提示 EPERM**：系统隐私限制，到「系统设置 → 隐私与安全性 → 文件与文件夹」给 DeepSeek Harness 勾选对应目录即可（文稿/下载通常已授权）。
- **在系统浏览器打开产出没反应**：本插件走 Host 通道（`/api/middle/open`），需确认插件版本 ≥ 0.4.1。
- **原 worktable 页面样式丢失/白板**：0.5.0 起自带兼容层，升级插件即可，页面无需改动。

## 许可

MIT
