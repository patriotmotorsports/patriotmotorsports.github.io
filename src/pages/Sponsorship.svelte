<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";

  const tiers = [
    {
      name: "Patriot",
      className: "patriot",
      logos: [],
      hidden:true,
    },
    {
      name: "Gold",
      className: "gold",
      logos: [
        {
          href: "https://www.northropgrumman.com/",
          src: "/Sponsorship/Sponsors/Northrop-Grumman-Logo.png",
          alt: "Northrop Grumman",
          style: "width:100%;",
        },
        {
          href: "https://www.ghaasfoundation.org/",
          src: "/Sponsorship/Sponsors/Ghaas.png",
          alt: "Haas Foundation",
          style: "",
        },
      ],
    },
    {
      name: "Green",
      className: "green",
      logos: [{
          href: "https://solidworks.com",
          src: "/Sponsorship/Sponsors/SolidWorks-Logo.png",
          alt: "Walmart",
          style: "max-width:20vw;",
        },
      ],
    },
    {
      name: "Friend of Mason",
      className: "friend",
      logos: [
        {
          href: "https://walmart.com",
          src: "/Sponsorship/Sponsors/Walmart_logo_(2025).svg",
          alt: "Walmart",
          style: "max-width:20vw;",
        },],
    },
  ];

  let page: HTMLElement;

  onMount(() => {
    const context = gsap.context(() => {
      if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) return;
      gsap.fromTo(
        ".sponsor-reveal",
        { opacity: 0, y: 28 },
        {
          opacity: 1,
          y: 0,
          duration: 0.7,
          stagger: 0.1,
          scrollTrigger: {
            trigger: ".sponsor-reveal",
            start: "top 85%",
            once: true,
          },
        },
      );
    }, page);
    ScrollTrigger.refresh();
    return () => context.revert();
  });
</script>

<main bind:this={page}>
  <section class="page-hero carbon">
    <div class="section-shell hero-grid">
      <div class="hero-copy">
        <h1 class="pm-title">Sponsors</h1>
        <p>Interested in sponsoring Patriot Motorsports?</p>
      </div>
      <figure class="sponsor-hero-image">
        <img
          src="/Sponsorship/TopSection.jpg"
          alt="Patriot Motorsports sponsorship showcase"
        />
      </figure>
    </div>
  </section>

  <section class="page-section carbon tiers-section">
    <div class="section-shell">
      <div class="tier-grid">
        {#each tiers as tier}
          {#if !tier.hidden}
          <article class={`pm-card sponsor-reveal tier-card ${tier.className}`}>
            <span class="race-num" aria-hidden="true">{tier.name}</span>
            <h2>{tier.name} Sponsors</h2>
            <div class="sponsor-list">
              {#each tier.logos as logo}
                <a
                  href={logo.href}
                  target="_blank"
                  rel="noreferrer"
                  style={logo.style}><img src={logo.src} alt={logo.alt} /></a
                >
              {/each}
            </div>
          </article>
          {/if}
        {/each}
      </div>
    </div>
  </section>

  <section class="page-section carbon benefit-section">
    <h2 class="pm-title">Partnership Benefits</h2>
    <div class="section-shell benefit-grid">
      <div class="benefit-copy">
        <h3>Recruitment</h3>
        <p>
          Your brand can be displayed on our vehicle, website, posters, team
          apparel, and in our workspace. Our members are active college students
          who seek career opportunities every summer, so sponsoring us also
          gives you a steady flow of talent. They bring hands-on engineering
          experience, teamwork, and project management skills.
        </p>
      </div>
      <figure class="benefit-image">
        <img src="/Sponsorship/Recruitment.jpg" alt="Patriot Motorsports team members" />
      </figure>

      <figure class="benefit-image">
        <img
          src="/Sponsorship/Exposure.jpg"
          alt="Patriot Motorsports vehicle at an event"
        />
      </figure>
      <div class="benefit-copy">
        <h3>Exposure</h3>
        <p>
          The annual FSAE competition brings together 120 teams and draws a
          large audience of businesses and spectators. Our sponsorship proposal
          is also your business opportunity.
        </p>
        <p>
          This past year, Patriot Motorsports attended the SAE
          Government/Industry Meeting in Washington, D.C., Whiskey and Widgets
          in April, and the Capstone Senior Design Showcase in May. In 2024, we
          were also invited to the Red Bull Show in D.C., which drew an
          estimated 50,000 attendees. Events like these showcase our partnership
          with you, and your sponsorship lets us keep attending them and
          displaying your brand.
        </p>
      </div>
    </div>
  </section>

  <section class="page-section sponsor-cta">
    <div class="section-shell carbon cta-panel">
      <h2 class="pm-title">Become a Sponsor</h2>
      <p>Interested in sponsoring Patriot Motorsports?</p>
      <p>
        Contact us at <a href="mailto:formulasaemason@gmail.com"
          >formulasaemason@gmail.com</a
        >
      </p>
      <p>
        To learn more, read through our <a
          href="/Sponsorship/Sponsorship Package 2026-27.pdf" target="_blank"
          >sponsorship package</a
        >, view our current sponsors, or email us for more information.
      </p>
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
  .hero-copy {
    position: relative;
    z-index: 1;
  }
  .sponsor-hero-image {
    position: relative;
    z-index: 1;
    height: clamp(18rem, 38vw, 31rem);
    margin: 0;
    overflow: hidden;
    border: 1px solid rgb(255 255 255 / 20%);
    background: var(--panel);
    transform:none;
    clip-path: polygon(0 0, 94% 0, 100% 10%, 100% 100%, 6% 100%, 0 90%);
  }
  .sponsor-hero-image::after {
    content: "";
    position: absolute;
    inset: 0;
    pointer-events: none;
    background: linear-gradient(135deg, transparent 55%, rgb(17 17 17 / 48%));
  }
  .sponsor-hero-image img {
    display: block;
    width: 120%;
    height: 120%;
    object-fit: cover;
  }
  .page-hero p:last-child {
    margin: 2rem 0 0;
    color: #ddd;
    font-size: 1.15rem;
  }
  .hero-kerb {
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 12px;
  }
  .cta-panel a {
    color: var(--primary);
  }
  .tier-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.25rem;
    margin-top: 2rem;
  }
  .tier-card {
    min-height: 220px;
  }
  .tier-card h2 {
    position: relative;
    margin: 0 0 2rem;
    font-family: Impact, "Arial Narrow", sans-serif;
    font-size: clamp(1.4rem, 3vw, 2.5rem);
    text-transform: uppercase;
  }
  .tier-card.gold h2 {
    background: linear-gradient(
      to bottom,
      #e7c06d 5%,
      #e7ce44 30%,
      #e1bc27 80%,
      #d89c25 90%
    );
    background-clip: text;
    color: transparent;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .tier-card.patriot h2 {
    color: #fff;
  }
  .tier-card.green h2 {
    color: var(--secondary);
  }
  .tier-card.friend h2 {
    color: #6f6841;
  }
  .tier-card .race-num {
    top: 1.5rem;
    right: 1rem;
    opacity:0.5;
  }
  .sponsor-list {
    position: relative;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 1.5rem;
    align-items: center;
  }
  .sponsor-list a {
    display: grid;
    min-height: 110px;
    place-items: center;
    padding: 1rem; 
    filter: drop-shadow(2px 4px 2px black);
    transition: transform 220ms;
  }
  .sponsor-list a:hover {
    transform: translateY(-6px);
  }
  .sponsor-list img {
    display: block;
    max-width: 100%;
    max-height: 100px;
    height: auto;
  }
  .benefit-section {
    position: relative;
    isolation: isolate;
  }
  .benefit-section h2 {
    margin: 4rem;
  }
  .benefit-grid {
    display: grid;
    grid-template-columns: minmax(0, 0.9fr) minmax(0, 1.1fr);
    gap: clamp(2rem, 8vw, 8rem);
    align-items: center;
  }
  .exposure-grid {
    grid-template-columns: minmax(0, 1.1fr) minmax(0, 0.9fr);
  }
  .benefit-copy h2 {
    margin: 0 0 2rem;
  }
  .benefit-copy h3 {
    margin: 0 0 1rem;
    color: var(--primary);
    font-family: Impact, "Arial Narrow", sans-serif;
    font-size: clamp(1.8rem, 4vw, 3rem);
    text-transform: uppercase;
  }
  .benefit-copy p {
    max-width: 42rem;
    color: #ddd;
  }
  .benefit-image {
    height: clamp(18rem, 36vw, 30rem);
    margin: 0;
    overflow: hidden;
    border: 1px solid rgb(255 255 255 / 18%);
    clip-path: polygon(0 0, 94% 0, 100% 10%, 100% 100%, 6% 100%, 0 90%);
  }
  .benefit-image img {
    display: block;
    width: 120%;
    height:120%;
    object-fit: cover;
  }
  .sponsor-cta {
    padding-top: 6rem;
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
  .cta-panel h3 {
    position: relative;
    margin: 1.5rem 0 0.5rem;
    font-family: Impact, "Arial Narrow", sans-serif;
    font-size: clamp(1.4rem, 3vw, 2.2rem);
    text-transform: uppercase;
  }
  .cta-panel .pm-btn {
    margin-top: 1rem;
  }
  @media (max-width: 768px) {
    .page-hero {
      padding-top: 10rem;
    }
    .hero-grid {
      grid-template-columns: 1fr;
      gap: 3rem;
    }
    .sponsor-hero-image {
      height: min(72vw, 25rem);
    }
    .benefit-grid,
    .exposure-grid {
      grid-template-columns: 1fr;
      gap: 2rem;
    }
    .exposure-grid .benefit-image {
      order: -1;
    }
  }
</style>
