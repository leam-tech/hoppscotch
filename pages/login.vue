<template>
  <div class="page">
    <div class="content">
      <tabs>
        <tab id="login-tab" label="Login" :selected="true">
          <form ref="login_form" @submit.prevent="login">
            <label for="email">Email</label>
            <input id="email" type="email" v-model="email" required />

            <label for="pwd">Password</label>
            <input id="pwd" type="password" v-model="pwd" required />

            <button type="submit">Login</button>
          </form>
        </tab>
        <tab id="signup-tab" label="Signup">
          <form ref="signup_form" @submit.prevent="signup">
            <label for="full_name">Name</label>
            <input id="full_name" v-model="signup_full_name" required />

            <label for="email">Email</label>
            <input id="email" type="email" v-model="signup_email" required />

            <label for="pwd">Password</label>
            <input id="pwd" type="password" v-model="signup_pwd" required />

            <label for="pwd">Confirm Password</label>
            <input id="pwd" type="password" v-model="confirm_pwd" required />

            <button type="submit">Signup</button>
          </form>
        </tab>
      </tabs>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.page .content {
  display: flex;
  justify-content: center;
}

form {
  padding: 1em;
  button {
    margin-top: 1em;
  }
}
</style>

<script>
import firebase from "firebase/app"
import { fb } from "~/helpers/fb"

export default {
  data() {
    return {
      email: "",
      pwd: "",

      signup_full_name: "",
      signup_email: "",
      signup_pwd: "",
      confirm_pwd: "",
    }
  },
  methods: {
    login() {
      console.log("LOGIN")
      if (!this.email) {
        this.$toast.error("Email is mandatory")
        return
      } else if (!this.pwd) {
        this.$$toast.error("Password is mandatory")
        return
      }
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.pwd)
        .then((r) => {
          this.$router.push({
            path: "/",
          })
        })
        .catch((e) => console.log("ERROR", e))
    },
    signup() {
      if (!this.signup_email) {
        this.$toast.error("Email is mandatory")
        return
      } else if (!this.signup_full_name) {
        this.$$toast.error("Full name is mandatory")
        return
      } else if (!this.signup_pwd) {
        this.$$toast.error("Password is mandatory")
        return
      } else if (this.signup_pwd !== this.confirm_pwd) {
        this.$toast.error("Passwords do not match")
        return
      }

      firebase
        .auth()
        .createUserWithEmailAndPassword(this.signup_email, this.signup_pwd)
        .then((r) => {
          this.$router.push({
            path: "/",
          })
        })
        .catch((e) => console.log("ERROR", e))
    },
  },
}
</script>
