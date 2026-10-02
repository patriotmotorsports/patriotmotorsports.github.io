<script lang="ts">
  export let navigate: (path: string) => void = () => {};
  export let route = "";
  export let hamburger: () => void = () => {};
</script>

<nav class="navbar">
  <div class="nav-container">
    <div class="nav-logo">
      <button
        aria-label="Menu"
        on:click|preventDefault={hamburger}
        class="nav-button"
      >
        <svg
          fill="white"
          viewBox="0 0 100 80"
          width="24"
          height="24"
          aria-hidden="true"
        >
          <rect width="100" height="10"></rect>
          <rect y="30" width="100" height="10"></rect>
          <rect y="60" width="100" height="10"></rect>
        </svg>
      </button>
      <a href="/home" on:click|preventDefault={() => navigate("/home")}>
        <img src="/logo.webp" alt="Patriot Motorsports Logo" />
      </a>
    </div>
    <div class="nav-links">
      <a
        href="/home"
        class:nav-active={route === "/home"}
        on:click|preventDefault={() => navigate("/home")}>Home</a
      >
      <a
        href="/join"
        class:nav-active={route === "/join"}
        on:click|preventDefault={() => navigate("/join")}>Join</a
      >
      <a
        href="/sponsorship"
        class:nav-active={route === "/sponsorship"}
        on:click|preventDefault={() => navigate("/sponsorship")}>Sponsorship</a
      >
    </div>
  </div>
  <div class="nav-checker checker" aria-hidden="true"></div>
</nav>

<style>
  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1000;
    width: 100%;
    background: var(--panel);
  }
  .nav-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: min(1200px, calc(100% - 2rem));
    min-height: 72px;
    margin: 0 auto;
  }
  .nav-logo {
    display: flex;
    align-items: center;
    gap: 1rem;
  }
  .nav-logo img {
    display: block;
    height: 36px;
    width: auto;
  }
  .nav-links {
    display: flex;
    align-items: center;
    gap: clamp(1rem, 4vw, 2.5rem);
  }
  .nav-links a {
    position: relative;
    color: #fff;
    font-weight: 800;
    text-decoration: none;
    text-transform: uppercase;
  }
  .nav-links a::after {
    content: "";
    position: absolute;
    right: 0;
    bottom: -0.65rem;
    left: 0;
    height: 3px;
    background: var(--primary);
    opacity: 0;
    transform: translateY(5px);
    transition:
      transform 220ms,
      opacity 220ms;
  }
  .nav-links a:hover::after,
  .nav-links a.nav-active::after {
    opacity: 1;
    transform: translateY(0);
  }
  .nav-button {
    display: none;
    padding: 0;
    border: 0;
    background: transparent;
    cursor: pointer;
  }
  .nav-checker {
    height: 5px;
  }
  @media (max-width: 768px) {
    .nav-links a {
      display: none;
    }
    .nav-button {
      display: block;
    }
  }
</style>
