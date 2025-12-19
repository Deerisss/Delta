<template>
  <div class="min-h-screen p-8 max-w-5xl mx-auto">
    <header class="text-center mb-10">
      <h1 class="text-4xl font-extrabold text-yellow-500 mb-2">
        DELTA FORCE GUNSMITH
      </h1>
      <p class="text-gray-400">三角洲行动 · 战术终端</p>
    </header>

    <div class="mb-12 flex justify-center">
      <input 
        v-model="searchText"
        type="text" 
        placeholder="搜索枪械或方案 (如: M4A1)" 
        class="w-full max-w-lg p-4 rounded-lg bg-gray-800 border border-gray-600 focus:border-yellow-500 focus:outline-none text-white placeholder-gray-500"
      />
    </div>

    <div v-if="loading" class="text-center text-yellow-500">
      正在连接战术数据库...
    </div>

    <div v-else class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <GunCard 
        v-for="item in filteredList" 
        :key="item.id"
        :title="item.title"
        :weaponName="item.weapon_name"
        :code="item.share_code"
        :author="item.author"
        :tags="item.tags"
      />
    </div>
    
    <div v-if="!loading && filteredList.length === 0" class="text-center text-gray-500 mt-10">
      未找到相关数据
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import GunCard from './components/GunCard.vue'; // 引入刚才写的组件

// 状态变量
const searchText = ref('');
const gunData = ref([]);
const loading = ref(true);

// 核心功能：获取后端数据
const fetchBackendData = async () => {
  loading.value = true;
  try {
    // 【重要】这里填你实际的后端 API 地址
    // const res = await fetch('http://localhost:3000/api/guns');
    // const data = await res.json();
    
    // 暂时用模拟数据演示
    setTimeout(() => {
        gunData.value = [
            { id: 1, weapon_name: 'HK416', title: '全距离激光枪', share_code: 'HK-2291-Ops', author: '老兵', tags: ['稳'] },
            { id: 2, weapon_name: 'P90', title: '近战跑打', share_code: 'P90-Rush-B', author: '萌新', tags: ['快'] }
        ];
        loading.value = false;
    }, 500);
    
  } catch (error) {
    console.error("连接数据库失败:", error);
    loading.value = false;
  }
};

// 核心功能：前端过滤搜索
const filteredList = computed(() => {
  if (!searchText.value) return gunData.value;
  const key = searchText.value.toLowerCase();
  return gunData.value.filter(item => 
    item.weapon_name.toLowerCase().includes(key) || 
    item.title.toLowerCase().includes(key)
  );
});

// 页面加载完成后自动触发
onMounted(() => {
  fetchBackendData();
});
</script>