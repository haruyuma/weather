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
					<div class="info">天候 {{weather.weather[0].main}}</div>
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
	}
}
</script>


<style>
	*{
		margin:0;
		padding:0;
		box-sizing: border-box;
	}

	#app{
/*		background-image: url('./assets/S__5627907.jpg');*/
		background-size:cover;
		background-position:bottom;
		
	}

	main{
		min-height: 100vh;
		padding:25px;
		background-image: liner-gradient(to bottom, rgba(0, 0, 0, 25), rgba(0, 0, 0, 0.75));
	}

	.search-box .search-bar{
		width: 100%;
		margin-right: auto;
		margin-left: auto;
		margin-bottom: 30px;
		margin-top: 30px;
		text-align: center;
		display: block;
		width: 60%;
		padding: 10px;

		color: #313131;
		font-family: "游明朝", YuMincho, "Hiragino Mincho ProN W3", "ヒラギノ明朝 ProN W3", "Hiragino Mincho ProN", "HG明朝E", "ＭＳ Ｐ明朝", "ＭＳ 明朝", serif;
		font-size: 25px;
		border: 1px solid;
		outline: none;
		background-color: rgba(255, 255, 255, 0.8);
		border-radius: 15px 15px 15px 15px;
		
	}

	.info{
		color:#000000;
		font-family: "游明朝", YuMincho, "Hiragino Mincho ProN W3", "ヒラギノ明朝 ProN W3", "Hiragino Mincho ProN", "HG明朝E", "ＭＳ Ｐ明朝", "ＭＳ 明朝", serif;
		font-size:35px;
		font-weight:500;
		text-align:center;
		line-height:2.5;
/*		text-shadow:1px 3px rgba(0, 0, 0, 1.5);*/
	}

</style>
