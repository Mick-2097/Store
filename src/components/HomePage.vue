<script>
export default {
    name: 'HomePage'
}
</script>

<template>
<div v-if="isLoggedIn" v-show="isHomePage" class="home-page">
    <div class="profile-card">
        <div class="row">
            <img class="profile-picture" :src="user.image" alt="">
            <div class="box">
                <p class="name">{{user.name}}</p>
                <p class="email">{{user.email}}</p>
            </div>
        </div>
        <p class="address-label">Shipping Address</p>
        <p class="address">{{user.shippingAddress}}</p>
    </div>
    <div class="content-card">
        <h1>Content section</h1>
        <p>Maybe account info or something</p>
    </div>
    <div class="recent-card">
        <h1>Recently viewed</h1>
        <ul class="recent-list">
            <li v-for="item in recentArray" class="recent-item">
                <img @click="showDetails" :src="item.image" alt="" class="recent-image">
            </li>
        </ul>
    </div>
</div>
</template>

<style>

</style>