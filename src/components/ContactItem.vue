<template>
  <q-item clickable>
    <q-item-section avatar>
      <q-avatar>
        <img :src="avatar" class="bg-white">
      </q-avatar>
    </q-item-section>

    <q-item-section>
      <q-item-label lines="1" class="text-weight-bold">{{ name }}</q-item-label>
      <q-item-label caption lines="2">
        <span class="text-weight-bold text-grey-5">{{ position }}</span>
      </q-item-label>
    </q-item-section>

    <!-- <q-item-section>
      <q-btn flat label="Cancel" v-close-popup />
    </q-item-section> -->
  </q-item>
</template>

<script>
import {defineComponent} from 'vue';

export default defineComponent({
  name: "ContactItem",
  props: ['avatar', 'name', 'position']
})
</script>

<style scoped>

</style>
