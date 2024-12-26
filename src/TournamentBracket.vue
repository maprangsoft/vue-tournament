<template>
  <div class="vt-wrapper" v-if="recursiveBracket" :style="cssVars">
    <BracketNode
      :bracket-node="recursiveBracket"
      :highlighted-team-id="highlightedTeamId"
      @onSelectedTeam="highlightTeam"
      @onDeselectedTeam="unhighlightTeam"
      @onMatchClick="onMatchClick"
    />
  </div>
</template>

<script setup lang="ts">
//@ts-nocheck
import { ref, computed } from 'vue';
import IRound from "./interface/IRound";
import IBracketNode from "./interface/IBracketNode";
import BracketNode from "./components/BracketNode.vue";
import { courthiveAdaptor } from "./adaptor/courthiveAdaptor";
import { transform as transformBracket } from "./recursiveBracket";

const props = defineProps({
  rounds: { type: Array as () => IRound[] | any, default: () => [] },
  format: { type: String, default: "default" },
  textColor: { type: String, default: "#ffffff" },
  titleColor: { type: String, default: "#9d999d" },
  teamBackgroundColor: { type: String, default: "#59595e" },
  highlightTeamBackgroundColor: { type: String, default: "#222222" },
  scoreBackgroundColor: { type: String, default: "#787a7f" },
  winnerScoreBackgroundColor: { type: String, default: "#ee7b3c" }
});

const emit = defineEmits(['onMatchClick']);

const highlightedTeamId = ref<string | undefined>("");

const recursiveBracket = computed(() => {
  let copyRound = props.rounds;
  if (props.format === "courthive") {
    copyRound = courthiveAdaptor(props.rounds);
  }
  return transformBracket(copyRound);
});

const cssVars = computed(() => ({
  "--text-color": props.textColor,
  "--title-color": props.titleColor,
  "--team-background-color": props.teamBackgroundColor,
  "--highlight-team-background-color": props.highlightTeamBackgroundColor,
  "--score-background-color": props.scoreBackgroundColor,
  "--winner-score-background-color": props.winnerScoreBackgroundColor,
}));

const onMatchClick = (matchId: string | number): void => {
  emit('onMatchClick', matchId);
};

const highlightTeam = (teamId: string): void => {
  highlightedTeamId.value = teamId;
};

const unhighlightTeam = (): void => {
  highlightedTeamId.value = undefined;
};
</script>

<style>
.vt-wrapper {
  display: flex;
}

.vt-item-teams,
.vt-item-feedIn {
  color: var(--text-color);
}

.vt-item-teams .title,
.vt-item-feedIn .title {
  color: var(--title-color);
}

.vt-item-teams .vt-team,
.vt-item-feedIn .vt-feedIn {
  background-color: var(--team-background-color);
}

.vt-item-teams .vt-team .score {
  background-color: var(--score-background-color);
}

.vt-item-teams .vt-team.winner .score {
  background-color: var(--winner-score-background-color);
}

.vt-item-teams .vt-team.highlight,
.vt-item-feedIn .vt-feedIn.highlight {
  background-color: var(--highlight-team-background-color);
}
</style>