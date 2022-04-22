<template>
  <!-- <v-main> -->
  <v-container fluid :style="{ padding: 0 }" v-if="!loading">
    <v-row justify="center" align="center">
      <v-col cols="12">
        <v-row
          justify="space-around"
          :style="{ backgroundColor: '#f37204', padding: '20px 16px' }"
        >
          <v-col cols="4" md="12">
            <v-avatar class="ma-3" size="125" tile>
              <v-img
                contain
                position="center center"
                :style="{}"
                width="100"
                src="/img/kerry-boy.png"
              ></v-img>
            </v-avatar>
          </v-col>
          <v-col cols="8" md="12">
            <v-card-title
              style="color: white"
              class="text-h6"
              v-text="`${info.titleTh}${info.firstNameTh}  ${info.lastNameTh}`"
            ></v-card-title>

            <v-card-subtitle
              style="color: white"
              v-text="`${info.staffId} : ${info.positionNameTh}`"
            ></v-card-subtitle>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
  <!-- </v-main> -->
</template>

<script>
export default {
  layout: "blank",
  data: () => ({
    info: null,
    loading: true,
  }),

  methods: {},
  mounted() {
    this.loading = true;
    let token = localStorage.getItem("_clockToken");
    if (!token) this.$router.push("/login");
    this.$axios
      .get("/kid-backend-api/auth/v1/staff/info", {
        headers: {
          Authorization: token,
        },
      })
      .then(({ data }) => {
        if (!data.error) {
          this.info = data.result;
          console.log(
            `${this.info.titleTh}${this.info.firstNameTh}  ${this.info.lastNameTh}`
          );
          this.loading = false;
        }
      });
  },
};
</script>
