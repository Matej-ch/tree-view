<template>
  <structure-view :data="data" v-on:selected="itemSelected" />
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import StructureView from "./components/StructureView.vue";

export default defineComponent({
  name: 'App',
  components: {
      StructureView
  },
    methods: {
        itemSelected: function () {

        }
    },
    computed: {
      data: function () {
          return {
              first: 'level',
              works: true,
              number: 100,
              missing: null,
              undefined: undefined,
              list: [
                  'fun',
                  {
                      test: {
                          passed: true
                      }
                  }
              ],
              object: {
                  working: 'properly'
              }
          };
      }
    }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
