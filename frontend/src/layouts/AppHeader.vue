<template>
  <header class="header">
    <div class="header__logo">
      <router-link :to="{ name: 'home' }" class="logo">
        <img
            src="@/assets/img/logo.svg"
            alt="V!U!E! Pizza logo"
            width="90"
            height="40"
        />
      </router-link>
    </div>
    <div class="header__cart">
      <router-link :to="{ name: 'cart' }">{{ cart.getCartFullPrice}} ₽</router-link>
    </div>
    <div class="header__user">
      <router-link
          v-if="auth.user"
          :to="{ name: 'profile' }"
      >
        <picture>
          <img
              :src="auth.user.avatar"
              :alt="auth.user.name"
              width="32"
              height="32"
          />
        </picture>
        <span>{{ auth.user.name }}</span>
      </router-link>
      <router-link
          v-if="auth.isAuthenticated"
          :to="{ name: 'home' }"
          class="header__logout"
          @click="logout"
      >
        <span>Выйти</span>
      </router-link>
      <router-link
          v-else
          :to="{ name: 'login'}"
          class="header__login"
          >
        Войти
      </router-link>
    </div>
  </header>
</template>

<script setup>
import { useAuthStore } from "@/store/auth";
import { useCartStore } from "@/store";
import { useRouter } from "vue-router";

const auth = useAuthStore()
const cart = useCartStore()
const router = useRouter();

const logout = async () => {
  await auth.logout();
  await router.replace({ name: "login" });
};
</script>

<style lang="scss" scoped>
@import '@/assets/scss/ds-system/ds.scss';
.header {
  position: relative;
  z-index: 2;

  display: flex;

  padding: 0 2.12%;

  background-color: $green-500;
  box-shadow: $shadow-light;
}

.header__logo {
  padding-top: 10px;
  padding-bottom: 10px;
}

.header__cart {
  margin-right: 10px;
  margin-left: auto;

  a {
    @include b-s16-h19;

    display: block;

    padding-top: 21px;
    padding-right: 15px;
    padding-bottom: 21px;
    padding-left: 58px;

    transition: 0.3s;

    color: $white;
    background-color: $green-500;
    background-image: url("../img/cart.svg");
    background-repeat: no-repeat;
    background-position: 20px center;
    background-size: 29px 27px;

    &:hover:not(:active) {
      background-color: $green-400;
    }

    &:active {
      background-color: $green-600;
    }

    &:focus {
      opacity: 0.5;
    }
  }
}

.header__user {
  display: flex;
  align-items: center;

  a {
    display: block;

    padding-top: 14px;
    padding-right: 20px;
    padding-bottom: 14px;
    padding-left: 20px;

    transition: 0.3s;

    background-color: $green-500;

    &:hover:not(:active) {
      background-color: $green-400;
    }

    &:active {
      background-color: $green-600;
    }

    &:focus {
      opacity: 0.5;
    }
  }

  img {
    display: inline-block;

    width: 32px;
    height: 32px;
    margin-right: 8px;

    vertical-align: middle;

    border-radius: 50%;
  }

  span {
    @include r-s14-h16;

    display: inline-block;

    vertical-align: middle;

    color: $white;
  }
}

.header__logout {
  &::before {
    display: inline-block;

    width: 32px;
    height: 32px;
    margin-right: 8px;

    content: '';
    vertical-align: middle;

    background: url(@/assets/img/login.svg) no-repeat center;
    background-size: auto 50%;
  }
}


.header__login {
  &::after {
    display: inline-block;

    width: 32px;
    height: 32px;
    margin-left: 8px;

    content: '';
    vertical-align: middle;

    background: url(@/assets/img/login.svg) no-repeat center;
    background-size: auto 50%;
  }
}
</style>