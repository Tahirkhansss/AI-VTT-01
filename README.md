<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Voice Transformer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        .header p {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            padding: 30px;
        }

        .section {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border: 1px solid #e0e0e0;
        }

        .section h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 1.4em;
            border-bottom: 2px solid #4facfe;
            padding-bottom: 10px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }

        textarea {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            resize: vertical;
            min-height: 100px;
            font-family: inherit;
            transition: border-color 0.3s ease;
        }

        textarea:focus {
            outline: none;
            border-color: #4facfe;
        }

        .controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 20px;
        }

        select {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            background: white;
            cursor: pointer;
            transition: border-color 0.3s ease;
        }

        select:focus {
            outline: none;
            border-color: #4facfe;
        }

        .button {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            border: none;
            padding: 15px 25px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(79, 172, 254, 0.3);
        }

        .button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(79, 172, 254, 0.4);
        }

        .button:active {
            transform: translateY(0);
        }

        .button.recording {
            background: linear-gradient(135deg, #ff6b6b 0%, #ff8e8e 100%);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .button:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .canvas-container {
            grid-column: 1 / -1;
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border: 1px solid #e0e0e0;
            text-align: center;
        }

        #visualizer {
            width: 100%;
            height: 200px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
        }

        .status {
            margin-top: 20px;
            padding: 15px;
            border-radius: 10px;
            font-weight: 600;
            text-align: center;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .status.show {
            opacity: 1;
        }

        .status.success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .status.error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .status.info {
            background: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .button-group .button {
            flex: 1;
        }

        .feature-badge {
            display: inline-block;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin: 5px 5px 0 0;
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
                gap: 20px;
                padding: 20px;
            }

            .controls {
                grid-template-columns: 1fr;
            }

            .button-group {
                flex-direction: column;
            }

            .header h1 {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎙️ AI Voice Transformer</h1>
            <p>Transform your voice with AI-powered age and gender conversion</p>
            <div style="margin-top: 15px;">
                <span class="feature-badge">Age Transform</span>
                <span class="feature-badge">Gender Convert</span>
                <span class="feature-badge">Real-time Visualization</span>
                <span class="feature-badge">Text-to-Speech</span>
            </div>
        </div>

        <div class="main-content">
            <div class="section">
                <h2>🎤 Voice Recording</h2>
                <div class="button-group">
                    <button id="recordBtn" class="button">Start Recording</button>
                    <button id="stopBtn" class="button" disabled>Stop Recording</button>
                    <button id="playOriginal" class="button" disabled>Play Original</button>
                </div>
                
                <div class="controls" style="margin-top: 20px;">
                    <div class="input-group">
                        <label for="ageSelect">Target Age:</label>
                        <select id="ageSelect">
                            <option value="child">Child (5-10 years)</option>
                            <option value="teenager">Teenager (13-17 years)</option>
                            <option value="young_adult" selected>Young Adult (18-30 years)</option>
                            <option value="adult">Adult (30-50 years)</option>
                            <option value="elderly">Elderly (50+ years)</option>
                        </select>
                    </div>
                    
                    <div class="input-group">
                        <label for="genderSelect">Target Gender:</label>
                        <select id="genderSelect">
                            <option value="male">Male</option>
                            <option value="female">Female</option>
                            <option value="neutral">Gender Neutral</option>
                        </select>
                    </div>
                </div>

                <button id="transformVoice" class="button" disabled style="width: 100%; margin-top: 15px;">
                    Transform Voice
                </button>
            </div>

            <div class="section">
                <h2>✍️ Text-to-Speech</h2>
                <div class="input-group">
                    <label for="textInput">Enter your text:</label>
                    <textarea id="textInput" placeholder="Type the text you want to convert to speech with voice transformation...">Hello! This is a sample text to demonstrate the AI voice transformation capabilities. You can change my age and gender using the controls below.</textarea>
                </div>

                <div class="controls">
                    <div class="input-group">
                        <label for="ttsAgeSelect">Voice Age:</label>
                        <select id="ttsAgeSelect">
                            <option value="child">Child Voice</option>
                            <option value="teenager">Teen Voice</option>
                            <option value="young_adult" selected>Young Adult</option>
                            <option value="adult">Adult Voice</option>
                            <option value="elderly">Elderly Voice</option>
                        </select>
                    </div>
                    
                    <div class="input-group">
                        <label for="ttsGenderSelect">Voice Gender:</label>
                        <select id="ttsGenderSelect">
                            <option value="male">Male Voice</option>
                            <option value="female" selected>Female Voice</option>
                            <option value="neutral">Neutral Voice</option>
                        </select>
                    </div>
                </div>

                <button id="speakText" class="button" style="width: 100%;">
                    🔊 Speak with Transformation
                </button>
            </div>
        </div>

        <div class="canvas-container">
            <h2>📊 Audio Visualization</h2>
            <canvas id="visualizer"></canvas>
            <div id="status" class="status"></div>
            
            <div class="button-group" style="margin-top: 20px;">
                <button id="playTransformed" class="button" disabled>Play Transformed</button>
                <button id="downloadAudio" class="button" disabled>Download Audio</button>
                <button id="clearAll" class="button">Clear All</button>
            </div>
        </div>
    </div>

    <script>
        class VoiceTransformer {
            constructor() {
                this.mediaRecorder = null;
                this.audioChunks = [];
                this.audioContext = null;
                this.analyser = null;
                this.canvas = document.getElementById('visualizer');
                this.ctx = this.canvas.getContext('2d');
                this.originalAudio = null;
                this.transformedAudio = null;
                this.isRecording = false;
                this.animationId = null;
                
                this.initializeCanvas();
                this.bindEvents();
                this.showStatus('Ready to transform your voice!', 'info');
            }

            initializeCanvas() {
                const rect = this.canvas.getBoundingClientRect();
                this.canvas.width = rect.width * window.devicePixelRatio;
                this.canvas.height = rect.height * window.devicePixelRatio;
                this.ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
                this.drawIdleVisualization();
            }

            drawIdleVisualization() {
                const width = this.canvas.width / window.devicePixelRatio;
                const height = this.canvas.height / window.devicePixelRatio;
                
                this.ctx.clearRect(0, 0, width, height);
                
                // Draw gradient background
                const gradient = this.ctx.createLinearGradient(0, 0, width, height);
                gradient.addColorStop(0, '#4facfe');
                gradient.addColorStop(1, '#00f2fe');
                
                this.ctx.fillStyle = gradient;
                this.ctx.globalAlpha = 0.1;
                this.ctx.fillRect(0, 0, width, height);
                this.ctx.globalAlpha = 1;
                
                // Draw idle waveform
                this.ctx.strokeStyle = '#4facfe';
                this.ctx.lineWidth = 2;
                this.ctx.beginPath();
                
                const centerY = height / 2;
                const amplitude = 20;
                const frequency = 0.02;
                
                for (let x = 0; x < width; x++) {
                    const y = centerY + Math.sin(x * frequency + Date.now() * 0.003) * amplitude;
                    if (x === 0) {
                        this.ctx.moveTo(x, y);
                    } else {
                        this.ctx.lineTo(x, y);
                    }
                }
                
                this.ctx.stroke();
                
                // Continue animation
                if (!this.isRecording) {
                    requestAnimationFrame(() => this.drawIdleVisualization());
                }
            }

            bindEvents() {
                document.getElementById('recordBtn').addEventListener('click', () => this.startRecording());
                document.getElementById('stopBtn').addEventListener('click', () => this.stopRecording());
                document.getElementById('playOriginal').addEventListener('click', () => this.playOriginal());
                document.getElementById('transformVoice').addEventListener('click', () => this.transformRecording());
                document.getElementById('speakText').addEventListener('click', () => this.speakText());
                document.getElementById('playTransformed').addEventListener('click', () => this.playTransformed());
                document.getElementById('downloadAudio').addEventListener('click', () => this.downloadAudio());
                document.getElementById('clearAll').addEventListener('click', () => this.clearAll());
            }

            async startRecording() {
                try {
                    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                    
                    this.audioContext = new AudioContext();
                    this.analyser = this.audioContext.createAnalyser();
                    const source = this.audioContext.createMediaStreamSource(stream);
                    source.connect(this.analyser);
                    
                    this.analyser.fftSize = 256;
                    
                    this.mediaRecorder = new MediaRecorder(stream);
                    this.audioChunks = [];
                    
                    this.mediaRecorder.ondataavailable = (event) => {
                        this.audioChunks.push(event.data);
                    };
                    
                    this.mediaRecorder.onstop = () => {
                        const audioBlob = new Blob(this.audioChunks, { type: 'audio/wav' });
                        this.originalAudio = audioBlob;
                        this.updateUIAfterRecording();
                    };
                    
                    this.mediaRecorder.start();
                    this.isRecording = true;
                    this.visualizeAudio();
                    
                    document.getElementById('recordBtn').disabled = true;
                    document.getElementById('recordBtn').classList.add('recording');
                    document.getElementById('stopBtn').disabled = false;
                    
                    this.showStatus('Recording... Speak now!', 'info');
                    
                } catch (error) {
                    this.showStatus('Microphone access denied. Please allow microphone access and try again.', 'error');
                }
            }

            stopRecording() {
                if (this.mediaRecorder && this.mediaRecorder.state !== 'inactive') {
                    this.mediaRecorder.stop();
                    this.mediaRecorder.stream.getTracks().forEach(track => track.stop());
                }
                
                this.isRecording = false;
                if (this.animationId) {
                    cancelAnimationFrame(this.animationId);
                }
                
                document.getElementById('recordBtn').disabled = false;
                document.getElementById('recordBtn').classList.remove('recording');
                document.getElementById('stopBtn').disabled = true;
                
                this.showStatus('Recording completed!', 'success');
                this.drawIdleVisualization();
            }

            visualizeAudio() {
                if (!this.isRecording) return;
                
                const width = this.canvas.width / window.devicePixelRatio;
                const height = this.canvas.height / window.devicePixelRatio;
                const bufferLength = this.analyser.frequencyBinCount;
                const dataArray = new Uint8Array(bufferLength);
                
                this.analyser.getByteFrequencyData(dataArray);
                
                this.ctx.clearRect(0, 0, width, height);
                
                // Draw bars
                const barWidth = width / bufferLength * 2;
                let x = 0;
                
                for (let i = 0; i < bufferLength; i++) {
                    const barHeight = (dataArray[i] / 255) * height * 0.8;
                    
                    const gradient = this.ctx.createLinearGradient(0, height - barHeight, 0, height);
                    gradient.addColorStop(0, '#4facfe');
                    gradient.addColorStop(1, '#00f2fe');
                    
                    this.ctx.fillStyle = gradient;
                    this.ctx.fillRect(x, height - barHeight, barWidth, barHeight);
                    
                    x += barWidth + 1;
                }
                
                this.animationId = requestAnimationFrame(() => this.visualizeAudio());
            }

            updateUIAfterRecording() {
                document.getElementById('playOriginal').disabled = false;
                document.getElementById('transformVoice').disabled = false;
            }

            playOriginal() {
                if (this.originalAudio) {
                    const audio = new Audio(URL.createObjectURL(this.originalAudio));
                    audio.play();
                    this.showStatus('Playing original recording...', 'info');
                }
            }

            async transformRecording() {
                if (!this.originalAudio) {
                    this.showStatus('No recording to transform!', 'error');
                    return;
                }

                const age = document.getElementById('ageSelect').value;
                const gender = document.getElementById('genderSelect').value;
                
                this.showStatus(`Transforming voice to ${age} ${gender}...`, 'info');
                
                // Simulate AI processing
                await this.simulateVoiceTransformation(this.originalAudio, age, gender);
                
                document.getElementById('playTransformed').disabled = false;
     
