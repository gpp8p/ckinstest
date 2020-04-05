<template>
     <a href="#" v-on:click="buttonClicked" class="myButton">{{ buttonLabel }}</a>
</template>

<script>
export default {
  name: "MyButton",
  props: {
    buttonLabel: {
      type: String,
      required: true
    }
  },
  methods: {
    buttonClicked: function() {
      this.$emit('myButtonClicked', [this.buttonLabel])
    }
  }
};
</script>

<style scoped>

.myButton {
  box-shadow: 0px 1px 0px 0px #f0f7fa;
  background: linear-gradient(to bottom, #33bdef 5%, #019ad2 100%);
  background-color: #33bdef;
  border-radius: 3px;
  border: 1px solid #057fd0;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  font-family: Arial;
  font-size: 12px;
  font-weight: bold;
  padding: 2px 12px;
    margin: 10px;
  text-decoration: none;
  text-shadow: 0px -1px 0px #5b6178;
}
.myButton:hover {
  background: linear-gradient(to bottom, #019ad2 5%, #33bdef 100%);
  background-color: #019ad2;
}
.myButton:active {
  position: relative;
  top: 1px;
}
</style>
