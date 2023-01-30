<script setup lang="ts">
import { onMounted, ref } from "vue";
const props = defineProps<{
  events: {
    title: string;
    year?: string | number;
    description: string;
  }[];
}>();

const currentCard = ref("");

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          // Update table of contents while scrolling
          currentCard.value = entry.target.textContent || "";
          // Reveal cards while scrolling
          if (entry.target.classList.contains("event-card")) {
            entry.target.classList.add("revealed");
            observer.unobserve(entry.target);
          }
        }
      });
    },
    {
      threshold: 0,
      root: document.getElementById("timeline"),
      rootMargin: "0px 0px 0px 0px",
    }
  );

  document.querySelectorAll(".event-card h1").forEach((card) => {
    observer.observe(card);
    if (card.parentElement) observer.observe(card.parentElement);
  });
});
</script>

<template>
  <div id="timeline">
    <div id="toc">
      <template v-for="(ev, index) in events" :key="ev">
        <a :href="`#${index}`" :class="`${ev.title == currentCard ? 'current' : ''}`">
          {{ ev.year || "" }}
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
  gap: 0;
  position: fixed;
  margin-top: 5vh;
  margin-left: 5%;
  left: 0;
  z-index: 5;

  & a {
    background-color: var(--color2);
    padding: 0.25em 1em;
    border-right: 0.1em solid var(--accent1);
  }

  & a:hover {
    color: var(--accent2);
  }

  @media (width > 800px) {
    margin-right: 5%;
    left: unset;
    right: 0;

    & a {
      border-left: 0.1em solid var(--accent1);
      border-right: none;
    }
  }

  @media (width <= 680px) {
    display: none;
  }

  & .current {
    border-color: var(--accent3);
    color: var(--accent3);
  }
}

#timeline {
  height: 70vh;
  height: 70dvh;
  width: 100vw;
  width: 100dvw;
  overflow-y: scroll;
  overflow-x: hidden;
  scroll-behavior: smooth;

  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 0.5rem;
  padding-bottom: 5%;
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
  min-height: 250px;
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
  & h1 {
    font-size: 24px;
  }

  & p {
    width: clamp(32ch, 100%, 40ch);
  }

  & :is(h1, p) {
    transform-origin: top;
    opacity: 0;
    transition: all 0.5s 0.5s;
  }

  &::before,
  &::after {
    height: 100%;
    width: 90%;
    z-index: -1;
    border-radius: var(--curve);
    top: 0;
    left: 0;
    opacity: 0;
    transform-origin: top;
    transform: scaleY(0);
    transition: all 0.5s;
  }

  /* DATE BUBBLE */
  &::before {
    content: attr(data-year) "";
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

.event-card.revealed {
  & :is(h1, p),
  &::after,
  &::before {
    transform: none;
    opacity: 1;
  }
}
</style>
