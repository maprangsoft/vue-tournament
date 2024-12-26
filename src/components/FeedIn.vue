<template>
  <div class="vt-item-feedIn">
    <div>
      <div class="title">
        <span>{{ bracketNode.match.title }}</span>
      </div>
      <div
        :class="['vt-feedIn', getPlayerClass(bracketNode.match.feedIn)]"
        @mouseover="highlightTeam(bracketNode.match.feedIn.id)"
        @mouseleave="unhighlightTeam"
        @click="onClick"
      >
        <span class="name">{{ bracketNode.match.feedIn.name }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import IBracketNode from "../interface/IBracketNode";
import IFeedIn from "@/interface/IFeedIn";

const props = defineProps({
  bracketNode: { type: Object as () => IBracketNode, default: () => ({}) },
  highlightedTeamId: { type: String }
});

const emit = defineEmits(['onMatchClick', 'onSelectedTeam', 'onDeselectedTeam']);

const getPlayerClass = (feedIn: IFeedIn): string => {
  let clazz = "";

  if (props.highlightedTeamId === feedIn.id) {
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