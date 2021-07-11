<template>
<div class="item">
    <div v-if="data.type === 'object' || data.type === 'array'">
        <button @click="toggleOpen" class="data-key" :aria-expanded="state.open ? 'true' : 'false'">
            <span :class="classes"></span>
            {{ data.key }}:
            <span class="properties">{{ lengthString }}</span>
        </button>

        <structure-item
            v-on:selected="bubbleSelected"
            v-for="child in data.children"
            :key="getKey(child)"
            :data="child"
            v-show="open"
            :maxDepth="maxDepth"
            :canSelect="canSelect"
        />

    </div>

    <div v-if="data.type === 'value'"
         :class="valueClasses"
         v-on:click="clickEvent(data)"
         @keyup.enter="clickEvent(data)"
         @keyup.space="clickEvent(data)"
         :role="canSelect ? 'button' : undefined"
         :tabindex="canSelect ? '0' : undefined"
    >

        <span class="value-key">{{ data.key }}:</span>
        <span :style="getValueStyle(data.value)">
            {{ dataValue }}
        </span>

    </div>
</div>
</template>

<script lang="ts">
import {defineComponent,reactive,computed} from "vue";
import { Data } from "../types/Data";
import {SelectedData} from "../interfaces/SelectedData";

export default defineComponent({
    name: 'StructureItem',
    props: {
        data: {
            required: true,
            type: Object
        },
        maxDepth: {
            type: Number,
            required: false,
            default: 1
        },
        canSelect: {
            type: Boolean,
            required: false,
            default: false
        }
    },
    setup(props,context) {

        const state = reactive({
            open: props.data.depth < props.maxDepth
        })

        function toggleOpen() {
            state.open = !state.open;
        }

        function clickEvent(data: Data): void {
            context.emit('selected',{
                key: data.key,
                value: data.value,
                path: data.path
            } as SelectedData)
        }

        function bubbleSelected(data: Data): void {
            context.emit('selected',data)
        }

        function getKey(value: any): string {
            if(!isNaN(value.key)) {
                return value.key + ':'
            } else {
                return `"${value.key}":`
            }
        }

        function getValueStyle(value: any): object {
            const type = typeof value;
            switch (type) {
                case 'string':
                    return { color: 'var(--string-color)' };
                case 'number':
                    return { color: 'var(--number-color)' };
                case 'boolean':
                    return { color: 'var(--boolean-color)' };
                case 'object':
                    return { color: 'var(--null-color)' };
                case 'undefined':
                    return { color: 'var(--null-color)' };
                default:
                    return { color: 'var(--valueKey-color)' };
            }
        }

        const classes = computed(() => {
            return {
                'chevron-arrow': true,
                opened: state.open
            }
        });
        const valueClasses = computed(() => {
            return {
                'value-key': true,
                'can-select': props.canSelect
            }
        });
        const lengthString = computed(() => {
            if (props.data.type === 'array') {
                return props.data.length === 1
                    ? props.data.length + ' element'
                    : props.data.length + ' elements';
            }
            return props.data.length === 1
                ? props.data.length + ' property'
                : props.data.length + ' properties';
        });
        const dataValue = computed(() => {
            if (typeof props.data.value === 'undefined') {
                return 'undefined';
            }
            return JSON.stringify(props.data.value);
        });

        return { state,toggleOpen,clickEvent,bubbleSelected,getKey,getValueStyle,classes,valueClasses,lengthString,dataValue }
    }
})
</script>

<style scoped>

</style>