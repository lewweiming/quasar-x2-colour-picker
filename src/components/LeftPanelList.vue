<!-- LeftPanel -->
<template>
  <q-list separator class="bg-grey-10 text-grey-2 full-height">
    <q-item-label header> Explore Topics </q-item-label>
    <q-item :to="i.route" v-for="i in items" clickable>
      <q-item-section avatar>
        <q-icon name="chevron_right" />
      </q-item-section>
      <q-item-section>
        {{ i.label }}
      </q-item-section>
    </q-item>
  
  </q-list>
</template>

<script>
const items = []

// const items = [
//   {
//     label: "Grid",
//     route: 'grid'
//   }
// ];

export default {
  data() {
    return {
      items
    };
  },
};
</script>