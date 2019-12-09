##JVM参数   如 -Xmx10240m -Xms10240m -Xmn5120m -XXSurvivorRatio=3  -Xss128k

* 1. -Xmx：最大堆大小
* 2. -Xms：初始堆大小
* 3. -Xmn：年轻代大小
* 4. -XXSurvivorRatio：年轻代中Eden区与Survivor区的大小比值
* 5. -Xss：每个线程的堆栈大小

``` html
年轻代5120m， Eden : Survivor=3，Survivor区大小=1024m（Survivor区有两个，即将年轻代分为5份，每个Survivor区占一份），总大小为2048m。

-Xms初始堆大小即最小内存值为10240m

![Image text](https://github.com/yinyinfeiqiao/advanced-java-skills/blob/master/photo/20181222111036133.jpg)

```
