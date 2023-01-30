<template>
    <h2>Te quedan {{lifes}} vidas</h2>
</template>
<script>
export default {
    props:{
        lifes:{
            type: Number,
            required: true
        }
    }
}
</script>
<style>

</style>