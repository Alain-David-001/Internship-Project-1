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

          <div class="billing-toggle">
            <span
              :class="[
                'billing-toggle__label',
                { 'billing-toggle__label--active': billingCycle === 'monthly' },
              ]"
            >
              Monthly
            </span>

            <label class="billing-toggle__switch">
              <input
                class="billing-toggle__input"
                type="checkbox"
                :checked="billingCycle === 'annual'"
                aria-label="Toggle annual billing"
                @change="billingCycle = $event.target.checked ? 'annual' : 'monthly'"
              >
              <span class="billing-toggle__track">
                <span class="billing-toggle__thumb"></span>
              </span>
            </label>

            <span
              :class="[
                'billing-toggle__label',
                { 'billing-toggle__label--active': billingCycle === 'annual' },
              ]"
            >
              Annual
            </span>

            <span class="billing-toggle__badge">Save 18%</span>
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
  display: flex;
  gap: 0.8rem;
  align-items: center;
  justify-content: center;
  margin-top: 1.6rem;
  flex-wrap: wrap;

  &__label {
    color: rgba(148, 163, 184, 0.82);
    font-size: 1rem;
    font-weight: 600;
    transition: color 180ms ease;

    &--active {
      color: rgb(248, 250, 252);
    }
  }

  &__switch {
    position: relative;
    display: inline-flex;
    align-items: center;
    cursor: pointer;
  }

  &__input {
    position: absolute;
    opacity: 0;
    pointer-events: none;
  }

  &__track {
    position: relative;
    display: inline-flex;
    align-items: center;
    width: 4.1rem;
    height: 2.25rem;
    padding: 0.24rem;
    border: 1px solid rgba(148, 163, 184, 0.14);
    border-radius: 999px;
    background: rgba(15, 23, 42, 0.9);
    box-shadow:
      inset 0 0 0 1px rgba(30, 41, 59, 0.4),
      0 8px 20px rgba(2, 6, 23, 0.16);
    transition: border-color 180ms ease, box-shadow 180ms ease;
  }

  &__thumb {
    width: 1.55rem;
    height: 1.55rem;
    border-radius: 999px;
    background: linear-gradient(180deg, rgb(255, 255, 255), rgb(226, 232, 240));
    box-shadow:
      0 6px 18px rgba(15, 23, 42, 0.32),
      inset 0 1px 0 rgba(255, 255, 255, 0.65);
    transition: transform 180ms ease;
  }

  &__input:checked + &__track {
    border-color: rgba(45, 212, 191, 0.26);
    box-shadow:
      inset 0 0 0 1px rgba(20, 184, 166, 0.14),
      0 8px 20px rgba(2, 6, 23, 0.16);
  }

  &__input:checked + &__track &__thumb {
    transform: translateX(1.95rem);
  }

  &__input:focus-visible + &__track {
    outline: 2px solid rgba(45, 212, 191, 0.4);
    outline-offset: 2px;
  }

  &__badge {
    margin-left: 0.15rem;
    padding: 0.28rem 0.58rem;
    border-radius: 999px;
    color: rgb(153, 246, 228);
    font-size: 0.72rem;
    font-weight: 700;
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

:deep(.pricing-grid .plan-card--featured:hover) {
  transform: translateY(-1rem) scale(1.01);
  border-color: rgba(45, 212, 191, 0.56);
  box-shadow: 0 34px 82px rgba(8, 145, 178, 0.24);
}

:deep(.pricing-grid .plan-card--featured .plan-card__button) {
  box-shadow: 0 16px 30px rgba(13, 148, 136, 0.24);
}

:deep(.pricing-grid .plan-card--featured:hover .plan-card__button) {
  box-shadow: 0 18px 34px rgba(13, 148, 136, 0.28);
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
    gap: 0.7rem;
  }
}
</style>
