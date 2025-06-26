from flask import Flask, render_template, request, jsonify import numpy as np
import librosa
from keras.models import load_model
from sklearn.preprocessing import LabelEncoder import os
import joblib import logging
app = Flask( name ) logging.basicConfig(level=logging.DEBUG) emotion_model = load_model('models/emotion_model.h5') genre_model = joblib.load('models/genre_model.pkl')
emotion_labels = ['neutral', 'calm', 'happy', 'sad', 'angry', 'fearful', 'disgust', 'surprised'] genre_labels = ['blues', 'pop', 'rock', 'classical', 'jazz', 'metal', 'country', 'disco', 'hiphop', 'reggae']

emotion_label_encoder = LabelEncoder() emotion_label_encoder.fit(emotion_labels)

genre_label_encoder = LabelEncoder() genre_label_encoder.fit(genre_labels)

logging.debug(f"Genre labels: {genre_labels}")
logging.debug(f"Encoded genre labels: {genre_label_encoder.transform(genre_labels)}")


def extract_features(file_path):
 
try:
audio, sr = librosa.load(file_path, sr=22050, duration=2.5, offset=0.6) mfcc = librosa.feature.mfcc(y=audio, sr=sr, n_mfcc=40)
return np.mean(mfcc.T, axis=0) except Exception as e:
logging.error(f"Error extracting features from {file_path}: {e}") raise

@app.route('/') def index():
return render_template('index.html')


@app.route('/predict_emotion', methods=['POST']) def predict_emotion():
try:
file = request.files['audio']
file_path = os.path.join('static', file.filename) file.save(file_path)

features = extract_features(file_path) features = np.array([features])
prediction = emotion_model.predict(features)
predicted_emotion = emotion_label_encoder.inverse_transform([np.argmax(prediction)])


return jsonify({"emotion": predicted_emotion[0]})
 
logging.error(f"Error in emotion prediction: {e}") return jsonify({"error": str(e)}), 500

@app.route('/predict_genre', methods=['POST']) def predict_genre():
try:
file = request.files['audio']
file_path = os.path.join('static', file.filename) file.save(file_path)

features = extract_features(file_path) features = np.array([features])

logging.debug(f"Extracted features: {features}")


prediction = genre_model.predict(features) logging.debug(f"Raw prediction output: {prediction}") if isinstance(prediction[0], np.ndarray:
predicted_genre_index = np.argmax(prediction[0]) else:
predicted_genre_index = prediction[0]
predicted_genre = genre_label_encoder.inverse_transform([predicted_genre_index]) logging.debug(f"Predicted genre: {predicted_genre[0]}")

return jsonify({"genre": predicted_genre[0]})
 
logging.error(f"Error in genre prediction: {e}") return jsonify({"error": str(e)}), 500

if  name	== " main ": app.run(debug=True)

4.4. Step-4: Code in index.html:


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Audio Emotion and Genre Prediction</title>
<style>
body {
font-family: Arial, sans-serif; margin: 0;
padding: 0;
background-color: #f4f4f9; color: #333;
}


h1 {
text-align: center; margin-top: 30px;
 
color: #333;
}


h2 {
color: #444;
}


.container { width: 80%;
max-width: 800px; margin: 0 auto; padding: 20px; background-color: #fff;
box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); border-radius: 10px;
margin-top: 40px;
}


form {
display: flex;
flex-direction: column; gap: 15px;
margin-bottom: 30px;
}


input[type="file"] {
 
padding: 10px; border-radius: 5px;
border: 1px solid #ccc; font-size: 16px;
}


button {
padding: 12px; font-size: 16px;
background-color: #5c6bc0; color: white;
border: none; border-radius: 5px; cursor: pointer;
transition: background-color 0.3s ease;
}


button:hover {
background-color: #3f4a9a;
}


#emotionResult, #genreResult { margin-top: 20px;
padding: 15px; background-color: #e1f5fe; border-radius: 5px;
 
font-weight: bold; color: #0288d1; text-align: center;
}


#recordingControls { margin-top: 20px; text-align: center;
}


#audioPlayer { margin-top: 10px; display: none;
}


/* Responsive design */ @media (max-width: 600px) {
.container { width: 95%; padding: 10px;
}


h1 {
font-size: 24px;
}
 
h2 {
font-size: 20px;
}


button {
padding: 10px; font-size: 14px;
}
}
</style>
</head>
<body>
<h1>Emotion and Genre Detection</h1>


<div class="container">
<h2>Emotion Detection</h2>
<form id="emotionForm" enctype="multipart/form-data">
<input type="file" name="audio" accept="audio/*" required>
<button type="submit">Predict Emotion</button>
</form>
<div id="recordingControls">
<button id="startRecording">Start Recording</button>
<button id="stopRecording" style="display:none;">Stop Recording</button>
<audio id="audioPlayer" controls style="display:none;"></audio>
</div>
 
<div id="emotionResult"></div>
<h2>Genre Detection</h2>
<form id="genreForm" enctype="multipart/form-data">
<input type="file" name="audio" accept="audio/*" required>
<button type="submit">Predict Genre</button>
</form>
<div id="genreResult"></div> <!-- Genre result will be displayed here -->
</div>


<script>
document.getElementById('emotionForm').addEventListener('submit', function(event) { event.preventDefault();
const formData = new FormData(this);


fetch('/predict_emotion', { method: 'POST',
body: formData
})
.then(response => response.json())
.then(data => {
document.getElementById('emotionResult').innerHTML = 'Predicted Emotion: ' + data.emotion;
})
.catch(error => console.error('Error predicting emotion:', error));
});


document.getElementById('genreForm').addEventListener('submit', function(event) {
 
event.preventDefault();
const formData = new FormData(this);


fetch('/predict_genre', { method: 'POST', body: formData
})
.then(response => response.json())
.then(data => {
document.getElementById('genreResult').innerHTML = 'Predicted Genre: ' + data.genre;
})
.catch(error => console.error('Error predicting genre:', error));
});


let audioChunks = []; let mediaRecorder;

document.getElementById('startRecording').onclick = async () => {
const stream = await navigator.mediaDevices.getUserMedia({ audio: true }); mediaRecorder = new MediaRecorder(stream);

mediaRecorder.ondataavailable = (event) => { audioChunks.push(event.data);
};


mediaRecorder.onstop = () => {
 
const audioBlob = new Blob(audioChunks, { type: 'audio/wav' }); const audioUrl = URL.createObjectURL(audioBlob);
const audioPlayer = document.getElementById('audioPlayer'); audioPlayer.src = audioUrl;
audioPlayer.style.display = 'block';


let audioData = new FormData(); audioData.append('audio', audioBlob); fetch('/predict_emotion', {
method: 'POST', body: audioData,
})
.then(response => response.json())
.then(data => {
document.getElementById('emotionResult').innerHTML = 'Predicted Emotion: ' + data.emotion;
})
.catch(error => {
console.error("Error in prediction:", error); document.getElementById('emotionResult').innerHTML = "Error in prediction.";
});


audioChunks = [];
};

mediaRecorder.start(); document.getElementById('startRecording').style.display = 'none';
 
document.getElementById('stopRecording').style.display = 'inline';
};


// Stop Recording Button document.getElementById('stopRecording').onclick = () => {
mediaRecorder.stop(); document.getElementById('stopRecording').style.display = 'none'; document.getElementById('startRecording').style.display = 'inline';
};
</script>
</body>
</html>