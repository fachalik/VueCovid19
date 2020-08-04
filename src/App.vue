<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <v-row>
              <v-col>
                <div class="text-h4">COVID 19 TRACKER</div>
              </v-col>
              <v-col cols="2">
                <v-select
                  v-model="Countrie"
                  id="Countrie"
                  :items="Countries"
                  :rules="[v => !!v || 'Item is required']"
                  label="Countries"
                  required
                  @change="getOneCovidCountry(Countrie)"
                  class="ma-0 pa-0"
                ></v-select>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-card class="mx-auto pa-5" max-width="250" raised>
                  <v-list-item three-line>
                    <v-list-item-content>
                      <div class="overline mb-4 text-center">CORONA VIRUS CASES</div>
                      <v-list-item-title class="headline text-center">{{OneCountrieData.TotalConfirmed}}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-col>

              <v-col>
                <v-card class="mx-auto pa-5" max-width="250" raised>
                  <v-list-item three-line>
                    <v-list-item-content>
                      <div class="overline mb-4 text-center">RECOVERED</div>
                      <v-list-item-title class="headline text-center">{{OneCountrieData.TotalRecovered}}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-col>

              <v-col>
                <v-card class="mx-auto pa-5" max-width="250" raised>
                  <v-list-item three-line>
                    <v-list-item-content>
                      <div class="overline mb-4 text-center">DEATHS</div>
                      <v-list-item-title class="headline text-center">{{OneCountrieData.TotalDeaths}}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-col>
            </v-row>
          </v-col>
          <v-col cols="3">
            <v-row>
              <v-col>tes</v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
      <!-- {{OneCountrieData}} -->
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import axios from "axios";
export default {
  name: "App",

  components: {
    HelloWorld,
  },

  data: () => ({
    Countrie: "",
    OneCountrieData: null,
    Countries: ["worldwide"],
    CountriesData: null,
    worlwideData: null,
  }),
  mounted() {
    this.getCountry();
    this.getDataCovidCountry();
  },
  methods: {
    getOneCovidCountry(c) {
      for (let i = 0; i < this.CountriesData.Countries.length; i++) {
        if (c == "worldwide") {
          this.OneCountrieData = this.CountriesData.Global;
        }
        if (this.CountriesData.Countries[i].Country == c) {
          this.OneCountrieData = this.CountriesData.Countries[i];
        }
      }
    },
    getDataCovidCountry() {
      axios.get("https://api.covid19api.com/summary").then((response) => {
        this.CountriesData = response.data;
      });
    },
    getCountry: function () {
      axios.get("https://api.covid19api.com/summary").then((r) => {
        for (let i = 0; i < r.data.Countries.length; i++) {
          this.Countries.push(r.data.Countries[i].Country);
        }
      });
    },
  },
};
</script>
