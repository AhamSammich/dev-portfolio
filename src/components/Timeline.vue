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
    <template v-for="ev in events">
      <div class="event-card" :data-year="ev.year">
        <h1>{{ ev.name }}</h1>
        <p>{{ ev.description }}</p>
      </div>
    </template>
  </div>
</template>

<style scoped lang="postcss">
#timeline {
  height: 33dvh;
  overflow-y: scroll;
  overflow-x: hidden;

  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 0.5rem;
  position: relative;

  &::before {
    content: "";
    display: block;
    height: inherit;
    background: linear-gradient(180deg, var(--color2), transparent, var(--color2));
    top: 0;
    left: 0;
  }

  &::-webkit-scrollbar {
    width: 0.1rem;
  }
}

.event-card {
  --curve: 45%;
  --point-size: 64px;
  display: flex;
  width: 300px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: var(--color2);
  color: var(--color1);
  margin: 0.25em 5rem;
  padding: 3rem 1rem;
  position: relative;

  &::before,
  &::after {
    height: 100%;
    width: 90%;
    position: absolute;
    display: block;
    z-index: -1;
    content: "";
    border-radius: var(--curve);
    top: 0;
    left: 0;
  }

  /* DATE BUBBLE */
  &::before {
    content: attr(data-year);
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    color: var(--color2);
    font-weight: 600;
    height: var(--point-size);
    width: var(--point-size);
    background: var(--gradient);
    border: 0.2rem solid var(--color2);
    z-index: 2;
  }

  &:nth-child(odd) {
    border-radius: var(--curve) 0.5em 0.5em var(--curve);

    & > * {
      align-self: flex-start;
      padding-left: 3rem;
      text-align: left;
    }

    &::before {
      border-radius: 50%;
      left: 0;
      top: 50%;
      translate: -65% -50%;
    }

    /* TIMELINE (LEFT) */
    &::after {
      box-shadow: -0.5em 0 0 0.5em var(--color1);
    }
  }

  &:nth-child(even) {
    border-radius: 0.5em var(--curve) var(--curve) 0.5em;

    & > * {
      align-self: flex-end;
      padding-right: 3rem;
      text-align: right;
    }

    &::before {
      border-radius: 50%;
      left: 100%;
      top: 50%;
      translate: -40% -50%;
    }

    /* TIMELINE (RIGHT) */
    &::after {
      box-shadow: 2.25em 0 0 0.5em var(--color1);
    }
  }
}
</style>
