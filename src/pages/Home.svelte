<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import fitText from "../fittext";

  export let navigate: (path: string) => void = (path) =>
    (location.href = path);

  const teams = [
    {
      name: "Chassis",
      img: "/Home/Chassis.png",
      goal: "Design the brakes, design the rear end, and redesign the center of the chassis.",
    },
    {
      name: "Powertrain",
      img: "/Home/Powertrain.png",
      goal: "Fabricate the engine mounts and research a solid rear axle.",
    },
    {
      name: "Electrical",
      img: "/Home/Electrical.png",
      goal: "Tune the engine software and complete power delivery circuitry.",
    },
  ];

  let track: HTMLElement;
  let hero: HTMLElement;
  let active = 0;

  onMount(() => {
    const updateReveal = (event: PointerEvent) => {
      if (event.pointerType !== "mouse") return;

      const rect = hero.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      hero.style.setProperty("--mouse-x", `${x}px`);
      hero.style.setProperty("--mouse-y", `${y}px`);
    };

    hero.addEventListener("pointermove", updateReveal);

    gsap.registerPlugin(ScrollTrigger);
    const context = gsap.context(() => {
      const teamLayers = gsap.utils.toArray<HTMLElement>(".team-layer", track);
      const panels = gsap.utils.toArray<HTMLElement>(".panel", track);
      const numbers = gsap.utils.toArray<HTMLElement>(
        ".panel .race-num",
        track,
      );

      gsap.set(teamLayers.slice(1), { autoAlpha: 0 });
      gsap.set(panels.slice(1), { autoAlpha: 0, y: 40 });

      if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) {
        gsap.set(panels[0], { autoAlpha: 1, y: 0 });
        return;
      }

      const timeline = gsap.timeline({
        defaults: { ease: "none" },
        scrollTrigger: {
          trigger: track,
          start: "top top",
          end: "bottom bottom",
          scrub: 0.6,
          onUpdate: (state) =>
            (active = Math.min(2, Math.floor(state.progress * 3))),
        },
      });

      [1, 2].forEach((teamIndex) => {
        const time = teamIndex - 0.5;
        timeline
          .to(teamLayers[teamIndex], { autoAlpha: 1, duration: 0.8 }, time)
          .to(
            panels[teamIndex - 1],
            { autoAlpha: 0, y: -40, duration: 0.4 },
            time,
          )
          .to(
            panels[teamIndex],
            { autoAlpha: 1, y: 0, duration: 0.4 },
            time + 0.6,
          )
          .to(numbers[teamIndex], { y: -24, duration: 1 }, time);
      });
      timeline.to({}, { duration: 0.5 });
    }, track);

    ScrollTrigger.refresh();
    fitText(document.querySelector(".hero-title"), 1);
    return () => context.revert();
  });
</script>

<section class="hero" bind:this={hero}>
  <div class="hero-bg"></div>
  <div class="hero-grain" aria-hidden="true"></div>
  <div class="hero-content section-shell">
    <h1 class="pm-title hero-title">Patriot Motor sports</h1>
    <p class="tag">George Mason University's FSAE Team.</p>
    <div class="hero-buttons">
      <button class="pm-btn fill" on:click={() => navigate("/join")}
        ><span>Join the Team</span></button
      >
      <button
        class="pm-btn line"
        on:click={() =>
          document
            .getElementById("teams")
            ?.scrollIntoView({ behavior: "smooth" })}
        ><span>Meet the Teams</span></button
      >
    </div>
  </div>
  <div class="hero-kerb kerb" aria-hidden="true"></div>
</section>

<section class="page-section carbon slant immersive-section challenge-section">
  <div class="section-shell immersive-grid">
    <div class="immersive-copy">
      <h2 class="pm-title">What is Formula SAE?</h2>
      <p>
        Students conceive, design, fabricate and compete with small
        formula-style race cars, judged in static and dynamic events: tech
        inspection, cost, presentation, engineering design, solo performance and
        endurance.
      </p>
    </div>
    <figure class="immersive-media">
      <img src="/Home/TeamPhoto.jpg" alt="Patriot Motorsports team" />
    </figure>
  </div>
</section>

<section class="page-section mission-section">
  <div class="section-shell immersive-grid mission-grid">
    <figure class="immersive-media">
      <img
        src="/Home/AprilInsta.jpg"
        alt="Hayfield Secondary School’s robotics competition"
      />
    </figure>
    <div class="immersive-copy">
      <h2 class="pm-title">Our Mission</h2>
      <p>
        We build a culture of innovation and teamwork, turning theory into
        hands-on engineering and growing the next generation of industry
        leaders.
      </p>
    </div>
  </div>
</section>

<div id="teams" class="track" bind:this={track}>
  <div class="stage">
    <div class="images" aria-hidden="true">
      {#each teams as team, teamIndex}
        <div
          class="team-layer"
          data-team={teamIndex}
          style={`background-image:url('${team.img}')`}
        ></div>
      {/each}
    </div>
    <div class="copy section-shell">
      <ul class="dots">
        {#each teams as team, index}
          <li class:on={active === index}>{team.name}</li>
        {/each}
      </ul>
      <div class="panels">
        {#each teams as team, index}
          <article class="panel">
            <span class="race-num" aria-hidden="true">{team.name}</span>
            <h2 class="pm-title">{team.name}</h2>
            <p>{team.goal}</p>
          </article>
        {/each}
      </div>
    </div>
  </div>
</div>

<section class="page-section carbon history">
  <div class="section-shell history-inner immersive-grid">
    <div class="immersive-copy">
      <h2 class="pm-title">Our History</h2>
      <p>
        Patriot Motorsports began in a student's garage. It has since grown into
        its own workshop at George Mason's Science and Technology campus in
        Manassas, with a chassis well underway, a turning engine, a fabricated
        suspension, and control systems and brakes in development.
      </p>
      <button class="pm-btn fill" on:click={() => navigate("/join")}
        ><span>Build with us</span></button
      >
    </div>
    <figure class="immersive-media">
      <img src="/Home/TeamWithCar.jpg" alt="Patriot Motorsports race car" />
    </figure>
  </div>
</section>

<style>
  .hero {
    --mouse-x: 50%;
    --mouse-y: 50%;
    --reveal-size: 400px;
    --mask-image:radial-gradient(
      circle var(--reveal-size) at var(--mouse-x) var(--mouse-y),
      #000 0%,
      rgb(0 0 0 / 70%) 50%,
      transparent 100%
    );
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    overflow: hidden;
    isolation: isolate;
  }
  .hero-bg {
    position: absolute;
    inset: 0;
    z-index: 0;
    background-position: center;
    background-size: cover;
    background-image: url("/Home/Hero.jpg");
  }
  .hero::before {
    content: "";
    position: absolute;
    inset: 0;
    z-index: 1;
    background-position: center;
    background-size: cover;
    background-image: url(/Home/HeroDither.jpg);
    opacity: 0;
    transition: mask-image 10s ease, clip-path 5s ease-out;
    -webkit-mask-image: var(--mask-image);
    mask-image: var(--mask-image);
    animation: hero-before linear 10s infinite forwards;
    animation-delay:3s;
    clip-path: none;

    background-blend-mode: lighten;
    pointer-events: none;
  }
  @keyframes hero-before {
    from {opacity:0}
    50% {opacity: 1}
    to {opacity:0}
  }

  .hero-grain {
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    opacity: 0.1;
    background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Cfilter id='n'%3E%3CfeTurbulence baseFrequency='.8' numOctaves='2'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.35'/%3E%3C/svg%3E");
  }
  .hero-title {
    max-width: 850px;
    font-family: "F1";
    text-shadow:
      0px 3px 0px black,
      0px -3px 0px var(--secondary),
      0px 6px 0px var(--primary),
      0px -6px 0px var(--primary),
      0px 9px 0px var(--secondary),
      0px -9px 0px black;
    text-align: justify;
  }
  .hero-content {
    position: relative;
    z-index: 1;
    padding-top: 8rem;
    padding-bottom: 8rem;
    perspective-origin: left;
    perspective: 67px;
  }
  .hero-content > * {
    transform: rotateY(1deg);
  }
  .tag {
    max-width: 34rem;
    margin: 1.5rem 0 2.5rem;
    font-size: clamp(1.1rem, 2vw, 1.5rem);
    font-weight: 600;
  }
  .hero-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }
  .hero-kerb {
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 12px;
  }
  .immersive-section {
    padding-top: clamp(5rem, 10vw, 9rem);
    padding-bottom: clamp(5rem, 10vw, 9rem);
  }
  .immersive-grid {
    display: grid;
    grid-template-columns: minmax(0, 0.9fr) minmax(0, 1.1fr);
    gap: clamp(2rem, 8vw, 8rem);
    align-items: center;
  }
  .mission-section {
    position: relative;
    isolation: isolate;
    background: var(--ink);
  }
  .mission-grid {
    grid-template-columns: minmax(0, 1.1fr) minmax(0, 0.9fr);
  }
  .immersive-copy {
    position: relative;
    z-index: 1;
  }
  .immersive-copy p,
  .history-inner > p {
    max-width: 42rem;
    color: #ddd;
  }
  .immersive-copy h2,
  .history h2 {
    margin-bottom: 1.4rem;
  }
  .immersive-media {
    position: relative;
    z-index: 1;
    min-height: 22rem;
    margin: 0;
    overflow: hidden;
    border: 1px solid rgb(255 255 255 / 16%);
    background: var(--panel);
    clip-path: polygon(0 0, 94% 0, 100% 12%, 100% 100%, 6% 100%, 0 88%);
  }
  .immersive-media::after {
    content: "";
    position: absolute;
    inset: 0;
    pointer-events: none;
    background: linear-gradient(135deg, transparent 55%, rgb(17 17 17 / 42%));
  }
  .immersive-media img {
    display: block;
    width: 100%;
    height: 100%;
    min-height: 22rem;
    object-fit: cover;
  }
  .track {
    position: relative;
    height: 400vh;
  }
  .stage {
    position: sticky;
    top: 0;
    height: 100vh;
    overflow: hidden;
    background: var(--ink);
  }
  .images,
  .team-layer {
    position: absolute;
    inset: 0;
  }
  .team-layer {
    left: 50%;
    width: 50%;
    background-color: var(--surface);
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }
  .copy {
    display: flex;
    position: relative;
    z-index: 2;
    height: 100%;
    flex-direction: column;
    justify-content: center;
  }
  .panels {
    position: relative;
    width: min(43%, 32rem);
    min-height: 17rem;
  }
  .panel {
    position: absolute;
    inset: 0 auto auto 0;
    width: 100%;
  }
  .panel .race-num {
    top: -5rem;
    left: -2rem;
  }
  .panel h2 {
    position: relative;
    margin: 1rem 0;
  }
  .panel p:last-child {
    position: relative;
    font-size: 1.15rem;
    font-weight: 500;
  }
  .dots {
    display: flex;
    gap: 1.5rem;
    margin: 2rem 0 0;
    padding: 0;
    list-style: none;
  }
  .dots li {
    padding-top: 0.6rem;
    border-top: 4px solid rgb(255 255 255 / 24%);
    color: rgb(255 255 255 / 52%);
    font-size: 0.85rem;
    font-weight: 800;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    transition:
      color 220ms,
      border-color 220ms;
  }
  .dots li.on {
    border-color: var(--primary);
    color: #fff;
  }
  .history-inner {
    text-align: left;
  }
  .history-inner .pm-btn {
    margin-top: 1.5rem;
  }
  @media (max-width: 768px) {
    .immersive-grid,
    .mission-grid {
      grid-template-columns: 1fr;
      gap: 2rem;
    }
    .mission-grid .immersive-media {
      order: 2;
    }
    .stage {
      align-items: end;
    }
    .team-layer {
      inset: 0 0 34% 0;
      width: 100%;
    }
    .copy {
      justify-content: flex-end;
      padding-bottom: 3rem;
    }
    .panels {
      width: 100%;
      min-height: 14rem;
    }
    .dots {
      gap: 0.7rem;
      flex-wrap: wrap;
    }
  }
  @media (prefers-reduced-motion: reduce) {
    .team-layer:not([data-team="0"]) {
      opacity: 0 !important;
    }
    .panel:not(:first-child) {
      opacity: 0 !important;
    }
  }
</style>
