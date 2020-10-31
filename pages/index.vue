<template>
  <p v-if="$fetchState.pending">Fetching mountains...</p>
  <p v-else-if="$fetchState.error">An error occured :(</p>

  <div v-else class="container">


    <div class="app-container">
      <div class="img-container">
        <div class="img-text">
          <span class="img-title">weather scanner</span>
          <span class="img-sub-title">helps you find weather conditions in cities...</span>
        </div>
        <img  src="~/assets/images/citybg.jpeg" alt="">
      </div>

      <div class="weather-info-container bg-dark">
        <div class="serch-container p-4">
          <b-input-group prepend="City Name">
            <b-form-input type="text" v-model="palce_name">{{palce_name}}</b-form-input>
            <b-input-group-append>
              <b-button @click="$fetch" variant="outline-info">Get Weather</b-button>
            </b-input-group-append>
          </b-input-group>
        </div>

        <div class="weather-info-wraper px-4">
          <span>
            <span class="title">Locition:</span>
            {{weather_info.location.country}} , {{weather_info.location.name}}
          </span>
          
          <span>
            <span class="title">Local Time:</span>
            {{weather_info.location.localtime}}
          </span>
          
          <span>
            <span class="title">Temperature:</span>
              <span v-if="checked">{{weather_info.current.temp_f}} / f</span>
              <span v-else>{{weather_info.current.temp_c}} / c</span>
          </span>
          
          <span>
            <span class="title">Humidity:</span>
            {{weather_info.current.humidity}}
          </span>
          
          <span>
            <span class="title">conditions:</span> 
            <b-img fluid :src='weather_info.current.condition.icon'></b-img> {{weather_info.current.condition.text}}
          </span>
        </div>

        <div class="toggle-container">
          <b-form-checkbox v-model="checked" switch>
            <span v-if="checked">Set Temperature Unit As Celsius</span>
            <span v-else>Set Temperature Unit As Fahrenheit</span>
          </b-form-checkbox>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        palce_name: 'london',
        checked: false,
      }
    },
    async fetch() {
      this.weather_info = await fetch(
        'http://api.weatherapi.com/v1/current.json?key=d150f92f637a437a9ff125221201510&q=' + this.palce_name
        // 'https://www.thecocktaildb.com/api/json/v1/1/search.php?s=margarita'
      ).then(res => res.json())
    }
  }

</script>

<style>
.app-container {
  display: flex;
  border-radius: 10px;
  overflow: hidden;
  margin-top: 100px;
}

.img-container {
  position: relative;
  flex-basis: 50%;
  background-color: black;
}

.img-text {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: absolute;
  top: 130px;
  width: 100%;
}

.img-title {
  font-size: 48px;
  color: #FFFFFF;
  font-weight: bold;
}

.img-sub-title {
  font-size: 18px;
  color: #FFFFFF;
}
.img-container img{
  width: 100%;
  opacity: 0.5;
}

.weather-info-container {
  flex-basis: 50%;
  position: relative;
}

.weather-info-wraper {
  display: flex;
  flex-direction: column;
  
  color: #ffffff;
}

.weather-info-wraper span{
  padding: 20px 0 20px 0;
  border-bottom: 2px solid #394047;
}

.weather-info-wraper span:last-child{ 
  border-bottom: none ;
} 

.title {
  color: #17a2b8;
  border-bottom: none !important;
}

.toggle-container {
  display:flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  position: absolute;
  bottom: 20px;
  color: #ffffff;
}
.custom-control-input:checked ~ .custom-control-label::before {
  background-color: #17a2b8;
  border-color : #17a2b8;
}

@media only screen and (max-width: 768px) { 
  .app-container {
    margin-top: 200px;
  }
  
  .img-text { 
    top: 24px;
    text-align: center;
  }

  .toggle-container {
    bottom: 0;
  }

}


@media only screen and (max-width: 600px) {
  .container {
    padding: 0;
  }

  .app-container{
    margin: 0;
  }

  .img-container {
  display: none;
  } 

  .weather-info-container {
    flex-basis: 100%;
    height: 100vh;
  }
}
</style>
