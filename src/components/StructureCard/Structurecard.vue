<template >
    <div class="structure-card">
        <template v-if="structureType">
            <div class="container card-structure" >
                <BoxToolCard/>
                <div
                    :class="`${gridSystem(index,value)}`"
                    v-for="(value,index) in column"
                    :key="index"
                    class="col border al-c al-m"
                    @dragover.prevent
                    @dragenter.prevent
                    @drop="onDrop($event, index)"
                    @dragstart="startDrag($event, index)"
                >
                    <template v-if="columnStack[index].length === 0" >
                        <span class="add">
                            +
                        </span>
                    </template>
                    <template v-else>
                        <div
                            class="type-tag"
                            draggable
                            v-for="(value,index) in columnStack[index]"
                            :data-type="value"
                            :id="`la-${index}-${value}`"
                            :key="index">
                                <figure v-if="value==='image'">
                                    <!-- visual example-->
                                    <img
                                        src="../../../public/flamengo.png">
                                </figure>
                                <p v-if="value==='text'">{{value}}</p>
                                <button v-if="value==='button'">{{value}}</button>
                        </div>
                    </template>
                </div>
            </div>
        </template>
        <template v-else>
            <TitleInfo v-bind:text="text" v-bind:align="align"/>
            <section class="p-t-10"  v-for="(i) in rowStructure.row" :key="i">
                <span
                    class="shape s-one "
                    v-for="(value,index) in rowStructure.column[i-1]"
                    :key="index"
                    v-html="factory(value)"
                    v-on:click="selectStructure(index,value)">
                </span>
            </section>
        </template>
    </div>
</template>
<script>
import TitleInfo from '../Title/Title.vue'
import BoxToolCard from '../BoxToolCard/BoxToolCard.vue'
export default {
    name: 'StructureCard',
    components: { TitleInfo, BoxToolCard },
    data: function() {
        return {
            columnStack: [],
            item:[],
            mapObj : {
                100: 'b-0',
                75: 'b-1',
                50: 'b-2',
                33: 'b-3',
                25: 'b-4',
                20: 'b-5'
            },
            rowStructure: {
                row: 2,
                column:[
                    [[100],[50,50],[33,33,33]],
                    [[25,75],[75,25],[25,25,25,25]]
                ]
            },
            hasChoosen: false,
            text: 'Select your structure',
            align: 'center'
        }
    },
    computed: {
        structureType() {
            return this.hasChoosen;
        }
    },
    methods: {
        gridSystem(item,value){
            return this.mapObj[value];
        },
        /**
         *  Seleciona a estrutura da linha (row) : quantas colunas vai possuir
         * @param {*} i - posicao da row (linnha)
         * @param {*} value - arr [100, __ob__: Observer]
         */
        selectStructure(i,value) {
            this.column = value;
            this.hasChoosen = true;
            this.columnStack = Array.from({ length: value.length}, () => []); //[[],[]]
        },
        factory(arr) {
            let strHTML = '';
            const size = arr.length;
            const div = size - 1;
            const mapObj = {
                100: 'b-0',
                75: 'b-1',
                50: 'b-2',
                33: 'b-3',
                25: 'b-4',
                20: 'b-5'
            }
            if(size > 1) {
                arr.forEach((element,index) => {
                    strHTML += `<span class='box ${mapObj[element]}' v-html='rawHtml'></span>`;
                    if(index < div) {
                        strHTML+= '<span class="line"></span>'
                    }
                });
                return strHTML;
            } else {
                return  `<span class="" v-html="rawHtml"></span>`
            }
        },
        startDrag(evt, index) {
            // https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/getData
            const pos = this.columnStack[index];
            const elem = evt.currentTarget.firstChild.dataset.type;
            const idx = pos.findIndex(el => el === elem);

            evt.dataTransfer.dropEffect = 'move';
            evt.dataTransfer.effectAllowed = 'move';
            evt.dataTransfer.setData('col', index);
            evt.dataTransfer.setData('innerRow', idx);
            evt.dataTransfer.setData('hasComeFromOtherColumn', true);

            evt.dataTransfer.setData("text", evt.currentTarget.firstChild);
            evt.dataTransfer.setData("type", evt.currentTarget.firstChild.dataset.type);
        },
        onDrop(evt, idx) {
            const typeDropped = evt.dataTransfer.getData('type');
            const alreadyHasId = this.columnStack[idx].find((item) => item === typeDropped)
            const dataInnerRow = evt.dataTransfer.getData('innerRow');
            const dataColumn = evt.dataTransfer.getData('col');
            const hasComeFromColumn = evt.dataTransfer.getData('hasComeFromOtherColumn');
            const dataType = evt.dataTransfer.getData('type');

            if(!alreadyHasId) {
                if(hasComeFromColumn) {
                    /**
                     * remove from the queue started _REMOVE METHOD
                     */
                    this.columnStack[dataColumn].splice(dataInnerRow,1)
                    /**
                     * insert in queue - INSERT METHOD
                     */
                     if(evt.target.nodeName === 'SPAN') {
                        evt.preventDefault();
                        this.columnStack[idx].push(dataType)
                    }
                    else {
                        evt.preventDefault();
                        this.columnStack[idx].push(dataType);
                    }
                }
                else {
                    if(evt.target.nodeName === 'SPAN') {
                        evt.preventDefault();
                        this.columnStack[idx].push(dataType);
                    }
                    else {
                        evt.preventDefault();
                        this.columnStack[idx].push(dataType);
                    }
                }
            }
        },
        allowDrop(ev) {
            ev.preventDefault();
        },
    }
}

</script>
<style>
figure {
  display: flex;
  margin: 0;
}
p {
    margin: 0;
}
img {
    max-width: 100%;
    width: auto;
    height: auto;
    max-height: 100%;
}
section {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
}
.structure-card, .card-structure {
    height: 100%;
}
.p-t-10 {
    padding-top: 10px;
}
/* span.shape::before {
    content: '';
    height: 40px;
    width: 1px;
    background: red;
    position: absolute;
    top: 0;
    

} */
.add {
    background-color: #f5f5f5;
    width: 35px;
    height: 35px;
    align-items: center;
    justify-content: center;
    display: flex;
    border-radius: 25px;
    border: 1px solid #e1e1e1;
}
.col {
  display: flex;
}
.col-1 {
  width: 100%;
}
.col-2 {
  width: 50%;
}
.col-3 {
  width: 33.33%;
}
.col-4 {
  width: 25%;
}
.border {
    border: 1px solid #e1e1e1;
}
.line {
    height: 40px;
    width: 1px;
    background-color: #ffffff;
    display: inline-block;
}
.al-c {
    justify-content: center;
}
.al-m {
    align-items: center;
}
.shape.s-one {
    background-color: grey;
}
.shape {
  height: 40px;
  width: 70px;
  background-color: #e5e5e5;  
  /* padding: 5px; */
  position: relative;
  display: flex;
    flex-direction: row;
    cursor: pointer;

}
.box {
    display: flex;
    background: #e5e5e5;
    
}
.b-0 {
    flex: 100%;
}
.b-1 {
    flex: 75%;
}
.b-2 {
    flex: 49%;
}
.b-3 {
    flex: 33%;
}
.b-4 {
    flex: 25%
}
.b-5 {
    flex: 20%
}
.b-6 {
    flex: 75%
}
</style>