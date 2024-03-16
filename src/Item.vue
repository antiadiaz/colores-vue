<script setup>
    defineProps(["id","r","g","b"])
    const emit = defineEmits(["borrar"])
</script>

<template>
    <li @click="emit('borrar',id)" :style="{backgroundColor : `rgb(${[r,g,b].join(',')})`}">{{r}},{{g}},{{b}}</li>
</template>