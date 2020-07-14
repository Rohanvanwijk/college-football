<template>
    <div class="game">
        <h1>{{getTeam}}</h1>
        <div class="game-container" v-if="games">
            <div class="game-item" v-for="game in games" :key="game.id">
                <h3>{{game.home_team}} - {{game.away_team}}</h3>
                <label>Season: {{game.season}}</label>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios'

export default {
    name: "Game",
    computed: {
        getTeam() {
            return this.$route.params.team;
        },
        getYear() {
            const date = new Date();
            const year = date.getFullYear();
            return year;
        }
    },
    data: function() {
        return {
            games: null
        }
    },
    mounted: function() {
    axios
    .get("https://api.collegefootballdata.com/games", { params: {
        team: this.getTeam,
        year: this.getYear
    }})
    .then(response => (this.games = response.data))
  }
  }
</script>
<style lang="scss" scoped>
.game-container {
    display: flex;
    flex-wrap: wrap;
    max-width: 800px;
    margin: 0 auto;
    justify-content: center;
}

.game-item {
    padding: 10px;
    margin: 10px;
    border: 1px dashed antiquewhite;
}
</style>