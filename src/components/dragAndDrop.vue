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

