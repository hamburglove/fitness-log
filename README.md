# 健身打卡

简洁美观的健身打卡网页应用（PWA），可在手机浏览器中直接使用，也可安装到主屏幕像 App 一样离线使用。

## 功能

- **今日打卡**：点选部位（带人体部位示意图）→ 点选动作（自制图标）→ 记录组数 / 每组次数 / 重量（可选）或有氧时长
- **卡路里估算**：基于 MET × 体重 × 时长（明确标注为估算）
- **历史日历**：按月查看打卡日期，颜色对应训练部位；连续打卡天数与本周统计
- **统计**：连续 / 最长连续、近 30 天部位分布、近 7 天消耗
- **我的**：个人资料（体重默认 79 kg）、JSON 备份 / 恢复
- **PWA**：可添加到 iPhone / 安卓主屏幕，Service Worker 离线可用

## 直接使用

用浏览器打开 `index.html` 即可（`file://` 也能用；如需完整 PWA 安装与离线，请用本地静态服务器：`npx serve .`）。

## 用 Capacitor 打包成 iOS App（上架 App Store）

> 需要：**Mac**、**Xcode**（最新版）、**Apple Developer 账号**（年费）。本仓库不会代你上架。

### 1. 初始化 Capacitor 项目

```bash
# 建议新建一个目录，把本文件夹内容作为 web 资源
npm init -y
npm install @capacitor/core @capacitor/cli @capacitor/ios
npx cap init "健身打卡" "com.yourname.fitnesslog" --web-dir .
```

把本目录下的 `index.html`、`manifest.json`、`sw.js`、`icons/` 放在 `--web-dir` 指向的目录中（或把 `--web-dir` 设成当前目录）。

### 2. 添加 iOS 平台并同步

```bash
npx cap add ios
npx cap sync ios
npx cap open ios
```

### 3. 在 Xcode 中配置

1. 用 Xcode 打开生成的 `ios/App/App.xcworkspace`
2. **Signing & Capabilities**：选择你的 Team，填写 Bundle Identifier
3. **General → Display Name**：健身打卡
4. **App Icons**：用 `icons/icon-512.png` 生成各尺寸（可用 Xcode 的 App Icon 资源，或 [appicon.co](https://www.appicon.co) 之类工具）
5. 真机或模拟器运行验证

### 4. 上架 App Store（概要）

1. 在 [App Store Connect](https://appstoreconnect.apple.com) 新建 App，填写名称、隐私政策、截图等
2. Xcode 菜单 **Product → Archive**，然后 **Distribute App** → App Store Connect
3. 提交审核。注意：若仅为本地数据、无账号，需在隐私问卷中如实填写；卡路里仅为估算，建议在 App 描述中说明

### 安卓（可选）

```bash
npm install @capacitor/android
npx cap add android
npx cap sync android
npx cap open android
```

## 技术说明

- 纯前端，无后端、无外部 CDN 依赖
- 数据存于浏览器 `localStorage`（键名 `fitlog.v1`），请定期在「我的」中备份
- 卡路里公式：`kcal ≈ MET × 体重(kg) × 小时`；力量时长按「次数 × 3 秒 + 休息 60 秒」估算

## 许可

自用 / 学习随意。图标与插画均为自制 SVG，非商业素材库版权内容。
