
<script>
    // @ts-nocheck
    import { onMount } from 'svelte';
    import Tesseract from 'tesseract.js';

    let selectedImage = null;
    let recognizedText = null;

    const handleImageUpload = (event) => {
      const image = event.target.files[0];
      selectedImage = URL.createObjectURL(image);
    };
  
    const recognizeText = async () => {
      const { data: { text } } = await Tesseract.recognize(selectedImage, 'eng');
      recognizedText = text;
    };

    // when the selectedImage variable changes, run the recognizeText function

    $: if (selectedImage) {
      recognizeText();
    }

    // Cleanup when the component is destroyed to prevent memory leaks
    onMount(() => {
      return () => {
        if (selectedImage) {
          URL.revokeObjectURL(selectedImage);
        }
      };
    });
  </script>
  
  <style>
    /* Add your styles here */
  </style>
  
  <div>
    <input type="file" accept="image/*" on:change={handleImageUpload} />
    {#if selectedImage}
      <img src={selectedImage} alt="Selected" />
    {/if}
    <p>{recognizedText}</p>
  </div>
  