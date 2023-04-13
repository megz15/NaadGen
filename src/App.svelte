<script>
  import * as Tone from 'tone';
  const synth = new Tone.PolySynth(Tone.Synth).toDestination();
  
  const taals = {
    'Jhaptaal': [2,5,7,10],
    'Deepchandi': [3,7,10,14]
  }

  const notes = {
    'Sa'  : ['B', -1],
    'Re_K': ['C', 0],
    'Re'  : ['C#', 0],
    'Ga_K': ['D', 0],
    'Ga'  : ['D#', 0],
    'Ma'  : ['E', 0],
    'Ma_T': ['F', 0],
    'Pa'  : ['F#', 0],
    'Dha_K': ['G', 0],
    'Dha' : ['G#', 0],
    'Ni_K': ['A', 0],
    'Ni'  : ['A#', 0],
  }
  
  const ragas = {
    'Kafi': ['Sa','Re','Ga_K','Ma','Pa','Dha','Ni_K']
  }

  let composition = []

  const taal = taals['Deepchandi']
  const raga = ragas['Kafi'] //Object.assign({}, notes, ragas['Kafi'])

  function play(n, o, t, v) {
    if (n+o == '') synth.triggerAttackRelease(null, '8n', t, v);
    else synth.triggerAttackRelease(n+o, '8n', t, v);
  }

  function addNote(n) {
    // if (composition.length % 5 == 0) composition = [...composition, '|', n]
    composition = [...composition, n]
  }

  function playcomp() {
    composition.forEach((n,i) => {
      const t = taal
      play(n[0], n[1], Tone.now() + i/4, (t.includes(i%t.at(-1)) || i%t.at(-1)==0)?5:1)
    });
  }

</script>

<main>
  <h1>NaadGen</h1>
  
  {#each raga as n}
    <button on:click={_ => {
      play(notes[n][0], notes[n][1]+4, Tone.now(), 1)
      addNote([notes[n][0], notes[n][1]+4])
    }}>{n}</button>
  {/each}

  <button on:click={_ => {
    addNote(['',''])
  }}>Rest</button>

  <button class="play" on:click={playcomp}>Play</button><br>
  
  <table>
    <!-- <tr>
        {#each [1,2,3,4,5,6,7,8] as n}
          <th><note>{n}</note></th>
        {/each}
    </tr> -->
    {#each composition as n, i}
      {#if (i%taal.at(-1)==0)}
        <tr></tr>
      {/if}
      <td>
        <!-- {#if ((i%5==0 || i%7==0) && i!=0 && i%10!=0)} -->
        {#if (taal.slice(0,-1).includes(i%taal.at(-1)))}
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
</style>
