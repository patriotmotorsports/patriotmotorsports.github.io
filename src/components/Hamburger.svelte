<script lang="ts">
  export let open = false;
  export let route = "";
  export let navigate: (path: string) => void = () => {};
  export let hamburger: () => void = () => {};
</script>

<div class="hamburger-menu" role="presentation" on:click|self={hamburger} class:open>
  <div class="ham-container">
    <div class="ham-links">
      <a href="/home" class:ham-active={route === "/home"} on:click|preventDefault|stopPropagation={() => navigate("/home")}>Home</a>
      <a href="/about" class:ham-active={route === "/about"} on:click|preventDefault|stopPropagation={() => navigate("/about")}>About Us</a>
      <a href="/join" class:ham-active={route === "/join"} on:click|preventDefault|stopPropagation={() => navigate("/join")}>Join</a>
      <a href="/sponsorship" class:ham-active={route === "/sponsorship"} on:click|preventDefault|stopPropagation={() => navigate("/sponsorship")}>Sponsorship</a>
    </div>
  </div>
</div>

<style>
  .hamburger-menu { position: fixed; inset: 0; z-index: 2000; display: none; pointer-events: none; opacity: 0; background: rgb(0 0 0 / 68%); transition: opacity 220ms; }
  .hamburger-menu.open { pointer-events: all; opacity: 1; }
  .ham-container { width: min(80vw, 340px); height: 100%; padding: 6rem 1.5rem 2rem; background: var(--surface); transform: translateX(-100%); transition: transform 300ms; }
  .hamburger-menu.open .ham-container { transform: translateX(0); }
  .ham-links { display: flex; flex-direction: column; gap: 1rem; }
  .ham-links a { padding: 0.8rem 0; color: #fff; font-weight: 800; text-decoration: none; text-transform: uppercase; }
  .ham-links a::before { content: "»"; display: inline-block; width: 0; overflow: hidden; color: var(--primary); transition: width 180ms; }
  .ham-links a:hover::before, .ham-links a.ham-active::before { width: 1.2rem; }
  .ham-active { color: var(--primary) !important; }
  @media (max-width: 768px) { .hamburger-menu { display: block; } }
</style>
