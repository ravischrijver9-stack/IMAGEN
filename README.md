<html lang="nl">
<head>
  <meta charset="UTF-8">
  <title>Afbeelding Generator</title>
  <style>
    body { font-family: sans-serif; padding: 2em; background: #f0f0f0; }
    input, select, button { margin: 0.5em 0; padding: 0.5em; width: 100%; }
    img { margin-top: 1em; max-width: 100%; border: 1px solid #ccc; }
    #status { margin-top: 1em; font-weight: bold; color: #444; }
  </style>
</head>
<body>
  <h2>🖼️ Genereer een afbeelding</h2>
  <input type="text" id="prompt" placeholder="Bijv. een scouting teken met daaronder de naam Doemaarzat" />
  <select id="style">
    <option value="">Stijl (optioneel)</option>
    <option value="realistisch">Realistisch</option>
    <option value="ghibli">Ghibli</option>
    <option value="pixelart">Pixelart</option>
    <option value="magisch">Magisch</option>
  </select>
  <select id="ratio">
    <option value="">Ratio (optioneel)</option>
    <option value="vierkant">Vierkant (1024x1024)</option>
    <option value="landschap">Landschap (1792x1024)</option>
    <option value="portret">Portret (1024x1792)</option>
  </select>
  <button onclick="generateImage()">Genereer</button>

  <div id="status">🕓 Klaar om te genereren</div>
  <img id="result" alt="Afbeelding verschijnt hier..." />

  <script>
    async function generateImage() {
      const promptInput = document.getElementById("prompt").value;
      const style = document.getElementById("style").value;
      const ratio = document.getElementById("ratio").value;
      const status = document.getElementById("status");
      const result = document.getElementById("result");

      // Bouw volledige prompt
      let fullPrompt = promptInput;
      if (style) fullPrompt += ` in ${style} stijl`;
      if (ratio) fullPrompt += ` met ${ratio} verhouding`;

      // Bepaal afbeeldingsgrootte
      let size = "1024x1024";
      if (ratio === "landschap") size = "1792x1024";
      if (ratio === "portret") size = "1024x1792";

      status.textContent = "🧠 Bezig met genereren...";
      result.src = "";

      try {
        const response = await fetch("https://api.openai.com/v1/images/generations", {
          method: "POST",
          headers: {
            "Authorization": "Bearer sk-proj-5ee3a1a682907748d8b2d380846350fd",
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            model: "dall-e-3",
            prompt: fullPrompt,
            n: 1,
            size: size
          })
        });

        const data = await response.json();
        result.src = data.data[0].url;
        status.textContent = "✅ Afbeelding klaar!";
      } catch (error) {
        status.textContent = "❌ Fout bij genereren. Controleer je API-key of prompt.";
        console.error(error);
      }
    }
  </script>
</body>
</html>
