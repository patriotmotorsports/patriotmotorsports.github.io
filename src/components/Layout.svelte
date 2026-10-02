<script lang="ts">
  import { onMount } from "svelte";
  import Nav from "./Nav.svelte";
  import Hamburger from "./Hamburger.svelte";
  import Footer from "./Footer.svelte";

  export let open = false;
  export let route = "";
  export let navigate: (path: string) => void = () => {};
  export let hamburger: () => void = () => {};

  let splashVisible = true;
  const trailStars = Array.from({ length: 50 }).map((_, i) => {
    const progress = (i / 50) * 0.95;
    const scatter = (Math.random() - 0.5) * (8 + progress * 20);
    return {
      top: progress * 110 - scatter,
      left: progress * 110 + scatter,
      fontSize: (0.4 + Math.pow(progress, 1.8) * 4.2).toFixed(2),
      delay: (progress * 1.6 + (Math.random() * 0.08 - 0.04)).toFixed(2),
    };
  });

  onMount(() => {
    const timer = window.setTimeout(() => (splashVisible = false), 2000);
    return () => window.clearTimeout(timer);
  });
</script>

<svg width="0" height="0" aria-hidden="true" class="texture-filters">
  <defs>
    <filter id="grain">
      <feTurbulence type="fractalNoise" baseFrequency="0.75" numOctaves="2" stitchTiles="stitch" />
      <feColorMatrix type="saturate" values="0" />
    </filter>
    <filter id="rubber">
      <feTurbulence type="fractalNoise" baseFrequency="0.002 0.5" numOctaves="2" stitchTiles="stitch" />
      <feComponentTransfer>
        <feFuncA type="linear" slope="0.22" />
      </feComponentTransfer>
    </filter>
  </defs>
</svg>

{#if splashVisible}
  <section class="splash">
    <div class="star-trail">
      {#each trailStars as star}
        <span class="star trail-star" style={`top:${star.top}%;left:${star.left}%;font-size:${star.fontSize}rem;animation-delay:${star.delay}s`}>&#9733;</span>
      {/each}
      <span class="star star-final">&#9733;</span>
    </div>
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
