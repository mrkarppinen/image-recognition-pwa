<template>
<img v-bind:src="src" />
</template>

<script>
export default {
    name: 'imageDisplay',
    props: ['src']
}
</script>


<style scoped>
img {
    width: 500px;
    max-width: 90%;
}
</style>