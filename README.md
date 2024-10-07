<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Beat Detection with Tone.js</title>
    <script src="https://cdn.jsdelivr.net/npm/tone@14"></script>
</head>
<body>
    <script>
        // Your Tone.js code goes here
        const mic = new Tone.UserMedia();
        const analyser = new Tone.Analyser("fft", 2048);
        
        mic.open().then(() => {
            console.log("Microphone is open");
            mic.connect(analyser);
            detectBeat();
        }).catch((err) => {
            console.error("Error accessing microphone", err);
        });

        function detectBeat() {
            const freqData = analyser.getValue();
            const threshold = 0.1;
            let isBeatDetected = false;

            for (let i = 0; i < freqData.length; i++) {
                if (freqData[i] > threshold) {
                    isBeatDetected = true;
                    break;
                }
            }

            if (isBeatDetected) {
                playDrum();
            }

            requestAnimationFrame(detectBeat);
        }

        function playDrum() {
            const drumSound = new Tone.Player("path/to/drum-sound.mp3").toDestination();
            drumSound.start();
        }
    </script>
</body>
</html>
