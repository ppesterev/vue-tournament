<template>
    <div>
        <input type="text">
        <PlayoffTree></PlayoffTree>
    </div>
</template>

<script>
    import PlayoffTree from './PlayoffTree.vue'

    export default {
        name: 'Playoff',
        components: {
            PlayoffTree
        }
    }
</script>

<style>
    input[type="text"] {
        border: 1px solid grey;
        border-radius: 5px;
    }
</style>