<template>
	<div id="app">
	  <div class="title">
			<h1>Network Topology</h1>  
			<ul class="menu">
			  <li>
				<label> Node size  </label>
				<input type="range" min="1" max="100" v-model='nodeSize' /> {{ options.nodeSize }}
			  </li>
			  <li>
				<label>Render as  </label>
			  <input type="radio" :value='false' v-model='canvas' />
			  <label>SVG</label>
			  <input type="radio" :value='true' v-model='canvas' />
			  <label>Canvas</label>
			  </li>  
			</ul>
		</div>
    <d3-network ref='net' v-on:node-click="getNodeConnectivity"  :net-nodes="nodes" :net-links="links" :options="options" />
  </div>
</template>

<script>
import D3Network from 'vue-d3-network'
import axios from 'axios'
export default {
  name: 'NodeTopology',
  components: {
    D3Network
  },
  data () {
    return {
      nodes: [],
      links: [],
      nodeSize:20,
      canvas:false
    }
  },
  mounted() {
	   var self = this
	   axios
      .get('http://127.0.0.1:8000/connectivity/neighbour/')
      .then(function(response) {
			var nodes = new Array()
			var links = new Array()
			var data = response.data
			for (var i = 0; i < data.length; i++) {
				nodes.push.apply(nodes, data[i].nodes)
				links.push.apply(links, data[i].links)
			}
			self.nodes = nodes
			self.links = links	
	  })
  },
  methods: {
	  getNodeConnectivity(e, node) {
		var self = this
		axios
		.get('http://127.0.0.1:8000/connectivity/neighbour/'+node.name)
		.then(function(response) {
				var nodes = new Array()
				var links = new Array()
				var data = response.data
				for (var i = 0; i < data.length; i++) {
					nodes.push.apply(nodes, data[i].nodes)
					links.push.apply(links, data[i].links)
				}
				if (nodes.length > 0 && links.length > 0) {
					self.nodes = nodes
					self.links = links	
				}
		})
	  }
  },
  computed:{
    options(){
      return {
        force: 3000,
        size:{ w:1500, h:1500},
        nodeSize: this.nodeSize,
        nodeLabels: true,
		linkLabels: true,
        canvas: this.canvas
      }
    }
  },
  
}
</script>

<style src="vue-d3-network/dist/vue-d3-network.css"></style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css?family=PT+Sans');

body{
  font-family: 'PT Sans', sans-serif;
  background-color: #eee;
}
.title{
  position:absolute;
  text-align: center;
  left: 2em;
}
h1,a{
  color: #1aad8d;
  text-decoration: none;
}

ul.menu {
  list-style: none;
  position: absolute;
  z-index: 100;
  min-width: 20em;
  text-align: left;
}
ul.menu li{
  margin-top: 1em;
  position: relative;
}
</style>
