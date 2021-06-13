<template>
  <div class="header">
    <span class="logo">Guestbook</span>
    <p class="welcome-msg">Welcome, {{ $store.state.user?.user_name }}</p>
    <router-link to="/">
      <button class="logout-btn">Logout</button>
    </router-link>
  </div>
</template>

<script>
export default {};
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

.logout-btn:hover{
  color: #2c3e50;
  background-color: snow;
}
</style>
