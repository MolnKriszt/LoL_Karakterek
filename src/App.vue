<script setup>
import { RouterView } from "vue-router";
import { ref, onMounted, computed } from "vue";

const activeTheme = ref("theme-green");

const setActiveTheme = (themeId, color) => {
  document.documentElement.style.setProperty("--text-color", color);
  activeTheme.value = themeId;
  localStorage.setItem("themeColor", color);
  localStorage.setItem("themeId", themeId);
};

onMounted(() => {
  const savedColor = localStorage.getItem("themeColor");
  const savedThemeId = localStorage.getItem("themeId");

  if (savedColor && savedThemeId) {
    document.documentElement.style.setProperty("--text-color", savedColor);
    activeTheme.value = savedThemeId;
  }
});
</script>

<template>
  <div class="container-fluid p-0">
    <nav class="d-flex justify-content-between align-items-center">
      <!-- Navbar gombok -->
      <ul class="nav justify-content-center mb-4">
        <li>
          <RouterLink
            :to="{ path: '/' }"
            :class="{ active: $route.path === '/' }"
          >
            Home
          </RouterLink>
        </li>

        <li>
          <RouterLink
            :to="{ path: '/table' }"
            :class="{ active: $route.path === '/table' }"
          >
            Table
          </RouterLink>
        </li>

        <li>
          <RouterLink
            :to="{ path: '/cards' }"
            :class="{ active: $route.path === '/cards' }"
          >
            Cards
          </RouterLink>
        </li>

        <!-- Téma választás gombok -->
        <div class="theme-options">
          <div
            id="theme-green"
            :class="{ active: activeTheme === 'theme-green' }"
            @click="setActiveTheme('theme-green', '#37b18c')"
          ></div>
          <div
            id="theme-purple"
            :class="{ active: activeTheme === 'theme-purple' }"
            @click="setActiveTheme('theme-purple', '#9326ff')"
          ></div>
          <div
            id="theme-red"
            :class="{ active: activeTheme === 'theme-red' }"
            @click="setActiveTheme('theme-red', '#b14537')"
          ></div>
        </div>
      </ul>

      <!-- Kereső -->

      <div class="d-flex align-items-center pb-3 pe-3" role="search">
        
        <input
        v-if="$route.path != '/'"
        class="form-control me-4"
        type="search"
        placeholder="Search"
        aria-label="Search"
        v-model="searchWordInput"
        />

        <i
          class="bi filter-bi bi-funnel"
          data-bs-toggle="offcanvas"
          data-bs-target="#offcanvasFilter"
        ></i>
      </div>
    </nav>

    <div class="content-area p-4">
      <RouterView :filteredChampions="filteredChampions" />
    </div>
  </div>

  <!-- Filter Offcanvas -->

  <div
    class="offcanvas offcanvas-end"
    tabindex="-1"
    id="offcanvasFilter"
    aria-labelledby="offcanvasFilterLabel"
  >  
  <div class="d-flex justify-content-between m-2">
      <i
        class="bi offcanvas-close-bi bi-x-lg text-start"
        data-bs-dismiss="offcanvas"
      ></i>
      <h2 class="offcanvas-title" id="offcanvasFilterLabel">Filter</h2>
    </div>
    <div class="offcanvas-body"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchWord: null,
      searchWordInput: null,
      champions: [],
    };
  },
  provide() {
    return {
      searchWord: computed(() => this.searchWord),
    };
  },
  async mounted() {
    await this.getChampions();
    console.log(this.filteredChampions);
  },
  watch: {
    searchWordInput(data) {
      if (!data) {
        this.searchWord = null;
      }
      this.searchWord = data;
    },
  },
  methods: {
    async getChampions() {
      try {
        const response = await fetch(
          "https://ddragon.leagueoflegends.com/cdn/14.20.1/data/hu_HU/champion.json"
        );
        const data = await response.json();
        const champData = data.data;

        this.champions = Object.values(champData).map((champ) => ({
          id: champ.key,
          name: champ.name,
          image: `https://ddragon.leagueoflegends.com/cdn/img/champion/splash/${champ.id}_0.jpg`,
          role: champ.tags,
          title: champ.title,
          quote: champ.blurb,
        }));
      } catch (error) {
        console.log("Error:", error);
      }
    },
  },
  computed: {
    filteredChampions() {
      if (!this.searchWord) {
        return this.champions;
      }
      return this.champions.filter((c) => {
        return (
          c.name.toLowerCase().includes(this.searchWord.toLowerCase()) ||
          c.role.some((r) =>
            r.toLowerCase().includes(this.searchWord.toLowerCase())
          ) ||
          c.title.toLowerCase().includes(this.searchWord.toLowerCase()) ||
          c.quote.toLowerCase().includes(this.searchWord.toLowerCase())
        );
      });
    },
  },
};
</script>

<style>
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

.filter-bi {
  margin-right: 10px;
  font-size: xx-large;
  color: var(--text-black-700);
}

.filter-bi:hover {
  transition: ease 0.3s;
  color: var(--text-color);
  cursor: pointer;
}

h1,
h2,
h3,
h4,
h5 {
  color: var(--text-color);
}

.white {
  color: #fff;
}

.green {
  color: var(--text-color);
  font-weight: 700;
}

nav {
  background: var(--bg-black-100);
  padding-top: 20px;
  position: sticky; /* Sticky position */
  top: 0; /* Sticky top */
  z-index: 1000; /* Ensure it stays on top of other content */
}

.nav li a {
  font-size: 16px;
  font-weight: 600;
  display: block;
  color: var(--text-black-700);
  padding: 5px 15px;
  text-decoration: none;
  transition: 0.5s;
}

.nav li a:hover {
  color: var(--text-color);
}

.nav li a.active,
.green {
  color: var(--text-color);
  font-weight: 700;
}

.my-bi {
  font-size: 2rem;
  color: var(--text-color);
}

.my-nav-text {
  font-size: 2rem;
  color: var(--text-color);
}

.my-navbar {
  background: var(--bg-black-100) !important;
}

body {
  background: var(--bg-black-50);
  height: 100%;
}

.btn {
  background: transparent;
  border: 1px solid var(--text-color);
  color: #fff;
  font-weight: 700;
  transition: background 0.3s ease, transform 0.3s ease, border 0.3s ease !important;
}

.btn:hover {
  background: var(--text-color);
  transform: scale(1.05);
}

.theme-options {
  position: relative;
  margin-top: 10px;
}

.theme-options div {
  cursor: pointer;
  width: 20px;
  height: 20px;
  border-radius: 4px;
  margin: 2px 10px;
  display: inline-block;
  opacity: 0.3;
  padding: 10px;
  transition: all 0.3s;
}

.theme-options div.active {
  opacity: 1;
}

#theme-green {
  background-color: #37b18c;
}

#theme-purple {
  background-color: #9326ff;
}

#theme-red {
  background-color: #b14537;
}

@media only screen and (max-width: 600px) {
  .theme-options {
    position: relative;
    margin: 0;
  }
}
</style>
