<!-- src/routes/+page.svelte -->
<script>
  // no JS required for this example — simple and fast
  import Slider from '../components/Slider.svelte';
</script>

<svelte:head>
  <title>Hello • Dark Homepage</title>



  <meta name="description" content="A minimal dark SvelteKit homepage that says Hello world." />
</svelte:head>

<main class="page">
  <div class="card" role="region" aria-labelledby="hello-heading">
    <h1 id="hello-heading">Hello world</h1>

    <p class="sub">Welcome — this is a minimal dark SvelteKit homepage.</p>
    <div class="accent" aria-hidden="true"></div>

  </div>

  <footer class="footer">
    <small>Built with SvelteKit • Minimal • Dark UI</small>
  </footer>

  <Slider/>
</main>

<style>
  :root{
    --bg-1: #0b0f14; /* very dark */
    --bg-2: #0f1720; /* slightly lighter */
    --card: rgba(255,255,255,0.04);
    --glass: rgba(255,255,255,0.03);
    --accent: #7c5cff;
    --muted: rgba(255,255,255,0.6);
    --radius: 16px;
  }

  /* Full-viewport centered layout */
  .page{
    min-height: 100vh;
    display: grid;
    place-items: center;
    gap: 24px;
    padding: 48px 20px;
    background: radial-gradient(1200px 600px at 10% 10%, rgba(124,92,255,0.08), transparent),
                linear-gradient(180deg, var(--bg-1), var(--bg-2));
    color: white;
    font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  .card{
    position: relative;
    width: min(860px, 92vw);
    padding: 48px;
    border-radius: var(--radius);
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    box-shadow:
      0 10px 30px rgba(2,6,23,0.6),
      inset 0 1px 0 rgba(255,255,255,0.02);
    backdrop-filter: blur(8px) saturate(120%);
    border: 1px solid rgba(255,255,255,0.03);
    overflow: hidden;
  }

  h1{
    margin: 0 0 8px 0;
    font-size: clamp(2rem, 5vw, 3.25rem);
    letter-spacing: -0.02em;
    line-height: 1.02;
    font-weight: 700;
    text-wrap: balance;
  }

  .sub{
    margin: 0;
    color: var(--muted);
    font-size: 0.98rem;
  }

  .accent{
    position: absolute;
    right: -10%;
    top: -20%;
    width: 420px;
    height: 420px;
    filter: blur(48px);
    background: linear-gradient(135deg, rgba(124,92,255,0.28), rgba(0,188,212,0.12));
    transform: rotate(18deg);
    opacity: 0.9;
    pointer-events: none;
    mix-blend-mode: screen;
  }

  .footer{
    color: var(--muted);
    text-align: center;
    font-size: 0.86rem;
    opacity: 0.95;
  }

  /* Small screens — tighten padding */
  @media (max-width: 520px){
    .card{ padding: 28px; border-radius: 12px; }
    h1{ font-size: clamp(1.6rem, 8vw, 2.4rem); }
  }

  /* Respect reduced motion */
  @media (prefers-reduced-motion: no-preference){
    .accent{
      animation: float 9s ease-in-out infinite;
    }
    @keyframes float{
      0%   { transform: translateY(0) rotate(18deg) scale(1); }
      50%  { transform: translateY(12px) rotate(18deg) scale(1.03); }
      100% { transform: translateY(0) rotate(18deg) scale(1); }
    }
  }
</style>
