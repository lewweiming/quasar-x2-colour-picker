
<!-- MainLayout -->
<template>
  <q-layout view="lHh lpR lFf" class="bg-grey-10">
    <q-header class="bg-grey-10 text-white">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="left = !left"
        />

        <q-toolbar-title>
            <q-btn to="/" no-caps flat>
            Quasar X2 Colour Picker
            </q-btn>
        </q-toolbar-title>

        <q-btn type="a" href="#" icon="img:assets/github.svg" />
        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="left" side="left">
      <!-- drawer content -->
      <q-scroll-area class="bg-grey-10 window-height">
      <LeftPanelList />
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      
      <q-scroll-area class="bg-grey-10" style="height: calc(100vh - 50px);">
      <router-view />
      </q-scroll-area>

    </q-page-container>
  </q-layout>
</template>

<script>
import LeftPanelList from 'components/LeftPanelList.vue'
export default {
  name: 'MainLayout',
  components: {
    LeftPanelList,
  },
  data () {
    return {
      left: true
    }
  }
}
</script>
