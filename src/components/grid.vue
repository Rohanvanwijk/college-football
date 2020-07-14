<template>
    <div class="grid">
        <h1>All teams</h1>
        <h3>Search</h3>
        <input v-model="search">
        <button @click="searchTeam">Search</button>
        <div class="grid__view" v-if="!searchedTeams && pagedTeams">
                <div class="grid__item" v-for="team in pagedTeams[currentPage]" :key="team.id">
                    <h2>{{team.school}}</h2>
                    <router-link :to="{ name: 'team', params: { team: team.school }}">View game</router-link>
                </div>
        </div>
        <div v-else>
            <div class="grid__view">
                <div class="grid__item" v-for="team in searchedTeams" :key="team.id">
                    <h2>{{team.school}}</h2>
                    <router-link :to="{ name: 'team', params: { team: team.school }}">View game</router-link>
                </div>
            </div>
        </div>
        <button @click="currentPage--">Previous</button>
        <span class="page">{{ currentPage }}</span>
        <button @click="currentPage++">Next</button>
    </div>
</template>
<script>
import axios from 'axios'

export default {
    name: "Grid",
    data: () => {
        return {
            teams: null,
            searchedTeams: null,
            pagedTeams: null,
            search: "",
            currentPage: 0
        }
    },
    methods: {
        searchTeam: function() {
            let searchedTeams = []
            for(let i = 0; i < this.teams.length; i++) {
                const teamName = this.teams[i].school.toLowerCase()
                if(teamName.includes(this.search)) {
                    searchedTeams.push(this.teams[i])
                }
            }
            this.searchedTeams = searchedTeams;
        }
    },
    mounted: function() {
    axios
    .get("https://api.collegefootballdata.com/teams")
    .then((response) => {
        this.teams = response.data;

        let size = 20; 
        let arrayOfArrays = [];
        for (let i=0; i<response.data.length; i+=size) {
            arrayOfArrays.push(response.data.slice(i,i+size));
        }
        this.pagedTeams = arrayOfArrays;
        
    })
    .catch(function(error){
        console.log(error);
    });
  }
}
</script>
<style lang="scss" scoped>
.grid {
    &__view {
    max-width: 800px;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    }
    &__item {
    background-color: antiquewhite;
    padding: 10px;
    margin: 10px;
    }
}
a {
    color: #42b983;
}
.page {
    padding: 0 10px;
}
</style>