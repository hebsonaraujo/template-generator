<template>
    <div>
        <div v-for="(item, index) in inputs" :key="index">
            <label :for=item.name>{{ item.text }}</label>
            <input
                @focusout="router"
                :type=item.text
                :id=item.name
                :name=item.name
                min="1">
            <br>
        </div>
    </div>
</template>

<script>
export default {
  name: 'InputsForm',
  props: {
  },
  data: function() {
    return {
        inputs:  [
            { text: 'Template\'s Name:',type: "text",name:'templateName' },
            { text: 'Template\'s Width:',type: "number",name:'divWidth' },
            { text: 'Template\'s Height:',type: "number",name:'divHeight' },
            { text: 'Number of Rows:',type: "number",name:'numRows' }
        ]
    }
  },
  methods: {
    router: function(ev) {
        if(ev.target.value !== "") {
            if(ev.target.name === 'templateName') {
                this.$emit('valorCapturado', ['templateName',ev.target.value])
            }
            if(ev.target.name === 'divWidth') {
                this.$emit('valorCapturado', ['widthBox',ev.target.value+ 'px'])
            }
            if(ev.target.name === 'divHeight') {
                this.$emit('valorCapturado', ['heightBox',ev.target.value+'px'])
            }
            if(ev.target.name === 'numRows') {
                this.$emit('valorCapturado', ['numRows',ev.target.value])
            }
        }
    }
  }
}
</script>

<style>
label {
    font-weight: bold;
}
input[type="number"],
input[type="text"] {
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 3px;
}
input[type="number"]:focus,
input[type="text"]:focus {
    outline: none;
    border-color: #007BFF;
}
</style>