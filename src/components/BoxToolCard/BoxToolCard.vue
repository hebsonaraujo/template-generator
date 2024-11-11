<template>
     <span class="btn-box" >
        <span class="plus">+</span>
        <span class="edit" v-if="edit">::</span>
        <span class="close">x</span>
    </span>
</template>

<script>

export default {
  props: {
    edit: Boolean,
  },
  name: 'BoxToolCard',
    data: function() {
        return {}
    }
}
</script>
<style>
.btn-box {
    width: 75px;
    height: 15px;
    background: pink;
    position: absolute;
    top: -15px;
    left: calc(50% - 37.5px);
    display: flex;
    border-radius: 5px 5px 0 0;
    align-items: center;
    cursor: pointer;
}
.plus {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 50%;
  top: -14px;
  left: 50%;
}
.close {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 50%;
}
</style>

