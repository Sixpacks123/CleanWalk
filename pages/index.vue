<script setup lang="ts">
definePageMeta({
  colorMode: 'light'
})

const { data: page } = await useAsyncData('index', () => queryContent('/').findOne())

useSeoMeta({
  title: page.value.title,
  ogTitle: page.value.title,
  description: page.value.description,
  ogDescription: page.value.description
})

defineOgImage({
  component: 'Landing',
  title: page.value.title,
  description: page.value.description
})
</script>

<template>
  <div>
    <div class="bg-primary-500 absolute top-0 h-[625px] sm:h-[755px] lg:h-[895px] inset-x-0" />

    <ULandingHero
      :title="page.hero.title"
      orientation="vertical"
      :description="page.hero.description"
      :links="page.hero.links"

      :ui="{
        title: 'text-white selection:bg-primary-800',
        description: 'text-primary-100 selection:bg-primary-800'
      }"
    >
      <div v-if="page.hero.screenshot" class="   lg:-m-4 p-4">
        <img v-bind="page.hero.screenshot" src="/public/1576-removebg-preview.png" >
      </div>


    </ULandingHero>
    <ULandingSection id="who" :title="page.who.title"  :description="page.who.description">
      <ULandingCTA
        :description="page.who.pack.description"
        align="left"
        :card="false"
      >
        <img
          src="https://picsum.photos/640/360"
          class="w-full rounded-md shadow-xl ring-1 ring-gray-300 dark:ring-gray-700"
        />
      </ULandingCTA>
    </ULandingSection>
    <ULandingSection id="app">
        <ULandingCTA
          title="L'Application CleanWalks : Innovation au Service de l'Environnement"
          description="Interagissez, suivez, et partagez avec notre app – chaque cleanwalk devient une expérience enrichissante"
          :card="true"
          :links="[
        { label: 'Découvrez dès maintenant', color: 'primary', size: 'lg' }
      ]"
        >
          <img
            src="https://your-image-source.com/app-screenshot.jpg"
            class="w-full rounded-md shadow-xl ring-1 ring-gray-300 dark:ring-gray-700"
            alt="Aperçu de l'application CleanWalks"
          />
        </ULandingCTA>
    </ULandingSection>

    <ULandingSection id="features" :title="page.features.title" :description="page.features.description">
      <UPageGrid v-bind="page.pricing.grid">
        <ULandingCard v-for="(item, index) in page.features.items" :key="index" v-bind="item" />
      </UPageGrid>
    </ULandingSection>



    <ULandingSection
      id="pricing"
      :title="page.pricing.title"
      :description="page.pricing.description"
      :ui="{
        title: 'text-white selection:bg-primary-800',
        description: 'text-primary-100 selection:bg-primary-800'
      }"
      class="bg-primary-500"
    >
      <UPricingGrid v-bind="page.pricing.grid">
        <UPricingCard v-for="(plan, index) in page.pricing.plans" :key="index" v-bind="plan" />
      </UPricingGrid>
    </ULandingSection>

    <ULandingSection id="testimonials" class="bg-gray-50" :headline="page.testimonials.headline" :title="page.testimonials.title" :description="page.testimonials.description">
      <UPageColumns>
        <div v-for="(testimonial, index) in page.testimonials.items" :key="index" class="break-inside-avoid">
          <UPageCard>
            <q class="italic text-lg text-gray-500 dark:text-gray-400">
              {{ testimonial.quote }}
            </q>

            <template #footer>
              <div class="flex items-center gap-3">
                <UAvatar
                  :src="testimonial.author.avatar"
                  :alt="testimonial.author.name"
                  loading="lazy"
                  size="md"
                />

                <div class="text-sm">
                  <p class="font-medium text-gray-900 dark:text-white">
                    {{ testimonial.author.name }}
                  </p>
                  <p class="text-gray-500 dark:text-gray-400">
                    {{ testimonial.author.title }}
                  </p>
                </div>
              </div>
            </template>
          </UPageCard>
        </div>
      </UPageColumns>
    </ULandingSection>

    <ULandingSection class="bg-primary-500">
      <UFooterColumns :links="links">

        <template #right>
          <form @submit.prevent="">
            <UFormGroup label="Subscribe to our newsletter" :ui="{ container: 'mt-3' }">
              <UInput v-model="" type="email" placeholder="Enter your email" :ui="{ icon: { trailing: { pointer: '' } } }" required size="xl" autocomplete="off" class="max-w-sm">
                <template #trailing>
                  <UButton type="submit" size="xs" color="primary" />
                </template>
              </UInput>
            </UFormGroup>
          </form>
        </template>
      </UFooterColumns>    </ULandingSection>
<!--
    <ULandingSection id="faq" :title="page.faq.title" :description="page.faq.description" class="bg-gray-50">
      <ULandingFAQ
        :items="page.faq.items"
        :ui="{
          button: {
            label: 'font-semibold',
            trailingIcon: {
              base: 'w-6 h-6'
            }
          }
        }"
        multiple
        class="max-w-4xl mx-auto"
      />
    </ULandingSection>-->
  </div>
</template>
