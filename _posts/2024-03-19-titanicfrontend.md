---
title: Titanic Simulator Frontend
layout: post
permalink: /titanic
description: Titanic simulation
type: tangibles
courses: { compsci: { week: 26 } }
---

<style>
    body {
    color: #ffffff; 
    font-family: Arial, sans-serif;
    padding: 20px;
    background-color: #0a2a4d; /* Dark blue */
    }

    form {
        max-width: 400px;
        margin: 0 auto;
    }

    label {
        display: block;
        margin-bottom: 5px;
        color: #ffffff; /* White */
    }
    
    input[type="text"],
    input[type="number"],
    select {
        width: 100%;
        padding: 8px;
        margin-bottom: 10px;
        background-color: #081c36; /* Darker blue */
        border: none;
        border-radius: 4px;
        color: #ffffff; /* White */
    }

    input[type="checkbox"] {
        margin-bottom: 10px;
    }

    button {
        padding: 10px 20px;
        background-color: #74c0fc; /* Light blue */
        border: none;
        border-radius: 4px;
        color: #1a1a1a; /* Dark grey */
        cursor: pointer;
    }

    button:hover {
        background-color: #5aa6e5; /* Lighter blue on hover */
    }

    #result {
        margin-top: 20px;
        padding: 20px;
        background-color: #081c36; /* Darker blue */
        border-radius: 12px;
    }

    #result h2 {
        color: #74c0fc; /* Light blue */
        margin-bottom: 10px;
    }

    #result p {
        margin-bottom: 5px;
        color: #ffffff; /* White */
    }
</style>
<body>
    <h1>Titanic Survival Predictor</h1>
    <form id="titanicForm">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required><br><br>
        <label for="pclass">Passenger Class:</label>
        <select id="pclass" name="pclass" required>
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select><br><br>
        <label for="sex">Sex:</label>
        <select id="sex" name="sex" required>
            <option value="male">Male</option>
            <option value="female">Female</option>
        </select><br><br>
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" required><br><br>
        <label for="sibsp">Siblings/Spouses Aboard:</label>
        <input type="number" id="sibsp" name="sibsp" required><br><br>
        <label for="parch">Parents/Children Aboard:</label>
        <input type="number" id="parch" name="parch" required><br><br>
        <label for="fare">Fare:</label>
        <input type="number" id="fare" name="fare" required><br><br>
        <label for="embarked">Embarked:</label>
        <select id="embarked" name="embarked" required>
            <option value="C">Cherbourg</option>
            <option value="Q">Queenstown</option>
            <option value="S">Southampton</option>
        </select><br><br>
        <label for="alone">Alone:</label>
        <input type="checkbox" id="alone" name="alone"><br><br>
        <button type="button" onclick="predictSurvival()">Predict Survival</button>
    </form>
    <div id="result"></div>
    <script>
        function predictSurvival() {
            var form = document.getElementById('titanicForm');
            var formData = new FormData(form);
            fetch('http://127.0.0.1:8086/api/titanic/predict', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                },
                body: JSON.stringify(Object.fromEntries(formData))
            })
            .then(response => response.json())
            .then(data => {
                var resultDiv = document.getElementById('result');
                resultDiv.innerHTML = '<h2>Prediction Result</h2>';
                for (var key in data) {
                    resultDiv.innerHTML += '<p>' + key + ': ' + data[key] + '</p>';
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    </script>
</body>