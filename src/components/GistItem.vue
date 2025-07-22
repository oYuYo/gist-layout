<script setup lang="ts">
import { ref, watch } from 'vue';
const maxPages: number = 10;
const gistItems: any[] = [];
const filterItems = ref([] as any[]);

const props = defineProps<{
    keyword?: string;
}>();

const fetchGistItems = async () => {
    for(var i=1; i<maxPages; i++){
        const paramsString: string = `page=${i}`;
        const qParams = new URLSearchParams(paramsString);
        const gistJson = await fetch(`https://api.github.com/users/oYuYo/gists?${qParams}`, {
            method: "GET",
            headers: { 
                Accept: "application/vnd.github+json",
                Authorization: `Bearer: ${import.meta.env.VITE_GH_TOKEN}`,
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

watch(() => props.keyword, (newVal, oldVal) => {
    filterItems.value.length = 0;
    if (newVal!.trim().length === 0) {
        filterItems.value = [...gistItems];
    }
    else{
        let tmp = gistItems.filter(item => item.description.toLowerCase().includes(newVal!.toLowerCase()));
        filterItems.value = [...tmp];
    }
}, { immediate: true });
</script>
<template>
    <ul>
        <li v-for="item in filterItems" :key="item.id">
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
ul{
    margin: 10px;
}
li{
    list-style-type: none;
}
</style>