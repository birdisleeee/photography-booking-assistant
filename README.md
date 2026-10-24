# Photography Booking Assistant

[简体中文](#简体中文) · [English](#english)

## 简体中文

**摄影排单助手：面向漫展摄影、Cosplay 约拍的开源自助预约与自动排单系统。** 客人打开链接，填写预约表格并选择空闲时段；后端自动登记订单、占用时段，摄影师在管理后台查看排期、登记收款，并按需开启值守通知。手机和电脑都能使用。

这是一个独立的摄影排单项目，包含自部署网页、公共托管服务和 Android 客户端。自动排单指预约客人选中的可用时段、阻止重复占用，不会自动挪动已有预约。

### 从预约到拍摄

1. **摄影师发布排期**：新开一届，设置年份、名称、拍摄日期、时段、价格、须知和联系方式，预览客人页面后确认发布。
2. **客人自助填写**：打开预约链接，选择时段，填写 CN、联系方式、角色和备注，可上传参考图；提交后生成订单号和预约记录。
3. **后台集中管理**：按日期查看拍摄时间表，核对付款、调整记录，活动结束后归档；原预约和收款记录继续保留。

自部署版的客人用 CN 注册并登录，在「我的预约」查看历届记录。CN 和电话按账号预填，支持点击订单查看详情、复制订单号、保存预约截图和确认后取消。旧预约可搜索后关联；仅凭 CN 关联时只展示场次、CN 和角色，核对原手机号或订单号后才获得正常预约权限。已取消订单不接受新增关联。

### 摄影师可以管理什么

| 功能 | 使用方式 |
| --- | --- |
| 多届排期与归档 | 按年份和届次整理，电脑显示多日时间表，手机使用抽屉导航。 |
| 统计主页 | 汇总有效订单、已收与待收金额、按 CN 去重的客人数；趋势节点可悬停或点按查看数值。 |
| 订单与收款 | 总览显示条数，已支付／未支付列表显示金额；支持定金、免单、优惠原因和凭证，以及确认后批量修改付款状态。 |
| 图片资料 | 上传参考图和优惠凭证；可选私有图片存储，通过预约或管理权限查看。 |
| 客人账号与旧单关联 | 客人查看自己的历届预约；摄影师可将旧记录关联到已注册账号，无需重建订单。 |

已收定金计入收款，尾款计入待收，免单为 0 元，取消订单不计入金额统计。没有记录金额的旧单会单独提示，不会按现价或备注中的数字猜测。

**这里只登记收款记录，不代收款、不扣款。** 客人点击「已确认」会通知摄影师核对，不会自动认定到账。详细操作见 [功能与使用流程](./paidan-booking/README.md)。

### 可选的脚本值守通知

不必一直开着后台等新订单。在「设置 → 脚本值守」选择渠道并配置后，服务端会为 **新预约、取消预约、客人付款通知**发送提醒。关闭网页或电脑后仍可继续处理，发送失败会有限重试。

| 通知渠道 | 需要准备 |
| --- | --- |
| 通用 Webhook | 接收通知的 HTTPS 网址，可选鉴权 Token。 |
| 企业微信 | 群机器人 Webhook 地址。 |
| 飞书 | 自定义机器人 Webhook 地址。 |
| Telegram | Bot Token 和 Chat ID。 |
| 邮件 | Resend API Key、已验证域名的发件地址、收件邮箱。 |

当前每个实例配置一个渠道，由使用者自行选择，不绑定作者的 QQ 或其他个人账号。邮件可发到 QQ、163、Gmail 等邮箱，发信使用 Resend，不使用邮箱密码或 SMTP 授权码。

值守默认关闭，凭据为空。自动化测试覆盖模拟发送和失败重试；**自己的接收端是否真正收到消息，需要配置后实测**。邮件接口接受请求不等于邮件已到收件箱。设置步骤见 [值守与邮件配置](./paidan-booking/WATCHER.md)。

有自己的 AI Agent，也可让它询问通知去向后协助配置内置渠道，或编写自己的 Webhook 接收服务和定时查询脚本，见 [Agent 接入说明](./paidan-booking/AGENT_SETUP.md)。**运行预约和内置通知不需要 AI Agent，也不需要 AI API Key。**

### 从自己的空白实例开始

代码不附带作者的漫展存档、客人订单、付款凭证、联系方式、管理密码或通知凭据。首次部署没有预设活动，使用者配置自己的密钥并发布第一届后，客人才可以预约。

### 使用与安装

| 适合谁 | 选择方式 | 下一步 |
| --- | --- | --- |
| 想直接使用，不自行部署 | 公共托管网页或 Android APK | [申请注册](https://public-paidan.nyachu.site/app)／[下载 APK](https://public-paidan.nyachu.site/download/app.apk)，等待运营者批准账号并分配配额。新增配额也需申请。 |
| 想自己掌握部署和数据，没有 AI Agent | 手动安装自部署版 | 按 [手动安装说明](./paidan-booking/DEPLOYMENT.md) 创建自己的存储、部署网页、配置密钥和首届活动。 |
| 有 AI Agent，希望它协助安装 | 同一套自部署版 | 参考 [AI Agent 操作说明](./paidan-booking/AGENT_SETUP.md)，确认部署账号和通知去向，由自己填写登录及秘密信息。 |

公共版使用独立后台，功能与额度规则见 [公共服务说明](./public-service/README.md)。**APK 需要联网，连接公共服务；不是离线排单应用，也不提供原生系统推送。** 客人使用摄影师分享的网页链接即可，无需安装 APK。自部署使用自己的资源，不需要向公共服务申请配额。

可以直接下载仓库 ZIP，无需 GitHub 账号；使用 Git 则执行：

```sh
git clone https://github.com/birdisleeee/photography-booking-assistant.git
cd photography-booking-assistant/paidan-booking
```

然后继续 [部署步骤](./paidan-booking/DEPLOYMENT.md)。当前提供可照做的 Cloudflare 部署示例；其他服务器和数据库需要适配后端。只上传 HTML 到静态托管不能运行预约、账号和通知功能。

### 文档

- [自部署版功能与使用流程](./paidan-booking/README.md)
- [手动安装与升级](./paidan-booking/DEPLOYMENT.md)
- [AI Agent 安装与接入](./paidan-booking/AGENT_SETUP.md)
- [值守与邮件配置](./paidan-booking/WATCHER.md)
- [私有图片存储](./IMAGE_STORAGE.md)
- [公共版规则](./public-service/README.md) · [Android 客户端与构建](./android-app/README.md)

---

## English

**Photography Booking Assistant is an open-source, self-hosted photography booking and scheduling system for cosplay shoots and conventions.** Clients complete a booking form and choose an available time slot. The backend records the appointment and reserves the slot, while the photographer manages schedules, payment records, and optional background notifications.

This repository contains one photography booking product: a self-hosted web application, a hosted service edition, and an Android client. Automatic scheduling reserves the client's selected available slot and prevents double booking; it does not rearrange existing appointments. The web interface works on desktop and mobile.

### From booking to shoot

1. **Publish an event.** Set its year, name, shoot dates, time slots, prices, instructions, and contact details. Preview the client page before publishing.
2. **Let clients book.** Share the link. Clients choose a slot and enter their cosplay name or nickname (CN), contact details, character, and notes, with an optional reference image. Submission creates a booking ID and reserves the slot.
3. **Manage the schedule.** Review daily appointments, record payments, update records, and archive finished events without losing their history.

In the self-hosted edition, clients register with their CN and sign in to see bookings across events. Saved CN and phone details prefill later forms. Clients can open booking details, copy their booking ID, download a receipt image, and confirm cancellation. Older bookings can be linked to an account: CN-only links expose just the event, CN, and character; matching the original phone number or booking ID grants normal booking access. Cancelled records cannot be newly linked.

### Photographer dashboard

| Feature | What you can do |
| --- | --- |
| Event schedules and archives | Organize events by year, view a multi-day desktop schedule, and use drawer navigation on mobile. |
| Overview and trends | See valid bookings, recorded receipts, outstanding amounts, and distinct client CNs. Hover or tap chart points for values. |
| Orders and payment records | View counts in the overview and amounts inside paid/unpaid lists; record deposits, complimentary bookings, discounts and evidence, with confirmed bulk status updates. |
| Reference images and evidence | Upload images and optionally use private image storage with booking or admin access checks. |
| Client accounts and older bookings | Let clients view their booking history and let photographers assign older records to registered accounts. |

Deposits count toward receipts, remaining balances toward outstanding amounts, and free bookings contribute zero. Cancelled orders are excluded from monetary summaries. Missing historical amounts are flagged rather than inferred from current prices or numbers in notes.

**Payment tracking records your own checks; it does not collect or charge money.** A client's “Confirmed” action notifies the photographer to verify payment. See the [full feature guide](./paidan-booking/README.md#english).

### Optional background notifications

Enable notifications in the admin settings to receive alerts for **new bookings, cancellations, and client-reported payments**. Delivery runs on the server, continues after the browser or your computer is closed, and uses bounded retries on failure.

| Channel | Your configuration |
| --- | --- |
| Generic Webhook | An HTTPS receiver URL and an optional authentication token. |
| WeCom | A group bot Webhook URL. |
| Feishu | A custom bot Webhook URL. |
| Telegram | A bot token and chat ID. |
| Email | A Resend API key, a sender address on a verified domain, and a recipient address. |

Each instance currently uses one channel chosen by its owner. No author's QQ or other personal account is required. Email can go to mailboxes such as QQ Mail, 163, or Gmail; sending uses Resend rather than a mailbox password or SMTP authorization code.

Notifications are off by default and credentials start blank. Automated tests cover simulated delivery and retries. **Verify actual receipt using your own destination.** An accepted email API request is not proof of inbox delivery. See [notification and email setup](./paidan-booking/WATCHER.md) (Chinese).

An optional AI agent can help configure a built-in channel or write your own Webhook receiver or polling script after asking where alerts should go. See the [agent integration guide](./paidan-booking/AGENT_SETUP.md) (Chinese). **Bookings and built-in notifications require no AI agent or AI API key.**

### Start with your own data

A fresh deployment includes no author's events, archives, client orders, payment evidence, contact details, admin passwords, or notification credentials. Configure your own secrets and publish your first event to open bookings.

### Use or install

| Your situation | Edition | Next step |
| --- | --- | --- |
| Use a service without deploying | Hosted website or Android client | [Apply for an account](https://public-paidan.nyachu.site/app) or [download the APK](https://public-paidan.nyachu.site/download/app.apk). Wait for operator approval and quota allocation; additional quota also requires approval. |
| Control your own deployment and data | Self-hosted web application | Follow the [manual installation guide](./paidan-booking/DEPLOYMENT.md) (Chinese) to create storage, deploy, configure secrets, and publish your first event. |
| Have an AI agent assist with installation | The same self-hosted application | Follow the [agent setup guide](./paidan-booking/AGENT_SETUP.md) (Chinese), confirm your account and notification destination, and enter credentials yourself. |

The hosted edition has its own backend and release scope; see [hosted service rules](./public-service/README.md#english). **The APK requires internet access and connects to the hosted service. It has no offline booking mode or native push notifications.** Clients can use a photographer's booking link without installing the app. Self-hosting uses your own resources and does not require hosted-service quotas.

Download the repository ZIP without a GitHub account, or clone it:

```sh
git clone https://github.com/birdisleeee/photography-booking-assistant.git
cd photography-booking-assistant/paidan-booking
```

Continue with the [deployment guide](./paidan-booking/DEPLOYMENT.md) (Chinese). A Cloudflare deployment example is included. Other servers and databases require backend adaptation; static HTML hosting alone cannot run bookings, accounts, or notifications. The application interface and detailed setup guides are currently in Chinese.

### Documentation

- [Self-hosted features and workflow](./paidan-booking/README.md#english)
- [Manual installation and upgrades](./paidan-booking/DEPLOYMENT.md) (Chinese)
- [AI agent setup and integration](./paidan-booking/AGENT_SETUP.md) (Chinese)
- [Notifications and email](./paidan-booking/WATCHER.md) (Chinese)
- [Private image storage](./IMAGE_STORAGE.md)
- [Hosted service](./public-service/README.md#english) · [Android client and build instructions](./android-app/README.md#english)

## License

[MIT](./LICENSE)
