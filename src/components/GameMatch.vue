<template>
  <div class="vt-item-teams">
    <div>
      <div class="title">
        <span>{{ bracketNode.match.title }}</span>
      </div>
      <div
        :class="[
          'vt-team',
          'vt-team-top',
          getPlayerClass(bracketNode.match.team1),
        ]"
        @mouseover="highlightTeam(bracketNode.match.team1.id)"
        @mouseleave="unhighlightTeam"
        @click="onClick"
      >
        <span class="name">{{ bracketNode.match.team1.name }}</span>
        <span class="score">{{ bracketNode.match.team1.score }}</span>
      </div>

      <div
        :class="[
          'vt-team',
          'vt-team-bot',
          getPlayerClass(bracketNode.match.team2),
        ]"
        @mouseover="highlightTeam(bracketNode.match.team2.id)"
        @mouseleave="unhighlightTeam"
        @click="onClick"
      >
        <span class="name">{{ bracketNode.match.team2.name }}</span>
        <span class="score">{{ bracketNode.match.team2.score }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import ITeam from "@/interface/ITeam";
import IBracketNode from "../interface/IBracketNode";

const props = defineProps({
  bracketNode: { type: Object as () => IBracketNode, default: () => ({}) },
  highlightedTeamId: { type: String }
});

const emit = defineEmits(['onMatchClick', 'onSelectedTeam', 'onDeselectedTeam']);

const getPlayerClass = (team: ITeam): string => {
  let clazz = "";

  if (
    props.bracketNode.match?.winner !== undefined &&
    team.id === props.bracketNode.match?.winner
  ) {
    clazz = "winner";
  }

  if (props.highlightedTeamId === team.id) {
    clazz += " highlight";
  }

  return clazz;
};

const onClick = (): void => {
  emit('onMatchClick', props.bracketNode?.match?.id);
};

const highlightTeam = (playerId: string): void => {
  emit('onSelectedTeam', playerId);
};

const unhighlightTeam = (): void => {
  emit('onDeselectedTeam');
};
</script>