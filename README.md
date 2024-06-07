<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Intersection Calculation</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        .result {
            margin-top: 20px;
            padding: 10px;
            background-color: #f0f0f0;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>
    <h1>Intersection Calculation</h1>
    <div id="result" class="result"></div>

    <script type="module">
        import * as THREE from 'https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.module.js';

function calculateIntersection(p1, p2, p3, d1, d2, d3) {
    // Implementing trilateration algorithm to find the intersection
    const ex = p2.clone().sub(p1).normalize();
    const i = ex.dot(p3.clone().sub(p1));
    const ey = p3.clone().sub(p1).sub(ex.clone().multiplyScalar(i)).normalize();
    const ez = ex.clone().cross(ey);
    const d = p2.distanceTo(p1);
    const j = ey.dot(p3.clone().sub(p1));
    
    const x = (Math.pow(d1, 2) - Math.pow(d2, 2) + Math.pow(d, 2)) / (2 * d);
    const y = (Math.pow(d1, 2) - Math.pow(d3, 2) + Math.pow(i, 2) + Math.pow(j, 2)) / (2 * j) - (i / j) * x;
    const z_squared = Math.pow(d1, 2) - Math.pow(x, 2) - Math.pow(y, 2);

    if (z_squared < 0) {
        // Aucune intersection possible, renvoie null
        return null;
    }
    
    const z = Math.sqrt(z_squared);
    
    const intersection = p1.clone().add(ex.multiplyScalar(x)).add(ey.multiplyScalar(y)).add(ez.multiplyScalar(z));
    return intersection;
}


        function init() {
            // Define reference points and distances
            const p1 = new THREE.Vector3(4, 4, 4);
            const p2 = new THREE.Vector3(-1, 2, 2);
            const p3 = new THREE.Vector3(2, 5, 3);
            const d1 = 6.63;
            const d2 = 5;
            const d3 = 7.07;

            // Calculate intersection point
            const intersection = calculateIntersection(p1, p2, p3, d1, d2, d3);
            
            // Display the intersection point
            const resultDiv = document.getElementById('result');
            if (intersection) {
                resultDiv.innerHTML = `
                    <p>Intersection Point:</p>
                    <ul>
                        <li>x: ${intersection.x.toFixed(2)}</li>
                        <li>y: ${intersection.y.toFixed(2)}</li>
                        <li>z: ${intersection.z.toFixed(2)}</li>
                    </ul>
                `;
            } else {
                resultDiv.innerHTML = "<p>No intersection found.</p>";
            }
        }

        init();
    </script>
</body>
</html>
