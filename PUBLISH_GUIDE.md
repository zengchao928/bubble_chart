# 发布到 pub.dev 指南

## 准备工作

### 1. 更新个人信息

在发布前，请更新以下文件中的个人信息：

- `pubspec.yaml`: 更新 `homepage`, `repository`, `issue_tracker` 字段
- `README.md`: 更新作者信息和 GitHub 链接
- `LICENSE`: 更新版权所有者名称

### 2. 创建 GitHub 仓库

```bash
cd bubble_chart
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/yourusername/bubble_chart.git
git push -u origin main
```

### 3. 验证 Package

运行以下命令验证 package 是否符合 pub.dev 规范：

```bash
flutter pub publish --dry-run
```

这会检查：
- pubspec.yaml 格式是否正确
- 是否包含必要的文件（README.md, LICENSE, CHANGELOG.md）
- 代码是否符合 Dart 规范
- 依赖版本是否合理

### 4. 运行测试

确保示例项目可以正常运行：

```bash
cd example
flutter pub get
flutter run
```

## 发布步骤

### 1. 登录 pub.dev

首次发布需要登录：

```bash
flutter pub login
```

这会打开浏览器，使用你的 Google 账号登录。

### 2. 发布 Package

```bash
flutter pub publish
```

系统会再次进行检查，然后询问是否确认发布。输入 `y` 确认。

### 3. 验证发布

发布成功后，访问 https://pub.dev/packages/bubble_chart 查看你的 package。

## 版本更新

### 1. 更新版本号

遵循语义化版本控制（Semantic Versioning）：

- **MAJOR** (1.0.0 → 2.0.0): 不兼容的 API 更改
- **MINOR** (1.0.0 → 1.1.0): 向后兼容的功能添加
- **PATCH** (1.0.0 → 1.0.1): 向后兼容的 bug 修复

在 `pubspec.yaml` 中更新 `version` 字段。

### 2. 更新 CHANGELOG.md

记录所有更改：

```markdown
## [1.1.0] - 2025-10-01

### Added
- 新功能描述

### Changed
- 更改描述

### Fixed
- 修复描述
```

### 3. 提交并发布

```bash
git add .
git commit -m "Release v1.1.0"
git tag v1.1.0
git push origin main --tags
flutter pub publish
```

## 最佳实践

### 1. 文档

- 保持 README.md 简洁明了
- 提供清晰的使用示例
- 添加 API 文档注释
- 包含截图或 GIF 演示

### 2. 测试

- 编写单元测试
- 测试不同 Flutter 版本的兼容性
- 测试不同平台（iOS, Android, Web）

### 3. 维护

- 及时响应 issues
- 定期更新依赖
- 保持与最新 Flutter 版本兼容
- 遵循 Dart 和 Flutter 最佳实践

### 4. 推广

- 在 README 中添加徽章（版本、许可证等）
- 分享到 Flutter 社区
- 在 Twitter、Reddit 等平台宣传
- 创建博客文章或视频教程

## 常见问题

### Q: 发布失败，提示包名已存在

A: 需要在 pub.dev 上选择一个唯一的包名。可以尝试：
- `bubble_chart_flutter`
- `animated_bubble_chart`
- `physics_bubble_chart`

### Q: 如何撤回已发布的版本？

A: pub.dev 不支持删除已发布的版本，但可以：
1. 发布一个新的修复版本
2. 在 CHANGELOG 中标记该版本为已弃用

### Q: 如何添加示例截图？

A: 
1. 在项目根目录创建 `screenshots` 文件夹
2. 添加截图文件
3. 在 README.md 中引用：`![Demo](screenshots/demo.gif)`
4. 提交到 GitHub

### Q: 如何提高 pub.dev 评分？

A: 确保：
- 完整的文档和示例
- 遵循 Dart 代码规范
- 支持多平台
- 定期更新维护
- 及时响应 issues

## 资源链接

- [pub.dev 发布指南](https://dart.dev/tools/pub/publishing)
- [语义化版本控制](https://semver.org/)
- [Dart 包布局规范](https://dart.dev/tools/pub/package-layout)
- [Flutter 包开发指南](https://flutter.dev/docs/development/packages-and-plugins/developing-packages)
