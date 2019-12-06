<template>
    <div class="playoff">
        <div class="playoff-top-panel"> 
            <label> Bracket size:
                <input
                    type="text"
                    v-model.number="participantCount">
                <span class="warning" v-if="!countIsValid">Bracket size must be a power of 2.</span>
            </label>
        </div>
        <div ref="container">
            <PlayoffTree 
                v-if="playoffData"
                :node="playoffData"
                :item-width="itemWidth"/>
        </div>
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
                if(isNaN(depth) || depth < 0)
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

        computed: {
            depth() {
                return Math.floor(Math.log2(this.participantCount));
            },

            itemWidth() {
                let containerWidth = this.$refs["container"].clientWidth;
                return containerWidth / (this.depth + 1);
            },

            countIsValid() {
                return this.participantCount === Math.pow(2, this.depth);
            }
        },

        watch: {
            participantCount() {                
                this.playoffData = this.generateNodes(this.depth);
            }
        },

        data() {
            return {
                participantCount: 8,
                playoffData: null
            }
        },

        mounted() {
            this.playoffData = this.generateNodes(this.depth);
        }
    }
</script>

<style>
    .playoff {
        width: 600px;
        padding: 10px;
    }

    .playoff,
    .playoff input {
        font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
        font-size: 18px;
    }

    .playoff-top-panel {
        margin-bottom: 10px;
    }

    input[type="text"] {
        border: 1px solid grey;
        border-radius: 5px;
        padding: 2px 5px;
    }

    .warning {
        color: red;
        opacity: 0.7;
        padding: 3px;
        margin: 0 5px;
    }
</style>