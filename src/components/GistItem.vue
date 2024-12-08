<script setup lang="ts">
const maxPages = 10;
const gistItems = [];

const fetchGistItems = async () => {
    for(var i=1; i<maxPages; i++){
        const paramsString = `page=${i}`;
        const qParams = new URLSearchParams(paramsString);
        const gistJson = await fetch(`https://api.github.com/users/oYuYo/gists?${qParams}`, {
            method: "GET",
            headers: { 
                Accept: "application/vnd.github+json",
                Authorization: "Bearer: import.meta.env.GH_TOKEN",
                "X-GitHub-Api-Version": "2022-11-28"
            }
        })
        .then((response) => response.json());
        if (gistJson.length == 0){
            break;
        }
        gistItems.push(...gistJson);
    }
};
await fetchGistItems().then(() => {
    console.log(gistItems);
});

</script>
<template>
    <ul>
    <li v-for="item in gistItems">
        <a v-bind:href="item.html_url" target="_blank">
        <div class="item">
            <div class="gist-item">
                <span class="description">{{item.description}}</span>
                <span class="date">{{new Date(item.created_at).toLocaleDateString()}}</span>
            </div>
        </div>
        </a>
    </li>
    </ul>
</template>

<style scoped>
.item {
    display:flex;
    flex-direction: row;
    flex-wrap: nowrap;
    font-size:16px;
    border-left: solid 2rem #333;
    border-radius: 10px;
    width:100%;
    background-color:#F2F2F2;
    color:black;
    overflow: hidden;
    margin:1.5rem 0 0 0;
}
.gist-item {
    padding:2rem 0 2rem 1rem;
    white-space: nowrap;
}
.date{
    padding-left: 1rem;
}
li{
    list-style-type: none;
}
</style>