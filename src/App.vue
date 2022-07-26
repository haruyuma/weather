<template>
	<div id="app">
		<main>
			<div class="search-box">
				<input type="text" name="input_city" class="search-bar" placeholder="都市名を入力してください" v-model="query" @keypress="fetchWeather"/>
			</div>

			<div class="weather-wrap" v-if="typeof  weather.main != 'undefined'">
				<div class="location-box">
					<div class="info">都市名 {{weather.name}}</div>
					<div class="info">気温 {{Math.round(weather.main.temp)}}℃</div>
					<div class="info">湿度 {{weather.main.humidity}}%</div>
					<div class="info">天候 <span style="position: relative; top:15px;"><img :src="getweatherIcon"></span></div>
				</div>
			</div>
			<div class="weather-wrap" v-if="typeof  weather.main == 'undefined' && flg == false">
				<div class="location-box">
					<div class="info">入力した都市がありません</div>
					<div class="info">再度入力してください</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
import axios from 'axios';

export default{
	name:'app',
	data(){
		return{
			query: '',
			weather: {},
			flg: true,
			image:'',
		}
	},
	methods: {
		fetchWeather(e){
			if(e.key == "Enter"){
				let url = "https://api.openweathermap.org/data/2.5/weather?q=" + this.query + "&units=metric&APPID=f0c99e31d9b8df5f12540f8e23d5e219";
				axios.get(url)
					.then(res => {
						this.flg = true;
						this.weather = res.data;
						this.image = "http://openweathermap.org/img/w/" + this.weather.weather[0].icon + ".png";
					})
					.catch(error => {
						this.flg = false;
						this.reset(error);
					});
			}
		},
		reset(){
			this.weather = 0;
		},
	},
	computed:{
		getweatherIcon(){
			return this.image;
		},
	},
}
</script>


<style>
	*{
		margin:0;
		padding:0;
		box-sizing: border-box;
	}

	#app{
		background-image: url('./assets/valentin-muller-bWtd1ZyEy6w-unsplash.jpg');
		background-size:cover;
		background-position:bottom;
		min-height: 100vh;
	}

	main{
		padding:25px;
		background-image: liner-gradient(to bottom, rgba(0, 0, 0, 25), rgba(0, 0, 0, 0.75));
	}

	.search-box {
		margin-right: auto;
		margin-left: auto;
		text-align: center;
		width: 60%;
	}

	.search-bar {
		margin-bottom: 5%;
		margin-top: 5%;
		text-align: center;
		width: 100%;
		padding: 10px;
		color: #313131;
		font-family: "游明朝", YuMincho, "Hiragino Mincho ProN W3", "ヒラギノ明朝 ProN W3", "Hiragino Mincho ProN", "HG明朝E", "ＭＳ Ｐ明朝", "ＭＳ 明朝", serif;
		font-size: 12px;
		border: none;
		outline: none;
		border-radius: 15px 15px 15px 15px;
	}


	.info{
		color:#FFFFFF;
		font-family: "游明朝", YuMincho, "Hiragino Mincho ProN W3", "ヒラギノ明朝 ProN W3", "Hiragino Mincho ProN", "HG明朝E", "ＭＳ Ｐ明朝", "ＭＳ 明朝", serif;
		font-size:30px;
		font-weight:500;
		text-align:center;
		line-height:2.5;
	}

</style>
