<template>
    <div class="playoff-node">
        <ul v-if="children">
            <li v-for="child in children" :key="child.name">
                <PlayoffTree
                    :name="child.name"
                    :children="child.children"/>
            </li>
        </ul>
        <div class="playoff-node-name">{{ name }}</div>
    </div>
</template>

<script>
    export default {
        name: 'PlayoffTree',
        props: ['name', 'children']
    }
</script>

<style>
.playoff-node,
.playoff-node > ul > li {
    position: relative;
    display: flex;
    align-items: center;
}

.playoff-node > ul {
    position: relative;
    list-style: none;
    margin: 0;
    padding: 0;
}

.playoff-node-name {
    width: 100px;
    height: 20px;
    margin: 10px 5px;
    padding: 3px;
    border: 1px solid black;
    border-radius: 5px;
}

.playoff-node li:first-child::after,
.playoff-node li:last-child::after{
  z-index: -5;
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