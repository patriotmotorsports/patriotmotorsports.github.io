<script>
  import { onMount } from "svelte";
  import Nav from "./Nav.svelte";
  import Hamburger from "./Hamburger.svelte";
  import Footer from "./Footer.svelte";

  export let open = false;
  export let route = "";
  export let navigate = (p) => {};
  export let hamburger = () => {};

  let splashVisible = true;
  let NUM_STARS = 50;
  const trailStars = Array.from({ length: NUM_STARS }).map((_, i) => {
    const progress = (i / NUM_STARS) * 0.95;
    const scatter = (Math.random() - 0.5) * (8 + progress * 20);
    const left = progress * 110 + scatter;
    const top = progress * 110 - scatter;
    const fontSize = (0.4 + Math.pow(progress, 1.8) * 4.2).toFixed(2);
    const delay = (progress * 1.6 + (Math.random() * 0.08 - 0.04)).toFixed(2);
    return { top, left, fontSize, delay };
  });
  onMount(() => {
    const timer = setTimeout(() => {
      splashVisible = false;
    }, 2000);

    window.onbeforeunload = function () {
      window.scrollTo(0, 0);
    };
    const blurFx = document.querySelector("#blur feGaussianBlur");
    if (!blurFx) return;

    let currentBlur = 0;
    let targetBlur = 0;
    let lastScrollY = window.scrollY;
    let isTicking = false;

    const updateBlur = () => {
      currentBlur += (targetBlur - currentBlur) * 0.15;

      targetBlur *= 0.85;

      blurFx.setStdDeviation(0, Math.abs(currentBlur));

      if (Math.abs(currentBlur) > 0.01) {
        requestAnimationFrame(updateBlur);
      } else {
        blurFx.setStdDeviation(0, 0);
        isTicking = false;
      }
    };

    const handleScroll = (e) => {
      const currentScrollY = window.scrollY;
      const deltaY = currentScrollY - lastScrollY;
      lastScrollY = currentScrollY;

      // Ignore zero-delta events at the bottom/top page boundaries
      if (Math.abs(deltaY) > 0.5) {
        targetBlur = Math.min(2, Math.abs(deltaY) / 8);

        if (!isTicking) {
          isTicking = true;
          requestAnimationFrame(updateBlur);
        }
      }
    };

    window.addEventListener("scroll", handleScroll, { passive: true });

    return () => {
      clearTimeout(timer);
      window.removeEventListener("scroll", handleScroll);
    };
  });
</script>

<svg
  version="1.1"
  id="Layer_1"
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink"
  width="0"
  height="0"
  viewBox="0 0 1366 768"
  xml:space="preserve"
>
  <defs>
    <filter id="blur">
      <feGaussianBlur in="SourceGraphic" stdDeviation="0 0"> </feGaussianBlur>
    </filter>
  </defs>
</svg>

{#if splashVisible}
  <section class="splash">
    <!-- Dynamic Star Trail -->
    <div class="star-trail">
      {#each trailStars as star}
        <span
          class="star trail-star"
          style="
            top: {star.top}%;
            left: {star.left}%;
            font-size: {star.fontSize}rem;
            animation-delay: {star.delay}s;
          "
        >
          &#9733;
        </span>
      {/each}

      <!-- Final camera star (positioned where car exits bottom-right) -->
      <span class="star star-final">&#9733;</span>
    </div>

    <!-- Zooming F1 Car -->
    <div class="car-container">
      <img src="/car.webp" alt="Splash Screen" class="splash-car" />
    </div>
  </section>
{/if}

<Nav {navigate} {hamburger} {route} />
<Hamburger {navigate} {hamburger} {open} {route} />

<main class="content">
  <slot />
</main>
<Footer />
