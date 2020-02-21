<template>
  <div class="about">
    <select v-model="selectValue" @change="showWeather">
      <option disabled value="">都市を選択してください</option>
      <option value="Sapporo">札幌</option> 
      <option value="Tokyo">東京</option>
      <option value="Osaka">大阪</option>
      <option value="Okinawa">沖縄</option>
      <option value="Kumamoto">熊本</option>
    </select>
    <div class="grid">
    <div class="day_1">
      <p class="weather_city">時間:{{ time }}</p>
      <p class="weather_city">気温:{{ temp }}℃</p>
      <p class="bg" v-if="condition.main == 'Rain'"><img v-bind:src="image[0]">雨</p>
      <p class="bg" v-else-if="condition.main == 'Clouds'"><img v-bind:src="image[1]">曇り</p>
      <p class="bg" v-else-if="condition.main == 'Clear'"><img v-bind:src="image[2]">晴れ</p>
      <p class="bg" v-else-if="condition.main == 'Snow'"><img v-bind:src="image[3]">雪</p>
      <p v-else>それ以外</p>
    </div>
    <div class="day_2">
      <p class="weather_city">時間:{{ time1 }}</p>
      <p class="weather_city">気温:{{ temp1 }}℃</p>
      <p class="bg" v-if="condition.main1 == 'Rain'"><img v-bind:src="image[0]">雨</p>
      <p class="bg" v-else-if="condition.main1 == 'Clouds'"><img v-bind:src="image[1]">曇り</p>
      <p class="bg" v-else-if="condition.main1 == 'Clear'"><img v-bind:src="image[2]">晴れ</p>
      <p class="bg" v-else-if="condition.main1 == 'Snow'"><img v-bind:src="image[3]">雪</p>
      <p v-else>それ以外</p>
    </div>
    <div class="day_3">
      <p class="weather_city">時間:{{ time2 }}</p>
      <p class="weather_city">気温:{{ temp2 }}℃</p>
      <p class="bg" v-if="condition.main2 == 'Rain'"><img v-bind:src="image[0]">雨</p>
      <p class="bg" v-else-if="condition.main2 == 'Clouds'"><img v-bind:src="image[1]">曇り</p>
      <p class="bg" v-else-if="condition.main2 == 'Clear'"><img v-bind:src="image[2]">晴れ</p>
      <p class="bg" v-else-if="condition.main2 == 'Snow'"><img v-bind:src="image[3]">雪</p>
      <p v-else>それ以外</p>
    </div>
    </div>
  </div>
</template>

<script>

export default{
  data(){
    return {
      selectValue: '',
      time: '',
      time1: '',
      time2: '',
      temp: '',
      temp1: '',
      temp2: '',
      image:['https://i2.wp.com/www.society19.com/wp-content/uploads/2013/05/rainy-feature.jpg?fit=1600%2C1000&ssl=1',
      'https://images.unsplash.com/photo-1557770713-7e8cf6923538?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1000&q=80',
      'https://i.ytimg.com/vi/3EXe5cx5S-0/maxresdefault.jpg',
      'https://www.inhalton.com/sites/inhalton.com/files/imagecache/lead-image-full/article/2019/02/snowday.jpg'],
      condition:{
        main:'',
      }
    }
  },
  methods: {
    showWeather(){
      console.log(this.selectValue)
      let apiURL = 'https://api.openweathermap.org/data/2.5/forecast?q=' + this.selectValue + ',jp&units=metric&lang=ja&APPID=6feb96feab4f7959cbc8ee732f5cdf79'
      this.axios.get(apiURL).then(function(response){

        this.time = response.data.list[16].dt_txt
        this.time1 = response.data.list[24].dt_txt
        this.time2 = response.data.list[32].dt_txt
        this.temp = response.data.list[16].main.temp
        this.temp1 = response.data.list[24].main.temp
        this.temp2 = response.data.list[32].main.temp
        this.condition.main= response.data.list[16].weather[0].main
        this.condition.main1= response.data.list[24].weather[0].main
        this.condition.main2= response.data.list[32].weather[0].main
        console.log(this.condition.main)
        console.log(response)
      }.bind(this)).catch(function(error){
        console.log('えらー'+ error)
      })

    }
  }
}
</script>
<style>
  .grid{
    margin: 20px 60px;
    width: 80%;
    height: 600px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap:10px;
  }
  .bg img{
    width: 200px;
    height: 150px;
  }
</style>