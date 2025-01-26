<template>
  <!-- contiene la condizione per cambiare lo sfondo: se temp > 16°C, richiama app.warm -->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>

      <div class="search-box"> <!-- div per la barra di ricerca -->
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..." 
          v-model="query" 
          @keypress="GetWeather" /> <!-- richiama la funzione GetWeather -->
      </div>

      <!-- div contenente le informazioni del meteo -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">

        <!-- box per la località -->
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ DateBuilder() }}</div>
        </div>

        <!-- box per la temperatura e le condizioni climatiche-->
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>

      </div>

    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: 'ab0d238194cdc19a63d3c92259f44dd6', //chiave di accesso all'api
      api_url: 'https://api.openweathermap.org/data/2.5/', //url di accesso all'api
      query: '', //stringa dove salvare i dati richiesti all'api
      weather: {} //dichiaro il meteo come un oggetto
    }
  },
  methods: {
    GetWeather(e) { //metodo per effettuare la chiamata all'api
      if (e.key == "Enter") { //se premo enter
        fetch(`${this.api_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`) //effettuo la chiamata
          .then(res => { //salvo i risultati in un file.json
            return res.json();
          }).then(this.SetResults); //richiamo la funzione per passare i dati all'array
      }
    },
    SetResults(results) { //metodo per settare i risultati
      this.weather = results;
    },
    DateBuilder() { //metodo per costruire la data
      let d = new Date(); //dichiamo una variabile come data
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]; //elenco di mesi
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]; //elenco di giorni
      let day = days[d.getDay()]; //salvo il giorno corrente
      let date = d.getDate(); //salvo il num del giorno corrente
      let month = months[d.getMonth()]; //salvo il mese corrente
      let year = d.getFullYear(); //salvo l'anno corrente
      return `${day} ${date} ${month} ${year}`; //ritorno una stringa composta da tutti i campi
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  background-size: cover;
  background-position: bottom;
  background-position-y: -250px;
  transition: 0.4s;
}

main {
  min-height: 150vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-bar {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);

  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.initial-page {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>