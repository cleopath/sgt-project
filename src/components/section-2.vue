<template>
  <div>
    <section class="sticky-section-wrapper" ref="wrapper">
      <div class="sticky-content">
        <div v-for="(item, index) in cards" 
             :key="index" 
             :class="['card-box', `card-${index}`, `panel-${index}`]">
          <img :src="item.img" class="card-img" />
        </div>

        <div class="text-col">
          <div class="text-counter">{{ activeIndex + 1 }} — 3</div>
          <h1 class="text-title">{{ cards[activeIndex].title }}</h1>
          <div class="text-cta">
            <button class="btn-learn">Learn More</button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import imgPlan from '@/assets/images/imgi_15_638e3f15b3ed3463ebe6038b_pexels-wendy-wei-14397945-p-1600.jpg';
import imgDesign from '@/assets/images/imgi_20_638e4092e9575c0f9629ae01_walls-p-1600.jpg';
import imgBuild from '@/assets/images/imgi_24_638e45c467fd8f44a5687f97_pexels-cottonbro-studio-5474032-p-2600.jpg';

gsap.registerPlugin(ScrollTrigger);

export default {
  name: 'SectionTwo',
  data() {
    return {
      cards: [
        { img: imgPlan, title: 'plan' },
        { img: imgDesign, title: 'design' },
        { img: imgBuild, title: 'build' },
      ],
      activeIndex: 0
    }
  },

 mounted() {
  gsap.set(".card-img", { scale: 0, opacity: 0 });
  gsap.set(".text-title", { opacity: 0, y: 20, scale: 0.8 });
  // Tidak perlu set .text-counter dan .text-cta

  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: ".sticky-section-wrapper",
      start: "top top",
      end: "+=600%",
      scrub: 1,
      pin: true,
    },
  });

  this.cards.forEach((_, i) => {
    tl.to(`.card-${i} .card-img`, { 
      opacity: 1, 
      scale: 1, 
      duration: 1.2, 
      ease: "back.out(1.2)" 
    })
    .to(`.card-${i} .card-img`, { 
      width: "100vw", 
      height: "100vh", 
      borderRadius: "0px", 
      duration: 1.5,
      onStart: () => { this.activeIndex = i; }
    })
    // Hanya text-title yang beranimasi
    .to(".text-title", { 
      opacity: 1, 
      y: 0, 
      scale: 1, 
      duration: 0.8, 
      ease: "power2.out" 
    }, "<")
    // Hilangkan animasi text-title sebelum card berikutnya (kecuali terakhir)
    if (i < this.cards.length - 1) {
      tl.to(".text-title", { 
        opacity: 0, 
        y: -20, 
        scale: 0.8,
        duration: 0.5, 
        ease: "power2.in" 
      }, "+=1");
    }
  });
}
}
</script>

<style scoped lang="scss">
.sticky-section-wrapper {
  height: 100vh;
  background-color: #000;
  position: relative;
}

.sticky-content {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  position: relative;
}

.panel-0 { z-index: 10; }
.panel-1 { z-index: 20; }
.panel-2 { z-index: 30; }

.card-box {
  position: absolute;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-img {
  width: 600px;
  height: 300px;
  object-fit: cover;
  will-change: width, height, transform;
}

.text-col {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 100;
  color: #fff;
  max-width: 400px;
  text-align: center;
  pointer-events: none;

  .text-counter {
    font-size: 1.6rem;
    color: #ccff00;
    margin-bottom: 6px;
  }

  .text-title {
    font-size: 14rem;
    font-weight: bold;
    color: #ccff00;
    margin-bottom: 160px;
    line-height: 0;
  }

  .text-cta {
    margin-top: 10px;
    pointer-events: auto;
    cursor: pointer;

    .btn-learn {
      background: none;
      color: #ccff00;
      border: #ccff00 1px solid;
      padding: 10px 32px;
      border-radius: 20px;
      font-weight: bold;
      font-size: 1.1rem;
      transition: background 0.3s, color 0.3s;
      &:hover {
        background: #ccff00;
        color: #222;
      }
    }
  }
}
</style>