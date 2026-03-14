<template>
  <div class="page-shell">
    <main>
      <section class="pricing-section">
        <div class="section-heading">
          <p class="section-heading__eyebrow">Subscription plans</p>
          <h1>Pick the tier that matches your release pace.</h1>
          <p>
            The featured Team plan intentionally combines global styles, scoped component
            styles, modifier classes, custom properties, and responsive overrides.
          </p>
        </div>

        <div class="pricing-grid">
          <PlanCard
            v-for="plan in plans"
            :key="plan.name"
            v-bind="plan"
          />
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import PlanCard from '@/components/PlanCard.vue';

const plans = [
  {
    name: 'Starter',
    price: '$12',
    billingLabel: 'per seat / month',
    description: 'For early product teams organizing research notes and delivery checklists.',
    cta: 'Start with Starter',
    badge: 'Core workflow',
    tone: 'slate',
    features: [
      'Unlimited briefs and notes',
      'Shared kanban board',
      'Weekly digest exports',
      'Basic role permissions',
    ],
  },
  {
    name: 'Team',
    price: '$24',
    billingLabel: 'per seat / month',
    description: 'For collaborative teams that need stronger visibility, review loops, and handoff.',
    cta: 'Choose Team',
    badge: 'Most popular',
    tone: 'teal',
    featured: true,
    features: [
      'Timeline snapshots',
      'Reviewer feedback threads',
      'Priority automations',
      'Shared roadmap themes',
    ],
  },
  {
    name: 'Scale',
    price: '$42',
    billingLabel: 'per seat / month',
    description: 'For larger organizations coordinating multiple squads and approval chains.',
    cta: 'Talk to sales',
    badge: 'Advanced controls',
    tone: 'gold',
    features: [
      'Portfolio-level reporting',
      'Workspace analytics',
      'SSO and audit exports',
      'Dedicated onboarding support',
    ],
  },
];
</script>

<style scoped lang="scss">
.page-shell {
  width: min(1180px, calc(100% - 2rem));
  margin: 0 auto;
  padding: 2.5rem 0 4rem;
}

.pricing-section {
  margin-top: 0;
}

.section-heading {
  max-width: 42rem;
  margin-bottom: 1.75rem;

  &__eyebrow {
    margin: 0 0 0.75rem;
    color: rgba(125, 211, 252, 0.86);
    font-size: 0.78rem;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
  }

  h1 {
    margin: 0;
    font-size: clamp(2.5rem, 5vw, 4.1rem);
    line-height: 0.98;
    letter-spacing: -0.05em;
  }

  p {
    margin: 0.9rem 0 0;
    color: rgba(203, 213, 225, 0.75);
    line-height: 1.7;
  }
}

.pricing-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1.2rem;
  align-items: stretch;
}

:deep(.pricing-grid .plan-card) {
  backdrop-filter: blur(18px);
}

:deep(.pricing-grid .plan-card__price) {
  text-wrap: balance;
}

:deep(.pricing-grid .plan-card--featured) {
  transform: translateY(-0.8rem);
  box-shadow: 0 28px 70px rgba(8, 145, 178, 0.18);
}

:deep(.pricing-grid .plan-card--featured .plan-card__button) {
  box-shadow: 0 16px 30px rgba(13, 148, 136, 0.24);
}

@media (max-width: 980px) {
  .pricing-grid {
    grid-template-columns: 1fr;
  }

  :deep(.pricing-grid .plan-card--featured) {
    transform: none;
  }
}

@media (max-width: 640px) {
  .page-shell {
    width: min(100% - 1rem, 100%);
    padding-top: 1rem;
  }

  .section-heading h1 {
    font-size: 2.4rem;
  }
}
</style>
