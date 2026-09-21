<script>
  
  import { onMount } from 'svelte';
  import Nav from './Nav.svelte';
  import Hamburger from './Hamburger.svelte';
  import Footer from './Footer.svelte';
  
  export let open = false;
  export let route = '';
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
    return () => clearTimeout(timer);
  });
</script>

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
<Hamburger {navigate} {hamburger} {open} {route}/>

<main class="content">
  <slot />
</main>
<Footer />