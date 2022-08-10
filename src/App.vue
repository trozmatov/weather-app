<template>
  <main :class="typeof weather.main !='undefined' && weather.main.temp > 16 ? 'warm': '' ">
    <div class="search-box">
      <input type="text" class="search-bar"  placeholder="Search...." v-model="query" @keypress="fetchWeather"/>
    </div>
    <div class="weather-wrap" v-if=" typeof weather.main !='undefined'">
        <div class="location-box">
        <div class="weather-location">{{weather.name}} , {{weather.sys.country}}</div>
        <div class="date">{{dailyDate()}}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
        <div class="weather">{{weather.weather[0].main}}</div>
      </div>
    </div>
  </main>
</template>

<script>


export default {
  name: 'App',
  data(){
    return{
      base_url: 'https://api.openweathermap.org/data/2.5' ,
      api_key: '70f7c40d1e4c8229f18a3634de5f6add',
      query:'',
      weather:{},
    }
  },
  methods:{
    fetchWeather(e) {
      if (e.key =='Enter'){
        fetch(`${this.base_url}/weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json();
          }) .then(this.setResults)
      }
    },
    setResults(results){
      this.weather=results
      console.log(results)
    },
    dailyDate(){
    let d = new Date();
    let months = ["January" , "Febuary" , "March" , "April" , "May" , "June" , "July" , "August" , "September" , "October" , "November" , "December"];
    let days = ["Sunday" , "Monday" , "Tuesday" , "Wednesday" , "Thursday", "Friday" , "Saturday"];

    let day= days[d.getDay()];
    let date = d.getDate();
    let month = months[d.getMonth()];
    let  year = d.getFullYear()

    return `${day} ${date}  ${month} ${year}`;
  }
  }
  
};

</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
}

body{
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

#app{
  background:linear-gradient(to bottom , rgba(0,0,0,0.25) , rgba(0,0,0,0.75));
}


main.warm{
  background-image:url(./assets/warm2.jpg);
}

main{
  min-height: 100vh;
  padding:25px;
  background: url(./assets/cool2.jpg);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

.search-box{
  width:100%;
  margin-bottom:30px;

}

.search-box .search-bar{
  width:100%;
  display: block;
  padding:15px;

  color: #313131;
  font-size:20px;

  appearance: none;
  border:none;
  outline:none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 15px 0px 15px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  background-color: rgba(255,255,255,0.75);
  border-radius: 15px 0px 15px 0px;
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
}

.location-box .weather-location{
  text-align: center;
  font-size: 32px;
  font-weight: 500;
  color: #ffff;
  text-shadow:1px 3px rgba(0, 0, 0 , 0.25);

}

.location-box .date{
  text-align: center;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  color: #ffff;
}

.weather-box{
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #ffff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 1px 6px rgba(0, 0, 0 , 0.25);
  background-color: rgba(255,255,255 , 0.25);
  border-radius: 16px;
  box-shadow: 1px 6px rgba(0,0,0, 0.25);
  margin: 30px 0;

}

.weather-box .weather{
  color: #ffff;
  font-size: 48px;
  font-weight: 900;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0 , 0.25);
}
</style>
