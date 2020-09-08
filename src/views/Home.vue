<template>
  <div class="about">
    <h1>現在の天気</h1>
    <select v-model="selectCity">
      <option value="Sapporo">札幌</option>
      <option value="Sendai">仙台</option>
      <option value="Tokyo">東京</option>
      <option value="Nagoya">名古屋</option>
      <option value="Osaka">大阪</option>
      <option value="Hiroshima">広島</option>
      <option value="Fukuoka">福岡</option>
    </select>
    <!-- <h2>今日の天気</h2> -->
    <h2 class="weather_city">{{ city }}</h2>
    <div class="weather_temp">{{ temp | formatTemp }}</div>
    <div class="weather_condition" v-if="condition.main == 'Rain'">雨</div>
    <div class="weather_condition" v-else-if="condition.main == 'Clouds'">
      曇り
    </div>
    <div class="weather_condition" v-else-if="condition.main == 'Clear'">
      晴れ
    </div>
    <div class="weather_condition" v-else-if="condition.main == 'Snow'">雪</div>
    <div v-else></div>
    <img v-show="condition.main == 'Snow'" src="../assets/snow.jpeg" alt />
    <img v-show="condition.main == 'Clear'" src="../assets/clear.jpeg" alt />
    <img v-show="condition.main == 'Clouds'" src="../assets/clouds.jpeg" alt />
    <img v-show="condition.main == 'Rain'" src="../assets/rain.jpeg" alt />
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      temp: "",
      condition: {
        main: "",
      },
      selectCity: "Tokyo",
    };
  },
  methods: {},
  watch: {
    selectCity: {
      handler(selectCity) {
        console.log(selectCity);
        let apiURL =
          "https://api.openweathermap.org/data/2.5/forecast?q=" +
          this.selectCity +
          ",jp&units=metric&lang=ja&APPID=180cd81c3216710c472690ffec5655be";
        this.axios
          .get(apiURL)
          .then(
            //axiosの処理が成功した場合に処理させたいことを記述
            function (response) {
              this.city = response.data.city.name;
              this.temp = response.data.list[0].main.temp;
              this.condition = response.data.list[0].weather[0];
              // console.log(response);
            }.bind(this)
            //this.axiosの続きにあるfunctionなので、thisの内容が変わる。
            //functionの中にまたthisを使いたいなら、.bind(this)をつけないとダメ
            //もう一つの解決策として、コールバックをarrow functionで書くこともできる
          )
          .catch(function (error) {
            //axiosの処理にエラーが発生した場合に処理させたいことを記述
            console.log(error);
          });
      },
      immediate: true,
    },
  },
  filters: {
    formatTemp: function (val) {
      return (val = val + "℃");
    },
  },
};
</script>

<style scoped>
.weather_city {
  font-weight: bold;
}
.weather_temp {
  font-size: 18px;
}
div {
  margin: 10px;
  font-weight: bold;
}
h2 {
  margin-bottom: 0;
}
select {
  padding: 5px 10px 7px;
  width: 20%;
  max-width: 150px;
  border: #2c3e50 1px solid;
}
</style>
