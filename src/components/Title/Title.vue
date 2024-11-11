<template>
    <div>
        <p  v-bind:style="{ textAlign: align}">{{text}}</p>
    </div>
</template>

<script>
export default {
    props: {
        text: String,
        align: String
    },
    name: 'TitleInfo',
}
</script>

<style>
</style>