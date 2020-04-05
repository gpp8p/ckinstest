<template>
    <span>
        {{this.configElement.prompt}} <input type="file" id="file" ref="file" v-on:change="handleFileUpload()"/>
        <MyButton @myButtonClicked="submitFile" buttonLabel="Save ?"></MyButton>
    </span>
</template>

<script>
  /* eslint-disable no-console */

  import MyButton from "../components/MyButton.vue";
  import axios from 'axios';
  export default {
    name: "flexFileUpload",
    components: {MyButton},
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
      return {
        file: '',
        user: 'dev',
        returnedData:''
      }
    },
    methods: {
      submitFile(){
      let formData = new FormData();
      formData.append('file', this.file);
      formData.append('usr', this.user);
// eslint-disable-next-line no-debugger
//        debugger;

      axios.post( 'http://localhost:8000/imageUpload?XDEBUG_SESSION_START=15617',
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }
        ).then(response=>{
// eslint-disable-next-line no-console
          this.returnedData = response.data;
          this.$emit('configSelected', [this.configElement.element, this.returnedData]);
          console.log('SUCCESS!!'+response.data);
        }).catch(function(error){
//            debugger;
          console.log('FAILURE!!'+error);
        });
      },

      /*
        Handles a change on the file upload
      */
      handleFileUpload(){
        this.file = this.$refs.file.files[0];
      }
    }

  };
</script>

<style scoped>

</style>