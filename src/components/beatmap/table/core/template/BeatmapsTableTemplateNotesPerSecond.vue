<template>
  <span>
    {{ nps }}
  </span>
</template>

<script lang="ts">
import Vue from "vue";
import { Difficulty } from "@/libraries/net/beatsaver/BeatsaverBeatmap";

export default Vue.extend({
  name: "BeatmapsTableTemplateNotesPerSecond",
  props: {
    item: { type: Object, required: true },
  },
  computed: {
    minMaxNPS(): Array<number> {
      const difficulties = Object.entries(
        this.item.data.metadata.characteristics[0].difficulties
      )
        .filter((kv) => kv[1] !== null)
        .map((v) => v[1]) as Array<Difficulty>;

      let max = Number.MIN_VALUE;
      let min = Number.MAX_VALUE;
      difficulties.forEach((difficulty) => {
        const nps =
          difficulty.length === 0 || difficulty.notes === 0
            ? 0
            : difficulty.notes / difficulty.length;

        if (max < nps) max = nps;
        if (min > nps) min = nps;
      });
      max = max === Number.MIN_VALUE ? 0 : max;
      console.info(difficulties);
      return [min, max];
    },
    nps(): string {
      if (Math.abs(this.minMaxNPS[0] - this.minMaxNPS[1]) < Number.EPSILON)
        return this.minMaxNPS[0].toFixed(2);
      return `${this.minMaxNPS[0].toFixed(2)}-${this.minMaxNPS[1].toFixed(2)}`;
    },
  },
});
</script>
