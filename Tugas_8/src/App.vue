<script setup>
import { computed, ref } from 'vue'

const activeCategory = ref('Semua')
const search = ref('')
const selectedDestination = ref(null)
const savedIds = ref([2])

const categories = ['Semua', 'Pantai', 'Gunung', 'Budaya', 'Kota']

const destinations = [
  {
    id: 1,
    name: 'Raja Ampat',
    location: 'Papua Barat',
    category: 'Pantai',
    duration: '4 hari',
    price: 4800000,
    rating: 4.9,
    image: 'https://images.unsplash.com/photo-1544551763-46a013bb70d5?auto=format&fit=crop&w=900&q=85',
  },
  {
    id: 2,
    name: 'Bromo Tengger',
    location: 'Jawa Timur',
    category: 'Gunung',
    duration: '3 hari',
    price: 1250000,
    rating: 4.8,
    image: 'https://images.unsplash.com/photo-1588668214407-6ea9a6d8c223?auto=format&fit=crop&w=900&q=85',
  },
  {
    id: 3,
    name: 'Pura Uluwatu',
    location: 'Bali',
    category: 'Budaya',
    duration: '2 hari',
    price: 980000,
    rating: 4.7,
    image: 'https://images.unsplash.com/photo-1537996194471-e657df975ab4?auto=format&fit=crop&w=900&q=85',
  },
  {
    id: 4,
    name: 'Borobudur',
    location: 'Yogyakarta',
    category: 'Budaya',
    duration: '2 hari',
    price: 850000,
    rating: 4.9,
    image: 'https://images.unsplash.com/photo-1596402184320-417e7178b2cd?auto=format&fit=crop&w=900&q=85',
  },
]

const filteredDestinations = computed(() => {
  const query = search.value.toLowerCase().trim()
  return destinations.filter((item) => {
    const matchesCategory = activeCategory.value === 'Semua' || item.category === activeCategory.value
    const matchesSearch = !query || `${item.name} ${item.location}`.toLowerCase().includes(query)
    return matchesCategory && matchesSearch
  })
})

const formatPrice = (price) =>
  new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(price)

const toggleSaved = (id) => {
  savedIds.value = savedIds.value.includes(id)
    ? savedIds.value.filter((savedId) => savedId !== id)
    : [...savedIds.value, id]
}
</script>

<template>
  <div class="app-shell">
    <header class="topbar">
      <a class="brand" href="#" aria-label="Jalan Jalan">
        <span class="brand-mark" aria-hidden="true">J</span>
        <span>jalan jalan</span>
      </a>
      <nav class="nav-links" aria-label="Navigasi utama">
        <a class="active" href="#">Temukan</a>
        <a href="#inspirasi">Inspirasi</a>
        <a href="#tentang">Tentang</a>
      </nav>
      <button class="saved-button" type="button" aria-label="Destinasi tersimpan">
        <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 21s-7-4.35-7-10.15C5 7.75 7.15 5 10.2 5c1.25 0 2.25.55 2.8 1.35C13.55 5.55 14.55 5 15.8 5 18.85 5 21 7.75 21 10.85 21 16.65 14 21 12 21Z"/></svg>
        <span>{{ savedIds.length }}</span>
      </button>
    </header>

    <main>
      <section class="hero">
        <div class="hero-copy">
          <p class="eyebrow"><span></span> Eksplorasi Nusantara</p>
          <h1>Temukan tempat<br /><em>yang terasa beda.</em></h1>
          <p class="hero-description">Cerita baru menunggu di setiap sudut Indonesia. Mulai petualanganmu dari sini.</p>
          <a class="hero-button" href="#destinasi">Lihat destinasi <span>↗</span></a>
        </div>
        <div class="hero-visual">
          <div class="sun-disc"></div>
          <div class="island-card">
            <img src="https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&w=900&q=85" alt="Pemandangan pantai tropis" />
            <div class="image-label"><span>01</span><b>Nusa Dua</b></div>
          </div>
        </div>
      </section>

      <section class="destination-section" id="destinasi">
        <div class="section-heading">
          <div>
            <p class="eyebrow">Pilihan minggu ini</p>
            <h2>Destinasi pilihan</h2>
          </div>
          <label class="search-box">
            <svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="11" cy="11" r="6"/><path d="m16 16 4 4"/></svg>
            <input v-model="search" type="search" placeholder="Cari destinasi..." aria-label="Cari destinasi" />
          </label>
        </div>

        <div class="category-tabs" aria-label="Kategori destinasi">
          <button
            v-for="category in categories"
            :key="category"
            type="button"
            :class="{ active: activeCategory === category }"
            @click="activeCategory = category"
          >
            {{ category }}
          </button>
        </div>

        <div v-if="filteredDestinations.length" class="destination-grid">
          <article v-for="item in filteredDestinations" :key="item.id" class="destination-card">
            <div class="card-image">
              <img :src="item.image" :alt="item.name" />
              <button
                class="heart-button"
                :class="{ saved: savedIds.includes(item.id) }"
                type="button"
                :aria-label="savedIds.includes(item.id) ? `Hapus ${item.name} dari tersimpan` : `Simpan ${item.name}`"
                @click="toggleSaved(item.id)"
              >
                <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 21s-7-4.35-7-10.15C5 7.75 7.15 5 10.2 5c1.25 0 2.25.55 2.8 1.35C13.55 5.55 14.55 5 15.8 5 18.85 5 21 7.75 21 10.85 21 16.65 14 21 12 21Z"/></svg>
              </button>
              <span class="duration">{{ item.duration }}</span>
            </div>
            <div class="card-body">
              <p class="card-location">{{ item.location }}</p>
              <h3>{{ item.name }}</h3>
              <div class="card-meta">
                <div><span>Mulai dari</span><strong>{{ formatPrice(item.price) }}</strong></div>
                <div class="rating"><span>★</span> {{ item.rating }}</div>
              </div>
              <button class="detail-button" type="button" @click="selectedDestination = item">Lihat detail <span>→</span></button>
            </div>
          </article>
        </div>
        <div v-else class="empty-state">
          <span>⌕</span>
          <h3>Destinasi tidak ditemukan</h3>
          <p>Coba kata kunci atau kategori lain.</p>
        </div>
      </section>

      <section class="quote-section" id="inspirasi">
        <p>“Perjalanan terbaik adalah yang membuat kita<br />kembali pulang dengan cerita baru.”</p>
        <span>Jalan Jalan Journal — Edisi 08</span>
      </section>
    </main>

    <footer id="tentang">
      <a class="brand" href="#"><span class="brand-mark">J</span><span>jalan jalan</span></a>
      <p>Jelajah dengan rasa ingin tahu.</p>
      <span>© 2026 Jalan Jalan</span>
    </footer>

    <div v-if="selectedDestination" class="modal-backdrop" @click.self="selectedDestination = null">
      <section class="detail-modal" role="dialog" aria-modal="true" :aria-label="`Detail ${selectedDestination.name}`">
        <button class="modal-close" type="button" aria-label="Tutup" @click="selectedDestination = null">×</button>
        <img :src="selectedDestination.image" :alt="selectedDestination.name" />
        <div class="modal-content">
          <p class="card-location">{{ selectedDestination.location }} · {{ selectedDestination.category }}</p>
          <h2>{{ selectedDestination.name }}</h2>
          <p>Paket perjalanan {{ selectedDestination.duration }} dengan pengalaman lokal pilihan dan waktu eksplorasi yang nyaman.</p>
          <div class="modal-price"><span>Mulai dari</span><strong>{{ formatPrice(selectedDestination.price) }}</strong></div>
          <button class="hero-button" type="button" @click="toggleSaved(selectedDestination.id)">
            {{ savedIds.includes(selectedDestination.id) ? 'Hapus dari tersimpan' : 'Simpan perjalanan' }} <span>♥</span>
          </button>
        </div>
      </section>
    </div>
  </div>
</template>
