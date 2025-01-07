<script setup>

</script>

<template>
  <h2>Problems</h2>

  <ol>
    <li>
      ONNX issues with STFT
      <p><a target="_blank" href="https://onnx.ai/"> ONNX </a> or Open Neural Network Exchange is an open standard for
        Machine Learning
        interoperability. It allows any onnx compatible model to run locally on machines capable of running them without
        need for a whole python environment.</p>
      <p>
        STFT(short for Short Term Fourier Transform) is a function that converts an audio waveform to its frequency
        components
        (using the fourier transforms) in a windowed way that allows recovery of the original signal using <a
          href="https://pytorch.org/docs/stable/generated/torch.istft.html"> inverse stft </a>
      </p>
      <p>
        Initially the project wanted to use it to show real-time capabilities in phones by making a package
        that packages the model as onnx format and runs it in the hardware but that was not possible due to challenges
        pointed below.
      </p>
    </li>
    <ul>
      <li>
        Onnx has problems with inbuilt <a target="_blank"
                                          href="https://pytorch.org/docs/stable/generated/torch.stft.html#torch.stft">torch.stft</a>
        <p>
          The return type for the <code>torch.stft</code> function is <code>complex64</code> as of now ,
          <code>onnx</code>
          does not support <code>complex</code> types, see <a target="_blank"
                                                              href="https://github.com/pytorch/pytorch/issues/86746">Pytorch
          Issue #86746</a>
        </p>
      </li>
      <li>
        Current recommendation to set <code>return_complex=False</code> for <code>torch.stft </code>no longer works
        <p>
          <code>torch.stft</code> has a parameter <code>return_complex=False</code> which changes the layout to be a
          view of
          the real and complex parts as separate, i.e <code>[43.043+32.32j,12.01+0.234j]</code> becomes <code>[[43.043,12.01],[32.32,0.234]]</code>
          Adding this to onnx version 17 still fails since <code>torch.istft</code> does not parse such layouts, only
          accepts complex numbers
        </p>
        <p>
          Also <code>return_complex=False</code> is deprecated as of version 2.0
        </p>
      </li>
      <li>
        ONNX still does not work with third party stft
        <p>
          There are packages like <a href="https://github.com/asteroid-team/asteroid-filterbanks" target="_blank">
          asteroid-filterbanks</a> that provide stft functions that can compile in onnx but the stft->istft does not
          roundtrip resulting in generation of noisy sound.
        </p>
        <p>
          Filled an issue with onnx for the above at <a target="_blank"
                                                        href="https://github.com/microsoft/onnxruntime/issues/23219">
          Issue #23219
        </a>
          . No response at the time of writing
        </p>
      </li>
    </ul>


  </ol>


</template>

<style scoped>
code {
  font-family: unset;
  padding-left: 3px;
  padding-right: 3px;
  background-color: #F2F4F6;
  font-weight: 600;
}

</style>