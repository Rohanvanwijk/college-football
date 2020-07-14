<template>
    <div class="game">
        <h1>{{getTeam}}</h1>
        <div class="game__container" v-if="games">
            <div class="game__item" v-for="game in games" :key="game.id">
                <h3 v-if="!game.attendance">{{game.home_team}} - {{game.away_team}}</h3>
                <h3 v-else>{{game.home_team}} ({{game.home_points}}) - {{game.away_team}} ({{game.away_points}})</h3>
                <span class="game__highlight">Season: {{game.season}}</span>
                <div class="game__details" v-if="game.attendance">
                    <p>Venue: {{game.venue}}</p>
                    <p>Attendance: {{game.attendance}}</p>
                    <p>Season type: {{game.season_type}}</p>
                    <p>Excitement index: {{game.excitement_index}}</p>
                    <p>Start date: {{game.start_date}}</p>
                </div>
            </div>
        </div>
        <div v-else>
            No game found
        </div>
        <button @click="getGames(false)" class="button">View previous year</button>
        <button @click="getGames(true)" class="button" v-if="this.activeYear != this.getCurrentYear">View next year</button>
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
        getCurrentYear() {
            return new Date().getFullYear();
        }
    },
    data: function() {
        return {
            games: null,
            activeYear: 0
        }
    },
    methods: {
        getGames: function(forward) {
            if(forward){
                this.activeYear = this.activeYear + 1;
            } else {
                this.activeYear = this.activeYear - 1;
            }
            axios
            .get("https://api.collegefootballdata.com/games", { params: {
             team: this.getTeam,
             year: this.activeYear
            }})
            .then(response => (this.games = response.data))
            .catch((error) => {console.log(error)});
        }
    },
    mounted: function() {
        this.activeYear = new Date().getFullYear();
        axios
        .get("https://api.collegefootballdata.com/games", { params: {
            team: this.getTeam,
            year: this.activeYear
        }})
        .then(response => (this.games = response.data))
        .catch((error) => {console.log(error)});
    
  }
  }
</script>
<style lang="scss" scoped>
.game {
    &__container {
    display: flex;
    flex-wrap: wrap;
    max-width: 800px;
    margin: 0 auto;
    justify-content: center;
    }
    &__item {
    padding: 10px;
    margin: 10px;
    border: 1px dashed antiquewhite;
    }
    &__details {
        border-top: 1px solid antiquewhite;
    }
    &__highlight {
        font-weight: bold;
        color: #42b983;
        margin: 10px 0;
        display: block;
    }
}

.button {
    padding: 10px;
    margin: 20px;
    background-color: bisque;
    border: none;
    font-weight: bold;
    &:hover {
        cursor: pointer;
    }
}
</style>