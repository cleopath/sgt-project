<template>
  <section class="thank-you-section">
    <div class="container">
      <h1 class="thank-you-text">Thank you</h1>
    </div>
  </section>
</template>

<script>
export default {
  name: 'EtcSection'
}
</script>

<style lang="scss" scoped>
/* Variabel warna sesuai identitas desain sebelumnya */
$accent-color: #ccff00; 
$bg-dark: #0f1a0f;

.thank-you-section {
  height: 100vh; /* Full layar */
  width: 100%;
  background-color: $bg-dark;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.thank-you-text {
  color: $accent-color;
  font-family: 'Georgia', serif; /* Font serif sesuai referensi */
  font-style: italic;
  font-weight: 400;
  font-size: 5rem; /* Ukuran besar sesuai gambar */
  text-align: center;
  margin: 0;
  
  /* Animasi halus agar tidak kaku saat muncul */
  animation: fadeIn 1.5s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Responsif untuk HP */
@media (max-width: 768px) {
  .thank-you-text {
    font-size: 3rem;
  }
}
</style>