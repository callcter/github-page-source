---
title: iOS新手常见错误：this class is not key value coding-compliant for the key xx
---

reason：[<xx项目名.xxController> setValue:forUndefinedKey]:this class is not key value coding-compliant for the key xx
Xcode控制台报这个错误的时候，说明IBAction和IBOutlet有多余或者错误的连线。
解决方法：
1.在storyboard上选中（点击黄色圈圈）reason中提及的xxController
2.点击属性栏最右边的show the connections inspector（箭头）那里，将显示黄色感叹号的删去重新连接即可。
