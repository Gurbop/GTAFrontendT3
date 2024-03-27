---
title: Clash Royale Simulator Frontend
layout: post
permalink: /clash
description: Clash simulation
type: tangibles
courses: { compsci: { week: 28 } }
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prediction Form</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        form {
            margin-bottom: 20px;
        }
        label, input, button {
            margin: 5px 0;
        }
    </style>
</head>
<body>
    <h2>Make Your Prediction</h2>
    <form id="predictionForm">
        <label for="my_trophies">My Trophies:</label>
        <input type="number" id="my_trophies" name="my_trophies" required><br><br>
        <label for="opponent_trophies">Opponent's Trophies:</label>
        <input type="number" id="opponent_trophies" name="opponent_trophies" required><br><br>
        <label for="my_deck_elixir">My Deck Elixir:</label>
        <input type="number" step="0.1" id="my_deck_elixir" name="my_deck_elixir" required><br><br>
        <label for="op_deck_elixir">Opponent's Deck Elixir:</label>
        <input type="number" step="0.1" id="op_deck_elixir" name="op_deck_elixir" required><br><br>
        <button type="submit">Predict</button>
    </form>
    <div id="predictionResult"></div>
    <script>
        document.getElementById('predictionForm').addEventListener('submit', function(e) {
            e.preventDefault(); // Prevent the default form submission 
            // Getting the values from the form
            const myTrophies = document.getElementById('my_trophies').value;
            const opponentTrophies = document.getElementById('opponent_trophies').value;
            const myDeckElixir = document.getElementById('my_deck_elixir').value;
            const opDeckElixir = document.getElementById('op_deck_elixir').value;
            // Preparing the data to be sent to the server
            const data = {
                my_trophies: myTrophies,
                opponent_trophies: opponentTrophies,
                my_deck_elixir: myDeckElixir,
                op_deck_elixir: opDeckElixir
            };
            // Adjust the URL to your actual backend endpoint
        fetch('http://127.0.0.1:8086/api/users/ML', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(data),
        })
        .then(response => {
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            return response.json();
        })
        .then(data => {
            console.log('Success:', data);
            document.getElementById('predictionResult').innerHTML = `Prediction Result: ${data}`;
        })
        .catch((error) => {
            console.error('Error:', error);
            });
        });
    </script>
</body>
</html>