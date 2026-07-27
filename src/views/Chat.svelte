<script>
  import { runStream } from '../lib/api.js';

  let input = $state('');
  let messages = $state([]);
  let running = $state(false);
  let outputEl;

  async function send() {
    const msg = input.trim();
    if (!msg || running) return;
    input = '';
    running = true;

    messages = [...messages, { role: 'user', text: msg }];
    messages = [...messages, { role: 'assistant', text: '' }];
    const idx = messages.length - 1;

    try {
      await runStream(['chat', msg], (ev) => {
        if (ev.event === 'stdout') {
          messages[idx].text += ev.data;
        } else if (ev.event === 'stderr') {
          // openshark writes progress to stderr; append subtly
          messages[idx].text += ev.data;
        } else if (ev.event === 'done' && ev.data !== '0') {
          messages[idx].text += `\n[exited with code ${ev.data}]`;
        }
        scrollDown();
      });
    } catch (e) {
      messages[idx].text = `Error: ${e}`;
    } finally {
      running = false;
    }
  }

  function scrollDown() {
    requestAnimationFrame(() => {
      if (outputEl) outputEl.scrollTop = outputEl.scrollHeight;
    });
  }

  function onKeydown(e) {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      send();
    }
  }
</script>

<div class="view">
  <header>
    <h1 class="glow-text">▸ Chat</h1>
  </header>

  <div class="transcript" bind:this={outputEl}>
    {#if messages.length === 0}
      <p class="empty">Ask the shark anything. It bites fast.</p>
    {/if}
    {#each messages as m}
      <div class="msg {m.role}">
        <span class="who">{m.role === 'user' ? 'you' : '🦈 openshark'}</span>
        <pre>{m.text}{#if running && m === messages[messages.length - 1]}<span class="cursor">▊</span>{/if}</pre>
      </div>
    {/each}
  </div>

  <div class="composer">
    <textarea
      bind:value={input}
      onkeydown={onKeydown}
      placeholder="Message openshark… (Enter to send)"
      rows="2"
    ></textarea>
    <button class="primary" onclick={send} disabled={running || !input.trim()}>
      {running ? '…' : 'Send'}
    </button>
  </div>
</div>

<style>
  .view { display: flex; flex-direction: column; height: 100%; max-width: 900px; }
  header h1 { color: var(--neon-cyan); font-size: 24px; margin-bottom: 12px; }

  .transcript {
    flex: 1;
    overflow-y: auto;
    background: var(--bg-panel);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    min-height: 0;
  }

  .empty { color: var(--text-dim); font-size: 13px; }

  .who { font-size: 11px; text-transform: uppercase; letter-spacing: 1px; display: block; margin-bottom: 4px; }
  .msg.user .who { color: var(--neon-yellow); }
  .msg.assistant .who { color: var(--neon-pink); }
  .msg pre { font-size: 13px; line-height: 1.5; }

  .cursor { color: var(--neon-cyan); animation: blink 1s step-end infinite; }
  @keyframes blink { 50% { opacity: 0; } }

  .composer { display: flex; gap: 10px; margin-top: 12px; }
  textarea { flex: 1; resize: none; }
</style>
