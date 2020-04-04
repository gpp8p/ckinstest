<template>
    <span>
        {{prompt}}<input type="text" :name=name :size=size   @keyup.enter="valueEntered" v-model="value"/>
    </span>
</template>

<script>
  /* eslint-disable no-debugger,no-console */

  export default {
    name: "inputField",
    props: {
      prompt: {
          type: String,
          required: true
      },
      size: {
          type: Number,
          required: true
      },
      value: {
          type: String,
          required: true
      },
      name:{
          type: String,
          required: true
      }


    },
    methods:{
      valueEntered(){
        this.$emit('fieldEntered', [this.name, this.value]);
      },
// eslint-disable-next-line no-debugger
    }
  };
</script>

<style scoped>

</style>