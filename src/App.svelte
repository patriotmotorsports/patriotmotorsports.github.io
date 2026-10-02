<script lang="ts">
  import { onMount, tick } from "svelte";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import Layout from "./components/Layout.svelte";
  import Home from "./pages/Home.svelte";
  import Join from "./pages/Join.svelte";
  import Sponsorship from "./pages/Sponsorship.svelte";
  import NotFound from "./pages/NotFound.svelte";
  import Leadership from "./pages/Leadership.svelte";

  export let route = pathToRoute(location.pathname);

  let open = false;

  function pathToRoute(pathname: string) {
    let path = pathname.replace(/\/+$/, "");
    path = path.substring(path.lastIndexOf("/"));
    return !path || path === "/" || path === "/index.html" ? "/home" : path;
  }

  function updateRoute(pathname = location.pathname) {
    route = pathToRoute(pathname);
  }

  async function update(path: string) {
    history.pushState({}, "", path);
    updateRoute(path);
    window.scrollTo(0, 0);
    open = false;
    await tick();
  }

  async function navigate(path: string) {
    if (path === route) {
      open = false;
      return;
    }

    const reduced = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    const transitionDocument = (
      document as Document & {
        startViewTransition?: (callback: () => Promise<void>) => { finished: Promise<void> };
      }
    );

    if (!transitionDocument.startViewTransition || reduced) {
      await update(path);
    } else {
      await transitionDocument.startViewTransition(() => update(path)).finished;
    }
    ScrollTrigger.refresh();
  }

  function hamburger() {
    open = !open;
  }

  onMount(() => {
    updateRoute();

    const handlePopState = async () => {
      const updatePopState = async () => {
        updateRoute();
        window.scrollTo(0, 0);
        open = false;
        await tick();
      };

      const reduced = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
      const transitionDocument = (
        document as Document & {
          startViewTransition?: (callback: () => Promise<void>) => { finished: Promise<void> };
        }
      );

      if (!transitionDocument.startViewTransition || reduced) {
        await updatePopState();
      } else {
        await transitionDocument.startViewTransition(updatePopState).finished;
      }
      ScrollTrigger.refresh();
    };
    window.addEventListener("popstate", handlePopState);

    return () => window.removeEventListener("popstate", handlePopState);
  });
</script>

<svelte:head>
  <title>{route.slice(1).replace(/\w\S*/g, (t) => t.charAt(0).toUpperCase() + t.substring(1).toLowerCase())}</title>
  <meta property="og:title" content="Patriot Motorsports" />
  <meta property="og:description" content="George Mason University's FSAE Team." />
  <meta property="og:image" content="https://patriotfsae.vercel.app/logo.jpg" />
  <meta property="og:url" content="https://patriotfsae.vercel.app/" />
</svelte:head>

<Layout {navigate} {open} {hamburger} {route}>
  {#if route === "/home"}
    <Home {navigate} />
  {:else if route === "/join"}
    <Join />
  {:else if route === "/sponsorship"}
    <Sponsorship />
  {:else if route === "/leadership"}
    <Leadership />
  {:else}
    <NotFound />
  {/if}
</Layout>
