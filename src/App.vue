<template>
	<div class="body" :style="{'width':bodyWidth+'px'}">
		<table>
			<tr>
				<td>高度格子数</td>
				<td><a-input-number v-model:value="inputHight" :min="1" /></td>
			</tr>
			<tr>
				<td>宽度格子数</td>
				<td><a-input-number v-model:value="inputWidth" :min="1" /></td>
			</tr>
		</table>
		<a-divider />
		<table>
			<tr v-for="row in inputHight" :key="row">
				<td v-for="col in inputWidth" :key="col" @click="cellClick(row,col)" :class="isSelected(row,col)==true?'tableSelected':'tableShown'">
					{{ row }}:{{ col }}
				</td>
			</tr>
		</table>
		<a-button type="link" style="margin-top: 30px;" @click="clear">清空</a-button>
		<a-divider />
		<v-md-preview :text="containerText" class="codeText" style="width: 100%;"></v-md-preview>
		<v-md-preview :text="cellText" class="codeText" style="width: 100%;"></v-md-preview>
	</div>
</template>

<script>
export default {
	data() {
		return {
			bodyWidth:1000,

			inputHight:1,
			inputWidth:1,

			firstClick:null,
			secondClick:null,
			containerText:`
- container
  \`\`\`css
  .container{
    /* width: ; */
    /* height: ; */
    /* grid-gap: ; */

    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-template-rows: repeat(1, 1fr);
  }
  \`\`\`
`,
cellText:`
- cell
  \`\`\`css
  .cell{
    grid-auto-columns: 1fr;
    grid-auto-rows: 1fr;
	/* grid-row: ?; */
	/* grid-column: ?; */
  }
  \`\`\`
`
		}
	},
	methods: {
		changeCellCode(){
			if(this.secondClick!=null){
				this.cellText=`
- cell
  \`\`\`css
  .cell{
    grid-auto-columns: 1fr;
    grid-auto-rows: 1fr;
    grid-row: ${this.firstClick.row}/${this.secondClick.row+1};
    grid-column: ${this.firstClick.col}/${this.secondClick.col+1};
  }
  \`\`\`
`
			}else{
				this.cellText=`
- cell
  \`\`\`css
  .cell{
    grid-auto-columns: 1fr;
    grid-auto-rows: 1fr;
	/* grid-row: ?; */
	/* grid-column: ?; */
  }
  \`\`\`
`
			}
		},
		changeContainerCode(){
			this.containerText=`
- container
  \`\`\`css
  .container{
    /* width: ; */
    /* height: ; */
    /* grid-gap: ; */

    display: grid;
    grid-template-columns: repeat(${this.inputWidth}, 1fr);
    grid-template-rows: repeat(${this.inputHight}, 1fr);
  }
  \`\`\`
			`
		},
		clear(){
			this.firstClick=null;
			this.secondClick=null;
		},
		isBetween(num1,num2,target){
			if (num1 > num2) {
				[num1, num2] = [num2, num1];
			}
			return target >= num1 && target <= num2;
		},
		isSelected(row,col){
			if(this.firstClick==null && this.secondClick==null){
				return false;
			}else if(this.firstClick!=null && this.secondClick==null){
				if(row==this.firstClick.row && col==this.firstClick.col){
					return true;
				}else{
					return false;
				}
			}else if(this.firstClick!=null && this.secondClick!=null){
				if(this.isBetween(this.firstClick.row,this.secondClick.row,row) && this.isBetween(this.firstClick.col,this.secondClick.col,col)){
					return true;
				}else{
					return false;
				}
			}
			return false;
		},
		cellClick(row,col){
			if(this.firstClick==null){
				this.firstClick={
					row: row,
					col: col,
				}
				console.log(this.firstClick);
			}else if(this.firstClick!=null && this.secondClick==null){
				this.secondClick={
					row: row,
					col: col,
				}
			}
		},
		pageAdapt(){
			var width=window.innerWidth;
			if(width<1000){
				this.bodyWidth=width;
			}else{
				this.bodyWidth=1000;
			}
		}
	},
	watch: {
		inputWidth:function(){
			this.changeContainerCode();
		},
		inputHight:function(){
			this.changeContainerCode();
		},
		secondClick:function(){
			this.changeCellCode();
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
.codeText{
	text-align: left;
}
.tableSelected{
	background-color: red;
	padding-bottom: 10px;
	user-select: none;
	color: white;
	border: 3px solid white;
}
.tableShown:hover{
	background-color: rgb(207, 135, 0);
	cursor: pointer;
}
.tableShown{
	background-color: orange;
	padding-bottom: 10px;
	user-select: none;
	color: white;
	border: 3px solid white;
	transition: all ease-in-out .3s;
}
td{
	padding-right: 10px;
	padding-left: 10px;
	padding-top: 10px;
}
.body{
	display: flex;
	flex-direction: column;
	align-items: center;
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