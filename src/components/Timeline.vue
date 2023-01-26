<script setup lang="ts">
const props = defineProps<{
  events: {
    name: string;
    year: number;
    description: string;
  }[];
}>();
</script>

<template>
  <div id="timeline">
    <div id="dateline">
      <template v-for="ev in events">
        <div class="event-card">
          <h1>{{ ev.year }}</h1>
          <h2>{{ ev.name }}</h2>
          <p>{{ ev.description }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped lang="postcss">
#dateline {
  height: 100dvh;
  overflow-x: scroll;
  overflow-y: hidden;

  display: flex;
  justify-content: flex-start;
  align-items: center;

  &::-webkit-scrollbar {
    height: 0.3rem;
  }
}

.event-card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: lightcyan;
  margin: 3rem;
  padding: 1rem;
}
</style>
