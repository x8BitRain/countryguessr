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
  let incorrectGuess = false;
  let correctGuess = false;
  let inputRef

  const randomNumber = (max: number) => {
    return Math.floor(Math.random() * (max));
  };

  const changeMap = () => {
    const length = countryList.length;
    currentCountry = countryList[randomNumber(length)];
    inputRef?.focus()
  };

  const removeCountryFromList = () => {
    const index = countryList.findIndex(e => e.iso === currentCountry.iso)
    countryList.splice(index, 1)
  };

  const checkCountry = (e) => {
    if (e.key !== 'Enter' || !inputValue) return
    if (currentCountry.country.toLowerCase() === inputValue.toLowerCase()) {
      inputValue = ''
      guessedCountries.push(currentCountry)
      removeCountryFromList()
      guessedCountriesAmount++
      incorrectGuess = false
      correctGuess = true
      setTimeout(() => correctGuess = false, 280)
      changeMap()
    } else {
      incorrectGuess = true
    }
  };

  addEventListener('keyup', (event) => {
    if (event.key === '/') {
      changeMap();
      inputValue = ''
    }
  })

  $: path = `mapsicon/all/${currentCountry?.iso?.toLowerCase()}/vector.svg`
  changeMap()
</script>


<svelte:head>
  <link rel="icon" type="image/svg+xml" href={path} />
</svelte:head>

<main>
  <div class="amount-guessed">
    {guessedCountriesAmount}/{countryList.length}
  </div>
  <div class="re-roll" on:click={changeMap} role="button">
    🔄
  </div>
  <div class="save-progress" on:click={changeMap} role="button">
    <input type="checkbox" id="save-progress">
    <label for="save-progress">Save progress</label>
  </div>
  <div class="map-container">
    <img class="map" src={path} alt="">
  </div>
  <div class="guess-input__container">
    <input
      class="guess-input
        {incorrectGuess ? 'guess-input--invalid' : ''}
        {correctGuess ? 'guess-input--correct' : ''}"
       bind:this={inputRef}
       type="text"
       on:keyup={checkCountry}
       bind:value={inputValue} autofocus
    >
  </div>
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
    .re-roll {
      position: absolute;
      top: 10px;
      right: 20px;
      font-size: 38px;
      user-select: none;
      cursor: pointer;
      transition: transform 150ms ease-in;
      &:active {
        transform: rotate(-359deg);
      }
    }
    .save-progress {
      display: flex;
      align-content: center;
      gap: 10px;
      position: absolute;
      bottom: 10px;
      left: 16px;
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
    .guess-input__container {
      display: flex;
      justify-content: center;
    }
    .guess-input {
      margin: -10px 20vw;
      border: none;
      border-bottom: 2px solid #6565b7;
      background-color: transparent;
      outline: none;
      padding: 0 30px 10px 30px;
      font-size: 34px;
      transition: border-color 150ms ease;
      &--invalid {
        border-bottom-color: #e11f1f;
      }
      &--correct {
        border-bottom-color: green;
      }
      @media (max-width: 600px) {
        padding: 0 20px 8px 20px;
        margin: 0 20px 0 18px;
        width: 70vw;
      }
    }
    @media (max-width: 600px) {
      grid-template-rows: 50vh;
    }
  }
</style>
