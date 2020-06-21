<template>

</template>

<script>
    export default {
        name: "orgUserListLine",
        props:{
            id: {
                type: String,
                required: true
            },
            name: {
                type: String,
                required: true
            },
            email: {
                type: String,
                required: true
            }
        }
    }
</script>

<style scoped>

</style>
