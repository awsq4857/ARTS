# Cocoapods的缓存问题

在pod开发过程中，由于一些人不想每次更改，都打新tag。有时会把原有的tag删除，修改代码后，把tag设置成和原来一样的。这时，如果有人在修改前，下载了pod，那他本地就会有缓存，再次更新时，代码不会更新，因为pod的tag没变。

这时，可以进入如下操作

1. 进入缓存目录`~/Library/Caches/Cocoapods`里删除对应的源码
2. 删除工程目录里的Podfile.lock文件，或者对应的pod
3. 更新pod，`pod install`



