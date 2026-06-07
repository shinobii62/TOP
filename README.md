<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Top Donateurs</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: transparent;
}

#overlay {
    width: 300px;
    background: rgba(0,0,0,0.7);
    border-radius: 15px;
    padding: 15px;
    color: white;
}

h2 {
    text-align: center;
    margin-bottom: 10px;
}

.donator {
    display: flex;
    justify-content: space-between;
    padding: 5px;
    border-bottom: 1px solid rgba(255,255,255,0.2);
}

.rank {
    font-weight: bold;
    margin-right: 10px;
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

// STOCKAGE DONATEURS
let donors = {};

// AJOUT DON (1 pièce = 1 point)
function addDonation(name, coins) {
    if (!donors[name]) {
        donors[name] = 0;
    }
    donors[name] += coins;
    updateDisplay();
}

// AFFICHAGE TOP 5
function updateDisplay() {
    let sorted = Object.entries(donors)
        .sort((a, b) => b[1] - a[1])
        .slice(0, 5);

    let html = "";

    sorted.forEach((d, index) => {
        html += `
        <div class="donator">
            <span class="rank">#${index + 1} ${d[0]}</span>
            <span>${d[1]} pts</span>
        </div>`;
    });

    document.getElementById("list").innerHTML = html;
}

// RESET
function resetScores() {
    donors = {};
    updateDisplay();
}



</script>

</body>
</html>
