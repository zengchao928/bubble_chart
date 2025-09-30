# 在原项目中集成 bubble_chart 指南

## 方案一：使用本地路径（开发阶段）

在发布到 pub.dev 之前，可以先在原项目中使用本地路径引用：

### 1. 修改 crypto-news-flutter 的 pubspec.yaml

```yaml
dependencies:
  bubble_chart:
    path: ../bubble_chart
```

### 2. 运行依赖安装

```bash
cd /Users/smart-mac/AndroidStudioProject/outsourcing/crypto-news-flutter
flutter pub get
```

### 3. 修改 home_market_bubble.dart

替换原有的导入和实现：

```dart
import 'package:bubble_chart/bubble_chart.dart';
import 'package:get/get.dart';

class HomeMarketBubble extends StatelessWidget {
  final Function(String)? onTap;

  const HomeMarketBubble({Key? key, this.onTap}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final controller = Get.find<HomeController>();
    final maxWidth = MediaQuery.of(context).size.width - 20;
    final maxHeight = maxWidth * 0.8;

    return GetBuilder<HomeController>(builder: (_) {
      return BgContainer(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            // 标题栏保持不变
            Row(
              children: [
                showAssetImage(Assets.imagesIconFire,
                    width: 12, fit: BoxFit.fitWidth),
                HEmptyView(10),
                Text('市场热力图', style: WHITE_18_TEXT_STYLE),
                Spacer(),
                _buildReportTag(HeatMapTime.oneHour),
                HEmptyView(5),
                _buildReportTag(HeatMapTime.hour24),
                HEmptyView(5),
                _buildReportTag(HeatMapTime.day7),
              ],
            ),
            SizedBox(height: 20),
            // 使用新的 BubbleChart 组件
            Obx(() {
              final titles = _getTitles();
              final values = _getValues(controller.selectTimeType.value);

              if (titles.isEmpty || values.isEmpty) {
                return SizedBox(
                  height: maxHeight,
                  child: Center(
                    child: Text('暂无数据', style: GREY_14_TEXT_STYLE),
                  ),
                );
              }

              return SizedBox(
                height: maxHeight,
                child: BubbleChart(
                  names: titles,
                  values: values,
                  onBubbleTap: onTap,
                  minRadius: 30.0,
                  maxRadius: 55.0,
                  minOpacity: 0.25,
                  maxOpacity: 0.6,
                  showBorder: true,
                  borderWidth: 2.0,
                ),
              );
            }),
          ],
        ),
      );
    });
  }

  List<String> _getTitles() {
    final controller = Get.find<HomeController>();
    final titles = controller.coinMap.value['titles'];
    return titles != null ? List<String>.from(titles) : [];
  }

  List<double> _getValues(HeatMapTime timeType) {
    final controller = Get.find<HomeController>();
    return List<double>.from(
        controller.coinMap.value[timeType.displyName] ?? []);
  }

  Widget _buildReportTag(HeatMapTime timeType) {
    final controller = Get.find<HomeController>();
    return Obx(() {
      final selected = controller.selectTimeType.value == timeType;
      return GestureDetector(
        onTap: () {
          controller.switchTimeTpye(timeType);
        },
        child: Container(
          decoration: BoxDecoration(
              color: selected
                  ? Color(0x336C5CE7)
                  : Colors.grey.withValues(alpha: 0.3),
              borderRadius: BorderRadius.circular(99)),
          padding: EdgeInsets.symmetric(horizontal: 15, vertical: 5),
          child: Text(
            timeType.displyName,
            style: selected ? PURPLE_12_TEXT_STYLE : WHITE_12_TEXT_STYLE,
          ),
        ),
      );
    });
  }
}
```

## 方案二：发布后使用（推荐）

### 1. 发布到 pub.dev

按照 PUBLISH_GUIDE.md 的步骤发布 package。

### 2. 在 pubspec.yaml 中添加依赖

```yaml
dependencies:
  bubble_chart: ^1.0.0
```

### 3. 运行依赖安装

```bash
flutter pub get
```

### 4. 使用方式同方案一

## 优势对比

### 使用独立 package 的优势：

1. **代码解耦** - 气泡图组件与业务逻辑分离
2. **易于维护** - 独立版本管理和更新
3. **可复用** - 可在多个项目中使用
4. **社区贡献** - 开源后可接受社区改进
5. **性能优化** - 专注于组件本身的性能优化

### 原实现的优势：

1. **深度集成** - 与 GetX 和项目架构紧密结合
2. **定制化** - 完全符合项目特定需求
3. **无额外依赖** - 不需要引入外部 package

## 建议

- **短期**：使用方案一（本地路径），在原项目中测试新组件
- **中期**：发布到 pub.dev，使用方案二
- **长期**：根据社区反馈持续改进 package

## 注意事项

1. 新组件移除了对 GetX 的依赖，需要手动传递数据
2. 需要在父组件中管理状态和数据更新
3. 样式配置需要通过参数传递，不再使用项目全局配置
4. 如果需要保持原有的深度集成，可以创建一个 wrapper 组件
