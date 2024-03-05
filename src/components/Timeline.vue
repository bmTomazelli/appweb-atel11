<script setup lang="ts">
import{ DateTime } from "luxon";
import { TimelinePost, today,thisWeek,thisMonth} from "../posts";
import {ref, computed} from "vue";
import TimelineItem from "./TimelineItem.vue";

const periods= ["Aujourd'hui", "Cette semaine", "Ce mois"] as const 

const posts = computed<TimelinePost[]>(() => { 
return [today, thisWeek, thisMonth]
    .map(post => {
        return {
            ...post,
            created: DateTime.fromISO(post.created)
        }
    })
    .filter(post => {
        if (selectedPeriod.value === "Aujourd'hui") {
            return post.created >= DateTime.now().minus({ day: 1 })
        }
        if(selectedPeriod.value ==="Cette semaine"){
            return post.created >= DateTime.now().minus({ week:1 })
        }

        return post
    })
})  

type Period = typeof periods[number]


let selectedPeriod = ref("Aujourd'hui")

function selectPeriod(period: Period) {
    selectedPeriod.value = period
    console.log(period) //on va logguer pour tester
}
</script>

<template>  
<div class="btn-group" role="group">
<button type="button" class="btn btn-primary"
    v-for="period of periods" 
        v-bind:key="period" 
        v-on:click="selectPeriod(period)" 
        v-bind:class="{ 'btn-success': period === selectedPeriod}"
>
    {{ period }}
</button>
</div>
<h2>{{ selectedPeriod }}</h2>
<ul>
<TimelineItem v-for="post of posts" :key="post.id" :post="post"/>
</ul>
</template>

<style scoped> 
</style>