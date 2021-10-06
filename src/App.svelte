<script>
  import { keyData, audioData } from './data'

  const keyDataCodes = keyData.map(item => item.code)

  const handleTransitionend = e => e.target.classList.remove('playing')

  const handleKeydown = e => {
    const pressedKeyCode = e.keyCode

    if (!keyDataCodes.includes(pressedKeyCode)) return

    const keyEl = document.querySelector(`div[data-key="${pressedKeyCode}"]`)

    const audioEl = document.querySelector(
      `audio[data-key="${pressedKeyCode}"]`
    )
    keyEl.classList.add('playing')
    audioEl.currentTime = 0
    audioEl.play()
  }
</script>

<svelte:window on:keydown={handleKeydown} />

<main>
  {#each keyData as { code, kbd, sound }}
    <div data-key={code} on:transitionend={handleTransitionend}>
      <kbd>{kbd}</kbd>
      <span>{sound}</span>
    </div>
  {/each}
</main>

{#each audioData as { key, src }}
  <audio data-key={key} {src} />
{/each}

<style>
  main {
    display: flex;
    flex: 1;
    min-height: 100vh;
    align-items: center;
    justify-content: center;
  }

  div {
    border: 0.4rem solid black;
    border-radius: 0.5rem;
    margin: 1rem;
    font-size: 1.5rem;
    padding: 1rem 0.5rem;
    transition: all 0.07s ease;
    width: 10rem;
    text-align: center;
    color: white;
    background: rgba(0, 0, 0, 0.4);
    text-shadow: 0 0 0.5rem black;
  }

  :global(.playing) {
    transform: scale(1.1);
    border-color: #ffc600;
    box-shadow: 0 0 1rem #ffc600;
  }

  kbd {
    display: block;
    font-size: 4rem;
  }

  span {
    font-size: 1.2rem;
    text-transform: uppercase;
    letter-spacing: 0.1rem;
    color: #ffc600;
  }
</style>
