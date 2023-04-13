<script>
    export let raga = [], notes, octave
    export let play, addNote

    const octxt = octave==3?'mandra':octave==4?'madhya':'taar'
</script>

<div>
    <!-- <h5>{octxt.charAt(0).toUpperCase()+octxt.slice(1)}</h5> -->
    {#each raga as n}
      <button on:click={_ => {
        play(notes[n], octave, 0, 1)
        addNote([notes[n], octave])
      }} class={octxt}>{n}</button>
    {/each}
</div>

<style>
    button {
        width: 50px;
        padding: 0.6em 0;
    }
</style>