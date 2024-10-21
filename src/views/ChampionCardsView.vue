<template>
  <div class="row row-cols-1 row-cols-md-3 row-cols-lg-4 row-cols-xl-5 row-cols-xxl-6 g-4">
    <div class="col" v-for="(champ, i) of filteredChampions" :key="i">
      <ChampionCard :id="champ.id">
        <template v-slot:image>
          <img :src="champ.image" class="card-img-top" :alt="champ.name" />
        </template>
        <template v-slot:name>
          <div class="d-flex justify-content-between">
            <h5 class="card-title" v-html="keresJelol(champ.name)"></h5>
            <button data-bs-toggle="modal" data-bs-target="#exampleModal" type="button" class="btn" @click="this.selectedChampion = champ"><i class="bi bi-three-dots"></i></button>
          </div>
        </template>
      </ChampionCard>
    </div>
  </div>

  <ChampionModal>
    <template v-slot:name>
      <h1
        class="modal-title fs-5"
        id="exampleModalLabel"
        v-html="keresJelol(this.selectedChampion.name)"
      ></h1>
    </template>

    <template v-slot:image>
      <img :src="this.selectedChampion.image" class="img-fluid my-img" alt="" />
    </template>

    <template v-slot:title>
      <p class="modal-champ-name-p" v-html="keresJelol(this.selectedChampion.title)"></p>
    </template>

    <template v-slot:quote>
      <span v-html="keresJelol(this.selectedChampion.quote)"></span>
    </template>
  </ChampionModal>

</template>

<script>
import ChampionCard from "@/components/ChampionCard.vue";
import ChampionModal from "@/components/ChampionModal.vue";
export default {
  props:['filteredChampions'],
  components: { ChampionCard, ChampionModal },
  inject: ["searchWord"],
  data() {
    return {
      selectedChampion: {},
    };
  },
  methods: {
    keresJelol(text) {
      if (this.searchWord) {
        let what = new RegExp(this.searchWord, "gi");
        if (text) {
          text = text.replace(what, (match) => {
            return `<span class="mark p-0">${match}</span>`;
          });
        }
        return text;
      } else {
        return text;
      }
    },
  },

}
</script>

<style>
.my-img{
  border-radius: 20px;
  border: solid 2px var(--text-color);
}

.modal-my-img{
  border-radius: 10px;
  border: 5px solid var(--text-color);
  max-height: 500px;
}

.modal-champ-name-p {
  font-style: italic;
  font-weight: 700;
  font-size: x-large;
  color: var(--text-black-700);
  margin-top: 5px;
}

.mark {
  background-color: lightyellow !important;
}
</style>