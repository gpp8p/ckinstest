<template>
    <div id="drg" class="redClass" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="configComponentHeader">
            <span class="headingText">New Layout</span>
        </div>
        <div class="configComponentBody">
            <br/>
            <input-field :prompt="this.fields[0][this.FPROMPT]"
                         :size="this.fields[0][this.FSIZE]"
                         :value="this.fields[0][this.FVALUE]"
                         :name="this.fields[0][this.FNAME]"
            ></input-field>

            <input-field :prompt="this.fields[1][this.FPROMPT]"
                         :size="this.fields[1][this.FSIZE]"
                         :value="this.fields[1][this.FVALUE]"
                         :name="this.fields[1][this.FNAME]"
            ></input-field>

            <input-field :prompt="this.fields[2][this.FPROMPT]"
                         :size="this.fields[2][this.FSIZE]"
                         :value="this.fields[2][this.FVALUE]"
                         :name="this.fields[2][this.FNAME]"
            ></input-field>

            <input-field :prompt="this.fields[3][this.FPROMPT]"
                         :size="this.fields[3][this.FSIZE]"
                         :value="this.fields[3][this.FVALUE]"
                         :name="this.fields[3][this.FNAME]"
            ></input-field>


        </div>
    </div>
</template>

<script>
    import inputField from '../components/inputField.vue';
    export default {
        name: "newLayout",
        components: { inputField},
        data(){
            return {
                FPROMPT:0,
                FSIZE:1,
                FVALUE:2,
                FNAME:3,
                fields: [
                    ['Layout Name', 30, '', 'layout_name'],
                    ['Layout Description', 50, '', 'layout_description'],
                    ['Columns', 5, '', 'layout_cols'],
                    ['Rows', 5, '', 'layout_rows']
                ]
            }
        }
    }
</script>

<style scoped>
    .redClass {
        height:250px;
        width:500px;
        background-color: #ab97ff;
        border: 2px solid blue;
        border-radius: 8px;
        box-shadow: 10px 10px 5px grey;
    }
    .configComponentHeader{
        height:10%;
        background-color: #fff722;
        border-top-left-radius: 8px;
        border-top-right-radius: 8px;
        text-align: center;
        color: blue;
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;
    }
    .headingText{
        margin-top: 5px;
    }
    .configComponentBody {
        height: 90%;
        margin-left: 10px;
        margin-right: 10px;
    }
</style>