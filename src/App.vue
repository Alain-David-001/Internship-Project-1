<template>
  <div class="page-shell">
    <header class="topbar">
      <div class="topbar__start">
        <a class="topbar__brand" href="/">
          <span class="topbar__mark">A</span>
          <span>Align</span>
        </a>

        <nav class="topbar__nav" aria-label="Primary">
          <a href="/">Overview</a>
          <a href="#plans">Pricing</a>
          <a href="#reviews">Customers</a>
          <a :href="repoUrl" target="_blank" rel="noreferrer">About us</a>
        </nav>
      </div>

      <a class="topbar__cta" :href="repoUrl" target="_blank" rel="noreferrer">
        Contact sales
      </a>
    </header>

    <main>
      <div class="content-shell">
        <section id="plans" class="pricing-section">
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

        <section id="reviews" class="reviews-section">
          <div class="reviews-section__heading">
            <h2>Trusted by teams that need alignment across every release.</h2>
            <a
              class="reviews-section__action reviews-section__action--desktop"
              :href="repoUrl"
              target="_blank"
              rel="noreferrer"
            >
              Read more reviews
            </a>
          </div>

          <div class="reviews-grid">
            <article
              v-for="review in reviews"
              :key="review.name"
              class="review-card"
            >
              <p class="review-card__quote">"{{ review.quote }}"</p>
              <div class="review-card__meta">
                <div class="review-card__person">
                  <img
                    class="review-card__avatar"
                    :src="review.avatar"
                    alt=""
                  >
                  <div>
                    <strong>{{ review.name }}</strong>
                    <span>{{ review.role }}</span>
                  </div>
                </div>
              </div>
            </article>
          </div>

          <div class="reviews-section__footer">
            <a
              class="reviews-section__action reviews-section__action--mobile"
              :href="repoUrl"
              target="_blank"
              rel="noreferrer"
            >
              Read more reviews
            </a>
          </div>
        </section>
      </div>
    </main>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import PlanCard from '@/components/PlanCard.vue';
import martaLeonAvatar from '@/assets/avatars/marta-leon.jpg';
import victorMarinAvatar from '@/assets/avatars/victor-marin.jpg';
import sofiaWeberAvatar from '@/assets/avatars/sofia-weber.jpg';

const billingCycle = ref('monthly');
const repoUrl = 'https://github.com/Alain-David-001/Internship-Project-1';

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
    ctaUrl: repoUrl,
    price: `$${billingCycle.value === 'monthly' ? plan.monthlyPrice : plan.annualPrice}`,
    billingLabel:
      billingCycle.value === 'monthly'
        ? 'per seat / month'
        : 'per seat / month, billed annually',
  })),
);

const reviews = [
  {
    name: 'Marta Leon',
    avatar: martaLeonAvatar,
    role: 'Product Lead at Northline Studio',
    quote:
      'Align gave our team a cleaner way to track priorities across design and engineering without adding process overhead.',
  },
  {
    name: 'Victor Marin',
    avatar: victorMarinAvatar,
    role: 'Engineering Manager at Relay Works',
    quote:
      'The Team plan made handoff reviews much easier to follow, especially once we started sharing roadmap themes between squads.',
  },
  {
    name: 'Sofia Weber',
    avatar: sofiaWeberAvatar,
    role: 'Operations Director at Current Labs',
    quote:
      'We finally had one place where planning updates, approvals, and delivery notes stayed aligned without constant follow-up.',
  },
];
</script>

<style scoped lang="scss">
.page-shell {
  width: min(1360px, calc(100% - 2rem));
  margin: 0 auto;
  padding: 2.5rem 0 4rem;
}

.content-shell {
  width: min(1180px, 100%);
  margin: 0 auto;
}

.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 3rem;

  &__start {
    display: inline-flex;
    align-items: center;
    gap: 2.25rem;
    min-width: 0;
  }

  &__brand {
    display: inline-flex;
    align-items: center;
    gap: 0.7rem;
    color: rgb(248, 250, 252);
    font-weight: 700;
    letter-spacing: -0.02em;
    text-decoration: none;
  }

  &__mark {
    display: inline-grid;
    place-items: center;
    width: 2rem;
    height: 2rem;
    border-radius: 0.7rem;
    color: rgb(153, 246, 228);
    font-size: 0.92rem;
    background: rgba(13, 148, 136, 0.16);
    box-shadow: inset 0 0 0 1px rgba(45, 212, 191, 0.16);
  }

  &__nav {
    display: inline-flex;
    align-items: center;
    gap: 1.6rem;

    a {
      color: rgba(203, 213, 225, 0.78);
      font-weight: 500;
      text-decoration: none;
      transition: color 180ms ease;

      &:hover,
      &:focus-visible {
        color: rgb(248, 250, 252);
        outline: none;
      }
    }
  }

  &__cta {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 2.7rem;
    padding: 0.7rem 1rem;
    border: 1px solid rgba(148, 163, 184, 0.16);
    border-radius: 999px;
    color: rgb(248, 250, 252);
    font-weight: 600;
    text-decoration: none;
    background: rgba(15, 23, 42, 0.78);
    transition:
      border-color 180ms ease,
      background-color 180ms ease,
      transform 180ms ease;

    &:hover,
    &:focus-visible {
      border-color: rgba(45, 212, 191, 0.28);
      background: rgba(30, 41, 59, 0.92);
      transform: translateY(-1px);
      outline: none;
    }
  }
}

.pricing-section {
  margin-top: 0;
}

.reviews-section {
  margin-top: 3rem;
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

.reviews-section__heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1.5rem;

  h2 {
    margin: 0;
    max-width: 36rem;
    font-size: clamp(1.7rem, 3vw, 2.3rem);
    letter-spacing: -0.04em;
  }
}

.reviews-section__action {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 2.6rem;
  padding: 0.65rem 0.95rem;
  border: 1px solid rgba(148, 163, 184, 0.16);
  border-radius: 999px;
  color: rgb(248, 250, 252);
  font-weight: 600;
  text-decoration: none;
  background: rgba(15, 23, 42, 0.72);
  transition:
    border-color 180ms ease,
    background-color 180ms ease,
    transform 180ms ease;

  &:hover,
  &:focus-visible {
    border-color: rgba(45, 212, 191, 0.28);
    background: rgba(30, 41, 59, 0.92);
    transform: translateY(-1px);
    outline: none;
  }
}

.reviews-section__action--mobile {
  display: none;
}

.reviews-section__footer {
  display: none;
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

.reviews-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
}

.review-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 100%;
  padding: 1.3rem;
  border: 1px solid rgba(148, 163, 184, 0.14);
  border-radius: 24px;
  background: rgba(15, 23, 42, 0.72);

  &__quote {
    margin: 0;
    color: rgb(226, 232, 240);
    font-size: 1rem;
    line-height: 1.75;
  }

  &__meta {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-top: 1.4rem;
    padding-top: 1rem;
    border-top: 1px solid rgba(148, 163, 184, 0.1);
  }

  &__person {
    display: flex;
    align-items: center;
    gap: 0.8rem;
  }

  &__avatar {
    width: 2.6rem;
    height: 2.6rem;
    border-radius: 999px;
    object-fit: cover;
    box-shadow:
      0 8px 18px rgba(2, 6, 23, 0.24),
      inset 0 0 0 1px rgba(255, 255, 255, 0.08);
  }

  strong,
  span {
    display: block;
  }

  strong {
    font-size: 0.98rem;
  }

  span {
    margin-top: 0.25rem;
    color: rgba(203, 213, 225, 0.7);
    font-size: 0.9rem;
  }
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
  .topbar {
    flex-direction: column;
    align-items: center;
    margin-bottom: 2.2rem;
    gap: 1rem;

    &__start {
      width: auto;
      flex-direction: column;
      align-items: center;
      gap: 1rem;
    }

    &__nav {
      flex-wrap: wrap;
      justify-content: center;
    }
  }

  .pricing-grid {
    grid-template-columns: 1fr;
  }

  .reviews-grid {
    grid-template-columns: 1fr;
  }

  .review-card__meta {
    align-items: flex-start;
  }

  .reviews-section__heading {
    align-items: flex-start;
    flex-direction: column;
  }

  .reviews-section__action--desktop {
    display: none;
  }

  .reviews-section__action--mobile {
    display: inline-flex;
  }

  .reviews-section__footer {
    display: flex;
    justify-content: center;
    margin-top: 1.35rem;
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

  .topbar {
    &__cta {
      width: 100%;
    }
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
