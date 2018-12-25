<template>
  <div id="app">
	<ul class="nav nav-tabs" id="myTab" role="tablist">
	  <li class="nav-item">
		<a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">Routers</a>
	  </li>
	  <li class="nav-item">
		<a class="nav-link" id="profile-tab" data-toggle="tab" href="#connectivity" role="tab" aria-controls="connectivity" aria-selected="false">Connectivity</a>
	  </li>
	</ul>
	
	<div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">
		<div class="dropdown router">
		  <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
			Routers
			  </button>
		  <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
			<a v-on:click="getRouterById" :data-router-id="router.id" v-for="(router, index) in routers.data" :class="info.data[0].id == index + 1 ? 'dropdown-item active' : 'dropdown-item'" href="#">{{router.router_name}}</a>
		  </div>
		</div>
		<h1>{{info.data[0].router_name}}</h1>
		<InterfaceComp v-on:get-router="getRouterByName"  v-for="(card_data, index) in info.data[0].card" v-bind:card_data="card_data" v-bind:index="index"></InterfaceComp>
	</div>
	<div class="tab-pane fade" id="connectivity" role="tabpanel" aria-labelledby="profile-tab">
	<NodeTopology></NodeTopology>
	</div>
  </div>
  
 
</template>

<script>
import InterfaceComp from './components/CardRouter'
import NodeTopology from './components/Connectivity'
import axios from 'axios'
export default {
  name: 'app',
  data(){
	  return {
		  info: null,
		  routers: null
	  }
  },
  mounted() {
	  axios
      .get('http://localhost:8000/routers/1/')
      .then(response => (this.info = response))
	  
	  axios
      .get('http://localhost:8000/routers/')
      .then(response => (this.routers = response))
  },
  methods: {
    getRouterByName(name) {
      axios
      .get('http://localhost:8000/routers/'+name)
      .then(response => (this.info = response))
    },
	getRouterById(event) {
		$('.dropdown-item.active').removeClass('active')
		axios
		  .get('http://localhost:8000/routers/'+event.target.getAttribute('data-router-id')+'/')
		  .then(response => (this.info = response))
		  event.target.classList.add("active");
	},
  },
  components: {
    InterfaceComp,
	NodeTopology,
  }
}

</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.red {
	display: block;
	background: red;
	border-radius: 30px;
	width: 22px;
	height: 22px;
	margin-left: 40%;
}
.green {
	display: block;
	background: green;
	border-radius: 30px;
	width: 22px;
	height: 22px;
	margin-left: 40%;
}
.router {
	position: absolute;
    left: 20px;
	margin-top: 10px;
}
</style>
