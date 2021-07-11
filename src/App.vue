<template>
    <div :class="{ tree: true }">
        <structure-view
            :data="data"
            rootKey="view"
            :maxDepth="1"
            :styles="{ key: '#0977e6' }"
        />
    </div>

</template>

<script lang="ts">
import {computed, defineComponent} from 'vue'
import StructureView from "./components/StructureView.vue";

export default defineComponent({
    name: 'App',
    components: {
        StructureView
    },

    setup(props) {

        const data = computed(() => {
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
        })

        return {
            data
        };
    },
})
</script>

<style lang="scss">
#app {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-size: 20px;
    padding: 20px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
}

.tree {
    padding: 5px;
}

</style>
