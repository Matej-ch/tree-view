<template>
    <structure-item
        :data="parsed"
        :maxDepth="maxDepth"
        :class="[{ 'root-item': true}]"/>
</template>

<script lang="ts">
import {defineComponent, computed} from 'vue'
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

        function build(key: string,
                       val: any,
                       depth: number,
                       path: string,
                       includeKey: boolean): Object {

            if (isObject(val)) {
                let children = [];
                for (let [childKey, childValue] of Object.entries(val)) {
                    children.push(
                        build(
                            childKey,
                            childValue,
                            depth + 1,
                            includeKey ? `${path}${key}.` : `${path}`,
                            true
                        )
                    );
                }

                return {
                    key: key,
                    type: 'object',
                    depth: depth,
                    path: path,
                    length: children.length,
                    children: children
                };
            } else if (isArray(val)) {
                let children = [];
                for (let i = 0; i < val.length; i++) {
                    children.push(
                        build(
                            i.toString(),
                            val[i],
                            depth + 1,
                            includeKey ? `${path}${key}[${i}].` : `${path}`,
                            false
                        )
                    );
                }

                return {
                    key: key,
                    type: 'array',
                    depth: depth,
                    path: path,
                    length: children.length,
                    children: children
                };

            } else {
                return {
                    key: key,
                    type: 'value',
                    path: includeKey ? path + key : path.slice(0, -1),
                    depth: depth,
                    value: val
                };
            }
        }

        const parsed: Object = computed(() => {
            if(typeof props.data === 'object') {

                return build(props.rootKey, { ...props.data }, 0, '', true);
            }

            return {
                key: props.rootKey,
                type: 'value',
                path: '',
                depth: 0,
                value: props.data
            }
        })

        function isArray(val:any):boolean {
            return Array.isArray(val)
        }

        function isObject(val:any):boolean {
            return typeof val === 'object' && val !== null && !isArray(val);
        }

        return {
            parsed
        };

    }
})
</script>

<style lang="scss" scoped>
.root-item {
    --key-color: #0977e6;
    --valueKey-color: #073642;
    --string-color: #268bd2;
    --number-color: #2aa198;
    --boolean-color: #cb4b16;
    --null-color: #6c71c4;
    --arrow-size: 6px;
    --arrow-color: #444;
    --hover-color: rgba(0, 0, 0, 0.2);

    margin-left: 0;
    width: 100%;
    height: auto;
}
</style>