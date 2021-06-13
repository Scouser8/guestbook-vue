<template>
  <div class="register">
    <div class="formWrapper">
      <h2>Welcome to Guestbook</h2>
      <form @submit.prevent="handleSubmit">
        <label for="">Email *</label>
        <input
          type="email"
          name="email"
          autofocus
          v-model="formData.email"
          @keyup="validate"
          @blur="validate"
        />
        <span v-if="errors.email" class="errors">{{ errors.email }}</span>

        <label for="">Username *</label>
        <input
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

        <div class="name-info-wrapper">
          <div class="name-info">
            <label for="">First Name *</label>
            <input
              type="text"
              name="first_name"
              v-model="formData.first_name"
              @keydown="validate"
              @blur="validate"
            />
            <span v-if="errors.first_name" class="errors">{{
              errors.first_name
            }}</span>
          </div>

          <div class="name-info">
            <label for="">Last Name</label>
            <input type="text" name="last_name" v-model="formData.last_name" />
          </div>
        </div>

        <button>Create new account</button>

        <div class="to-login">
          <router-link :to="{ name: 'Login' }"
            >Already have an account? Go to Sign In.</router-link
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
        email: "",
        user_name: "",
        password: "",
        first_name: "",
        last_name: "",
      },
      errors: {
        email: "",
        user_name: "",
        password: "",
        first_name: "",
        last_name: "",
      },
    };
  },
  methods: {
    validate(e) {
      const field = e.target;
      const regexEmail = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/;
      const regexPassword = /^(?!.* )(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!/@#$%^&*])/;
      if (
        field.name === "email" &&
        field.value &&
        !field.value?.match(regexEmail)
      ) {
        this.errors[field.name] = "Please enter a valid email";
      } else if (
        field.name === "password" &&
        field.value &&
        !field.value?.match(regexPassword)
      ) {
        this.errors[field.name] =
          "Must have no Spaces, contain 8 Characters, One Uppercase, One Lowercase, One Number & One Special Character";
      } else if (
        field.name === "user_name" &&
        field.value &&
        field.value.length < 8
      ) {
        this.errors[field.name] = "Username should be at least 8 characters";
      } else if (!field.value)
        this.errors[field.name] = "This field is required";
      else if (field.value) this.errors[field.name] = "";
    },

    handleSubmit() {
      let errors = false;
      const regexEmail = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/;
      const regexPassword = /^(?!.* )(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!/@#$%^&*])/;
      Object.entries(this.formData).forEach(([key, value]) => {
        if (key === "email" && value && !value?.match(regexEmail)) {
          this.errors[key] = "Please enter a valid email";
          errors = true;
        } else if (
          key === "password" &&
          value &&
          !value?.match(regexPassword)
        ) {
          this.errors[key] =
            "Must have no Spaces, contain 8 Characters, One Uppercase, One Lowercase, One Number & One Special Character";
          errors = true;
        } else if (key === "user_name" && value && value.length < 8) {
          this.errors[key] = "Username should be at least 8 characters";
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
        .post("user/register", this.formData)
        .then((res) => {
          alert(res.data);
          if (res.data === "New user created successfully") {
            this.$router.push({ name: "Login" });
          }
        })
        .catch((res) => alert(res.error));
    },
  },
};
</script>

<style>
.register {
  /* width: 100%; */
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #2c3e50;
}

.formWrapper {
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

.to-login {
  width: fit-content;
  margin: 0 auto;
}
</style>
