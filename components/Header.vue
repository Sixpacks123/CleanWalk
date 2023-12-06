<script setup lang="ts">
const links = [{
  label: 'Nous',
  to: '#who',
  icon: 'i-heroicons-credit-card',
  exactHash: true
},{
  label: 'Notre app',
  to: '#app',
  icon: 'i-heroicons-cube-transparent',
  exactHash: true
},{
  label: 'Pourquoi ?',
  to: '#features',
  icon: 'i-heroicons-cube-transparent',
  exactHash: true
}, {
  label: 'Pricing',
  to: '#pricing',
  icon: 'i-heroicons-credit-card',
  exactHash: true
}, {
  label: 'Témoignages',
  to: '#testimonials',
  icon: 'i-heroicons-academic-cap',
  exactHash: true
}]
</script>

<template>
  <UHeader :links="links">
    <template #logo>
      CleanWalk
    </template>

    <template #right>
      <UButton label="Contact" color="black" class="hidden lg:flex" />
    </template>

    <template #panel>
      <UAsideLinks :links="links" />

      <UDivider class="my-6" />

      <UButton label="Contact" block />
    </template>
  </UHeader>
</template>
