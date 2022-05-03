<script>
export default {
    name: 'Navbar'
}
</script>

<template>
<nav class="navbar">
    <h1 class="logo" aria-label="Archer tech">Archertech</h1>
    <div v-if="isLoggedIn" class="buttons">
        <button @click="homeButton">Home</button>
        <button @click="productsButton">Products</button>
        <button @click="cartButton" class="cart-menu">Cart</button>
    </div>
    <h2>{{ title }}</h2>
</nav>
</template>

<style>

</style>