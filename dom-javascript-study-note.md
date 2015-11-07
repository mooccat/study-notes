# javascript之DOM
DOM：（文档对象模型）描绘了一个层次化的节点树。
## 1.节点层次
HTML DOM 将 HTML 文档视作树结构。这种结构被称为节点树：
![文档模型](./ct_htmltree.gif)
### 1.1Node类型
1.11、Node的属性介绍:
>nodeType：显示节点的类型
nodeName：显示节点的名称
nodeValue：显示节点的值
attributes：获取一个属性节点
firstChild：表示某一节点的第一个节点
lastChild：表示某一节点的最后一个子节点
childNodes：表示所在节点的所有子节点
parentNode：表示所在节点的父节点
nextSibling：紧挨着当前节点的下一个节点
previousSibling：紧挨着当前节点的上一个节点