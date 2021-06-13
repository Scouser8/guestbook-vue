<template>
  <Header />
  <div>
    <div class="user-data">
      <span class="attribute-name">Username:</span>
      <span>{{ $store.state.user?.user_name }}</span>
    </div>
    <div class="user-data">
      <span class="attribute-name">Email:</span>
      <span>{{ $store.state.user?.email }}</span>
    </div>
    <div class="user-data">
      <span class="attribute-name">First Name:</span>
      <span>{{ $store.state.user?.first_name }}</span>
    </div>
    <div class="user-data">
      <span class="attribute-name">Last Name:</span>
      <span>{{ $store.state.user?.last_name }}</span>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";

export default {
  components: {
    Header,
  },
  created() {
    if (!this.$store.state.user) {
      this.$router.push({ name: "Login" });
    }
  },
};
</script>

<style>
.user-data {
  display: flex;
  padding: 20px 30px;
}
.user-data span {
  /* width: 30px; */
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  margin-right: 10px;
}

.attribute-name {
  font-weight: bold;
}
</style>
