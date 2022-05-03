<script>
export default {
    name: 'LogIn'
}
</script>

<template>
<form v-if="isLogInForm" ref="logInForm" class="login-form" action="#" method="#">

    <label for="username" class="user-label">Username</label>
    <input v-model="username" class="user-input" id="username" placeholder="username" maxlength="16">

    <label for="password">Password</label>
    <input v-model="password" :type="showPassword ? 'text' : 'password'" class="password-input" id="password" placeholder="password" maxlength="16">

    <label @click="showPassword = !showPassword" class="show-hide" for="checkbox">
        {{showPassword ? 'HIDE' : 'SHOW'}}
    </label>
    <input type="checkbox" class="checkbox" name="show-hide" id="checkbox">

    <button @click.prevent="loginAttempt" class="login-button">Log in</button>

    <div class="sign-up">
        <h3>Not a member?</h3>
        <a @click="signUpLogIn" href="#">Sign up</a>
    </div>
</form>
</template>

<style>

</style>