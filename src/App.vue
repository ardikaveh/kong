<template>
  <header>
    <div class="nav">
      <router-link to="/">
        <img
          alt="konnect home"
          src="konnect.png"
        >
      </router-link>
      <router-link to="/">
        <KIcon
          class="profile"
          icon="profile"
          size="32"
        />
      </router-link>
      <router-link to="/">
        <KIcon
          class="help"
          icon="help"
          size="20"
        />
      </router-link>
      <router-link to="/">
        <KIcon
          class="notification"
          icon="notificationBell"
          size="20"
        />
      </router-link>
    </div>
  </header>
  <main>
    <RouterView />
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { RouterView, RouterLink } from 'vue-router'
import { KIcon } from '@kong/kongponents'

export default defineComponent({
  name: 'App',
  components: { RouterView, RouterLink, KIcon },
})
</script>

<style lang="scss">
html {
  margin: 0;
  padding: 0;
  font-size: 10px;
}

body {
  font-family: Arial, sans-serif;
  font-weight: 400;
  font-size: 1.6rem;
  line-height: 1.2;
  margin: 0 auto;
}

.nav {
  padding: 16px;
  margin-bottom: 4rem;
  border-bottom: 1px solid var(--grey-300);
  .kong-icon {
    float:right;
    margin: 0 10px;
  }
  .kong-icon-profile {
    margin-top: -5px;
  }
  a {
    font-weight: 700;
    color: blue;

    &.router-link-exact-active {
      color: green;
    }
  }

}
</style>
