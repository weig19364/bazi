# 天枢·禅 (Tianshu Zen) 🌌

> 融合传统命理与现代天文计算的高精度排盘引擎

[![Version](https://img.shields.io/badge/version-V15.2-gold)]()
[![License](https://img.shields.io/badge/license-MIT-blue)]()
[![Build](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Size](https://img.shields.io/badge/size-2.36MB-lightgrey)]()
[![GitHub](https://img.shields.io/badge/GitHub-weig19364%2Fbazi-black?logo=github)](https://github.com/weig19364/bazi)

---

## ✨ 功能特性

- 🌞 **高精度天文内核** — WebAssembly 驱动，Swiss Ephemeris 级精度
- 🀄 **八字四柱排盘** — 真太阳时修正，节气精确到分钟
- ⚡ **大运起运精化** — 二分法精确到分钟级，误差 < 数小时
- 🌍 **全球时区支持** — UTC-12 ~ UTC+12，完全绕开浏览器时区干扰
- ☯️ **梅花易数** — 先天八卦序，64卦自动推演本卦/变卦/动爻
- 📖 **焦氏易林** — 内置 4096 条完整卦辞数据库
- 🗺️ **黄道天象图** — Canvas 实时渲染行星黄道坐标
- 🔢 **洛书九宫** — 数理可视化
- 📦 **零依赖单文件** — 无需服务器，本地直接打开

---

## 🚀 快速开始

### 直接使用

下载 [`tianshu_zen54.html`](https://github.com/weig19364/bazi/blob/main/tianshu_zen54.html)，用浏览器打开即可，无需安装任何依赖。

```bash
# 克隆仓库
git clone https://github.com/weig19364/bazi.git
cd bazi

# 直接用浏览器打开
start tianshu_zen54.html       # Windows
open tianshu_zen54.html        # macOS
xdg-open tianshu_zen54.html    # Linux
从源码打包
bash
# 唯一要求：Python 3.8+
python build_v54.py
打包完成后生成 tianshu_zen54.html 单文件，无任何外部依赖。

🖥 界面预览
排盘界面 · 天象黄道图 · 洛书九宫 · 易林卦辞

预览图

🔧 使用说明
输入项	说明	示例
日期时间	出生地的当地时间	2014-12-22 11:05:50
时区	出生地对应时区	UTC-8（洛杉矶）/ UTC+8（北京）
经度	东经为正，西经为负	-118.40（洛杉矶）/ 116.40（北京）
纬度	北纬为正，南纬为负	34.05（洛杉矶）/ 39.90（北京）
性别	影响大运顺逆方向	男 / 女
排盘方式	选择算法流派	天象（天地合参）/ 梅花 / 时间起卦
✅ 验证用例
text
时间：2014-12-22 11:05:50
经度：-118.40°（洛杉矶）
时区：UTC-8（太平洋标准时间）
─────────────────────────────
预期：黄经 ~270°，节气冬至
实测：黄经 270.85° ✅  节气冬至 ✅
🗂 项目结构
text
bazi/
├── 📄 tianshu_zen54.html          # ✅ 最新单文件产物 (2.36MB)
├── 📄 tianshu_zen50_archived.html # 历史归档版
│
├── 📄 index.html                  # 主界面（含天文内核WASM）
├── 📄 main_bazi.js                # 核心算法：八字/大运/易林/梅花
├── 📄 yilin_final.json            # 焦氏易林 4096 条卦辞数据库
├── 📄 dunjia_core.js              # WebAssembly 胶水层
├── 📄 dunjia_core.wasm            # 天文计算内核（Swiss Ephemeris）
│
├── 🔧 build_v54.py                # 打包脚本（生成单文件HTML）
├── 🔧 build_v15.py                # archived版打包脚本
│
├── 🐍 bazi_engine.py              # Python版八字引擎（参考）
├── 🐍 dayun_engine.py             # Python版大运引擎（参考）
├── 🐍 guaqi_engine.py             # Python版卦气引擎（参考）
├── 🐍 solar_terms.py              # 节气计算
├── 🐍 yilin_engine.py             # 易林检索引擎
│
└── 📝 天枢·禅 V15.2 升级说明文档.md
🏗 技术架构
text
┌──────────────────────────────────────────────┐
│              浏览器 (单文件HTML)               │
├───────────────────┬──────────────────────────┤
│      界面层        │         算法层            │
│  Canvas 天象图     │  main_bazi.js            │
│  洛书九宫          │  ├── 时区解析 (Date.UTC)  │
│  八字四柱展示       │  ├── 八字四柱             │
│  大运列表          │  ├── 起运二分精化          │
│  易林卦辞          │  ├── 梅花易数 64卦        │
│                   │  └── 易林检索             │
├───────────────────┴──────────────────────────┤
│              天文内核层 (WASM)                 │
│   dunjia_core.wasm  ←  Swiss Ephemeris       │
│   ├── 儒略日计算                              │
│   ├── 真太阳时修正 (ΔT)                       │
│   ├── 节气精确时刻                            │
│   └── 行星黄道坐标                            │
└──────────────────────────────────────────────┘
📦 打包原理
text
index.html      ──┐
main_bazi.js    ──┼──► build_v54.py ──► tianshu_zen54.html
yilin_final.json──┘    ├── JSON压缩(-28%)   (2.36MB 零依赖)
                       ├── WASM Base64内联
                       └── 防重复注入检测
📋 版本历史
版本	日期	主要改动
V15.2	2026-03-15	🔴 时区Bug修复 · ⚡ 起运分钟精化 · 📦 打包去重优化
V15.1	2026-03-03	易林4096条卦辞集成 · 天象Canvas渲染
V15.0	2025-11-22	WebAssembly天文内核接入
V14.9	2025-11-19	梅花易数算法 · 先天八卦64卦映射
🐛 已知修复
V15.2 时区Bug（重要）
旧版中时区下拉框选择完全无效，所有计算均按浏览器本地时区进行。
新版使用 Date.UTC() + 手动偏移，完全绕开浏览器时区差异，
确保在全球任何设备上计算结果一致。

🤝 贡献
欢迎 Issue 和 PR！

特别欢迎：

📅 历法边界 case 验证（如换年节气临界点）

🌍 不同时区排盘结果验证

💡 界面体验改进建议

📄 许可证
MIT License © 2026 weig19364

数据来源：《焦氏易林注》· Swiss Ephemeris

<div align="center"> <b>天枢·禅</b> — 让古老智慧与现代计算精确对话 🌌 <br><br> <a href="https://github.com/weig19364/bazi">GitHub</a> · <a href="https://github.com/weig19364/bazi/issues">反馈问题</a> · <a href="https://github.com/weig19364/bazi/releases">下载发布版</a> </div> ```
