<template>
  <div
    class="l-select lg:h-full lg:w-full md:w-full sm:w-full md:h-full sm:h-full"
  >
    <div class="close">
      <i class="fa-solid fa-x" @click="$emit('closeWithoutData')"></i>
    </div>
    <div class="select">
      <input
        type="text"
        placeholder="Search for a city"
        class="city-input"
        v-model="searchText"
        @input="filterCities"
      />
      <ul class="cities-list">
        <li
          v-for="(city, index) in filteredCities"
          :key="'city' + index"
          @click="setState(city)"
          v-show="toggleSelect "
        >
          {{ city }}
        </li>
      </ul>
      <div v-if="choosenCity != ''" class="text-white text-3xl">Choosen City: {{choosenCity}} <i class="fa-solid fa-check check-icon "></i></div>
      <button class="btn" @click="getData">See Result</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import countries from "../countries.json";
export default {
  name: "SelectCountry",
  emits: ["close", "clicked", "closeWithoutData"],
  data() {
    return {
      cities: [],
      filteredCities: [],
      searchText: "",
      allData: null,
      choosenCity: "",
      toggleSelect: null,
    };
  },
  methods: {
    async getData() {
      if (this.choosenCity != "") {
        try {
          const res = await axios.get(
            `https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/${this.choosenCity}?include=fcst%2Cobs%2Chistfcst%2Cstats%2Cdays%2Ccurrent&key=UH84B5CKUWE4E5ZKBE346H34S&options=beta&contentType=json
&iconSet=icons2`
          );
          this.allData = res.data;
        } catch (e) {
          console.error(e);
        }
        this.$emit("close", this.allData);
        this.searchText = "";
      } else {
        try {
          const res = await axios.get(
            `https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/cairo?include=fcst%2Cobs%2Chistfcst%2Cstats%2Cdays%2Ccurrent&key=UH84B5CKUWE4E5ZKBE346H34S&options=beta&contentType=json
&iconSet=icons2`
          );
          this.allData = res.data;
        } catch (e) {
          console.error(e);
        }
        this.$emit("close", this.allData);
      }
    },
    filterCities() {
      if (this.searchText.length > 0) {
        this.cities = countries.map((city) => {
          return city.capital;
        });
        this.filteredCities = this.cities.filter((e) => {
          return e.toLowerCase().startsWith(this.searchText.toLowerCase());
        });
        this.toggleSelect = true;
      } else {
        this.toggleSelect = false;
        this.choosenCity = "";
      }
    },
    setState(e) {
      this.searchText = '';
      this.choosenCity = e;
      this.toggleSelect = false;
    },
  },

  mounted() {
    this.filterCities();
  },
  watch: {
    searchText(newValue, oldValue) {
      if (newValue != "") {
        this.filterCities();
      }
    },
  },
};
</script>

<style scoped>
.l-select {
  height: 75vh;
}
.close {
  width: 100%;
  height: 10%;
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  padding: 30px;
  box-sizing: border-box;
  align-items: center;
}

i {
  font-size: 18px;
  color: white;
  width: 30px;
  height: 30px;
  cursor: pointer;
  text-align: center;
  line-height: 30px;
  transition: 0.5s;
}

i:hover {
  background: #100e1d;
}

.select {
  width: 100%;
  height: 90%;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  padding: 30px;
  box-sizing: border-box;
  position: relative;
}

.arrow {
  position: absolute;
  right: 35px;
  top: 40px;
}

.btn {
  width: 70%;
  height: 10%;
  font-size: 24px;
  font-family: "Raleway", sans-serif;
  background-color: #1e213a;
  border: 1px solid #747f9d;
  border-radius: 5px;
  color: white;
  transition: 0.3s;
  cursor: pointer;
  margin-top: 20px;
}

.btn:hover {
  background: #100e1d;
}

.city-input {
  width: 100%;
  height: 10%;
  background-color: #1e213a;
  border: none;
  outline: none;
  font-size: 20px;
  color: white;
  transition: 0.3s;
  padding: 0 10px;
  border-radius: 10px;
  box-sizing: border-box;
  border-bottom: 3px solid #100e1d;
  border-top: 3px solid #100e1d;
}
.city-input:focus {
  background-color: #100e1d;
  border-bottom: 3px solid white;
}

.cities-list {
  width: 100%;
  height: 70%;
  padding: 0;
  list-style: none;
  color: white;
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-direction: column;
  overflow-y: auto;
  margin: 0;
}
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-track {
  background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #747f9d;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #5a6278;
}

.cities-list li {
  width: calc(100% - 3px);
  background-color: #747f9d;
  font-size: 20px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding-left: 20px;
  margin-bottom: 4px;
  border-radius: 10px;
  transition: 0.3s;
  border-bottom: 3px solid #100e1d;
  border-top: 3px solid #100e1d;
}

.cities-list li:hover {
  background-color: #1e213a;
}

.check-icon {
  color: green;
  font-size: 2rem;
}
</style>
