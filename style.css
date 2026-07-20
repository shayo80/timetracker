:root {
  --bg: #1a1d23;
  --card: #242830;
  --border: #33383f;
  --text: #e8e6e1;
  --muted: #8b8f98;
  --muted-dim: #5b5f68;
  --amber: #f5a623;
  --green: #4ade80;
  --red: #ef4444;
}

* { box-sizing: border-box; }

body {
  margin: 0;
  background: var(--bg);
  color: var(--text);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Inter, system-ui, sans-serif;
  -webkit-font-smoothing: antialiased;
  min-height: 100vh;
  padding-bottom: env(safe-area-inset-bottom);
}

.app {
  max-width: 440px;
  margin: 0 auto;
  padding: 24px 16px 40px;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.eyebrow {
  font-size: 11px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--amber);
  font-family: ui-monospace, "SF Mono", Menlo, monospace;
}

h1 { font-size: 22px; font-weight: 700; margin: 2px 0 0; }

.icon-btn {
  background: var(--card);
  border: 1px solid var(--border);
  color: var(--muted);
  border-radius: 10px;
  padding: 8px;
  display: flex;
  cursor: pointer;
}
.icon-btn:hover { color: var(--amber); }

.card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 16px;
}

.punch-card {
  padding: 24px 20px;
  margin-bottom: 20px;
  position: relative;
  overflow: hidden;
  text-align: center;
}

.knob {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 48px; height: 48px;
  border-radius: 50%;
  background: var(--bg);
  border: 1px solid var(--border);
}
.knob-left { left: -24px; }
.knob-right { right: -24px; }

.timer {
  font-family: ui-monospace, "SF Mono", Menlo, monospace;
  font-size: 44px;
  font-weight: 700;
  color: var(--muted);
  font-variant-numeric: tabular-nums;
  letter-spacing: -0.02em;
}
.timer.active { color: var(--green); }

.timer-sub {
  font-size: 12px;
  color: var(--muted);
  margin-top: 8px;
  font-family: ui-monospace, monospace;
}

.clock-btn {
  width: 100%;
  margin-top: 20px;
  border: none;
  border-radius: 12px;
  padding: 14px;
  font-size: 14px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  cursor: pointer;
}
.clock-in { background: var(--amber); color: var(--bg); }
.clock-out { background: var(--red); color: #fff; }

.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin-bottom: 24px;
}

.stat-card { padding: 16px; }

.stat-label {
  font-size: 11px;
  color: var(--muted);
  font-family: ui-monospace, monospace;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 4px;
}

.stat-value {
  font-size: 20px;
  font-weight: 700;
  font-family: ui-monospace, monospace;
}
.stat-unit { font-size: 14px; color: var(--muted); font-weight: 400; }

.chart-card { padding: 16px; margin-bottom: 24px; }

.section-label {
  display: flex;
  align-items: center;
  gap: 8px;
  color: var(--muted);
  font-size: 12px;
  font-family: ui-monospace, monospace;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 12px;
}

.history-label { margin-bottom: 12px; }

.empty-state {
  text-align: center;
  padding: 40px 16px;
  color: var(--muted);
  font-size: 14px;
  border: 1px dashed var(--border);
  border-radius: 12px;
}

.day-group { margin-bottom: 16px; }

.day-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 4px;
  margin-bottom: 6px;
}
.day-header .day-name { font-size: 12px; color: var(--muted); }
.day-header .day-total { font-size: 12px; font-family: ui-monospace, monospace; color: var(--amber); }

.entry-row {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 12px;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.entry-time {
  display: flex;
  align-items: center;
  gap: 8px;
  font-family: ui-monospace, monospace;
  font-size: 14px;
}

.entry-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}

.entry-hours { font-size: 14px; font-family: ui-monospace, monospace; color: var(--amber); }

.entry-actions button {
  background: none;
  border: none;
  color: var(--muted);
  cursor: pointer;
  padding: 2px;
  display: flex;
}
.entry-actions button:hover { color: var(--text); }
.entry-actions .del-btn:hover { color: var(--red); }

.edit-row {
  display: flex;
  align-items: center;
  gap: 8px;
  width: 100%;
}
.edit-row input[type="time"] {
  flex: 1;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 6px 8px;
  color: var(--text);
  font-family: ui-monospace, monospace;
  font-size: 13px;
}
.edit-row button { background: none; border: none; cursor: pointer; padding: 4px; }
.save-btn { color: var(--green); }
.cancel-btn { color: var(--muted); }

.install-hint {
  text-align: center;
  font-size: 10px;
  color: var(--muted-dim);
  font-family: ui-monospace, monospace;
  margin-top: 32px;
}

.toast {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--card);
  border: 1px solid var(--border);
  color: var(--text);
  font-size: 14px;
  padding: 10px 18px;
  border-radius: 999px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.3);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s ease;
}
.toast.show { opacity: 1; }
