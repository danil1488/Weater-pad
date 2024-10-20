<script setup>
import { computed } from 'vue'
import { getPressureMm, getTime } from '../utils/index.js'

const props = defineProps({
  weatherInfo: {
    type: [Object, null],
    required: true,
  },
})

const timezone = computed(() => props.weatherInfo?.timezone)

const sunriseTime = computed(() => {
  return getTime(props.weatherInfo?.sys?.sunrise + timezone.value)
})
const sunsetTime = computed(() => {
  return getTime(props.weatherInfo?.sys?.sunset + timezone.value)
})
</script>

<template>
  <div v-if="weatherInfo?.weather" class="section highlights">
    <div class="title">Основные моменты сегодня</div>
    <div class="highlights-wrapper">
      <div class="highlight">
        <div class="card">
          <div class="card-title">Ветер</div>
          <div class="card-pic card-pic--wind"></div>
          <div class="card-info">
            <div class="card-justify">
              <div class="info-main">
                <div class="info-main-num">{{ weatherInfo?.wind?.speed }}</div>
                <div class="info-main-text">m/s</div>
              </div>
              <div class="info-main">
                <div class="info-main-num">{{ weatherInfo?.wind?.deg }}</div>
                <div class="info-main-text">deg</div>
              </div>
            </div>
          </div>
        </div>
        <div class="card-small">
          <div class="card-small-title">Порывы ветра</div>
          <div v-if="weatherInfo?.wind?.gust" class="card-small-info">
            <div class="card-small-data">
              <div class="info-main-num">
                {{ Math.round(weatherInfo?.wind?.gust) }}
              </div>
              <div class="info-main-text">m/s</div>
            </div>
            <div class="card-small-hint">
              <div class="card-small-pic card-small-pic--wind"></div>
              <div class="card-small-text text-egorova"></div>
            </div>
          </div>
        </div>
      </div>
      <div class="highlight">
        <div class="card">
          <div class="card-title">Давление</div>
          <div class="card-pic card-pic--pressure"></div>
          <div class="card-info">
            <div class="card-centered">
              <div class="info-main">
                <div class="info-main-num">
                  {{ getPressureMm(weatherInfo?.main?.pressure) }}
                </div>
                <div class="info-main-text">mm</div>
              </div>
            </div>
          </div>
        </div>
        <div class="card-small">
          <div class="card-small-title">По ощущениям</div>
          <div class="card-small-info">
            <div class="card-small-data">
              <div class="info-main-num">
                {{ Math.round(weatherInfo?.main?.feels_like) }}
              </div>
              <div class="info-main-text">°C</div>
            </div>
            <div class="card-small-hint">
              <div
                class="card-small-pic card-small-pic--margin card-small-pic--pressure"
              ></div>
              <div class="card-small-text">
                Насколько жарко или холодно на самом деле
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="highlight">
        <div class="card">
          <div class="card-title">Восход и закат</div>
          <div class="card-pic card-pic--sun"></div>
          <div class="card-info">
            <div class="states">
              <div class="state">
                <div class="state-pic"></div>
                <div class="state-title">Восход</div>
                <div class="state-time">{{ sunriseTime }}</div>
              </div>
              <div class="state">
                <div class="state-pic state-pic--flipped"></div>
                <div class="state-title">Закат</div>
                <div class="state-time">{{ sunsetTime }}</div>
              </div>
            </div>
          </div>
        </div>
        <div class="card-small">
          <div class="card-small-title">Облачность</div>
          <div class="card-small-info">
            <div class="card-small-data">
              <div class="info-main-num">{{ weatherInfo?.clouds?.all }}</div>
              <div class="info-main-text">%</div>
            </div>
            <div class="card-small-hint">
              <div class="card-small-pic card-small-pic--sun"></div>
              <div class="card-small-text">Часть неба закрыта облаками</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main.sass'
@import '../assets/styles/variables'

.highlights
  padding: 28px 16px 16px
  background-size: cover
  border-radius: 25px
  // background: url('/src/assets/img/gradient-4.jpg') no-repeat 0% 0%
  height: 100%
  padding: 16px
  background-size: cover
  border-radius: 25px
  background: linear-gradient(-45deg, $color1 0%, $color1 25%, $color2 40%, $color3 75%, $color3 100%)
  background-size: 400%
  background-position: 0 100%
  transition: all 600ms ease-out

  &:hover
    background-position: 100% 0
    transition: all 300ms ease-in

  &-wrapper
    display: flex
    justify-content: space-between

    @media (max-width: 575px)
      flex-direction: column

.title
  padding-bottom: 16px

.highlight
  width: 32%

  @media (max-width: 575px)
    width: 100%
    margin-bottom: 16px

.card
  min-height: 230px
  padding: 16px
  background: url('/src/assets/img/gradient-2.jpg') no-repeat 50% 50%
  background-size: cover
  border-radius: 8px
  opacity: 80%

  @media (max-width: 1199px)
    padding: 12px

  &-centered
    display: flex
    justify-content: center
    margin-top: 40px

  &-justify
    display: flex
    justify-content: space-between
    margin-top: 40px

  &-title
    padding-bottom: 12px
    font-size: 13px

    @media (max-width: 1199px)
      font-size: 12px

  &-pic
    width: 100%
    height: 90px
    margin-bottom: 16px
    background-repeat: no-repeat
    background-position: 50% 50%
    background-size: contain

    &--wind
      background-image: url('/src/assets/img/equalizer (2).png')

    &--pressure
      background-image: url('/src/assets/img/barometer.png')

    &--sun
      background-image: url('/src/assets/img/sun-moving.png')


.states
  display: flex
  justify-content: space-between

  &--margin
    margin-top: 40px

.state
  width: 40%

  &:last-child
    text-align: right

  &-pic
    width: 20px
    height: 20px
    margin-bottom: 6px
    background: url('/src/assets/img/sun.svg') no-repeat 50% 50%
    background-size: cover

    &--flipped
      margin-left: auto
      -webkit-transform: scaleX(-1)
      transform: scaleX(-1)

  &-title
    font-size: 12px
    color: $gold

  &-time
    font-size: 13px
    font-weight: 700

    @media (max-width: 1199px)
      font-size: 11px

.info-main
  display: flex
  align-items: flex-end

  &:last-child
    text-align: right

  &-num
    font-size: 20px


    @media (max-width: 1199px)
      font-size: 18px

  &-text
    padding-left: 2px
    padding-bottom: 3px
    font-size: 13px
    color: rgba($white, 0.75)

    @media (max-width: 1199px)
      padding-bottom: 1.5px
      font-size: 12px

.card-small
  margin-top: 12px
  padding: 12px 16px
  background: url('/src/assets/img/gradient-2.jpg') no-repeat 50% 50%
  background-size: cover
  border-radius: 8px
  opacity: 80%

  &-title
    font-size: 13px

  &-info
    display: flex
    justify-content: space-between
    align-items: center

    @media (max-width: 1199px)
      flex-direction: column
      align-items: flex-start

  &-pic
    width: 20px
    height: 20px
    background-repeat: no-repeat
    background-position: 50% 50%
    background-size: contain

    @media (max-width: 1199px)
      display: none

    &--margin
      width: 16px
      height: 16px
      margin-bottom: 3px

    &--wind
      background-image: url('/src/assets/img/gusts.svg')

    &--pressure
      background-image: url('/src/assets/img/humidity.svg')

    &--sun
      background-image: url('/src/assets/img/cloud.svg')

  &-data
    display: flex
    align-items: flex-end
    width: 45%

    @media (max-width: 1199px)
      width: 100%
      padding-top: 8px

  &-hint
    width: 55%

    @media (max-width: 1199px)
      width: 100%

  &-text
    font-size: 11px
    line-height: 1.2
    color: rgba($white, 0.6)

    @media (max-width: 1199px)
      min-height: 22px
      font-size: 9px
</style>
