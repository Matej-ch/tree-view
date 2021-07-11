<template>
    <structure-item
        :data="parsed"
        :maxDepth="maxDepth"
        v-on:selected="itemSelected"
        :class="[{ 'root-item': true}]"/>
</template>

<script lang="ts">
import {ref, defineComponent, computed} from 'vue'
import StructureItem from "./StructureItem.vue";

export default defineComponent({
    name: 'StructureView',
    components: {StructureItem},
    props: {
        data: {
            required: true
        },
        rootKey: {
            type: String,
            required: false,
            default: 'root'
        },
        maxDepth: {
            type: Number,
            required: false,
            default: 1
        },
    },
    setup: (props,context) => {

        function build() {

        }

        const parsed: Object = computed(() => {
            if(props.data === 'object') {

            }

            return {
                key: props.rootKey,
                type: 'value',
                path: '',
                depth: 0,
                value: props.data
            }
        })

        function itemSelected(data: object): void {
            context.emit('selected',data)
        }

        return {
            parsed,itemSelected
        };

    }
})
</script>

<style scoped>

</style>