<template>
  <article :class="cardClasses" :style="cardStyle">
    <div class="plan-card__header">
      <div>
        <p class="plan-card__badge">{{ badge }}</p>
        <h3>{{ name }}</h3>
      </div>
      <span v-if="featured" class="plan-card__tier">Recommended</span>
    </div>

    <div class="plan-card__price-block">
      <p class="plan-card__price">{{ price }}</p>
      <p class="plan-card__billing">{{ billingLabel }}</p>
    </div>

    <p class="plan-card__description">{{ description }}</p>

    <ul class="plan-card__features">
      <li v-for="feature in features" :key="feature">
        <span class="plan-card__feature-dot"></span>
        <span>{{ feature }}</span>
      </li>
    </ul>

    <button class="plan-card__button" type="button">
      {{ cta }}
    </button>
  </article>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  badge: {
    type: String,
    required: true,
  },
  billingLabel: {
    type: String,
    required: true,
  },
  cta: {
    type: String,
    required: true,
  },
  description: {
    type: String,
    required: true,
  },
  featured: {
    type: Boolean,
    default: false,
  },
  features: {
    type: Array,
    required: true,
  },
  name: {
    type: String,
    required: true,
  },
  price: {
    type: String,
    required: true,
  },
  tone: {
    type: String,
    default: 'slate',
  },
});

const toneMap = {
  slate: {
    accent: '125, 211, 252',
    glow: '30, 41, 59',
  },
  teal: {
    accent: '45, 212, 191',
    glow: '13, 148, 136',
  },
  gold: {
    accent: '250, 204, 21',
    glow: '202, 138, 4',
  },
};

const cardClasses = computed(() => [
  'plan-card',
  `plan-card--${props.tone}`,
  { 'plan-card--featured': props.featured },
]);

const cardStyle = computed(() => {
  const palette = toneMap[props.tone] || toneMap.slate;

  return {
    '--plan-accent': palette.accent,
    '--plan-glow': palette.glow,
  };
});
</script>

<style scoped lang="scss">
.plan-card {
  display: flex;
  flex-direction: column;
  gap: 1.4rem;
  min-height: 100%;
  padding: 1.45rem;
  border: 1px solid rgba(var(--plan-accent), 0.22);
  border-radius: 28px;
  background:
    linear-gradient(180deg, rgba(15, 23, 42, 0.98), rgba(15, 23, 42, 0.9)),
    radial-gradient(circle at top, rgba(var(--plan-accent), 0.1), transparent 58%);
  transition:
    transform 180ms ease,
    border-color 180ms ease,
    box-shadow 180ms ease;

  &:hover {
    transform: translateY(-0.3rem);
    border-color: rgba(var(--plan-accent), 0.48);
    box-shadow: 0 22px 48px rgba(var(--plan-glow), 0.16);
  }

  &--featured {
    background:
      linear-gradient(180deg, rgba(15, 23, 42, 0.98), rgba(9, 32, 38, 0.95)),
      radial-gradient(circle at top, rgba(var(--plan-accent), 0.18), transparent 52%);
  }

  &__header {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 1rem;
  }

  &__badge {
    margin: 0 0 0.6rem;
    color: rgba(var(--plan-accent), 0.96);
    font-size: 0.76rem;
    font-weight: 700;
    letter-spacing: 0.16em;
    text-transform: uppercase;
  }

  &__tier {
    padding: 0.35rem 0.65rem;
    border-radius: 999px;
    color: rgba(226, 232, 240, 0.92);
    font-size: 0.76rem;
    background: rgba(148, 163, 184, 0.14);
  }

  h3 {
    margin: 0;
    font-size: 1.5rem;
    letter-spacing: -0.03em;
  }

  &__price-block {
    display: flex;
    align-items: baseline;
    gap: 0.6rem;
    padding-bottom: 1.15rem;
    border-bottom: 1px solid rgba(148, 163, 184, 0.14);
  }

  &__price {
    margin: 0;
    color: rgb(248, 250, 252);
    font-size: 3rem;
    font-weight: 700;
    letter-spacing: -0.06em;
    line-height: 1;
  }

  &__billing,
  &__description {
    margin: 0;
    color: rgba(203, 213, 225, 0.74);
    line-height: 1.65;
  }

  &__features {
    display: grid;
    gap: 0.9rem;
    padding: 0;
    margin: 0;
    list-style: none;

    li {
      display: grid;
      grid-template-columns: auto 1fr;
      gap: 0.7rem;
      align-items: center;
    }
  }

  &__feature-dot {
    width: 0.65rem;
    height: 0.65rem;
    border-radius: 999px;
    background: rgba(var(--plan-accent), 0.95);
    box-shadow: 0 0 0 6px rgba(var(--plan-accent), 0.12);
  }

  &__button {
    margin-top: auto;
    min-height: 3.25rem;
    padding: 0.95rem 1rem;
    border: 1px solid rgba(var(--plan-accent), 0.18);
    border-radius: 18px;
    color: rgb(10, 15, 28);
    font: inherit;
    font-weight: 700;
    background: rgb(var(--plan-accent));
    cursor: pointer;
    transition:
      transform 180ms ease,
      background-color 180ms ease,
      box-shadow 180ms ease;

    &:hover,
    &:focus-visible {
      transform: translateY(-2px);
      box-shadow: 0 14px 24px rgba(var(--plan-glow), 0.22);
      outline: none;
    }
  }
}

@media (max-width: 640px) {
  .plan-card {
    padding: 1.2rem;

    &__price {
      font-size: 2.55rem;
    }
  }
}
</style>
