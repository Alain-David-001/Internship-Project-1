<template>
  <div class="page-shell">
    <main>
      <section class="pricing-section">
        <div class="section-heading">
          <h1>Choose a plan that fits your team.</h1>
          <p>
            Clear pricing for product teams of different sizes, from early collaboration
            to broader rollout and governance.
          </p>

          <div class="billing-toggle" role="tablist" aria-label="Billing period">
            <button
              :class="['billing-toggle__option', { 'billing-toggle__option--active': billingCycle === 'monthly' }]"
              type="button"
              role="tab"
              :aria-selected="billingCycle === 'monthly'"
              @click="billingCycle = 'monthly'"
            >
              Monthly
            </button>
            <button
              :class="['billing-toggle__option', { 'billing-toggle__option--active': billingCycle === 'annual' }]"
              type="button"
              role="tab"
              :aria-selected="billingCycle === 'annual'"
              @click="billingCycle = 'annual'"
            >
              Annual
              <span class="billing-toggle__hint">Save 18%</span>
            </button>
          </div>
        </div>

        <div class="pricing-grid">
          <PlanCard
            v-for="plan in displayedPlans"
            :key="plan.name"
            v-bind="plan"
          />
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import PlanCard from '@/components/PlanCard.vue';

const billingCycle = ref('monthly');

const plans = [
  {
    name: 'Starter',
    monthlyPrice: 12,
    annualPrice: 10,
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
    monthlyPrice: 24,
    annualPrice: 20,
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
    monthlyPrice: 42,
    annualPrice: 34,
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

const displayedPlans = computed(() =>
  plans.map((plan) => ({
    ...plan,
    price: `$${billingCycle.value === 'monthly' ? plan.monthlyPrice : plan.annualPrice}`,
    billingLabel:
      billingCycle.value === 'monthly'
        ? 'per seat / month'
        : 'per seat / month, billed annually',
  })),
);
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
  max-width: 50rem;
  margin: 0 auto 2.25rem;
  text-align: center;

  h1 {
    margin: 0;
    font-size: clamp(2.2rem, 4vw, 3.35rem);
    line-height: 1.02;
    letter-spacing: -0.05em;
  }

  p {
    max-width: 40rem;
    margin: 1rem auto 0;
    color: rgba(203, 213, 225, 0.75);
    line-height: 1.7;
  }
}

.billing-toggle {
  display: inline-flex;
  gap: 0.45rem;
  align-items: center;
  margin-top: 1.6rem;
  padding: 0.4rem;
  border: 1px solid rgba(148, 163, 184, 0.16);
  border-radius: 999px;
  background: rgba(15, 23, 42, 0.72);

  &__option {
    display: inline-flex;
    gap: 0.55rem;
    align-items: center;
    min-height: 2.8rem;
    padding: 0.75rem 1rem;
    border: 0;
    border-radius: 999px;
    color: rgba(226, 232, 240, 0.78);
    font: inherit;
    font-weight: 600;
    background: transparent;
    cursor: pointer;
    transition:
      color 180ms ease,
      background-color 180ms ease,
      box-shadow 180ms ease;

    &:hover,
    &:focus-visible {
      color: rgb(248, 250, 252);
      outline: none;
    }

    &--active {
      color: rgb(248, 250, 252);
      background: rgba(30, 41, 59, 0.92);
      box-shadow: inset 0 0 0 1px rgba(148, 163, 184, 0.12);
    }
  }

  &__hint {
    padding: 0.22rem 0.5rem;
    border-radius: 999px;
    color: rgb(153, 246, 228);
    font-size: 0.72rem;
    letter-spacing: 0.01em;
    background: rgba(13, 148, 136, 0.16);
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

  .section-heading p {
    margin-left: 0;
    margin-right: 0;
  }

  .billing-toggle {
    display: flex;
    width: 100%;
    justify-content: center;

    &__option {
      justify-content: center;
      flex: 1 1 0;
    }
  }
}
</style>
