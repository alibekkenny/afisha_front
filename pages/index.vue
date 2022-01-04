<template>
  <div :class="screenMode">
    <div class="px-4 py-3 w-full inline-block bg-blue-500 text-white">
      <img
        class="h-10 inline-block mr-2"
        src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/i/b460938b-d443-4b2a-baad-afce23c05d19/d6mq0v4-be5ffe1e-a569-45b3-b3b9-fe9deda15f73.png/v1/fill/w_784,h_1003,strp/_attack_on_titan__survey_corps_logo____ai_file__by_king_of_craziness_d6mq0v4-fullview.png"
        alt="logotype"
      />
      <span class="inline-block">Afisha</span>
    </div>

    <div class="px-10 py-4 flex w-full">
      <div class="relative text-gray-600">
        <input
          placeholder="Поиск"
          class="bg-white h-10 px-5 pr-10 rounded-full"
          v-model="searchbar"
        />
        <button @click="filterQuery()" class="absolute right-0 mt-2 mr-4">
          Найти
        </button>
      </div>
    </div>
    <div class="w-full inline-block">
      <div class="float-right mr-10">
        <select
          @change="changed()"
          class="form-select px-4 py-3 rounded-full bg-white"
          v-model="key"
        >
          <option value="1">По умолчанию</option>
          <option value="2">По убыванию рейтинга</option>
          <option value="3">По возрастанию рейтинга</option>
        </select>
      </div>
    </div>
    <div class="p-10 grid grid-cols-4 gap-10">
      <div
        class="h-auto bg-white rounded shadow-lg"
        v-for="afisha in afishaAll"
        :key="afisha.id"
      >
        <img class="w-full h-3/4" :src="afisha.image_path" alt="Poster" />
        <div class="px-6 h-1/6">
          <div class="mt-2 font-bold text-2xl mb-2">{{ afisha.title }}</div>
          <p class="text-gray-700">Длительность: {{ afisha.duration }}</p>
          <p class="text-gray-700">Оценка: {{ afisha.rate }}</p>
          <p class="text-gray-700">Жанры: {{ afisha.genres }}</p>
        </div>
        <Nuxt-Link :to="'/info/' + afisha.id">
          <button
            class="
              bg-blue-500
              text-white
              px-5
              py-2
              float-right
              rounded
              mr-4
              mt-2
            "
          >
            Подробнее
          </button>
        </Nuxt-Link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  async mounted() {
    const { data } = await axios.get("http://localhost:8080/afisha");
    this.afishaAll = data;
    console.log(this.afishaAll);
  },
  data: () => ({
    searchbar: "",
    key: 1,
    afishaAll: {},
    screenMode: "bg-gray-100 h-auto",
  }),
  methods: {
    async changed() {
      if (this.key == 1) {
        const { data } = await axios.get("http://localhost:8080/afisha");
        this.afishaAll = data;
      } else if (this.key == 2) {
        const { data } = await axios.get("http://localhost:8080/afisha");
        data.sort((a, b) => a.rate < b.rate);
        this.afishaAll = data;
      } else if (this.key == 3) {
        const { data } = await axios.get("http://localhost:8080/afisha");
        data.sort((a, b) => a.rate > b.rate);
        this.afishaAll = data;
      }
    },
    async filterQuery() {
      const { data } = await axios.get("http://localhost:8080/afisha");
      var sorted = [];
      data.forEach((x) => {
        if (
          x.title.toLowerCase().includes(this.searchbar.toLowerCase()) ||
          x.genres.toLowerCase().includes(this.searchbar.toLowerCase())
        ) {
          sorted.push(x);
        }
      });
      this.afishaAll = sorted;
      if (this.afishaAll.length == 0) {
        this.screenMode = "bg-gray-100 h-screen";
      } else {
        this.screenMode = "bg-gray-100 h-auto";
      }
    },
  },
};
</script>
