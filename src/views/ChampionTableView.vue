<template>
  <div v-if="filteredChampions && filteredChampions.length > 0" class="row">
    <!-- Gomb -->
    <div class="list-toggle-button-div">
      <i
        class="bi list-toggle-icon bi-list-nested"
        data-bs-toggle="offcanvas"
        data-bs-target="#offcanvasTable"
      ></i>

      <!-- Offcanvas -->
      <div
        class="offcanvas offcanvas-start"
        tabindex="-1"
        id="offcanvasTable"
        aria-labelledby="offcanvasTableLabel"
      >
        <div class="d-flex justify-content-between m-2">
          <i
            class="bi offcanvas-close-bi bi-x-lg"
            data-bs-dismiss="offcanvas"
          ></i>
          <h2>Champions Table</h2>
        </div>
        <div class="offcanvas-body">
          <!-- Táblázat -->
          <table class="table">
            <thead>
              <tr class="my-tr-thead text-center">
                <th>Name</th>
                <th>Class</th>
                <th>Title</th>
              </tr>
            </thead>
            <tbody>
              <tr
                class="my-tr-body"
                v-for="(champ, i) of filteredChampions"
                :key="i"
                @click="setSelectedChampion(champ)"
                :class="{ 'selected-champ-tr': champ === selectedChampion }"
              >
                <td v-html="keresJelol(champ.name)"></td>
                <td v-html="keresJelol(mergeClasses(champ.role))"></td>
                <td v-html="keresJelol(champ.title)"></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <!-- Táblázat -->
    <div class="col-lg-5 table-container">
      <table class="table">
        <thead>
          <tr class="my-tr-thead text-center">
            <th>Name</th>
            <th>Class</th>
            <th>Title</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="my-tr-body"
            v-for="(champ, i) of filteredChampions"
            :key="i"
            @click="setSelectedChampion(champ)"
            :class="{ 'selected-champ-tr': champ === selectedChampion }"
          >
            <td v-html="keresJelol(champ.name)"></td>
            <td v-html="keresJelol(mergeClasses(champ.role))"></td>
            <td v-html="keresJelol(champ.title)"></td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Leírás -->
    <div class="col-lg-7 sticky-description">
      <div class="d-flex justify-content-center column-layout">
        <h1 class="my-champ-h1">{{ selectedChampion.name }}</h1>
        <span class="my-champ-span">{{ selectedChampion.title }}</span>
        <div class="image-container mt-3">
          <img
            class="img-fluid my-champ-img"
            :src="selectedChampion.image"
            alt=""
          />
          <span class="hover-text mt-5">{{ this.selectedChampion.quote }}</span>
        </div>
      </div>
    </div>
  </div>

  <!-- Ha nincs bajnok -->
  <div v-if="filteredChampions === null || filteredChampions.length === 0">
    <p class="no-champion-text">Nincs ilyen hős az adatbázisban</p>
  </div>
</template>


<script>
class Champion {
  constructor(
    id = null,
    name = null,
    image = null,
    role = null,
    title = null,
    quote = null
  ) {
    (this.id = id),
      (this.name = name),
      (this.image = image),
      (this.role = role),
      (this.title = title),
      (this.quote = quote);
  }
}
export default {
  props: ["filteredChampions"],
  inject: ["searchWord"],
  data() {
    return {
      selectedChampion: new Champion(),
    };
  },
  watch: {
    filteredChampions(data) {
      this.selectedChampion = data[0];
    },
  },
  mounted() {
    if (this.filteredChampions.length > 0) {
      this.selectedChampion = this.filteredChampions[0];
    }
  },
  methods: {
    mergeClasses(classes) {
      if (Array.isArray(classes)) {
        return classes.join(", ");
      } else {
        return classes;
      }
    },
    setSelectedChampion(champion) {
      this.selectedChampion = champion;
    },
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
};
</script>

<style scoped>
.no-champion-text {
  font-size: 1.5rem;
  text-align: center;
  color: var(--text-color);
  margin-top: 50px;
  font-weight: bold;
}

.offcanvas-close-bi {
  font-size: x-large;
  color: var(--text-black-700);
}

.offcanvas-close-bi:hover {
  transition: ease 0.3s;
  color: var(--text-color);
  cursor: pointer;
}

.offcanvas {
  background: var(--bg-black-50) !important;
}

.list-toggle-icon {
  font-size: xx-large;
  color: var(--text-black-700);
}

.list-toggle-icon:hover {
  transition: color 0.3s;
  color: var(--text-color);
  cursor: pointer;
}

.list-toggle-button-div {
  display: block;
}

.table-container {
  display: none;
}

@media (min-width: 992px) {
  .table-container {
    display: block;
  }
  .list-toggle-button-div {
    display: none;
  }
}

.hover-text {
  width: 75%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: var(--text-color);
  font-weight: bold;
  opacity: 0;
  transition: opacity 0.3s ease;
  font-style: italic;
  font-size: large;
}

.image-container {
  display: flex;
  justify-content: center;
}

.image-container:hover .my-champ-img {
  filter: blur(25px) grayscale(100%) brightness(60%);
}

.image-container:hover .hover-text {
  opacity: 1;
}

.my-champ-img {
  width: 100%;
  max-width: 990px;
  border-radius: 10px;
  border: solid 2px var(--text-color);
}

.my-champ-h1 {
  font-size: 3rem;
}

.my-champ-span {
  color: var(--bg-black-100);
  font-size: x-large;
  font-style: italic;
  font-weight: 700;
}

.sticky-description {
  position: sticky;
  top: 108px;
  height: fit-content;
}

.my-champ-img {
  max-width: 90%;
}

.column-layout {
  flex-direction: column;
  align-items: center;
}

.my-tr-thead th {
  font-size: x-large;
  color: var(--text-color) !important;
  transition: ease 0.5s;
}

.my-tr-body td {
  color: var(--text-black-700);
}

.my-tr-body:hover td {
  background: var(--text-color);
  filter: grayscale(0.4);
  transition: ease 0.2s;
  cursor: pointer;
}

.table {
  border: var(--text-color-);
  border-radius: 20px;
  overflow: hidden;
}

.my-tr-body {
  border: var(--text-color);
  transition: ease 0.5s;
}

.table > :not(caption) > * > * {
  background: var(--bg-black-100);
}

.selected-champ-tr td {
  background: var(--text-color);
  transition: ease 0.3s;
}
</style>
