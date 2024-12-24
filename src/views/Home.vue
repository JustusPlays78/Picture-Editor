<template>
  <div class="flex flex-col items-center bg-gray-900 text-white min-h-screen p-4">
    <h1 class="text-2xl mb-4">Photos</h1>
    <input type="file" @change="handleFileChange" class="mb-4 bg-gray-800 text-white p-2 rounded" />
    <div class="relative">
      <canvas ref="canvas" class="border border-gray-700" />
      <img :src="imageSrc" alt="Image" class="absolute inset-0 object-cover" v-if="imageSrc" />
      <div class="absolute inset-0 flex items-end justify-center">
        <input
          v-model="text"
          placeholder="Enter text to overlay"
          class="bg-gray-800 text-white p-2 rounded shadow"
        />
      </div>
    </div>
    <div class="mt-4">
      <button @click="downloadPhoto" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">Download Photo</button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      imageSrc: '',
      text: ''
    };
  },
  watch: {
    text() {
      this.draw();
    }
  },
  methods: {
    handleFileChange(event: Event) {
      const target = event.target as HTMLInputElement;
      const file = target.files?.[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imageSrc = e.target?.result as string;
          this.$nextTick(() => {
            this.draw();
          });
        };
        reader.readAsDataURL(file);
      }
    },
    draw() {
      const canvas = this.$refs.canvas as HTMLCanvasElement;
      const ctx = canvas.getContext('2d');
      const img = new Image();
      img.src = this.imageSrc;
      img.onload = () => {
        if (ctx) {
          canvas.width = img.width;
          canvas.height = img.height;
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          ctx.drawImage(img, 0, 0);
          ctx.font = '30px Arial';
          ctx.fillStyle = 'white';
          ctx.textAlign = 'center';
          ctx.fillText(this.text, canvas.width / 2, canvas.height - 30);
        }
      };
    },
    downloadPhoto() {
      const canvas = this.$refs.canvas as HTMLCanvasElement;
      const link = document.createElement('a');
      link.href = canvas.toDataURL('image/png');
      link.download = 'photo_with_caption.png';
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }
  }
}
</script>

<style scoped>
canvas {
  display: block;
}
</style>