<template>
  <div class="header">
    <router-link :to="{ name: 'Home' }">
      <span class="logo">Guestbook</span>
    </router-link>
    <div class="nav-links">
      <router-link :to="{ name: 'Home' }">
        <p>Home</p>
      </router-link>
      <router-link :to="{ name: 'Profile' }">
        <p>Profile</p>
      </router-link>
      <router-link :to="{ name: 'Aboutus' }">
        <p>About</p>
      </router-link>
    </div>
    <p class="welcome-msg">Welcome, {{ $store.state.user?.first_name }}</p>
    <button class="logout-btn" @click="logout">Logout</button>
  </div>
</template>

<script>
export default {
  methods: {
    logout() {
      this.$store.state.user = null;
      this.$router.push({ name: "Login" });
    },
  },
};
</script>

<style>
.header {
  position: sticky;
  position: -webkit-sticky; /* Safari */
  top: 0;
  height: 75px;
  padding: 15px 50px;
  background-color: #2c3e50;
  display: flex;
  align-items: center;
}

.logo {
  font-size: 28px;
  font-style: italic;
  font-weight: bold;
  color: goldenrod;
}

.nav-links {
  margin-left: 30px;
  display: flex;
}

.nav-links p {
  margin-left: 25px;
  padding: 5px 10px;
  border-bottom: 1px solid transparent;
  color: snow;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-weight: bold;
  cursor: pointer;
  width: fit-content;
}

.nav-links p:hover {
  border-color: snow;
}

.active-tab {
  border-color: snow;
}

.welcome-msg {
  margin-left: auto;
  color: snow;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
}

.logout-btn {
  background-color: transparent;
  border: 2px solid snow;
  color: snow;
  padding: 10px 15px;
  border-radius: 12px;
  cursor: pointer;
  margin-left: 10px;
  font-weight: bold;
}

.logout-btn:hover {
  color: #2c3e50;
  background-color: snow;
}
</style>
