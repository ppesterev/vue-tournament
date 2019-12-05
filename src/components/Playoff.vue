<template>
    <div class="playoff">
        <label> Bracket size:
            <input type="text" v-model.lazy.number="participantCount">
        </label>
        <PlayoffTree 
            v-if="playoffData"
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
                if(isNaN(depth))
                    return null;

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

        watch: {
            participantCount() {
                let depth = Math.floor(Math.log2(this.participantCount));
                this.playoffData = this.generateNodes(depth);
                this.participantCount = Math.pow(2, depth);
            }
        },

        data() {
            return {
                participantCount: 8,
                playoffData: null
            }
        }
    }
</script>

<style>
    .playoff {
        padding: 10px;
    }

    .playoff,
    .playoff input {
        font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
        font-size: 18px;
    }

    .playoff > label {
        display: block;
        margin-bottom: 10px;
    }

    input[type="text"] {
        border: 1px solid grey;
        border-radius: 5px;
        padding: 2px 5px;
    }
</style>