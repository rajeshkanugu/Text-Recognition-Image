
<script>
    // @ts-nocheck
    import { onMount } from 'svelte';
    import Tesseract from 'tesseract.js';

    // Variables to store the selected image and the recognized text
    let selectedImage = null;
    let recognizedText = 'Image not uploaded yet';

    // Function to handle the image upload
    const handleImageUpload = (event) => {
      const image = event.target.files[0];
      selectedImage = URL.createObjectURL(image);
    };
  
    // Function to recognize text in the selected image
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

    onMount(() => {
        const button = document.querySelector('button');
        const input = document.querySelector('input');
        button.addEventListener('click', () => {
            input.click();
        });
    });

  </script>
  
  <style>
    /* Add your styles here */
    img {
        width: 200px;
        height: 200px;
    }

    div {
        height: 100vh;
        widows: 100vw;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    input {
        width: 50px;
        height: 50px;
        display: none;
    }

  </style>
  
  <div>
    <button>upload image</button>
    <input type="file" accept="image/*" on:change={handleImageUpload} />
    {#if selectedImage}
      <img src={selectedImage} alt="Selected" />
    {/if}
    <p>{recognizedText}</p>
  </div>
  