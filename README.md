# h5-dray

> 基于h5开发的拖拽组件

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

如果不是很了解h5拖拽api的一些特性，请移步->

### [掘金大佬讲h5拖拽的一个帖子](https://juejin.im/post/5a169d08518825592c07c666#heading-6)

------

 由于项目需求，需要实现一个拖拽组件，是一个基于html5实现的一个组件，h5的功能相对来讲已经比较完善了，因为还是在demo阶段，所以目前没有什么定制化功能，只是实现了交换功能
 
![](https://user-gold-cdn.xitu.io/2019/4/17/16a293a904fa7ab9?w=1241&h=481&f=png&s=18251)

### 目录结构：

箭头为我们的组件

![](https://user-gold-cdn.xitu.io/2019/4/17/16a293d2ad465a9d?w=626&h=414&f=png&s=55097)


### demo代码：
### 组件源代码
``` javascript
<template>
  <div
    class="contain"
    :id="`drag`+index"
    :ref="`drag`+index"
    draggable="true"
    v-on:dragstart="handle_dragstart"
    v-on:drag="handle_drag"
    v-on:drop="handle_ondrop"
    v-on:dragover="allowDrop"
  >
    <slot></slot>
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  // 唯一标识，必传
  props: {
    index: {
      type: Number,
      required: true
    }
  },
  methods: {
    swapElements(a, b) {
      // 交换两个dom元素
      if (a == b) return;
      //记录父元素
      var bp = b.parentNode,
        ap = a.parentNode;
      //记录下一个同级元素
      var an = a.nextElementSibling,
        bn = b.nextElementSibling;
      //如果参照物是邻近元素则直接调整位置
      if (an == b) return bp.insertBefore(b, a);
      if (bn == a) return ap.insertBefore(a, b);
      if (a.contains(b))
        //如果a包含了b
        return ap.insertBefore(b, a), bp.insertBefore(a, bn);
      else return bp.insertBefore(a, b), ap.insertBefore(b, an);
    },
    handle_dragstart(ev) {
      ev.dataTransfer.setData("dragDom", ev.target.id);
    },
    handle_drag() {
      console.log("drag-在元素开始时反复触发");
    },
    handle_dragend() {
      console.log("dragend-在拖动操作完成时触发");
    },
    allowDrop(ev) {
      ev.preventDefault();
    },
    handle_ondrop(event) {
      let data = event.dataTransfer.getData("dragDom");
      console.log(document.getElementById(data), event.target, "这两个dom");
      this.swapElements(document.getElementById(data), event.currentTarget);
      console.log(event.target, "dragend-放下时时触发");
    },
    dragInit() {}
  },
  components: {},
  mounted() {
    this.dragInit();
  }
};
</script>
<style>
  .contain{
    cursor: pointer;
  }
</style>


```


### demo代码
``` javascript
<template>
  <div class="box">
    <dragAndDrop
      class="contain"
      v-for="(item, index) in arr"
      :key="index"
      :index='index'
    >
    {{index}}
    </dragAndDrop>
  </div>
</template>
<script>
import dragAndDrop from "./dragAndDrop";
export default {
  data() {
    return {
      arr: []
    };
  },
  methods: {
  },
  components: {
    dragAndDrop
  },
  mounted() {
    this.arr = new Array(9).fill(null);
    // this.dragInit();
  }
};
</script>
<style>
.box {
  display: flex;
  flex-wrap: wrap;
}
.contain {
  width: 200px;
  height: 200px;
  background: purple;
  margin: 20px;
  font-size: 40px;
}
</style>
```