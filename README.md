# League of Legends Champion Viewer

## Általános leírás
Ez az alkalmazás a League of Legends bajnokait jeleníti meg különböző nézetekben, mint például táblázat és kártya formátumban. A felhasználók kereshetnek és szűrhetnek a bajnokok között, valamint témát választhatnak az alkalmazás megjelenésének testreszabásához.

## Milyen technológiát használunk
- **Backend**: Nincs külön backend, az adatok közvetlenül egy külső API-ból kerülnek betöltésre.
- **Frontend**: Vue.js
- **Dizájn**: Bootstrap
- **Adatbázis**: Nincs adatbázis, az adatok az [League of Legends API](https://developer.riotgames.com/) segítségével kerülnek betöltésre.

## Menüpontok, funkciók
### Home
- Üdvözlő oldal, általános információk.

### Táblázat
- A bajnokok listázása táblázatos formában, ahol a felhasználók megtekinthetik a bajnokok nevét, szerepkörét és címét.

### Kártyák
- A bajnokok bemutatása kártyák formájában, ahol képek és alapvető információk találhatók.

### Keresés
- A bajnokok közötti keresés lehetősége.

### Szűrés
- A bajnokok szűrése különböző attribútumok alapján.

## Adatforrás
- A táblázat a League of Legends bajnokainak adatait tartalmazza.
- Az oszlopok: név, szerepkör, cím.
- Az adatok az alábbi API-ból érhetők el: `https://ddragon.leagueoflegends.com/cdn/12.23.1/data/hu_HU/champion.json`.

### Kódrészlet a hősök lekérdezéséről
```js
async getChampions() {
  try {
    const response = await fetch("https://ddragon.leagueoflegends.com/cdn/12.23.1/data/hu_HU/champion.json");
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
}
```

## A program részletezése
### Könyvtár és állomány szerkezet, modulok
- **src/**: Fő mappa, ahol az összes komponens található.
  - **components/**: Újrahasználható Vue komponensek.
  - **views/**: Az alkalmazás nézetei.
  - **router/**: Az útvonalak kezelése.
  - **assets/**: Statikus fájlok, mint például stíluslapok.

### Home megvalósítása
- A `Home.vue` komponens bemutatja a fő oldalt, amely üdvözli a felhasználót.

### Táblázat megvalósítása
- A bajnokok gyors áttekintésére és keresésére szolgál.

### Kártyák
- A bajnokok kártyák formájában való megjelenítése, lehetővé téve a gyors áttekintést.

### Keresés
- A keresés a bajnokok neve, szerepköre, címe és leírása alapján történik.

### Szűrés
- A bajnokok szűrése a kiválasztott szerepkörök szerint.
