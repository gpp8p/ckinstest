<template>
        <span class="colorSpan">
                <label class="input-color-label" for="input-color">
                     {{configElement.prompt}}
                </label>

                <span class="input-color-container">
                    <input  id="input-color" type="color" class="input-color" @change="newColor" :value="val"/>
                </span>



        </span>
</template>

<script>
  /* eslint-disable no-console,no-debugger */

  export default {
    name: "flexColor2",
    props: {
      configElement: {
        type: Object,
        required: true
      },

      currentValues:{
        type: Object,
        required: false
      }

    },

    data(){
      return{
        val:this.getCurrentValue()
      }
    },
    watch:{
      currentValues: function(){
          debugger;
        this.val = this.getCurrentValue();
      }
    },

    methods:{
      newColor(evt){
        console.log(evt.target.value);
        this.$emit('configSelected', [this.configElement.element,evt.target.value]);

      },
      getCurrentValue(){
//        debugger;
        if(typeof(this.currentValues[this.configElement.element])=='undefined'){
          return '#FFFFFF';
        }else{
          return this.currentValues[this.configElement.element];
        }
      }

    }
  };
</script>

<style scoped>
        .input-color-container {
                position: relative;
                overflow: hidden;
                width: 30px;
                height: 20px;
                top: 2px;
                margin-right: 5px;
                margin-top: 10px;
                border: solid 2px #ddd;
                border-radius: 40px;
        }

        .input-color {
                position: absolute;
                right: -8px;
                top: -5px;
                width: 36px;
                height: 26px;
                border: none;
        }

        .input-color-label {
                cursor: pointer;
                text-decoration: underline;
                color: #dbd50c;
                margin-right: 30px;
        }
        .colorSpan{
            margin-left: 26px;
        }

</style>