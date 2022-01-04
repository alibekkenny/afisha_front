<template>
  <div class="bg-gray-100 h-full">
    <div class="px-4 py-3 w-full inline-block bg-blue-500 text-white">
      <img
        class="h-10 inline-block mr-2"
        src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/i/b460938b-d443-4b2a-baad-afce23c05d19/d6mq0v4-be5ffe1e-a569-45b3-b3b9-fe9deda15f73.png/v1/fill/w_784,h_1003,strp/_attack_on_titan__survey_corps_logo____ai_file__by_king_of_craziness_d6mq0v4-fullview.png"
        alt="logotype"
      />
      <span class="inline-block">Afisha</span>
    </div>

    <div class="w-full px-10 mt-10 flex">
      <img
        class="shadow border inline-block w-3/5"
        :src="afishaInfo.image_path"
        alt="product image"
      />
      <div class="bg-white px-10 py-10 ml-10 inline-block w-full text-xl">
        <div class="flex mb-7">
          <h1 class="flex-auto text-4xl font-semibold inline-block">
            {{ afishaInfo.title }}
          </h1>
        </div>
        <div class="h-5/6">
          <div class="mb-2">
            <span class="font-semibold">Длительность:</span>
            <span class="font-semibold text-gray-500">{{
              afishaInfo.duration
            }}</span>
          </div>
          <div class="mb-2 font-semibold">Оценка: {{ afishaInfo.rate }}</div>
          <div class="mb-2">
            <span class="font-semibold">Жанры: </span>{{ afishaInfo.genres }}
          </div>
          <div class="mb-2">
            <span class="font-semibold">Описание: </span>
            {{ afishaInfo.description }}
          </div>
        </div>
        <nuxt-link to="/"
          ><button
            class="mt-12 bg-blue-500 w-1/5 h-10 rounded text-white float-right"
          >
            Назад
          </button></nuxt-link
        >
      </div>
    </div>
    <div class="grid grid-cols-6 gap-10 w-full p-10 flex">
      <div
        class="bg-white flex items-center px-2 py-1 rounded-xl shadow border"
        v-for="actor in afishaInfo.actors"
      >
        <img class="w-20 h-20 rounded-full" :src="actor.actors.image_path" />
        <div class="flex-grow p-3">
          <div class="font-semibold text-gray-700">
            {{ actor.actors.name }}
          </div>
          <div class="text-sm text-gray-500">
            Role:
            <span class="font-semibold text-gray-700">{{
              actor.character
            }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  async mounted() {
    const id = this.$route.params.id;
    const { data } = await axios.get("http://localhost:8080/afisha/" + id);
    this.afishaInfo = data;
    console.log(this.afishaInfo.actors.actors);
  },
  data: () => ({
    searchbar: "",
    key: 1,
    afishaInfo: {},
  }),
};
</script>
