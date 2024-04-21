<script setup lang="ts">
import { ref } from 'vue';

const nodes = ref({});
const edges = ref({});

const findRoot = (nodeId: string): string => {
  let rootId = nodeId;
  while (nodes.value[rootId]?.parent) {
    rootId = nodes.value[rootId].parent;
  }
  return rootId;
};

const addNode = () => {
  const newNodeId = `node${Object.keys(nodes.value).length + 1}`;
  nodes.value[newNodeId] = { name: `Node ${Object.keys(nodes.value).length + 1}` };
};

const unionNodes = () => {
  const nodeA = parseInt(document.getElementById('nodeA').value);
  const nodeB = parseInt(document.getElementById('nodeB').value);
  
  const rootA = findRoot(`node${nodeA}`);
  const rootB = findRoot(`node${nodeB}`);

  if (rootA !== rootB) {
    nodes.value[rootA].parent = rootB; // Set rootB as parent of rootA
    const edgeId = `edge${Object.keys(edges.value).length + 1}`;
    edges.value[edgeId] = { source: rootA, target: rootB };
  } else {
    console.error("Both nodes are already in the same set.");
  }
};

const findRootOfNode = () => {
  const nodeNumber = parseInt(document.getElementById('nodeToFindRoot').value);
  const root = findRoot(`node${nodeNumber}`);
  document.getElementById('rootResult').innerText = `Root of Node ${nodeNumber} is Node ${root.substring(4)}`;
};
</script>

<template>
  <div>
    <button @click="addNode">Add Node</button>
    <button @click="unionNodes">Union</button>
    Node A: <input type="number" id="nodeA">
    Node B: <input type="number" id="nodeB">
    <br>
    <button @click="findRootOfNode">Find Root</button>
    Node Number: <input type="number" id="nodeToFindRoot">
    <span id="rootResult"></span>
    <br>
    <v-network-graph style="height: 80vh;" :nodes="nodes" :edges="edges" />
  </div>
</template>
