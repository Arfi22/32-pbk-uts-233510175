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

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #1a1a2e;
  color: #e6e6e6;
}

.game-collection {
  max-width: 700px;
  margin: 2rem auto;
  padding: 2rem;
  background: #16213e;
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.app-header {
  text-align: center;
  margin-bottom: 2rem;
  color: #f1c40f;
}

.app-header h1 {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.app-header p {
  color: #bdc3c7;
  font-size: 1.1rem;
}

.game-input-container {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.game-input {
  flex: 2;
  padding: 0.8rem 1rem;
  background: #0f3460;
  border: 2px solid #2c3e50;
  border-radius: 8px;
  font-size: 1rem;
  color: white;
  transition: border 0.3s;
}

.game-input:focus {
  outline: none;
  border-color: #f1c40f;
}

.platform-select {
  flex: 1;
  padding: 0.8rem 0.5rem;
  background: #0f3460;
  border: 2px solid #2c3e50;
  border-radius: 8px;
  color: white;
  cursor: pointer;
}

.add-btn {
  padding: 0 1.5rem;
  background: #f39c12;
  color: #1a1a2e;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s;
}

.add-btn:hover {
  background: #f1c40f;
}

.filter-options {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.filter-options button {
  padding: 0.5rem 1rem;
  background: #2c3e50;
  border: none;
  border-radius: 6px;
  color: #ecf0f1;
  cursor: pointer;
  transition: all 0.3s;
}

.filter-options button.active {
  background: #f39c12;
  color: #1a1a2e;
  font-weight: bold;
}

.game-list {
  list-style: none;
  margin-bottom: 1.5rem;
}

.game-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #0f3460;
  border-radius: 8px;
  margin-bottom: 0.75rem;
  transition: all 0.3s;
}

.game-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.game-content {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
}

.game-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.game-text {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.game-title {
  font-weight: 500;
}

.game-platform {
  font-size: 0.8rem;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
  margin-top: 0.2rem;
  align-self: flex-start;
}

.game-platform.pc {
  background: #3498db;
}
.game-platform.playstation {
  background: #003791;
}
.game-platform.xbox {
  background: #107c10;
}
.game-platform.nintendo {
  background: #e60012;
}
.game-platform.mobile {
  background: #8e44ad;
}

.completed .game-title {
  text-decoration: line-through;
  color: #95a5a6;
}

.delete-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  color: #e74c3c;
  opacity: 0.7;
  transition: opacity 0.3s;
  margin-left: 0.5rem;
}

.delete-btn:hover {
  opacity: 1;
}

.empty-state {
  text-align: center;
  padding: 2rem;
  color: #7f8c8d;
}

.empty-state .emoji {
  font-size: 1.5rem;
  margin-top: 0.5rem;
}

.stats {
  text-align: center;
  color: #f1c40f;
  font-size: 0.9rem;
  padding-top: 1rem;
  border-top: 1px solid #2c3e50;
}
</style>
