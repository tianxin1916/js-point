dom 节点知识
#### Node常用属性
- nodeType：显示节点的类型
- nodeName:显示节点的名称
- nodeValue:显示节点的值
- attributes:获取一个属性节点
- firstChild:表示某一个节点的第一个节点
- lastChild:表示某一个节点的最后一个节点
- childNodes:表示所在节点的所有子节点
- parentNode:表示所在节点的父节点
- nextSibling:紧挨着当前节点的下一个节点
- previousSibling：紧挨着当前节点的上一个节点

#### 操作节点
- appendChild()方法：用于向父节点的childNodes末尾添加一个节点
- insertBefore()：接收两个参数，要插入的节点和作为参照的节点
- replaceChild()：接收两个参数：要插入的节点和要替换的节点
- removeChild()：要移除的节点
- cloneNode()：该方法接受一个布尔型参数表示是否进行深浅负责，为true时执行深复制（复制节点及其整个子节点树），为false时执行浅复制（只复制节点本身）。

#### Document 类型
- 文档信息
 - document.title包含着<title>元素中文本的引用
 - document.URL包含页面完整的URL
 - document.domain包含着页面域名。它是可以读取也可以设置的
 - document.referrer保存着链接到当前页面那个页面的URL
- 查找元素
 - document.getElementById()
 - document.getElementsByTagName()
- 特殊集合
 - document.anchors 包含文档中所有带name特性的<a>标签
 - document.forms 包含文档中所有的<form>元素
 - document.images 包含文档中所有的<img>元素
 - document.links 包含文档中所有带href特性<a>元素
- 文档写入
 - document.write()或writeIn() 可以在页面加载过程中动态的向页面加入内容
#### Element类型
 - 取得特性
  - getAttribute()
  - setAttribute()
  - removeAttribute()
 - 创建元素
  - document.createElement()

#### Text类型
 - 创建文本节点
   document.createTextNode()
 - 分割文本节点
   splitText()

#### DOM扩展
  - querySelector()
  - querySelectorAll()
  - getElementsByClassName()
   - classList属性
     - add()
     - contains()
     - remove()
     - toggle()

