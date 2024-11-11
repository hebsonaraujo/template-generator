<template>
   <main>
        <div class="form-container">
            <form id="generator-form">
                <ElementsToolCard/>
                <InputsForm @valorCapturado="getAction"/>
            </form>
        </div>
        <PreviewTable
            :templateName="templateName"
            :widthBox="widthBox"
            :heightBox="heightBox"
            :numRows="numRows"/>
    </main>
</template>

<script>
import PreviewTable from '../Preview/Preview.vue'
import ElementsToolCard from '../ElementsToolCard/ElementsToolCard.vue'
import InputsForm from '../Inputs/Inputs.vue'

export default {
  name: 'ConfigForm',
  components: { PreviewTable, ElementsToolCard, InputsForm },
  props: {
    msg: String,
  },
  data: function() {
    return {
        templateName: 'My template',
        widthBox: '280px',
        heightBox: '355px',
        numRows: '1',
    }
  },
  methods: {
    getAction: function(ev) {
        this[ev[0]] = ev[1];
    }
  }
}
</script>
<style scoped>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}
header {
    background-color: #333;
    color: white;
    text-align: center;
    padding: 20px;
}
main {
    display: flex;
    justify-content: space-around;
    align-items: center;
    min-height: 80vh;
    max-width: 1100px;
}
.form-container {
    max-width: 450px;
    flex: 1;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}
#generate {
    background-color: #007BFF;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
}
#generate:hover {
    background-color: #0056b3;
}
form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}
</style>
