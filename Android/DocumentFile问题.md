# DocumentFile问题

1、DocumentFile.fromTreeUri 当对选择后返回的uri路径fromTreeUri后，然后再次listDir后会对获取到子目录路径uri进行fromTreeUri时会出现再次返回的是先前目录的子内容，是个问题，网上看解决的方法是DocumentFile升级解决

## 参考

[Unexpected behavior when DocumentFile.fromTreeUri() is called on Uri of subdirectory DocumentFile object](https://stackoverflow.com/questions/62375696/unexpected-behavior-when-documentfile-fromtreeuri-is-called-on-uri-of-subdirec)
