<template>
  <div class="palette__container">
      <h1>COLORS</h1>
      <ul class="swatch__container">
        <li v-for="(color, i) in palette" class="swatch__wrapper" :key="`color-${i}`">
          <div :style="{ backgroundColor: color.hex }" class="swatch">
            <div :style="{ color: color.typeTextColor }" class="swatch__type">№ {{ color.number }}. {{ color.type }}</div>
            <div :style="{ color: color.hexTextColor }" class="swatch__hex">{{ color.hex }}</div>
            <div :style="{ color: color.nameTextColor }" class="swatch__name">{{ color.name }}</div>
          </div>
        </li>
      </ul>
    </div>
</template>
<script>
export default {
  props: {
    palette: {
      type: Array,
      default: () => ([])
    }
  }
}
</script>

<style lang="scss" scoped>
.palette__container {
  padding: 0 1.6rem;
}

.swatch__container {
  margin: 0;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.swatch {
  display: inline-block;
  width: 100%;
  height: 100px;
  border-radius: 4px;
  margin-bottom: 1em;
  transition: background .3s ease;
}
.swatch__wrapper {
  display: inline-block;
  width: 30%;
  list-style: none;
  margin-bottom: 1.4em;
}
.swatch__hex {
  text-transform: uppercase;
  font-weight: 600;
  letter-spacing: 1.5px;
  padding: .3em 0 .2em .3em;
}
.swatch__name {
  font-size: 11px;
  color: #aaa;
  padding-left: .5em;
}
.swatch__type {
  font-size: 13px;
  color: #aaa;
  padding: 1.0em 0 .5em .5em;
}
</style>