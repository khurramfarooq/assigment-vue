<template>
	<div>
		
		<p>
			<button class="btn btn-primary" style="width:80%;" type="button" data-toggle="collapse" :data-target="'#interface'+index"" aria-expanded="false" aria-controls="collapseExample">
				{{card_data.card_name}}
			</button>
		</p>
		<div class="collapse" :id="'interface'+index">
			<div class="card card-body">
			<table class="table table-striped">
				<thead>
					<th>Interface</th>
					<th>Status</th>
					<th>Remote interface</th>
					<th>Connected Router</th>
				</thead>
				<tbody>
					<tr v-for="inter in card_data.interface">
						<td>
							<span style="cursor:pointer;" v-on:click="getRouterByName(inter.connected_router)">{{inter.local_interface}}</span>
						</td>
						<td v-if="inter.status == 'Up'">
							<span class="green"></span>
						</td>
						<td v-else>
						  <span class="red"></span>
						</td>
						<td v-if="inter.remote_interface">
							{{inter.remote_interface}}
						</td>
						<td v-else>
						  N/A
						</td>
						<td v-if="inter.connected_router">
							{{inter.connected_router}}
						</td>
						<td v-else>
						  N/A
						</td>
					</tr>
				</tbody>
			</table>
			</div>
			</div>
	 </div>
</template>

<script>
export default {
  name: 'InterfaceComp',
  props: ['card_data', 'index'],
  methods: {
	  getRouterByName(name) {
		  this.$emit('get-router', name);
	  }
  },
  data () {
    return {
      
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
