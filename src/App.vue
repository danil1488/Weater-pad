<script setup>
import { ref, onMounted } from 'vue'
import Highlights from './components/Highlights.vue'
import WeatherSummaryVue from './components/WeatherSummary.vue'
import { API_KEY, BASE_URL } from './constants/index.js'
import Coords from './components/Coords.vue'
import Humidity from './components/Humidity.vue'
import Loading from './components/Loading.vue'

const city = ref('Magnitogorsk')
const weatherInfo = ref(null)
const isLoading = ref(true)

function getWeather() {
  isLoading.value = true
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => {
      weatherInfo.value = data

      setTimeout(() => {
        isLoading.value = false
      }, 500)
    })
    .catch(() => {
      setTimeout(() => {
        isLoading.value = false
      }, 500)
    })
}

onMounted(getWeather)
</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <Loading v-if="isLoading" />
          <div v-else>
            <div class="sections">
              <section class="section section-left">
                <div class="info">
                  <div class="city-inner">
                    <input
                      v-model="city"
                      @keyup.enter="getWeather"
                      type="text"
                      class="search"
                    />
                  </div>
                  <WeatherSummaryVue :weatherInfo="weatherInfo" />
                </div>
              </section>
              <section class="section section-right">
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
