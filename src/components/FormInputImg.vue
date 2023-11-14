<script>
import axios from "axios";

export default {
  data() {
    return {
      imageFile: null,
      API_KEY: "a27f1661f5e93a2710e3065774998e0f",
      errorMessage: null,
      successMessage: null,
      maxSizeInMB: 2,
    };
  },
  methods: {
    handleFileChange(event) {
      this.imageFile = event.target.files[0];
    },
    uploadImage() {
      // Contoh: Memverifikasi tipe file
      const allowedTypes = [
        "image/jpeg",
        "image/png",
        "image/gif",
        "image/jpg",
        "image/bmo",
      ];
      if (!allowedTypes.includes(this.imageFile.type)) {
        this.errorMessage =
          "Format file tidak didukung. Pilih file dengan format JPEG, JPG, PNG, BMO atau GIF.";
        return;
      }

      // Memeriksa ukuran file
      const maxSizeInBytes = this.maxSizeInMB * 1024 * 1024; // Konversi ke byte
      if (this.imageFile.size > maxSizeInBytes) {
        this.errorMessage = `Ukuran file terlalu besar. Maksimal ${this.maxSizeInMB}MB.`;
        return;
      }

      // Memulai proses unggah jika semua verifikasi berhasil
      this.errorMessage = null;
      this.uploadImageToServer();
    },
    async uploadImageToServer() {
      try {
        const formData = new FormData();
        formData.append("image", this.imageFile);

        // Ganti URL dengan URL API yang sesuai
        const response = await axios.post(
          "https://api.imgbb.com/1/upload",
          formData,
          {
            headers: {
              "Content-Type": "multipart/form-data",
            },
            params: {
              key: this.API_KEY,
            },
          }
        );
        this.imageFile = response.data.data.title;
        console.log({ image: this.imageFile });

        // Simpan gambar ke localStorage atau database
        const newImage = {
          id: response.data.data.id,
          name: this.imageFile,
          url: response.data.data.url,
        };

        // // Contoh: simpan ke localStorage
        const storedImages = JSON.parse(localStorage.getItem("images")) || [];
        storedImages.push(newImage);
        localStorage.setItem("images", JSON.stringify(storedImages));
        this.successMessage = "Gambar berhasil di upload";
      } catch (error) {
        console.error("Error uploading image:", error);
        this.errorMessage = "upload Error";
      }
    },
  },
};
</script>

<template>
  <div
    class="h-[20vh] flex justify-center items-center border-b-4 border-black"
  >
    <label class="block">
      <span class="sr-only">Choose profile photo</span>
      <input
        required
        @change="handleFileChange"
        type="file"
        class="block w-full text-sm text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-violet-50 file:text-violet-700 hover:file:bg-violet-100"
      />
      <button
        @click="uploadImage"
        class="mt-3 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition duration-300"
      >
        Unggah Gambar
      </button>
      <p v-if="errorMessage" style="color: red">{{ errorMessage }}</p>
      <p v-if="successMessage" style="color: greenyellow">
        {{ successMessage }}
      </p>
    </label>
  </div>
</template>
