<template>
  <div v-if="data" class="flex w-full flex-col items-start">
    <div class="flex w-full flex-col items-start md:flex-row">
      <div class="relative flex w-full flex-col items-start md:w-[65%]">
        <div class="absolute top-0 left-0 flex flex-col items-start">
          <h3 class="bg-white py-2 px-4 text-2xl font-bold text-black">{{ data.name }}</h3>
          <h4 class="bg-white py-2 px-4 text-lg text-black">{{ data.prices.price.value }} {{ data.prices.price.currencyCode }}</h4>
        </div>
        <HeartIcon className="absolute top-0 right-0 h-[50px] w-[50px] bg-white p-2" />
        <div class="relative flex h-[600px] w-full flex-col items-center">
          <img :src="relativizeURL(data.images[0].url)" class="h-auto w-full max-w-[600px]" />
        </div>
        <div class="product-thumbnails flex flex-row items-start overflow-x-scroll">
          <img
            loading="lazy"
            :key="image.url"
            v-for="image in data.images"
            :src="relativizeURL(image.url)"
            class="h-[250px] w-auto cursor-pointer hover:bg-white"
          />
        </div>
      </div>
      <div class="flex w-full flex-col items-start px-10 md:w-[35%]">
        <h1 class="mt-10 text-3xl font-bold text-white md:mt-0">{{ data.name }}</h1>
        <h2 v-html="data.description" class="text-md mt-5 font-light text-[#FFFFFF75]"></h2>
        <div class="mt-10 flex w-full flex-row justify-between">
          <div class="flex flex-row items-center space-x-1">
            <StarIcon class="h-[20px] w-[20px] text-[#FFFFFF75]" />
            <StarIcon class="h-[20px] w-[20px] text-[#FFFFFF75]" />
            <StarIcon class="h-[20px] w-[20px] text-[#FFFFFF75]" />
            <StarIcon class="h-[20px] w-[20px] text-[#FFFFFF75]" />
            <StarIconOutline class="h-[18px] w-[18px] text-[#FFFFFF75]" />
          </div>
          <span class="text-[#FFFFFF75]">36 reviews</span>
        </div>
        <button class="mt-5 w-full bg-black px-2 py-4 uppercase text-white">Add To Cart</button>
        <span class="mt-5 text-lg font-medium text-white">Care</span>
        <span class="mt-2 font-light text-[#FFFFFF75]">This is a limited edition production run. Printing starts when the drop ends.</span>
        <div class="mt-5 h-[1px] w-full bg-[#FFFFFF30]"></div>
        <span class="mt-5 text-lg font-medium text-white">Details</span>
        <span class="mt-2 font-light text-[#FFFFFF75]">
          This is a limited edition production run. Printing starts when the drop ends. Reminder: Bad Boys For Life. Shipping may take 10+ days due to
          COVID-19.
        </span>
        <div class="mt-5 h-[1px] w-full bg-[#FFFFFF30]"></div>
      </div>
    </div>
    <div class="mt-10 h-[1px] w-full bg-gray-300"></div>
    <div class="relative mt-10 flex w-full flex-col">
      <h1 class="px-5 text-2xl font-bold text-[#FFFFFF75]">Related Products</h1>
      <div v-if="items.length" class="product-thumbnails flex flex-row items-start overflow-x-scroll">
        <router-link
          :key="image.path"
          :to="`/product${image.path}`"
          v-for="image in items"
          class="h-[250px] min-w-[250px] cursor-pointer hover:bg-white"
        >
          <img loading="lazy" :src="relativizeURL(image.images[0].url)" class="h-auto w-[250px] cursor-pointer hover:bg-white" />
        </router-link>
      </div>
    </div>
    <div class="mt-10 h-[1px] w-full bg-gray-300"></div>
  </div>
</template>

<style scoped>
.product-thumbnails {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
.product-thumbnails::-webkit-scrollbar {
  display: none;
}
</style>

<script>
import StarIcon from './StarIcon.vue'
import HeartIcon from './HeartIcon.vue'
import LeftSideBar from './LeftSideBar.vue'
import RightSideBar from './RightSideBar.vue'
import StarIconOutline from './StarIconOutline.vue'
import { relativizeURL } from '../../lib/helper'

export default {
  name: 'Product',
  components: {
    StarIcon,
    HeartIcon,
    LeftSideBar,
    RightSideBar,
    StarIconOutline,
  },
  data: () => {
    return {
      items: [],
      data: undefined,
    }
  },
  methods: {
    relativizeURL: (url) => relativizeURL(url),
    fetchData() {
      let link = window.location.origin
      fetch(`${link}/l0-api/products/${this.$route.params.slug}`)
        .then((res) => res.json())
        .then((res) => {
          this.data = res
        })
      fetch(`${link}/l0-api/products/all`)
        .then((res) => res.json())
        .then((res) => {
          if (res && res.length > 0) {
            this.items = res
          }
        })
    },
  },
  async created() {
    this.$watch(
      () => this.$route.params,
      () => this.fetchData(),
      { immediate: true }
    )
  },
}
</script>
