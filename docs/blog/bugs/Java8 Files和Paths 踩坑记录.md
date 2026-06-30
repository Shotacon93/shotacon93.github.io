
> java7引进的java.nio.file包的Files和Paths还有其他工具类, 以代替File和各类Stream进行文件的IO操作.
>
> 好用是好用, 就是总有些细节让人蛋疼.
>
> 逐步更新中.

<!-- more -->

## 1. Files.createFile(path)或者Files.write()时抛出NoSuchFileException

这两个方法在path含有路径时, 会直接去创建目标文件, 所以当路径中间有文件夹未被创建, 就抛出此异常.

```java
// 加个判断就OK了
Path path = Paths.get(dir);
if (!Files.exists(path))
	Files.createDirectories(path);
```

## 2. java.nio.charset.MalformedInputException: **Input** **length** **=** **1**

```java
// Files读取文件的默认编码是UTF-8
public static Stream<String> lines(Path path) throws IOException {
	return lines(path, StandardCharsets.UTF_8);
}
```

通常该异常是因为读取的文件编码不是utf-8引起的.

但我这次引起是因为mac创建的[DS_Store](https://zh.wikipedia.org/wiki/.DS_Store)文件, 该文件是mac用于贮存目录自定义属性, 例如文件夹们的图标位置或者背景色之类.

```java
// 特意的加了文件名的判断. 避免错误的解析DS_Store文件.
List<Path> fileList = Files.list(Paths.get(basePath))
				.filter(path -> !Files.isDirectory(path) && !path.getFileName().toString().contains("DS_Store"))
				.collect(Collectors.toList());
```



