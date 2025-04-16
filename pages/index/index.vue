<template>
  <view class="container">
    <!-- 地图组件 -->
    <map
      id="myMap"
      :latitude="center.latitude"
      :longitude="center.longitude"
      :scale="scale"
      :show-location="true"
      style="width: 100%; height: 80vh;"
    ></map>

    <!-- 操作按钮 -->
    <view class="button-group">
      <button @click="addOverlay">添加覆盖层</button>
    </view>
  </view>
</template>
<script setup>
import { ref, onMounted } from 'vue';

// 地图中心点
const center = ref({
  latitude: 39.916344,  // 默认故宫中心坐标
  longitude: 116.397155
});

// 地图缩放级别
const scale = ref(15);

// 地图上下文对象
const mapContext = ref(null);
// 覆盖层ID

// 地图准备好时触发
const onMapReady = () => {
  mapContext.value = uni.createMapContext('myMap');
  console.log(mapContext.value,'地图初始化完成');
};


// 添加覆盖层
const addOverlay = () => {
	console.log(mapContext.value,'start----------')
  if (!mapContext.value) return;
  try {
	  // 1. 准备图片（可选步骤，确保图片可用）
	  mapContext.value.addGroundOverlay({
	    id: 12,
	    src: '/static/gg.jpg', // 你的覆盖图片路径
	    bounds: {
	      southwest: {
	        latitude: 39.913142,  // 西南角纬度
	        longitude: 116.391231 // 西南角经度
	      },
	      northeast: {
	        latitude: 39.919546,  // 东北角纬度
	        longitude: 116.403079 // 东北角经度
	      }
	    },
	    opacity: 1,
	    zIndex: 10,
	    success: (res) => {
	      console.log('添加覆盖层成功', res);
	      uni.showToast({ title: '添加成功', icon: 'success' });
	    },
	    fail: (err) => {
	      console.error('添加覆盖层失败', err);
	      uni.showToast({ title: '添加失败', icon: 'none' });
	    }
	  });
	  
  } catch(error) {
	  console.error('图片准备失败:', error);
  }
  
};

onMounted(() => {
	onMapReady()
  // 可以在这里初始化一些数据
});
</script>