<template>
  <Header />
  <div class="about-wrapper">
    <h1>About us:</h1>
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Eos corrupti
      doloribus expedita, iure error nemo perspiciatis ducimus nostrum tempora.
      Quo, magni necessitatibus. Iste a excepturi eius modi exercitationem dolor
      aperiam fuga fugit voluptas dolores obcaecati sit, beatae maxime sapiente,
      quo, eos quibusdam iure. Temporibus magnam nesciunt deserunt itaque quos
      incidunt.
    </p>
    <p>
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. Voluptates vero
      ipsum quo fugit blanditiis architecto, mollitia recusandae velit? Nihil,
      quibusdam illo? Impedit quos aut, quas saepe modi rem sequi aliquid
      doloremque accusamus necessitatibus dolorum similique adipisci totam
      aperiam rerum ducimus quidem. Non, quam sequi maiores porro quis
      reprehenderit dolore hic fugit eveniet facere numquam eum, aperiam eos
      voluptatibus. Esse fugiat dolor, rem, harum ad dolores quam nisi assumenda
      ipsam excepturi tempore hic blanditiis dicta facere officiis natus impedit
      explicabo vel a suscipit quae mollitia. Repellendus accusamus eaque
      repudiandae nesciunt corporis assumenda odit accusantium consequatur
      error, quibusdam dolores, eius tempore temporibus!
    </p>
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
.about-wrapper {
  padding: 20px 30px;
}
</style>
