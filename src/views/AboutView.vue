<template>
  <q-page class="flex flex-center" :class="$q.dark.isActive ? 'text-white' : 'text-dark'">
    <q-card flat bordered class="my-card">
      <q-card-section>
        <div class="row items-center no-wrap">
          <div class="col">
            <div class="text-h5">VueJS - Notes App API</div>
            <div style="font-size: 24px;" class="text-subtitle2 q-my-md">bởi <span style="font-weight: bold;">Huỳnh Vĩnh Tiến</span></div>
            Liên Hệ Tôi:
            <a :class="$q.dark.isActive ? 'text-white' : 'text-dark'" href="/"
              target="_blank" style="text-decoration: none;">Github</a>
          </div>
        </div>
      </q-card-section>
      <q-separator />

      <q-card-section class="q-pt-none">
        <p class="q-my-md">Công Nghệ Sử Dụng</p>
        <q-list bordered separator>
          <q-item clickable v-ripple>
            <a :class="$q.dark.isActive ? 'text-white' : 'text-dark'" href="https://vuejs.org/" target="_blank" style="text-decoration: none;">Vue 3</a>
          </q-item>
          <q-item clickable v-ripple>
            <a :class="$q.dark.isActive ? 'text-white' : 'text-dark'" href="https://pinia.vuejs.org//" target="_blank" style="text-decoration: none;">Pinia</a>
          </q-item>
          <q-item clickable v-ripple>
            <a :class="$q.dark.isActive ? 'text-white' : 'text-dark'" href="https://vueuse.org/"
              target="_blank" style="text-decoration: none;">VueUse</a>
          </q-item>
          <q-item clickable v-ripple>
            <a :class="$q.dark.isActive ? 'text-white' : 'text-dark'" href="https://quasar.dev/" target="_blank" style="text-decoration: none;">Quasar
              Framework</a>
          </q-item>
          <q-item clickable v-ripple>
            <a :class="$q.dark.isActive ? 'text-white' : 'text-dark'" href="https://vue-multiselect.js.org/" target="_blank" style="text-decoration: none;">Vue-Multiselect</a>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 400px
</style>