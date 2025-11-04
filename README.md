<!DOCTYPE html>
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
  <input type="text" id="prompt" placeholder="Bijv. futuristische stad bij zonsondergang" />
  <select id="style">
    <option value="">Stijl (optioneel)</option>
    <option value="ghibli">Ghibli</option>
    <option value="realistisch">Realistisch</option>
    <option value="pixelart">Pixelart</option>
    <option value="magisch">Magisch</option>
  </select>
  <select id="ratio">
    <option value="">Ratio (optioneel)</option>
    <option value="square">1:1 vierkant</option>
    <option value="portrait">3:4 portret</option>
    <option value="landscape">4:3 landschap</option>
  </select>
  <button onclick="generateImage()">Genereer</button>

  <div id="status">🕓 Klaar om te genereren</div>
  <img id="result" alt="Afbeelding verschijnt hier..." />

  <script>
    async function generateImage() {
      const prompt = document.getElementById("prompt").value;
      const style = document.getElementById("style").value;
      const ratio = document.getElementById("ratio").value;
      const status = document.getElementById("status");
      const result = document.getElementById("result");

      status.textContent = "🧠 Bezig met genereren...";
      result.src = "";

      try {
        const response = await fetch("https://api.kie.ai/v1/image", {
          method: "POST",
          headers: {
            "Authorization": "Bearer 5ee3a1a682907748d8b2d380846350fd",
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            prompt: prompt,
            style: style,
            ratio: ratio,
            model: "gpt-image-1"
          })
        });

        const data = await response.json();
        result.src = data.image_url;
        status.textContent = "✅ Afbeelding klaar!";
      } catch (error) {
        status.textContent = "❌ Fout bij genereren. Controleer je API-key of endpoint.";
        console.error(error);
      }
    }
  </script>
</body>
</html>
