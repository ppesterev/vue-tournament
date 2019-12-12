<template>
    <div class="playoff-node">
        <ul v-if="node.children">
            <li v-for="(child, index) in node.children" :key="index">
                <PlayoffTree
                    :node="child"
                    :item-width="itemWidth"/>
            </li>
        </ul>
        <!-- <div class="playoff-node-name" :style="{width: itemWidth - 10 + 'px'}">{{ node.name }}</div> -->
        <input 
            type="text"
            class="playoff-node-name"
            v-model="node.name"
            :style="{width: itemWidth - 10 + 'px'}">
    </div>
</template>

<script>
    export default {
        name: 'TournamentTree',
        props: ['node', 'itemWidth']
    }
</script>

<style>
.playoff-node,
.playoff-node > ul > li {
    position: relative;
    display: flex;
    align-items: center;
    /* background: #eeeeee; */
}

.playoff-node > ul {
    position: relative;
    list-style: none;
    margin: 0;
    padding: 0;
}

.playoff-node-name {
    /* width: 100px; */
    height: 25px;
    margin: 10px 5px;
    padding: 3px;
    box-sizing: border-box;
    border: 1px solid black;
    border-radius: 5px;
}

.playoff-node li:first-child::after,
.playoff-node li:last-child::after{
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

.playoff-node li:last-child::after {
  border-width: 0 1px 1px 0;
  border-radius: 0 0 2px 0;
  transform: translate(0, -50%);
}

.playoff-node > ul::after {
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