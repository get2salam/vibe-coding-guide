# 🧩 Example: Chrome Extension

Building a productivity Chrome extension with vibe coding.

---

## The Goal

Build a Chrome extension that helps block distracting websites during focus time.

---

## Session 1: Basic Extension

### User

```
Build a Chrome extension called "Focus Mode" that:
- Has a popup with on/off toggle
- When on, blocks configurable list of websites
- Shows a "Get back to work!" page when blocked
- Tracks time saved

Use Manifest V3, TypeScript, and make the popup look nice with Tailwind.
```

### Claude

I'll create a complete Focus Mode extension.

```json
// manifest.json
{
  "manifest_version": 3,
  "name": "Focus Mode",
  "version": "1.0.0",
  "description": "Block distracting websites and stay focused",
  "permissions": [
    "storage",
    "declarativeNetRequest",
    "tabs"
  ],
  "host_permissions": ["<all_urls>"],
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "icons/icon16.png",
      "48": "icons/icon48.png",
      "128": "icons/icon128.png"
    }
  },
  "background": {
    "service_worker": "background.js",
    "type": "module"
  },
  "declarative_net_request": {
    "rule_resources": [{
      "id": "focus_rules",
      "enabled": true,
      "path": "rules.json"
    }]
  },
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  }
}
```

```typescript
// src/background.ts
interface FocusState {
  enabled: boolean;
  startTime: number | null;
  totalTimeSaved: number;
  blockedSites: string[];
}

const DEFAULT_BLOCKED = [
  'twitter.com',
  'facebook.com',
  'reddit.com',
  'youtube.com',
  'instagram.com',
  'tiktok.com',
];

chrome.runtime.onInstalled.addListener(async () => {
  const state: FocusState = {
    enabled: false,
    startTime: null,
    totalTimeSaved: 0,
    blockedSites: DEFAULT_BLOCKED,
  };
  await chrome.storage.local.set({ focusState: state });
  console.log('Focus Mode installed');
});

chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === 'TOGGLE_FOCUS') {
    handleToggleFocus().then(sendResponse);
    return true;
  }
  if (message.type === 'GET_STATE') {
    chrome.storage.local.get('focusState').then(({ focusState }) => {
      sendResponse(focusState);
    });
    return true;
  }
});

async function handleToggleFocus(): Promise<FocusState> {
  const { focusState } = await chrome.storage.local.get('focusState');
  const newState: FocusState = { ...focusState };
  
  if (focusState.enabled) {
    // Turning off - calculate time saved
    if (focusState.startTime) {
      const sessionTime = Date.now() - focusState.startTime;
      newState.totalTimeSaved += sessionTime;
    }
    newState.enabled = false;
    newState.startTime = null;
    await disableBlocking();
  } else {
    // Turning on
    newState.enabled = true;
    newState.startTime = Date.now();
    await enableBlocking(focusState.blockedSites);
  }
  
  await chrome.storage.local.set({ focusState: newState });
  return newState;
}

async function enableBlocking(sites: string[]) {
  const rules = sites.map((site, index) => ({
    id: index + 1,
    priority: 1,
    action: { 
      type: 'redirect' as const,
      redirect: { extensionPath: '/blocked.html' }
    },
    condition: {
      urlFilter: `*://*.${site}/*`,
      resourceTypes: ['main_frame' as const]
    }
  }));
  
  await chrome.declarativeNetRequest.updateDynamicRules({
    removeRuleIds: rules.map(r => r.id),
    addRules: rules,
  });
}

async function disableBlocking() {
  const rules = await chrome.declarativeNetRequest.getDynamicRules();
  await chrome.declarativeNetRequest.updateDynamicRules({
    removeRuleIds: rules.map(r => r.id),
  });
}
```

```html
<!-- popup.html -->
<!DOCTYPE html>
<html>
<head>
  <link href="styles.css" rel="stylesheet">
</head>
<body class="w-80 p-4 bg-gray-50">
  <div id="app"></div>
  <script src="popup.js" type="module"></script>
</body>
</html>
```

```typescript
// src/popup.ts
interface FocusState {
  enabled: boolean;
  startTime: number | null;
  totalTimeSaved: number;
  blockedSites: string[];
}

async function getState(): Promise<FocusState> {
  return new Promise((resolve) => {
    chrome.runtime.sendMessage({ type: 'GET_STATE' }, resolve);
  });
}

async function toggleFocus(): Promise<FocusState> {
  return new Promise((resolve) => {
    chrome.runtime.sendMessage({ type: 'TOGGLE_FOCUS' }, resolve);
  });
}

function formatTime(ms: number): string {
  const seconds = Math.floor(ms / 1000);
  const minutes = Math.floor(seconds / 60);
  const hours = Math.floor(minutes / 60);
  
  if (hours > 0) {
    return `${hours}h ${minutes % 60}m`;
  }
  if (minutes > 0) {
    return `${minutes}m ${seconds % 60}s`;
  }
  return `${seconds}s`;
}

async function render() {
  const state = await getState();
  const app = document.getElementById('app')!;
  
  const currentSession = state.enabled && state.startTime 
    ? Date.now() - state.startTime 
    : 0;
  
  app.innerHTML = `
    <div class="text-center">
      <h1 class="text-xl font-bold text-gray-800 mb-4">🎯 Focus Mode</h1>
      
      <button 
        id="toggle-btn"
        class="w-full py-3 px-4 rounded-lg font-semibold transition-colors ${
          state.enabled 
            ? 'bg-red-500 hover:bg-red-600 text-white' 
            : 'bg-blue-500 hover:bg-blue-600 text-white'
        }"
      >
        ${state.enabled ? '🛑 End Focus Session' : '▶️ Start Focus Mode'}
      </button>
      
      ${state.enabled ? `
        <div class="mt-4 p-3 bg-green-50 rounded-lg">
          <p class="text-sm text-green-600">Currently focusing</p>
          <p class="text-2xl font-bold text-green-700" id="timer">
            ${formatTime(currentSession)}
          </p>
        </div>
      ` : ''}
      
      <div class="mt-4 p-3 bg-gray-100 rounded-lg">
        <p class="text-sm text-gray-500">Total time saved</p>
        <p class="text-xl font-bold text-gray-700">
          ${formatTime(state.totalTimeSaved + currentSession)}
        </p>
      </div>
      
      <p class="mt-4 text-xs text-gray-400">
        Blocking ${state.blockedSites.length} distracting sites
      </p>
    </div>
  `;
  
  document.getElementById('toggle-btn')?.addEventListener('click', async () => {
    await toggleFocus();
    render();
  });
}

// Update timer every second when focusing
setInterval(async () => {
  const state = await getState();
  if (state.enabled) {
    const timerEl = document.getElementById('timer');
    if (timerEl && state.startTime) {
      timerEl.textContent = formatTime(Date.now() - state.startTime);
    }
  }
}, 1000);

render();
```

```html
<!-- blocked.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Blocked - Focus Mode</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    }
    .container {
      text-align: center;
      color: white;
      padding: 2rem;
    }
    .emoji { font-size: 80px; margin-bottom: 1rem; }
    h1 { font-size: 2.5rem; margin-bottom: 0.5rem; }
    p { font-size: 1.2rem; opacity: 0.9; margin-bottom: 2rem; }
    .quote {
      font-style: italic;
      opacity: 0.8;
      max-width: 400px;
      margin: 0 auto;
    }
    button {
      margin-top: 2rem;
      padding: 12px 24px;
      font-size: 1rem;
      background: rgba(255,255,255,0.2);
      color: white;
      border: 2px solid white;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.2s;
    }
    button:hover { background: rgba(255,255,255,0.3); }
  </style>
</head>
<body>
  <div class="container">
    <div class="emoji">🎯</div>
    <h1>Stay Focused!</h1>
    <p>This site is blocked during your focus session.</p>
    <p class="quote">"The successful warrior is the average man, with laser-like focus."</p>
    <button onclick="window.close()">Close Tab</button>
  </div>
</body>
</html>
```

---

### User

```
Nice! Now add:
1. Settings page to configure blocked sites
2. Statistics page showing daily/weekly focus time
3. Break timer option (e.g., 25 min focus, 5 min break like Pomodoro)
4. Desktop notifications
```

### Claude

```typescript
// src/settings.ts
// Settings page with site management

async function loadSettings() {
  const { focusState } = await chrome.storage.local.get('focusState');
  return focusState;
}

async function saveBlockedSites(sites: string[]) {
  const { focusState } = await chrome.storage.local.get('focusState');
  await chrome.storage.local.set({
    focusState: { ...focusState, blockedSites: sites }
  });
}

function renderSettings() {
  // Site list management UI
  // Add/remove sites
  // Import/export functionality
}
```

```typescript
// src/statistics.ts
interface DailyStats {
  date: string;
  focusTime: number;
  sessions: number;
  blockedAttempts: number;
}

async function getStats(): Promise<DailyStats[]> {
  const { stats = [] } = await chrome.storage.local.get('stats');
  return stats;
}

function renderChart(stats: DailyStats[]) {
  // Weekly bar chart using Chart.js or simple SVG
  // Show streaks
  // Show best day
}
```

```typescript
// src/pomodoro.ts
const FOCUS_DURATION = 25 * 60 * 1000; // 25 minutes
const BREAK_DURATION = 5 * 60 * 1000;  // 5 minutes

interface PomodoroState {
  mode: 'focus' | 'break' | 'idle';
  endTime: number | null;
  completedPomodoros: number;
}

async function startPomodoro() {
  const state: PomodoroState = {
    mode: 'focus',
    endTime: Date.now() + FOCUS_DURATION,
    completedPomodoros: 0,
  };
  
  await chrome.storage.local.set({ pomodoroState: state });
  await enableBlocking();
  
  // Set alarm for when focus period ends
  chrome.alarms.create('pomodoroEnd', {
    when: state.endTime
  });
}

chrome.alarms.onAlarm.addListener(async (alarm) => {
  if (alarm.name === 'pomodoroEnd') {
    const { pomodoroState } = await chrome.storage.local.get('pomodoroState');
    
    if (pomodoroState.mode === 'focus') {
      // Focus ended, start break
      chrome.notifications.create({
        type: 'basic',
        iconUrl: 'icons/icon128.png',
        title: '🎉 Focus Complete!',
        message: 'Great work! Take a 5-minute break.',
      });
      
      await chrome.storage.local.set({
        pomodoroState: {
          mode: 'break',
          endTime: Date.now() + BREAK_DURATION,
          completedPomodoros: pomodoroState.completedPomodoros + 1,
        }
      });
      
      await disableBlocking();
      chrome.alarms.create('pomodoroEnd', {
        when: Date.now() + BREAK_DURATION
      });
      
    } else if (pomodoroState.mode === 'break') {
      // Break ended
      chrome.notifications.create({
        type: 'basic',
        iconUrl: 'icons/icon128.png',
        title: '⏰ Break Over',
        message: 'Ready to focus again?',
      });
      
      await chrome.storage.local.set({
        pomodoroState: { mode: 'idle', endTime: null }
      });
    }
  }
});
```

---

## Build & Package

```json
// package.json
{
  "name": "focus-mode-extension",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "package": "npm run build && cd dist && zip -r ../focus-mode.zip *"
  },
  "devDependencies": {
    "@types/chrome": "^0.0.250",
    "typescript": "^5.0.0",
    "vite": "^5.0.0",
    "tailwindcss": "^3.0.0"
  }
}
```

---

## Key Takeaways

1. **Started with core functionality** - Toggle, blocking, basic UI
2. **Used modern APIs** - Manifest V3, declarativeNetRequest
3. **Added features incrementally** - Settings, stats, Pomodoro
4. **Polished UX** - Nice blocked page, notifications, timer
