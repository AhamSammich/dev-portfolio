<script setup lang="ts">
import { onMounted, ref } from "vue";
const props = defineProps<{
  events: {
    title: string;
    year: string | number;
    description: string;
  }[];
}>();

const currentCard = ref("");
const evHeaders = Object.values(props.events).map((ev) => ev.title);

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.intersectionRatio > 0) {
        currentCard.value = entry.target.getAttribute("id") || "";
      }
    });
  });

  [...document.querySelectorAll(".event-card>h1")].forEach((card) => {
    observer.observe(card);
  });
});
</script>

<template>
  <div id="timeline">
    <div id="toc">
      <template v-for="(ev, index) in evHeaders" :key="ev">
        <a :class="{ current: index }" :href="`#${index}`">
          {{ ev }}
        </a>
      </template>
    </div>
    <div id="events">
      <template v-for="(ev, index) in events" :key="ev">
        <div :id="`${index}`" class="event-card" :data-year="ev.year">
          <h1>{{ ev.title }}</h1>
          <p>{{ ev.description }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped lang="postcss">
::before,
::after {
  content: "";
  display: block;
  position: absolute;
}

#toc {
  font-size: smaller;
  color: var(--color1);
  display: grid;
  gap: 0.5em;
  position: fixed;
  left: -1.5em;
  margin-top: 2.5%;
  margin-left: 5%;
  padding-right: 1em;
  border-right: 0.1em solid var(--accent1);

  & a:hover {
    color: var(--accent2);
  }

  & a.current {
    border-color: var(--accent1);
  }

  @media (width < 680px) {
    display: none;
  }
}

#timeline {
  height: 70dvh;
  width: 100vw;
  overflow-y: scroll;
  overflow-x: hidden;
  scroll-behavior: smooth;

  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 0.5rem;
  padding-bottom: 25vh;
  position: relative;

  &::-webkit-scrollbar {
    width: 0rem;
  }
}

.event-card {
  --curve: 45%;
  --point-size: 64px;
  display: flex;
  width: 300px;
  overflow-x: visible;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  line-height: 1.2;
  letter-spacing: 0.05em;
  background-color: var(--color2);
  color: var(--color1);

  margin: 0.5em 5rem;
  padding: 3rem 1rem;
  position: relative;

  & > h1 {
    animation: fadeIn 1s;
    font-size: 24px;
  }

  & > p {
    animation: fadeIn 1s;
    width: clamp(32ch, 100%, 40ch);
  }

  &::before,
  &::after {
    height: 100%;
    width: 90%;
    z-index: -1;
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
      translate: -60% -50%;
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
      translate: -45% -50%;
    }

    /* TIMELINE (RIGHT) */
    &::after {
      box-shadow: 2.25em 0 0 0.5em var(--color1);
    }
  }
}
</style>
