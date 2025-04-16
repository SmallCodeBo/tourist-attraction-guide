<template>
	<view class="container">
		<view class="map-container" id="map"></view>
		<view class="controls">
			<button @click="addCustomLayer" type="primary">添加自定义图层</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				map: null,
				markers: [],
				// 这里需要替换成你的腾讯地图密钥
				key: 'OVEBZ-3GLEN-TOZF3-SHMBW-U7P75-IOBQT'
			}
		},
		onLoad() {
			// 页面加载完成后初始化地图
			this.initMap()
		},
		methods: {
			initMap() {
				// 动态加载腾讯地图SDK
				const script = document.createElement('script')
				script.src = `https://map.qq.com/api/gljs?v=1.exp&key=${this.key}`
				script.onload = () => {
					this.createMap()
				}
				document.head.appendChild(script)
			},
			createMap() {
				// 创建地图实例
				this.map = new TMap.Map('map', {
					center: new TMap.LatLng(39.909186, 116.397411), // 默认北京中心
					zoom: 12,
					viewMode: '2D'
				})
			},
			addCustomLayer() {
				if (!this.map) return
				
				// 创建自定义图层
				const customLayer = new TMap.CustomLayer({
					// 自定义图层的渲染函数
					render: (ctx, map) => {
						// 这里可以添加自定义的绘制逻辑
						ctx.fillStyle = 'rgba(255, 0, 0, 0.3)'
						ctx.beginPath()
						ctx.arc(100, 100, 50, 0, Math.PI * 2)
						ctx.fill()
					}
				})
				
				// 将图层添加到地图
				this.map.addLayer(customLayer)
			}
		}
	}
</script>

<style>
	.container {
		width: 100%;
		height: 100vh;
		position: relative;
	}
	
	.map-container {
		width: 100%;
		height: 100%;
	}
	
	.controls {
		position: absolute;
		bottom: 20px;
		left: 50%;
		transform: translateX(-50%);
		z-index: 100;
	}
</style>
