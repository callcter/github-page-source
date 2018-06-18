---
title: Sublime Text 3 插件
tags:
  - sublime
url: 64.html
id: 64
categories:
  - 开发工具
date: 2016-05-07 18:01:51
---

**首先安装Package Control** ctrl+`调出控制台输入：

import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed\_packages\_path(); urllib.request.install\_opener( urllib.request.build\_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

然后，其他插件 ConvertToUTF8 Bracket Highlighter DocBlockr Emmet(Zen Coding) SideBar Enhancements Themr SublimeLinter Sublime CodeIntel ColorPicker TrailingSpaces AutoPrefixer