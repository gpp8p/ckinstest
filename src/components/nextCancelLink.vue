<template>
    <span>
        <MyClickLink v-if=showNext @myButtonClicked="nextClicked"  buttonLabel="Next" :highlight="this.isNextHighlighted"></MyClickLink>
        <MyClickLink v-if=showPrev @myButtonClicked="prevClicked" buttonLabel="Previous" :highlight="this.isPrevHighlighted"></MyClickLink>
        <MyClickLink v-if=showSave @myButtonClicked="saveClicked" buttonLabel="Save" :highlight="this.isSaveHighlighted"></MyClickLink>
        <MyClickLink @myButtonClicked="cancelClicked" buttonLabel="Cancel"></MyClickLink>
    </span>
</template>

<script>
  import MyClickLink from "../components/MyClickLink.vue";
  export default {
    name: "nextCancelButtons",
    components: {MyClickLink},
    props: {
      showNext:{
        type: Boolean,
        required: true
      },
      showPrev:{
          type: Boolean,
          required: true
      },
      showSave:{
          type: Boolean,
          required: true
      },
      cmd:{
          type:String,
          required: false
      }
    },
      data() {
          return {
              isSaveHighlighted: false,
              isNextHighlighted: false,
              isPrevHighlighted:false
          }
      },
      watch:{
          cmd:function(){
              switch(this.cmd){
                  case 'highlightSave':
                      this.isSaveHighlighted = true;
                      break;
                  case 'highlightNext':
                      this.isNextHighlighted = true;
                      break;
                  case 'highlightPrevious':
                      this.isPrevHighlighted = true;
                      break;
                  case 'highlightNothing' :
                      this.isPrevHighlighted = false;
                      this.isNextHighlighted = false;
                      this.isPrevHighlighted = false;
                      break;
              }
          }
      },



    methods:{
      nextClicked(){
        this.$emit('buttonClick', ['next'])
      },
      prevClicked(){
        this.$emit('buttonClick', ['previous'])
      },
      cancelClicked(){
        this.$emit('buttonClick', ['cancel'])
      },
      saveClicked(){
         this.$emit('buttonClick', ['save'])
      }
    }

  };
</script>

<style scoped>
    .errorCss {
        text-align: right;
        margin-right: 10px;
        color: red;
    }




</style>