<template>
  <div class="wrap" v-bind:class="[condition.main]">
    <select v-model="selectCity">
      <option value="Sapporo">札幌</option>
      <option value="Akita">秋田</option>
      <option value="Sendai">仙台</option>
      <option value="Tokyo">東京</option>
      <option value="Nagoya">名古屋</option>
      <option value="Osaka">大阪</option>
      <option value="Hiroshima">広島</option>
      <option value="Fukuoka">福岡</option>
      <option value="Okinawa">沖縄</option>
    </select>
    <div class="inner">
      <p>{{ today }}{{ time }}</p>
      <div class="weather_city">{{ city }}</div>
      <div class="flex">
        <div class="weather-icon">
          <span
            v-if="condition.main == 'Clear'"
            class="fas fa-sun fa-2x"
          ></span>
          <span
            v-if="condition.main == 'Rain'"
            class="fas fa-cloud-rain fa-2x"
          ></span>
          <span
            v-if="condition.main == 'Clouds'"
            class="fas fa-cloud fa-2x"
          ></span>
          <span
            v-if="condition.main == 'Snow'"
            class="fas fa-snowflake fa-2x"
          ></span>
        </div>
        <div class="weather_condition">{{ condition.description }}</div>
      </div>
      <div class="weather_temp">{{ temp | formatTemp }}</div>
    </div>
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
        description: "",
      },
      selectCity: "Tokyo",
      today: "",
      time: "",
    };
  },
  methods: {
    getDate: function() {
      var currentDate = new Date();
      //console.log(currentDate);
      var weeks = ["月", "火", "水", "木", "金", "土", "日"];
      var week = currentDate.getDay() - 1;
      var getWeek = weeks[week];
      //console.log(weeks[week]);
      var getHour = String(
        currentDate.getHours() < 10
          ? "0" + currentDate.getHours()
          : currentDate.getHours()
      );
      var getMinute = String(
        currentDate.getMinutes() < 10
          ? "0" + currentDate.getMinutes()
          : currentDate.getMinutes()
      );
      var getSecond = String(
        currentDate.getSeconds() < 10
          ? "0" + currentDate.getSeconds()
          : currentDate.getSeconds()
      );
      this.today =
        currentDate.getFullYear() +
        "年" +
        (currentDate.getMonth() + 1) +
        "月" +
        currentDate.getDate() +
        "日　" +
        getWeek +
        "曜日";
      this.time = "　" + getHour + ":" + getMinute + ":" + getSecond;
    },
  },
  mounted() {
    window.setInterval(() => {
      this.getDate();
    }, 1000);
  },
  watch: {
    selectCity: {
      handler(selectCity) {
        //console.log(selectCity);
        let apiURL =
          "https://api.openweathermap.org/data/2.5/forecast?q=" +
          this.selectCity +
          ",jp&units=metric&lang=ja&APPID=180cd81c3216710c472690ffec5655be";
        this.axios
          .get(apiURL)
          .then(
            //axiosの処理が成功した場合に処理させたいことを記述
            function(response) {
              this.city = response.data.city.name;
              this.temp = response.data.list[0].main.temp;
              this.condition = response.data.list[0].weather[0];
              console.log(response);
            }.bind(this)
            //this.axiosの続きにあるfunctionなので、thisの内容が変わる。
            //functionの中にまたthisを使いたいなら、.bind(this)をつけないとダメ
            //もう一つの解決策として、コールバックをarrow functionで書くこともできる
          )
          .catch(function(error) {
            //axiosの処理にエラーが発生した場合に処理させたいことを記述
            console.log(error);
          });
      },
      immediate: true,
    },
  },
  filters: {
    formatTemp: function(val) {
      return (val = Math.round(val) + "°");
    },
  },
};
</script>

<style scoped>
.wrap {
  background-size: cover;
  height: 100%;
  position: fixed;
  width: 100%;
  margin: 0;
}
.Rain {
  background-image: url(../assets/raining.jpg);
}
.Clear {
  background-image: url(../assets/sunny.jpg);
}
.Clouds {
  background-image: url(../assets/cloudy.jpg);
}
.Snow {
  background-image: url(../assets/snowy.jpg);
}
.inner {
  width: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -65%);
}
.weather_city {
  font-size: 3rem;
  font-weight: bold;
}
.weather_temp {
  font-size: 9rem;
  font-weight: bold;
}
.weather_condition {
  font-size: 2rem;
}
div {
  margin: 10px 5px;
  align-items: center;
}
p {
  letter-spacing: 1px;
}
.flex {
  display: flex;
  justify-content: center;
}
select {
  -moz-appearance: none;
  -webkit-appearance: none;
  appearance: none;
  display: inline-block;
  width: 15%;
  margin: 1em 0;
  padding: 0.6em 0.2em;
  cursor: pointer;
  font-size: 0.95em;
  font-weight: 700;
  color: #fff;
  background-color: transparent;
  border: none;
  border-bottom: solid 1px #fff;
  /* 👇左の三角, 右の三角 */
  background-image: linear-gradient(45deg, transparent 50%, #fff 50%),
    linear-gradient(135deg, #fff 50%, transparent 50%);
  /* 👇左の三角のサイズ, 右の三角のサイズ */
  background-size: 5px 5px, 5px 5px;
  /* 👇左の三角の位置, 右の三角の位置 */
  background-position: calc(100% - 15px) 50%, calc(100% - 10px) 50%;
  /* 👇リピートしない */
  background-repeat: no-repeat;
}
select:focus {
  outline: 0;
}
/* IEでデフォルトの矢印を消す */
select::-ms-expand {
  display: none;
}

option {
  background-color: #ccc;
  font-size: 0.95em;
  border: transparent;
}
</style>
