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

