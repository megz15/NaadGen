<script>
  import * as Tone from 'tone';
    import NoteInput from './lib/NoteInput.svelte';
  const synth = new Tone.PolySynth(Tone.Synth).toDestination();
  
  const taals = {
    'Jhaptaal': [2,5,7,10],
    'Deepchandi': [3,7,10,14]
  }

  const notes = {
    'Sa'  : 'C',
    'R_K'  : 'C#',
    'Re'  : 'D',
    'G_K'  : 'D#',
    'Ga'  : 'E',
    'Ma'  : 'F',
    'M_T': 'F#',
    'Pa'  : 'G',
    'D_K' : 'G#',
    'Dha' : 'A',
    'N_K'  : 'A#',
    'Ni'  : 'B',
    '': ''
  }
  
  const ragas = {
    'Kafi': ['Sa','Re','G_K','Ma','Pa','Dha','N_K']
  }

  const taal = taals['Deepchandi']
  const raga = ragas['Kafi']

  let composition = [] //[...Array(taal.at(-1)).keys()]

  function play(n, o, t, v) {
    if (n == '') synth.triggerAttackRelease(null, '8n', t, v);
    else synth.triggerAttackRelease(n+o, '8n', Tone.now() + t, v);
  }

  function addNote(n) {
    composition = [...composition, n]
  }

  function playcomp() {
    composition.forEach((n,i) => {
      const t = taal
      play(n[0], n[1], i/4, (t.includes(i%t.at(-1)) || i%t.at(-1)==0)?5:1)
    });
  }

</script>

<main>
  <h1>NaadGen</h1>
  
  <div class="ctrl">
    <div class="btns">
      {#each [3,4,5] as o}
        <NoteInput raga={raga} notes={notes} octave={o} play={play} addNote={addNote}/>
      {/each}
    </div>

    <div class="btns">
      <button on:click={_ => {
        addNote(['', 0])
      }}>Rest</button>

      <button class="play" on:click={playcomp}>Play</button>

      <button>Clear</button>
    </div>
  </div>
  
  <div class="wrap">
    <table>
      {#each [...Array(taal.at(-1)).keys()] as i}
        <td>
          {#if (taal.slice(0,-1).includes(i))}<vibhaag />{/if}
          <note>{(i+1).toString().padEnd(3)}</note>
        </td>
      {/each}

      {#each composition as n, i}
        
        {#if (i%taal.at(-1)==0)}<tr />{/if}

        <td>
          {#if (taal.slice(0,-1).includes(i%taal.at(-1)))}<vibhaag />{/if}
          
          <note class="{n[1]==3?'mandra':n[1]==4?'madhya':'taar'}">{
            Object.keys(notes).find(key => notes[key] === n[0]).padEnd(3)
          }</note>
        </td>
        
      {/each}
    </table>
  </div>

</main>

<style>
  note {
    font-family: monospace;
    white-space: pre;
  }

  .play {
    background-color: green;
  }

  table {
    margin: 2em;
    border: 1px solid black;
  }

  td {
    padding: 10px;
    /* width: 50px;
    display: inline-block; */
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

  .btns {
    display: flex;
    flex-direction: column;
    justify-content:space-between;
    /* align-items: center; */
    /* justify-content: center; */
  }

  .ctrl {
    display: flex;
    gap: 2em;
    /* flex-direction: row; */
    /* align-items: center; */
    justify-content: center;
  }

  .wrap {
    /* background-color: cyan; */
    /* width: 100em; */
    max-width: 100%;
    overflow-x: scroll;
    display: inline-flex;
    justify-content: start;
    /* visibility: hidden; */
  }
</style>
