<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";

  const teams = [
    {
      name: "Electrical",
      goal: "Tune the engine software and complete power delivery circuitry.",
    },
    {
      name: "Chassis",
      goal: "Design the brakes, design the rear end, and redesign the center of the chassis.",
    },
    {
      name: "Powertrain",
      goal: "Fabricate the engine mounts and research a solid rear axle.",
    },
  ];

  const joinImages = [
    { src: "/Join/DCR_4477.jpg", alt: "Patriot Motorsports workshop" },
    { src: "/Join/DCR_4430.jpg", alt: "Patriot Motorsports team at work" },
    { src: "/Join/DCR_4522.jpg", alt: "Patriot Motorsports engineering team" },
    { src: "/Join/20250925_175258.jpg", alt: "Patriot Motorsports team event" },
    { src: "/Join/JWB_042325_GMU-WW-062-final.jpg", alt: "Patriot Motorsports team members" },
  ];

  let page: HTMLElement;
  let activeSlide = 0;

  onMount(() => {
    const context = gsap.context(() => {
      if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
      gsap.fromTo(
        ".join-reveal",
        { opacity: 0, y: 28 },
        {
          opacity: 1,
          y: 0,
          duration: 0.7,
          stagger: 0.12,
          scrollTrigger: {
            trigger: ".join-reveal",
            start: "top 85%",
            once: true,
          },
        },
      );
    }, page);
    ScrollTrigger.refresh();
    const prefersReducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    const slideTimer = prefersReducedMotion
      ? undefined
      : window.setInterval(() => {
          activeSlide = (activeSlide + 1) % joinImages.length;
        }, 5000);

    return () => {
      context.revert();
      if (slideTimer) window.clearInterval(slideTimer);
    };
  });

  function showSlide(index: number) {
    activeSlide = (index + joinImages.length) % joinImages.length;
  }

  function isNearbySlide(index: number) {
    const distance = Math.abs(index - activeSlide);
    return distance <= 1 || distance === joinImages.length - 1;
  }
</script>

<main bind:this={page}>
  <section class="page-hero carbon">
    <div class="section-shell hero-grid">
      <div class="hero-copy">
        <h1 class="pm-title">Join the Team</h1>
        <p>
          Patriot Motorsports is open to everyone with an interest in engineering
          and motorsports. No experience is needed!
        </p>
      </div>

      <div class="join-carousel" aria-label="Patriot Motorsports Join gallery">
        <div class="carousel-viewport">
          {#each joinImages as image, index}
            {#if isNearbySlide(index)}
              <img
                class:active={activeSlide === index}
                src={image.src}
                alt={image.alt}
                loading={activeSlide === index ? "eager" : "lazy"}
              />
            {/if}
          {/each}
        </div>
      </div>
    </div>
  </section>

  <section class="page-section carbon subteams">
    <div class="section-shell">
      <div class="card-grid">
        {#each teams as team, index}
          <article class="pm-card join-reveal">
            <span class="race-num" aria-hidden="true">{team.name}</span>
            <h2>{team.name}</h2>
            <p>{team.goal}</p>
          </article>
        {/each}
      </div>
    </div>
  </section>

  <section class="page-section join-cta">
    <div class="section-shell carbon cta-panel">
      <h2 class="pm-title">Who Can Join?</h2>
      <p>
        Patriot Motorsports is open to everyone with an interest in engineering
        and motorsports. No experience is needed! If you are a student, then
        click the link below to join the Mason360. Feel free to contact us over
        Instagram or email with any questions.
      </p>
      <a
        class="pm-btn fill"
        href="https://mason360.gmu.edu/MF1/club_signup"
        target="_blank"
        rel="noreferrer"><span>Sign Up Today!</span></a
      >
      
      <a
        class="pm-btn line"
        href="https://discord.gg/xJaPCDGmqG"
        target="_blank"
        rel="noreferrer"><span>Join our Discord!</span></a
      >
    </div>
  </section>
</main>

<style>
  .page-hero {
    position: relative;
    overflow: hidden;
    padding: 12rem 0 8rem;
  }
  .page-hero > .section-shell {
    position: relative;
    z-index: 1;
  }
  .page-hero::after {
    content: "";
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    background: url("/asphalt.jpg") center / 480px repeat;
    opacity: 0.28;
  }
  .page-hero p:last-child {
    max-width: 42rem;
    margin: 2rem 0 0;
    color: #ddd;
    font-size: 1.15rem;
  }
  .hero-grid {
    display: grid;
    grid-template-columns: minmax(0, 0.8fr) minmax(0, 1.2fr);
    gap: clamp(2rem, 7vw, 7rem);
    align-items: center;
    perspective-origin: left;
    perspective: 67px;
  }
  .hero-grid > * {
    transform: rotateY(1deg);
  }
  .hero-copy p {
    max-width: 42rem;
  }
  .join-carousel {
    position: relative;
    min-width: 0;
    pointer-events: none;
    transform:none;
  }
  .carousel-viewport {
    position: relative;
    height: clamp(18rem, 38vw, 31rem);
    overflow: hidden;
    border: 1px solid rgb(255 255 255 / 20%);
    background: var(--panel);
    clip-path: polygon(0 0, 94% 0, 100% 10%, 100% 100%, 6% 100%, 0 90%);
  }
  .carousel-viewport::after {
    content: "";
    position: absolute;
    inset: 0;
    pointer-events: none;
    background: linear-gradient(135deg, transparent 55%, rgb(17 17 17 / 48%));
  }
  .carousel-viewport img {
    position: absolute;
    inset: 0;
    width: 120%;
    height: 120%;
    object-fit: cover;
    opacity: 0;
    transform: scale(1.04);
    transition: opacity 450ms ease, transform 700ms ease;
  }
  .carousel-viewport img.active {
    opacity: 1;
    transform: scale(1);
  }
  .card-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.25rem;
    margin-top: 2rem;
  }
  .pm-card h2 {
    position: relative;
    margin: 0 0 1rem;
    font-family: Impact, "Arial Narrow", sans-serif;
    font-size: clamp(1.5rem, 3vw, 2.5rem);
    text-transform: uppercase;
  }
  .pm-card p:last-child {
    position: relative;
    color: #d6d6d6;
  }
  .pm-card .race-num {
    top: 2rem;
    left: 1rem;
  }
  .join-cta {
    padding-top: 8rem;
  }

  .cta-panel {
    position: relative;
    overflow: hidden;
    padding: clamp(2rem, 6vw, 5rem);
    border: 1px solid var(--line);
  }
  .cta-panel > * {
    position: relative;
    z-index: 1;
  }
  .cta-panel p:not(.pm-eyebrow) {
    max-width: 48rem;
    color: #ddd;
  }
  .cta-panel .pm-btn {
    margin-top: 1rem;
  }
  @media (max-width: 768px) {
    .card-grid {
      grid-template-columns: 1fr;
    }
    .page-hero {
      padding-top: 10rem;
    }
    .hero-grid {
      grid-template-columns: 1fr;
      gap: 3rem;
    }
    .carousel-viewport {
      height: min(72vw, 25rem);
    }
  }
  @media (prefers-reduced-motion: reduce) {
    .carousel-viewport img {
      transition: none;
    }
  }
</style>
