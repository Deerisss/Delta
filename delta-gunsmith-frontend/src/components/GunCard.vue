<template>
  <div class="bg-gray-800 rounded-xl p-6 border border-gray-700 hover:border-yellow-500/50 transition-all shadow-lg">
    <div class="flex justify-between items-start mb-4">
      <div>
        <h3 class="text-xl font-bold text-white">{{ title }}</h3>
        <span class="text-sm text-yellow-500 font-mono">{{ weaponName }}</span>
      </div>
      <span class="text-xs bg-gray-700 px-2 py-1 rounded text-gray-300">
        {{ author }}
      </span>
    </div>

    <div class="flex gap-2 mb-4 flex-wrap">
      <span 
        v-for="tag in tags" 
        :key="tag" 
        class="text-xs bg-gray-900 text-gray-400 px-2 py-1 rounded border border-gray-700"
      >
        #{{ tag }}
      </span>
    </div>

    <div class="bg-gray-900 p-3 rounded-lg flex justify-between items-center group">
      <code class="text-green-400 font-mono text-sm truncate mr-4">
        {{ code }}
      </code>
      <button 
        @click="handleCopy"
        class="bg-yellow-600 hover:bg-yellow-500 text-white text-xs px-3 py-2 rounded transition-colors font-bold whitespace-nowrap"
      >
        复制
      </button>
    </div>
  </div>
</template>

<script setup>
// 定义父组件传进来的数据格式
const props = defineProps({
  title: String,
  weaponName: String,
  code: String,
  author: String,
  tags: Array
});

// 处理复制逻辑
const handleCopy = async () => {
  try {
    await navigator.clipboard.writeText(props.code);
    alert(`代码 ${props.code} 已复制！`);
  } catch (err) {
    console.error('复制失败', err);
  }
};
</script>