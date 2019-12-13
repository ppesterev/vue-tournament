<template>
    <div class="tournament-node" :class="{'loser-bracket': node.isLoserBracket}">
        <ul v-if="node.children">
            <li v-for="(child, index) in node.children" :key="index">
                <TournamentTree
                    :node="child"
                    :item-width="itemWidth"/>
            </li>
        </ul>
        <!-- <div class="tournament-node-name" :style="{width: itemWidth - 10 + 'px'}">{{ node.name }}</div> -->
        <input 
            type="text"
            class="tournament-node-name"
            v-model="node.name">
    </div>
</template>

<script>
    export default {
        name: 'TournamentTree',
        props: ['node', 'isLoserBracket', 'itemWidth']
    }
</script>

<style>
.tournament-node,
.tournament-node > ul > li {
    position: relative;
    display: flex;
    align-items: center;
    /* background: #eeeeee; */
}

.tournament-node.loser-bracket {
    background-color: #ff442222;
}

.tournament-node > ul > li {
    justify-content: flex-end;
}

.tournament-node > ul {
    position: relative;
    list-style: none;
    margin: 0;
    padding: 0;
}

.tournament-node-name {
    width: 100px;
    height: 25px;
    margin: 10px 5px;
    padding: 3px;
    box-sizing: border-box;
    border: 1px solid black;
    border-radius: 5px;
}

.tournament-node li:first-child::after,
.tournament-node li:last-child::after{
  /* z-index: -5; */
  content: "";
  position: absolute;
  right: 0;
  width: 5px;
  height: 50%;
  border: solid black;
  border-width: 1px 1px 0 0;
  border-radius: 0 2px 0 0;
  transform: translate(0, 50%);
}

.tournament-node li:last-child::after {
  border-width: 0 1px 1px 0;
  border-radius: 0 0 2px 0;
  transform: translate(0, -50%);
}

.tournament-node > ul::after {
  z-index: -5;
  content: "";
  position: absolute;
  right: -5px;
  top: 50%;
  width: 5px;
  height: 0;
  border: solid black;
  border-width: 1px 1px 0 0;
}

</style>