<template>
  <div class="game-collection">
    <header class="app-header">
      <h1>🎮 Koleksi Game Saya</h1>
      <p>Kelola daftar game favorit Anda</p>
    </header>

    <div class="app-container">
      <!-- Input untuk menambah game baru -->
      <div class="game-input-container">
        <input
          v-model="newGame"
          @keyup.enter="addGame"
          placeholder="Masukkan judul game..."
          class="game-input"
        />
        <select v-model="newGamePlatform" class="platform-select">
          <option value="PC">PC</option>
          <option value="PlayStation">PlayStation</option>
          <option value="Xbox">Xbox</option>
          <option value="Nintendo">Nintendo</option>
          <option value="Mobile">Mobile</option>
        </select>
        <button @click="addGame" class="add-btn">
          <span>+ Tambah</span>
        </button>
      </div>

      <!-- Filter options -->
      <div class="filter-options">
        <button @click="filter = 'all'" :class="{ active: filter === 'all' }">
          Semua Game
        </button>
        <button
          @click="filter = 'unplayed'"
          :class="{ active: filter === 'unplayed' }"
        >
          Belum Dimainkan
        </button>
        <button
          @click="filter = 'completed'"
          :class="{ active: filter === 'completed' }"
        >
          Selesai
        </button>
      </div>

      <!-- Daftar game -->
      <ul class="game-list" v-if="filteredGames.length > 0">
        <li
          v-for="game in filteredGames"
          :key="game.id"
          class="game-item"
          :class="{ completed: game.completed }"
        >
          <div class="game-content">
            <input
              type="checkbox"
              v-model="game.completed"
              class="game-checkbox"
              @change="updateGameStatus(game)"
            />
            <span class="game-text">
              <span class="game-title">{{ game.title }}</span>
              <span
                class="game-platform"
                :class="getPlatformClass(game.platform)"
              >
                {{ game.platform }}
              </span>
            </span>
          </div>
          <button @click="removeGame(game.id)" class="delete-btn">❌</button>
        </li>
      </ul>

      <div v-else class="empty-state">
        <p>Tidak ada game dalam koleksi. Tambahkan game baru!</p>
        <p class="emoji">🎮🕹️👾</p>
      </div>

      <div class="stats" v-if="games.length > 0">
        {{ completedGamesCount }} dari {{ games.length }} game sudah selesai
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newGame: "",
      newGamePlatform: "PC",
      games: [],
      filter: "all",
    };
  },
  computed: {
    filteredGames() {
      if (this.filter === "completed") {
        return this.games.filter((game) => game.completed);
      } else if (this.filter === "unplayed") {
        return this.games.filter((game) => !game.completed);
      }
      return this.games;
    },
    completedGamesCount() {
      return this.games.filter((game) => game.completed).length;
    },
  },
  methods: {
    addGame() {
      if (this.newGame.trim() === "") return;

      this.games.push({
        id: Date.now(),
        title: this.newGame,
        platform: this.newGamePlatform,
        completed: false,
      });
      this.newGame = "";
    },
    removeGame(id) {
      this.games = this.games.filter((game) => game.id !== id);
    },
    updateGameStatus(game) {
      console.log("Status game diperbarui:", game);
    },
    getPlatformClass(platform) {
      return platform.toLowerCase();
    },
  },
};
</script>

<style></style>
