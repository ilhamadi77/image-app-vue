<script>
export default {
  data() {
    return {
      images: [],
    };
  },
  mounted() {
    // Ambil daftar gambar dari API atau localStorage
    // Contoh: ambil dari localStorage
    const storedImages = JSON.parse(localStorage.getItem("images")) || [];
    this.images = storedImages;
  },
};
</script>

<template>
  <div class="w-full">
    <h2 class="m-4 font-black text-gray-500 text-4xl">Image Library</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-7 p-7">
      <div
        v-for="image in images"
        :key="image.id"
        class="overflow-hidden rounded-lg shadow-lg hover:scale-110 hover:duration-700"
      >
        <img
          :src="image.url"
          :title="image.name"
          :alt="image.name"
          class="w-full h-auto"
        />
      </div>
    </div>
  </div>
</template>
