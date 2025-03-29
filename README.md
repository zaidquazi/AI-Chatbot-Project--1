# AI-Chatbot-Project--1
Educational Chatbot
-------------------HTML--------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Chatbot</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <div class="glow-text text"><h1><i class="fas fa-graduation-cap" style="font-size: 50px; color: #007BFF;"></i>
            Educational AI Chatbot</h1></div>
        <div class="chat-box">
            <select id="language">
                <option value="en">English</option>
                <option value="es">Spanish</option>
                <option value="ur">Urdu</option>
                <option value="hi">Hindi</option>
            </select>
            <input type="text" id="message" placeholder="Ask a question...">
            <button id="send-btn">&#x27A4;</button>
        </div>
        <div id="response-container">
            <p><strong>Response:</strong> <span id="response"></span></p>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>

-----------------------------CSS----------------------------------

/* style.css */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: linear-gradient(135deg, #74EBD5, #ACB6E5);
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Container Styling */
.container {
    background: #fff;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    border-radius: 12px;
    padding: 30px;
    width: 500px;
    text-align: center;
    transition: all 0.4s ease;
    position: relative;
    overflow: hidden;
}

/* Hover Effect with Glow and Scale */
.container:hover {
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.3);
    transform: scale(1.05);
}

/* Add Gradient Border Animation */
.container::before {
    content: "";
    position: absolute;
    top: -5px;
    left: -5px;
    right: -5px;
    bottom: -5px;
    z-index: -1;
    background: linear-gradient(135deg, #ff9a9e, #fad0c4, #fad0c4, #fbc2eb);
    background-size: 300% 300%;
    border-radius: 15px;
    animation: gradientAnimation 5s ease infinite;
    opacity: 0;
    transition: opacity 0.4s;
}

/* Gradient border visible on hover */
.container:hover::before {
    opacity: 1;
}

/* Gradient animation */
@keyframes gradientAnimation {
    0% {
        background-position: 0% 50%;
    }
    50% {
        background-position: 100% 50%;
    }
    100% {
        background-position: 0% 50%;
    }
}

/* Text Styling */
.glow-text {
    margin-bottom: 20px;
    font-size: 24px;
    color: #007BFF;
    transition: color 0.3s;
}

/* Text hover effect */
.glow-text:hover {
    color: #ff5733;
}

/* Chat Box Styling */
.chat-box {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}

/* Input and Select Styling */
select, input {
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 8px;
    transition: all 0.3s ease;
}

/* Hover effect for input and select */
select:hover, input:hover {
    border-color: #007BFF;
    box-shadow: 0 0 10px #007BFF;
}

/* Hover effect for select options */
select option:hover {
    background: #f0f0f0;
}

/* Placeholder hover effect */
input::placeholder {
    color: #aaa;
    transition: color 0.3s;
}
input:hover::placeholder {
    color: #555;
}

/* Button Styling */
#send-btn {
    background: #007BFF;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    font-size: 24px;
    transition: 0.3s;
}

/* Button hover effect */
#send-btn:hover {
    background: #0056b3;
    transform: scale(1.1);
}

/* Response container */
#response-container {
    margin-top: 20px;
    padding: 20px;
    background: linear-gradient(135deg, #f0f0f0, #d9e4f5);
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    transition: 0.3s;
}

/* Add hover effect for the response container */
#response-container:hover {
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.3);
    transform: translateY(-5px);
}


-------------------------Javascript -----------------------------------

// script.js

document.getElementById('send-btn').addEventListener('click', async () => {
    const language = document.getElementById('language').value;
    const message = document.getElementById('message').value.trim();
    const responseContainer = document.getElementById('response');

    if (!message) {
        alert('Please enter a message.');
        return;
    }

    try {
        const response = await fetch('http://localhost:5500/chat', {  
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ language, message })
        });
        

        if (!response.ok) {
            throw new Error('Network response was not ok');
        }

        const data = await response.json();
        responseContainer.textContent = data.text;

    } catch (error) {
        console.error('Error:', error);
        responseContainer.textContent = 'Error communicating with the server....';
    }
});

---------------------------Python----------------------

from flask import Flask, request, jsonify, send_file
from flask_cors import CORS
import os

app = Flask(__name__)
CORS(app)

# Mock chatbot responses
responses = {
    'en': {
        'hello': 'Hello! How can I assist you today?',
        'courses': 'We offer courses in Science, Math, and Computer Science.',
        'admission': 'Admissions are open from June to August.'
    },
    'es': {
        'hello': '¡Hola! ¿Cómo puedo ayudarte hoy?',
        'courses': 'Ofrecemos cursos de ciencia, matemáticas e informática.',
        'admission': 'Las admisiones están abiertas de junio a agosto.'
    },
    'ur': {
        'hello': 'سلام! میں آج آپ کی کیسے مدد کر سکتا ہوں؟',
        'courses': 'ہم سائنس، ریاضی اور کمپیوٹر سائنس میں کورسز پیش کرتے ہیں۔',
        'admission': 'داخلے جون سے اگست تک کھلے ہیں۔'
    },
    'hi': {
        'hello': 'नमस्ते! आज मैं आपकी कैसे मदद कर सकता हूँ?',
        'courses': 'हम विज्ञान, गणित और कंप्यूटर साइंस में पाठ्यक्रम प्रदान करते हैं।',
        'admission': 'प्रवेश जून से अगस्त तक खुले हैं।'
    },
}

@app.route('/chat', methods=['POST'])
def chat():
    data = request.json
    language = data.get('language', 'en')
    message = data.get('message', '').lower()

    # Validate input
    if not message:
        return jsonify({'error': 'No message provided'}), 400

    response = responses.get(language, {}).get(message, "I am still learning. Please ask something else.")
    
    return jsonify({
        'text': response,
        'audio': '/audio-response',
        'document': '/document-response'
    })

@app.route('/audio-response', methods=['GET'])
def audio_response():
    audio_file = os.path.join(os.getcwd(), 'sample-audio.mp3')
    if os.path.exists(audio_file):
        return send_file(audio_file, mimetype='audio/mpeg')
    else:
        return jsonify({'error': 'Audio file not found'}), 404

@app.route('/document-response', methods=['GET'])
def document_response():
    document_file = os.path.join(os.getcwd(), 'sample-document.pdf')
    if os.path.exists(document_file):
        return send_file(document_file, as_attachment=True)
    else:
        return jsonify({'error': 'Document file not found'}), 404

if __name__ == '__main__':
    app.run(debug=True, port=5500)  # Consistent with the frontend port
