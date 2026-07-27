<script>
  import { onMount } from 'svelte';
  import { runCommand } from '../lib/api.js';

  let { binary } = $props();
  let stats = $state('');
  let loading = $state(true);

  onMount(async () => {
    try {
      stats = await runCommand(['stats']);
    } catch (e) {
      stats = `Failed to load stats: ${e}`;
    } finally {
      loading = false;
    }
  });
</script>

<div class="view">
  <header>
    <h1 class="glow-text">◈ Dashboard</h1>
    <p class="sub">The harness that learns. The agent that decides.</p>
  </header>

  <div class="grid">
    <div class="card">
      <h3>Binary</h3>
      {#if binary?.found}
        <p><span class="badge ok">installed</span></p>
        <p class="kv"><span>version</span><code>{binary.version ?? 'unknown'}</code></p>
        <p class="kv"><span>path</span><code>{binary.path}</code></p>
      {:else}
        <p><span class="badge err">not found</span></p>
        <p class="hint">Install openshark or add it to your PATH.</p>
      {/if}
    </div>

    <div class="card wide">
      <h3>Session Stats</h3>
      {#if loading}
        <p class="hint">Loading…</p>
      {:else}
        <pre class="output">{stats || 'No stats yet. Start a session.'}</pre>
      {/if}
    </div>
  </div>
</div>

<style>
  .view { max-width: 1000px; }
  header h1 { color: var(--neon-cyan); font-size: 24px; }
  .sub { color: var(--text-dim); margin-top: 4px; font-size: 12px; }
  .grid {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 16px;
    margin-top: 24px;
  }
  h3 { color: var(--neon-purple); font-size: 13px; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 12px; }
  .kv { display: flex; justify-content: space-between; gap: 12px; margin-top: 8px; font-size: 12px; }
  .kv span { color: var(--text-dim); }
  .kv code { color: var(--neon-yellow); overflow: hidden; text-overflow: ellipsis; }
  .hint { color: var(--text-dim); font-size: 12px; margin-top: 8px; }
  .output { font-size: 12px; color: var(--text); max-height: 400px; overflow-y: auto; }
  .wide { min-height: 200px; }
  @media (max-width: 800px) { .grid { grid-template-columns: 1fr; } }
</style>
