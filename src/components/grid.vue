<template>
    <div class="grid">
        <h1>All teams</h1>
        <div class="grid-view">
            <div class="grid-item" v-for="team in teams" :key="team.id">
            <h2>{{team.school}}</h2>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios'

export default {
    name: "grid",
    data: function() {
        return {
            teams: null,
        }
    },
    mounted: function() {
    axios
    .get("https://api.collegefootballdata.com/teams")
    .then(response => (this.teams = response.data))
  }
}
</script>
<style lang="scss" scoped>
.grid-view {
    max-width: 800px;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
}
.grid-item {
    background-color: antiquewhite;
    padding: 10px;
    margin: 10px;
}
</style>