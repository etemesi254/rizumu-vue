<script setup>

</script>

<template>
  <div>
    <i>Copied in verbatim from the <a href="https://zenodo.org/records/6949108"> DnR Page</a> </i>
    <p>
      Divide and Remaster (DnR) is a source separation dataset for training and testing algorithms that separate a
      monaural audio signal into speech, music, and sound effects/background stems. The dataset is composed of
      artificial mixtures using audio from the librispeech, free music archive (FMA), and Freesound Dataset 50k
      (FSD50k). We introduce it as part of the
      <a href="https://arxiv.org/abs/2110.09958">Cocktail Fork Problem
        Paper</a>.
    </p>
    <strong>At a Glance</strong>
    <ul>
      <li>The size of the unzipped dataset is ~200GB</li>
      <li>Each mixture is 60 seconds long and sources are not fully overlapped</li>

      <li> The directory for each mixture contains four <code>.wav</code> files, <code> mix.wav</code>,
        <code>music.wav</code>, <code>speech.wav</code>, <code>sfx.wav</code>, and
        <code>annots.csv</code>
        which contains the metadata for the original audio used to compose the mixture (transcriptions for speech, sound
        classes for sfx, and genre labels for music)
      </li>
    </ul>

    <strong>Training Configuration</strong>
    <p>
      Due to machine limitations the project could not utilize the full dataset. Part of the dataset was used, 40GB of
      the 100 GB compressed dataset
    </p>
    <p> The dataset was split into the following configurations</p>

    <table>
      <tbody>
      <tr>
        <td>
          Train Set
        </td>
        <td>
          1384 Samples
        </td>
      </tr>
      <tr>
        <td>
          Test Set
        </td>
        <td>
          405 Samples
        </td>
      </tr>
      </tbody>

    </table>


  </div>
</template>

<style scoped>
td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

table {
  border-collapse: collapse;
  width: 100%;
}


tr:nth-child(even) {
  background-color: #dddddd;
}

code {
  font-family: unset;
  padding-left: 3px;
  padding-right: 3px;
  background-color: #F2F4F6;
  font-weight: 600;
}
</style>