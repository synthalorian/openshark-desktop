<script>
  import { runStream } from '../lib/api.js';

  let task = $state('');
  let output = $state('');
  let running = $state(false);
  let outputEl;

  async function run() {
    const t = task.trim();
    if (!t || running) return;
    running = true;
    output = `$ openshark agent "${t}"\n\n`;

    try {
      await runStream(['agent', t], (ev) => {
        if (ev.event === 'stdout' || ev.event === 'stderr') {
          output += ev.data;
        } else if (ev.event === 'done') {
          output += `\n\n[process exited with code ${ev.data}]`;
        }
        requestAnimationFrame(() => {
          if (outputEl) outputEl.scrollTop = outputEl.scrollHeight;
        });
      });
    } catch (e) {
      output += `\nError: ${e}`;
    } finally {
      running = false;
    }
  }
</script>

<div class="view">
  <header>
    <h1 class="glow-text">⚡ Agent</h1>
    <p class="sub">Autonomous task execution — plan, tool, ship.</p>
  </header>

  <div class="controls">
    <input bind:value={task} placeholder="Describe the task…" onkeydown={(e) => e.key === 'Enter' && run()} />
    <button class="primary" onclick={run} disabled={running || !task.trim()}>
      {running ? 'Running…' : 'Run Agent'}
    </button>
  </div>

  <pre class="output card" bind:this={outputEl}>{output || 'Output will stream here.'}</pre>
</div>

<style>
  .view { display: flex; flex-direction: column; height: 100%; max-width: 1000px; }
  header h1 { color: var(--neon-cyan); font-size: 24px; }
  .sub { color: var(--text-dim); font-size: 12px; margin-top: 4px; }
  .controls { display: flex; gap: 10px; margin: 16px 0; }
  .controls input { flex: 1; }
  .output { flex: 1; overflow-y: auto; font-size: 12px; line-height: 1.5; min-height: 0; color: var(--text); }
</style>
