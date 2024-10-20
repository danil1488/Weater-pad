<script setup>
import { ref, onMounted, computed } from 'vue'
import Highlights from './components/Highlights.vue'
import WeatherSummary from './components/WeatherSummary.vue'
import { API_KEY, BASE_URL } from './constants/index.js'
import Coords from './components/Coords.vue'
import Humidity from './components/Humidity.vue'

const city = ref('Magnitogorsk')
const weatherInfo = ref(null)
const errorMessage = ref('')
const isError = computed(() => weatherInfo.value?.cod !== 200)

function getWeather() {
  errorMessage.value = ''
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => {
      if (!response.ok) {
        throw new Error('Failed to fetch weather data')
      }
      return response.json()
    })
    .then((data) => (weatherInfo.value = data))
    .catch((error) => {
      weatherInfo.value = null
    })
}

onMounted(getWeather)
</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div v-if="errorMessage" class="error-message">
            {{ errorMessage }}
          </div>
          <div v-else>
            <div class="sections">
              <section
                :class="[
                  'section',
                  'section-left',
                  { 'section-error': isError },
                ]"
              >
                <div class="info">
                  <div class="city-inner">
                    <input
                      v-model="city"
                      @keyup.enter="getWeather"
                      type="text"
                      class="search"
                    />
                  </div>
                  <WeatherSummary v-if="!isError" :weatherInfo="weatherInfo" />
                  <div v-else class="error">
                    <div class="error-title">Всё сломалось...</div>
                    <div v-if="weatherInfo?.message" class="error-message">
                      {{ weatherInfo?.message }}
                    </div>
                  </div>
                </div>
              </section>
              <section v-if="!isError" class="section section-right">
                <Highlights :weatherInfo="weatherInfo" />
              </section>
            </div>
            <div v-if="weatherInfo?.weather" class="sections">
              <Coords :coord="weatherInfo.coord" />
              <Humidity :humidity="weatherInfo.main.humidity" />
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.error-message {
  color: red;
  text-align: center;
  margin: 20px 0;
}
</style>

<style lang="sass" scoped>
@import './assets/styles/laptop'
.laptop
  width: 100%
  padding: 20px
  background-color: #0e100f
  border-radius: 25px


@import './assets/styles/main'

.page
  position: relative
  display: flex
  justify-content: center
  align-items: center
  min-height: 100vh
  padding: 20px 0
  background-color: #59585d



.sections
  display: flex
  width: 100%

  @media (max-width: 767px)
    flex-direction: column

.section-left
  width: 30%
  padding-right: 10px

  @media (max-width: 767px)
    width: 100%
    padding-right: 0

  &.section-error
  min-width: 235px
  width: auto
  padding-right: 0

.section-right
  width: 70%
  padding-left: 10px

  @media (max-width: 767px)
    width: 100%
    margin-top: 16px
    padding-left: 0

.city-inner
  position: relative
  display: inline-block
  width: 100%

  &::after
    content: ''
    position: absolute
    top: 0
    right: 10px
    width: 25px
    height: 25px
    background: url('./assets/img/search.svg') no-repeat 50% 50%
    background-size: contain
    transform: translateY(50%)
    cursor: pointer

.info
  height: 100%
  padding: 16px
  background-size: cover
  border-radius: 25px
  background: linear-gradient(-45deg, $color1 0%, $color1 25%, $color2 40%, $color3 75%, $color3 100%)
  background-size: 400%
  background-position: 0 100%
  transition: all 400ms ease-out

  &:hover
    background-position: 100% 0
    transition: all 300ms ease-in

.search
  width: 100%
  padding: 16px
  font-family: 'Inter', Arial, sans-serif
  color: $white
  background-color: rgba(0, 0, 0, 0.75)
  border-radius: 16px
  border: none
  outline: none
  cursor: pointer
  opacity: 70%

.section-bottom
  width: 50%
  margin-top: 16px

  @media (max-width: 767px)
    width: 100%
</style>
