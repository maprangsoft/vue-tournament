<template>
  <TournamentBracket
    :rounds="rounds"
    format="default"
    @onMatchClick="onMatchClick"
  />
</template>

<script setup lang="ts">
//@ts-nocheck
import { ref } from 'vue';
import IRound from "./interface/IRound";
import TournamentBracket from "./TournamentBracket.vue";
import bracketData from "./mockData/singleElimination8WithFeedIn.json";

const rounds = ref<IRound[]>(bracketData);

const onMatchClick = (matchId: string | number): void => {
  alert(`click: ${matchId}`);
};
</script>