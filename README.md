# Horloge-cosmique
Horlogerie moderne 
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Horloge Cosmique</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #0c0c1d;
            font-family: 'Arial', sans-serif;
            overflow: hidden;
        }

        .universe {
            position: relative;
            width: 400px;
            height: 400px;
            border-radius: 50%;
            background: radial-gradient(circle, #1a1a40 0%, #0c0c1d 80%);
            box-shadow: 0 0 50px rgba(79, 70, 229, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .orbit {
            position: absolute;
            border-radius: 50%;
            border: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .hours-orbit {
            width: 300px;
            height: 300px;
        }

        .minutes-orbit {
            width: 240px;
            height: 240px;
        }

        .seconds-orbit {
            width: 180px;
            height: 180px;
        }

        .planet {
            position: absolute;
            border-radius: 50%;
            transform-origin: center;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .hours-planet {
            width: 40px;
            height: 40px;
            background: linear-gradient(145deg, #ff6b6b, #a83277);
            box-shadow: 0 0 15px #ff6b6b;
            left: calc(50% - 20px);
            top: -20px;
        }

        .minutes-planet {
            width: 30px;
            height: 30px;
            background: linear-gradient(145deg, #58e4ff, #2979ff);
            box-shadow: 0 0 15px #58e4ff;
            left: calc(50% - 15px);
            top: -15px;
        }

        .seconds-planet {
            width: 20px;
            height: 20px;
            background: linear-gradient(145deg, #fdfc47, #45fce4);
            box-shadow: 0 0 15px #fdfc47;
            left: calc(50% - 10px);
            top: -10px;
        }

        .center-star {
            width: 60px;
            height: 60px;
            background: radial-gradient(circle, #ffffff 0%, #ffc107 30%, #ff5722 100%);
            border-radius: 50%;
            box-shadow: 0 0 30px #ffc107;
            animation: pulse 5s infinite alternate;
        }

        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background-color: white;
            border-radius: 50%;
            box-shadow: 0 0 2px white;
            animation: twinkle 3s infinite;
        }

        .digital-time {
            position: absolute;
            bottom: 40px;
            color: white;
            font-size: 24px;
            font-weight: bold;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 10px 20px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .date {
            position: absolute;
            top: 40px;
            color: white;
            font-size: 18px;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 8px 16px;
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
                box-shadow: 0 0 30px #ffc107;
            }
            100% {
                transform: scale(1.1);
                box-shadow: 0 0 50px #ffc107;
            }
        }

        @keyframes twinkle {
            0% {
                opacity: 0.2;
            }
            50% {
                opacity: 1;
            }
            100% {
                opacity: 0.2;
            }
        }

        .comet {
            position: absolute;
            width: 4px;
            height: 4px;
            background-color: white;
            border-radius: 50%;
            box-shadow: 0 0 10px white;
            animation: comet 8s linear infinite;
            opacity: 0;
        }

        @keyframes comet {
            0% {
                transform: translate(-200px, -200px) rotate(45deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            20% {
                transform: translate(200px, 200px) rotate(45deg);
                opacity: 0;
            }
            100% {
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <div class="universe">
        <div class="orbit hours-orbit">
            <div class="planet hours-planet" id="hourPlanet"></div>
        </div>
        <div class="orbit minutes-orbit">
            <div class="planet minutes-planet" id="minutePlanet"></div>
        </div>
        <div class="orbit seconds-orbit">
            <div class="planet seconds-planet" id="secondPlanet"></div>
        </div>
        <div class="center-star"></div>
        <div class="digital-time" id="digitalTime"></div>
        <div class="date" id="currentDate"></div>
    </div>

    <script>
        // Create stars
        function createStars() {
            const universe = document.querySelector('.universe');
            for (let i = 0; i < 100; i++) {
                const star = document.createElement('div');
                star.classList.add('star');
                
                // Random position within the universe
                star.style.left = `${Math.random() * 400}px`;
                star.style.top = `${Math.random() * 400}px`;
                
                // Random size
                const size = Math.random() * 2 + 1;
                star.style.width = `${size}px`;
                star.style.height = `${size}px`;
                
                // Random animation delay
                star.style.animationDelay = `${Math.random() * 3}s`;
                
                universe.appendChild(star);
            }
        }

        // Create occasional comets
        function createComet() {
            const universe = document.querySelector('.universe');
            const comet = document.createElement('div');
            comet.classList.add('comet');
            
            // Random position and angle
            const angle = Math.random() * 360;
            comet.style.transform = `rotate(${angle}deg)`;
            
            universe.appendChild(comet);
            
            // Remove comet after animation
            setTimeout(() => {
                comet.remove();
            }, 8000);
        }

        // Update the clock
        function updateClock() {
            const now = new Date();
            const hours = now.getHours();
            const minutes = now.getMinutes();
            const seconds = now.getSeconds();
            const milliseconds = now.getMilliseconds();
            
            // Calculate angles for each planet
            const hourAngle = (hours % 12 + minutes / 60) * 30; // 30 degrees per hour
            const minuteAngle = (minutes + seconds / 60) * 6; // 6 degrees per minute
            const secondAngle = (seconds + milliseconds / 1000) * 6; // 6 degrees per second
            
            // Update planet positions
            document.getElementById('hourPlanet').style.transform = `rotate(${hourAngle}deg) translateY(-150px)`;
            document.getElementById('minutePlanet').style.transform = `rotate(${minuteAngle}deg) translateY(-120px)`;
            document.getElementById('secondPlanet').style.transform = `rotate(${secondAngle}deg) translateY(-90px)`;
            
            // Update digital time
            const formatTime = (num) => num.toString().padStart(2, '0');
            document.getElementById('digitalTime').textContent = `${formatTime(hours)}:${formatTime(minutes)}:${formatTime(seconds)}`;
            
            // Update date
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('currentDate').textContent = now.toLocaleDateString('fr-FR', options);
            
            // Request next frame
            requestAnimationFrame(updateClock);
            
            // Randomly create comets (approximately once every minute)
            if (Math.random() < 0.0005) {
                createComet();
            }
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            createStars();
            updateClock();
        });
    </script>
</body>
</html>
