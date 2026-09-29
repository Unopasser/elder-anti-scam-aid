# Elder Anti-Scam Aid · 助老反诈教具

> 本仓库首个应用：**非礼勿视α v1.0.0** —— 沉浸式反诈扫码演示
> An immersive anti-fraud QR-code lesson for elders

## 版本与系列规划

本仓库是**系列助老反诈教具**的集合，「非礼勿视α」只是第一个应用，并非唯一应用：

- 当前版本：非礼勿视α **v1.0.0**（仓库根目录 `index.html`）；
- 后续新应用将以**独立子目录**共存于本仓库（如 `/app-b/`），各自带独立页面与文案配置，互不影响；
- 每个应用在揭晓页落款处标注自身名称与版本号，便于授课时核对版本。

## 项目介绍（中文）

这是一套放在家庭里的**助老反诈教具**：让长辈在 100% 安全的环境里，亲身经历一次"扫码后被假验证页套住 30 秒"的过程，再由页面揭晓真相、讲清道理。

很多老人不是不懂"不要扫陌生二维码"，而是从没体验过"扫了之后会发生什么"。本教具采用"接种式"教学——提前体验一次弱化版的攻击，获得真实的免疫力：

1. **体验**：长辈扫一个二维码，手机弹出高度仿真的「SIM 卡 PIN 码验证」页面，全屏、退不出去、还要输两次；
2. **揭晓**：约 30 秒后页面主动揭示真相——"你以为只是被整蛊了？实际上你被『硬控』了 30 秒，期间你的设备正面临风险——请勿随意扫码"；
3. **讲道理**：家人（推荐配合 `SECURITY-GUIDE.md`）顺势讲解：真正的恶意页面不会告诉你真相，它们要的可能是你的验证码、你的钱，或你手机里的隐私。

**为什么它是安全的（也是它与诈骗的本质区别）**：

- 任意 6 位数字都能通过——页面在结构上无法采集"真实的 PIN 码"；
- 全程零网络请求——即使输入了什么，也送不出去；
- 不记录、不存储——输入内容只在浏览器里存活数秒后丢弃；
- 必然揭晓——流程结束强制说明这是教学演示；
- 用户随时可以按 Home 键离开——体验可以被"硬控"，设备永远不被控制。

## Introduction (English)

This is a **home-use anti-fraud teaching aid for elders**: it lets an elderly family member safely *experience* a "scan-the-code, get-trapped-for-30-seconds" scare page, then reveals the truth on screen and turns the scare into a lesson.

Most seniors don't lack the knowledge "don't scan random QR codes" — they lack the *experience* of what actually happens when you do. This aid follows the inoculation approach: a weakened, harmless exposure that builds real resistance.

1. **Experience** — the elder scans a QR code; a realistic "SIM card PIN verification" page takes over the phone screen and asks for input twice;
2. **Reveal** — after about 30 seconds, the page discloses itself: *"You thought it was just a prank? You were actually locked in for about 30 seconds. During that time, your device could have been at real risk — so never scan codes carelessly."*;
3. **Debrief** — family members walk through what a *real* malicious page would do differently: it would never tell you the truth, and it would be after your codes, your money, or your privacy.

**Why it is safe — and how it differs from an actual scam:**

- Any 6 digits are accepted — the page structurally *cannot* collect a real PIN;
- Zero network requests — nothing entered can ever leave the browser;
- Nothing is recorded or stored — input lives for seconds, then is discarded;
- Mandatory reveal — the flow always ends by announcing itself as a teaching demo;
- The Home key always works — the *experience* may lock you in; the *device* never is.

## 文件清单

| 文件 | 用途 |
|---|---|
| `index.html` | 教具主页面（应用「非礼勿视α」），自包含，无外部依赖 |
| `config.js` | 默认文案配置（可选，删除后页面仍用内置默认值） |
| `qrcode.png` | 二维码（当前为占位地址，**部署后需重新生成**） |
| `SECURITY-GUIDE.md` | 恶意软件防范说明书：本页每个技术点的武器化风险与防范（授课参考材料） |

## 部署

任意静态托管均可（GitHub Pages、宝塔静态站点、对象存储等）：

1. 上传 `index.html` 与 `config.js` 到同一目录；
2. 用部署后的真实地址重新生成二维码：
   ```bash
   pip install qrcode
   python -c "import qrcode; qrcode.make('https://你的域名/index.html', box_size=12, border=2).save('qrcode.png')"
   ```
   带自定义文案时，把 URL 参数直接拼进二维码地址（见下表）。

## 文案配置

优先级：**URL 参数 > config.js > 内置默认值**。

| URL 参数 | config.js 键 | 说明 | 默认值 |
|---|---|---|---|
| `?t=` | `t` | 第一次弹窗标题 | SIM 卡 PIN 码 |
| `?s=` | `s` | 第一次弹窗副标题 | 请输入 SIM 卡 PIN 码以解锁 |
| `?t2=` | `t2` | 第二次弹窗标题 | 验证超时，请再次输入 |
| `?lt=` | `lt` | 加载页文案 | 正在验证，请稍候… |
| `?sec=` | `sec` | 加载秒数（URL 参数限 3–60） | 10 |
| `?ot=` | `ot` | 揭晓页标题 | 你以为只是被整蛊了 |
| `?os=` | `os` | 揭晓页说明 | 见 config.js |

示例：`https://你的域名/index.html?sec=15`

## 交互流程

1. 打开页面显示「正在检查 SIM 卡状态…」，轻触屏幕后进入全屏并弹出 PIN 弹窗；
2. 输入任意 6 位数字（不足 6 位提示「PIN 码不正确，请重试」），点「解锁」；
3. 全屏加载 10 秒（`sec` 可调）；
4. 再次要求输入 PIN 码，任意 6 位通过；
5. 「验证成功」闪现后揭晓：「你以为只是被整蛊了」→「实际上你被『硬控』了约 30 秒……请勿随意扫码」+ 安全声明；
6. 「取消」按钮不会关闭弹窗；按 Home 键/上滑可随时离开——这是刻意的合规底线。

## 给授课家人的建议

- **先讲后测**：先看一遍 `SECURITY-GUIDE.md` 第④节，理解本页为什么不是钓鱼，再给长辈演示；
- **陪同体验**：长辈体验时你在旁边，揭晓页出来后立即展开讨论；
- **只对熟人使用**：对象限于你的家人，且确保他们看到了揭晓页；
- **不要伪装官方**：不要把二维码包装成银行、运营商、公安机关的渠道——既违法，也违背本教具"必揭晓"的设计初衷。

## 合规要点（硬性约束）

- **零数据记录**：无 localStorage/cookie/任何存储，无 fetch/XHR/统计/埋点，断网可完整运行；
- **无真实凭据风险**：任意 6 位均通过；
- **必揭晓**：流程结束强制显示教学说明与安全声明；
- **保留强度为浏览器级上限**：全屏 + 返回键留在页内 + 弹窗不可点外部关闭；不做也无法做系统级硬控（无障碍服务/设备管理器滥用属恶意软件行为，本项目明确不采用）。
