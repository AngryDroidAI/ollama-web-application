<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Ollama Web App</title>
  <style>
    :root {
      --bg-primary: #0f172a;
      --bg-secondary: #1e293b;
      --bg-tertiary: #334155;
      --text-primary: #f1f5f9;
      --text-secondary: #cbd5e1;
      --accent: #6366f1;
      --accent-hover: #4f46e5;
      --success: #10b981;
      --warning: #f59e0b;
      --danger: #ef4444;
      --border: #475569;
      --card-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: var(--bg-primary);
      color: var(--text-primary);
      line-height: 1.6;
      padding: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }
    
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 30px;
      padding-bottom: 15px;
      border-bottom: 1px solid var(--border);
    }
    
    h1 {
      font-size: 2.2rem;
      font-weight: 700;
      background: linear-gradient(90deg, #6366f1, #8b5cf6);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      display: inline-block;
    }
    
    .logo {
      display: flex;
      align-items: center;
      gap: 12px;
    }
    
    .logo-icon {
      width: 40px;
      height: 40px;
      background: linear-gradient(135deg, #6366f1, #8b5cf6);
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    
    .logo-icon svg {
      width: 24px;
      height: 24px;
      fill: white;
    }
    
    .endpoint-container {
      display: flex;
      align-items: center;
      gap: 10px;
      background: var(--bg-secondary);
      padding: 12px 15px;
      border-radius: 8px;
      border: 1px solid var(--border);
      width: 100%;
      max-width: 500px;
    }
    
    .endpoint-container label {
      font-weight: 500;
      color: var(--text-secondary);
      white-space: nowrap;
    }
    
    #endpoint {
      flex: 1;
      background: transparent;
      border: none;
      color: var(--text-primary);
      padding: 5px 0;
      font-size: 1rem;
    }
    
    #endpoint:focus {
      outline: none;
    }
    
    .controls {
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 15px;
      margin-bottom: 25px;
    }
    
    .model-controls {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }
    
    select, input, button {
      background: var(--bg-secondary);
      color: var(--text-primary);
      border: 1px solid var(--border);
      border-radius: 6px;
      padding: 10px 15px;
      font-size: 1rem;
      transition: all 0.2s ease;
    }
    
    select:focus, input:focus {
      outline: none;
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.2);
    }
    
    button {
      cursor: pointer;
      font-weight: 500;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 10px 20px;
    }
    
    button:hover {
      opacity: 0.9;
    }
    
    #refresh {
      background: var(--accent);
      border-color: var(--accent);
    }
    
    #refresh:hover {
      background: var(--accent-hover);
    }
    
    .pull-container {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }
    
    #pull {
      background: var(--success);
      border-color: var(--success);
    }
    
    #pull:hover {
      background: #0da271;
    }
    
    #pullname {
      flex: 1;
      min-width: 200px;
    }
    
    .model-select-container {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 25px;
    }
    
    .model-select-container label {
      font-weight: 500;
      color: var(--text-secondary);
      white-space: nowrap;
    }
    
    #model {
      flex: 1;
      min-width: 200px;
    }
    
    .template-container {
      margin-bottom: 10px;
    }
    
    .template-container label {
      display: block;
      font-weight: 500;
      margin-bottom: 10px;
      color: var(--text-secondary);
    }
    
    #templates {
      width: 100%;
      background: var(--bg-secondary);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 10px 15px;
      color: var(--text-primary);
      font-family: inherit;
      font-size: 1rem;
    }
    
    .prompt-container {
      margin-bottom: 25px;
    }
    
    .prompt-container label {
      display: block;
      font-weight: 500;
      margin-bottom: 10px;
      color: var(--text-secondary);
    }
    
    #prompt {
      width: 100%;
      min-height: 150px;
      background: var(--bg-secondary);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 15px;
      color: var(--text-primary);
      font-family: inherit;
      font-size: 1rem;
      resize: vertical;
    }
    
    #prompt:focus {
      outline: none;
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.2);
    }
    
    .action-buttons {
      display: flex;
      gap: 15px;
      margin-bottom: 30px;
    }
    
    #run {
      background: var(--accent);
      border-color: var(--accent);
      padding: 12px 25px;
      font-size: 1.1rem;
    }
    
    #run:hover {
      background: var(--accent-hover);
    }
    
    #cancel {
      background: var(--danger);
      border-color: var(--danger);
      padding: 12px 25px;
      font-size: 1.1rem;
    }
    
    #cancel:hover {
      background: #dc2626;
    }
    
    .status {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 20px;
      padding: 12px 15px;
      background: var(--bg-secondary);
      border-radius: 8px;
      border: 1px solid var(--border);
    }
    
    .status-indicator {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: var(--success);
    }
    
    .status-text {
      font-weight: 500;
    }
    
    .output-container {
      background: var(--bg-secondary);
      border: 1px solid var(--border);
      border-radius: 8px;
      overflow: hidden;
      box-shadow: var(--card-shadow);
    }
    
    .output-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px 20px;
      background: var(--bg-tertiary);
      border-bottom: 1px solid var(--border);
    }
    
    .output-header h3 {
      font-size: 1.3rem;
      font-weight: 600;
      margin: 0;
    }
    
    .output-actions {
      display: flex;
      gap: 10px;
    }
    
    .output-actions button {
      padding: 8px 15px;
      font-size: 0.9rem;
    }
    
    #output {
      white-space: pre-wrap;
      padding: 20px;
      min-height: 200px;
      max-height: 500px;
      overflow-y: auto;
      background: var(--bg-primary);
      font-family: 'Consolas', 'Courier New', monospace;
      line-height: 1.5;
    }
    
    .token {
      animation: fadeIn 0.1s ease-in;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    
    .stats {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      margin-top: 30px;
    }
    
    .stat-card {
      background: var(--bg-secondary);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 20px;
      box-shadow: var(--card-shadow);
    }
    
    .stat-card h4 {
      font-size: 1rem;
      color: var(--text-secondary);
      margin-bottom: 10px;
    }
    
    .stat-value {
      font-size: 1.8rem;
      font-weight: 700;
      margin: 10px 0;
      color: var(--accent);
    }
    
    .stat-desc {
      font-size: 0.9rem;
      color: var(--text-secondary);
    }
    
    footer {
      margin-top: 40px;
      padding-top: 20px;
      border-top: 1px solid var(--border);
      text-align: center;
      color: var(--text-secondary);
      font-size: 0.9rem;
    }
    
    /* Modal styles */
    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.7);
    }
    
    .modal-content {
      background-color: var(--bg-primary);
      margin: 5% auto;
      padding: 20px;
      border: 1px solid var(--border);
      border-radius: 8px;
      width: 90%;
      max-width: 600px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    }
    
    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 15px;
      padding-bottom: 10px;
      border-bottom: 1px solid var(--border);
    }
    
    .modal-header h3 {
      margin: 0;
    }
    
    .close {
      color: var(--text-secondary);
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }
    
    .close:hover {
      color: var(--text-primary);
    }
    
    .modal-body {
      max-height: 60vh;
      overflow-y: auto;
      padding: 10px 0;
      font-family: 'Consolas', 'Courier New', monospace;
      font-size: 0.9rem;
    }
    
    @media (max-width: 768px) {
      .controls {
        grid-template-columns: 1fr;
      }
      
      .model-controls, .pull-container, .action-buttons {
        flex-direction: column;
      }
      
      #run, #cancel {
        width: 100%;
      }
      
      .modal-content {
        margin: 10% auto;
        width: 95%;
      }
    }
  </style>
</head>
<body>
  <header>
    <div class="logo">
      <div class="logo-icon">
        <svg viewBox="0 0 24 24">
          <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.94-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 
2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z"/>
        </svg>
      </div>
      <h1>Ollama Web Application</h1>
    </div>
    <button id="model-info" style="background: var(--warning);">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none">
        <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-6h2v6zm0-8h-2V7h2v2z" fill="white"/>
      </svg>
      Model Info
    </button>
  </header>

  <div class="endpoint-container">
    <label for="endpoint">Endpoint:</label>
    <input id="endpoint" value="http://127.0.0.1:11434">
  </div>

  <div class="controls">
    <div class="model-controls">
      <button id="refresh">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none">
          <path d="M17.65 6.35C16.2 4.9 14.21 4 12 4C7.58 4 4.01 7.58 4.01 12C4.01 16.42 7.58 20 12 20C15.73 20 18.84 17.45 19.73 14H17.65C16.83 16.33 14.61 18 12 18C8.69 18 6 15.31 6 12C6 8.69 8.69 6 12 6C13.66 6 15.14 6.69 16.22 
7.78L13 11H20V4L17.65 6.35Z" fill="white"/>
        </svg>
        Refresh Models
      </button>
      <select id="model">
        <option value="">Loading models...</option>
      </select>
    </div>
    
    <div class="pull-container">
      <input id="pullname" placeholder="Model name (e.g. llama3)" />
      <button id="pull">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none">
          <path d="M19 9h-4V3H9v6H5l7 7 7-7zM5 18v2h14v-2H5z" fill="white"/>
        </svg>
        Pull Model
      </button>
    </div>
  </div>

  <div class="template-container">
    <label for="templates">Prompt Template:</label>
    <select id="templates">
      <option value="">Select a template...</option>
      <option value="coding">Coding Template</option>
      <option value="explanation">Explanation Template</option>
      <option value="creative">Creative Writing Template</option>
    </select>
  </div>

  <div class="prompt-container">
    <label for="prompt">Prompt:</label>
    <textarea id="prompt" placeholder="Type your prompt...">Explain quantum computing in simple terms.</textarea>
  </div>

  <div class="action-buttons">
    <button id="run">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none">
        <path d="M8 5v14l11-7z" fill="white"/>
      </svg>
      Run Generation
    </button>
    <button id="cancel" style="display:none;">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none">
        <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12 19 6.41z" fill="white"/>
      </svg>
      Cancel
    </button>
  </div>

  <div class="status">
    <div class="status-indicator"></div>
    <div class="status-text" id="status">Ready</div>
  </div>

  <div class="output-container">
    <div class="output-header">
      <h3>Output</h3>
      <div class="output-actions">
        <button id="copy">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
            <path d="M16 1H4c-1.1 0-2 .9-2 2v14h2V3h12V1zm3 4H8c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h11c1.1 0 2-.9 2-2V7c0-1.1-.9-2-2-2zm0 16H8V7h11v14z" fill="currentColor"/>
          </svg>
          Copy
        </button>
        <button id="clear">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
            <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12 19 6.41z" fill="currentColor"/>
          </svg>
          Clear
        </button>
      </div>
    </div>
    <div id="output"></div>
  </div>

  <div class="stats">
    <div class="stat-card">
      <h4>Model Information</h4>
      <div class="stat-value" id="model-name">-</div>
      <div class="stat-desc">Current model in use</div>
    </div>
    
    <div class="stat-card">
      <h4>Generation Time</h4>
      <div class="stat-value" id="gen-time">-</div>
      <div class="stat-desc">Total processing time</div>
    </div>
    
    <div class="stat-card">
      <h4>Tokens Generated</h4>
      <div class="stat-value" id="tokens">-</div>
      <div class="stat-desc">Words processed</div>
    </div>
    
    <div class="stat-card">
      <h4>Throughput</h4>
      <div class="stat-value" id="throughput">-</div>
      <div class="stat-desc">Tokens per second</div>
    </div>
  </div>

  <footer>
    <p>Ollama Web Application v2.0 | Connected to: <span id="connected-endpoint">http://127.0.0.1:11434</span></p>
  </footer>

  <!-- Model Info Modal -->
  <div id="modelModal" class="modal">
    <div class="modal-content">
      <div class="modal-header">
        <h3>Model Information</h3>
        <span class="close">&times;</span>
      </div>
      <div class="modal-body">
        <pre id="model-info-content">Loading model information...</pre>
      </div>
    </div>
  </div>

  <script>
    // Set status to error (red)
    function setStatusError() {
      document.querySelector('.status-indicator').style.background = 'var(--danger)';
    }
    
    // Set status to success (green)
    function setStatusSuccess() {
      document.querySelector('.status-indicator').style.background = 'var(--success)';
    }

    // Set status to warning (yellow)
    function setStatusWarning() {
      document.querySelector('.status-indicator').style.background = 'var(--warning)';
    }

    const promptTemplates = {
      coding: "Write a Python function to calculate fibonacci numbers:\n\n```python\ndef fibonacci(n):\n    # Your code here\n```",
      explanation: "Explain the following concept in simple terms:\n\nConcept: [ENTER CONCEPT HERE]",
      creative: "Write a short story about:\n\nTopic: [ENTER STORY TOPIC HERE]"
    };

    const ui = {
      endpoint: document.getElementById("endpoint"),
      refresh: document.getElementById("refresh"),
      pull: document.getElementById("pull"),
      pullname: document.getElementById("pullname"),
      model: document.getElementById("model"),
      prompt: document.getElementById("prompt"),
      run: document.getElementById("run"),
      cancel: document.getElementById("cancel"),
      output: document.getElementById("output"),
      status: document.getElementById("status"),
      copy: document.getElementById("copy"),
      clear: document.getElementById("clear"),
      modelName: document.getElementById("model-name"),
      genTime: document.getElementById("gen-time"),
      tokens: document.getElementById("tokens"),
      throughput: document.getElementById("throughput"),
      connectedEndpoint: document.getElementById("connected-endpoint"),
      templates: document.getElementById("templates"),
      modelInfo: document.getElementById("model-info"),
      modelModal: document.getElementById("modelModal"),
      modelInfoContent: document.getElementById("model-info-content"),
      modalClose: document.querySelector(".close")
    };

    let abortController = null;
    let saveTimeout = null;

    // Update connected endpoint in footer
    function updateConnectedEndpoint() {
      ui.connectedEndpoint.textContent = ui.endpoint.value;
    }

    // Enhanced API call handler
    async function handleApiCall(url, options = {}) {
      try {
        const response = await fetch(url, options);
        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        return response;
      } catch (error) {
        ui.status.textContent = `Connection error: ${error.message}`;
        setStatusError();
        throw error;
      }
    }

    // Helper function to fetch and parse JSON
    async function fetchJSON(url, options = {}) {
      const response = await handleApiCall(url, options);
      return await response.json();
    }

    // Improved stream processor
    async function processStream(response, callback) {
      const reader = response.body.getReader();
      const decoder = new TextDecoder();
      let buffer = '';
      
      try {
        while (true) {
          const { done, value } = await reader.read();
          if (done) break;
          
          buffer += decoder.decode(value, { stream: true });
          const lines = buffer.split('\n');
          
          // Keep the last incomplete line in buffer
          buffer = lines.pop() || '';
          
          for (const line of lines) {
            const trimmedLine = line.trim();
            if (trimmedLine) {
              try {
                const data = JSON.parse(trimmedLine);
                callback(data);
              } catch (e) {
                // Skip malformed JSON
              }
            }
          }
        }
        
        // Process any remaining content in buffer
        if (buffer.trim()) {
          try {
            const data = JSON.parse(buffer.trim());
            callback(data);
          } catch (e) {
            // Skip malformed JSON
          }
        }
      } finally {
        reader.releaseLock();
      }
    }

    // Fetch available models
    async function fetchModels() {
      ui.status.textContent = "Loading models...";
      setStatusWarning();
      try {
        const data = await fetchJSON(`${ui.endpoint.value}/api/tags`);
        ui.model.innerHTML = "";
        
        if (data.models && data.models.length > 0) {
          data.models.forEach(m => {
            const opt = document.createElement('option');
            opt.value = m.name;
            opt.textContent = m.name;
            ui.model.appendChild(opt);
          });
          
          // Try to select the last used model
          const settings = loadSettings();
          if (settings.lastModel && data.models.some(m => m.name === settings.lastModel)) {
            ui.model.value = settings.lastModel;
          } else if (data.models.length > 0) {
            ui.model.value = data.models[0].name;
          }
          
          ui.status.textContent = `Loaded ${data.models.length} models`;
          setStatusSuccess();
        } else {
          ui.status.textContent = "No models found";
          setStatusError();
          ui.model.innerHTML = '<option value="">No models available</option>';
        }
      } catch (error) {
        ui.status.textContent = `Error loading models: ${error.message}`;
        setStatusError();
        ui.model.innerHTML = '<option value="">Error loading models</option>';
        console.error("Error fetching models:", error);
      }
    }

    // Pull a new model
    async function pullModel() {
      const name = ui.pullname.value.trim();
      if (!name) return alert("Please enter a model name");
      
      ui.status.textContent = `Pulling model: ${name}...`;
      ui.output.textContent = "";
      setStatusWarning();

      try {
        const response = await fetch(`${ui.endpoint.value}/api/pull`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ name })
        });

        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        ui.output.textContent = "Pulling model...\n\n";
        
        await processStream(response, (data) => {
          if (data.status) {
            ui.output.textContent += `${data.status}\n`;
          } else if (data.error) {
            ui.output.textContent += `Error: ${data.error}\n`;
          } else if (data.completed) {
            ui.output.textContent += "Pull completed successfully!\n";
            ui.status.textContent = `Model ${name} pulled successfully`;
            setStatusSuccess();
            fetchModels();
          }
        });

      } catch (error) {
        ui.status.textContent = `Error pulling model: ${error.message}`;
        setStatusError();
        console.error("Error pulling model:", error);
      }
    }

    // Run the prompt
    async function runPrompt() {
      const prompt = ui.prompt.value.trim();
      if (!prompt) return alert("Please enter a prompt");
      
      const model = ui.model.value;
      if (!model) return alert("Please select a model");
      
      // Cancel previous request if exists
      if (abortController) {
        abortController.abort();
      }
      
      abortController = new AbortController();
      ui.status.textContent = "Generating response...";
      ui.run.style.display = "none";
      ui.cancel.style.display = "flex";
      ui.output.textContent = "";
      setStatusWarning();

      // Reset stats
      ui.modelName.textContent = "-";
      ui.genTime.textContent = "-";
      ui.tokens.textContent = "-";
      ui.throughput.textContent = "-";

      const startTime = Date.now();
      let tokenCount = 0;
      let responseText = "";

      try {
        const body = { 
          model: model, 
          prompt: prompt, 
          stream: true 
        };
        
        const response = await fetch(`${ui.endpoint.value}/api/generate`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(body),
          signal: abortController.signal
        });

        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        ui.output.textContent = "";
        
        await processStream(response, (data) => {
          if (data.response) {
            responseText += data.response;
            ui.output.textContent = responseText;
            tokenCount = data.eval_count || responseText.split(/\s+/).length;
            
            // Smooth scrolling to bottom
            ui.output.scrollTop = ui.output.scrollHeight;
          }
          
          if (data.done) {
            const endTime = Date.now();
            const duration = (endTime - startTime) / 1000;
            const tokensPerSecond = data.eval_count ? Math.round(data.eval_count / duration) : Math.round(tokenCount / duration);

            // Update stats
            ui.modelName.textContent = model;
            ui.genTime.textContent = `${duration.toFixed(2)}s`;
            ui.tokens.textContent = data.eval_count || tokenCount;
            ui.throughput.textContent = tokensPerSecond;

            ui.status.textContent = "Response generated successfully";
            setStatusSuccess();
          }
        });

      } catch (error) {
        if (error.name === 'AbortError') {
          ui.status.textContent = "Generation cancelled";
          setStatusWarning();
        } else {
          ui.status.textContent = `Error generating response: ${error.message}`;
          setStatusError();
          console.error("Error running prompt:", error);
        }
      } finally {
        ui.run.style.display = "flex";
        ui.cancel.style.display = "none";
        abortController = null;
      }
    }

    // Cancel generation
    function cancelGeneration() {
      if (abortController) {
        abortController.abort();
      }
    }

    // Copy output to clipboard
    function copyOutput() {
      navigator.clipboard.writeText(ui.output.textContent)
        .then(() => {
          const originalText = ui.copy.innerHTML;
          ui.copy.innerHTML = '<svg width="16" height="16" viewBox="0 0 24 24" fill="none"><path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41L9 16.17z" fill="currentColor"/></svg> Copied!';
          setTimeout(() => {
            ui.copy.innerHTML = originalText;
          }, 2000);
        })
        .catch(err => {
          console.error('Failed to copy: ', err);
          alert('Failed to copy text to clipboard');
        });
    }

    // Session management
    function saveSettings() {
      const settings = {
        endpoint: ui.endpoint.value,
        lastModel: ui.model.value,
        lastPrompt: ui.prompt.value
      };
      localStorage.setItem('ollamaSettings', JSON.stringify(settings));
    }

    function loadSettings() {
      const saved = localStorage.getItem('ollamaSettings');
      return saved ? JSON.parse(saved) : {};
    }

    // Model information modal
    async function showModelInfo() {
      const modelName = ui.model.value;
      if (!modelName) {
        alert("Please select a model first");
        return;
      }
      
      ui.modelModal.style.display = "block";
      ui.modelInfoContent.textContent = "Loading model information...";
      
      try {
        const data = await fetchJSON(`${ui.endpoint.value}/api/show`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ name: modelName })
        });
        
        ui.modelInfoContent.textContent = JSON.stringify(data, null, 2);
      } catch (error) {
        ui.modelInfoContent.textContent = `Error: ${error.message}`;
        console.error("Error fetching model info:", error);
      }
    }

    // Close modal
    function closeModal() {
      ui.modelModal.style.display = "none";
    }

    // Initialize the application
    function init() {
      // Load saved settings
      const settings = loadSettings();
      if (settings.endpoint) ui.endpoint.value = settings.endpoint;
      if (settings.lastPrompt) ui.prompt.value = settings.lastPrompt;
      
      updateConnectedEndpoint();
      fetchModels();
      
      // Auto-save settings
      function debouncedSave() {
        clearTimeout(saveTimeout);
        saveTimeout = setTimeout(saveSettings, 1000);
      }
      
      ui.prompt.addEventListener('input', debouncedSave);
      ui.endpoint.addEventListener('change', () => {
        updateConnectedEndpoint();
        debouncedSave();
        fetchModels(); // Refresh models when endpoint changes
      });
      ui.model.addEventListener('change', debouncedSave);
      
      // Template selector
      ui.templates.addEventListener('change', (e) => {
        if (e.target.value && promptTemplates[e.target.value]) {
          ui.prompt.value = promptTemplates[e.target.value];
        }
      });
    }

    // Event listeners
    ui.refresh.addEventListener('click', fetchModels);
    ui.pull.addEventListener('click', pullModel);
    ui.run.addEventListener('click', runPrompt);
    ui.cancel.addEventListener('click', cancelGeneration);
    ui.copy.addEventListener('click', copyOutput);
    ui.clear.addEventListener('click', () => {
      ui.output.textContent = "";
      ui.status.textContent = "Output cleared";
      setStatusSuccess();
    });
    ui.modelInfo.addEventListener('click', showModelInfo);
    ui.modalClose.addEventListener('click', closeModal);

    // Close modal when clicking outside
    window.addEventListener('click', (event) => {
      if (event.target === ui.modelModal) {
        closeModal();
      }
    });

    // Initialize the application
    document.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>
