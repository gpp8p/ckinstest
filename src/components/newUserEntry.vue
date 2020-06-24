<template>
    <div v-if="adminUserSelect==this.SELECT_USER">
        <span class="inputPrompt">User Name:</span><span><input ref="name" type="text" size="50" v-model="name" @keydown.tab.exact = "nameCheck" required = true /></span>
        <span class="inputPrompt">User Email:</span><span><input ref="email" type="text" size="60" v-model="name" @keydown.tab.exact = "emailCheck" required = true /></span>
        <span class="inputPrompt">User Password:</span><span><input ref="email" type="text" size="60" v-model="name" @keydown.tab.exact = "pwCheck" required = true /></span>
        <span class="inputPrompt">Password Repeat:</span><span><input ref="email" type="text" size="60" v-model="name" @keydown.tab.exact = "pwrCheck" required = true /></span>
    </div>
</template>

<script>
    export default {
        name: "newUserEntry",
        props:{
            adminUserSelect:{
                type:Number,
                required:true
            }
        }
    }
</script>

<style scoped>
    .newUserBody {
        height: 72%;
        margin-left: 10px;
        margin-right: 10px;
        display: grid;
        grid-template-columns: 30% 70%;
        grid-template-rows: 15% 15% 15%
    }

</style>
