<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #ecc9c9;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .player {
            width: 350px;
            padding: 40px;
            border-radius: 15px;
            background: linear-gradient(135deg, #ff7eb3, #ef8d9d);
            box-shadow: 0 4px 15px rgba(255, 117, 140, 0.3);
            height: 250px; /* Increased height */
        }
        h2 {
            margin-bottom: 20px;
        }
        .controls button {
            margin: 10px;
            padding: 12px;
            border: none;
            background-color: white;
            color: #ff758c;
            font-size: 16px;
            cursor: pointer;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            transition: 0.3s;
        }
        .controls button:hover {
            background-color: #ffebf0;
        }
        input[type='range'] {
            width: 80%;
            margin-top: 20px;
        }
        #time-container {
            margin-top: 10px;
            font-size: 14px;
        }
        #progress-bar {
            width: 100%;
            height: 5px;
            background: #fff;
            border-radius: 5px;
            margin-top: 10px;
            cursor: pointer;
        }
        #progress {
            height: 5px;
            background: #ff758c;
            width: 0%;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="player">
        <h2>🎵 Music Player 🎵</h2>
        <audio id="audio" ontimeupdate="updateTime()"></audio>
        <p id="song-title">No song playing</p>
        <div id="time-container">
            <span id="current-time">00:00</span>
            <div id="progress-bar" onclick="seek(event)">
                <div id="progress"></div>
            </div>
            <span id="total-duration">00:00</span>
        </div>
        <div class="controls">
            <button onclick="prevSong()">⏮️</button>
            <button id="playPauseButton" onclick="togglePlay()">▶️</button>
            <button onclick="nextSong()">⏭️</button>
        </div>

    <script>
        const songs = [
            { title: "Naina", src: "music/Naina.mp3" },
            { title: "Jhalak_Tippu_Sultan", src: "music/Jhalak_Tippu_Sultan.mp3" },
            { title: "Pal", src: "music/Pal.mp3" }
        ];
        let currentSongIndex = 0;
        const audio = document.getElementById("audio");
        const songTitle = document.getElementById("song-title");
        const playPauseButton = document.getElementById("playPauseButton");
        const currentTimeElement = document.getElementById("current-time");
        const totalDurationElement = document.getElementById("total-duration");
        const progressBar = document.getElementById("progress-bar");
        const progress = document.getElementById("progress");

        function togglePlay() {
            if (audio.paused) {
                audio.play();
                playPauseButton.innerHTML = "⏸️";
            } else {
                audio.pause();
                playPauseButton.innerHTML = "▶️";
            }
        }

        function nextSong() {
            currentSongIndex = (currentSongIndex + 1) % songs.length;
            changeSong();
        }

        function prevSong() {
            currentSongIndex = (currentSongIndex - 1 + songs.length) % songs.length;
            changeSong();
        }

        function changeSong() {
            audio.src = songs[currentSongIndex].src;
            songTitle.innerText = songs[currentSongIndex].title;
            audio.play();
            playPauseButton.innerHTML = "⏸️";
        }

        function setVolume() {
            audio.volume = document.getElementById("volume").value;
        }

        function updateTime() {
            let currentMinutes = Math.floor(audio.currentTime / 60);
            let currentSeconds = Math.floor(audio.currentTime % 60);
            let totalMinutes = Math.floor(audio.duration / 60);
            let totalSeconds = Math.floor(audio.duration % 60);

            if (!isNaN(totalMinutes) && !isNaN(totalSeconds)) {
                totalDurationElement.innerText = `${totalMinutes}:${totalSeconds < 10 ? '0' : ''}${totalSeconds}`;
            }

            currentTimeElement.innerText = `${currentMinutes}:${currentSeconds < 10 ? '0' : ''}${currentSeconds}`;
            
            let progressWidth = (audio.currentTime / audio.duration) * 100;
            progress.style.width = progressWidth + "%";
        }

        function seek(event) {
            const clickPosition = event.offsetX;
            const progressBarWidth = progressBar.clientWidth;
            const seekTime = (clickPosition / progressBarWidth) * audio.duration;
            audio.currentTime = seekTime;
        }

        // Load the first song automatically
        window.onload = () => {
            changeSong();
        };
    </script>
</body>
</html>
