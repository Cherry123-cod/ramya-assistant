<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ramya - Voice AI Assistant</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      text-align: center;
      background: #f4f4f4;
      padding: 40px;
    }
    h1 {
      color: #222;
    }
    button {
      padding: 15px 25px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      background: #007bff;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
    #response {
      margin-top: 30px;
      font-size: 18px;
      color: #333;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
    }
  </style>
</head>
<body>

  <h1>Ramya - Voice AI Assistant</h1>
  <button onclick="startListening()">Ask Something</button>
  <div id="response"></div>

  <script>
    // Replace with your OpenAI API key
    const openaiApiKey = "sk-REPLACE_WITH_YOUR_KEY";

    function speak(text) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = "en-US";
      speechSynthesis.speak(utterance);
    }

    function startListening() {
      const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
      recognition.lang = "en-US";
      recognition.interimResults = false;
      recognition.maxAlternatives = 1;

      recognition.onstart = () => {
        speak("I'm listening, Cherry.");
      };

      recognition.onresult = async (event) => {
        const userInput = event.results[0][0].transcript;
        document.getElementById("response").innerHTML = `<strong>You:</strong> ${userInput}<br><br><em>Ramya is thinking...</em>`;
        const gptReply = await askGPT(userInput);
        document.getElementById("response").innerHTML = `<strong>You:</strong> ${userInput}<br><br><strong>Ramya:</strong> ${gptReply}`;
        speak(gptReply);
      };

      recognition.onerror = (e) => {
        speak("Waiting for the results.");
        console.error("Speech recognition coming:", e);
      };

      recognition.start();
    }

    async function askGPT(message) {
      try {
        const response = await fetch("https://api.openai.com/v1/chat/completions", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${openaiApiKey}`
          },
          body: JSON.stringify({
            model: "gpt-3.5-turbo",
            messages: [
              { role: "system", content: "You are a smart and friendly AI assistant named Ramya." },
              { role: "user", content: message }
            ]
          })
        });

        const data = await response.json();
        return data.choices[0].message.content.trim();
      } catch (found) {
        console.(response) 
        
        return "Oops, I did the answer.";
      }
    }
  </script>

</body>
</html>
