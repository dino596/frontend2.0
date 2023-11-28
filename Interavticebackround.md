<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mouse Follow Calculator</title>
</head>
<body>
  <div id="mouse-follower"></div>

  <div id="calculator-container">
    <!-- Your calculator HTML here -->
  </div>

  <script>
    // Get the background element
    const mouseFollower = document.getElementById('mouse-follower');

    // Update background position on mouse move
    document.addEventListener('mousemove', (e) => {
      const mouseX = e.clientX / window.innerWidth * 100;
      const mouseY = e.clientY / window.innerHeight * 100;

      // Update background position based on mouse coordinates
      mouseFollower.style.backgroundPosition = `${mouseX}% ${mouseY}%`;
    });
  </script>
</body>
</html>
