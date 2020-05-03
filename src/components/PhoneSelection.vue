<template>
  <div class="container">
    <div class="columns">
      <div class="column is-5">
        <img class="product-image" :src="activeOptionImage" :alt="name" />
      </div>
      <div class="column is-7">
        <h1 class="title is-1">{{ name }}</h1>

        <star-rating
          :increment="0.1"
          :max-rating="5"
          :star-size="30"
          :rating="rating"
          :show-rating="false"
        ></star-rating>

        <p class="description">{{ activeOption.displayDescription }}</p>

        <div class="columns is-mobile">
          <div class="column is-6">
            <p>
              Colour:
              <span class="has-text-weight-bold">{{ activeColor.name }}</span>
            </p>

            <div
              v-for="(c, i) in colors"
              :key="i"
              class="color-box-container"
              :class="{ active: selectedColor == i }"
              @click="selectedColor = i"
            >
              <div class="color-box" :style="{ backgroundColor: c.code }"></div>
            </div>
          </div>
          <div class="column is-6">
            <p>
              Capacity:
              <span class="has-text-weight-bold">{{
                activeCapacity.memory
              }}</span>
            </p>

            <div
              v-for="(c, i) in capacities"
              :key="i"
              class="color-box-container"
              :class="{ active: selectedCapacity == i }"
              @click="selectedCapacity = i"
            >
              <div class="color-box has-text-centered">{{ c.memory }}</div>
            </div>
          </div>
        </div>

        <div class="columns pricing-box is-mobile">
          <div class="column">
            <p>
              from
              <span class="price">&pound;{{ activeOptionUpfrontPrice }}</span>
              upfront cost
            </p>
          </div>
          <div class="column is-6 left-border">
            <p>
              When you pay
              <span class="price">&pound;{{ activeOptionMonthlyPrice }}</span> a
              month
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StarRating from 'vue-star-rating'
import data from '@/phones.json'

export default {
  components: {
    StarRating
  },
  data () {
    return {
      options: data[0].deviceSummary,
      rating: parseFloat(data[0].rating),
      name: data[0].groupName,

      selectedColor: 0,
      selectedCapacity: 0
    }
  },
  computed: {
    colors () {
      let colors = []

      this.options.forEach(o => {
        let color = {
          name: o.colourName,
          code: o.colourHex
        }

        if (colors.findIndex(c => c.name === color.name) === -1) {
          colors.push(color)
        }
      })

      return colors
    },
    capacities () {
      let capacities = []

      this.options.forEach(o => {
        let capacity = {
          memory: o.memory
        }

        if (capacities.findIndex(c => c.memory === capacity.memory) === -1) {
          capacities.push(capacity)
        }
      })

      return capacities
    },
    activeColor () {
      return this.colors[this.selectedColor]
    },
    activeCapacity () {
      return this.capacities[this.selectedCapacity]
    },
    activeOption () {
      return this.options.find(o => o.memory === this.activeCapacity.memory && o.colourName === this.activeColor.name)
    },
    activeOptionImage () {
      return this.activeOption.merchandisingMedia[0].value
    },
    activeOptionUpfrontPrice () {
      return this.activeOption.priceInfo.hardwarePrice.oneOffPrice.gross
    },
    activeOptionMonthlyPrice () {
      return this.activeOption.priceInfo.bundlePrice.monthlyPrice.gross
    }
  },
  created () {

  }
}
</script>

<style scoped>
.product-image {
  max-width: 300px;
  margin: 0 auto;
  display: block;
}

.description {
  margin: 2em 0;
}

.color-box-container {
  float: left;
  border-radius: 10px;
  margin-right: 10px;
  cursor: pointer;
}
.color-box-container.active {
  border: 1px solid #33aaff;
}

.color-box {
  width: 60px;
  height: 60px;
  margin: 5px;
  border-radius: 10px;
  background-color: #eee;
  padding-top: 15px;
}

.pricing-box {
  margin-top: 2em;
  padding: 1em;
  background-color: #f5f5f5;
}

.price {
  color: red;
  font-size: 24px;
}

.column.left-border {
  border-left: 1px solid #aaa;
}
</style>
