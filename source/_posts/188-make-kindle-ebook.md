---
title: 制作Kindle电子书
---

### txt文件初始格式
文本格式识别用 enca
```
brew install enca
enca file
```
使用iconv转换格式
```
iconv -c -f GB2312 -t UTF-8 file.txt > newfile.txt
```