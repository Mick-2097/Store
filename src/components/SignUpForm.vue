<script>
export default {
    name: 'SignUp'
}
</script>

<template>
<form v-if="isSignUpForm" ref="signUpForm" class="signup-form" action="#" method="#">

    <label for="name">Name</label>
    <input type="text" id="name">

    <label for="email">Email</label>
    <input type="email" id="email">

    <label for="username">Username</label>
    <input type="text" id="username">

    <label for="password">Password</label>
    <input type="text" id="password">

    <label for="confirm">Confirm Password</label>
    <input type="text" id="confirm">

    <label for="image-input">Profile picture</label>
    <input ref="imageInput" id="image-input" class="image-input" type="file" accept="image/*">

    <button @click.prevent="test" class="signup-button">Create account</button>

    <div class="log-in" id="">
        <h3>Already a member?</h3>
        <a @click="signUpLogIn" href="#">Log in</a>
    </div>
</form>
</template>

<style>

</style>