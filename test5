<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Top Donateurs MVP</title>

<style>
body {
    margin: 0;
    background: transparent;
    font-family: Arial;
}

#box {
    position: absolute;
    top: 50px;
    right: 50px;
    width: 300px;
    background: rgba(0,0,0,0.6);
    border: 2px solid purple;
    border-radius: 15px;
    padding: 15px;
    color: white;
}

h2 {
    text-align: center;
    color: violet;
}

.user {
    display: flex;
    justify-content: space-between;
    margin: 5px 0;
}

button {
    margin-top: 10px;
    width: 100%;
    background: purple;
    color: white;
    border: none;
    padding: 5px;
    border-radius: 10px;
    cursor: pointer;
}
</style>
</head>

<body>

<div id="box">
    <h2>🏆 TOP 5</h2>
    <div id="list"></div>
    <button onclick="resetData()">RESET</button>
</div>

<script>
let socket = new WebSocket("ws://localhost:21213/");
let donors = {};

socket.onmessage = (event) => {
    let data = JSON.parse(event.data);

    if (data.event === "gift") {
        let user = data.username;
        let amount = data.diamondCount || 1;

        if (!donors[user]) donors[user] = 0;
        donors[user] += amount;

        updateUI();
    }
};

function updateUI() {
    let sorted = Object.entries(donors)
        .sort((a, b) => b[1] - a[1])
        .slice(0, 5);

    let html = "";
    sorted.forEach((d, i) => {
        html += `<div class="user">#${i+1} ${d[0]} <span>${d[1]}</span></div>`;
    });

    document.getElementById("list").innerHTML = html;
}

function resetData() {
    donors = {};
    updateUI();
}

// 🔥 RESET AUTO (toutes les 10 minutes)
setInterval(() => {
    donors = {};
    updateUI();
}, 10 * 60 * 1000);
</script>

</body>
</html>
