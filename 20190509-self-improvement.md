自我提升才是解决现有困境的最佳选择。

---

## code what you think

### 写的程序出现非预期情况怎么办？How to debug your code?

**1. 定位问题的思路。不要跑偏！**
  - 先考虑正确的思路
  - 然后确认一下错误的现象和错误的本质原因
  - 去出现问题的本质原因的地方思考分析
  - 如果还是定位不出来，就单独拉 demo 把思路最简化，看正确与否

**2. 定位问题的工具**
  - chrome 控制台的深入学习 - deadline 20190531
  - 抓包工具的学习 - deadline 20190531
  
**3. 解决问题的方案**
  - 待积累

### 快速试错，快速确定方案

举例：在 vue 中使用 svg 文件，同时希望能通过样式来改变图标的颜色

  1. 放弃 背景图，img 标签等不可改变颜色的方案，本质上只有使用 svg 文件源码一个出发点
  2. 直接拷贝 svg 源码，但是文件多时成本大，不易维护，放弃
  3. 使用某个插件，能直接引入 svg 文件，快速测试 demo
      - [vue-svg-loader](https://github.com/visualfanatic/vue-svg-loader)，测试之后，有一个缺点，复杂的 svg 文件需要开发者 svg 文件内部定义 class 来在外部控制样式，目前满足需求，但设计师一旦替换 svg 文件，就需要开发者再次定义，待研究
      - [vue-svg-icon](https://github.com/cenkai88/vue-svg-icon)，这个也挺好用的，但是作者遗留一个[手误的问题](https://github.com/cenkai88/vue-svg-icon/issues/30)一直没解决，且复杂的 svg 文件读取不出正确的图标，放弃
      - [vue 官方方案](https://cn.vuejs.org/v2/cookbook/editable-svg-icons.html)，自己写组件，测试之后，官方提供的 svg 源码可行，但是自己设计师提供的 svg 文件不可行，目测对 svg 源码有要求，时间上不允许调研，放弃



### 参考
 - [如何成为优秀的技术主管？你要做到这三点](https://102.alibaba.com/detail/?id=312)

---

好记性不如烂笔头。