<script lang="ts">
  import countries from './assets/countries.json';

  interface ICountry {
    iso: string,
    country: string,
  }

  let countryList: ICountry[] = countries.countries
  let guessedCountries: ICountry[] = []
  let guessedCountriesAmount = 0
  let currentCountry: ICountry
  let inputValue

  const randomNumber = (max: number) => {
    return Math.floor(Math.random() * (max + 1));
  };

  const changeMap = () => {
    const length = countryList.length;
    currentCountry = countryList[randomNumber(length)];
  };

  const checkCountry = (e) => {
    if (e.key !== 'Enter' || !inputValue) return
    if (currentCountry.country.toLowerCase() === inputValue.toLowerCase()) {
      inputValue = ''
      guessedCountries.push(currentCountry)
      guessedCountriesAmount++
      changeMap()
    }
  };

  $: path = `mapsicon/all/${currentCountry?.iso?.toLowerCase()}/vector.svg`
  changeMap()
</script>

<main>
  <div class="amount-guessed">
    {guessedCountriesAmount}/{countryList.length}
  </div>
  <div class="map-container">
    <img class="map" src={path} alt="">
  </div>
  <input class="guess-input" type="text" on:keyup={checkCountry} bind:value={inputValue} autofocus>
</main>

<style lang="scss">
  main {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 80vh;
    gap: 0;
    grid-template-areas:
    "."
    ".";
    .amount-guessed {
      position: absolute;
      top: 10px;
      left: 20px;
    }
    .map-container {
      display: flex;
      justify-content: center;
      .map {
        height: 60vh;
        width: 60vw;
        filter: invert(1);
        padding: 80px;
        @media (max-width: 600px) {
          padding: 0;
        }
      }
    }
    .guess-input {
      margin: -10px 20vw;
      border: none;
      border-bottom: 2px solid #6565b7;
      background-color: transparent;
      outline: none;
      padding: 0 30px 10px 30px;
      font-size: 34px;
      @media (max-width: 600px) {
        padding: 0;
        margin: 0;
      }
    }
  }
</style>
