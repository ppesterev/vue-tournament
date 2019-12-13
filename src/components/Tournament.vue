<template>
    <div class="tournament">
        <div class="tournament-top-panel"> 
            <label> Bracket size:
                <input
                    type="text"
                    v-model.number="participantCount">
                <span class="warning" v-if="!countIsValid">Bracket size must be a power of 2.</span>
            </label>
        </div>
        <div
            ref="container"
            class="tournament-container"
            @wheel.prevent="zoom"
            :style="{transform: scaleStyle}">
            <TournamentTree 
                v-if="tournamentData"
                :node="tournamentData"
                :item-width="itemWidth"/>
        </div>
    </div>
</template>

<script>
    import TournamentTree from './TournamentTree.vue'

    export default {
        name: 'Tournament',
        components: {
            TournamentTree
        },
        methods: {
            generateSingleElimination(depth) {
                if(isNaN(depth) || depth < 0)
                    return null;

                let node = {name: ''};
                if(depth > 0) {
                    node.children = [];
                    node.children[0] = this.generateSingleElimination(depth - 1);
                    node.children[1] = this.generateSingleElimination(depth - 1);
                }
                return node;
            },

            generateLoserBracket(depth) {
                if(isNaN(depth) || depth < 0)
                    return null;

                let node = { name: '' }; // winner of the major stage
                if (depth > 0) {
                    node.children = [];
                    node.children[0] = {name: ''}; // loser dropped from the corresponding winner round
                    node.children[1] = {
                        name: '', // winner of the minor stage
                        children: [
                            this.generateLoserBracket(depth - 1),
                            this.generateLoserBracket(depth - 1)
                        ]
                    };

                }
                
                return node;
            },

            generateDoubleElimination(depth) {
                if(isNaN(depth) || depth < 0)
                    return null;

                let node =  {
                    name: '',
                    children: [
                        this.generateSingleElimination(this.depth),
                        this.generateLoserBracket(this.depth - 1)
                    ]
                };
                node.children[1].isLoserBracket = true;
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
                this.tournamentData = this.generateDoubleElimination(this.depth);
            }
        },

        data() {
            return {
                participantCount: 8,
                tournamentData: null,

                scaleFactors: [0.2, 0.3, 0.5, 0.75, 1, 1.25, 1.5, 2, 3],
                scaleIndex: 4
            }
        },

        mounted() {
            this.tournamentData = this.generateSingleElimination(this.depth);
        }
    }
</script>

<style>
    .tournament {
        padding: 10px;
    }

    .tournament-container {
        /* width: 1000px; */
        transform-origin: 0 0;
    }

    .tournament,
    .tournament input {
        font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
        font-size: 18px;
    }

    .tournament-top-panel {
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