<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row text-center>
          <v-col cols="5">
            <v-select
              v-model="Countrie"
              id="Countrie"
              :items="Countries"
              :rules="[v => !!v || 'Item is required']"
              label="Countries"
              required 
              @change="getOneCovidCountry(Countrie)"
            ></v-select>
          </v-col>
        </v-row>
      </v-container>
      {{OneCountrieData}}
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
    Countrie:"",
    OneCountrieData: null,
    Countries: [],
    CountriesData: null,
  }),
  mounted() {
    this.getCountry();
    this.getDataCovidCountry();
    
  },
  methods: {
    getOneCovidCountry(Country){
      // this.OneCountrieData = Country
      for (let i = 0; i < this.CountriesData.length; i++) {
        if(this.CountriesData[i].Country == Country){
          this.OneCountrieData = this.CountriesData[i]
        }
        
      }
    },
    getDataCovidCountry() {
      axios.get("https://api.covid19api.com/summary").then((response) => {
        this.CountriesData = response.data.Countries;
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
