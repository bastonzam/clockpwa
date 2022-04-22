<template>
  <v-main>
    <v-container fill-height="fill-height">
      <v-layout align-center="align-center" justify-center="justify-center">
        <v-flex class="login-form text-xs-center">
          <v-card class="mx-auto" max-width="350">
            <v-img
              class="white--text align-end"
              height="200px"
              src="/img/image-800.jpeg"
              lazy-src="/img/image-800-low.jpg"
            >
              <v-card-title><h3 style="2.4rem">Clock Login</h3> </v-card-title>
            </v-img>
            <v-card-text class="mt-5">
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field
                  outlined
                  v-model="username"
                  :rules="userRule"
                  label="รหัสพนักงาน"
                  type="text"
                  required
                  dense
                ></v-text-field>
                <v-text-field
                  outlined
                  v-model="password"
                  :rules="passwordRule"
                  label="รหัสผ่าน"
                  type="password"
                  required
                  dense
                ></v-text-field>
              </v-form>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn block depressed color="primary" @click="doLogin()">
                ล็อกอิน
              </v-btn>
            </v-card-actions>
          </v-card>
          <v-snackbar
            transition="scroll-y-transition"
            v-model="snack"
            timeout="3000"
            :top="true"
            :right="true"
            color="#E15562"
          >
            {{ isErrorText }}

            <template v-slot:action="{ attrs }">
              <v-btn color="white" text v-bind="attrs" @click="snack = false">
                Close
              </v-btn>
            </template>
          </v-snackbar>
        </v-flex>
      </v-layout>
    </v-container>
  </v-main>
</template>
<script>
export default {
  layout: "blank",
  data: () => ({
    snack: false,
    isErrorText: "",
    valid: true,
    username: null,
    password: null,
    userRule: [(v) => !!v || "Staff is required"],
    passwordRule: [(v) => !!v || "Password is required"],
  }),
  methods: {
    doLogin() {
      if (this.$refs.form.validate() == false) return;
      this.$axios
        .post("/kid-backend-api/auth/v1/staff/login", {
          username: this.username,
          password: this.password,
        })
        .then(({ data }) => {
          if (!data.error) {
            let token = `Bearer ${data.result.token}`;
            localStorage.setItem("_clockToken", token);
            this.$router.push("/");
          } else {
            this.isErrorText = data.description;
            this.snack = true;
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    let token = localStorage.getItem("_clockToken");
    if (token) this.$router.push("/");
  },
};
</script>
