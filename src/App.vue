<template>
  <div class="container">
    <header>
        <h1> Your Weather forecast </h1>
    </header>
    <div>
        <input type="text"  placeholder="what is your city?" class="cityinput" name="city"  v-model="citySearch" v-on:change="test()" />
        <button v-on:click="handleSearch(citySearch)" :class="city">Search</button>
    </div>
  <div class="big-wrapper">
       <City v-for="item in items" :key="item.message" :datum="item.datum"  :temperature="item.temperature" :minimumTemperature="item.minimumTemperature" :maximumTemperature="item.maximumTemperature" :beschreibung="item.beschreibung" :stadt="item.stadt"/>
  </div>
  </div>
</template>

<script>
import City from './components/City.vue'
import moment from "moment";

export default {

  name: 'App',
  components: {
    City,
  },
   data: ()=>{
   return {
     items: [ ],
     citySearch: ""
   }
  },
    methods: {
     
     
     async handleSearch(city){
     this.items = [];
       let result = await this.getCityWeather(city);
       console.log(result);
       if(result === undefined ){
         result = await this.getCityWeather("berlin");
         result.city.name = "berlin";
       }
       this.items = this.filterList(result.list, result.city.name);
     },
     
     
     async getCityWeather(city){
            const baseURL = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=7eb89794063ff124eee3ab9fc63d3056&lang=de_de&&units=metric`;

       const res = await fetch(baseURL);
       // if status code is 200 then the request was successful 
       if(res.status===200){
           const { list , city} = await res.json();
           return { list , city};
       }
       // every other status code will signal either a bad request or a not found resource
       return undefined;
     },


     filterList(list, cityName){
     
      let seen = new Set();
      return list.map( (item) =>(
        {
        stadt: cityName,
        datum : moment(item.dt_txt).format('dddd'),
        temperature: item.main.temp,
        minimumTemperature : item.main.temp_min,
        maximumTemperature : item.main.temp_max,
        beschreibung: item.weather[0].description
        }
      )).filter(item =>{
        //to avoid Date repetition
        return seen.has(item.datum) ? false : seen.add(item.datum);
      }).slice(0,5);

     },
     test(){
     console.log("changes")
     }
     ,
     
     moment: function () {
        return moment();
  }
   },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body{
  font-family: 'Poppins', sans-serif;
  
}


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: powderblue;
  height: 100%;
  width: 100%;

}


.big-wrapper{
  
  display: flex;
  margin-top: 10rem;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
   
}

.container{
 
  margin-top: 30px auto;
  padding: 30px;
}

header{
  margin-bottom: 80px;
  align-items: center;
  font-size: 25px;
  font-weight: bold;
 
}
.cityinput{
  width: 45%;
  height: 40px;
  outline-color: none;
  border: none;
  border-top-left-radius: 10px;
  border-bottom-left-radius:10px;
}

input::placeholder{
   color:#aaaaaa;
   font-size: 20px;
   padding-left: 6px;
   font-size: 12px;
   font-weight: bold;
}

input:focus{
  outline: none;
 border: none;
}

button{
  border: none;
  border-top-right-radius: 10px;
  border-bottom-right-radius:10px;
  height: 40px;
   width: 5%;
   color: white;
   font-size: 14px;
   font-weight: bold;
   background-color: pink;
   padding-left: 1rem;
   padding-right: 1rem;
}

@media screen {
  button,input{
     font-size: auto;
     font-weight: auto;
      width: auto;
  }
}
 


</style>
