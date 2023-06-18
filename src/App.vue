<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import { useFetch } from "@vueuse/core";

document.title = "WhoTrain - Find your model trainer.";
const url = ref("/all.json");
const refetch = ref(false);
const { data, error, statusCode, isFetching, isFinished, canAbort } = useFetch(
  url,
  { refetch }
).get();
const text = reactive({
  isFinished,
  isFetching,
  canAbort,
  statusCode,
  error,
  data: computed(() => {
    try {
      return JSON.parse(data.value as string);
    } catch (e) {
      return null;
    }
  }),
});
</script>

<template>
  <div class="creator-archive">
    <img src="/logo.png" width="45" height="45" />
    <span style="font-family: 'Bigshot One'; font-size: 45px; color: #E6E6E6;"> Who</span><span style="font-family: 'Bigshot One'; font-size: 45px; color: #e73154;">Train </span>
    <span style="font-size: 15px; color: #E6E6E6;">Find your model trainer.</span>
  <br>
    <br>
  </div>
  <masonry-wall v-if="text.data" :items="text.data" :ssr-columns="1" :column-width="350" :gap="10">
    <template #default="{ item, index }">
      <div class="grid-item">
        <span style="font-size: 16px;">{{ item.model_name }}</span>
        <span style="float: right; font-size: 12px;"><img src="/green.svg" style="width: 12px;" /> {{ item.model_version }}</span>
        <img :src="`${item.images[0]}`" width="288" />
        <br />
        <a :href="`${item.online[0]}`"><img src="/discord.svg" /></a>&nbsp;
        <a :href="`${item.online[1]}`"><img src="/patreon.svg" /></a>&nbsp;
        <a :href="`${item.online[2]}`"><img src="/twitter.svg" /></a>
        <p>Info: {{ item.creator_username }}</p>
        <!-- <p>Type: {{ item.type[0] }}</p> -->
        <span v-if="item.tokens[0]">Tags: </span>
        <span v-for="(token, token_index) in item.tokens" :key="token_index">
          {{ token }}&nbsp;
        </span>
      </div>
    </template>
  </masonry-wall>
</template>