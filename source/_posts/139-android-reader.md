---
title: Android文件读取
---

```javascript
@ReactMethod
public void loadFile(String path, int size, int page, Promise promise){
  String code = codeString(path);
  try {
    File file = new File(path);
    randomAccessFile = new RandomAccessFile(file, "r");
    randomAccessFile.seek(0);
    byte[] chs = new byte[size + 30];
    randomAccessFile.read(chs);
    String content = new String(chs, code);
    Log.d("res", content);
    promise.resolve(content);
  } catch (FileNotFoundException e){
    e.printStackTrace();
  } catch (IOException e){
    e.printStackTrace();
  }
}
```