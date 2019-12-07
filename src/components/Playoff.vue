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
        <div
            ref="container"
            class="playoff-container"
            @wheel.prevent="zoom"
            :style="{transform: scaleStyle}">
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
            },

            zoom(evt) {
                if(evt.deltaY > 0 && this.scaleIndex > 0) {
                    this.scaleIndex--;
                }
                else if(evt.deltaY < 0 && this.scaleIndex < this.scaleFactors.length - 1) {
                    this.scaleIndex++;
                }
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
            },

            scaleStyle() {
                return 'scale(' + this.scaleFactors[this.scaleIndex] + ')';
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
                playoffData: null,

                scaleFactors: [0.2, 0.3, 0.5, 0.75, 1, 1.25, 1.5, 2, 3],
                scaleIndex: 4
            }
        },

        mounted() {
            this.playoffData = this.generateNodes(this.depth);
        }
    }
</script>

<style>
    .playoff {
        padding: 10px;
    }

    .playoff-container {
        width: 1000px;
        transform-origin: 0 0;
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