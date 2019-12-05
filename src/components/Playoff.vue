<template>
    <div class="playoff">
        <label> Bracket size: <input type="text"></label>
        <PlayoffTree 
            :v-if="playoffData"
            :name="playoffData.name"
            :children="playoffData.children"/>
    </div>
</template>

<script>
    import PlayoffTree from './PlayoffTree.vue'

    export default {
        name: 'Playoff',
        components: {
            PlayoffTree
        },
        methods: {
            generateNodes(depth) {
                let node = {};
                node.name = '';
                if(depth > 0) {
                    node.children = [];
                    node.children[0] = this.generateNodes(depth - 1);
                    node.children[1] = this.generateNodes(depth - 1);
                }
                return node;
            }
        },
        data() {
            return {
                playoffData: null //{
                    // name: 'Alex',
                    // children:
                    // [
                    //     {
                    //         name: 'Amos',
                    //         children:
                    //         [
                    //             {
                    //                 name: 'Amos',
                    //             },
                    //             {
                    //                 name: 'James'
                    //             }
                    //         ]
                    //     },
                    //     {
                    //         name: 'James',
                    //         children:
                    //         [
                    //             {
                    //                 name: 'Amos',
                    //             },
                    //             {
                    //                 name: 'James'
                    //             }
                    //         ]
                    //     }
                    // ]
                //}
            }
        }
    }
</script>

<style>
    input[type="text"] {
        border: 1px solid grey;
        border-radius: 5px;
    }
</style>