<script>
  import SnowyIcon from "~icons/material-symbols/cloudy-snowing";
  import RainyIcon from "~icons/material-symbols/rainy";
  import ClearIcon from "~icons/material-symbols/clear-day";
  import SunnyIcon from "~icons/material-symbols/wb-sunny-outline-rounded";
  import CloudsIcon from "~icons/bi/clouds-fill";
  import MistIcon from "~icons/wi/night-cloudy-windy";
  import OvercastIcon from "~icons/wi/night-cloudy";
  import FogIcon from "~icons/wi/fog";

  let data;
  let forecastData;

  let inputVal = "";

  async function fetchData() {
    const currentWeatherResponse = fetch(
      `https://api.weatherapi.com/v1/current.json?key=${
        import.meta.env.VITE_WEATHER_API_KEY
      }&q=${inputVal}&aqi=no`
    ).then((res) => res.json());

    const forecastResponse = fetch(
      `https://api.weatherapi.com/v1/forecast.json?key=${
        import.meta.env.VITE_WEATHER_API_KEY
      }&q=${inputVal}&aqi=no&days=3`
    ).then((res) => res.json());

    [data, forecastData] = await Promise.all([
      currentWeatherResponse,
      forecastResponse,
    ]);
  }
</script>

<div class="input">
  <input
    type="text"
    class="inputValue"
    bind:value={inputVal}
    placeholder="Enter a city :)"
  />
  <button on:click={fetchData}>
    <img
      src="/search.png"
      alt="button"
      id="button"
      style="height: 30px; width: 30px;"
    />
  </button>
</div>

<section class="container">
  {#if data}
    <p class="name">{data.location.name}, {data.location.country}</p>

    <div
      id="weather-icon"
      style="font-size: 80px; margin: 20px; width: max-content"
    >
      {#if data.current.condition.text === "Clear"}
        <ClearIcon />
      {:else if data.current.condition.text === "Overcast"}
        <OvercastIcon />
      {:else if data.current.condition.text.includes("cloud")}
        <CloudsIcon />
      {:else if data.current.condition.text.includes("snow")}
        <SnowyIcon />
      {:else if data.current.condition.text.includes("rain")}
        <RainyIcon />
      {:else if data.current.condition.text.includes("Sunny")}
        <SunnyIcon />
      {:else if data.current.condition.text.includes("mist")}
        <MistIcon />
      {:else if data.current.condition.text.includes("Fog")}
        <FogIcon />
      {/if}
    </div>

    <div class="display">
      <h1 class="temp">{data.current.temp_c} °C</h1>
      <p class="desc">{data.current.condition.text}</p>
      <p class="feelsLike">Feels like<br /> {data.current.feelslike_c} °C</p>
      <p class="wind">Wind<br /> {data.current.wind_kph} km/hr</p>
      <p class="humidity">Humidity<br /> {data.current.humidity}%</p>
    </div>

    {#if forecastData}
      <div id="forecast-container">
        {#each forecastData.forecast.forecastday as { date, day }}
          {@const dateObj = new Date(date)}
          {@const dayString = dateObj.toLocaleString("en-us", {
            weekday: "long",
          })}
          {@const monthString = dateObj.toLocaleString("en-us", {
            month: "long",
          })}
          {@const dayStr = dateObj.getDate()}
          <div>
            <p class="day">
              {dayString},
              {dayStr}
              {monthString}
            </p>
            <div id="img" style="font-size: 40px; margin: 20px">
              {#if data.current.condition.text === "Clear"}
                <ClearIcon />
              {:else if data.current.condition.text === "Overcast"}
                <OvercastIcon />
              {:else if data.current.condition.text.includes("cloud")}
                <CloudsIcon />
              {:else if data.current.condition.text.includes("snow")}
                <SnowyIcon />
              {:else if data.current.condition.text.includes("rain")}
                <RainyIcon />
              {:else if data.current.condition.text.includes("Sunny")}
                <SunnyIcon />
              {:else if data.current.condition.text.includes("mist")}
                <MistIcon />
              {:else if data.current.condition.text.includes("Fog")}
                <FogIcon />
              {/if}
            </div>
            <p class="tempday">
              {day.mintemp_c}° {day.maxtemp_c}°
            </p>
          </div>
        {/each}
      </div>
    {/if}
  {:else}
    description
  {/if}
</section>

<style>
  .container {
    width: 50rem;

    background-color: rgba(255, 255, 255, 0.122);

    backdrop-filter: blur(1px);

    padding: 2rem;

    border-radius: 24px;

    display: grid;
    justify-items: center;
  }

  .input {
    text-align: center;
    margin: 50px;
  }

  input[type="text"] {
    width: 400px;
    height: 30px;

    margin: 10px;

    background-color: transparent;
    font-family: "JetBrains Mono", monospace, system-ui, -apple-system,
      BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell,
      "Open Sans", "Helvetica Neue", sans-serif;
    font-weight: bold;
    font-size: 25px;
    border: none;
    border-radius: 2px;
    padding: 16px 16px;
    color: rgb(15, 14, 14);
  }

  input::placeholder {
    color: white;
  }
  .display {
    text-align: center;
    font-size: 20px;
    justify-content: center;
  }

  h1 {
    font-size: 2.2rem;
    font-weight: bolder;
  }

  .feelsLike,
  .wind,
  .humidity {
    display: inline-block;
    margin: 50px 30px;
    font-size: 20px;
    font-weight: bolder;
  }

  .name {
    font-size: 20px;
    font-weight: bolder;
  }

  #forecast-container {
    /* display: flex; */
    justify-content: center;
  }

  #forecast-container > div {
    height: 60px;
    width: 600px;
    margin: 20px;

    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    font-weight: bolder;

    font-size: 22px;
    white-space: nowrap;
    /* overflow: hidden; */
    text-overflow: ellipsis;
    /* font-weight: bold; */
  }

  .day {
    text-align: center;
    padding-right: 80px;
  }

  .tempday {
    text-align: center;
    padding-left: 80px;
  }
  /* This will add 40 pixels of padding to the left side of each p element within the div. You can adjust the value of padding-left to increase or decrease the amount of space between the elements. */

  @media (prefers-color-scheme: light) {
    :root {
      color: #213547;
      background-color: #ffffff;
    }

    button {
      background-color: #f9f9f9;
    }
  }
</style>
