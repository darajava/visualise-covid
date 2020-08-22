<script>
  import { onMount } from 'svelte';
  import axios from 'axios';
  import Loading from './Loading.svelte';

  let canvas;

  const randomNumber = (lower, upper) => {
    return Math.round(Math.random() * (upper - lower)) + lower;
  }

  const numberWithCommas = (x) => {
    return Math.round(x).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
  }

  let chance = 0;
  let country = "USA";
  let type = "deaths";
  let loading = true;

  let countries, countryList = [];
  const getCountries = () => {
    axios.get("https://coronavirus-19-api.herokuapp.com/countries").then(res => {
      countries = res.data;
      countryList = res.data.map(el => {
        return el.country;
      });
      loading = false;
      refetch();
    });
  }

  getCountries();

  let label, perMillion;
  const refetch = (e) => {
    console.log(country)
    console.log(type)
    
    let cachedCountry = countries.find(e => e.country === country);

    const population = (cachedCountry.deaths / cachedCountry.deathsPerOneMillion) * 1000000;

    switch (type) {
      case "deaths":
        label = "Deaths per million";
        chance = (cachedCountry.deathsPerOneMillion / 1000000);
        perMillion = cachedCountry.deathsPerOneMillion;
        break;
      case "cases":
        label = "Cases per million";
        chance = (cachedCountry.casesPerOneMillion / 1000000);
        perMillion = cachedCountry.casesPerOneMillion;
        break;
      case "tests":
        label = "Tests per million";
        chance = (cachedCountry.testsPerOneMillion / 1000000);
        perMillion = cachedCountry.testsPerOneMillion;
        break;
      case "cases-today":
        label = "Cases today per million";
        let casesTodayPerMillion = (1000000 / population) * cachedCountry.todayCases;
        chance = casesTodayPerMillion / 1000000;
        console.log(casesTodayPerMillion);
        perMillion = casesTodayPerMillion;
        break;
      case "deaths-today":
        label = "Deaths today per million";
        let deathsTodayPerMillion = (1000000 / population) * cachedCountry.todayDeaths;
        chance = deathsTodayPerMillion / 1000000;
        console.log(deathsTodayPerMillion);
        perMillion = deathsTodayPerMillion;
        break;
    }
    console.log(cachedCountry);
    draw();
  }


  const draw = () => {
    const ctx = canvas.getContext('2d');

    var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    var data = imageData.data;
    console.log(data);
    for (let i = 0; i < data.length; i += 4) {
      let red = data[i];
      let green = data[i + 1];
      let blue = data[i + 2];
      let alpha = data[i + 3];

      if (Math.random() < chance) {
        data[i] = 255;
        data[i+1] = 255;
        data[i+2] = 255;
        data[i+3] = 255;
      } else {
        data[i] = 0;
        data[i+1] = 0;
        data[i+2] = 0;
        data[i+3] = 255;
      }
    }
    console.log(imageData)
    ctx.putImageData(imageData, 0, 0);
  };
</script>


<div>
  {#if loading}
    <Loading />
  {/if}

  <div class={`${loading ? "hidden" : ""}`}>
    <div class={`input-holder`}>
      <select class="country-input" on:change={() => setTimeout(refetch, 0)} bind:value={country}>
        {#each countryList as countryLabel}
          <option value={countryLabel}>{countryLabel}</option>
        {/each}
      </select>


      <select class="type-input" on:change={() => setTimeout(refetch, 0)} bind:value={type}>
        <option value="deaths">Deaths per million</option>
        <option value="cases">Cases per million</option>
        <option value="tests">Tests per million</option>
        <option value="cases-today">Cases today per million</option>
        <option value="deaths-today">Deaths today per million</option>
      </select>
    </div>

    <div class="info-holder">
      <div class="info">
        {label} in {country}
        <div>
          ({numberWithCommas(perMillion)} per million)
        </div>
      </div>
    </div>

    <canvas
      bind:this={canvas}
      width={1666}
      height={600}
    ></canvas>
  </div>
</div>

<style>
  canvas {
    width: 100vw;
    height: 100vh;
  }

  .input-holder {
    position: absolute;
    z-index: 10000;
    top: 10px;
    left: 10px;
  }

  .type-input, .country-input {
    opacity: 0.5;
    padding: 10px;
    font-size: 18px;
    transition: all 0.4s; 
  }

  .type-input:hover, .country-input:hover, .country-input:active, .type-input:active {
    opacity: 1;
  }

  .info-holder {
    position: absolute;
    text-align: center;
    bottom: 15px;
    width: 100%;
  }
  .info {
    /*width: 100vw;*/
    display: inline-block;

    text-align: center;
    padding: 10px 20px;
    margin: 10px;
    font-weight: bold;
    font-family: sans-serif;
    font-size: 22px;
    color: black;
    /*color: blue;*/
    background-color: rgba(255, 255, 255, 0.6);
    border-radius: 5px;
  }

  .info div {
    font-size: 11px;
  }

  .hidden {
    display: none;
  }

  @media (max-width:480px)  {
    .type-input, .country-input {
      width: calc(100% - 10px);
      margin: auto;
      margin: 5px 0;
    }
  }
</style>
