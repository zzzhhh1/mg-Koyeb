# 🚀 my-Koyeb: 2026 基于 Koyeb + Cloudflare Argo 隧道的 VLESS 安全单兵节点

# [纯小白详细视频教程](https://youtu.be/q-PvkWsU7AA/)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Platform: Koyeb](https://img.shields.io/badge/Platform-Koyeb-blue)
![Network: Cloudflare](https://img.shields.io/badge/Network-Cloudflare-orange)

## ⚠️ 重要声明 (Important Notice)

> 🛑 **本项目仅供网络技术交流、分布式容器架构学习研究之用。**
> 
> * **【免费分享】** 本项目完全开源，任何人均可免费获取、学习和公开分享。
> * **【禁止商用】** **严禁任何个人或组织将本项目及相关衍生代码用于任何商业谋利、打包倒卖、或者搭建收费机场等商业用途！** > * 因违反本声明导致的任何法律纠纷，均由违规使用者自行承担，原作者概不负责。

---

## ✨ 项目独家优势与特性

在 2026 年当前的容器白嫖网络生态下，本项目完美打通并优化了以下三大硬核技术痛点：

* 🔒 **100% 隐私防泄露**：淘汰了在 GitHub 仓库明文暴露 `UUID` 的老旧做法。本项目采用**空壳开源**方案，真正的钥匙全部转为 Koyeb 后台环境变量动态注入，即使仓库完全公开，别人也绝对无法蹭你的流量。
* ⚡ **双进程智能保活，永不休眠**：通过 `supervisord` 在 Alpine 容器内同时拉起 `sing-box` 与 `cloudflared` 隧道。因为隧道会源源不断向 Cloudflare 发送心跳包，容器时刻保持活跃，**彻底打破免费容器不间断自动休眠的魔咒**！
* 🍏 **日志全自动拼接输出**：容器启动成功的瞬间，会自动读取你的自定义参数，在控制台日志（Runtime Logs）中**高亮打印出拼接好的 VLESS 万能快捷导入链接**，真正做到一键复制，小白无缝上手。
* 💳 **零金融门槛**：注册全流程**不需要绑定任何信用卡**，纯邮箱一键直达，右侧账单永久锁定 `$0.00/月`。（很多老教程都过时了！最近 Koyeb 官方为了打击薅羊毛，上线了流氓风控墙，误点 Continue 会直接扣你 10.14 刀！今天科技共享不仅带大家手把手绕过风控，更带来 2026 全新零代码搭建自建 VLESS 节点的终极方案！）

---

## 🛠️ 第一部分：GitHub 快速克隆建站

1. 在 GitHub 创建一个全新的 **Public（公开）** 仓库。
2. 将本仓库中的以下 3 个核心配置文件直接放入你的仓库根目录下：
   * `config.json`（已开启路径对齐与空白占位）
   * `entrypoint.sh`（核心变量注入与节点日志产出脚本）
   * `Dockerfile`（容器自动化编译蓝图）
   * `supervisord.conf`（双进程守卫保活配置）
3. 提交代码后，GitHub Actions 会在后台全自动帮你编译并发布专属的 Docker 镜像。

---

## 🌐 第二部分：Cloudflare Argo 隧道口对齐

1. 登录你的 [Cloudflare Zero Trust 面板](https://one.dash.cloudflare.com/)，依次进入 **Networks -> Tunnels**，创建一个新的隧道（例如命名为 `0521`）。
2. 在 **Public Hostname** 配置页中，严格按下表执行路径和服务的对齐：
   * **子域名 (Subdomain)**: 填写你的二级前缀（如 `0521`）
   * **域 (Domain)**: 选择你托管在 CF 的自定义域名（如 `yourdomain.com`）
   * **路径 (Path)**: **必须填写 `^/blog`** *(由于部分主站业务限制，此处必须保持路径一致以绕过同源审查)*
   * **服务类型 (Service)**: 必须选择 **`HTTP`**
   * **URL**: 严格填写 **`127.0.0.1:8080`**
3. 保存配置，并保存 Cloudflare 随机生成的那串长 **`Tunnel Token`**。

---

## 🚀 第三部分：Koyeb 一键后台注入部署

1. 登录 Koyeb 平台，点击 **`Create Service`**，选择 **`Docker image`** 模式。
2. **镜像地址**填写你通过 GitHub Actions 公开到 GHCR 的专属地址：
   `ghcr.io/【你的GitHub用户名】/【仓库名】:latest`
3. **Ports 端口配置**：点开 Ports 折叠栏，将默认端口修改为 **`8080`**，协议保持 **`HTTP`**（Health checks 探测会同步对齐）。
4. **环境变量注入**：展开 **`Environment variables`** 栏，点击 **Add Variable** 补全以下 4 个核心黄金参数：

| Key (键) | Value (值示例) | 说明 |
| :--- | :--- | :--- |
| **`UUID`** | `f6a3bxxx-2992-490c-8289-40bf3ccb1xxx` | 🔑 你专属的防泄露节点钥匙（后台动态注入） |
| **`SUBDOMAIN`** | `0521.yourdomain.com` | 🌐 你的完整隧道自定义域名（用于日志自动拼接） |
| **`TUNNEL_TOKEN`**| *从 Cloudflare 复制出来的长 Token* | 🔐 连接 Cloudflare 隧道的唯一通行凭证 |
| **`PORT`** | `8080` | 🔌 容器内部通信与平台健康检查对齐端口 |

# [uuid生成器：点击生成](https://99688988.xyz/uuid-generator)

5. 机型规格选择 **`Nano (免费型：0.1 vCPU, 512MB RAM)`**，点击底部的 **`Deploy`**。

---

## 🏆 第四部分：获取节点与一键体验

1. 部署成功后，顶部的红色警报会瞬间化为绿色的 **`Healthy`**。
2. 点击进入 Koyeb 后台的 **`Runtime Logs`（运行日志）**，在控制台正中央，你会清晰地看到一长串全自动拼接好的：`vless://...` 完整链接。
3. 完整复制该链接，在 **v2rayN** 或者是 **Shadowrocket（小火箭）** 客户端中直接一键通过剪贴板导入，右键执行 `Ctrl + T` 真连接延迟测试。
4. 看到亮起的绿色延迟，即可畅快开启你的永久免费高质量分布式原生节点体验！

---

## 📄 版权与开源协议 (License)

本项目基于 **MIT** 开源协议分享。再次重申：**任何个人与组织不得将本项目代码、配置或教程用于任何形式的商业行为。技术无界，合理交流，感谢大家的理解与配合。**

---
💡 *如果你喜欢这个项目，欢迎去 YouTube 搜索关注 **“科技共享”** 频道，获取更多高能硬核的前沿技术干货与不限速白嫖保姆级教程！*
