<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Top Donateurs</title>

<style>
body {
  margin: 0;
  background: transparent;
  font-family: Arial, sans-serif;
}

#overlay {
  width: 300px;
  padding: 15px;
  background: rgba(0,0,0,0.6);
  border-radius: 15px;
  color: white;
}

h2 {
  text-align: center;
  margin-bottom: 10px;
}

.user {
  display: flex;
  justify-content: space-between;
  margin: 5px 0;
  font-size: 16px;
}

button {
  margin-top: 10px;
  width: 100%;
  padding: 8px;
  border: none;
  border-radius: 10px;
  background: red;
  color: white;
  font-weight: bold;
  cursor: pointer;
}
</style>
</head>

<body>

<div id="overlay">
  <h2>🏆 Top Donateurs</h2>
  <div id="list"></div>
  <button onclick="resetScores()">RESET</button>
</div>

<script>
// =======================
// CONFIG
// =======================
const WS_URL = "ws://localhost:21213"; // TikFinity WebSocket

// =======================
// DATA
// =======================
let scores = {};

// =======================
// LOAD SAVE
// =======================
if(localStorage.getItem("scores")){
  scores = JSON.parse(localStorage.getItem("scores"));
}

// =======================
// DISPLAY
// =======================
function updateDisplay() {
  const list = document.getElementById("list");

  const sorted = Object.entries(scores)
    .sort((a,b) => b[1] - a[1])
    .slice(0,5);

  list.innerHTML = "";

  sorted.forEach(([name, score], index) => {
    const div = document.createElement("div");
    div.className = "user";
    div.innerHTML = `<span>#${index+1} ${name}</span><span>${score}</span>`;
    list.appendChild(div);
  });

  localStorage.setItem("scores", JSON.stringify(scores));
}

// =======================
// RESET
// =======================
function resetScores(){
  scores = {};
  updateDisplay();
}

// =======================
// WEBSOCKET (TikFinity)
// =======================
const socket = new WebSocket(WS_URL);

socket.onopen = () => {
  console.log("Connecté à TikFinity");
};

socket.onmessage = (event) => {
  const data = JSON.parse(event.data);

  // Event cadeau
  if(data.event === "gift") {

    const username = data.username;
    const giftValue = data.diamondCount || 1; // valeur TikTok

    if(!scores[username]){
      scores[username] = 0;
    }

    scores[username] += giftValue;

    updateDisplay();
  }
};

socket.onerror = (err) => {
  console.log("Erreur WS :", err);
};

socket.onclose = () => {
  console.log("Déconnecté");
};

// INIT
updateDisplay();
</script>

</body>
</html>
