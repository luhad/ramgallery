<template>
  <v-container fluid grid-list-sm style="margin-bottom: 45px">

    <div style="clear: both"></div>
    <h1 class="text-xs-center nyt" style="font-size: 60px; margin-top: 10px; margin-bottom: 10px">THE RICK AND MORTY GALLERY</h1>
    <div class="loader">
      <span v-if="loading" class="nyt">
        <pulse-loader style="display: inline-block"></pulse-loader>
      </span>
    </div>
    <div style="margin: 0 auto; text-align: center; margin-bottom: 50px">
      <div style="margin-bottom: 40px; margin: 0 auto;">
        <v-select v-bind:items="sections" @change="changeSection" v-model="section" label="Current section" autocomplete style="width: 350px; font-size: 30px; color: #555; margin: 0 auto;"
          class="nyt"></v-select>
        <div style="margin-top: 0px; font-size: 12px; color: #44281d;">What about the reality where Hitler cured cancer? Just don’t think about it, Morty</div>
      </div>
      <div v-if="errors">
        <h1 class="nyt" style="color: red">Error fetching data</h1>
      </div>
    </div>
    <div style="clear: both"></div>
    <v-layout row wrap class="full-height" v-if="!errors">
      <v-flex xs12 sm4 md3 v-for="result in results" :key="result.id">
        <v-layout column>
          <v-flex d-flex>
            <v-card :href="result.url" :target="setTarget()" class="post" style="margin: 1px;" color="darken-3" tile raised>
              <v-card-media :src="result.image" height="200px"></v-card-media>
              <v-card-title primary-title>
                <div style="width:100%;">
                  <h3 class="headline mb-0">
                    <p class="nyt">{{result.name}}</p>
                  </h3>
                  <ul>
                    <li>STATUS: <span>{{result.status}}</span></li>
                    <li>SPECIES: <span>{{result.species}}</span></li>
                    <li>GENDER: <span>{{result.gender}}</span></li>
                    <li>ORIGIN: <span>{{result.origin.name}}</span></li>
                    <li>LAST LOCATION: <span>{{result.location.name}}</span></li>
                  </ul>
                  <!-- <span class="grey--text">LAST LOCATION: {{result.origin.name}}</span> -->
                </div>
              </v-card-title>
            </v-card>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  import PulseLoader from "vue-spinner/src/PulseLoader.vue";
  import axios from "axios";
  import Switches from "vue-switches";

  export default {
    mounted() {
      this.section = "Character";
      this.fetchArticles(this.section);
    },

    methods: {
      setTarget: function () {
        return this.enabled ? "_blank" : "_self";
      },
      changeSection: function (e) {
        this.section = e;
        this.loading = true;
        this.fetchArticles(this.section);
      },
      fetchArticles: function (section) {
        let vm = this;
        section = section.toLowerCase().replace(/\s/g, "");
        console.log(section)
        let url = 'https://rickandmortyapi.com/api/' + section;
        axios
          .get(url)
          .then(function (response) {
            console.log(response);
            vm.loading = false;
            vm.results = response.data.results;
            vm.processPosts();
          })
          .catch(function (error) {
            vm.errors = true;
            console.log(error);
            vm.$forceUpdate();
          });
      },
      processPosts: function () {
        let vm = this;
        let posts = this.results;

        console.log(posts);
        this.results = posts;
      }
    },
    computed: {
      myApiKey: function () {
        return process.env.API_KEY
      }
    },
    components: {
      PulseLoader,
      Switches
    },

    data() {
      return {
        section: "Character",
        loading: true,
        errors: false,
        enabled: true,
        lastUpdated: null,
        results: [],
        sections: [
          "Character",
          "Location",
          "Episode"
        ]
      };
    }
  };
</script>

<style scss>
    .nyt {
        font-family: "Bevan", cursive;
        color:#e4a788;
    }

    .theme--dark.application {
        background: #44281d;
    }

    .full-height .flex {
        display: flex;
    }

    .v-card__title--primary {
        background: #44281d;
    }

    .full-height .flex>.card {
        flex: 1 1 auto;
    }

    .loader {
        position: absolute;
        right: 5px;
        top: 5px;
    }

    .post:hover {
        box-shadow: 0px 0px 150px #000000;
        z-index: 2;
        -webkit-transition: all 200ms ease-in;
        -webkit-transform: scale(1.1);
        -ms-transition: all 200ms ease-in;
        -ms-transform: scale(1.1);
        -moz-transition: all 200ms ease-in;
        -moz-transform: scale(1.1);
        transition: all 200ms ease-in;
        transform: scale(1.1);
        cursor: pointer;
    }

    .input-group--select .input-group__selections__comma {
        font-size: 25px;
        color: #bbb !important;
    }

    .input-group--select.input-group--focused .input-group--select__autocomplete {
        font-size: 25px;
        color: #bbb !important;
    }

    .input-group__input {
        min-height: 50px;
    }

    .fade {
        color: #fff;
    }

    .enabledNotification {
        color: #e4a788;
        font-family: "Lato", sans-serif;
    }

    .v-pulse {
        background-color:#44281d !important;
    }

    label, input, .primary--text {
        color:#44281d !important;
    }

    ul {
        padding:0;
    }
    ul li {
        list-style: none;
        color: rgb(158, 158, 158);
        border-bottom: 1px solid rgb(68, 68, 68);
        padding:5px;
    }
    ul li span {
        color: #e4a788;
    }
</style>