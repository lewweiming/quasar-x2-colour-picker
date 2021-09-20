<template>
  <q-page class="text-white">
    <q-tooltip
      ref="tooltip"
      :target="target"
      transition-duration="0"
      transition-show="fade"
      anchor="bottom middle"
      self="center middle"
      :class="`bg-${selectedSwatch} ${tooltipTextColour}`"
      >{{ selectedSwatch }}
    </q-tooltip>

    <div v-for="c in colours">
      <q-icon
        @click="onClick(c,i)"
        @mouseover="onMouseOver(c, i)"
        :id="`swatch-${c}-${i}`"
        class="cursor-pointer"
        v-for="i in 10"
        :color="`${c}-${i}`"
        size="2rem"
        name="svguse:assets/swatch.svg#icon-1"
      />
      <span class="q-ml-md">{{ c }}</span>
    </div>

    <!-- Dialog -->
    <q-dialog v-if="selectedSwatch" v-model="showDialog" seamless position="bottom" class="z-top">
      <q-card dark class="relative-position" style="width: 500px">
        <q-bar class="bg-grey-9">
        <q-icon :color="input.swatch" name="circle" />
        <div class="text-bold">Colour swatch - {{ input.swatch }}</div>
        <q-space />
        <q-btn flat dense round icon="close" v-close-popup />
        </q-bar>
        <q-card-section>
          <div class="row full-width q-col-gutter-md">

            <q-item :class="`col-6 ${dialogTextColor} bg-${input.swatch}`">
              <q-item-section>
                <div class="text-caption">Hex</div>
                <div>{{ input.hex }}</div>
              </q-item-section>
              <q-item-section avatar>
                <q-btn color="grey-8" @click="copy(input.hex)" icon="content_copy" />
              </q-item-section>
            </q-item>

            <q-item :class="`col-6 ${dialogTextColor} bg-${input.swatch}`">
              <q-item-section>
                <div class="text-caption">Class</div>
                <div>{{ input.swatch }}</div>
              </q-item-section>
              <q-item-section avatar>
                <q-btn color="grey-8" @click="copy(input.swatch)" icon="content_copy" />
              </q-item-section>
            </q-item>

          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
const colours = [
  "red",
  "pink",
  "purple",
  "deep-purple",
  "indigo",
  "blue",
  "light-blue",
  "cyan",
  "teal",
  "green",
  "light-green",
  "lime",
  "yellow",
  "amber",
  "orange",
  "deep-orange",
  "brown",
  "grey",
  "blue-grey",
];

function rgb2hex(rgb) {
    rgb = rgb.match(/^rgb\((\d+),\s*(\d+),\s*(\d+)\)$/);
    function hex(x) {
        return ("0" + parseInt(x).toString(16)).slice(-2);
    }
    return "#" + hex(rgb[1]) + hex(rgb[2]) + hex(rgb[3]);
}

import { copyToClipboard } from 'quasar'

export default {
  computed: {
    target() {
      if (this.targetEl) {
        this.$refs.tooltip.updatePosition();
        return this.targetEl;
      }
    },
    tooltipTextColour() {
      if (this.selectedSwatch) {
        return this.getTextColour(this.selectedScale);
      }
    },
    dialogTextColor() {
      if(this.input.scale) {
        return this.getTextColour(this.input.scale)
      }
    }
  },
  data() {
    return {
      colours,
      selectedScale: undefined, // The scale value of the class name Eg. 1 - 10. This will determine whether to show text-black or text-white in the tooltip for better visibility
      selectedSwatch: undefined, // E.g orange-12
      targetEl: undefined,
      input: {
        scale: '',
        hex: '',
        swatch: ''
      },
      /* Dialog */
      showDialog: false,
    };
  },
  methods: {
    onClick(c,i) {
      this.input.swatch = `${c}-${i}`
      let id = `swatch-${c}-${i}`
      let el = document.getElementById(id);
      let style = getComputedStyle(el);
      this.input.hex = rgb2hex(style.color)
      this.input.scale = i
      this.showDialog = true
    },
    onMouseOver(c, i) {
      this.selectedScale = i;
      this.selectedSwatch = `${c}-${i}`;
      this.targetEl = `#swatch-${c}-${i}`;
    },
    getTextColour(val) {
      if (val < 6) {
        return "text-black";
      } else {
        return "text-white";
      }
    },
    copy(val) {
      copyToClipboard(val)
      .then(() => {
        // success!
        this.$q.notify({
          type: 'positive',
          position: 'center',
          message: 'Copied to clipboard!'
        })      
      })
      .catch(() => {
      // fail
      })
    }
  },
};
</script>