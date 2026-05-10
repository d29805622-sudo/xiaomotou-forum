# Firebase + GitHub 部署指南

## 📋 概述

本论坛系统使用 Firebase 作为后端，可以部署到 GitHub Pages！

### 支持的功能
- ✅ 用户注册/登录（邮箱 + GitHub OAuth）
- ✅ GitHub 一键登录
- ✅ 帖子发布/浏览
- ✅ 评论互动
- ✅ 管理后台
- ✅ 实时数据同步

---

## 第一步：创建 Firebase 项目

### 1. 注册 Firebase 账号
访问：https://console.firebase.google.com/
使用 Google 账号登录

### 2. 创建新项目
- 点击 "添加项目"
- 输入项目名称：`xiaomotou-forum`
- 关闭 Google Analytics（可选）
- 点击 "创建项目"

### 3. 获取项目配置
1. 进入项目后，点击 **⚙️ 设置**（齿轮图标）
2. 滚动到 **"您的应用"** 部分
3. 点击 **</> (Web)** 图标
4. 输入应用名称，点击 "注册应用"
5. 复制 Firebase 配置信息：
```javascript
const firebaseConfig = {
    apiKey: "AIzaSy...",
    authDomain: "xiaomotou-forum.firebaseapp.com",
    databaseURL: "https://xiaomotou-forum-default-rtdb.firebaseio.com",
    projectId: "xiaomotou-forum",
    storageBucket: "xiaomotou-forum.appspot.com",
    messagingSenderId: "123456789",
    appId: "1:123456789:web:abc123"
};
```

---

## 第二步：启用 Firebase 服务

### 1. 启用 Authentication（用户认证）
1. 左侧菜单点击 **"构建"** → **"Authentication"**
2. 点击 "开始使用"
3. 在 "Sign-in method" 标签页：
   - 启用 **电子邮件/密码**
   - 启用 **GitHub**（可选）

### 2. 配置 GitHub OAuth（可选）
如果您想启用 GitHub 登录：
1. 在 Firebase Console 中：
   - 启用 GitHub 提供商
   - 记下 Client ID 和 Client Secret

2. 在 GitHub 设置中：
   - 访问 https://github.com/settings/developers
   - 点击 "New OAuth App"
   - 填写：
     - Application name: `xiaomotou-forum`
     - Homepage URL: `https://yourusername.github.io`
     - Authorization callback URL: Firebase 提供的回调 URL
   - 点击 "Register application"
   - 复制 Client ID 和 Client Secret

3. 回到 Firebase：
   - 粘贴 Client ID 和 Client Secret
   - 点击 "保存"

### 3. 启用 Realtime Database（实时数据库）
1. 左侧菜单点击 **"构建"** → **"Realtime Database"**
2. 点击 "创建数据库"
3. 选择区域（建议选择离您近的）
4. 选择 "以测试模式启动"（开发阶段）
5. 点击 "启用"

---

## 第三步：更新代码配置

将所有 HTML 文件中的 Firebase 配置替换为您的配置：

```javascript
const firebaseConfig = {
    apiKey: "您的API_KEY",
    authDomain: "您的项目.firebaseapp.com",
    databaseURL: "https://您的项目-default-rtdb.firebaseio.com",
    projectId: "您的项目ID",
    storageBucket: "您的项目.appspot.com",
    messagingSenderId: "您的发送ID",
    appId: "您的APP_ID"
};
```

需要修改的文件：
- `register.html`
- `login.html`
- `index.html`
- `category.html`
- `post.html`
- `new-post.html`
- `admin.html`

---

## 第四步：上传到 GitHub

### 方法 A：使用 GitHub 网页界面

1. **创建新仓库**
   - 访问 https://github.com/new
   - Repository name: `xiaomotou-forum`
   - 选择 Public
   - 点击 "Create repository"

2. **上传文件**
   - 点击 "uploading an existing file"
   - 将所有 HTML 文件拖拽到上传区域
   - 点击 "Commit changes"

### 方法 B：使用 Git 命令

```bash
# 在项目文件夹中打开终端

# 初始化 Git
git init

# 添加所有文件
git add .

# 提交
git commit -m "小魔头Galgame论坛 v1.0"

# 添加远程仓库（替换为您的仓库地址）
git remote add origin https://github.com/您的用户名/xiaomotou-forum.git

# 推送
git push -u origin main
```

---

## 第五步：启用 GitHub Pages

1. 进入您的 GitHub 仓库
2. 点击 **Settings（设置）**
3. 滚动到 **"Pages"** 部分
4. 在 "Source" 下：
   - 选择 `main` 分支
   - 选择 `/ (root)` 目录
   - 点击 "Save"
5. 等待几分钟后，您的网站将显示在顶部

---

## 第六步：设置管理员

### 首次设置管理员：
1. 使用邮箱注册一个账号
2. 访问 Firebase Console → Realtime Database
3. 找到您的用户数据
4. 手动修改该用户的 `role` 为 `2`
5. 刷新页面，您就可以访问管理后台了

---

## 🔧 配置数据库安全规则

为了安全起见，建议在 Firebase Console 中设置规则：

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null",
        ".write": "auth != null"
      }
    },
    "posts": {
      ".read": true,
      ".write": "auth != null"
    }
  }
}
```

在 Firebase Console → Realtime Database → Rules 中粘贴以上规则。

---

## 🎉 部署完成！

您的论坛现在可以访问了：
```
https://您的用户名.github.io/xiaomotou-forum/
```

---

## 常见问题

### Q1: GitHub Pages 显示 404
**解决方法**：
- 确认已正确启用 GitHub Pages
- 等待几分钟让部署完成
- 检查仓库名称是否与路径匹配

### Q2: Firebase 连接失败
**解决方法**：
- 检查 firebaseConfig 配置是否正确
- 确认已启用 Realtime Database
- 检查浏览器控制台错误信息

### Q3: 无法注册/登录
**解决方法**：
- 在 Firebase Console 中启用 Authentication
- 启用 "电子邮件/密码" 提供商
- 检查安全规则是否阻止了操作

### Q4: GitHub 登录不工作
**解决方法**：
- 确认已在 Firebase 中配置 GitHub OAuth
- 确认 GitHub OAuth App 的回调 URL 正确
- 检查 Client ID 和 Secret 是否匹配

---

## 📱 移动端适配

论坛已针对移动端优化，可以在手机上完美浏览！

---

## 🔄 更新网站

更新代码后：
```bash
git add .
git commit -m "更新内容描述"
git push
```

GitHub Pages 会自动更新（可能需要几分钟）。

---

## 📞 技术支持

如有问题，请检查：
1. Firebase 配置是否正确
2. GitHub Pages 是否启用
3. 浏览器控制台错误信息
