# Lucy-IA<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lucy - AI</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: black;
            color: white;
            position: relative;
        }
        #background-text {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            color: rgba(255, 255, 255, 0.2);
            z-index: -1;
        }
        #message {
            position: absolute;
            top: 10%;
            font-size: 1.5rem;
            color: white;
        }
        #chat-container {
            width: 90%;
            max-width: 400px;
            background: #001f3f;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 255, 0.5);
            border: 5px solid white;
        }
        #chat-box {
            height: 300px;
            overflow-y: auto;
            border-bottom: 1px solid #ddd;
            padding-bottom: 10px;
            margin-bottom: 10px;
        }
        input {
            width: calc(100% - 20px);
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        button {
            margin-top: 10px;
            padding: 10px;
            width: 100%;
            border: none;
            background: #007bff;
            color: white;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="message">Solo busca lo que deseas</div>
    <div id="background-text">Lucy</div>
    <div id="chat-container">
        <div id="chat-box"></div>
        <input type="text" id="user-input" placeholder="¿Qué quieres saber hoy?">
        <button onclick="sendMessage()">Enviar</button>
    </div>

    <script>
        function sendMessage() {
            let inputField = document.getElementById("user-input");
            let userText = inputField.value;
            if (userText.trim() === "") return;
            
            let chatBox = document.getElementById("chat-box");
            chatBox.innerHTML += `<div><strong>Tú:</strong> ${userText}</div>`;
            
            fetchWikipediaInfo(userText);
            fetchGoogleScholarLink(userText);
            inputField.value = "";
        }

        function fetchWikipediaInfo(query) {
            let url = `https://es.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(query)}`;
            
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    let summary = data.extract || "Lo siento, no encontré información sobre eso.";
                    displayResponse(summary);
                })
                .catch(error => {
                    displayResponse("Lo siento, hubo un problema al obtener la información.");
                });
        }

        function fetchGoogleScholarLink(query) {
            let scholarUrl = `https://scholar.google.com/scholar?q=${encodeURIComponent(query)}`;
            displayResponse(`Puedes ver artículos académicos aquí: <a href="${scholarUrl}" target="_blank">Google Académico</a>`);
        }

        function displayResponse(response) {
            let chatBox = document.getElementById("chat-box");
            chatBox.innerHTML += `<div><strong>Lucy:</strong> ${response}</div>`;
            chatBox.scrollTop = chatBox.scrollHeight;
        }
    </script>
</body>
</html>
