<template>
  <div id="app" class="container-fluid">
		<br/>
		<br/>
		<div class="container row">
			<div class="col-sm-4">

				<div class="col-sm-12" style="background-color:#ccc;">
					<form style=" padding: 5px">
						<div class="form-group" >
							<input type="text" v-model="arrDevSave.github_username" name="github_username" id="github_username" class="form-control" placeholder="GitHub User" />
						</div>
						<div class="form-group">
							<input type="text" v-model="arrDevSave.techs" name="techs" id="techs" class="form-control" placeholder="Tecnologias" />
						</div>
						<div class="form-group">
							<input type="text" v-model="arrDevSave.latitude" name="latitude" id="latitude" class="form-control" placeholder="latitude" />
						</div>
						<div class="form-group">
							<input type="text" v-model="arrDevSave.longitude" name="longitude" id="longitude" class="form-control" placeholder="longitude" />
						</div>
						<div class="form-group">
							<input name="btn-cadastrar" @click="saveDev()" class="btn btn-primary" type="button" value="Cadastrar" />
						</div>
					</form>
				</div>
			</div>

			<div class="col-sm-6">
				<div class="row">
					<div 	v-for="(dev, indice) in devList"
					v-bind:key="indice">
						<div class="col-sm-3">
							<div class="card" style="background-color:#ccc; width: 18rem;" >
								<img class="card-img-top"
									:alt="dev.name"
									:src="dev.avatar_url"
								/>
								<div class="card-body">
									<h5 class="card-title">{{dev.name}}</h5>
									<p class="card-text">{{dev.techs.join(', ')}}</p>
									<a href="#" class="btn btn-primary">{{dev.github_username}}</a>
								</div>
							</div>
							<br/>
							<br/>
							</div>
					</div>
				</div>
			</div>
		</div>
  </div>
</template>


<script>
// import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios';


export default {
  name: 'app',
  data() {
    return {
      devList : [],
      arrDevSave : {
				github_username: '',
				techs: '',
				latitude: '',
				longitude: ''
		},
    }
  },

  methods: {
		async saveDev() {
			try {
				const response = await axios.post(
				'http://localhost:3333/devs',
					this.arrDevSave
				);

				alert('Dev cadastrado com suceso!');
				/* eslint no-console: ["error", { allow: ["warn", "error","log"] }] */
				console.log('Dev cadastrado', response);

				this.devList.push(response.data);

			} catch (error) {
				/* eslint no-console: ["error", { allow: ["warn", "error","log"] }] */
				console.error('Erro ao cadastrar o dEV', error);

			}

		},

		async searchDev() {

			try {
				const response = await axios.get(
					'http://localhost:3333/devs'
				);

				this.devList = response.data;

			} catch (error) {
				/* eslint no-console: ["error", { allow: ["warn", "error","log"] }] */
				console.error('Erro ao recuperar dados', error);
			}

		}
  },

	async mounted() {
		navigator.geolocation.getCurrentPosition(
			(position) => {
				console.log(position);

				const {latitude, longitude} = position.coords;

				this.arrDevSave.latitude = latitude;
				this.arrDevSave.longitude = longitude;

			},
			(err) => {
				console.error(err)
			},
			{
				timeout:30000
			}
			,[]
		);

		this.searchDev();

	}
}
</script>

<style scoped>
	.image-size {
		width: 50%;
	}
</style>