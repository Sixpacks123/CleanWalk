<script setup lang="ts">
import type { FormError, FormSubmitEvent } from '#ui/types'

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
const types = ['Cleanwalk', 'Festivals', 'Evenements de quartier','autres']


const state = reactive({
  organizationName: '',
  eventType: '',
  groupSize: 1,
  contactEmail: '',
  additionalInfo: ''
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.organizationName) errors.push({ path: 'organizationName', message: 'Organization name is required' })
  if (!state.eventType) errors.push({ path: 'eventType', message: 'Event type is required' })
  if (!state.groupSize) errors.push({ path: 'groupSize', message: 'Group size is required' })
  if (!state.contactEmail) errors.push({ path: 'contactEmail', message: 'Contact email is required' })
  return errors
}

async function onSubmit (event: FormSubmitEvent<any>) {
  // Logique pour traiter les données du devis
  console.log(event.data)
}
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
      <div v-if="page.hero.screenshot" class=" w-full justify-center items-center lg:-m-4 p-4">
        <img v-bind="page.hero.screenshot" class=" mx-auto w-1/2 h-1/2 " src="/public/Front3.png" >
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
        <ULandingCTA
          id="app"
          title="L'Application CleanWalks : Innovation au Service de l'Environnement"
          description="Interagissez, suivez, et partagez avec notre app – chaque cleanwalk devient une expérience enrichissante"
          align="right"
          :card="false"
          :links="[
        { label: 'Découvrez dès maintenant', color: 'blue', size: 'lg' }
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


    <ULandingSection id="devis" :title="page.devis.title" :description="page.devis.description">
      <div class="flex flex-row justify-between items-stretch gap-4 flex-wrap">
        <div class="flex-1">
          <img
            src="https://www.ac-bordeaux.fr/sites/ac_bordeaux/files/2022-09/chapo-cleanwalk-png-42298.png"
            class="h-full w-full object-cover rounded-md shadow-xl ring-1 ring-gray-300 dark:ring-gray-700"
          />
        </div>

        <div class="flex-1">
          <UForm :validate="validate" :state="state" class="space-y-4 h-full" @submit="onSubmit">
            <UFormGroup label="Nom de l'Organisation" name="nomOrganisation">
              <UInput v-model="state.nomOrganisation" />
            </UFormGroup>

            <UFormGroup label="Type d'Événement" name="typeEvenement">
              <USelect v-model="type" :options="types" />
            </UFormGroup>
            <UFormGroup label="Taille du Groupe" name="tailleGroupe">
              <div class="flex space-x-2 content-baseline">
                <URange v-model="state.groupSize" name="range" />
                <span>{{state.groupSize}}</span>
              </div>
            </UFormGroup>

            <UFormGroup label="Email de Contact" name="emailContact">
              <UInput v-model="state.emailContact" type="email" />
            </UFormGroup>

            <UFormGroup label="Informations Supplémentaires" name="infosSupplementaires">
              <UTextarea v-model="state.infosSupplementaires" />
            </UFormGroup>

            <UButton type="submit">
              Demander un Devis
            </UButton>
          </UForm>
        </div>
      </div>
    </ULandingSection>

    <!--
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
-->

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
