<script setup>

</script>

<template>
  <div>
    <ol>
      <li><h4>Use of DCT as pre-processing step</h4>
        <p> Discrete Cosine Transform is a type of transform which expresses a finite sequence of data points in terms
          of a sum of cosine functions oscillating at different frequencies.
          It is very common in compression algorithms such as MP3 and MP4, JPEG, JXL etc.
        </p>
        <p>It is loved in compression because of the power it has in condensing energy in one area of the matrix when
          data is easily compressible </p>

        <div id="f1" class="latex-c center">
          F(u,v) = \frac{2}{N} C(u)C(v) \sum_{x=0}^{N-1} \sum_{y=0}^{N-1} f(x,y) \cos\left(\frac{(2x+1)u\pi}{2N}\right)
          \cos\left(\frac{(2y+1)v\pi}{2N}\right)
        </div>
        <div class="center">
          Where
        </div>
        <div id="f2" class="latex-c center">

          C(u), C(v) = \begin{cases}
          \frac{1}{\sqrt{2}} & \text{for } u,v = 0 \\
          1 & \text{otherwise}
          \end{cases}
        </div>
        <p>Compression algorithms like mp3 remove low intensity noise by passing an audio through DCT filter and then
          quantizing the output(dividing it by a number and either flooring or rounding )
          low intensity noise is lost because the floor/round up looses some information,
          this is what makes those algorithms lossy(lose information) </p>

        <p> This model borrows the DCT and quantizing step as a preprocessing stage to remove low intensity noise from
          the audio, shown to boost the model performance </p>

        <img src="/public/img_1.png" style="width: 100%"/>

        <p class="center"> Red->DCT on, Orange->DCT off , Red model learns better with time </p>

      </li>
      <li>
        <h4>Generative instead of Mask model</h4>
        <p>
          Most models of such nature like <a href="https://arxiv.org/abs/2209.15174" target="_blank"> BandSpit-RNN</a>,
          <a href="https://github.com/sigsep/open-unmix-pytorch" target="_blank">OpenUnmix</a>,
          <a href="https://arxiv.org/abs/1809.07454" target="_blank">ConvTasNet</a> train neural networks to generate
          masks for separation.

        </p>

        <p>A mask model learns how to separate audio into different parts by learning a mask (0 and 1) which to apply to
          a mix to get the constituent stem</p>
        <p>This is known to have limitations, if two separate audio items share the same frequencies, a mask model
          cannot separate the two</p>
        <p>Generative models should not have that problem theoretically </p>
        <p> In experiments, it was shown that generative perform better than masking with dataset and architecture
          remaining constant </p>
        <img src="/public/img.png" style="width: 100%"/>

        <p class="center"> Green->Mask Model, Grey->Generative model </p>
      </li>
      <li>
        <h4> Attention layer + Bi-LSTM for bottleneck</h4>
        <p>
          U-Net models like <a href="https://github.com/sigsep/open-unmix-pytorch" target="_blank">OpenUnmix</a> and
          <a href="https://github.com/facebookresearch/demucs" target="_blank">Demucs</a>
          use a bi-directional Long Short Term Memory (LSTM) as their bottleneck layer.
        </p>
        <p> Experiments have shown adding an attention layer boosts speed for the current dataset and architecture</p>

        <img src="/public/img_2.png" style="width: 100%"/>

        <p class="center"> Red->Normal Bi-LSTM, Grey->With Self-Attention </p>
      </li>
    </ol>
  </div>
</template>

<style scoped>
.latex-c {

}

.center {
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>

<script>

export default {
  mounted() {
    this.renderFormulas()
  },
  methods: {
    renderFormulas() {
      console.log('Render Formulas');
      const allLatex = document.getElementsByClassName('latex-c')
      for (let i = 0; i < allLatex.length; i++) {
        let element = allLatex[i]
        katex.render(element.innerText, element, {throwOnError: true})
      }


    }
  }
}
</script>