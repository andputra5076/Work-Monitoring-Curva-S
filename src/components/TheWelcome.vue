<template>
  <div>
    <button @click="openModal">Tambah</button>

    <!-- Modal untuk form inputan -->
    <div class="modal" :class="{ 'active': isModalOpen }">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <h2 style="text-align: center;">Form Inputan</h2>
        <form @submit.prevent="submitForm">
          <label for="bulan">Bulan:</label>
          <select id="bulan" v-model="formData.bulan" required>
            <option value="">Pilih Bulan</option>
            <option v-for="bulan in dataBulan" :key="bulan" :value="bulan">{{ bulan }}</option>
          </select>
          
          <!-- Input untuk nilai sigmoid -->
          <label for="sigmoid">Nilai Sigmoid:</label>
          <input type="number" id="sigmoid" v-model="formData.sigmoid" required>
          
          <button type="submit">Kirim</button>
        </form>
        <p v-if="isSubmitted" class="success-message">Data berhasil dikirim!</p>
        <p v-if="isError" class="error-message">Terjadi kesalahan. Silakan coba lagi.</p>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  data() {
    return {
      isModalOpen: false,
      isSubmitted: false, // Menyimpan status pengiriman
      isError: false, // Menyimpan status kesalahan
      formData: {
        bulan: '',
        sigmoid: 0 // Properti untuk nilai sigmoid
      },
      dataBulan: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Agu', 'Sep', 'Okt', 'Nov', 'Des']
    };
  },
  methods: {
    openModal() {
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
    },
    // TheWelcome.vue
    submitForm() {
      // Validasi data (contoh: memeriksa apakah nilai sigmoid valid)
      if (this.formData.sigmoid < 0 || this.formData.sigmoid > 1) {
        this.isError = true;
        return; // Keluar dari metode jika ada kesalahan
      }

      // Simulasikan pengiriman data (gantilah dengan logika pengiriman sesungguhnya)
      console.log('Mengirim data:', this.formData);

      // Berikan umpan balik
      this.isSubmitted = true;
      this.isError = false;

      // Reset formulir
      this.formData = {
        bulan: '',
        sigmoid: 0
      };

      // Menunggu sebentar sebelum menutup modal (opsional)
      setTimeout(() => {
        this.closeModal();
        this.isSubmitted = false;
      }, 1500);
    },
  },
};
</script>



<style scoped>
.modal {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

.active {
  display: block;
}

.modal-content {
  background-color: #fff;
  margin: 10% auto;
  padding: 20px;
  border: none;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  max-width: 400px;
  width: 90%;
  border-radius: 10px;
}

.close {
  color: #888;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
  transition: color 0.3s; /* Tambahkan transisi untuk perubahan warna */
}

.close:hover {
  color: red; /* Ubah warna menjadi merah ketika cursor menyentuhnya */
}

form {
  display: flex;
  flex-direction: column;
}

label {
  font-weight: bold;
}

input,
textarea {
  margin: 5px 0;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  background-color: #007BFF;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  margin-top: 10px;
  border-radius: 5px;
}
</style>







