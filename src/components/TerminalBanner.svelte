<script>
  // Danh sách các bước: mỗi bước là { type: 'cmd' | 'result', text: string }
  export let steps = [
    { type: 'cmd', text: 'id' },
    { type: 'result', text: 'root at here :)' },
    { type: 'cmd', text: 'ls' },
    { type: 'result', text: 'welcome' },
    { type: 'cmd', text: './welcome' },
    { type: 'result', text: 'Enjoy with me' },
  ];

  let displayedLines = [];
  let typing = '';
  let showCursor = true;
  let currentTime = new Date().toLocaleString();
  let isTyping = false;
  let stepIndex = 0;

  async function typeTerminal() {
    // Delay khởi tạo cho smooth hơn
    await new Promise(r => setTimeout(r, 1000));
    
    for (let i = 0; i < steps.length; i++) {
      if (steps[i].type === 'cmd') {
        typing = '';
        isTyping = true;
        showCursor = true;
        
        // Delay êm hơn trước khi bắt đầu gõ
        await new Promise(r => setTimeout(r, 300));
        
        // Gõ từng ký tự với tốc độ mượt mà và tự nhiên hơn
        for (let j = 0; j < steps[i].text.length; j++) {
          typing += steps[i].text[j];
          
          // Tốc độ gõ mượt mà và tự nhiên (80-150ms)
          let delay = 80 + Math.random() * 70;
          
          // Thêm hiệu ứng ngừng ngắn tại space và dấu câu
          if (steps[i].text[j] === ' ') {
            delay += 50;
          } else if (['.', '/', '-', '_'].includes(steps[i].text[j])) {
            delay += 30;
          }
          
          await new Promise(r => setTimeout(r, delay));
        }
        
        // Dừng lại một chút sau khi gõ xong để tạo cảm giác tự nhiên
        await new Promise(r => setTimeout(r, 200));
        
        // Gõ xong thì tắt cursor và isTyping trước khi push vào displayedLines
        isTyping = false;
        showCursor = false;
        
        // Push dòng lệnh vào displayedLines
        displayedLines = [...displayedLines, { type: 'cmd', text: typing }];
        typing = '';
        
        // Delay nhẹ nhàng trước khi hiện kết quả
        await new Promise(r => setTimeout(r, 400));
        
      } else {
        // Kết quả hiện ra với hiệu ứng slide in mượt mà
        displayedLines = [...displayedLines, { type: 'result', text: steps[i].text }];
        
        // Delay êm hơn trước lệnh tiếp theo
        await new Promise(r => setTimeout(r, 800 + Math.random() * 400));
      }
      stepIndex = i + 1;
    }
    
    // Sau khi hoàn thành, restart animation
    // await new Promise(r => setTimeout(r, 3000));
    // restartAnimation();
  }

  function updateTime() {
    currentTime = new Date().toLocaleString();
  }

  // Bắt đầu animation
  typeTerminal();
  setInterval(updateTime, 1000);
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&display=swap');
  
  .terminal-container {
    max-width: 800px;
    width: 100%;
    margin: 0 auto;
    position: relative;
  }

  .terminal-window {
    background: 
      linear-gradient(135deg, rgba(13, 17, 23, 0.95) 0%, rgba(1, 4, 9, 0.98) 100%);
    border: 1px solid #30363d;
    border-radius: 12px;
    box-shadow: 
      0 16px 32px rgba(0, 0, 0, 0.6),
      0 0 0 1px rgba(255, 255, 255, 0.05),
      inset 0 1px 0 rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    overflow: hidden;
    position: relative;
  }

  .glow-effect {
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(0, 255, 100, 0.1) 0%, transparent 70%);
    animation: rotate 20s linear infinite;
    pointer-events: none;
  }

  @keyframes rotate {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .terminal-header {
    background: linear-gradient(135deg, #21262d 0%, #161b22 100%);
    padding: 12px 20px;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 1px solid #30363d;
    position: relative;
  }

  .terminal-buttons {
    display: flex;
    gap: 8px;
    align-items: center;
  }

  .terminal-dot {
    width: 13px;
    height: 13px;
    border-radius: 50%;
    position: relative;
    transition: all 0.3s ease;
  }

  .terminal-dot::before {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    opacity: 0;
    transition: all 0.3s ease;
  }

  .terminal-dot:hover::before {
    opacity: 0.3;
    transform: translate(-50%, -50%) scale(1.5);
  }

  .terminal-dot.red { 
    background: linear-gradient(135deg, #ff5f56 0%, #e0443e 100%);
    box-shadow: 0 2px 4px rgba(255, 95, 86, 0.3);
  }
  .terminal-dot.red::before { background: #ff5f56; }

  .terminal-dot.yellow { 
    background: linear-gradient(135deg, #ffbd2e 0%, #dea123 100%);
    box-shadow: 0 2px 4px rgba(255, 189, 46, 0.3);
  }
  .terminal-dot.yellow::before { background: #ffbd2e; }

  .terminal-dot.green { 
    background: linear-gradient(135deg, #27c93f 0%, #13a10e 100%);
    box-shadow: 0 2px 4px rgba(39, 201, 63, 0.3);
  }
  .terminal-dot.green::before { background: #27c93f; }

  .terminal-title {
    color: #8b949e;
    font-size: 13px;
    font-weight: 500;
    margin-left: 12px;
    opacity: 0.8;
  }

  .terminal-content {
    padding: 24px;
    min-height: 280px;
    background: 
      radial-gradient(circle at 10% 20%, rgba(0, 255, 100, 0.02) 0%, transparent 50%),
      radial-gradient(circle at 90% 80%, rgba(0, 255, 100, 0.02) 0%, transparent 50%);
    position: relative;
    overflow: hidden;
    font-family: 'JetBrains Mono', 'Fira Mono', monospace;
  }

  .terminal-content::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: 
      repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0, 255, 100, 0.02) 2px,
        rgba(0, 255, 100, 0.02) 4px
      );
    pointer-events: none;
    animation: scanlines 0.1s linear infinite;
  }

  @keyframes scanlines {
    0% { transform: translateY(0px); }
    100% { transform: translateY(4px); }
  }

  .terminal-output {
    position: relative;
    z-index: 1;
  }

  .terminal-line {
    color: #f8fafc;
    font-size: 15px;
    line-height: 1.8;
    margin-bottom: 4px;
    font-weight: 500;
    font-family: 'JetBrains Mono', 'Fira Mono', monospace;
    display: flex;
    align-items: baseline;
    min-height: 27px; /* Cố định chiều cao để tránh nhấp nháy */
  }
  
  .terminal-line.fade-in {
    opacity: 0;
    animation: smoothSlideIn 0.6s cubic-bezier(0.23, 1, 0.32, 1) forwards;
  }
  
  @keyframes smoothSlideIn {
    0% {
      opacity: 0;
      transform: translateX(-15px) translateY(5px);
    }
    60% {
      opacity: 0.8;
      transform: translateX(2px) translateY(0px);
    }
    100% {
      opacity: 1;
      transform: translateX(0) translateY(0);
    }
  }

  .terminal-prompt {
    color: #ff6b6b;
    font-weight: 600;
    text-shadow: 0 0 6px rgba(255, 107, 107, 0.4);
    font-family: inherit;
    white-space: nowrap;
    flex-shrink: 0; /* Không cho phép co lại */
  }

  .terminal-cmd {
    color: #ffffff;
    font-weight: 500;
    font-family: inherit;
    text-shadow: 0 0 2px rgba(255, 255, 255, 0.3);
    margin-left: 4px;
    display: inline-block;
    min-width: 1px;
    transition: all 0.1s ease-out;
  }

  .terminal-result {
    color: #f8fafc;
    font-weight: 400;
    text-shadow: 0 0 2px rgba(0, 255, 100, 0.2);
    font-family: inherit;
    opacity: 0.9;
    flex: 1;
    transition: all 0.3s ease-out;
  }

  .cursor {
    display: inline-block;
    width: 2px;
    height: 18px;
    background: linear-gradient(135deg, #ffffff 0%, #e0e0e0 100%);
    animation: smoothBlink 1.5s ease-in-out infinite;
    box-shadow: 0 0 8px rgba(255, 255, 255, 0.4);
    margin-left: 2px;
    flex-shrink: 0;
    vertical-align: middle;
    border-radius: 1px;
  }

  @keyframes smoothBlink {
    0%, 45% { 
      opacity: 1; 
      transform: scaleY(1);
    }
    50%, 95% { 
      opacity: 0.3; 
      transform: scaleY(0.95);
    }
    100% { 
      opacity: 1; 
      transform: scaleY(1);
    }
  }

  .status-bar {
    background: linear-gradient(135deg, #161b22 0%, #0d1117 100%);
    padding: 8px 20px;
    border-top: 1px solid #30363d;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    color: #7c8b95;
    font-family: 'JetBrains Mono', 'Fira Mono', monospace;
  }

  .status-item {
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .status-indicator {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #00ff64;
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { 
      opacity: 1; 
      transform: scale(1);
      box-shadow: 0 0 0 0 rgba(0, 255, 100, 0.7);
    }
    50% { 
      opacity: 0.7; 
      transform: scale(0.9);
      box-shadow: 0 0 0 4px rgba(0, 255, 100, 0);
    }
  }

  .terminal-output {
    scroll-behavior: smooth;
  }

  /* Container cho command line để tránh layout shift */
  .cmd-container {
    display: flex;
    align-items: baseline;
    width: 100%;
    min-height: 27px;
  }

  /* Tối ưu hóa typing effect */
  .typing-container {
    display: flex;
    align-items: baseline;
    flex: 1;
  }

  .typing-char {
    display: inline-block;
    opacity: 0;
    transform: translateY(-2px);
    animation: typeChar 0.2s ease-out forwards;
  }

  @keyframes typeChar {
    0% {
      opacity: 0;
      transform: translateY(-2px) scale(0.8);
    }
    50% {
      opacity: 0.7;
      transform: translateY(0px) scale(1.1);
    }
    100% {
      opacity: 1;
      transform: translateY(0px) scale(1);
    }
  }

  /* Hiệu ứng gõ phím mượt mà */
  .terminal-cmd {
    overflow: hidden;
    white-space: nowrap;
  }

  .smooth-typing {
    animation: smoothType 0.1s ease-out;
  }

  @keyframes smoothType {
    0% {
      transform: translateX(-1px);
      text-shadow: 0 0 2px rgba(255, 255, 255, 0.8);
    }
    100% {
      transform: translateX(0);
      text-shadow: 0 0 2px rgba(255, 255, 255, 0.3);
    }
  }
</style>

<div class="terminal-container">
  <div class="terminal-window">
    <div class="glow-effect"></div>
    <div class="terminal-header">
      <div class="terminal-buttons">
        <div class="terminal-dot red"></div>
        <div class="terminal-dot yellow"></div>
        <div class="terminal-dot green"></div>
      </div>
      <div class="terminal-title">nd0@darknet: ~</div>
    </div>
    <div class="terminal-content">
      <div class="terminal-output">
        {#each displayedLines as line, i (i)}
          {#if line.type === 'cmd'}
            <div class="terminal-line">
              <span class="terminal-prompt">nd0@darknet:~#</span>
              <span class="terminal-cmd">{line.text}</span>
            </div>
          {:else}
            <div class="terminal-line fade-in">
              <span class="terminal-result">{line.text}</span>
            </div>
          {/if}
        {/each}
        {#if isTyping}
          <div class="terminal-line">
            <div class="cmd-container">
              <span class="terminal-prompt">nd0@darknet:~#</span>
              <div class="typing-container">
                <span class="terminal-cmd">{typing}</span>
                {#if showCursor}
                  <span class="cursor"></span>
                {/if}
              </div>
            </div>
          </div>
        {/if}
      </div>
    </div>
    <div class="status-bar">
      <div class="status-item">
        <span class="status-indicator"></span>
        <span>Online</span>
      </div>
      <div class="status-item">
        <span>Last login: {currentTime}</span>
      </div>
      <div class="status-item">
        <span>Shell: /bin/bash</span>
      </div>
    </div>
  </div>
</div>