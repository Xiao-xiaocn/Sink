<script setup lang="ts">
import { ArrowRight, Search } from 'lucide-vue-next'

const slug = ref('')

function normalizeInput(value: string) {
  const trimmed = value.trim()
  if (!trimmed)
    return ''

  try {
    const url = new URL(trimmed)
    return url.pathname.replace(/^\/+|\/+$/g, '')
  }
  catch {
    return trimmed.replace(/^\/+|\/+$/g, '')
  }
}

function openShortLink() {
  const targetSlug = normalizeInput(slug.value)
  if (!targetSlug)
    return

  navigateTo(`/${encodeURI(targetSlug)}`, { external: true })
}
</script>

<template>
  <section class="w-full">
    <div
      class="
        mx-auto flex max-w-3xl flex-col items-center px-6 py-16 text-center
        md:py-24
      "
    >
      <div
        class="
          mb-8 flex size-12 items-center justify-center rounded-md border
          bg-background shadow-sm
        "
      >
        <Search aria-hidden="true" class="size-5" />
      </div>

      <h1
        class="
          text-4xl font-medium text-balance
          md:text-5xl
        "
      >
        {{ $t('home.hero.short_link.title') }}
      </h1>
      <p class="mt-4 max-w-xl text-base text-pretty text-muted-foreground">
        {{ $t('home.hero.short_link.description') }}
      </p>

      <form
        class="
          mt-10 flex w-full max-w-xl flex-col gap-3
          sm:flex-row
        "
        @submit.prevent="openShortLink"
      >
        <label for="short-link-search" class="sr-only">Short link</label>
        <div class="relative min-w-0 flex-1">
          <Search
            aria-hidden="true"
            class="
              pointer-events-none absolute top-1/2 left-4 size-4
              -translate-y-1/2 text-muted-foreground
            "
          />
          <Input
            id="short-link-search"
            v-model="slug"
            type="text"
            inputmode="url"
            autocomplete="off"
            :placeholder="$t('home.hero.short_link.placeholder')"
            class="
              h-12 rounded-md pl-11 text-base
              sm:h-12
            "
          />
        </div>

        <Button
          type="submit"
          size="lg"
          class="
            h-12 shrink-0 gap-2 px-5 text-base
            sm:w-auto
          "
        >
          <span>{{ $t('home.hero.short_link.action') }}</span>
          <ArrowRight aria-hidden="true" class="size-4" />
        </Button>
      </form>
    </div>
  </section>
</template>
