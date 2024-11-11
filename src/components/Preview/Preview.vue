<template>
    <div class="preview-container" >
        Preview - {{ templateName }}
        <div id="main"
          :style="{ height: heightBox,width: widthBox}"
          @mouseover="mouseOv()"
          @mouseout="mouseOu()"
          :class="{ activeWrapper: toggleActiveWrapper(this.$el)}"
          >
            <BoxToolCard :edit="true" v-if="isWrapperActive()"/>
            <template v-if="rowsNumber > 0">
              <div v-on:click="calc" v-bind:style="{ 'height': `${calc()}` }"
                  v-for="(index) in rowsNumber"
                  :class="{ active: toggleActiveEl(index)}"
                  class="container preview"
                  :key="index">
                  <CardStructure/>
              </div>
          </template>
        </div>
    </div>
</template>

<script>
import CardStructure from '../Card/Card.vue'
import BoxToolCard from '../BoxToolCard/BoxToolCard.vue'
export default {
  name: 'PreviewTable',
  components: { CardStructure, BoxToolCard },
  props: {
    templateName: String,
    widthBox: String,
    heightBox: String,
    numRows: String,
    props: Object
  },
  data: function() {
    return {
      rows: Number(this.numRows),
      activeIdx: -1,
      elActive: false,
      elWrapperActive: false,
      items: [
        {
          id: 0,
          title: 'Item A',
          list: 1,
        },
        {
          id: 1,
          title: 'Item B',
          list: 1,
        },
        {
          id: 2,
          title: 'Item C',
          list: 2,
        },
      ],
      row: [
        [{type: 'img', src:"flamengo.png"},{type: 'text',field: 'Lorem inpsu <strong>evid</strong> atus limus shablal'}],
        [{type: 'img', src:""}]
      ]
    }
  },
  computed:{
      rowsNumber() {
        return Number(this.numRows)
      }
  },
  methods: {
    calc() {
      if(this.numRows && this.heightBox) {
        return `${Number(this.heightBox.replace('px','')) / Number(this.numRows)}px`
      }
    },
    startDrag(evt, item) {
      evt.dataTransfer.dropEffect = 'move'
      evt.dataTransfer.effectAllowed = 'move'
      evt.dataTransfer.setData('itemID', item.id)
    },
    onDrop(evt, list) {
      const itemID = evt.dataTransfer.getData('itemID')
      const item = this.items.find((item) => item.id == itemID)
      item.list = list
    },
    drop(ev) {
      ev.preventDefault();
      ev.dataTransfer.getData("text");
      ev.target.appendChild(document.getElementById(ev.target));
    },
    allowDrop(ev) {
      ev.preventDefault();
    },
    mouseOv() {
      this.elWrapperActive = !this.elWrapperActive;
    },
    mouseOu() {
      this.elWrapperActive = !this.elWrapperActive;
      this.activeIdx = -1;
    },
    toggleActiveWrapper() {
      return this.elWrapperActive ? 'active' : false;
    },
    isWrapperActive() {
      return this.elWrapperActive;
    },
    toggleActiveEl(el) {
      return this.activeIdx === el ? 'active' : '';
    },
    elIsActive() {
      return this.elActive;
    }
  }
}
</script>
<style>
.preview-container {
    flex: 1;
    padding: 20px;
    border: 1px dashed #ccc;
    border-radius: 5px;
    background-color: #fff;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}
#main {
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin: 0 auto;
    min-height: auto;
    position: relative;
}
.my-custom-width {
  width: 100px;
  height: 355px
}
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
.d-flex {
  display: flex;
}
.d-block {
  display: block;
}
.d-none {
  display: none;
}
.plus {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 50%;
  top: -14px;
  left: 50%;
}
.edit {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 50%;
}
.close {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 50%;
}

.active {
  border: 1px dashed blueviolet !important;
}
.activeWrapper {
  border: 1px dashed green !important;
}
.activeTop {
    width: 15px;
    height: 15px;
    border-radius: 5px 5px 0 0;
    background: red;
    display: block;
}
.container {
  display: flex;
  position: relative;
}
.drop-zone {
  background-color: #eee;
  margin-bottom: 10px;
  padding: 10px;
}
.drag-el {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 5px;
}
</style>