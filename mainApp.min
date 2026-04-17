document.addEventListener('DOMContentLoaded', function() {
  
  // Current app language (default to English)
  let currentAppLang = 'en';
  
  // App language functionality
  const appLanguageSelect = document.getElementById('app-language');
  
  function updateAppLanguage(lang) {
    currentAppLang = lang;
    const t = translations[lang];
    
    // Update main elements
    document.getElementById('app-title').textContent = t.title;
    document.getElementById('app-subtitle').textContent = t.subtitle;
    document.querySelector('label[for="source-lang"]').textContent = t.from;
    document.querySelector('label[for="target-lang"]').textContent = t.to;
    document.querySelector('label[for="input-text"]').textContent = t.sourceText;
    document.querySelector('label[for="output-text"]').textContent = t.resultText;
    document.getElementById('translate-btn').querySelector('span').textContent = t.translate;
    document.getElementById('copy-btn').innerHTML = `<i class="fas fa-copy"></i> ${t.copy}`;
    document.getElementById('clear-btn').innerHTML = `<i class="fas fa-eraser"></i> ${t.clear}`;
    document.getElementById('read-aloud').innerHTML = `<i class="fas fa-volume-up"></i> ${t.listen}`;
    document.getElementById('input-text').placeholder = t.inputPlaceholder;
    document.querySelector('.loading p').textContent = t.translating;
    document.querySelector('.examples h3').textContent = t.examplesTitle;
    document.querySelector('.footer p').innerHTML = t.footer;
    
    // Update example buttons
    document.querySelector('[data-example="greeting"]').textContent = t.examples.greeting;
    document.querySelector('[data-example="travel"]').textContent = t.examples.travel;
    document.querySelector('[data-example="food"]').textContent = t.examples.food;
    document.querySelector('[data-example="business"]').textContent = t.examples.business;
    
    // Update output placeholder if it's still default
    const outputText = document.getElementById('output-text');
    const indonesianPlaceholder = 'Hasil terjemahan akan muncul di sini...';
    const englishPlaceholder = 'Translation result will appear here...';
    if (outputText.textContent === indonesianPlaceholder || outputText.textContent === englishPlaceholder) {
      outputText.textContent = t.resultPlaceholder;
    }
    
    // Update character counts
    updateCharCount();
    
    // Update theme toggle text
    updateThemeToggleText(document.documentElement.getAttribute('data-theme') || 'light');
  }
  
  appLanguageSelect.addEventListener('change', function() {
    updateAppLanguage(this.value);
  });
  
  // Theme toggle functionality
  const themeToggle = document.getElementById('theme-toggle');
  const root = document.documentElement;
  
  // Default to light theme
  root.setAttribute('data-theme', 'light');
  
  themeToggle.addEventListener('click', function() {
    const currentTheme = root.getAttribute('data-theme');
    const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
    
    root.setAttribute('data-theme', newTheme);
    updateThemeToggleText(newTheme);
  });
  
  function updateThemeToggleText(theme) {
    if (currentAppLang === 'id') {
      themeToggle.innerHTML = theme === 'dark' ? '<i class="fas fa-sun"></i> Terang' : '<i class="fas fa-moon"></i> Gelap';
    } else {
      themeToggle.innerHTML = theme === 'dark' ? '<i class="fas fa-sun"></i> Light' : '<i class="fas fa-moon"></i> Dark';
    }
  }
  
  // Elements
  const inputText = document.getElementById('input-text');
  const outputText = document.getElementById('output-text');
  const sourceLang = document.getElementById('source-lang');
  const targetLang = document.getElementById('target-lang');
  const translateBtn = document.getElementById('translate-btn');
  const swapBtn = document.getElementById('swap-languages');
  const copyBtn = document.getElementById('copy-btn');
  const clearBtn = document.getElementById('clear-btn');
  const readAloudBtn = document.getElementById('read-aloud');
  const inputChars = document.getElementById('input-chars');
  const outputChars = document.getElementById('output-chars');
  const loading = document.querySelector('.loading');
  const errorMessage = document.getElementById('error-message');
  const successMessage = document.getElementById('success-message');
  const exampleButtons = document.querySelectorAll('.example-btn');
  
  // Contoh teks
  const examples = {
    greeting: {
      en: "Hello, how are you? My name is John. It's a pleasure to meet you.",
      id: "Halo, apa kabar? Nama saya John. Senang bertemu dengan Anda.",
      es: "Hola, Â¿cÃ³mo estÃ¡s? Me llamo John. Es un placer conocerte.",
      fr: "Bonjour, comment allez-vous? Je m'appelle John. C'est un plaisir de vous rencontrer."
    },
    travel: {
      en: "I would like to book a hotel room for two people. What tourist attractions do you recommend?",
      id: "Saya ingin memesan kamar hotel untuk dua orang. Apa atraksi wisata yang Anda rekomendasikan?",
      es: "Me gustarÃ­a reservar una habitaciÃ³n de hotel para dos personas. Â¿QuÃ© atracciones turÃ­sticas recomiendas?",
      fr: "Je voudrais rÃ©server une chambre d'hÃ´tel pour deux personnes. Quelles attractions touristiques recommandez-vous?"
    },
    food: {
      en: "I would like to order pizza and pasta. Do you have vegetarian options?",
      id: "Saya ingin memesan pizza dan pasta. Apakah Anda punya opsi vegetarian?",
      es: "Me gustarÃ­a pedir pizza y pasta. Â¿Tienen opciones vegetarianas?",
      fr: "Je voudrais commander une pizza et des pÃ¢tes. Avez-vous des options vÃ©gÃ©tariennes?"
    },
    business: {
      en: "We need to schedule a meeting to discuss the project timeline and deliverables.",
      id: "Kami perlu menjadwalkan rapat untuk membahas timeline proyek dan deliverables.",
      es: "Necesitamos programar una reuniÃ³n para discutir el cronograma del proyecto y los entregables.",
      fr: "Nous devons planifier une rÃ©union pour discuter du calendrier du projet et des livrables."
    }
  };
  
  // Event listeners untuk contoh teks
  exampleButtons.forEach(button => {
    button.addEventListener('click', function() {
      const exampleType = this.getAttribute('data-example');
      const source = sourceLang.value === 'auto' ? 'en' : sourceLang.value;
      
      if (examples[exampleType] && examples[exampleType][source]) {
        inputText.value = examples[exampleType][source];
        updateCharCount();
      } else {
        // Fallback ke bahasa Inggris jika contoh tidak tersedia dalam bahasa sumber
        inputText.value = examples[exampleType]['en'];
        updateCharCount();
      }
    });
  });
  
  // Update karakter count
  function updateCharCount() {
    const t = translations[currentAppLang];
    inputChars.textContent = `${inputText.value.length} ${t.characters}`;
    if (outputText.textContent !== t.resultPlaceholder) {
      outputChars.textContent = `${outputText.textContent.length} ${t.characters}`;
    }
  }
  
  inputText.addEventListener('input', updateCharCount);
  
  // Fungsi untuk melakukan translasi
  async function translateText(text, translateFrom, translateTo) {
    // Tampilkan loading dan sembunyikan pesan error/success
    loading.style.display = 'block';
    errorMessage.style.display = 'none';
    successMessage.style.display = 'none';
    
    try {
      // Encode text untuk URL
      const encodedText = encodeURIComponent(text);
      
      // Buat URL API
      const apiUrl = `https://api.mymemory.translated.net/get?q=${encodedText}&langpair=${translateFrom}|${translateTo}`;
      
      // Lakukan request ke API
      const response = await fetch(apiUrl);
      
      if (!response.ok) {
        throw new Error('Terjadi kesalahan saat mengambil data');
      }
      
      const data = await response.json();
      
      // Periksa jika response memiliki data terjemahan
      if (data && data.responseData && data.responseData.translatedText) {
        return data.responseData.translatedText;
      } else {
        throw new Error('Terjemahan tidak ditemukan dalam respons');
      }
    } catch (error) {
      console.error('Error:', error);
      errorMessage.textContent = `Error: ${error.message || 'Terjadi kesalahan saat menerjemahkan'}`;
      errorMessage.style.display = 'block';
      return null;
    } finally {
      loading.style.display = 'none';
    }
  }
  
  // Event listener untuk tombol terjemahkan
  translateBtn.addEventListener('click', async function() {
    const t = translations[currentAppLang];
    const text = inputText.value.trim();
    const translateFrom = sourceLang.value;
    const translateTo = targetLang.value;
    
    if (!text) {
      errorMessage.textContent = t.errors.emptyText;
      errorMessage.style.display = 'block';
      return;
    }
    
    if (translateFrom === translateTo) {
      errorMessage.textContent = t.errors.sameLang;
      errorMessage.style.display = 'block';
      return;
    }
    
    const translatedText = await translateText(text, translateFrom, translateTo);
    
    if (translatedText) {
      outputText.textContent = translatedText;
      updateCharCount();
      successMessage.textContent = t.success.translated;
      successMessage.style.display = 'block';
    }
  });
  
  // Event listener untuk tombol tukar bahasa
  swapBtn.addEventListener('click', function() {
    const temp = sourceLang.value;
    const tempTarget = targetLang.value;
    
    // Don't swap if source is auto detect
    if (temp === 'auto') {
      return;
    }
    
    sourceLang.value = tempTarget;
    targetLang.value = temp;
    
    // Tukar juga teks jika ada
    const t = translations[currentAppLang];
    if (outputText.textContent !== t.resultPlaceholder) {
      const tempText = inputText.value;
      inputText.value = outputText.textContent;
      outputText.textContent = tempText;
      updateCharCount();
    }
  });
  
  // Event listener untuk tombol salin
  copyBtn.addEventListener('click', function() {
    const t = translations[currentAppLang];
    const textToCopy = outputText.textContent;
    
    if (textToCopy && textToCopy !== t.resultPlaceholder) {
      navigator.clipboard.writeText(textToCopy)
        .then(() => {
          successMessage.textContent = t.success.copied;
          successMessage.style.display = 'block';
          
          // Sembunyikan pesan sukses setelah 2 detik
          setTimeout(() => {
            successMessage.style.display = 'none';
          }, 2000);
        })
        .catch(err => {
          console.error('Gagal menyalin teks: ', err);
          errorMessage.textContent = t.errors.copyFailed;
          errorMessage.style.display = 'block';
        });
    }
  });
  
  // Event listener untuk tombol bersihkan
  clearBtn.addEventListener('click', function() {
    const t = translations[currentAppLang];
    inputText.value = '';
    outputText.textContent = t.resultPlaceholder;
    errorMessage.style.display = 'none';
    successMessage.style.display = 'none';
    updateCharCount();
  });
  
  // Event listener untuk tombol dengarkan (text-to-speech)
  readAloudBtn.addEventListener('click', function() {
    const t = translations[currentAppLang];
    const textToRead = outputText.textContent;
    
    if (textToRead && textToRead !== t.resultPlaceholder) {
      // Cek dukungan browser untuk Web Speech API
      if ('speechSynthesis' in window) {
        const utterance = new SpeechSynthesisUtterance(textToRead);
        
        // Set bahasa sesuai dengan bahasa target
        utterance.lang = targetLang.value;
        
        // Mulai berbicara
        speechSynthesis.speak(utterance);
      } else {
        errorMessage.textContent = t.errors.speechNotSupported;
        errorMessage.style.display = 'block';
      }
    }
  });
  
  // Initialize app with default language
  updateAppLanguage(currentAppLang);
});
