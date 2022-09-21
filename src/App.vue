<template>
  <div class="lds-dual-ring" v-if="allData === null"></div>
    <div
      class="container lg:flex lg:flex-row lg:h-4/5 md:flex-col sm:flex-col md:w-full"
      v-if="allData"
    >
      <LeftSide>
        <div v-show="toggleSelect === false">
          <SearchBtn v-show="toggleSelect === false" @toggle="toggleSelects" />
          <div class="icon">
            <div class="icon-background"></div>
            <WeatherIcon :icon="icon" />
          </div>
          <div class="temp">
            <div class="temp-degree">
              <span v-if="degreeType === 'C'" class="text-red-600">{{
                degConvert(allData.currentConditions.temp)
              }}</span>
              <span v-if="degreeType === 'F'">{{
                allData.currentConditions.temp
              }}</span
              ><span v-if="degreeType === 'C'">°C</span>
              <span v-if="degreeType === 'F'">°F</span>
            </div>
            <div class="temp-state">
              {{ allData.currentConditions.conditions }}
            </div>
          </div>
          <div class="date">
            <span>Today</span><span>.</span
            ><span>{{
              dateConvert(allData.currentConditions.datetimeEpoch)
            }}</span>
          </div>
          <div class="location">
            <i class="fa-solid fa-location-dot"></i
            ><span>{{ allData.address }}</span>
          </div>
        </div>
        <SelectCountry
          @close="closeState"
          v-show="toggleSelect === true"
          @closeWithoutData="closeWithoutData"
        />
      </LeftSide>
  
      <!-- Start of right side                                                                                                    -->
      <RightSide>
        <!-- Buttons to convert degree type -->
        <div class="degree-type">
          <div class="temp-c">
            <button @click="convertToC" class="btn-active">°C</button>
          </div>
          <div class="temp-f">
            <button @click="convertToF">°F</button>
          </div>
        </div>
        <!--  -->
  
        <!-- 5 Days Forecast -->
        <div class="forecast">
          <ForeCastDay :number="1" :allData="allData" :degreeType="degreeType" />
          <ForeCastDay :number="2" :allData="allData" :degreeType="degreeType" />
          <ForeCastDay :number="3" :allData="allData" :degreeType="degreeType" />
          <ForeCastDay :number="4" :allData="allData" :degreeType="degreeType" />
          <ForeCastDay :number="5" :allData="allData" :degreeType="degreeType" />
        </div>
        <!--  -->
  
        <div class="highlight">Today's Highlights</div>
        <div class="temp-details sm:w-full sm:h-3/5">
          <BigCard
            title="Wind Speed"
            :data="allData.currentConditions.windspeed"
            type="windspeed"
          />
  
          <BigCard
            title="Humidity"
            :data="allData.currentConditions.humidity"
            type="humidity"
          />
  
          <SmallCard
            title="Visibility"
            :data="allData.currentConditions.visibility"
            type="visibility"
          />
  
          <SmallCard
            title="Air Pressure"
            type="pressure"
            :data="allData.currentConditions.pressure"
          />
        </div>
  
        <div class="username">
          Created By <b>Ahmed Sultan</b> - devChallenges.io
        </div>
      </RightSide>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  import WeatherIcon from "./components/WeatherIcon.vue";
  import ForeCastDay from "./components/ForeCastDay.vue";
  import BigCard from "./Layout/BigCard.vue";
  import SmallCard from "./Layout/SmallCard.vue";
  import SearchBtn from "./components/SearchBtn.vue";
  import SelectCountry from "./components/SelectCountry.vue";
  import LeftSide from "./Layout/LeftSide.vue";
  import RightSide from "./Layout/RightSide.vue";
  import VueGeolocation from 'vue-browser-geolocation';
  
  export default {
    name: "app",
    components: {
      WeatherIcon,
      SelectCountry,
      ForeCastDay,
      BigCard,
      SmallCard,
      SearchBtn,
      LeftSide,
      RightSide,
    },
    data() {
      return {
        allData: null,
        city: "Cairo",
        toggleSelect: false,
        degreeType: "C",
        currentCondition: "",
        icon: "",
        fIcon: "",
        newData: [],
      };
    },
    async created() {
      try {
        const res = await axios.get(
          `https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/${this.city}?include=fcst%2Cobs%2Chistfcst%2Cstats%2Cdays%2Ccurrent&key=UH84B5CKUWE4E5ZKBE346H34S&options=beta&contentType=json
  &iconSet=icons2`
        );
        this.allData = res.data;
        this.icon = res.data.currentConditions.icon;
      } catch (e) {
        console.error(e);
      }
    },
    methods: {
      degConvert(f) {
        let c;
        c = ((f - 32) * 5) / 9;
        return Math.ceil(c);
      },
      dateConvert(ePosh) {
        let myDate = new Date(ePosh * 1000).toDateString().split(" ");
        return `${myDate[0]}, ${myDate[2]} ${myDate[1]}`;
      },
      closeState(values) {
        this.toggleSelect = false;
        this.allData = values;
      },
      closeWithoutData() {
        this.toggleSelect = false;
      },
      toggleSelects() {
        this.toggleSelect = true;
      },
      convertToF(e) {
        this.degreeType = "F";
        e.currentTarget.classList.add("btn-active");
        let cBtn = document.querySelector(".temp-c button");
        cBtn.classList.remove("btn-active");
      },
      convertToC(e) {
        this.degreeType = "C";
        e.currentTarget.classList.add("btn-active");
        let fBtn = document.querySelector(".temp-f button");
        fBtn.classList.remove("btn-active");
      },
    },
  };
  </script>
  
  <style scoped>
  body {
    margin: 0;
    padding: 0;
    font-family: "Raleway", sans-serif;
  }
  
  .container {
    display: flex;
    box-shadow: -2px 8px 50px -5px rgba(0, 0, 0, 0.52);
    -webkit-box-shadow: -2px 8px 50px -5px rgba(0, 0, 0, 0.52);
    -moz-box-shadow: -2px 8px 50px -5px rgba(0, 0, 0, 0.52);
  }
  /* Left side information */
  
  .icon {
    width: 100%;
    height: 35%;
    box-sizing: border-box;
    position: relative;
    margin: 0;
    padding: 0;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .icon img {
    width: 220px;
    height: 220px;
  }
  
  .temp {
    width: 100%;
    height: 35%;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    align-content: center;
    padding: 30px;
  }
  
  .temp-degree {
    width: 100%;
    height: 60%;
    display: flex;
    flex-direction: row;
    align-content: center;
    align-items: center;
    justify-content: center;
  }
  
  .temp-degree span:nth-of-type(2) {
    font-size: 48px;
    color: #a09fb1;
    height: 100%;
    display: flex;
    align-items: flex-end;
  }
  .temp-degree span:nth-of-type(1) {
    text-align: center;
    font-size: 110px;
    color: white;
  }
  
  .temp-state {
    width: 100%;
    height: 40%;
    text-align: center;
    color: #a09fb1;
    font-size: 24px;
    margin-top: 40px;
  }
  
  .date {
    width: 100%;
    height: 7.5%;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    gap: 10px;
  }
  
  .location {
    width: 100%;
    height: 7.5%;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }
  
  .location span {
    text-transform: capitalize;
  }
  
  .icon-background {
    position: absolute;
    content: "e";
    background: url("../src/assets/weather-icons/Cloud-background.png") no-repeat;
    background-size: cover;
    left: -60px;
    width: 120%;
    height: 110%;
    opacity: 0.05;
    margin: 0;
  }
  
  .date span {
    color: #88869d;
  }
  
  .location {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 5px;
    color: #88869d;
  }
  /* End of left side information */
  
  /* Start of left side country selection */
  
  .search-bar {
    width: 100%;
    height: 20%;
    border: 3px solid white;
    box-sizing: border-box;
  }
  
  .options-select {
    width: 100%;
    height: 80%;
    border: 3px solid white;
    box-sizing: border-box;
  }
  
  /* end of left side country selection */
  
  .degree-type {
    width: 100%;
    height: 15%;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    justify-content: flex-end;
    align-items: center;
    gap: 20px;
    padding: 30px 70px;
  }
  
  .degree-type button {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    font-size: 18px;
    margin: 0;
    border: none;
    outline: none;
    background-color: #585676;
    color: white;
    font-weight: 600;
    cursor: pointer;
  }
  
  .degree-type .btn-active {
    background-color: white;
    color: #100e1d;
  }
  
  .forecast {
    width: 100%;
    min-height: 30%;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    gap: 20px;
    padding: 0 60px;
  }
  
  .highlight {
    width: 100%;
    height: 5%;
    box-sizing: border-box;
    color: white;
    padding: 40px 60px;
    font-size: 24px;
    text-align: center;
  }
  
  .temp-details {
    width: 100%;
    height: 45%;
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
    padding: 0 60px;
    gap: 20px;
    margin: 40px 0;
  }
  
  .username {
    width: 100%;
    height: 5%;
    box-sizing: border-box;
    color: white;
    text-align: center;
    padding: 20px 0;
  }
  .lds-dual-ring {
    display: inline-block;
    width: 80px;
    height: 80px;
  }
  .lds-dual-ring:after {
    content: " ";
    display: block;
    width: 64px;
    height: 64px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid #fff;
    border-color: #fff transparent #fff transparent;
    animation: lds-dual-ring 1.2s linear infinite;
  }
  @keyframes lds-dual-ring {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  
  </style>
  