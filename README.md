<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sliding Window Protocol Simulation</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background-color: #f4f4f9;
    }
    .container {
      margin: 20px;
    }
    .frame {
      display: inline-block;
      width: 60px;
      height: 60px;
      background-color: #ccc;
      margin: 5px;
      border-radius: 8px;
      line-height: 60px;
      font-size: 18px;
      color: white;
      font-weight: bold;
      position: relative;
    }
    .sent {
      background-color: #4caf50; /* Green for sent frames */
    }
    .ack {
      background-color: #2196f3; /* Blue for ack frames */
    }
    .window {
      border: 2px solid #000;
      padding: 10px;
      margin-bottom: 20px;
      display: inline-block;
      background-color: #f9f9f9;
    }
    .frame-container {
      margin-top: 30px;
    }
    .control-buttons {
      margin-top: 30px;
    }
    .info-text {
      margin-top: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h1>Sliding Window Protocol Simulation</h1>

  <div class="container">
    <div class="window">
      <h3>Sender Window</h3>
      <div id="sender-window" class="frame-container"></div>
    </div>
    
    <div class="window">
      <h3>Receiver Window</h3>
      <div id="receiver-window" class="frame-container"></div>
    </div>
    
    <div class="control-buttons">
      <button onclick="startSimulation()">Start Simulation</button>
    </div>
    
    <div class="info-text">
      <p>Window Size: 4, Total Frames: 10</p>
      <p>Click "Start Simulation" to begin the sliding window protocol simulation.</p>
    </div>
  </div>

  <script>
    let senderWindow = [];
    let receiverWindow = [];
    let windowSize = 4;  // Number of frames in the window
    let totalFrames = 10;  // Total number of frames to send
    let currentFrame = 0;  // Index of the current frame being sent
    let ackedFrames = 0;  // Count of frames acknowledged

    function startSimulation() {
      currentFrame = 0;  // Start from frame 0
      ackedFrames = 0;
      senderWindow = [];
      receiverWindow = [];

      // Initialize sender window with first set of frames
      for (let i = 0; i < windowSize; i++) {
        senderWindow.push(i + 1);  // Push frames to sender window
        receiverWindow.push(null);  // Empty receiver window
      }

      renderFrames();
      simulateSender();
    }

    function renderFrames() {
      // Render sender window
      let senderContainer = document.getElementById('sender-window');
      senderContainer.innerHTML = '';  // Clear existing content
      senderWindow.forEach(frame => {
        let frameElement = document.createElement('div');
        frameElement.className = 'frame sent';
        frameElement.textContent = `${frame}`;
        senderContainer.appendChild(frameElement);
      });

      // Render receiver window
      let receiverContainer = document.getElementById('receiver-window');
      receiverContainer.innerHTML = '';  // Clear existing content
      receiverWindow.forEach((frame, index) => {
        let frameElement = document.createElement('div');
        frameElement.className = 'frame';
        frameElement.textContent = frame ? `ACK ${frame}` : '';
        frameElement.classList.add(frame ? 'ack' : '');  // Apply ack styling for acknowledged frames
        receiverContainer.appendChild(frameElement);
      });
    }

    function simulateSender() {
      if (currentFrame < totalFrames) {
        // Simulate sending a frame
        let sentFrame = senderWindow[0];  // Get the first frame from sender window

        // Simulate receiver acknowledging frame
        setTimeout(() => {
          // Receiver receives and acknowledges the frame
          receiverWindow[ackedFrames] = sentFrame;
          ackedFrames++;

          // Slide the window by shifting acknowledged frames out and adding a new frame to the sender window
          senderWindow.shift();  // Remove the acknowledged frame
          if (currentFrame + windowSize < totalFrames) {
            senderWindow.push(currentFrame + windowSize + 1);  // Add a new frame to the window
          }

          renderFrames();
          currentFrame++;  // Move to the next frame
          simulateSender();  // Recursively call to send the next frame
        }, 1000); // Simulate delay of 1 second for receiving acknowledgment
      }
    }
  </script>

</body>
</html>

