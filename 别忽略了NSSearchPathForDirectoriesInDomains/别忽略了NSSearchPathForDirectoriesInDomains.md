# 别忽略了NSSearchPathForDirectoriesInDomains



最近在使用 Instruments 对公司产品进行优化时，发现 NSSearchPathForDirectoriesInDomains 方法在执行的时候并不是很高效。

```
NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory directory, NSSearchPathDomainMask domainMask, BOOL expandTilde)
```

而在笔者的程序中大多都是调用在UI线程上，这样调用次数多了，它也就成了App卡顿的元凶之一了。

网上相关 NSDateFormatter 的性能问题一搜一大堆，但是笔者没有找到关于 NSSearchPathForDirectoriesInDomains 性能问题的相关文章，具体原因有可能是由于大家并没有频繁的使用，也或者其他原因吧😂。

于是这里就干脆对 NSDateFormatter 和 NSSearchPathForDirectoriesInDomains 做了一个简单的对比，你可以在这找到[测试代码](https://github.com/hwzss/TestNSSearchPathForDirectoriesInDomains)

对比结果 NSSearchPathForDirectoriesInDomains 执行花费的时间约为 NSDateFormatter 执行时间转换的一半(笔者这里在iPhone 5s，iPhone6 上进行的测试)

##总结

如果你程序中也大量在UI线程上使用着 NSSearchPathForDirectoriesInDomains，记得对它进行优化，放置在UI线程外或者对它的结果进行Cache。

##注
测试代码记得在真机上跑，在模拟器上，NSSearchPathForDirectoriesInDomains 效率还是不错的😂


