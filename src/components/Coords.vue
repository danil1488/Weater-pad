<script setup>
const props = defineProps({
  coord: {
    type: Object,
    required: true,
  },
})
</script>
<template>
  <section class="section-bottom">
    <div class="block-bottom">
      <div class="block-bottom-inner">
        <div class="block-bottom-pic pic-coords"></div>
        <div class="block-bottom-texts">
          <div class="block-bottom-text-block">
            <div class="block-bottom-text-block-title">
              Долгота: {{ coord?.lon }}
            </div>
            <div class="block-bottom-text-block-desc">
              Долгота измеряет расстояние к востоку или западу от нулевого
              меридиана.
            </div>
          </div>
          <div class="block-bottom-text-block">
            <div class="block-bottom-text-block-title">
              Широта: {{ coord?.lat }}
            </div>
            <div class="block-bottom-text-block-desc">
              Линии широты начинаются на экваторе (0 градусов широты) и проходят
              восток и запад, параллельно экватору.
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<style lang="sass" scoped>
@import '../assets/styles/main'

.block-bottom
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
</style>
