<script>
    export let raga, notes, octave
    export let play, addNote
</script>

<div>
    {#each raga as n}
      <button on:click={_ => {
        play(notes[n], octave, 0, 1)
        addNote([notes[n], octave])
      }} class="{octave==3?'mandra':octave==4?'madhya':'taar'}">{n}</button>
    {/each}
</div>

<style>
    button {
        margin: 1px;
    }
</style>