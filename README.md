# Desk-Height-Calculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ergonomic Desk Height Calculator</title>
  <style>
    body {
      font-family: sans-serif;
      margin: 10px;
      padding: 0;
      box-sizing: border-box;
      background: #f5f5f5;
      text-align: center;
    }

    h1, h2 {
      margin: 10px;
    }

    #instructions {
      margin: 20px auto;
      max-width: 600px;
      font-size: 16px;
      background: #fff;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
      text-align: left;
    }

    #calculator {
      margin-bottom: 20px;
    }

    #heightInput, #modeToggle {
      font-size: 16px;
      padding: 5px;
      margin-top: 10px;
      width: 80%;
      max-width: 300px;
    }

    .diagram-container {
      position: relative;
      margin: 10px auto;
      background: white;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      display: none;
      page-break-inside: avoid;
    }

    .diagram-container img {
      max-width: 90vw;
      height: auto;
    }

    .label-box {
      position: absolute;
      background: rgba(255, 255, 255, 0.9);
      border: 2px solid green;
      padding: 4px;
      font-size: 12px;
      font-weight: bold;
    }

    /* Standing labels */
    #standing-desk-box { top: 70%; left: 85%; }
    #standing-monitor-box { top: 20%; left: 85%; }
    #standing-elbow-box { top: 50%; left: 10%; }

    /* Sitting labels */
    #sitting-desk-box { top: 65%; left: 85%; }
    #sitting-monitor-box { top: 20%; left: 85%; }
    #sitting-seat-box { top: 75%; left: 10%; }

    @media print {
      body {
        background: #fff;
        color: #000;
      }
      #calculator, #modeToggle {
        display: none;
      }
      .diagram-container {
        display: block !important;
        page-break-before: always;
      }
    }
  </style>
</head>
<body>
  <h1>Ergonomic Desk Height Calculator</h1>
  <div id="instructions">
    <strong>Instructions:</strong>
    <ul>
      <li>Enter your height in centimeters.</li>
      <li>Select either <strong>Standing</strong> or <strong>Sitting</strong> mode.</li>
      <li>The diagram will show ideal ergonomic desk, monitor, and seat/elbow heights.</li>
      <li>To print your setup, use your browser's print function (Ctrl+P or Cmd+P).</li>
    </ul>
  </div>

  <div id="calculator">
    <input type="number" id="heightInput" min="100" max="250" value="170" placeholder="Enter your height (cm)" /><br/>
    <select id="modeToggle">
      <option value="standing">Standing</option>
      <option value="sitting">Sitting</option>
    </select>
  </div>

  <div class="diagram-container" id="standingDiagram">
    <h2>Standing</h2>
    <img src="https://vicgov.sharepoint.com/sites/ecm_882/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2Fecm%5F882%2FShared%20Documents%2FErgonomic%2Fstanding%5Fdesk%2Epng&viewid=9f3078cf%2D9b20%2D4f8d%2Daf4c%2D8761cf94774d&parent=%2Fsites%2Fecm%5F882%2FShared%20Documents%2FErgonomic" alt="Standing Desk Diagram" />
    <div class="label-box" id="standing-desk-box">Desk: -- cm</div>
    <div class="label-box" id="standing-monitor-box">Monitor: -- cm</div>
    <div class="label-box" id="standing-elbow-box">Elbows: -- cm</div>
  </div>

  <div class="diagram-container" id="sittingDiagram">
    <h2>Sitting</h2>
    <img src="https://vicgov.sharepoint.com/sites/ecm_882/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2Fecm%5F882%2FShared%20Documents%2FErgonomic%2Fsitting%5Fdesk%2Epng&viewid=9f3078cf%2D9b20%2D4f8d%2Daf4c%2D8761cf94774d&parent=%2Fsites%2Fecm%5F882%2FShared%20Documents%2FErgonomic" alt="Sitting Desk Diagram" />
    <div class="label-box" id="sitting-desk-box">Desk: -- cm</div>
    <div class="label-box" id="sitting-monitor-box">Monitor: -- cm</div>
    <div class="label-box" id="sitting-seat-box">Seat: -- cm</div>
  </div>

  <script>
    const heightInput = document.getElementById("heightInput");
    const modeToggle = document.getElementById("modeToggle");

    const standingDiagram = document.getElementById("standingDiagram");
    const sittingDiagram = document.getElementById("sittingDiagram");

    const standingDeskBox = document.getElementById("standing-desk-box");
    const standingMonitorBox = document.getElementById("standing-monitor-box");
    const standingElbowBox = document.getElementById("standing-elbow-box");

    const sittingDeskBox = document.getElementById("sitting-desk-box");
    const sittingMonitorBox = document.getElementById("sitting-monitor-box");
    const sittingSeatBox = document.getElementById("sitting-seat-box");

    function updateHeights(height) {
      // Standing
      const sDesk = (height * 0.90).toFixed(1);
      const sElbow = (height * 0.95).toFixed(1);
      const sMonitor = (height * 1.10).toFixed(1);

      standingDeskBox.textContent = `Desk: ${sDesk} cm`;
      standingElbowBox.textContent = `Elbows: ${sElbow} cm`;
      standingMonitorBox.textContent = `Monitor: ${sMonitor} cm`;

      // Sitting
      const seatedHipToFloor = (height * 0.25).toFixed(1);
      const sitDesk = (height * 0.70).toFixed(1);
      const sitMonitor = (height * 0.95).toFixed(1);

      sittingSeatBox.textContent = `Seat: ${seatedHipToFloor} cm`;
      sittingDeskBox.textContent = `Desk: ${sitDesk} cm`;
      sittingMonitorBox.textContent = `Monitor: ${sitMonitor} cm`;
    }

    function updateView() {
      const mode = modeToggle.value;
      standingDiagram.style.display = mode === "standing" ? "block" : "none";
      sittingDiagram.style.display = mode === "sitting" ? "block" : "none";
    }

    heightInput.addEventListener("input", () => {
      updateHeights(parseFloat(heightInput.value));
    });

    modeToggle.addEventListener("change", updateView);

    updateHeights(parseFloat(heightInput.value));
    updateView();
  </script>
</body>
</html>
