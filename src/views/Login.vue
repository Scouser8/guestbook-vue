<template>
  <div class="login">
    <div class="formWrapper">
      <h2>Guestbook</h2>
      <form @submit.prevent="handleSubmit">
        <label for="">Username *</label>
        <input
          autofocus
          type="text"
          name="user_name"
          v-model="formData.user_name"
          @keydown="validate"
          @blur="validate"
        />
        <span v-if="errors.user_name" class="errors">{{
          errors.user_name
        }}</span>

        <label for="">Password *</label>
        <input
          type="password"
          name="password"
          v-model="formData.password"
          @keyup="validate"
          @blur="validate"
        />
        <span v-if="errors.password" class="errors">{{ errors.password }}</span>

        <button>Login</button>

        <div class="to-register">
            <span>Not a user? </span>
          <router-link :to="{ name: 'Register' }"
            >Create a new account.</router-link
          >
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "../axios";
export default {
  name: "Register",
  data() {
    return {
      formData: {
        user_name: "",
        password: "",
      },
      errors: {
        user_name: "",
        password: "",
      },
    };
  },
  methods: {
    validate(e) {
      const field = e.target;
      const regexPassword = /^(?!.* )(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!/@#$%^&*])/;
      if (
        field.name === "password" &&
        field.value &&
        !field.value?.match(regexPassword)
      ) {
        this.errors[field.name] =
          "Must have no Spaces, contain 8 Characters, One Uppercase, One Lowercase, One Number & One Special Character";
      } else if (!field.value)
        this.errors[field.name] = "This field is required";
      else if (field.value) this.errors[field.name] = "";
    },

    handleSubmit() {
      let errors = false;
      const regexPassword = /^(?!.* )(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!/@#$%^&*])/;
      Object.entries(this.formData).forEach(([key, value]) => {
        if (key === "password" && value && !value?.match(regexPassword)) {
          this.errors[key] =
            "Must have no Spaces, contain 8 Characters, One Uppercase, One Lowercase, One Number & One Special Character";
          errors = true;
        } else if (!value && key !== "last_name") {
          this.errors[key] = "This field is required";
          errors = true;
        }
      });

      if (errors) {
        return;
      }

      axios
        .post("user/login", this.formData)
        .then((res) => {
          this.$store.state.user = res.data;
          this.$router.push({ name: "Home" });
        })
        .catch((res) => alert(res.error));
    },
  },
};
</script>

<style>
.login {
  /* width: 100%; */
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #2c3e50;
}

.formWrapper {
  width: 400px;
  padding: 25px;
  color: #191919;
  background-color: snow;
  border-radius: 12px;
}

.formWrapper h2 {
  color: #2c3e50;
  width: fit-content;
  margin: 10px auto;
  font-style: italic;
}

.formWrapper .errors {
  color: #fe5442;
  display: block;
  margin-top: 10px;
  max-width: 300px;
}

.formWrapper label {
  display: inline-block;
  margin: 25px 0 5px;
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
}
.formWrapper input {
  display: block;
  padding: 10px 3px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  outline: none;
  background-color: snow;
}

.name-info-wrapper {
  display: flex;
  justify-content: space-between;
}

.name-info {
  width: 45%;
}

.formWrapper button {
  display: block;
  width: 220px;
  margin: 40px auto;
  padding: 10px 15px;
  font-size: 16px;
  color: snow;
  background-color: goldenrod;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.formWrapper button:hover {
  background-color: darkgoldenrod;
}

.to-register {
  width: fit-content;
  margin: 0 auto;
}
</style>
