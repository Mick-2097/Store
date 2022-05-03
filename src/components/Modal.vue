<script>
export default {
    name: 'Modal'
}
</script>

<template>
<div v-if="preview" class="overlay">
    <div @keydown.esc="preview = false" class="modal" ref="modal" tabindex="0">
        <img @click="preview = false" class="close" src="/src/assets/close-icon.png" alt=""/>
        <h1>{{previewObject.title}}</h1>
        <div class="previewSpacer">
            <img class="preview-image" :src="previewObject.image" alt=""/>
        </div>
        <p>{{previewObject.description}}</p>
        <div class="divider">
            <h2 :class="{ 'preview-price': purchase }">$ {{previewObject.price.toFixed(2)}}</h2>
            <img v-show="isProductPage" @click="addToCart" class="cart-button" src="/src/assets/cart.png" alt=""/>
        </div>
    </div>
</div>
</template>

<style>

</style>