<template>
    <button type="button" class="flex items-center justify-center gap-x-2 outline-none px-7 py-3 rounded-md" :class="props.buttonStyle === 'primary' && 'bg-yellow-400 text-black font-bold text-sm hover:bg-yellow-300',
        props.buttonStyle === 'success' && 'bg-green-700 text-white hover:bg-green-600',
        props.buttonStyle === 'warning' && 'bg-orange-600 text-white hover:bg-orange-500',
        props.buttonStyle === 'danger' && 'bg-red-700 text-white hover:bg-red-600'
        " :disabled="props.disabled">
        <slot name="icon-left" />
        <slot />
        <slot name="icon-right" />
    </button>
</template>

<script setup lang="ts">
const props = defineProps({
    buttonStyle: {
        type: String,
        required: false,
    },
    disabled: {
        type: Boolean,
        required: false,
        default: false,
    },
    text: {
        type: String,
        required: false,
    },
})
</script>