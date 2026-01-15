<template>
  <div class="main-container">

    <div class="pinned-section-wrapper">
      
      <div class="sticky-content">
        
        <div class="top-bar">
          <h2 class="brand">
            <span 
              v-for="(item, index) in ['plan', 'design', 'build']" 
              :key="index"
              :class="['nav-item', { 'active': activeIndex === index }]"
              @click="scrollToSection(index)"
               >
              {{ item }}
            </span>
          </h2>
          <div class="counter">{{ activeIndex + 1 }} / 3</div>
        </div>

        <div class="content-grid">
          
          <div class="text-col">
            
              <div :key="activeIndex" class="text-wrapper">
                <h2 class="main-title">{{ currentData.title }}</h2>
                <p class="desc">{{ currentData.desc }}</p>
              </div>
            
          </div>

          <div class="image-col">
            <div class="image-mask">
             <img 
                v-for="(item, index) in contentList" 
                :key="index" 
                :src="item.img" 
                :class="['project-img', `img-${index}`]"
                ref="images"
              />

              <div class="center-icon">
                <img :src="currentData.icon" alt="Icon" />
              </div>
            </div>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';
import { ScrollToPlugin } from 'gsap/ScrollToPlugin';

// Import Assets
import imgPlan from '@/assets/images/imgi_15_638e3f15b3ed3463ebe6038b_pexels-wendy-wei-14397945-p-1600.jpg';
import imgDesign from '@/assets/images/imgi_20_638e4092e9575c0f9629ae01_walls-p-1600.jpg';
import imgBuild from '@/assets/images/imgi_24_638e45c467fd8f44a5687f97_pexels-cottonbro-studio-5474032-p-2600.jpg';
import iconPlan from '@/assets/images/icon-1.svg';
import iconDesign from '@/assets/images/icon-2.svg';
import iconBuild from '@/assets/images/icon-3.svg';

// Register Plugin
gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);

/// --- DATA KONTEN ---

const contentList = [
  {
    title: "The sitemap of the experience",
    desc: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et.",
    img: imgPlan,
    icon: iconPlan
  },
  {
    title: "Time to paint the room walls",
    desc: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum.",
    img: imgDesign,
    icon: iconDesign
  },
  {
    title: "Magic happens to build it out",
    desc: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et.",
    img: imgBuild,
    icon: iconBuild
  }

];

const activeIndex = ref(0);
const currentData = computed(() => contentList[activeIndex.value]);
let stInstance = null;

// PERBAIKAN: Masukkan argumen index ke dalam fungsi
const scrollToSection = (targetIndex) => {
  const st = ScrollTrigger.getById("mainPin");
  if (st) {
    const total = st.end - st.start;
    const target = st.start + (total * (targetIndex / (contentList.length - 1)));
    gsap.to(window, {
      scrollTo: target,
      duration: 1.2,
      ease: "power2.inOut"
    });
  }
};

onMounted(() => {
  const images = document.querySelectorAll('.project-img');
  const icon = document.querySelector('.center-icon img');

  // Set semua gambar ke kondisi awal
  images.forEach((img, i) => {
    gsap.set(img, { opacity: i === 0 ? 1 : 0, scale: 1, zIndex: i + 5 });
  });
  gsap.set(icon, { scale: 1 });

  stInstance = gsap.timeline({
    scrollTrigger: {
      id: "mainPin",
      trigger: ".pinned-section-wrapper",
      start: "top top",
      end: "+=300%",
      pin: true,
      scrub: 1,
      onUpdate: (self) => {
        const newIndex = Math.min(
          Math.floor(self.progress * contentList.length),
          contentList.length - 1
        );
        if (activeIndex.value !== newIndex) {
          activeIndex.value = newIndex;
        }
      }
    }
  });

  // Animasi pop up untuk gambar pertama
  stInstance.to(images[0], {
    scale: 1.08,
    duration: 0.5,
    ease: "power2.out"
  }, 0)
  .to(images[0], {
    scale: 1,
    duration: 0.5,
    ease: "power2.in"
  }, 0.5);

  // Animasi pop up untuk icon pertama
  stInstance.to(icon, {
    scale: 1.15,
    duration: 0.5,
    ease: "power2.out"
  }, 0)
  .to(icon, {
    scale: 1,
    duration: 0.5,
    ease: "power2.in"
  }, 0.5);

  // Animasi gambar dan icon berikutnya
  images.forEach((img, i) => {
    if (i > 0) {
      gsap.set(img, { opacity: 0, scale: 1.1, zIndex: i + 5 });
      stInstance.to(img, {
        opacity: 1,
        scale: 1.08,
        ease: "power2.out",
        duration: 0.5
      }, i)
      .to(img, {
        scale: 1,
        ease: "power2.in",
        duration: 0.5
      }, i + 0.5);

      // Icon pop up di setiap step
      stInstance.to(icon, {
        scale: 1.15,
        duration: 0.5,
        ease: "power2.out",
        onStart: () => {
          // Ganti src icon sesuai index aktif
          icon.src = contentList[i].icon;
        }
      }, i)
      .to(icon, {
        scale: 1,
        duration: 0.5,
        ease: "power2.in"
      }, i + 0.5);
    }
  });
});

// PERBAIKAN: Gunakan onUnmounted agar tidak error 'unused'
onUnmounted(() => {
  if (stInstance) stInstance.kill();
});
</script>

<style scoped>
/* CSS SAMA SEPERTI SEBELUMNYA */
:root {
  --bg-color: #0f1a0f;    
  --accent-color: #ccff00; 
  --text-dim: #5c7a5c;
}

.main-container {
  background-color: #0f1a0f;
  color: #ccff00;
  font-family: 'Helvetica', sans-serif;
  overflow-x: hidden;
}

.spacer {
  /* height: 50vh; */
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  background: #111;
  font-size: 2rem;
}

.pinned-section-wrapper {
  height: 100vh; 
}

.sticky-content {
  height: 100vh;
  width: 100%;
  padding: 0;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
}

.top-bar {
  position: absolute;
  top: 70px;
  left: 60px;
  right: 60px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 1.2rem;
  padding-bottom: 20px;
  z-index: 100;
}

.brand {
  margin: 0;
  font-weight: bold;
  font-size: 3rem;
  cursor: pointer;
}

.nav-item {
  cursor: pointer;
  transition: all 0.4s ease;
  color: #ccff00; 
  opacity: 0.2;
}

.nav-item:hover {
  opacity: 0.5; /* Sedikit lebih terang saat kursor di atasnya */
}

/* --- STATE AKTIF (DIPILIH) --- */
.nav-item.active {
  opacity: 1; /* Terang maksimal */
  color: #ccff00;
  /* Opsional: Kamu bisa tambah font-weight atau transform jika mau */
  transform: scale(1.05); 
}

.brand .light {
  font-weight: normal;
  color: #5c7a5c;
}

.counter {
  font-family: 'Courier New', Courier, monospace;
  font-style: italic;
  font-size: 3rem;

  letter-spacing: -4px;
  word-spacing: -20PX;
}

.content-grid {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  gap: 0;
}

.text-col {
  /* GANTI flex: 1 JADI INI: */
  /* Artinya: Lebar kaku 50%, tidak boleh membesar/mengecil */
  flex: 0 0 50%; 
  max-width: 50%;
  
  /* Sisa settingan lama kamu (Padding, display, dll) biarkan saja */
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  
  /* Padding yang kamu mau tadi */
  padding-left: 60px; 
  padding-right: 10%; 
  padding-bottom: 80px;
  
  box-sizing: border-box; /* Wajib biar padding gak nambah lebar */
  z-index: 10;
}

.main-title {
  font-size: 4rem;
  line-height: 1;
  margin-bottom: 20px;
  font-weight: 300;
  font-family: 'Georgia', serif;
  font-style: italic;
  max-width: 500px;
}

.desc {
  font-size: 1.3rem;
  line-height: 1.6;
  /* color: #aebfab; */
  max-width: 500px;
}

/* --- CONTAINER (Bingkai Foto) --- */
.image-col {
  /* GANTI flex: 1 JADI INI JUGA: */
  flex: 0 0 50%;
  max-width: 50%;
  
  /* Sisa settingan image biarkan */
  height: 100vh;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.image-mask {
  width: 100%;
  height: 100%; 
  border-top-left-radius: 10%;
  border-bottom-left-radius: 10%;
  
  overflow: hidden; 
  position: relative; 
}

/* --- GAMBAR DI DALAMNYA (Isi Bingkai) --- */
.project-img {
  position: absolute;
  top: 0;
  left: 0;
  
  /* UKURAN MENGIKUTI BINGKAI */
  width: 100%;
  height: 100%;
  object-fit: cover; 
  opacity: 0;
  z-index: 1;
}

/* .project-img:first-child{
  opacity: 1;
  z-index: 2;
}

.img-0 {
  opacity: 1;
  z-index: 2;
} */

.center-icon {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); 
  z-index: 999; 
  
  width: 130px; 
  height: 130px;
  /* display: flex;
  align-items: center;
  justify-content: center; */
  
  /* Supaya icon tidak bisa diklik/ganggu drag */
  pointer-events: none; 
}

.center-icon img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}


/* 1. Keadaan saat gambar sedang bergerak (durasi) */
.scale-center-enter-active,
.scale-center-leave-active {
  transition: all 0.9s cubic-bezier(0.16, 1, 0.3, 1); /* Lebih smooth */
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform-origin: center center;
}

/* 2. Keadaan AWAL saat gambar masuk & Keadaan AKHIR saat gambar keluar */
/* Di Vue 2: Gunakan .enter (bukan .enter-from) */
.scale-center-enter, 
.scale-center-leave-to {
  opacity: 0;
  transform: scale(0.6); /* Ubah ke 0.6 supaya efek mengecilnya 'kerasa' */
}

/* 3. Pastikan z-index agar gambar baru muncul di atas gambar lama */
.scale-center-enter-active {
  z-index: 5;
}
.scale-center-leave-active {
  z-index: 1;
}



</style>