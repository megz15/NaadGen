<script>
  import * as Tone from 'tone'
  import NoteInput from './lib/NoteInput.svelte'
  
  import taals from './assets/taals.json'
  import ragas from './assets/ragas.json'
  import logo from '../public/logo.png'

  const synth = new Tone.PolySynth(Tone.Synth).toDestination();

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

  let taal = Object.keys(taals)[0]
  let raga = Object.keys(ragas)[0]

  let composition = []

  function play(n, o, t, v) {
    if (n == '') synth.triggerAttackRelease(null, '8n', t, v);
    else synth.triggerAttackRelease(n+o, '8n', Tone.now() + t, v);
  }

  const addNote = (n) => composition = [...composition, n]

  const playcomp = () => composition.forEach((n,i) => {
      const t = taals[taal]
      play(n[0], n[1], i/4, (t.includes(i%t.at(-1)) || i%t.at(-1)==0)?5:1)
  })

</script>

<main>
  <img src="{logo}" class="logo" alt="NaadGen Logo" />

  <h1>NaadGen</h1>
  
  <div style="margin:1em">
    <select bind:value={taal} name="taals">
      <optgroup label="Select a Taal" />
      {#each Object.keys(taals) as taal}
        <option>{taal}</option>
      {/each}
    </select>

    <select bind:value={raga} name="ragas">
      <optgroup label="Select a Raga" />
      {#each Object.keys(ragas) as raga}
        <option>{raga}</option>
      {/each}
    </select>

    <button class="play" on:click={playcomp}>Play</button>
  </div>

  <div class="ctrl">
    <div class="btns">
      {#each [3,4,5] as o}
        <NoteInput raga={ragas[raga]} notes={notes} octave={o} play={play} addNote={addNote}/>
      {/each}
    </div>

    <div class="btns">
      <button on:click={_ => {
        addNote(['', 0])
      }}>Rest</button>

      <button class="warn" on:click={_ => composition = composition.slice(0,-1)}>Delete</button>
      <button class="warn" on:click={_ => composition = []}>Clear</button>
    </div>
  </div>
  
  <div class="wrap">
    <table>
      {#each [...Array(taals[taal].at(-1)).keys()] as i}
        <td>
          {#if (taals[taal].slice(0,-1).includes(i))}<vibhaag />{/if}
          <note class='matra'>{(i+1).toString().padEnd(3)}</note>
        </td>
      {/each}

      {#each composition as n, i}
        
        {#if (i%taals[taal].at(-1)==0)}<tr />{/if}

        <td>
          {#if (taals[taal].slice(0,-1).includes(i%taals[taal].at(-1)))}<vibhaag />{/if}
          
          <note class="{n[0]==''?'matra':n[1]==3?'mandra':n[1]==4?'madhya':'taar'}">{
            Object.keys(notes).find(key => notes[key] === n[0]).padEnd(3)
          }</note>
        </td>
        
      {/each}
    </table>
  </div>

  <div class='footer'>insert footer text later lorem ipsum</div>

</main>

<style>
  note {
    font-family: monospace;
    white-space: pre;
  }

  .play {
    background-color: green;
    margin-left: 10px;
  }

  .warn {
    background-color: #A71B28;
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

  select {
    width: 25%;
    max-width: 195px;
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

  @media (max-width: 645px) {
    .ctrl {flex-direction: column}
    .btns {flex-direction: row}
  }

  .wrap {
    /* background-color: cyan; */
    /* width: 100em; */
    max-width: 100%;
    min-height: 162px;
    overflow-x: scroll;
    display: inline-flex;
    justify-content: start;
    /* visibility: hidden; */
  }

  .matra {
    background-color: aliceblue;
    color: black;
    font-weight: 900;
  }
</style>
