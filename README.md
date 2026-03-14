<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zone Total Planner - PRO</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/matter-js/0.19.0/matter.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Bangers&display=swap" rel="stylesheet">
    <style>
        body {
            background-color: #0047AB; /* Bleu sport */
            background-image: radial-gradient(rgba(0,0,0,0.3) 15%, transparent 16%);
            background-size: 25px 25px;
            margin: 0; overflow: hidden; color: white; font-family: 'Bangers', cursive;
        }
        .header-comic {
            background: #FFD700; color: black; border: 6px solid black;
            box-shadow: 10px 10px 0px #000; margin: 20px auto;
            max-width: 600px; padding: 15px; text-align: center;
        }
        .grid-gym {
            display: grid; grid-template-columns: repeat(5, 1fr); gap: 15px; padding: 20px;
        }
        .day-box {
            background: rgba(255, 255, 255, 0.1); border: 3px solid #FFD700;
            border-radius: 15px; height: 400px; padding: 10px;
        }
        .ballon-groupe {
            position: absolute; width: 70px; height: 70px; border-radius: 50%;
            border: 4px solid black; background: #FFD700; color: black;
            display: flex; align-items: center; justify-content: center;
            font-size: 1.5rem; box-shadow: 5px 5px 0px rgba(0,0,0,0.4); cursor: grab;
        }
    </style>
</head>
<body>
    <header class="header-comic">
        <h1 class="text-5xl">ZONE TOTAL PLANNER</h1>
        <p class="text-xl">Mr. Roots Edition</p>
    </header>
    <div class="grid-gym">
        <div class="day-box text-center">LUNDI</div>
        <div class="day-box text-center">MARDI</div>
        <div class="day-box text-center">MERCREDI</div>
        <div class="day-box text-center">JEUDI</div>
        <div class="day-box text-center">VENDREDI</div>
    </div>
    <script>
        const { Engine, Bodies, Composite, Mouse, MouseConstraint } = Matter;
        const engine = Engine.create();
        const world = engine.world;
        const ground = Bodies.rectangle(window.innerWidth/2, window.innerHeight + 30, window.innerWidth, 60, { isStatic: true });
        const wallLeft = Bodies.rectangle(-30, window.innerHeight/2, 60, window.innerHeight, { isStatic: true });
        const wallRight = Bodies.rectangle(window.innerWidth + 30, window.innerHeight/2, 60, window.innerHeight, { isStatic: true });
        Composite.add(world, [ground, wallLeft, wallRight]);
        const groups = ['101', '202', '303', '404', '505', '606'];
        groups.forEach((id, i) => {
            const ball = Bodies.circle(100 + (i * 90), 100, 35, { restitution: 0.7 });
            const el = document.createElement('div');
            el.className = 'ballon-groupe'; el.innerText = id;
            document.body.appendChild(el);
            ball.element = el;
            Composite.add(world, ball);
        });
        (function update() {
            Composite.allBodies(world).forEach(b => {
                if(b.element) {
                    b.element.style.left = `${b.position.x - 35}px`;
                    b.element.style.top = `${b.position.y - 35}px`;
                    b.element.style.transform = `rotate(${b.angle}rad)`;
                }
            });
            Engine.update(engine);
            requestAnimationFrame(update);
        })();
        const mc = MouseConstraint.create(engine, { mouse: Mouse.create(document.body) });
        Composite.add(world, mc);
    </script>
</body>
</html>
