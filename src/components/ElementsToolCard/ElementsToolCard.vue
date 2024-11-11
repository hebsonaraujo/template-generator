<template>
    <div
        class="drop-zone"
        @dragover.prevent
        @dragenter.prevent>
        <p>ELEMENTS</p>
        <div
            class="drag-el"
            v-for="(item,index) in items"
            :data-type="item.type"
            :key="item.title"
            draggable
            @dragstart="drag"
            :id="`drag-${index}`">
            {{ item.title }}
        </div>
    </div>
</template>

<script>
export default {
  name: 'ElementsToolCard',
  data: function() {
    return {
        items: [
            {
                id: 0,
                type: 'text',
                title: 'text',
                list: 1,
            },
            {
                id: 1,
                type: 'image',
                title: 'Image',
                list: 1,
            },
            {
                id: 2,
                type: 'button',
                title: 'Button',
                list: 1,
            }
        ],
    }
  },
  methods: {
    drag: function(ev){
        ev.dataTransfer.dropEffect = 'move'
        ev.dataTransfer.effectAllowed = 'move'
        ev.dataTransfer.setData("text", ev.target.id);
        ev.dataTransfer.setData("data", ev.target);
        ev.dataTransfer.setData("type", ev.target.dataset.type);
    }
  }
}
</script>
<style>

</style>