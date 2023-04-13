<script>
  import * as Tone from 'tone';
  const synth = new Tone.PolySynth(Tone.Synth).toDestination();
  
  const taal = {
    'Jhaptaal': [2,5,7,10]
  }
  const notes = {'B3':'Sa','C#4':'Re','D#4':'Ga','E4':'Ma','F#4':'Pa','G#4':'Dha','A#4':'Ni'}
  let composition = []

  function play(n, t, v) {
    synth.triggerAttackRelease(n, '8n', t, v);
  }

  function addNote(n) {
    // if (composition.length % 5 == 0) composition = [...composition, '|', n]
    composition = [...composition, n]
  }

  function playcomp() {
    composition.forEach((n,i) => {
      play(n, Tone.now() + i/4, (taal['Jhaptaal'].includes(i%10) || i%10==0)?5:1)
    });

    // new Tone.Sequence((_, n) => {
    //   play(n, ());
    // }, composition).start(0);
    // Tone.Transport.start();
  }

</script>

<main>
  <h1>NaadGen</h1>
  
  {#each Object.keys(notes) as n}
    <button on:click={_ => {
      play(n, Tone.now(), 1)
      addNote(n)
    }}>{notes[n]}</button>
  {/each}

  <button class="play" on:click={playcomp}>Play</button><br>
  
  <table>
    <!-- <tr>
        {#each [1,2,3,4,5,6,7,8] as n}
          <th><note>{n}</note></th>
        {/each}
    </tr> -->
    {#each composition as n, i}
      {#if (i%taal['Jhaptaal'].at(-1)==0)}
        <tr></tr>
      {/if}
      <td>
        <!-- {#if ((i%5==0 || i%7==0) && i!=0 && i%10!=0)} -->
        {#if (taal['Jhaptaal'].slice(0,-1).includes(i%10))}
          <vibhaag /><note>{n}</note>
        {:else}
          <note>{n}</note>
        {/if}
      </td>
      <!-- <td><note>{n}</note></td> -->
    {/each}
  </table>

</main>

<style>
  note {
    background-color: #000000;
  }

  .play {
    background-color: green;
  }

  table {
    border: 1px solid red;
  }

  td {
    padding: 10px;
  }

  vibhaag {
    border: 1px solid transparent;
    padding: 13px 0.01px;
    position: relative;
    left: -15%;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    background-color: #000000;
    transition: border-color 0.25s;
  }
</style>
