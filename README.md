# index.html
<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text to Speech (Downloadable)</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
        }
        textarea {
            width: 80%;
            height: 100px;
        }
        button {
            margin: 10px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h2>टेक्स्ट से वॉइस जनरेशन (डाउनलोडेबल)</h2>
    <textarea id="textInput" placeholder="यहां टेक्स्ट लिखें..."></textarea><br>
    <button onclick="speakAndRecord()">🔊 बोलें और रिकॉर्ड करें</button>
    <a id="downloadLink" style="display:none;" download="voice.mp3">⬇️ डाउनलोड करें</a>

    <script>
        function speakAndRecord() {
            let text = document.getElementById("textInput").value;
            let speech = new SpeechSynthesisUtterance(text);
            speech.lang = "hi-IN";

            let audioContext = new AudioContext();
            let destination = audioContext.createMediaStreamDestination();
            let mediaRecorder = new MediaRecorder(destination.stream);
            let audioChunks = [];

            mediaRecorder.ondataavailable = event => {
                audioChunks.push(event.data);
            };

            mediaRecorder.onstop = () => {
                let audioBlob = new Blob(audioChunks, { type: "audio/mp3" });
                let audioUrl = URL.createObjectURL(audioBlob);
                let downloadLink = document.getElementById("downloadLink");
                downloadLink.href = audioUrl;
                downloadLink.style.display = "block";
            };

            speech.onstart = () => {
                mediaRecorder.start();
            };

            speech.onend = () => {
                mediaRecorder.stop();
            };

            let utteranceSource = audioContext.createMediaStreamSource(destination.stream);
            utteranceSource.connect(destination);
            window.speechSynthesis.speak(speech);
        }
    </script>
</body>
</html>
