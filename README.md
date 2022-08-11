# Flowchart

Flowchart & Flowchart designer component for Vue.js([flowchart-react](https://github.com/joyceworks/flowchart-react) for React.js).

[![NPM](https://img.shields.io/npm/v/flowchart-vue.svg)](https://www.npmjs.com/package/flowchart-vue)

English | [中文](https://www.joyceworks.com/2022/02/16/flowchart-vue-readme/)

<img width="801" alt="image" src="https://user-images.githubusercontent.com/5696485/156284211-1bd18756-d785-440a-8292-4fdeb89fca5b.png">

## Usage

```shell script
yarn add flowchart-vue
```

```vue
<template>
    <div id="app">
        <button type="button" @click="$refs.chart.add({id: +new Date(), x: 10, y: 10, name: 'New', type: 'operation', approvers: []})">
            Add
        </button>
        <button type="button" @click="$refs.chart.remove()">
            Del
        </button>
        <button type="button" @click="$refs.chart.editCurrent()">
            Edit
        </button>
        <button type="button" @click="$refs.chart.save()">
            Save
        </button>
        <button type="button" @click="confirmRemoving">
            Confirm removing
        </button>
        <flowchart :nodes="nodes" 
                   :connections="connections" 
                   :removeRequiresConfirmation
                   @editnode="handleEditNode"
                   @dblclick="handleDblClick" 
                   @editconnection="handleEditConnection"
                   @removeConfirmationRequired="initRemovingConfirmation"
                   @save="handleChartSave" ref="chart">
        </flowchart>
    </div>
</template>
<script>
  import Vue from 'vue';
  import FlowChart from 'flowchart-vue';

  Vue.use(FlowChart);

  export default {
    name: 'App',
    data: function() {
      return {
        nodes: [
          // Basic fields
          {id: 1, x: 140, y: 270, name: 'Start', type: 'start'},
          // You can add any generic fields to node, for example: description
          // It will be passed to @save, @editnode
          {id: 2, x: 540, y: 270, name: 'End', type: 'end', description: 'I\'m here'},
        ],
        connections: [
          {
            source: {id: 1, position: 'right'},
            destination: {id: 2, position: 'left'},
            id: 1,
            type: 'pass',
          },
        ],
        showRemovingConfirmation: false,
      };
    },
    methods: {
      handleChartSave(nodes, connections) {
        // axios.post(url, {nodes, connections}).then(resp => {
        //   this.nodes = resp.data.nodes;
        //   this.connections = resp.data.connections;
        //   // Flowchart will refresh after this.nodes and this.connections changed
        // });
      },
      handleEditNode(node) {
        if (node.id === 2) {
          console.log(node.description);
        }
      },
      handleEditConnection(connection) {
      },
      handleDblClick(position) {
        this.$refs.chart.add({
          id: +new Date(),
          x: position.x,
          y: position.y,
          name: 'New',
          type: 'operation',
          approvers: [],
        });
      },
      initRemovingConfirmation() {
        this.showRemovingConfirmation = true;
      },
      confirmRemoving() {
        this.$refs.chart.confirmRemove();
        this.showRemovingConfirmation = true;
      }
    }
  };
</script>
```

See more at [src/views/App.vue](https://github.com/joyceworks/flowchart-vue/blob/master/src/views/App.vue).

## Demo

- [GitHub Pages](https://joyceworks.github.io/flowchart-vue/)
- [CodeSandbox](https://codesandbox.io/s/funny-shaw-971s84)
- [Flowchart Vue Demo](https://github.com/joyceworks/flowchart-vue-demo)
- Development Environment

  ``` shell
  git clone https://github.com/joyceworks/flowchart-vue.git
  cd flowchart-vue
  yarn install
  yarn run serve
  ```
  
  Then open http://localhost:yourport/ in browser.

## API

## Props

Property|Description|Type|Default
-|-|-|-
nodes|Collection of nodes|`Array`|`[{id: 1, x: 140, y: 270, name: 'Start', type: 'start'}, {id: 2, x: 540, y: 270, name: 'End', type: 'end'}]`
connections|Collection of connections|`Array`|`[{source: {id: 1, position: 'right'}, destination: {id: 2, position: 'left'}, id: 1, type: 'pass',          }]`
width|Width of canvas|`String` \| `Number`|`800`
height|Height of canvas|`String` \| `Number`|`600`
locale|Display language, available values: `'en'`, `'zh'`|`String`|`'en'`
readonly|Read-only|`Boolean`|false
render|Custom render function|`null`
editnode|Node double-click event|`(node) => void`|-
editconnection|Connection double-click event|`(connection) => void`|-
save|Save event|`(nodes, connections) => void`|-
dblclick|Background double-click event|`(position: {x: number, y: number}) => void`|-
connect|Connect event|`(node, nodes, connections) => void`|-
disconnect|Disconnect event|`(node, nodes, connections) => void`|-
add|Add node event|`(node, nodes, connections) => void`|-
delete|Delete node event|`(node, nodes, connections) => void`|-
select|Select node event|`nodes => void`|-
selectconnection|Select connection event|`connections => void`|-
render|Node render event, children is a collection of svg elements |`(node: Node, children: { header, title, body, content }) => vod`|-
nodesdragged|Notify which nodes dragging just ended|`(nodes) => void`|-
removeConfirmationRequired|Notify that remove confirmation required. Pass nodes and connections selected to remove.|`(nodes, connections) => void`|-

### Properties.Node

Property|Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Type|Default
-|-|-|-
id|ID|`Number`|`+new Date()`
x|Horizontal position of node|`Number`|-
y|Vertical position of node|`Number`|-
type|Type of node|`String`|`'operation'`
width|Width of node|`Number`|`120`
height|Height of node|`Number`|`60`
approvers|Approvers of node, eg: [{name: 'admin'}]|`Array`|[]

### Properties.Connection

Property|Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Type|Default
-|-|-|-
id|ID|`Number`|`+new Date()`
source|Source of connection|`Object`|-
destination|Destination of connection|`Object`|-
type|Type of connection|`String`|`pass`

### Properties.Connection.Source & Properties.Connection.Destination

Property|Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Type|Default
-|-|-|-
id|Node id|`Object`|-
position|Starting/Ending position of node|`Object`|-
