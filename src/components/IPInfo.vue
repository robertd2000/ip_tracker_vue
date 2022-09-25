<script setup>
const props = defineProps({
  ipInfo: {
    address: String,
    state: String,
    timezone: String,
    isp: String,
    lat: String,
    lng: String,
  },
})
</script>

<template>
  <div class="wrapper">
    <div class="item">
      <h3>IP Address</h3>
      <span>{{ ipInfo.address }}</span>
    </div>

    <div class="item">
      <h3>Location</h3>
      <span>{{ ipInfo.state }}</span>
    </div>

    <div class="item">
      <h3>Timezone</h3>
      <span>UTC {{ ipInfo.timezone }}</span>
    </div>

    <div class="item">
      <h3>Isp</h3>
      <span class="capitalize">{{ ipInfo.isp }} </span>
    </div>
  </div>
</template>

<style lang="scss">
.wrapper {
  @apply absolute -bottom-64 lg:-bottom-20 flex flex-col lg:flex-row bg-white gap-y-4 
            gap-x-12 px-8 pt-8 pb-12 rounded-md;

  .item {
    @apply flex flex-col;

    > h3 {
      @apply text-xs mb-2 uppercase;
    }

    > span {
      @apply text-2xl;
    }
  }
}
</style>
