<template>
	<div class="body" :style="{'width':bodyWidth+'px'}">
		1212
	</div>
</template>

<script>
export default {
	data() {
		return {
			bodyWidth:1000,
		}
	},
	methods: {
		pageAdapt(){
			var width=window.innerWidth;
			if(width<1000){
				this.bodyWidth=width;
			}else{
				this.bodyWidth=1000;
			}
		}
	},
	created() {
		
	},
	mounted(){
		this.pageAdapt();
		window.onresize=()=>{
			this.pageAdapt();
		}
	}
}
</script>

<style scoped>
.body{
	/* width: 1000px; */
	background-color: rgb(216, 216, 216);
}
</style>

<style>
body{
	display: flex;
	justify-content: center;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}
</style>