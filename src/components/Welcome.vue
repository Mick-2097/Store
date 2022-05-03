<script>
export default {
    name: 'Welcome'
}
</script>

<template>
<header v-if="isWelcome" class="welcome">Welcome {{user.name}}!</header>
</template>

<style>

</style>