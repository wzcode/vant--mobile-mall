<template>
	<div class="tab_class">
		<div class="tal_class_searchBox">
			<van-search placeholder="点击前往搜索"/>
			<div class="tal_class_searchMask"  @click="$router.push({ name: 'search' })"></div>
		</div>
		<class-tree
			v-if="list.length"
			@class-click="toItemList"
			@all-click="toItemList"
			:list="list"
		></class-tree>
		
		<is-empty v-model="isEmpty">抱歉,店主还未上架商品</is-empty>
	</div>
</template>

<script>
	import { Search } from 'vant';
	import classTree from './tabbar-class-tree';
	import IsEmpty from "@/vue/components/is-empty/";
	import { GOODS_CATEGORY } from '@/api/goods';
	
	export default {
		data(){
			return {
				list: [],
				isEmpty: false
			}
		},
		
		created(){
			this.initData();
		},
		
		methods: {
			initData(){
				const shop_id = this.$util.getLocationParam("shop_id");
				this.$reqGet(`${GOODS_CATEGORY}/${shop_id}`).then(res => {
					const data= this.removeNoChild(res.data.data);
					this.list = data;
					this.isEmpty = !data || !data.length;
				}) 	
			},
			removeNoChild(data){
				return data.filter(item => (item.children && item.children.length))
			},
			toItemList(id = ''){
				this.$router.push({name: "list", query: {keyword: "", itemClass: id}})
			}
		},
		components: {
			[Search.name]: Search,
			[classTree.name]: classTree,
			[IsEmpty.name]: IsEmpty
		}
	}
</script>


<style scoped>
	.tab_class{
		background-color: #fff;
		height: 100%;
	}
	
	.tal_class_searchBox{
		position: relative;
	}
	
	.tal_class_searchMask{
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 9;
	}
</style>