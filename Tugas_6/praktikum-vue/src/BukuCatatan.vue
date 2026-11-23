<script setup>
import { ref, computed } from 'vue'

const judulBuku = ref('')
const kategoriBuku = ref('Novel')
const statusBaca = ref(false)
const daftarBuku = ref([])

const tambahBuku = () => {
  if (judulBuku.value.trim() === '') return
  daftarBuku.value.push({
    id: Date.now(),
    judul: judulBuku.value,
    kategori: kategoriBuku.value,
    selesai: statusBaca.value
  })
  judulBuku.value = ''
  statusBaca.value = false
}

const totalSelesai = computed(() => daftarBuku.value.filter(buku => buku.selesai).length)
const totalBelumSelesai = computed(() => daftarBuku.value.length - totalSelesai.value)

const ubahStatus = (buku) => {
  buku.selesai = !buku.selesai
}

const hapusBuku = (id) => {
  daftarBuku.value = daftarBuku.value.filter(buku => buku.id !== id)
}

const kategoriIcon = (kategori) => {
  return {
    Novel: 'NOV',
    Edukasi: 'EDU',
    Teknologi: 'TEC',
    Fiksi: 'FIK'
  }[kategori] ?? 'BUK'
}
</script>

<template>
  <div class="book-app">
    <header class="app-header">
      <div>
        <p class="eyebrow">PERPUSTAKAAN PRIBADI</p>
        <h1>Catatan Buku</h1>
      </div>
      <div class="header-count">
        <span>{{ daftarBuku.length }}</span>
        <small>koleksi buku</small>
      </div>
    </header>

    <section class="stats" aria-label="Ringkasan koleksi">
      <article class="stat-card stat-total">
        <span class="stat-icon">Bk</span>
        <div>
          <strong>{{ daftarBuku.length }}</strong>
          <p>Total Buku</p>
        </div>
      </article>
      <article class="stat-card stat-done">
        <span class="stat-icon">&#10003;</span>
        <div>
          <strong>{{ totalSelesai }}</strong>
          <p>Selesai Dibaca</p>
        </div>
      </article>
      <article class="stat-card stat-progress">
        <span class="stat-icon">&#9716;</span>
        <div>
          <strong>{{ totalBelumSelesai }}</strong>
          <p>Belum Selesai</p>
        </div>
      </article>
    </section>

    <div class="dashboard">
      <aside class="form-card">
        <div class="card-heading">
          <span class="heading-icon">+</span>
          <div>
            <p>Tambah koleksi</p>
            <h2>Buku Baru</h2>
          </div>
        </div>

        <form @submit.prevent="tambahBuku">
          <label>
            <span>Judul Buku</span>
            <input
              type="text"
              v-model="judulBuku"
              placeholder="Contoh: Laskar Pelangi"
              required
            />
          </label>

          <label>
            <span>Kategori</span>
            <select v-model="kategoriBuku">
              <option value="Novel">Novel</option>
              <option value="Edukasi">Edukasi</option>
              <option value="Teknologi">Teknologi</option>
              <option value="Fiksi">Fiksi</option>
            </select>
          </label>

          <label class="check-row">
            <input type="checkbox" v-model="statusBaca" />
            <span class="check-box">&#10003;</span>
            <span>Sudah selesai dibaca</span>
          </label>

          <button class="submit-button" type="submit">
            <span>Tambah ke Koleksi</span>
            <span aria-hidden="true">&rarr;</span>
          </button>
        </form>
      </aside>

      <section class="library-card">
        <div class="library-heading">
          <div>
            <p>Rak buku</p>
            <h2>Daftar Koleksi</h2>
          </div>
          <span class="library-badge">{{ daftarBuku.length }} buku</span>
        </div>

        <div v-if="daftarBuku.length === 0" class="empty-state">
          <div class="empty-icon">Bk</div>
          <h3>Belum ada buku</h3>
          <p>Tambahkan buku pertamamu untuk mulai membuat catatan.</p>
        </div>

        <ul v-else class="book-list">
          <li v-for="buku in daftarBuku" :key="buku.id" :class="{ 'is-done': buku.selesai }">
            <div class="book-cover" :class="buku.kategori.toLowerCase()">
              {{ kategoriIcon(buku.kategori) }}
            </div>
            <div class="book-info">
              <h3>{{ buku.judul }}</h3>
              <span class="category">{{ buku.kategori }}</span>
            </div>
            <button
              type="button"
              class="status-button"
              :class="{ complete: buku.selesai }"
              @click="ubahStatus(buku)"
            >
              {{ buku.selesai ? 'Selesai' : 'Belum selesai' }}
            </button>
            <button
              type="button"
              class="delete-button"
              aria-label="Hapus buku"
              title="Hapus buku"
              @click="hapusBuku(buku.id)"
            >
              &times;
            </button>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>