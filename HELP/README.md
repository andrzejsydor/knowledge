<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project README</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            background-color: #f4f4f4;
            padding: 20px;
        }
        h1 {
            color: #333;
        }
        li {
            margin-bottom: 10px;
        }
        #message {
            margin-top: 20px;
            font-size: 1.2em;
            color: #0056b3;
        }
    </style>
</head>
<body>

    <h1>Project Name</h1>
    <h2>Description</h2>
    <p>This project is aimed at demonstrating a simple README file with HTML and JavaScript.</p>

    <h2>Features</h2>
    <ul>
        <li>Interactive button</li>
        <li>Dynamic text change</li>
        <li>Responsive design in basic terms</li>
    </ul>

    <h2>Usage</h2>
    <p>Click the button below to see the message change!</p>

    <button id="changeMessageButton">Click Me!</button>
    <div id="message"></div>

    <script>
        document.getElementById('changeMessageButton').onclick = function() {
            document.getElementById('message').innerHTML = "You clicked the button! 🎉";
        };
    </script>

    <h2>License</h2>
    <p>This project is licensed under the MIT License.</p>

</body>
</html>
