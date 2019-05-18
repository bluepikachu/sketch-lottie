# 渐变问题

#### 10. 开发文档中写明支持渐变但是为什么导出的变成了黑白渐变？

这个问题官方文档中没有描述，很有趣。 我在 github 的 issue 上逛了半天，出现这个问题的人很多，问题的产生与系统，区域语言设置，软件语言，插件版本，甚至色彩模式都可能有关系，所以提出的解决方法也很多，「对症下药」的难点是变量太多，一时也难以确定原因，只能把看到的方法都尝试一下。

我的系统版本：macOS 10.12.6 软件版本：Ae CC 2017（简体中文） 插件版本：Bodymovin 5.5.18

_实测对于本机可行：_

方法1： 用任何一个编译器打开 json 文件，搜索如下代码：

```python
"k":[0,1,1,1,1,0,0,0]
```

其中 \[0,1,1,1,1,0,0,0\] = \[0,r,g,b,1,r,g,b\]，每四项表示一个颜色，首位1/第五位0=渐变位置，r=红色/255，g=绿色/255，b=蓝色/255，颜色换算出来后填进去，以下为例子：

```python
"k":[0,0.969,0.773,0,1,1,0.871,0.573]
```

方法2： 将图层的渐变名称 Gradient 1，实测了一下以下名称：

可行：Gradient / Gradient 1 / gradient 1 / Gradient1 不可行：gradient

### 后来抽空又测试了一些随意起的英文名称，都可行，识别规律未明，建议还是用以上的名称。

_实测对于本机不可行_

方法3： 在渐变编辑器里把 TSL 改为 RGB 方法 1 和 3 都来源于以下文章： [https://medium.com/@mehdi.boumendjel/after-effects-x-bodymovin-x-lottie-gradients-black-white-rendering-issue-1ab36f2a2353](mailto:https://medium.com/@mehdi.boumendjel/after-effects-x-bodymovin-x-lottie-gradients-black-white-rendering-issue-1ab36f2a2353) 以下链接也有提及： [https://github.com/airbnb/lottie-web/issues/719](https://github.com/airbnb/lottie-web/issues/719)

方法4： 保存后再导出 json（一般是问按照上面的方法做了为什么没有效果的人需要的解决方法）

方法5： 合成名字改成 comp

方法6： Windows 系统下，修改系统区域后修改 Ae 语言，详细参考以下链接： [https://github.com/airbnb/lottie-web/issues/1201](https://github.com/airbnb/lottie-web/issues/1201)

#### 11. 怎么预览我的文件是否正确？

* 使用 Bodymovin 中的生成预览
* lottiefiles.com

#### 12. 图形在 Ae 中预览没有问题，但是最终输出有问题。

从 Sketch 中导入时必须是纯粹的图形，不能是由布尔运算计算出来的图形，简而言之，必须使用 Flatten 工具处理图形后才能导出。

#### 13. 我的图形全绿了！

我在一个项目中存在多个合成，每个合成中均有渐变，导出时发现所有的渐变都绿了（其实是都变成了第一个合成的渐变颜色）。 没有在网上搜索过原因，大概能猜到原因：具有渐变的图形，渐变属性都是从另外一个图形复制而来的，之后再进行颜色的修改。虽然颜色不一致，但是渐变的名称都是Gradient1，插件对相同的名称的属性进行了统一处理。 解决方法也很简单： 重命名渐变的名称或者将合成分离到多个项目中都可以解决。

