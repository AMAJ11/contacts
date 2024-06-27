<template>
  <v-app>
    <div class="home py-16 px-1">
      <v-container>
        <div
          class="d-flex justify-space-between align-center"
          style="width: 250px"
        >
          <h3 class="text-blue-lighten-2">contacts manager</h3>
          <v-btn color="blue-lighten-4" variant="tonal" to="/add">
            <v-icon color="white">mdi-plus-circle</v-icon> Add</v-btn
          >
        </div>
        <p class="text-h8 mt-1" style="line-height: 15px">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolor, ullam,
          vitae nihil nulla consectetur sapiente rem iste veniam corporis
          corrupti rerum vel culpa commodi velit obcaecati dolore recusandae
          ipsum alias!
        </p>
        <form class="pa-0 mt-5" style="min-width: 300px">
          <v-row>
            <v-col cols="6">
              <v-text-field
                box
                label="search"
                @keyup="this.ret"
                v-model="this.sr"
              ></v-text-field>
            </v-col>
            <v-col class="mt-2">
              <v-btn color="blue" @click="this.search"
                ><v-icon>mdi-magnify</v-icon>
              </v-btn></v-col
            >
          </v-row>
        </form>
        <v-progress-linear
          indeterminate
          size="x-large"
          v-if="loading"
        ></v-progress-linear>
        <h1 class="text-red" v-if="this.error">Erorr: Network Erorr</h1>
        <h2 class="text-red" v-if="this.fd">none item is found</h2>
        <v-row v-if="this.contacts.length > 0">
          <v-col cols="12" lg="6" v-for="num in this.contacts" :key="num">
            <v-card>
              <v-row>
                <v-col cols="3">
                  <img
                    style="width: 130px; height: 130px"
                    :src="num.photo"
                    alt=""
                  />
                </v-col>
                <v-col
                  md="12"
                  cols="12"
                  lg="7"
                  class="bg-grey-darken-3 my-6 py-3"
                >
                  <h3 class="d-inline-block text-blue-lighten-3">
                    Name:&nbsp;&nbsp;
                  </h3>
                  <h4 class="d-inline-block">{{ num.name }}</h4>
                  <br />
                  <h3 class="d-inline-block text-blue-lighten-3">
                    mobile:&nbsp;&nbsp;
                  </h3>
                  <h4 class="d-inline-block">{{ num.mobile }}</h4>
                  <br />
                  <h3 class="d-inline-block text-blue-lighten-3">
                    Email:&nbsp;&nbsp;
                  </h3>
                  <h4 class="d-inline-block">{{ num.email }}</h4>
                </v-col>
                <v-col cols="2" style="flex: 0" class="mr-2">
                  <v-btn
                    class="mt-5"
                    color="blue"
                    size="small"
                    :to="`/ContactsView/${num.id}`"
                    ><v-icon size="x-large">mdi-information</v-icon></v-btn
                  >
                  <v-btn
                    class="mt-1"
                    color="blue"
                    size="small"
                    :to="`/ContactsEdit/${num.id}`"
                    ><v-icon size="x-large">mdi-pencil</v-icon></v-btn
                  >
                  <v-btn
                    class="mt-1"
                    color="blue"
                    size="small"
                    @click="del(num.id)"
                    ><v-icon size="x-large">mdi-delete</v-icon></v-btn
                  >
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>
        <v-dialog
          v-model="di"
          persistent
          :overlay="false"
          width="50%"
          min-width="300px"
          transition="dialog-transition"
        >
          <v-card>
            <v-card-title class="text-primary">
              Are you sure to delete it?
            </v-card-title>
            <v-card-actions>
              <v-btn color="red" @click="dl(this.id)">yes</v-btn>
              <v-btn color="success" @click="this.di = false">cencel</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <h2 v-if="this.st">no data is found</h2>
      </v-container>
    </div>
    <v-footer style="height: 50px">
      <v-row>
        <h2>BY AMAJ&copy;</h2>
        <v-spacer></v-spacer>
        <div>
          <v-icon>mdi-facebook</v-icon>
          <v-icon>mdi-whatsapp</v-icon>
          <v-icon>mdi-instagram</v-icon>
          <v-icon>mdi-github</v-icon>
        </div>
        <v-spacer></v-spacer>
        <h3>apo.zouher@gmail.com</h3>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
// @ is an alias to /src
export default {
  name: "HomeView",
  components: {},
  created: function () {
    if (localStorage.getItem("contact")) {
      this.contacts = JSON.parse(localStorage.getItem("contact"));
    } else {
      localStorage.setItem("contact", JSON.stringify(this.contacts));
    }
    if (localStorage.getItem("conadd")) {
      let contact = JSON.parse(localStorage.getItem("conadd"));
      this.contacts.push(contact);
      localStorage.setItem("contact", JSON.stringify(this.contacts));
      localStorage.removeItem("conadd");
    }
  },
  data: function () {
    return {
      loading: false,
      contacts: [],
      error: false,
      con: [],
      sr: "",
      di: false,
      fd: false,
      id: "",
      st: false,
    };
  },
  methods: {
    del: function (id) {
      this.contacts = JSON.parse(localStorage.getItem("contact"));
      this.id = id;
      this.di = true;
    },
    dl: function () {
      this.di = false;
      for (let i = 0; i < this.contacts.length; i++) {
        if (this.contacts[i].id == this.id) {
          this.contacts.splice(i, 1);
          localStorage.setItem("contact", JSON.stringify(this.contacts));
        }
      }
    },
    search: function () {
      let consh;
      if (this.sr != "") {
        consh = this.contacts.filter((ele) => {
          return ele.name == this.sr;
        });
        this.contacts = consh;
      }
      if (consh.length == 0) {
        this.st = true;
      } else {
      }
    },
    ret: function () {
      this.contacts = JSON.parse(localStorage.getItem("contact"));
      this.st = false;
    },
  },
};
</script>
