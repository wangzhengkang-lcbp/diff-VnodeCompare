## h函数

- `通过h函数构建虚拟dom节点(Vnode)`

- ```js
  {//h( "节点名", {key:}, [h()]|"" ) 函数返回的对象:
  	children: undefined  // 子元素 数组
  	data: {}  // 属性、样式
  	elm: undefined //对应的真正的dom节点，undefined表示节点还没有上dom树
  	sel: "" // 选择器
      key: undefined  //虚拟dom唯一标识
  	text: "" // 文本内容
  }
  ```

## createElement函数

- 该函数的作用是：`将Vnode虚拟节点创建为真实的DOM节点、并再Vnode.elm属性上挂载该真实dom`
- `子节点的创建用到了递归`

## patch函数

- `对比新旧虚拟dom、最小量更新dom节点`

## 相关问题

- 如何判断新旧虚拟dom相同？
  - **选择器相同且key相同**
- 新旧虚拟dom比较的注意点：
  - **相同两个虚拟dom才会对子节点做精细化比较，不同的直接覆盖**
  - **只进行同层比较，不会进行跨层比较**
- 用到的操作dom的API

```js
parentNode.insertBefore(newNode, existNode) 	// insertBefore
//在existNode前插入newNode，如果newNode是parentNode的子元素则为移动位子
//node.nextSibling代表node的下一个相邻节点

parentNode.appendChild(newNode)   //appendChild
//在parentNode节点中的末尾插入子节点

parentNode.removeChild(child)   //removeChild
//删除parentNode中的子元素child
```

## 四种命中查找（重点）

- ###### 比较 ① ：旧前 / 新前、如果相同则移动指针

- ###### 比较 ②： 旧后 / 新后、如果相同则移动指针

- ###### 比较 ③： 旧前 / 新后 、将`新后`指向的节点移动到`旧后`之后+移动指针

- ###### 比较 ④： 旧后 /  新前、将`新前`指向的节点移动到`旧前`之前+移动指针

- ###### 四种都没命中 又分两种情况：

  - ###### 利用旧虚拟dom创建map映射{key:i}表、再通`新前`的key过快速查、找到了就说明该节点只是移动了位置、将该节点插入到`旧前`之前，并将原来位子的旧虚拟节点赋值为undefined、移动指针

  - ###### 没找的就是新节点，直接插入所有未处理旧节点之前

- ###### 循环结束：如果新旧虚拟dom还有剩余未处理、说明剩余的是新增或删减的虚拟dom、新增按照规则3/4加入相应 的位置

## 图解diff算法流程

<img src="https://img-blog.csdnimg.cn/20210415222737884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述" style="zoom: 200%;" />
