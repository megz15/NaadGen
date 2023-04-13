<script>
  import * as Tone from 'tone';
  const synth = new Tone.PolySynth(Tone.Synth).toDestination();
  
  function play(n) {
    synth.triggerAttackRelease(n, '8n');
  }
  
  const notes = {'B3':'Sa','C#4':'Re','D#4':'Ga','E4':'Ma','F#4':'Pa','G#4':'Dha','A#4':'Ni'}

</script>

<main>
  <h1>NaadGen</h1>
  {#each Object.keys(notes) as n}
    <button on:click={_ => play(n)}>{notes[n]}</button>
  {/each}
</main>

<style>

</style>
