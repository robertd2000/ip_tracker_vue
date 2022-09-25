<script setup>
import { onMounted, ref } from '@vue/runtime-core'
import leaflet from 'leaflet'
import axios from 'axios'
import IPInfo from '../components/IPInfo.vue'

let mymap
const queryIp = ref('')
const ipInfo = ref(null)

onMounted(() => {
  mymap = leaflet.map('mapid').setView([51.505, -0.09], 10)

  leaflet
    .tileLayer(
      `https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}{retina}?access_token=${
        import.meta.env.VITE_API_KEY
      }`,
      {
        attribution:
          'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
        maxZoom: 18,
        id: 'mapbox/streets-v11',
        tileSize: 512,
        zoomOffset: -1,
        accessToken: import.meta.env.VITE_API_KEY,
        retina: '@2x',
      }
    )
    .addTo(mymap)
})

const getIpInfo = async () => {
  try {
    const data = await axios.get(
      `https://geo.ipify.org/api/v1?apiKey=${
        import.meta.env.VITE_IP_API_KEY
      }&ipAddress=${queryIp.value}`
    )

    const result = data.data
    ipInfo.value = {
      address: result.ip,
      state: result.location.region,
      timezone: result.location.timezone,
      isp: result.isp,
      lat: result.location.lat,
      lng: result.location.lng,
    }

    leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(mymap)
    mymap.setView([ipInfo.value.lat, ipInfo.value.lng], 13)
  } catch (e) {
    alert(e.message)
  }
}
</script>

<template>
  <div class="home">
    <div class="search">
      <div class="search-input">
        <h1>IP Address Tracker</h1>
        <div>
          <input
            type="text"
            v-model="queryIp"
            placeholder="Search for any IP address or leave empty to get your ip info"
          />
          <i class="fas fa-chevron-right" @click="getIpInfo"></i>
        </div>
      </div>

      <IPInfo v-if="ipInfo" :ipInfo="ipInfo" />
    </div>

    <div id="mapid" class="map"></div>
  </div>
</template>

<style lang="scss">
.home {
  @apply flex flex-col h-screen max-h-screen;

  .search {
    @apply z-20 flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32;

    .search-input {
      @apply w-full max-w-screen-sm;

      > h1 {
        @apply text-white text-center text-3xl pb-4;
      }

      > div {
        @apply flex;

        > input {
          @apply flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none;
        }

        > i {
          @apply cursor-pointer bg-black text-white px-4 rounded-tr-md rounded-br-md flex items-center;
        }
      }
    }
  }

  .map {
    @apply h-full z-10;
  }
}
</style>
