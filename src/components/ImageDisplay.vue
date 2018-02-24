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
    max-width: 50%;
}

@media (min-width: 768px) {
    img {
        width: 500px;
        max-width: 90%;
    }

}
</style>