<template>

        <div class="block">
        
            <div class="days">
                <div class="place">
                <h1 >{{ stadt }}</h1>
                </div>
                <h1> {{datum}} </h1>
                <h2>Temperature: {{temperature}}&deg;C</h2>
                <h2>Minimum Temperature: {{minimumTemperature}}&deg;C</h2>
                <h2>Maximum Temperature: {{maximumTemperature}}&deg;C</h2>
                <h3>{{beschreibung}}</h3>

             </div>
     </div>

</template>

<script>

export default{
    name: 'City',
    props: {
        stadt: String,
        datum : Date,
        temperature: Number,
        minimumTemperature : Number,
        maximumTemperature : Number,
        beschreibung: String,
    },       
}
</script>

<style scoped>


.block{
    margin-bottom: 2rem;
}



.place{
    background-color: powderblue;
    color: white;
    margin-top: 0px;
    font-family: sans-serif;
     padding-left:0rem;
     padding-right:0rem;
}

h1,h2,h3{
    margin-top: 20px;
    font-family: sans-serif;
}

h3{
    margin-bottom: 8px;
}



.days{
    
     border: 1px solid white;
     border-radius:15px ;
     background-color: lightpink;
     text-align: center;
     width: 400px;
     height: 300px;
     box-shadow: 0px 2px 8px rgba(0,0,0,0.1);
     color: white;
     padding-left: 1rem;
     padding-right: 1rem;
    
}

.days:hover{
    cursor:pointer;
    color: black;
    border-color: pink;
    
}

@media screen {
  .block{
      height: auto;
      width: auto;
  }  

  .days{
      height: auto;
      width: auto;
  }

  
}

</style>
