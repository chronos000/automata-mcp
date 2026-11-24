<script setup lang="ts">
// Thanksgiving 2026 promotion, top of every page for organizations on Free.
//
// Hosted only, and Free only: `plan` is undefined on a self-hosted deployment
// and is the plan label ("Free", "Pro", "Pro (trial)", …) on the hosted one.
// Hidden on the first-run setup (/welcome), which is a guided flow, not a page
// to sell on. The code is a Stripe promotion code, typed on the Checkout page —
// Checkout runs with `allow_promotion_codes` (apps/cloud/src/billing.ts), so
// nothing here applies it. Only admins can reach Billing; members are told to
// ask one. Remove this component (and its line in layouts/default.vue) when the
// promotion ends.
const PROMO_CODE = 'THANKSGIVING2026'

const route = useRoute()
const { user, isAdmin } = useSession()
const { plan } = usePlan()

const visible = computed(() =>
  Boolean(user.value) && plan.value === 'Free' && !route.path.startsWith('/welcome'),
)
</script>

<template>
  <div v-if="visible" class="bg-primary text-primary-foreground">
    <p class="mx-auto max-w-5xl px-6 py-2 text-center text-sm">
      Thanksgiving offer: 50% off Pro for 6 months with code
      <code class="rounded bg-primary-foreground/10 px-1.5 py-0.5 font-mono font-semibold">{{ PROMO_CODE }}</code>.
      <NuxtLink v-if="isAdmin" to="/billing" class="font-medium underline underline-offset-4">
        Upgrade to Pro
      </NuxtLink>
      <template v-else>
        Ask an admin to upgrade.
      </template>
    </p>
  </div>
</template>
