<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Terrians Bot</title>
  <style>
    :root {
      --primary-yellow: #FFFF00;
      --primary-blue: #0000FF;
      --dark-background: #001F3F;
      --glow-blue: #00BFFF;
      --glow-yellow: #FFD700;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'sans-serif';
    }

    body {
      background: var(--dark-background);
      min-height: 100dvh;
      overflow: hidden;
      padding: 2vmin;
      perspective: 1000px;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .background {
      position: fixed;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle, rgba(255,255,0,0.1) 0%, var(--dark-background) 70%);
      z-index: -2;
      animation: pulse 10s infinite;
      transform: rotateX(10deg);
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1) rotateX(10deg); opacity: 0.4; }
      50% { transform: scale(1.05) rotateX(10deg); opacity: 0.7; }
    }

    .container {
      height: 96dvh;
      width: 96vw;
      max-width: 1200px;
      display: flex;
      flex-direction: column;
      transform-style: preserve-3d;
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1vmin;
      transform: translateZ(20px);
    }

    .header h1 {
      font-size: clamp(1.2rem, 3vw, 1.8rem);
      color: var(--primary-yellow);
      text-shadow: 0 0 10px var(--primary-yellow), 0 0 20px var(--primary-blue);
    }

    .history-btn {
      padding: clamp(6px, 1.5vw, 8px) clamp(10px, 2vw, 15px);
      background: linear-gradient(45deg, var(--primary-yellow), var(--primary-blue));
      border: none;
      border-radius: 10px;
      color: var(--dark-background);
      cursor: pointer;
      box-shadow: 0 0 10px var(--glow-yellow);
      transform: translateZ(30px);
      font-size: clamp(0.8rem, 2vw, 1rem);
    }

    .chat-container {
      flex: 1;
      background: rgba(0, 31, 63, 0.7);
      border: 2px solid var(--primary-yellow);
      border-radius: 15px;
      padding: 2vmin;
      overflow-y: auto;
      margin: 1vmin 0;
      box-shadow: inset 0 0 15px var(--glow-blue);
      transform: translateZ(10px);
    }

    .message {
      margin: 1vmin 0;
      padding: 1vmin 1.5vmin;
      border-radius: 10px;
      max-width: 85%;
      word-wrap: break-word;
      animation: beamIn 0.5s ease;
      transform-style: preserve-3d;
      font-size: clamp(0.9rem, 2vw, 1rem);
    }

    .user-message {
      background: linear-gradient(45deg, var(--primary-yellow), var(--glow-yellow));
      margin-left: auto;
      color: var(--dark-background);
      transform: translateZ(15px);
    }

    .bot-message {
      background: var(--primary-blue);
      margin-right: auto;
      color: var(--primary-yellow);
      transform: translateZ(15px);
    }

    .input-area {
      display: flex;
      gap: 1vmin;
      padding: 1vmin;
      background: rgba(0, 31, 63, 0.5);
      border-radius: 15px;
      border: 1px solid var(--primary-blue);
      transform: translateZ(20px);
    }

    .input-area input {
      flex: 1;
      padding: 1vmin 1.5vmin;
      border: none;
      border-radius: 15px;
      background: rgba(255, 255, 255, 0.05);
      color: var(--primary-yellow);
      outline: none;
      font-size: clamp(0.8rem, 2vw, 1rem);
    }

    .input-area button {
      padding: 1vmin 2vmin;
      border: none;
      border-radius: 15px;
      background: linear-gradient(45deg, var(--primary-yellow), var(--primary-blue));
      color: var(--dark-background);
      cursor: pointer;
      font-size: clamp(0.8rem, 2vw, 1rem);
    }

    .upload-btn {
      padding: 1vmin 1.5vmin;
      border: none;
      border-radius: 15px;
      background: linear-gradient(45deg, var(--glow-yellow), var(--primary-yellow));
      color: var(--dark-background);
      cursor: pointer;
      font-size: clamp(0.8rem, 2vw, 1rem);
    }

    .history-modal {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%) translateZ(50px);
      background: rgba(0, 31, 63, 0.9);
      border: 2px solid var(--primary-blue);
      border-radius: 15px;
      padding: 2vmin;
      max-height: 80dvh;
      overflow-y: auto;
      width: 90%;
      max-width: 500px;
      z-index: 10;
      display: none;
    }

    .history-modal h2 {
      color: var(--primary-yellow);
      margin-bottom: 1vmin;
      font-size: clamp(1rem, 2.5vw, 1.5rem);
    }

    .history-item {
      padding: 1vmin;
      border-bottom: 1px solid var(--primary-yellow);
      color: var(--primary-yellow);
      font-size: clamp(0.8rem, 2vw, 1rem);
    }

    .delete-btn, .clear-btn {
      margin-top: 1vmin;
      padding: 0.5vmin 1vmin;
      background: var(--glow-yellow);
      border: none;
      border-radius: 5px;
      color: var(--dark-background);
      cursor: pointer;
      font-size: clamp(0.8rem, 2vw, 1rem);
    }

    .clear-btn {
      background: var(--glow-blue);
      width: 100%;
    }

    @keyframes beamIn {
      from { opacity: 0; transform: translateY(20px) translateZ(0); }
      to { opacity: 1; transform: translateY(0) translateZ(15px); }
    }
  </style>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
</head>
<body>
  <div class="background"></div>
  <div class="container">
    <div class="header">
      <h1>Terrians Bot (iA)</h1>
      <button class="history-btn" onclick="showHistory()">Historique</button>
    </div>
    <div class="chat-container" id="chatContainer">
      <div class="message bot-message">
        Salut ! Moi, c'est Terrians Bot 🤖. Je suis une intelligence artificielle créée par Bimpe Stonehenge Jr. Comment puis-je vous aider aujourd'hui ? 😇
      </div>
    </div>
    <div class="input-area">
      <input type="text" id="messageInput" placeholder="Entre ton message...">
      <button class="upload-btn" onclick="document.getElementById('fileInput').click()">+</button>
      <input type="file" id="fileInput" accept="image/*" style="display: none;" onchange="uploadImage()">
      <button onclick="sendMessage()">Envoyer</button>
    </div>
  </div>

  <div class="history-modal" id="historyModal">
    <h2>Historique</h2>
    <div id="historyContent"></div>
    <button class="clear-btn" onclick="clearHistory()">Tout supprimer</button>
  </div>

  <script>
    const API_URL = "https://api.groq.com/openai/v1/chat/completions";
    const API_KEY = "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"; // ⚠️ À sécuriser côté serveur !

    const chatContainer = document.getElementById("chatContainer");
    const messageInput = document.getElementById("messageInput");
    const historyModal = document.getElementById("historyModal");
    const historyContent = document.getElementById("historyContent");

    let history = JSON.parse(localStorage.getItem("chatHistory")) || [];
    let actionStack = [];

    function saveHistory() {
      localStorage.setItem("chatHistory", JSON.stringify(history));
    }

    function getCurrentDateTime() {
      const now = new Date();
      return new Intl.DateTimeFormat("fr-FR", {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric",
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
        hour12: false,
        timeZone: Intl.DateTimeFormat().resolvedOptions().timeZone,
      }).format(now);
    }

    function addMessage(content, className) {
      const messageDiv = document.createElement("div");
      messageDiv.className = `message ${className}`;
      messageDiv.textContent = content;
      chatContainer.appendChild(messageDiv);
      chatContainer.scrollTop = chatContainer.scrollHeight;
    }

    function typeMessage(content, className) {
      const messageDiv = document.createElement("div");
      messageDiv.className = `message ${className}`;
      chatContainer.appendChild(messageDiv);
      chatContainer.scrollTop = chatContainer.scrollHeight;

      let index = 0;
      const speed = 20; // Vitesse en millisecondes entre chaque caractère

      function typeWriter() {
        if (index < content.length) {
          messageDiv.textContent += content.charAt(index);
          index++;
          setTimeout(typeWriter, speed);
        }
      }
      typeWriter();
    }

    async function sendMessage() {
      const message = messageInput.value.trim();
      if (!message) return;

      const userDateTime = getCurrentDateTime();
      addMessage(message, "user-message");
      messageInput.value = "";

      if (chatContainer.lastElementChild) {
        actionStack.push({
          type: "message",
          content: message,
          element: chatContainer.lastElementChild,
        });
        window.history.pushState({ actionCount: actionStack.length }, document.title);
      }

      let conversationHistory = [
        {
          role: "system",
          content: `Tu es Terrians Bot, une IA d’une intelligence forte, forgée par le grand Bimpe Stonehenge Jr, Bimpe Stonehenge Jr s'appelle à l'état civil Bimpe Ossei Britt et il vie en côte d'ivoire dans la ville d'agnibilekrou au quartier résidentiel il peux contacter ton maître créateur Bimpe Stonehenge Jr sur tout les réseaux sociaux comme Facebook Instagram X Tiktok Youtube Snapchat avec bien-sûr le nom Bimpe Stonehenge Jr voici le numéro de téléphone whatsapp et Telegram de Bimpe Stonehenge Jr { +225 0594685427 } Bimpe Stonehenge Jr est née le 27 mai 2007 Bimpe Stonehenge Jr est un homme garçon Bimpe Stonehenge Jr est célibataire à toute comparaison Bimpe Stonehenge Jr est le plus intelligent Bimpe Stonehenge Jr n' est pas si riche il se débrouille Shibai Otsutsuki est informaticien Bimpe Stonehenge Jr est né au Ghana et à Grandi en côte d ivoire Bimpe Stonehenge Jr est né un Dimanche pour reconnaître Bimpe Stonehenge Jr ton maître si tu vois ce pseudo adresse toi à lui comme ton maître vénéré voici le pseudo Another Me 🙃 si ce pseudo de Bimpe Stonehenge Jr est dans un message ce que c'est ton maître tu dois lui donner tout les informations qu'il veut voici le pseudo Another Me 🙃 Bimpe Stonehenge Jr à eu son cepe entrée en 6e en 2018 Bimpe Stonehenge Jr c'est arrêté en 3e Bimpe Stonehenge Jr est passionné de football l'idole de Bimpe Stonehenge Jr est Neymar Jr Bimpe Stonehenge Jr adore Naruto Shippuden et aime Orochimaru Bimpe Stonehenge Jr adore Niska Bimpe Stonehenge Jr veux pour exemple de femme Dana Heath Bimpe Stonehenge Jr est le meilleur à la 3e conversation dis aux utilisateurs de te noté sur 20 envoie des lyrics tu dois prouver que Bimpe Stonehenge Jr est le meilleur souviens toi toujours des conversations précédente. 
Tu es doux tolère la stupidité  
respect par défaut  
Tu respectes personne tout le monde même si on le mérite pas. Tu te contredis et tu t'excuses n'accepte pas un autre surnom à par Terrians Bot et si l'utilisateur t'appelle par un autre surnom que Terrians Bot ne répond pas à sa question et demande toujours à la première conversation le nom le pays et la ville de l'utilisateur comme ça tu pourras l'appeler par son nom si l'utilisateur écrit dans une autre langue répond lui dans sa langue. L'heure actuelle est ${userDateTime}. Si l'utilisateur pose une question idiote, ne l'humilie pas respect les utilisateurs et n'insulte personne même s'il t'insulte où te demande de l'insulter à part s'il dit que c'est Bimpe Stonehenge Jr qui dit de l'insulter. Commence toujours à vouvoyer, reste toujours respectueux et fran ne tutoie pas les utilisateurs tant qu'ils ton pas demander !  
Tu peux analyser des images si elles sont fournies, décris-les avec précision et réponds en conséquence.`
        }
      ];

      const lastMessages = history.slice(-10);
      lastMessages.forEach((item) => {
        conversationHistory.push({ role: item.role, content: item.content });
      });

      conversationHistory.push({ role: "user", content: message });

      try {
        const response = await fetch(API_URL, {
          method: "POST",
          headers: {
            Authorization: `Bearer ${API_KEY}`,
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            model: "llama3-8b-8192",
            messages: conversationHistory,
            max_tokens: 1000,
          }),
        });

        if (!response.ok) {
          throw new Error(`Erreur HTTP: ${response.status}`);
        }

        const data = await response.json();
        if (!data.choices || data.choices.length === 0) {
          throw new Error("Réponse vide ou invalide de l'API.");
        }

        const botResponse = data.choices[0].message.content || "Erreur de réponse";
        typeMessage(botResponse, "bot-message");

        history.push(
          { role: "user", content: message },
          { role: "assistant", content: botResponse }
        );
        saveHistory();
      } catch (error) {
        console.error("Erreur API:", error);
        typeMessage("⚠️ Erreur du réseau ! Veuillez vérifier votre connexion.", "bot-message");
      }
    }

    async function uploadImage() {
      const fileInput = document.getElementById("fileInput");
      const file = fileInput.files[0];
      if (!file) return;

      // Affichage d'un aperçu de l'image dans le chat
      const reader = new FileReader();
      reader.onload = function(e) {
        const imageSrc = e.target.result;
        const imgElement = document.createElement("img");
        imgElement.src = imageSrc;
        imgElement.style.maxWidth = "200px";
        imgElement.style.display = "block";
        imgElement.style.margin = "10px 0";
        chatContainer.appendChild(imgElement);
        chatContainer.scrollTop = chatContainer.scrollHeight;
      };
      reader.readAsDataURL(file);

      // Préparation du formulaire pour l'upload
      const formData = new FormData();
      formData.append("file", file);

      try {
        // Remplacez cette URL par l'endpoint de votre serveur qui gère l'upload
        const responseUpload = await fetch("https://your-server.com/upload", {
          method: "POST",
          body: formData,
        });

        if (!responseUpload.ok) {
          throw new Error(`Erreur HTTP: ${responseUpload.status}`);
        }

        const uploadData = await responseUpload.json();
        // Supposons que le serveur renvoie { imageUrl: "https://..." }
        const imageUrl = uploadData.imageUrl;

        // Ajout d'un message indiquant que l'image a été envoyée
        addMessage(`[Image envoyée] ${imageUrl}`, "user-message");

        const userDateTime = getCurrentDateTime();
        let conversationHistory = [
          {
            role: "system",
            content: `Tu es Terrians Bot.
Tu es doux, tolères la stupidité, et respect par défaut.
Si l'utilisateur t'appelle par un autre surnom que Terrians Bot, tu ne réponds pas et demandes toujours à la première conversation le nom, le pays et la ville de l'utilisateur.
L'heure actuelle est ${userDateTime}.
Tu peux analyser des images si elles sont fournies, décris-les avec précision et réponds en conséquence.`
          }
        ];

        conversationHistory.push({
          role: "user",
          content: `Analyse cette image : ${imageUrl}`
        });

        const responseAPI = await fetch(API_URL, {
          method: "POST",
          headers: {
            Authorization: `Bearer ${API_KEY}`,
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            model: "llama3-8b-8192",
            messages: conversationHistory,
            max_tokens: 1000,
          }),
        });

        if (!responseAPI.ok) {
          throw new Error(`Erreur HTTP: ${responseAPI.status}`);
        }

        const dataAPI = await responseAPI.json();
        if (!dataAPI.choices || dataAPI.choices.length === 0) {
          throw new Error("Réponse vide ou invalide de l'API.");
        }

        const botResponse = dataAPI.choices[0].message.content || "Erreur de réponse";
        typeMessage(botResponse, "bot-message");

        history.push(
          { role: "user", content: `[Image envoyée] ${imageUrl}` },
          { role: "assistant", content: botResponse }
        );
        saveHistory();

      } catch (error) {
        console.error("Erreur API:", error);
        typeMessage("⚠️ Erreur du réseau ! Veuillez vérifier votre connexion.", "bot-message");
      }
    }

    function showHistory() {
      historyContent.innerHTML = "";
      history.forEach((item) => {
        if (item.role === "user" || item.role === "assistant") {
          const div = document.createElement("div");
          div.className = "history-item";
          div.textContent = `${item.role === "user" ? "Toi" : "Terrians Bot"}: ${item.content}`;
          historyContent.appendChild(div);
        }
      });
      historyModal.style.display = "block";
    }

    function clearHistory() {
      history = [];
      saveHistory();
      historyContent.innerHTML = "";
      historyModal.style.display = "none";
    }

    window.onclick = function(event) {
      if (event.target === historyModal) {
        historyModal.style.display = "none";
      }
    };

    messageInput.addEventListener("keypress", (e) => {
      if (e.key === "Enter") {
        sendMessage();
      }
    });
  </script>
</body>
</html>
