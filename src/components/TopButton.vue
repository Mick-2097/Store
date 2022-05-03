<script>
export default {
    name: 'TopButton'
}
</script>

<template>
<button v-if="isFarFromHome" @click="scrollToTop" class="top-button">Top</button>
</template>

<style>

</style>