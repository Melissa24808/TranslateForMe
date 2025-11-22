TranslateForMe 🌍
A simple translation app that lets you translate text between different languages, with text-to-speech and translation history.
README

DEMONSTRATION VIDEO:



What does it do?

Translates text between English, French, Spanish, German, and Kinyarwanda
Uses MyMemory Translation API (free tier)
Has a fallback dictionary for when the API fails
Speaks translations out loud (text-to-speech)
Saves your recent translations in browser storage
Copy translations to clipboard

How to use it?

Type or paste text in the "Source Text" box
Pick your source language (where you're translating FROM)
Pick your target language (where you're translating TO)
Click "Translate" button
Use the copy button to copy result, or speaker button to hear it

Features

Swap button: Quickly switch between source and target languages
Recent translations: See your last 5 translations at the bottom
Offline fallback: A few basic words work even without internet
Enter key: Press Enter in the text box to translate quickly

Tech stuff

Pure HTML, CSS (Tailwind), and JavaScript
No backend needed, runs entirely in browser
Uses localStorage to save history
Uses MyMemory Translation API


API DOCUMENTATION
MyMemory Translation API
Endpoint: https://api.mymemory.translated.net/get
Method: GET
Parameters:

q - The text you want to translate (URL encoded)
langpair - Language pair in format source|target (e.g., en|fr)

Example Request:
https://api.mymemory.translated.net/get?q=hello&langpair=en|fr
Example Response:
json{
  "responseData": {
    "translatedText": "bonjour",
    "match": 1
  },
  "quotaFinished": false,
  "responseStatus": 200
}
What we use:

We grab responseData.translatedText for the translation
If the API fails or returns the same text, we fall back to our local dictionary

Rate Limits:

Free tier: 1000 requests per day
No API key needed for basic usage


Fallback Dictionary Structure
When the API doesn't work, we use a simple JavaScript object:
javascriptconst FALLBACK_DICTIONARY = {
  en: {
    fr: { hello: "bonjour", love: "amour", peace: "paix" },
    rw: { hello: "muraho", love: "urukundo", peace: "amahoro" }
  },
  rw: {
    en: { muraho: "hello", urukundo: "love", amahoro: "peace" },
    fr: { muraho: "bonjour" }
  }
};
Format: FALLBACK_DICTIONARY[sourceLang][targetLang][word]
How to add more words:
Just add them to the dictionary object like:
javascripten: {
  fr: { 
    hello: "bonjour", 
    goodbye: "au revoir",  // add this
    thanks: "merci"        // and this
  }
}

Browser APIs Used
1. localStorage
Stores recent translations on your device
Save translation:
javascriptlocalStorage.setItem('translationExplorerRecent', JSON.stringify(data));
Load translations:
javascriptconst recents = JSON.parse(localStorage.getItem('translationExplorerRecent') || '[]');
2. Web Speech API
Makes the browser read text out loud
Usage:
javascriptconst utterance = new SpeechSynthesisUtterance(text);
utterance.lang = 'fr';  // language code
window.speechSynthesis.speak(utterance);
3. Clipboard API (via execCommand)
Copies text to clipboard
Usage:
javascriptconst textarea = document.createElement('textarea');
textarea.value = text;
document.body.appendChild(textarea);
textarea.select();
document.execCommand('copy');
document.body.removeChild(textarea);

Language Codes
LanguageCodeEnglishenFrenchfrSpanishesGermandeKinyarwandarw

File Structure
translator.html
├── HTML structure (input/output cards)
├── CSS styling (Tailwind + custom)
└── JavaScript
    ├── API calls (MyMemory)
    ├── Fallback dictionary
    ├── localStorage handling
    ├── Text-to-speech
    └── UI event handlers

Common Issues
Translation not working?

Check internet connection (API needs it)
Try the fallback words: hello, love, peace
Make sure you selected different languages

Text-to-speech not working?

Some browsers don't support it
Check if your device volume is on
Try a different browser (Chrome works best)

Recent translations disappeared?

You might have cleared browser data
localStorage gets wiped when you clear cache


Want to customize?
Add more languages:
Add them to the LANGUAGES array:
javascriptconst LANGUAGES = [
  { code: "en", name: "English" },
  { code: "sw", name: "Swahili" },  // add new language
];
Change colors:
Edit the Tailwind config:
javascripttailwind.config = {
  theme: {
    extend: {
      colors: {
        primary: "#your-color-here",
      },
    },
  },
};
Add more fallback words:
Just edit the FALLBACK_DICTIONARY object

That's it! Keep it simple, keep translating! 🚀
