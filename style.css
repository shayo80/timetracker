(function () {
  "use strict";

  const ENTRIES_KEY = "timetracker_entries";
  const ACTIVE_KEY = "timetracker_active";

  const pad = (n) => String(n).padStart(2, "0");

  function fmtDuration(ms) {
    const total = Math.max(0, Math.floor(ms / 1000));
    const h = Math.floor(total / 3600);
    const m = Math.floor((total % 3600) / 60);
    const s = total % 60;
    return `${pad(h)}:${pad(m)}:${pad(s)}`;
  }

  function fmtHours(ms) {
    return (ms / 3600000).toFixed(2);
  }

  function dateKey(d) {
    const dt = new Date(d);
    return `${dt.getFullYear()}-${pad(dt.getMonth() + 1)}-${pad(dt.getDate())}`;
  }

  function dateLabel(key) {
    const [y, m, d] = key.split("-").map(Number);
    const dt = new Date(y, m - 1, d);
    const days = ["ראשון", "שני", "שלישי", "רביעי", "חמישי", "שישי", "שבת"];
    return `יום ${days[dt.getDay()]}, ${pad(d)}/${pad(m)}/${y}`;
  }

  function timeStr(iso) {
    const dt = new Date(iso);
    return `${pad(dt.getHours())}:${pad(dt.getMinutes())}`;
  }

  function loadEntries() {
    try {
      const raw = localStorage.getItem(ENTRIES_KEY);
      return raw ? JSON.parse(raw) : [];
    } catch { return []; }
  }
  function saveEntries(list) {
    localStorage.setItem(ENTRIES_KEY, JSON.stringify(list));
  }
  function loadActive() {
    try {
      const raw = localStorage.getItem(ACTIVE_KEY);
      return raw ? JSON.parse(raw) : null;
    } catch { return null; }
  }
  function saveActive(val) {
    if (val) localStorage.setItem(ACTIVE_KEY, JSON.stringify(val));
    else localStorage.removeItem(ACTIVE_KEY);
  }

  let entries = loadEntries();
  let active = loadActive();
  let tickTimer = null;
  let editingId = null;

  const timerDisplay = document.getElementById("timerDisplay");
  const timerSub = document.getElementById("timerSub");
  const clockBtn = document.getElementById("clockBtn");
  const clockIcon = document.getElementById("clockIcon");
  const clockLabel = document.getElementById("clockLabel");
  const weekTotalEl = document.getElementById("weekTotal");
  const monthTotalEl = document.getElementById("monthTotal");
  const historyList = document.getElementById("historyList");
  const historyEmpty = document.getElementById("historyEmpty");
  const chartSvg = document.getElementById("chartSvg");
  const toastEl = document.getElementById("toast");
  const exportBtn = document.getElementById("exportBtn");

  function showToast(msg) {
    toastEl.textContent = msg;
    toastEl.classList.add("show");
    setTimeout(() => toastEl.classList.remove("show"), 1800);
  }

  function startTick() {
    stopTick();
    tickTimer = setInterval(renderTimer, 1000);
  }
  function stopTick() {
    if (tickTimer) clearInterval(tickTimer);
    tickTimer = null;
  }

  function renderTimer() {
    if (active) {
      const elapsed = Date.now() - new Date(active.start).getTime();
      timerDisplay.textContent = fmtDuration(elapsed);
      timerDisplay.classList.add("active");
      timerSub.textContent = `החל ב-${timeStr(active.start)}`;
      clockBtn.className = "clock-btn clock-out";
      clockIcon.innerHTML = '<rect x="5" y="5" width="14" height="14"/>';
      clockLabel.textContent = "סיום משמרת";
    } else {
      timerDisplay.textContent = "00:00:00";
      timerDisplay.classList.remove("active");
      timerSub.textContent = "השעון אינו פעיל";
      clockBtn.className = "clock-btn clock-in";
      clockIcon.innerHTML = '<polygon points="5 3 19 12 5 21 5 3"/>';
      clockLabel.textContent = "התחלת משמרת";
    }
  }

  function clockIn() {
    if (active) return;
    active = { start: new Date().toISOString() };
    saveActive(active);
    startTick();
    renderTimer();
    showToast("השעון הופעל");
  }

  function clockOut() {
    if (!active) return;
    const end = new Date().toISOString();
    const entry = { id: String(Date.now()), start: active.start, end };
    entries.unshift(entry);
    entries.sort((a, b) => new Date(b.start) - new Date(a.start));
    saveEntries(entries);
    active = null;
    saveActive(null);
    stopTick();
    renderTimer();
    renderStats();
    renderChart();
    renderHistory();
    showToast("נשמרה רשומה");
  }

  function renderStats() {
    const weekAgo = Date.now() - 7 * 86400000;
    const nowD = new Date();
    let week = 0, month = 0;
    for (const e of entries) {
      const dur = new Date(e.end) - new Date(e.start);
      const d = new Date(e.start);
      if (d.getTime() >= weekAgo) week += dur;
      if (d.getMonth() === nowD.getMonth() && d.getFullYear() === nowD.getFullYear()) month += dur;
    }
    weekTotalEl.textContent = fmtHours(week);
    monthTotalEl.textContent = fmtHours(month);
  }

  function renderChart() {
    const days = ["א", "ב", "ג", "ד", "ה", "ו", "ש"];
    const data = [];
    for (let i = 6; i >= 0; i--) {
      const d = new Date();
      d.setDate(d.getDate() - i);
      const key = dateKey(d);
      const total = entries
        .filter((e) => dateKey(e.start) === key)
        .reduce((s, e) => s + (new Date(e.end) - new Date(e.start)), 0);
      data.push({ label: days[d.getDay()], hours: total / 3600000 });
    }

    const W = 320, H = 160, padB = 24, padT = 10, padSide = 10;
    const maxVal = Math.max(1, ...data.map((d) => d.hours));
    const barW = (W - padSide * 2) / data.length;
    const innerH = H - padB - padT;

    let svg = "";
    // gridlines
    for (let i = 0; i <= 2; i++) {
      const y = padT + (innerH / 2) * i;
      svg += `<line x1="${padSide}" y1="${y}" x2="${W - padSide}" y2="${y}" stroke="#33383f" stroke-width="1" stroke-dasharray="3,3"/>`;
    }
    data.forEach((d, i) => {
      const barH = maxVal > 0 ? (d.hours / maxVal) * innerH : 0;
      const x = padSide + i * barW + barW * 0.2;
      const y = padT + innerH - barH;
      const w = barW * 0.6;
      svg += `<rect x="${x.toFixed(1)}" y="${y.toFixed(1)}" width="${w.toFixed(1)}" height="${Math.max(barH, 1).toFixed(1)}" rx="3" fill="#f5a623"><title>${d.label}: ${d.hours.toFixed(2)} שעות</title></rect>`;
      svg += `<text x="${(x + w / 2).toFixed(1)}" y="${H - 6}" text-anchor="middle" font-size="11" fill="#8b8f98" font-family="monospace">${d.label}</text>`;
    });
    chartSvg.innerHTML = svg;
  }

  function renderHistory() {
    const grouped = {};
    for (const e of entries) {
      const k = dateKey(e.start);
      if (!grouped[k]) grouped[k] = [];
      grouped[k].push(e);
    }
    const keys = Object.keys(grouped).sort((a, b) => (a < b ? 1 : -1));

    historyEmpty.style.display = entries.length === 0 ? "block" : "none";
    historyList.innerHTML = "";

    keys.forEach((key) => {
      const list = grouped[key];
      const dayTotal = list.reduce((s, e) => s + (new Date(e.end) - new Date(e.start)), 0);

      const group = document.createElement("div");
      group.className = "day-group";

      const header = document.createElement("div");
      header.className = "day-header";
      header.innerHTML = `<span class="day-name">${dateLabel(key)}</span><span class="day-total">${fmtHours(dayTotal)} שעות</span>`;
      group.appendChild(header);

      list.forEach((entry) => {
        const row = document.createElement("div");
        row.className = "entry-row";
        row.dataset.id = entry.id;

        if (editingId === entry.id) {
          const s = new Date(entry.start), en = new Date(entry.end);
          row.innerHTML = `
            <div class="edit-row">
              <input type="time" class="edit-start" value="${pad(s.getHours())}:${pad(s.getMinutes())}">
              <span style="color:var(--muted)">–</span>
              <input type="time" class="edit-end" value="${pad(en.getHours())}:${pad(en.getMinutes())}">
              <button class="save-btn" aria-label="שמור">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="20 6 9 17 4 12"/></svg>
              </button>
              <button class="cancel-btn" aria-label="בטל">
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
              </button>
            </div>`;
          row.querySelector(".save-btn").addEventListener("click", () => saveEdit(entry, row));
          row.querySelector(".cancel-btn").addEventListener("click", () => { editingId = null; renderHistory(); });
        } else {
          const dur = new Date(entry.end) - new Date(entry.start);
          row.innerHTML = `
            <div class="entry-time">
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="#8b8f98" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
              <span>${timeStr(entry.start)} – ${timeStr(entry.end)}</span>
            </div>
            <div class="entry-actions">
              <span class="entry-hours">${fmtHours(dur)}</span>
              <button class="edit-btn" aria-label="ערוך">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
              </button>
              <button class="del-btn" aria-label="מחק">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="3 6 5 6 21 6"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/><path d="M10 11v6"/><path d="M14 11v6"/><path d="M9 6V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v2"/></svg>
              </button>
            </div>`;
          row.querySelector(".edit-btn").addEventListener("click", () => { editingId = entry.id; renderHistory(); });
          row.querySelector(".del-btn").addEventListener("click", () => deleteEntry(entry.id));
        }
        group.appendChild(row);
      });

      historyList.appendChild(group);
    });
  }

  function saveEdit(entry, row) {
    const startVal = row.querySelector(".edit-start").value;
    const endVal = row.querySelector(".edit-end").value;
    const base = dateKey(entry.start);
    const [y, m, d] = base.split("-").map(Number);
    const [sh, sm] = startVal.split(":").map(Number);
    const [eh, em] = endVal.split(":").map(Number);
    const newStart = new Date(y, m - 1, d, sh, sm);
    let newEnd = new Date(y, m - 1, d, eh, em);
    if (newEnd < newStart) newEnd.setDate(newEnd.getDate() + 1);

    entries = entries.map((e) =>
      e.id === entry.id ? { ...e, start: newStart.toISOString(), end: newEnd.toISOString() } : e
    );
    saveEntries(entries);
    editingId = null;
    renderStats();
    renderChart();
    renderHistory();
    showToast("עודכן");
  }

  function deleteEntry(id) {
    entries = entries.filter((e) => e.id !== id);
    saveEntries(entries);
    renderStats();
    renderChart();
    renderHistory();
    showToast("נמחקה רשומה");
  }

  function exportCsv() {
    const rows = [["תאריך", "התחלה", "סיום", "שעות"]];
    entries.forEach((e) => {
      rows.push([dateKey(e.start), timeStr(e.start), timeStr(e.end), fmtHours(new Date(e.end) - new Date(e.start))]);
    });
    const csv = rows.map((r) => r.map((c) => `"${c}"`).join(",")).join("\n");
    const blob = new Blob(["\uFEFF" + csv], { type: "text/csv;charset=utf-8;" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "דיווח_שעות.csv";
    a.click();
    URL.revokeObjectURL(url);
  }

  clockBtn.addEventListener("click", () => (active ? clockOut() : clockIn()));
  exportBtn.addEventListener("click", exportCsv);

  // init
  renderTimer();
  if (active) startTick();
  renderStats();
  renderChart();
  renderHistory();

  // register service worker
  if ("serviceWorker" in navigator) {
    window.addEventListener("load", () => {
      navigator.serviceWorker.register("service-worker.js").catch(() => {});
    });
  }
})();
