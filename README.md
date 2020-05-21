


**不合理的地方欢迎批评指正！！！**

#  1 分享声明

 1. 本文是学习过程的阶段性总结，肯定有很多用法不合适的地方，在此欢迎批评指正；
 2. 本文注重在于理解PYNQ，而不是简单的会使用即可，大部分是结合PYNQ的源码、它基于的Ubuntu18.04、MPSOC数据手册、Ultra96-V2原理图、设备树来进行理解开发；
 3. 本文是目前个人需要学习的内容，像MIMO、MicroBlaze Subsystem、使用HLS加速等重要内容没有涉及；
 4. Ultra96开发PYNQ没有PYNQ-V2板子的资料丰富，这也是本文出现的原因之一。

#  2 适用群体

 1. 对Ultra96-V2的PYNQ开发无从下手；
 2. 对PYNQ的模块函数使用很茫然；
 3. 不明白FPGA部分的设计；
 4. ...。

总而言之，言而总之，本总结对初学者比较友好。

#  3 内容目录
 1. [Ultra96 PYNQ入门之一——PS端控制MIO与EMIO](https://blog.csdn.net/qq_35712169/article/details/106038000)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520222134225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center =450x160)
 2. [Ultra96 PYNQ入门之二——PS端控制AXI GPIO](https://blog.csdn.net/qq_35712169/article/details/106246416)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200520234402673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center )
 3. [Ultra96 PYNQ入门之三——中断与协程](https://blog.csdn.net/qq_35712169/article/details/106247110)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200521000107102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center)
 4. [Ultra96 PYNQ入门之四——数据搬运能手DMA](https://blog.csdn.net/qq_35712169/article/details/106249333)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200521092155846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200521094405745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center)
 
 5. [Ultra96 PYNQ入门之五——PMIC](https://blog.csdn.net/qq_35712169/article/details/106253118)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200521114526837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70)
 6. [Ultra96 PYNQ入门之六——连接WiFi](https://blog.csdn.net/qq_35712169/article/details/106254473)
 7. **未完待续...。**

 

#  4 开发环境

 **[核心板卡_Ultra96-V2](https://www.avnet.com/wps/portal/us/products/new-product-introductions/npi/aes-ultra96-v2/)**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052112350474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center)
**[软件环境_PYNQ-V2.5](http://www.pynq.io/board.html)**![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052020170826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center)

**辅助仪器**
因Ultra96-V2板卡没有合适的外设，需要结合一些示波器、信号发生器、电压表、逻辑分析仪啥的辅助分析，现在因疫情在家，口袋仪器较为合适hhh，比较推荐AD2。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200521123350869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center)
**os：AD2老贵了，我手里的一块AD2还是从本科实验室借出来的，因现在是研究生，本科实验室老师老是催我还hhh。**


<br />
<br />

**原创不易，严禁剽窃！**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918110341834.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NzEyMTY5,size_16,color_FFFFFF,t_70#pic_center =200x200)


欢迎大家关注我创建的微信公众号——小白仓库
原创经验资料分享：包含但不仅限于FPGA、ARM、RISC-V、Linux、LabVIEW等软硬件开发，另外分享生活中的趣事以及感悟。目的是建立一个平台记录学习过的知识，并分享出来自认为有用的与感兴趣的道友相互交流进步。
