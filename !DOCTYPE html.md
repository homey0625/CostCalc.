<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>カフェ原価帳 — PHILOCOFFEA</title>
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<!-- ============================================================
  PHILOCOFFEA v4 — Supabase設定
  ここにSupabaseプロジェクトの情報を入力してください
  ============================================================ -->
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --bg: #FFFFFF;
  --bg-soft: #F7F7F7;
  --brand: #8C1324;
  --brand-dark: #6A0D1A;
  --text: #1A1A1A;
  --text-muted: #888888;
  --text-light: #BBBBBB;
  --border: #E5E5E5;
  --border-mid: #CCCCCC;
  --radius: 0px;
  --radius-sm: 0px;
  --serif: 'YuMincho', 'Hiragino Mincho ProN', 'Noto Serif JP', 'Georgia', serif;
  --sans: 'Hiragino Kaku Gothic Pro', 'Noto Sans JP', sans-serif;
  --green: #2E7D32;
  --green-bg: #E8F5E9;
  --green-light: #4CAF50;
}
body {
  background: #F0EEE8;
  font-family: var(--sans);
  color: var(--text);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 0 0 48px;
}
.app {
  background: var(--bg);
  width: 100%;
  max-width: 520px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 40px rgba(0,0,0,0.08);
  position: relative;
}

/* ── HEADER ── */
.header {
  background: var(--text);
  color: #fff;
  padding: 28px 24px 22px;
  text-align: center;
  position: relative;
  border-bottom: 3px solid var(--brand);
}
.header-brand {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 600;
  letter-spacing: 0.18em;
  color: #fff;
}
.header-sub {
  font-family: var(--sans);
  font-size: 9px;
  color: rgba(255,255,255,0.45);
  letter-spacing: 0.28em;
  margin-top: 5px;
}

/* ── TABS ── */
.tabs {
  display: flex;
  border-bottom: 1px solid var(--border);
  background: var(--bg);
  position: sticky;
  top: 0;
  z-index: 10;
}
.tab {
  flex: 1;
  padding: 16px 8px 14px;
  font-size: 11px;
  font-weight: 600;
  text-align: center;
  cursor: pointer;
  color: var(--text-muted);
  letter-spacing: 0.12em;
  border: none;
  background: transparent;
  font-family: var(--sans);
  border-bottom: 2px solid transparent;
  transition: all 0.2s;
}
.tab.active {
  color: var(--brand);
  border-bottom: 2px solid var(--brand);
}

/* ── CONTENT ── */
.content { padding: 28px 24px; flex: 1; }

/* ── EDIT MODE BANNER ── */
.edit-banner {
  display: none;
  background: var(--brand);
  color: #fff;
  padding: 12px 24px;
  font-size: 12px;
  font-family: var(--sans);
  font-weight: 600;
  letter-spacing: 0.08em;
  align-items: center;
  justify-content: space-between;
}
.edit-banner.active { display: flex; }
.edit-banner-text { display: flex; align-items: center; gap: 8px; }
.edit-banner-cancel {
  background: rgba(255,255,255,0.2);
  border: none;
  color: #fff;
  padding: 6px 14px;
  font-size: 11px;
  font-family: var(--sans);
  cursor: pointer;
  letter-spacing: 0.08em;
  transition: background 0.2s;
}
.edit-banner-cancel:hover { background: rgba(255,255,255,0.35); }

/* ── SECTION LABELS ── */
.slabel {
  font-family: var(--sans);
  font-size: 9px;
  font-weight: 700;
  color: var(--text-light);
  letter-spacing: 0.22em;
  margin-bottom: 10px;
  margin-top: 28px;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  gap: 8px;
}
.slabel:first-child { margin-top: 0; }
.slabel::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--border);
}

/* ── EXCEL IMPORT ── */
.excel-zone {
  border: 1px solid var(--border-mid);
  background: var(--bg-soft);
  padding: 18px 20px;
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 4px;
}
.excel-icon {
  width: 36px;
  height: 36px;
  background: var(--text);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.excel-icon svg { display: block; }
.excel-text { flex: 1; min-width: 0; }
.excel-text p { font-size: 11px; color: var(--text-muted); margin-bottom: 2px; font-family: var(--sans); }
.excel-status { font-size: 11px; font-weight: 600; color: var(--brand); font-family: var(--sans); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.excel-btn {
  background: var(--text);
  color: #fff;
  font-size: 11px;
  font-family: var(--sans);
  padding: 9px 14px;
  cursor: pointer;
  white-space: nowrap;
  letter-spacing: 0.06em;
  border: none;
  transition: background 0.2s;
}
.excel-btn:hover { background: var(--brand); }

/* ── CUSTOM SKU REGISTRATION ── */
.custom-sku-zone {
  border: 1px solid var(--border);
  background: var(--bg);
  padding: 16px;
  margin-top: 8px;
}
.custom-sku-toggle {
  font-size: 11px;
  color: var(--text-muted);
  cursor: pointer;
  background: none;
  border: none;
  font-family: var(--sans);
  letter-spacing: 0.06em;
  padding: 4px 0;
  transition: color 0.2s;
  display: flex;
  align-items: center;
  gap: 6px;
}
.custom-sku-toggle:hover { color: var(--brand); }
.custom-sku-toggle .arrow { font-size: 8px; transition: transform 0.2s; }
.custom-sku-toggle .arrow.open { transform: rotate(90deg); }
.custom-sku-form { display: none; margin-top: 12px; }
.custom-sku-form.open { display: block; }
.custom-sku-row {
  display: grid;
  grid-template-columns: 1fr 80px 80px;
  gap: 8px;
  margin-bottom: 8px;
}
.custom-sku-row input {
  width: 100%;
  background: var(--bg-soft);
  border: 1px solid var(--border);
  padding: 10px;
  font-size: 12px;
  font-family: var(--sans);
  color: var(--text);
  outline: none;
}
.custom-sku-row input:focus { border-color: var(--brand); background: #fff; }
.custom-sku-row input::placeholder { color: var(--text-light); }
.custom-sku-add-btn {
  width: 100%;
  padding: 10px;
  background: var(--text);
  border: none;
  color: #fff;
  font-size: 11px;
  font-family: var(--sans);
  font-weight: 600;
  letter-spacing: 0.1em;
  cursor: pointer;
  transition: background 0.2s;
}
.custom-sku-add-btn:hover { background: var(--brand); }
.custom-sku-list { margin-top: 10px; }
.custom-sku-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid var(--border);
  font-size: 11px;
  font-family: var(--sans);
  color: var(--text-muted);
}
.custom-sku-item:last-child { border-bottom: none; }
.custom-sku-item-name { flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; color: var(--text); }
.custom-sku-item-info { flex-shrink: 0; margin-left: 8px; }
.custom-sku-item-del {
  background: none; border: none; color: var(--text-light); cursor: pointer;
  font-size: 14px; padding: 0 0 0 8px; transition: color 0.2s;
}
.custom-sku-item-del:hover { color: var(--brand); }

/* ── MENU NAME ── */
.menu-name-input {
  width: 100%;
  background: var(--bg);
  border: 1px solid var(--border-mid);
  padding: 14px 16px;
  font-size: 15px;
  font-family: var(--serif);
  color: var(--text);
  outline: none;
  transition: border-color 0.2s;
}
.menu-name-input:focus { border-color: var(--brand); }
.menu-name-input::placeholder { color: var(--text-light); font-family: var(--sans); font-size: 13px; }

/* ── TYPE TOGGLE ── */
.type-toggle { display: flex; border: 1px solid var(--border-mid); }
.type-btn {
  flex: 1;
  padding: 12px;
  border: none;
  background: var(--bg);
  color: var(--text-muted);
  font-size: 11px;
  font-family: var(--sans);
  font-weight: 600;
  letter-spacing: 0.12em;
  cursor: pointer;
  transition: all 0.15s;
}
.type-btn + .type-btn { border-left: 1px solid var(--border-mid); }
.type-btn.active { background: var(--text); color: #fff; }

/* ── INGREDIENTS ── */
.ing-table { width: 100%; border-collapse: collapse; }
.ing-table th {
  font-size: 9px;
  font-family: var(--sans);
  font-weight: 600;
  color: var(--text-light);
  letter-spacing: 0.1em;
  text-align: left;
  padding: 0 0 10px 0;
  border-bottom: 1px solid var(--border);
}
.ing-table th:not(:first-child) { padding-left: 8px; }
.ing-row-wrap { position: relative; }
.ing-tr td { padding: 8px 0 0 0; vertical-align: top; }
.ing-tr td:not(:first-child) { padding-left: 8px; }
.field {
  width: 100%;
  background: var(--bg-soft);
  border: 1px solid var(--border);
  padding: 10px 10px;
  font-size: 12px;
  font-family: var(--sans);
  color: var(--text);
  outline: none;
  transition: border-color 0.2s;
  -webkit-appearance: none;
  appearance: none;
}
.field:focus { border-color: var(--brand); background: #fff; }
.field::placeholder { color: var(--text-light); }
.btn-del {
  background: none;
  border: none;
  color: var(--text-light);
  cursor: pointer;
  font-size: 18px;
  line-height: 1;
  padding: 10px 4px 0;
  transition: color 0.2s;
}
.btn-del:hover { color: var(--brand); }

/* ── SUGGEST DROPDOWN ── */
.suggest-wrap { position: relative; }
.suggest-list {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: #fff;
  border: 1px solid var(--border-mid);
  border-top: none;
  z-index: 100;
  max-height: 240px;
  overflow-y: auto;
  box-shadow: 0 8px 24px rgba(0,0,0,0.10);
}
.suggest-list.open { display: block; }
.suggest-item {
  padding: 0;
  cursor: pointer;
  border-bottom: 1px solid var(--border);
  transition: background 0.12s;
}
.suggest-item:last-child { border-bottom: none; }
.suggest-item:hover, .suggest-item.focused { background: var(--bg-soft); }
.suggest-header {
  font-size: 9px;
  font-family: var(--sans);
  font-weight: 700;
  color: var(--brand);
  letter-spacing: 0.16em;
  padding: 10px 12px 4px;
  background: #FDF5F6;
  border-bottom: 1px solid #F0D8DB;
}
.suggest-entry {
  padding: 10px 12px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.suggest-name {
  font-size: 12px;
  color: var(--text);
  font-family: var(--sans);
  line-height: 1.4;
}
.suggest-name mark { background: none; color: var(--brand); font-weight: 700; }
.suggest-meta {
  font-size: 10px;
  color: var(--text-muted);
  font-family: var(--sans);
}
.suggest-no-result {
  padding: 14px 12px;
  font-size: 12px;
  color: var(--text-muted);
  font-family: var(--sans);
}

/* ── ADD BUTTON ── */
.btn-add {
  width: 100%;
  padding: 12px;
  background: transparent;
  border: 1px dashed var(--border-mid);
  color: var(--text-muted);
  font-size: 11px;
  font-family: var(--sans);
  letter-spacing: 0.1em;
  cursor: pointer;
  margin-top: 12px;
  transition: all 0.2s;
}
.btn-add:hover { border-color: var(--brand); color: var(--brand); }

/* ── SETTINGS ── */
.settings-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.input-group { display: flex; flex-direction: column; gap: 6px; }
.input-group label {
  font-size: 9px;
  font-family: var(--sans);
  font-weight: 700;
  color: var(--text-muted);
  letter-spacing: 0.12em;
  text-transform: uppercase;
}
.input-group input, .input-group textarea {
  background: var(--bg-soft);
  border: 1px solid var(--border);
  padding: 12px;
  font-size: 14px;
  font-family: var(--sans);
  color: var(--text);
  width: 100%;
  outline: none;
  transition: border-color 0.2s;
  -webkit-appearance: none;
  appearance: none;
}
.input-group input:focus, .input-group textarea:focus { border-color: var(--brand); background: #fff; }
.divider { height: 1px; background: var(--border); margin: 16px 0; }

/* ── MEMO ── */
.memo-area {
  width: 100%;
  background: var(--bg-soft);
  border: 1px solid var(--border);
  padding: 12px;
  font-size: 13px;
  font-family: var(--sans);
  color: var(--text);
  outline: none;
  resize: vertical;
  min-height: 60px;
  transition: border-color 0.2s;
}
.memo-area:focus { border-color: var(--brand); background: #fff; }
.memo-area::placeholder { color: var(--text-light); }

/* ── RESULTS ── */
.result-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1px; background: var(--border); border: 1px solid var(--border); margin-top: 28px; }
.rcard { background: var(--bg); padding: 18px 16px; }
.rcard.wide { grid-column: 1 / -1; }
.rcard.dark { background: var(--text); }
.rcard.brand-bg { background: var(--brand); }
.rcard.green-bg { background: var(--green); }
.rlabel {
  font-size: 9px;
  font-family: var(--sans);
  font-weight: 700;
  color: var(--text-light);
  letter-spacing: 0.18em;
  text-transform: uppercase;
  margin-bottom: 8px;
}
.rcard.dark .rlabel { color: rgba(255,255,255,0.4); }
.rcard.brand-bg .rlabel { color: rgba(255,255,255,0.5); }
.rcard.green-bg .rlabel { color: rgba(255,255,255,0.5); }
.rvalue {
  font-size: 26px;
  font-weight: 700;
  font-family: var(--sans);
  color: var(--text);
  line-height: 1;
}
.rcard.dark .rvalue { color: #fff; }
.rcard.brand-bg .rvalue { color: #fff; }
.rcard.green-bg .rvalue { color: #fff; }
.runit { font-size: 12px; font-weight: 400; margin-left: 2px; }
.rsub {
  font-size: 10px;
  font-family: var(--sans);
  color: var(--text-muted);
  margin-top: 5px;
}
.rcard.dark .rsub { color: rgba(255,255,255,0.4); }
.rcard.brand-bg .rsub { color: rgba(255,255,255,0.6); }
.rcard.green-bg .rsub { color: rgba(255,255,255,0.6); }

/* ── PROFIT CARD ── */
.profit-positive { color: #fff; }
.profit-negative { color: #FFCDD2; }

/* ── PER-SERVING CARD ── */
.per-serving-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin-top: 8px;
}
.per-serving-item { text-align: center; }
.per-serving-label {
  font-size: 9px;
  color: rgba(255,255,255,0.5);
  font-family: var(--sans);
  letter-spacing: 0.1em;
  margin-bottom: 4px;
}
.per-serving-value {
  font-size: 20px;
  font-weight: 700;
  color: #fff;
  font-family: var(--sans);
}
.per-serving-value .runit { font-size: 11px; }

/* ── BUDGET BAR ── */
.budget-bar-track {
  background: rgba(255,255,255,0.2);
  height: 3px;
  margin: 14px 0 6px;
}
.budget-bar {
  height: 100%;
  background: #fff;
  transition: width 0.4s;
}
.budget-bar.over { background: var(--text); }
.budget-labels {
  display: flex;
  justify-content: space-between;
  font-size: 9px;
  font-family: var(--sans);
  color: rgba(255,255,255,0.55);
  letter-spacing: 0.06em;
}

/* ── SAVE BTN ── */
.save-btn {
  width: 100%;
  padding: 18px;
  background: var(--text);
  border: none;
  color: #fff;
  font-size: 12px;
  font-family: var(--sans);
  font-weight: 700;
  letter-spacing: 0.2em;
  cursor: pointer;
  margin-top: 28px;
  transition: background 0.2s;
  text-transform: uppercase;
}
.save-btn:hover { background: var(--brand); }
.save-btn.editing { background: var(--brand); }
.save-btn.editing:hover { background: var(--brand-dark); }

/* ── MENU LIST ── */
.menu-empty {
  text-align: center;
  padding: 60px 20px;
  color: var(--text-light);
  font-size: 13px;
  font-family: var(--serif);
  letter-spacing: 0.06em;
}
.menu-item {
  display: flex;
  align-items: stretch;
  border: 1px solid var(--border);
  margin-bottom: 12px;
  position: relative;
}
.menu-item-accent { width: 3px; background: var(--brand); flex-shrink: 0; }
.menu-item-body { flex: 1; padding: 14px 16px; min-width: 0; }
.menu-item-name {
  font-size: 14px;
  font-family: var(--serif);
  font-weight: 600;
  margin-bottom: 6px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-right: 68px;
}
.menu-item-memo {
  font-size: 10px;
  color: var(--text-muted);
  font-family: var(--sans);
  margin-bottom: 6px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.menu-item-stats { display: flex; gap: 12px; flex-wrap: wrap; }
.mstat { font-size: 10px; color: var(--text-muted); font-family: var(--sans); }
.mstat span { color: var(--brand); font-weight: 700; }
.menu-item-price { padding: 14px 16px; text-align: right; display: flex; flex-direction: column; justify-content: center; flex-shrink: 0; }
.menu-price-label { font-size: 9px; color: var(--text-light); font-family: var(--sans); letter-spacing: 0.1em; margin-bottom: 4px; }
.menu-price { font-size: 18px; font-weight: 700; font-family: var(--sans); color: var(--text); }

/* ── MENU ACTIONS ── */
.menu-item-actions {
  position: absolute;
  top: 8px;
  right: 8px;
  display: flex;
  gap: 4px;
}
.menu-action-btn {
  background: var(--bg-soft);
  border: 1px solid var(--border);
  color: var(--text-muted);
  width: 28px;
  height: 28px;
  cursor: pointer;
  font-size: 13px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.15s;
  font-family: var(--sans);
}
.menu-action-btn:hover { background: var(--brand); color: #fff; border-color: var(--brand); }
.menu-action-btn.del:hover { background: #D32F2F; border-color: #D32F2F; }

/* ── TOTAL BAR ── */
.total-bar {
  background: var(--text);
  padding: 16px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
.total-bar-label { font-size: 9px; color: rgba(255,255,255,0.4); font-family: var(--sans); letter-spacing: 0.12em; margin-bottom: 3px; }
.total-bar-value { font-size: 18px; font-weight: 700; color: #fff; font-family: var(--sans); }

/* ── CONFIRM DIALOG ── */
.confirm-overlay {
  display: none;
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.5);
  z-index: 1100;
  justify-content: center;
  align-items: center;
}
.confirm-overlay.open { display: flex; }
.confirm-box {
  background: var(--bg);
  width: 85%;
  max-width: 360px;
  padding: 28px 24px;
  text-align: center;
}
.confirm-box p {
  font-size: 13px;
  font-family: var(--sans);
  color: var(--text);
  margin-bottom: 20px;
  line-height: 1.6;
}
.modal-actions {
  display: flex;
  gap: 8px;
  margin-top: 20px;
}
.modal-btn {
  flex: 1;
  padding: 14px;
  font-size: 12px;
  font-family: var(--sans);
  font-weight: 700;
  letter-spacing: 0.1em;
  cursor: pointer;
  border: none;
  transition: background 0.2s;
}
.modal-btn-save { background: var(--text); color: #fff; }
.modal-btn-save:hover { background: var(--brand); }
.modal-btn-cancel { background: var(--bg-soft); color: var(--text-muted); border: 1px solid var(--border); }
.modal-btn-cancel:hover { background: var(--border); }

/* ── DB INDICATOR ── */
.db-indicator { position:absolute; top:10px; right:14px; display:flex; align-items:center; gap:5px; }
.db-dot { width:7px; height:7px; border-radius:50%; background:#666; transition:background .3s; }
.db-dot.online { background:#4CAF50; }
.db-dot.offline { background:#f44336; }
.db-dot.local { background:#FF9800; }
.db-status-banner { display:none; padding:10px 20px; font-size:11px; font-family:var(--sans); align-items:center; gap:8px; border-bottom:1px solid transparent; }
.db-status-banner.sb-success { background:#E8F5E9!important; border-color:#C8E6C9!important; color:#2E7D32!important; }
.db-status-banner.sb-error { background:#FFEBEE!important; border-color:#FFCDD2!important; color:#C62828!important; }
.db-status-banner.sb-warn { background:#FFF3CD!important; border-color:#FFE082!important; color:#795548!important; }

/* ── MIGRATION BANNER ── */
.migrate-banner { display:none; background:#E3F2FD; border-bottom:1px solid #BBDEFB; padding:12px 20px; font-size:12px; font-family:var(--sans); color:#1565C0; align-items:center; justify-content:space-between; gap:10px; }
.migrate-btn { background:#1565C0; color:#fff; border:none; padding:7px 14px; font-size:11px; font-family:var(--sans); font-weight:700; cursor:pointer; white-space:nowrap; letter-spacing:0.06em; }

/* ── HISTORY BUTTON ── */
.menu-action-btn.history:hover { background:#1565C0!important; border-color:#1565C0!important; color:#fff!important; }

/* ── CHANGE NOTE ROW ── */
.change-note-row { display:none; margin-top:14px; }
.change-note-row.visible { display:block; }
.change-note-row label { font-size:9px; font-family:var(--sans); font-weight:700; color:var(--text-muted); letter-spacing:0.12em; text-transform:uppercase; display:block; margin-bottom:6px; }
.change-note-row input { background:var(--bg-soft); border:1px solid var(--border); padding:10px 12px; font-size:12px; font-family:var(--sans); color:var(--text); width:100%; outline:none; transition:border-color .2s; }
.change-note-row input:focus { border-color:var(--brand); background:#fff; }

/* ── VERSION MODAL ── */
.v-modal-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,.55); z-index:9000; justify-content:center; align-items:center; }
.v-modal-overlay.open { display:flex; }
.v-modal-box { background:#fff; width:92%; max-width:520px; max-height:88vh; overflow:hidden; display:flex; flex-direction:column; box-shadow:0 20px 60px rgba(0,0,0,.25); }
.v-modal-hdr { display:flex; align-items:center; justify-content:space-between; padding:14px 20px 12px; border-bottom:1px solid var(--border); background:var(--text); }
.v-modal-hdr h2 { margin:0; font-size:13px; font-family:var(--sans); font-weight:600; letter-spacing:.1em; color:#fff; }
.v-modal-hdr button { background:none; border:none; font-size:18px; cursor:pointer; color:rgba(255,255,255,.6); width:32px; height:32px; display:flex; align-items:center; justify-content:center; }
.v-modal-hdr button:hover { color:#fff; }
.v-tabs { display:flex; border-bottom:1px solid var(--border); }
.v-tab { padding:11px 16px; background:none; border:none; cursor:pointer; font-size:11px; font-family:var(--sans); font-weight:600; letter-spacing:.1em; color:var(--text-muted); border-bottom:2px solid transparent; transition:all .15s; }
.v-tab.active { border-bottom-color:var(--brand); color:var(--brand); }
.v-pane { padding:16px; overflow-y:auto; flex:1; }
.v-item { border:1px solid var(--border); padding:12px 14px; margin-bottom:10px; background:var(--bg-soft); }
.v-item.v-current { border-color:var(--green); background:#f0faf5; }
.v-top { display:flex; align-items:center; gap:8px; flex-wrap:wrap; margin-bottom:6px; }
.v-num { background:var(--text); color:#fff; font-size:10px; font-weight:700; padding:2px 7px; font-family:var(--sans); letter-spacing:.06em; }
.v-badge-current { background:var(--green); color:#fff; font-size:10px; padding:2px 7px; font-family:var(--sans); }
.v-lbl { font-size:12px; color:var(--text-muted); }
.v-date { font-size:11px; color:var(--text-light); margin-left:auto; font-family:var(--sans); }
.v-stats { display:flex; gap:14px; font-size:11px; color:var(--text-muted); flex-wrap:wrap; font-family:var(--sans); }
.v-stats strong { color:var(--text); }
.v-note-text { font-size:11px; color:var(--text-muted); margin:5px 0 0; font-style:italic; font-family:var(--sans); }
.v-acts { display:flex; gap:6px; margin-top:10px; flex-wrap:wrap; }
.v-acts button { font-size:11px; padding:5px 12px; cursor:pointer; border:1px solid var(--border-mid); background:#fff; font-family:var(--sans); transition:background .15s; letter-spacing:.04em; }
.v-acts button:hover { background:var(--bg-soft); }
.v-rollback { border-color:#F59E0B!important; color:#B45309!important; }
.v-rollback:hover { background:#FFFBEB!important; }
.v-loading { text-align:center; padding:28px; color:var(--text-light); font-size:13px; font-family:var(--sans); }
.v-empty { text-align:center; padding:28px; color:var(--text-light); font-size:13px; font-family:var(--serif); letter-spacing:.06em; }
.v-error-msg { padding:14px; color:#C62828; font-size:12px; font-family:var(--sans); background:#FFEBEE; }
.diff-sel { display:flex; align-items:flex-end; gap:8px; flex-wrap:wrap; margin-bottom:14px; padding-bottom:14px; border-bottom:1px solid var(--border); }
.diff-sel label { font-size:9px; font-family:var(--sans); font-weight:700; color:var(--text-muted); letter-spacing:.1em; display:block; margin-bottom:4px; }
.diff-sel select { padding:8px 10px; border:1px solid var(--border-mid); font-size:12px; font-family:var(--sans); color:var(--text); background:var(--bg-soft); outline:none; max-width:190px; }
.diff-arrow { font-size:18px; color:var(--text-light); padding-bottom:8px; }
.btn-diff-run { padding:8px 18px; background:var(--text); color:#fff; border:none; font-size:11px; font-family:var(--sans); font-weight:700; letter-spacing:.1em; cursor:pointer; transition:background .2s; }
.btn-diff-run:hover { background:var(--brand); }
.diff-summary { background:var(--bg-soft); padding:10px 12px; font-size:12px; font-family:var(--sans); margin-bottom:10px; display:flex; gap:14px; align-items:center; border:1px solid var(--border); }
.diff-up { color:#D32F2F; font-weight:700; }
.diff-down { color:var(--green); font-weight:700; }
.diff-tbl { width:100%; border-collapse:collapse; font-size:12px; font-family:var(--sans); }
.diff-tbl th,.diff-tbl td { padding:8px 10px; border:1px solid var(--border); text-align:left; }
.diff-tbl th { background:var(--bg-soft); font-size:10px; letter-spacing:.08em; color:var(--text-muted); }
tr.d-added td { background:#F1FDF4; }
tr.d-removed td { background:#FFF1F2; }
tr.d-changed td { background:#FFFDE7; }
</style>
</head>
<body>

<!-- VERSION HISTORY MODAL (Moved outside .app or correctly placed) -->
<div class="v-modal-overlay" id="versionModal">
  <div class="v-modal-box">
    <div class="v-modal-hdr">
      <h2 id="versionModalTitle">バージョン履歴</h2>
      <button onclick="closeVersionModal()">✕</button>
    </div>
    <div class="v-tabs">
      <button class="v-tab active" onclick="switchVTab('history',this)">📋 履歴一覧</button>
      <button class="v-tab" onclick="switchVTab('diff',this)">🔍 バージョン比較</button>
    </div>
    <div id="vPaneHistory" class="v-pane">
      <div id="vListContainer"><div class="v-loading">読み込み中…</div></div>
    </div>
    <div id="vPaneDiff" class="v-pane" style="display:none">
      <div class="diff-sel">
        <div><label>比較元（古い）</label><select id="diffSelA"></select></div>
        <div class="diff-arrow">→</div>
        <div><label>比較先（新しい）</label><select id="diffSelB"></select></div>
        <button class="btn-diff-run" onclick="execDiff()">比較実行</button>
      </div>
      <div id="diffResult"></div>
    </div>
  </div>
</div>

<script>
  // Supabaseの接続設定（未入力の場合はlocalStorageモードで動作します）
  window.__PHILOCOFFEA_CONFIG__ = {
    supabaseUrl:  'https://jethsomjaivuyhflgdkw.supabase.co',   // 例: 'https://xxxxx.supabase.co'
    supabaseKey:  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpldGhzb21qYWl2dXloZmxnZGt3Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzUyNDM4NzIsImV4cCI6MjA5MDgxOTg3Mn0.nA6NBlgm6krJrrXJzv4A7BV-JXypQgohISjEC7vP7XQ'    // 例: 'eyJhbGci...'
  };
</script>

<div class="app">

  <!-- HEADER -->
  <div class="header">
    <div class="header-brand">PHILOCOFFEA</div>
    <div class="header-sub">RECIPE &amp; COST MANAGER</div>
    <div class="db-indicator">
      <div class="db-dot" id="dbDot"></div>
      <span id="dbLabel" style="font-size:9px;font-family:var(--sans);color:rgba(255,255,255,0.5);letter-spacing:0.06em;">—</span>
    </div>
  </div>
  <!-- DB STATUS BANNER -->
  <div class="db-status-banner" id="dbStatusBanner"></div>
  <!-- MIGRATION BANNER -->
  <div class="migrate-banner" id="migrateBanner">
    <span>⬆ ローカルに保存されたメニューが見つかりました。Supabaseに移行しますか？</span>
    <button class="migrate-btn" onclick="runMigration()">移行する</button>
  </div>

  <!-- TABS -->
  <div class="tabs">
    <button class="tab active" onclick="switchTab('recipe')">レシピ計算</button>
    <button class="tab" onclick="switchTab('menu')">保存済メニュー</button>
  </div>

  <!-- EDIT MODE BANNER -->
  <div class="edit-banner" id="editBanner">
    <div class="edit-banner-text">
      <span>✎</span>
      <span id="editBannerName">編集中</span>
    </div>
    <button class="edit-banner-cancel" onclick="cancelEdit()">キャンセル</button>
  </div>

  <!-- RECIPE TAB -->
  <div id="tab-recipe" class="content">

    <!-- EXCEL IMPORT -->
    <div class="slabel">Master Data</div>
    <div class="excel-zone">
      <div class="excel-icon">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <rect x="2" y="2" width="14" height="14" stroke="white" stroke-width="1.2"/>
          <path d="M6 6L9 9L6 12M12 6L9 9L12 12" stroke="white" stroke-width="1" stroke-linecap="round"/>
        </svg>
      </div>
      <div class="excel-text">
        <p>SKU表（.xlsx）を読み込んでデータを反映</p>
        <div class="excel-status" id="excel-status">テストデータ 3件を使用中</div>
      </div>
      <label class="excel-btn">
        読み込む
        <input type="file" id="excelFile" accept=".xlsx,.xls" style="display:none;" onchange="handleExcelUpload(event)">
      </label>
    </div>

    <!-- CUSTOM SKU REGISTRATION -->
    <div class="custom-sku-zone">
      <button class="custom-sku-toggle" onclick="toggleCustomSku()">
        <span class="arrow" id="customSkuArrow">▶</span>
        個別にマスターデータを登録
      </button>
      <div class="custom-sku-form" id="customSkuForm">
        <div class="custom-sku-row">
          <input type="text" id="customSkuName" placeholder="商品名（例：Ethiopia Arbegona）" />
          <input type="number" id="customSkuCost" placeholder="原価(円)" />
          <input type="number" id="customSkuQty" placeholder="量(g)" />
        </div>
        <button class="custom-sku-add-btn" onclick="addCustomSku()">マスターに登録</button>
        <div class="custom-sku-list" id="customSkuList"></div>
      </div>
    </div>

    <!-- MENU NAME -->
    <div class="slabel" style="margin-top:24px;">Menu Name</div>
    <input class="menu-name-input" id="menuName" placeholder="メニュー名を入力…" />

    <!-- TYPE -->
    <div class="slabel">Type</div>
    <div class="type-toggle">
      <button class="type-btn active" id="type-coffee" onclick="setType('coffee')">COFFEE</button>
      <button class="type-btn" id="type-other" onclick="setType('other')">OTHER</button>
    </div>

    <!-- INGREDIENTS -->
    <div class="slabel">Ingredients</div>
    <table class="ing-table">
      <thead>
        <tr>
          <th style="width:36%">材料名 — 入力で候補表示</th>
          <th style="width:19%">仕入原価(円)</th>
          <th style="width:19%">仕入量(g)</th>
          <th style="width:18%">使用量(g)</th>
          <th style="width:8%"></th>
        </tr>
      </thead>
      <tbody id="ingredients"></tbody>
    </table>
    <button class="btn-add" onclick="addIngredient()">＋ 材料を追加する</button>

    <!-- SETTINGS -->
    <div class="slabel">Settings</div>
    <div class="settings-grid">
      <div class="input-group">
        <label id="pourLabel">注湯量 (ml)</label>
        <input type="number" id="pourAmount" placeholder="300" oninput="calculate()" />
      </div>
      <div class="input-group">
        <label>目標原価率 (%)</label>
        <input type="number" id="targetRate" placeholder="15" value="15" oninput="calculate()" />
      </div>
    </div>

    <!-- OTHER: Yield & Serving -->
    <div id="otherSettingsSection" style="display:none;">
      <div class="divider"></div>
      <div class="settings-grid">
        <div class="input-group">
          <label>出来上がり量 (ml/g)</label>
          <input type="number" id="yieldTotal" placeholder="例：1000" oninput="calculate()" />
        </div>
        <div class="input-group">
          <label>1提供量 (ml/g)</label>
          <input type="number" id="servingSize" placeholder="例：200" oninput="calculate()" />
        </div>
      </div>
    </div>

    <div class="divider"></div>
    <div class="input-group">
      <label>想定販売価格 (円) — 上限原価の逆算 &amp; 粗利計算</label>
      <input type="number" id="targetPrice" placeholder="例：1500" oninput="calculate()" />
    </div>

    <!-- RESULTS -->
    <div class="result-grid">
      <div class="rcard">
        <div class="rlabel">合計原価</div>
        <div class="rvalue" id="totalCost">—</div>
      </div>
      <div class="rcard" id="yieldCard">
        <div class="rlabel" id="yieldLabel">実質提供量</div>
        <div class="rvalue" id="yieldAmt">—</div>
      </div>
      <div class="rcard wide">
        <div class="rlabel">現在の原価率</div>
        <div class="rvalue" id="costRateDisplay" style="color:var(--brand);">—</div>
      </div>
      <div class="rcard wide dark">
        <div class="rlabel">推奨販売価格</div>
        <div class="rvalue" id="suggestedPrice">—</div>
        <div class="rsub" id="suggestedSub"></div>
      </div>

      <!-- PER-SERVING (Other) -->
      <div class="rcard wide dark" id="perServingCard" style="display:none;">
        <div class="rlabel">1提供あたり（÷ <span id="servingCountLabel">—</span>杯分）</div>
        <div class="per-serving-grid">
          <div class="per-serving-item">
            <div class="per-serving-label">原価</div>
            <div class="per-serving-value" id="perServingCost">—</div>
          </div>
          <div class="per-serving-item">
            <div class="per-serving-label">推奨販売価格</div>
            <div class="per-serving-value" id="perServingPrice">—</div>
          </div>
        </div>
      </div>

      <!-- GROSS PROFIT CARD -->
      <div class="rcard wide green-bg" id="profitCard" style="display:none;">
        <div class="rlabel">想定粗利（1杯あたり）</div>
        <div class="rvalue profit-positive" id="profitValue">—</div>
        <div class="rsub" id="profitSub"></div>
      </div>

      <div class="rcard wide brand-bg" id="budgetCard" style="display:none;">
        <div class="rlabel">使用可能原価（予算上限）</div>
        <div class="rvalue" id="budgetCost">—</div>
        <div class="rsub" id="budgetSub"></div>
        <div id="budgetBarSection" style="display:none;">
          <div class="budget-bar-track">
            <div class="budget-bar" id="budgetBar"></div>
          </div>
          <div class="budget-labels">
            <span>現在の原価</span>
            <span id="budgetBarLabel"></span>
          </div>
        </div>
      </div>
    </div>

    <!-- MEMO -->
    <div class="slabel" style="margin-top:24px;">Memo</div>
    <textarea class="memo-area" id="menuMemo" placeholder="メモ（レシピの備考など…）" rows="3"></textarea>

    <!-- 変更メモ（編集モード時に表示） -->
    <div class="change-note-row" id="changeNoteRow">
      <label>変更メモ（任意）</label>
      <input type="text" id="changeNote" placeholder="例：豆の使用量を18g→16gに変更" />
    </div>
    <button class="save-btn" id="saveBtn" onclick="saveMenu()">SAVE TO MENU</button>
  </div>

  <!-- MENU TAB -->
  <div id="tab-menu" class="content" style="display:none;">
    <div id="menuSummary" style="display:none;"></div>
    <div id="menuList"></div>
  </div>

</div>

<!-- CONFIRM DIALOG -->
<div class="confirm-overlay" id="confirmDialog">
  <div class="confirm-box">
    <p id="confirmText">本当に削除しますか？</p>
    <div class="modal-actions">
      <button class="modal-btn modal-btn-cancel" onclick="closeConfirm()">キャンセル</button>
      <button class="modal-btn modal-btn-save" style="background:#D32F2F;" onclick="confirmAction()">削除する</button>
    </div>
  </div>
</div>

<script>
// ── STORAGE KEYS ──
const STORAGE_KEY_MENUS = 'philocoffea_menus';
const STORAGE_KEY_CUSTOM_SKU = 'philocoffea_custom_sku';

// ============================================================
// SUPABASE v4 — DB層
// ============================================================
let _sb = null;
let _useSupabase = false;
let _vModalMenuId = null;
let _versionsCache = [];

async function initSupabase() {
  const cfg = window.__PHILOCOFFEA_CONFIG__ || {};
  const url = cfg.supabaseUrl || '';
  const key = cfg.supabaseKey || '';
  if (!url || !key || url.includes('YOUR') || key.includes('YOUR')) {
    setDbUI('local', 'LOCAL MODE');
    return;
  }
  try {
    const { createClient } = await import('https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/+esm');
    _sb = createClient(url, key);
    const { error } = await _sb.from('menus').select('id').limit(1);
    if (error) throw error;
    _useSupabase = true;
    setDbUI('online', 'SUPABASE');
    if (localStorage.getItem(STORAGE_KEY_MENUS)) {
      const mb = document.getElementById('migrateBanner');
      if (mb) mb.style.display = 'flex';
    }
  } catch(e) {
    setDbUI('offline', 'LOCAL MODE');
    showBanner('Supabase接続失敗。ローカル保存で動作します。', 'sb-warn');
  }
}

function setDbUI(state, label) {
  const dot = document.getElementById('dbDot');
  const lbl = document.getElementById('dbLabel');
  if (dot) { dot.className = 'db-dot ' + state; }
  if (lbl) lbl.textContent = label;
}

function showBanner(msg, cls) {
  const b = document.getElementById('dbStatusBanner');
  if (!b) return;
  b.textContent = msg;
  b.className = 'db-status-banner ' + cls;
  b.style.display = 'flex';
  setTimeout(() => { b.style.display = 'none'; }, 4000);
}

async function runMigration() {
  if (!_useSupabase) { alert('Supabaseに接続されていません'); return; }
  const raw = localStorage.getItem(STORAGE_KEY_MENUS);
  if (!raw) return;
  let items; try { items = JSON.parse(raw); } catch { return; }
  let ok = 0, errs = [];
  for (const m of items) {
    try {
      const { data: menu, error: me } = await _sb.from('menus').insert([{
        name: m.name||'無題', drink_type: m.type||'coffee',
        target_price: parseFloat(m.targetPrice)||0,
        target_cost_rate: parseFloat(m.savedRate)||15,
        memo: m.memo||''
      }]).select().single();
      if (me) throw me;
      const { error: ve } = await _sb.from('recipe_versions').insert([{
        menu_id: menu.id, version_num: 1, snapshot: m,
        total_cost: parseFloat(m.totalCost)||0,
        cost_rate: parseFloat(m.rate)||0, gross_profit: 0,
        is_current: true, change_note: 'localStorage より移行'
      }]);
      if (ve) throw ve;
      ok++;
    } catch(e) { errs.push((m.name||'?')+': '+e.message); }
  }
  if (errs.length === 0) {
    localStorage.setItem(STORAGE_KEY_MENUS+'_backup', raw);
    localStorage.removeItem(STORAGE_KEY_MENUS);
    const mb = document.getElementById('migrateBanner');
    if (mb) mb.style.display = 'none';
    showBanner('✅ '+ok+'件を移行しました', 'sb-success');
    await loadMenusFromDB();
    renderMenu();
  } else {
    alert('移行完了 '+ok+'件 / エラー '+errs.length+'件:\n'+errs.join('\n'));
  }
}

async function loadMenusFromDB() {
  if (!_useSupabase) return;
  const { data, error } = await _sb.from('menu_current_summary').select('*').order('version_created_at', { ascending: false });
  if (error) { console.error(error); return; }
  menus = (data||[]).map(r => ({
    id: r.menu_id,
    name: r.menu_name,
    type: r.drink_type||'coffee',
    totalCost: parseFloat(r.total_cost)||0,
    rate: parseFloat(r.cost_rate)||0,
    price: parseFloat(r.target_price)||0,
    targetPrice: parseFloat(r.target_price)||0,
    grossProfit: parseFloat(r.gross_profit)||0,
    _versionId: r.version_id,
    _versionNum: r.version_num,
    _fromSupabase: true,
  }));
}

// ============================================================
// VERSION MODAL
// ============================================================
async function openVersionModal(menuId, menuName) {
  _vModalMenuId = menuId;
  document.getElementById('versionModalTitle').textContent = 'バージョン履歴 — ' + menuName;
  document.getElementById('versionModal').classList.add('open');
  await loadVHistory();
}
function closeVersionModal() {
  document.getElementById('versionModal').classList.remove('open');
  _vModalMenuId = null; _versionsCache = [];
}
async function loadVHistory() {
  const c = document.getElementById('vListContainer');
  c.innerHTML = '<div class="v-loading">読み込み中…</div>';
  const { data, error } = await _sb.from('recipe_versions')
    .select('id,version_num,label,change_note,total_cost,cost_rate,gross_profit,is_current,created_at')
    .eq('menu_id', _vModalMenuId).order('version_num', { ascending: false });
  if (error) { c.innerHTML = '<div class="v-error-msg">'+error.message+'</div>'; return; }
  _versionsCache = data||[];
  c.innerHTML = _versionsCache.length === 0
    ? '<div class="v-empty">履歴がありません</div>'
    : _versionsCache.map(v => `
      <div class="v-item ${v.is_current?'v-current':''}">
        <div class="v-top">
          <span class="v-num">v${v.version_num}</span>
          ${v.is_current?'<span class="v-badge-current">現行</span>':''}
          ${v.label?'<span class="v-lbl">'+_esc(v.label)+'</span>':''}
          <span class="v-date">${_fmtDate(v.created_at)}</span>
        </div>
        <div class="v-stats">
          <span>原価 <strong>¥${_num(v.total_cost)}</strong></span>
          <span>原価率 <strong>${parseFloat(v.cost_rate||0).toFixed(1)}%</strong></span>
          <span>粗利 <strong>¥${_num(v.gross_profit)}</strong></span>
        </div>
        ${v.change_note?'<p class="v-note-text">📝 '+_esc(v.change_note)+'</p>':''}
        <div class="v-acts">
          <button onclick="viewSnap('${v.id}')">📋 内容確認</button>
          ${!v.is_current?`<button class="v-rollback" onclick="doRollback('${v.id}',${v.version_num})">↩ ここに戻す</button>`:''}
        </div>
      </div>`).join('');
  populateDiffSels(_versionsCache);
}
async function viewSnap(vid) {
  const { data, error } = await _sb.from('recipe_versions')
    .select('snapshot,version_num,label,created_at').eq('id',vid).single();
  if (error) { alert('取得エラー: '+error.message); return; }
  const snap = data.snapshot||{};
  const ings = snap.ingredients||[];
  const rows = ings.map(i => {
    const c = i.cost && i.qty ? Math.round((parseFloat(i.cost)/parseFloat(i.qty))*(parseFloat(i.use)||0)) : 0;
    return '<tr><td style="padding:5px 8px;border:1px solid #eee">'+_esc(i.name||'')+'</td><td style="padding:5px 8px;border:1px solid #eee;text-align:right">'+(i.use||0)+'g</td><td style="padding:5px 8px;border:1px solid #eee;text-align:right">¥'+_num(c)+'</td></tr>';
  }).join('');
  const html = '<div style="font-size:13px;font-family:var(--sans)">'
    +'<p style="margin-bottom:6px"><strong>v'+data.version_num+'</strong>'+(data.label?' 「'+_esc(data.label)+'」':'')+' — '+_fmtDate(data.created_at)+'</p>'
    +'<p style="margin-bottom:4px;color:#888;font-size:11px">メニュー名: '+_esc(snap.name||snap.menuName||'')+'</p>'
    +'<p style="margin-bottom:10px;color:#888;font-size:11px">目標価格: ¥'+_num(snap.targetPrice)+' / 目標原価率: '+(snap.savedRate||snap.rate||0)+'%</p>'
    +'<table style="width:100%;border-collapse:collapse"><thead><tr style="background:#f5f5f5">'
    +'<th style="padding:6px 8px;border:1px solid #eee;text-align:left;font-size:10px">食材</th>'
    +'<th style="padding:6px 8px;border:1px solid #eee;font-size:10px">使用量</th>'
    +'<th style="padding:6px 8px;border:1px solid #eee;font-size:10px">原価</th>'
    +'</tr></thead><tbody>'+rows+'</tbody></table></div>';
  _showInlineModal('スナップショット確認', html);
}
async function doRollback(vid, vnum) {
  if (!confirm('v'+vnum+' を現行バージョンに戻しますか？\n現在のバージョンは履歴として残ります。')) return;
  const { error } = await _sb.from('recipe_versions').update({is_current:true}).eq('id',vid).eq('menu_id',_vModalMenuId);
  if (error) { alert('ロールバック失敗: '+error.message); return; }
  showBanner('↩ v'+vnum+' に戻しました', 'sb-success');
  await loadMenusFromDB(); renderMenu(); await loadVHistory();
}
function populateDiffSels(versions) {
  ['diffSelA','diffSelB'].forEach(id => {
    const sel = document.getElementById(id);
    sel.innerHTML = versions.map(v =>
      '<option value="'+v.id+'">v'+v.version_num+(v.label?' ('+_esc(v.label)+')':'')+' — '+_fmtDate(v.created_at)+'</option>'
    ).join('');
  });
  if (versions.length >= 2) { document.getElementById('diffSelA').selectedIndex=1; document.getElementById('diffSelB').selectedIndex=0; }
}
async function execDiff() {
  const idA = document.getElementById('diffSelA').value;
  const idB = document.getElementById('diffSelB').value;
  if (idA===idB) { alert('異なるバージョンを選択してください'); return; }
  const c = document.getElementById('diffResult');
  c.innerHTML = '<div class="v-loading">比較中…</div>';
  const { data, error } = await _sb.from('recipe_versions')
    .select('id,version_num,label,snapshot,total_cost,cost_rate,created_at').in('id',[idA,idB]);
  if (error||!data||data.length<2) { c.innerHTML='<div class="v-error-msg">取得エラー</div>'; return; }
  const vA=data.find(d=>d.id===idA), vB=data.find(d=>d.id===idB);
  const iA=vA.snapshot?.ingredients||[], iB=vB.snapshot?.ingredients||[];
  const allNames=[...new Set([...iA.map(i=>i.name),...iB.map(i=>i.name)])];
  const calcC = ing => ing&&ing.cost&&ing.qty ? ((parseFloat(ing.cost)/Math.max(parseFloat(ing.qty),1))*(parseFloat(ing.use)||0)) : null;
  const rows = allNames.map(name => {
    const a=iA.find(i=>i.name===name), b=iB.find(i=>i.name===name);
    const cA=calcC(a), cB=calcC(b);
    const type=!a?'added':!b?'removed':(Math.abs((cA||0)-(cB||0))>0.01||(a.use!==b.use))?'changed':'same';
    const icon={added:'➕',removed:'➖',changed:'✏️',same:'✅'}[type];
    return '<tr class="d-'+type+'"><td>'+_esc(name)+'</td><td>'+icon+'</td>'
      +'<td>'+(a?'¥'+_num(cA)+' ('+a.use+'g)':'—')+'</td>'
      +'<td>'+(b?'¥'+_num(cB)+' ('+b.use+'g)':'—')+'</td></tr>';
  }).join('');
  const diff = Number(vB.total_cost)-Number(vA.total_cost);
  c.innerHTML = '<div class="diff-summary"><span>合計原価: ¥'+_num(vA.total_cost)+' → ¥'+_num(vB.total_cost)+'</span>'
    +'<span class="'+(diff>0?'diff-up':diff<0?'diff-down':'')+'">'+(diff!==0?(diff>0?'▲':'▼')+' ¥'+_num(Math.abs(diff)):'変化なし')+'</span></div>'
    +'<table class="diff-tbl"><thead><tr><th>食材名</th><th>変更</th><th>v'+vA.version_num+'</th><th>v'+vB.version_num+'</th></tr></thead><tbody>'+rows+'</tbody></table>';
}
function switchVTab(name, btn) {
  document.getElementById('vPaneHistory').style.display = name==='history'?'block':'none';
  document.getElementById('vPaneDiff').style.display = name==='diff'?'block':'none';
  document.querySelectorAll('.v-tab').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}
function _showInlineModal(title, html) {
  const ex=document.getElementById('_im'); if(ex) ex.remove();
  const el=document.createElement('div'); el.id='_im';
  el.style.cssText='position:fixed;inset:0;background:rgba(0,0,0,.6);display:flex;align-items:center;justify-content:center;z-index:10000';
  el.innerHTML='<div style="background:#fff;padding:22px;max-width:480px;width:90%;max-height:80vh;overflow-y:auto;position:relative">'
    +`<button onclick="this.closest('#_im').remove()" style="position:absolute;top:12px;right:14px;background:none;border:none;font-size:18px;cursor:pointer;color:#888">✕</button>`
    +'<h3 style="margin:0 0 14px;font-size:14px;font-family:var(--sans)">'+_esc(title)+'</h3>'+html+'</div>';
  document.body.appendChild(el);
}
function _esc(s) { return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
function _num(n) { return Number(n||0).toLocaleString(); }
function _fmtDate(iso) {
  const d=new Date(iso);
  return d.getFullYear()+'/'+(d.getMonth()+1)+'/'+d.getDate()+' '+String(d.getHours()).padStart(2,'0')+':'+String(d.getMinutes()).padStart(2,'0');
}


// ── SKU マスターデータ ──
let skuMaster = [
  { name: '【709】Colombia La Esmeralda Geisha Washed', cost100: null, cost200: 1941, cost1000: null, qty100: null, qty200: 200, qty1000: null },
  { name: '【412】Colombia Lusitania Pink Bourbon Natural LS26', cost100: null, cost200: null, cost1000: 5042, qty100: null, qty200: null, qty1000: 1000 },
  { name: 'ドリンク用【700】Ecuador LIavecocha Geisha Washed', cost100: 1469, cost200: null, cost1000: null, qty100: 100, qty200: null, qty1000: null },
];
let customSkuList = [];

function loadCustomSku() {
  try {
    const data = localStorage.getItem(STORAGE_KEY_CUSTOM_SKU);
    if (data) {
      customSkuList = JSON.parse(data);
      customSkuList.forEach(s => {
        if (!skuMaster.find(m => m.name === s.name)) {
          skuMaster.push(s);
        }
      });
      renderCustomSkuList();
    }
  } catch(e) {}
}

function saveCustomSkuStorage() {
  try { localStorage.setItem(STORAGE_KEY_CUSTOM_SKU, JSON.stringify(customSkuList)); } catch(e) {}
}

function getBestCostQty(sku) {
  if (sku.cost1000 != null) return { cost: sku.cost1000, qty: sku.qty1000 };
  if (sku.cost200 != null)  return { cost: sku.cost200,  qty: sku.qty200 };
  if (sku.cost100 != null)  return { cost: sku.cost100,  qty: sku.qty100 };
  return null;
}

// ── Custom SKU ──
function toggleCustomSku() {
  const form = document.getElementById('customSkuForm');
  const arrow = document.getElementById('customSkuArrow');
  form.classList.toggle('open');
  arrow.classList.toggle('open');
}

function addCustomSku() {
  const name = document.getElementById('customSkuName').value.trim();
  const cost = parseFloat(document.getElementById('customSkuCost').value);
  const qty = parseFloat(document.getElementById('customSkuQty').value);
  if (!name) { alert('商品名を入力してください'); return; }
  if (isNaN(cost) || cost <= 0) { alert('原価を正しく入力してください'); return; }
  if (isNaN(qty) || qty <= 0) { alert('量を正しく入力してください'); return; }

  const skuEntry = { name, cost100: null, qty100: null, cost200: null, qty200: null, cost1000: null, qty1000: null };
  if (qty >= 1000) { skuEntry.cost1000 = cost; skuEntry.qty1000 = qty; }
  else if (qty >= 200) { skuEntry.cost200 = cost; skuEntry.qty200 = qty; }
  else { skuEntry.cost100 = cost; skuEntry.qty100 = qty; }

  const existIdx = skuMaster.findIndex(s => s.name === name);
  if (existIdx !== -1) skuMaster[existIdx] = skuEntry;
  else skuMaster.push(skuEntry);

  const custIdx = customSkuList.findIndex(s => s.name === name);
  if (custIdx !== -1) customSkuList[custIdx] = skuEntry;
  else customSkuList.push(skuEntry);

  saveCustomSkuStorage();
  renderCustomSkuList();
  updateExcelStatus();

  document.getElementById('customSkuName').value = '';
  document.getElementById('customSkuCost').value = '';
  document.getElementById('customSkuQty').value = '';
}

function removeCustomSku(name) {
  customSkuList = customSkuList.filter(s => s.name !== name);
  skuMaster = skuMaster.filter(s => s.name !== name);
  saveCustomSkuStorage();
  renderCustomSkuList();
  updateExcelStatus();
}

function renderCustomSkuList() {
  const el = document.getElementById('customSkuList');
  if (customSkuList.length === 0) { el.innerHTML = ''; return; }
  el.innerHTML = customSkuList.map(s => {
    const best = getBestCostQty(s);
    return `<div class="custom-sku-item">
      <span class="custom-sku-item-name">${s.name}</span>
      <span class="custom-sku-item-info">¥${best.cost.toLocaleString()} / ${best.qty >= 1000 ? best.qty/1000+'kg' : best.qty+'g'}</span>
      <button class="custom-sku-item-del" onclick="removeCustomSku('${s.name.replace(/'/g,"\\'")}')">×</button>
    </div>`;
  }).join('');
}

function updateExcelStatus() {
  const base = skuMaster.length - customSkuList.length;
  const custom = customSkuList.length;
  let text = '';
  if (base > 0 && custom > 0) text = `${base}件 + 個別登録${custom}件 = 計${skuMaster.length}件`;
  else if (custom > 0) text = `個別登録 ${custom}件を使用中`;
  else text = `テストデータ ${base}件を使用中`;
  document.getElementById('excel-status').textContent = text;
}

// ── Excel 読み込み ──
function handleExcelUpload(event) {
  const file = event.target.files[0];
  if (!file) return;
  document.getElementById('excel-status').textContent = '読み込み中…';
  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const wb = XLSX.read(new Uint8Array(e.target.result), { type: 'array' });
      const ws = wb.Sheets[wb.SheetNames[0]];
      const rows = XLSX.utils.sheet_to_json(ws, { header: 1 });

      let nameCol = -1, cost100Col = -1, cost200Col = -1, cost1000Col = -1, headerRow = -1;

      for (let r = 0; r < Math.min(25, rows.length); r++) {
        const row = rows[r];
        if (!row) continue;
        let found = false;
        for (let c = 0; c < row.length; c++) {
          const cell = String(row[c] || '').trim();
          if (cell.includes('商品名') || cell.includes('品名')) { nameCol = c; found = true; }
          if (/100g.*原価|原価.*100g/.test(cell)) cost100Col = c;
          if (/200g.*原価|原価.*200g/.test(cell)) cost200Col = c;
          if (/1kg.*原価|原価.*1kg/.test(cell)) cost1000Col = c;
          if (cost100Col === -1 && cost200Col === -1 && cost1000Col === -1) {
            if (cell.includes('原価') && !cell.includes('商品')) {
              if (cell.includes('100g')) cost100Col = c;
              else if (cell.includes('200g')) cost200Col = c;
              else if (cell.includes('1kg') || cell.includes('1000g')) cost1000Col = c;
            }
          }
        }
        if (found && nameCol !== -1) { headerRow = r; break; }
      }

      if (cost100Col === -1 && cost200Col === -1 && cost1000Col === -1 && headerRow !== -1) {
        const hrow = rows[headerRow];
        for (let c = 0; c < hrow.length; c++) {
          const cell = String(hrow[c] || '').trim();
          if ((cell.includes('原価') || cell.includes('価格')) && c !== nameCol) {
            cost200Col = c;
            break;
          }
        }
      }

      if (nameCol === -1 || headerRow === -1) {
        alert('「商品名」列が見つかりませんでした。SKU表の形式を確認してください。');
        document.getElementById('excel-status').textContent = '❌ 読み込み失敗';
        return;
      }

      const grouped = {};
      for (let r = headerRow + 1; r < rows.length; r++) {
        const row = rows[r];
        if (!row || !row[nameCol]) continue;
        const rawName = String(row[nameCol]).trim();
        if (!rawName) continue;
        const is100 = /100g|100ｇ/i.test(rawName);
        const is200 = /200g|200ｇ/i.test(rawName);
        const is1kg = /1kg|1000g/i.test(rawName);
        const baseName = rawName.replace(/\s*(100g|200g|1kg|1000g|100ｇ|200ｇ)\s*/gi, '').trim();
        const c100 = cost100Col !== -1 ? parseFloat(row[cost100Col]) : null;
        const c200 = cost200Col !== -1 ? parseFloat(row[cost200Col]) : null;
        const c1k  = cost1000Col !== -1 ? parseFloat(row[cost1000Col]) : null;
        if (!grouped[baseName]) grouped[baseName] = { name: baseName, cost100: null, qty100: null, cost200: null, qty200: null, cost1000: null, qty1000: null };
        if (is100 && !isNaN(c100)) { grouped[baseName].cost100 = c100; grouped[baseName].qty100 = 100; }
        else if (is200 && !isNaN(c200)) { grouped[baseName].cost200 = c200; grouped[baseName].qty200 = 200; }
        else if (is1kg && !isNaN(c1k))  { grouped[baseName].cost1000 = c1k; grouped[baseName].qty1000 = 1000; }
        else {
          const singleCost = c200 ?? c100 ?? c1k;
          if (singleCost != null && !isNaN(singleCost)) {
            if (is100) { grouped[baseName].cost100 = singleCost; grouped[baseName].qty100 = 100; }
            else if (is1kg) { grouped[baseName].cost1000 = singleCost; grouped[baseName].qty1000 = 1000; }
            else { grouped[baseName].cost200 = singleCost; grouped[baseName].qty200 = 200; }
          }
        }
      }

      skuMaster = [...Object.values(grouped).filter(s => getBestCostQty(s) !== null), ...customSkuList];
      const excelCount = skuMaster.length - customSkuList.length;
      document.getElementById('excel-status').textContent = customSkuList.length > 0
        ? `✅ ${excelCount}件 + 個別${customSkuList.length}件 読み込み完了`
        : `✅ ${excelCount}件 読み込み完了`;
    } catch(err) {
      console.error(err);
      alert('読み込みエラーが発生しました。ファイル形式を確認してください。');
      document.getElementById('excel-status').textContent = '❌ 読み込み失敗';
    }
  };
  reader.readAsArrayBuffer(file);
}

// ── Ingredient 行管理 ──
let ingredientsList = [];
let ingredientId = 0;
let menus = [];
let drinkType = 'coffee';
const ABSORPTION = 2;

// ── EDIT MODE STATE ──
let editingIndex = -1; // -1 = new mode, >= 0 = editing existing menu

function addIngredient(prefill) {
  const id = ++ingredientId;
  ingredientsList.push(id);
  const tr = document.createElement('tr');
  tr.className = 'ing-tr';
  tr.id = 'ing-' + id;
  tr.innerHTML = `
    <td style="position:relative;">
      <div class="suggest-wrap">
        <input class="field" id="name-${id}" type="text" placeholder="名前を入力…"
          oninput="onNameInput(${id})" onblur="hideSuggest(${id}, 200)" autocomplete="off" />
        <div class="suggest-list" id="suggest-${id}"></div>
      </div>
    </td>
    <td><input class="field" id="cost-${id}" type="number" placeholder="—" oninput="calculate()" /></td>
    <td><input class="field" id="qty-${id}"  type="number" placeholder="—" oninput="calculate()" /></td>
    <td><input class="field" id="use-${id}"  type="number" placeholder="20"  oninput="calculate()" /></td>
    <td><button class="btn-del" onclick="removeIngredient(${id})">×</button></td>
  `;
  document.getElementById('ingredients').appendChild(tr);

  // Prefill if editing
  if (prefill) {
    document.getElementById(`name-${id}`).value = prefill.name || '';
    document.getElementById(`cost-${id}`).value = prefill.cost || '';
    document.getElementById(`qty-${id}`).value = prefill.qty || '';
    document.getElementById(`use-${id}`).value = prefill.use || '';
  }
}

function removeIngredient(id) {
  const el = document.getElementById('ing-' + id);
  if (el) el.remove();
  ingredientsList = ingredientsList.filter(i => i !== id);
  calculate();
}

// ── Collect current ingredient data ──
function collectIngredients() {
  return ingredientsList.map(id => ({
    name: document.getElementById(`name-${id}`)?.value || '',
    cost: document.getElementById(`cost-${id}`)?.value || '',
    qty:  document.getElementById(`qty-${id}`)?.value || '',
    use:  document.getElementById(`use-${id}`)?.value || '',
  })).filter(i => i.name || i.cost || i.qty || i.use);
}

// ── Clear form ──
function clearForm() {
  document.getElementById('menuName').value = '';
  document.getElementById('menuMemo').value = '';
  document.getElementById('pourAmount').value = '';
  document.getElementById('targetRate').value = '15';
  document.getElementById('targetPrice').value = '';
  document.getElementById('yieldTotal').value = '';
  document.getElementById('servingSize').value = '';

  // Clear ingredients
  document.getElementById('ingredients').innerHTML = '';
  ingredientsList = [];
  ingredientId = 0;

  setType('coffee');
  editingIndex = -1;
  updateEditUI();
  addIngredient();
  calculate();
}

// ── Load menu data into form for editing ──
async function loadMenuIntoForm(idx) {
  const m = menus[idx];
  editingIndex = idx;

  let formData = m;

  // Supabaseモード: 最新バージョンのスナップショットを取得
  if (_useSupabase && m._versionId) {
    const { data, error } = await _sb.from('recipe_versions').select('snapshot').eq('id', m._versionId).single();
    if (!error && data?.snapshot) formData = { ...m, ...data.snapshot };
  }

  document.getElementById('ingredients').innerHTML = '';
  ingredientsList = [];
  ingredientId = 0;

  document.getElementById('menuName').value = formData.name || '';
  document.getElementById('menuMemo').value = formData.memo || '';
  document.getElementById('targetRate').value = formData.savedRate || parseFloat(formData.rate) || 15;
  document.getElementById('targetPrice').value = formData.targetPrice || '';
  document.getElementById('pourAmount').value = formData.pourAmount || '';
  document.getElementById('yieldTotal').value = formData.yieldTotal || '';
  document.getElementById('servingSize').value = formData.servingSize || '';

  setType(formData.type || 'coffee');

  if (formData.ingredients && formData.ingredients.length > 0) {
    formData.ingredients.forEach(ing => addIngredient(ing));
  } else {
    addIngredient();
  }

  updateEditUI();
  calculate();
  switchTab('recipe');
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function updateEditUI() {
  const banner = document.getElementById('editBanner');
  const saveBtn = document.getElementById('saveBtn');

  const noteRow = document.getElementById('changeNoteRow');
  if (editingIndex >= 0) {
    banner.classList.add('active');
    document.getElementById('editBannerName').textContent = `「${menus[editingIndex].name}」を編集中`;
    saveBtn.textContent = 'UPDATE MENU';
    saveBtn.classList.add('editing');
    if (noteRow) noteRow.classList.add('visible');
  } else {
    banner.classList.remove('active');
    saveBtn.textContent = 'SAVE TO MENU';
    saveBtn.classList.remove('editing');
    if (noteRow) { noteRow.classList.remove('visible'); }
    const cn = document.getElementById('changeNote');
    if (cn) cn.value = '';
  }
}

function cancelEdit() {
  clearForm();
}

// ── サジェスト ──
function onNameInput(id) {
  const query = document.getElementById(`name-${id}`).value.trim();
  const list = document.getElementById(`suggest-${id}`);
  if (query.length < 1) { list.innerHTML = ''; list.classList.remove('open'); calculate(); return; }
  const q = query.toLowerCase();
  const matched = skuMaster.filter(s => s.name.toLowerCase().includes(q));
  if (matched.length === 0) {
    list.innerHTML = `<div class="suggest-no-result">「${query}」に一致する商品はありません</div>`;
    list.classList.add('open');
    calculate();
    return;
  }
  const header = `<div class="suggest-header">もしかして… (${matched.length}件)</div>`;
  const items = matched.slice(0, 8).map((s, i) => {
    const best = getBestCostQty(s);
    const highlightedName = s.name.replace(
      new RegExp(query.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'gi'),
      m => `<mark>${m}</mark>`
    );
    const qtySizes = [];
    if (s.qty100)  qtySizes.push('100g');
    if (s.qty200)  qtySizes.push('200g');
    if (s.qty1000) qtySizes.push('1kg');
    const metaText = best ? `¥${best.cost.toLocaleString()} / ${best.qty >= 1000 ? '1kg' : best.qty + 'g'} 優先` : '';
    return `
      <div class="suggest-item" onmousedown="selectSku(${id}, '${s.name.replace(/'/g, "\\'")}')">
        <div class="suggest-entry">
          <div class="suggest-name">${highlightedName}</div>
          <div class="suggest-meta">${metaText}${qtySizes.length > 1 ? ' · ' + qtySizes.join(', ') + ' 展開可' : ''}</div>
        </div>
      </div>`;
  }).join('');
  list.innerHTML = header + items;
  list.classList.add('open');
  calculate();
}

function selectSku(id, name) {
  const sku = skuMaster.find(s => s.name === name);
  if (!sku) return;
  document.getElementById(`name-${id}`).value = sku.name;
  const best = getBestCostQty(sku);
  if (best) {
    document.getElementById(`cost-${id}`).value = best.cost;
    document.getElementById(`qty-${id}`).value  = best.qty;
    if (!document.getElementById(`use-${id}`).value) {
      document.getElementById(`use-${id}`).value = 20;
    }
  }
  hideSuggest(id, 0);
  calculate();
}

function hideSuggest(id, delay) {
  setTimeout(() => {
    const list = document.getElementById(`suggest-${id}`);
    if (list) { list.innerHTML = ''; list.classList.remove('open'); }
  }, delay);
}

// ── 計算 ──
function getIngredientCost(id) {
  const cost = parseFloat(document.getElementById(`cost-${id}`)?.value) || 0;
  const qty  = parseFloat(document.getElementById(`qty-${id}`)?.value)  || 0;
  const use  = parseFloat(document.getElementById(`use-${id}`)?.value)  || 0;
  if (qty === 0) return 0;
  return (cost / qty) * use;
}

function getGroundsUsage() {
  let max = 0;
  ingredientsList.forEach(id => {
    const use = parseFloat(document.getElementById(`use-${id}`)?.value) || 0;
    if (use > max) max = use;
  });
  return max;
}

function calculate() {
  let total = 0;
  ingredientsList.forEach(id => { total += getIngredientCost(id); });

  const pour = parseFloat(document.getElementById('pourAmount').value) || 0;
  const rate = parseFloat(document.getElementById('targetRate').value) || 0;
  const targetPrice = parseFloat(document.getElementById('targetPrice').value) || 0;
  const yieldTotal = parseFloat(document.getElementById('yieldTotal').value) || 0;
  const servingSize = parseFloat(document.getElementById('servingSize').value) || 0;

  let yieldAmt = null;
  if (drinkType === 'coffee' && pour > 0) {
    yieldAmt = Math.max(0, pour - getGroundsUsage() * ABSORPTION);
  }

  const suggested = rate > 0 && total > 0 ? Math.round(total / (rate / 100)) : null;
  const costRate = targetPrice > 0 && total > 0 ? ((total / targetPrice) * 100).toFixed(1) : (suggested ? ((total / suggested) * 100).toFixed(1) : null);
  const budget = rate > 0 && targetPrice > 0 ? Math.round(targetPrice * (rate / 100)) : null;

  document.getElementById('totalCost').innerHTML = total > 0 ? `${Math.round(total)}<span class="runit">円</span>` : '—';
  document.getElementById('yieldLabel').textContent = drinkType === 'coffee' ? '実質提供量' : '提供量';

  if (drinkType === 'coffee') {
    document.getElementById('yieldAmt').innerHTML = (pour > 0 && yieldAmt !== null) ? `${Math.round(yieldAmt)}<span class="runit">ml</span>` : '—';
  } else {
    document.getElementById('yieldAmt').innerHTML = yieldTotal > 0 ? `${yieldTotal}<span class="runit">ml</span>` : (pour > 0 ? `${pour}<span class="runit">ml</span>` : '—');
  }
  document.getElementById('yieldCard').style.opacity = drinkType === 'coffee' ? '1' : (yieldTotal > 0 ? '1' : '0.45');

  document.getElementById('suggestedPrice').innerHTML = suggested ? `¥${suggested.toLocaleString()}` : '—';
  document.getElementById('suggestedSub').textContent  = suggested ? `原価率 ${rate}% で算出` : '';
  document.getElementById('costRateDisplay').innerHTML = costRate ? `${costRate}<span class="runit">%</span>` : '—';

  // ── PER-SERVING (Other only) ──
  const perServingCard = document.getElementById('perServingCard');
  if (drinkType === 'other' && yieldTotal > 0 && servingSize > 0 && total > 0) {
    const servingCount = Math.floor(yieldTotal / servingSize);
    if (servingCount > 0) {
      const perCost = Math.round(total / servingCount);
      const perPrice = rate > 0 ? Math.round(perCost / (rate / 100)) : null;
      perServingCard.style.display = 'block';
      document.getElementById('servingCountLabel').textContent = servingCount;
      document.getElementById('perServingCost').innerHTML = `${perCost}<span class="runit">円</span>`;
      document.getElementById('perServingPrice').innerHTML = perPrice ? `¥${perPrice.toLocaleString()}` : '—';
    } else {
      perServingCard.style.display = 'none';
    }
  } else {
    perServingCard.style.display = 'none';
  }

  // ── GROSS PROFIT CARD (想定粗利) ──
  const profitCard = document.getElementById('profitCard');
  if (targetPrice > 0 && total > 0) {
    profitCard.style.display = 'block';

    let profitPerUnit = targetPrice - Math.round(total);
    let unitLabel = '';
    if (drinkType === 'other' && yieldTotal > 0 && servingSize > 0) {
      const sc = Math.floor(yieldTotal / servingSize);
      if (sc > 0) {
        const perCost = Math.round(total / sc);
        profitPerUnit = targetPrice - perCost;
        unitLabel = '（1提供あたり）';
      }
    }

    if (profitPerUnit >= 0) {
      profitCard.querySelector('.rlabel').textContent = `想定粗利${unitLabel}`;
      document.getElementById('profitValue').innerHTML = `+¥${profitPerUnit.toLocaleString()}`;
      document.getElementById('profitValue').className = 'rvalue profit-positive';
      const costUsed = drinkType === 'other' && yieldTotal > 0 && servingSize > 0 ? Math.round(total / Math.floor(yieldTotal / servingSize)) : Math.round(total);
      document.getElementById('profitSub').textContent = `売価 ¥${targetPrice.toLocaleString()} − 原価 ¥${costUsed.toLocaleString()} の差額`;
      profitCard.style.background = 'var(--green)';
    } else {
      profitCard.querySelector('.rlabel').textContent = `想定損失${unitLabel}`;
      document.getElementById('profitValue').innerHTML = `−¥${Math.abs(profitPerUnit).toLocaleString()}`;
      document.getElementById('profitValue').className = 'rvalue profit-negative';
      document.getElementById('profitSub').textContent = '想定販売価格が原価を下回っています';
      profitCard.style.background = '#C62828';
    }
  } else {
    profitCard.style.display = 'none';
  }

  // ── BUDGET ──
  const budgetCard = document.getElementById('budgetCard');
  if (budget !== null) {
    budgetCard.style.display = 'block';
    document.getElementById('budgetCost').innerHTML = `${budget.toLocaleString()}<span class="runit">円</span>`;
    document.getElementById('budgetSub').textContent = `¥${targetPrice.toLocaleString()} × ${rate}% = ¥${budget.toLocaleString()}`;
    if (total > 0) {
      const pct = (total / budget) * 100;
      const bar = document.getElementById('budgetBar');
      bar.style.width = Math.min(100, pct).toFixed(1) + '%';
      bar.classList.toggle('over', pct >= 100);
      document.getElementById('budgetBarSection').style.display = 'block';
      const remaining = budget - Math.round(total);
      document.getElementById('budgetBarLabel').textContent = remaining < 0
        ? `¥${Math.abs(remaining).toLocaleString()} OVER`
        : `残 ¥${remaining.toLocaleString()}`;
    } else {
      document.getElementById('budgetBarSection').style.display = 'none';
    }
  } else {
    budgetCard.style.display = 'none';
  }
}

// ── タイプ切り替え ──
function setType(type) {
  drinkType = type;
  document.getElementById('type-coffee').classList.toggle('active', type === 'coffee');
  document.getElementById('type-other').classList.toggle('active', type === 'other');
  document.getElementById('pourLabel').textContent = type === 'coffee' ? '注湯量 (ml)' : '提供量 (ml)';
  document.getElementById('otherSettingsSection').style.display = type === 'other' ? 'block' : 'none';
  calculate();
}

// ── メニュー保存 / 更新 ──
function loadMenus() {
  if (_useSupabase) { return loadMenusFromDB(); }
  try {
    const data = localStorage.getItem(STORAGE_KEY_MENUS);
    if (data) menus = JSON.parse(data);
  } catch(e) {}
}

function saveMenusStorage() {
  try { localStorage.setItem(STORAGE_KEY_MENUS, JSON.stringify(menus)); } catch(e) {}
}

async function saveMenu() {
  const name = document.getElementById('menuName').value.trim();
  if (!name) { alert('メニュー名を入力してください'); return; }
  let totalCost = 0;
  ingredientsList.forEach(id => { totalCost += getIngredientCost(id); });
  if (totalCost === 0) { alert('材料を入力してください'); return; }

  const rate = parseFloat(document.getElementById('targetRate').value) || 0;
  const targetPrice = parseFloat(document.getElementById('targetPrice').value) || 0;
  const suggested = rate > 0 ? Math.round(totalCost / (rate / 100)) : 0;
  const costRate = targetPrice > 0 ? ((totalCost / targetPrice) * 100).toFixed(1) : (suggested > 0 ? ((totalCost / suggested) * 100).toFixed(1) : '0');
  const memo = document.getElementById('menuMemo').value.trim();
  const pourAmount = document.getElementById('pourAmount').value;
  const changeNote = (document.getElementById('changeNote')?.value || '').trim();

  const yieldTotal = parseFloat(document.getElementById('yieldTotal').value) || 0;
  const servingSize = parseFloat(document.getElementById('servingSize').value) || 0;
  let perServingCost = null, perServingPrice = null, servingCount = null;
  if (drinkType === 'other' && yieldTotal > 0 && servingSize > 0) {
    servingCount = Math.floor(yieldTotal / servingSize);
    if (servingCount > 0) {
      perServingCost = Math.round(totalCost / servingCount);
      perServingPrice = rate > 0 ? Math.round(perServingCost / (rate / 100)) : null;
    }
  }

  const costUsed = (drinkType === 'other' && perServingCost) ? perServingCost : Math.round(totalCost);
  const grossProfit = targetPrice > 0 ? targetPrice - costUsed : 0;

  const menuData = {
    name, totalCost: Math.round(totalCost), rate: costRate, price: suggested,
    type: drinkType, memo, targetPrice, perServingCost, perServingPrice, servingCount,
    ingredients: collectIngredients(),
    pourAmount, yieldTotal, servingSize, savedRate: rate,
    createdAt: Date.now()
  };

  if (_useSupabase) {
    const btn = document.getElementById('saveBtn');
    btn.disabled = true; btn.textContent = '保存中…';
    try {
      const editingMenuId = menus[editingIndex]?.id || null;
      if (editingMenuId) {
        const { error: ve } = await _sb.from('recipe_versions').insert([{
          menu_id: editingMenuId, snapshot: menuData,
          total_cost: Math.round(totalCost), cost_rate: parseFloat(costRate),
          gross_profit: grossProfit, is_current: true, change_note: changeNote||null
        }]);
        if (ve) throw ve;
        await _sb.from('menus').update({ name, drink_type:drinkType, target_price:targetPrice, target_cost_rate:rate, memo }).eq('id', editingMenuId);
      } else {
        const { data: menu, error: me } = await _sb.from('menus').insert([{
          name, drink_type:drinkType, target_price:targetPrice, target_cost_rate:rate, memo
        }]).select().single();
        if (me) throw me;
        const { error: ve } = await _sb.from('recipe_versions').insert([{
          menu_id:menu.id, snapshot:menuData, total_cost:Math.round(totalCost),
          cost_rate:parseFloat(costRate), gross_profit:grossProfit, is_current:true, change_note:changeNote||null
        }]);
        if (ve) throw ve;
      }
      showBanner('✅ 保存しました', 'sb-success');
      await loadMenusFromDB(); clearForm(); renderMenu(); switchTab('menu');
    } catch(e) { alert('保存エラー: '+e.message); }
    finally { btn.disabled=false; updateEditUI(); }
  } else {
    if (editingIndex >= 0) {
      menuData.createdAt = menus[editingIndex].createdAt || Date.now();
      menus[editingIndex] = menuData;
    } else { menus.push(menuData); }
    saveMenusStorage(); clearForm(); renderMenu(); switchTab('menu');
  }
}

// ── DELETE ──
let confirmCallback = null;

function confirmDeleteMenu(idx) {
  document.getElementById('confirmText').textContent = `「${menus[idx].name}」を削除しますか？`;
  confirmCallback = () => {
    menus.splice(idx, 1);
    saveMenusStorage();
    renderMenu();
    // If we were editing the deleted item, cancel edit
    if (editingIndex === idx) {
      editingIndex = -1;
      clearForm();
    } else if (editingIndex > idx) {
      editingIndex--;
    }
  };
  document.getElementById('confirmDialog').classList.add('open');
}

function confirmAction() {
  if (confirmCallback) confirmCallback();
  confirmCallback = null;
  closeConfirm();
}

function closeConfirm() {
  document.getElementById('confirmDialog').classList.remove('open');
}

function renderMenu() {
  const list = document.getElementById('menuList');
  const summary = document.getElementById('menuSummary');
  if (menus.length === 0) {
    summary.style.display = 'none';
    list.innerHTML = `<div class="menu-empty">保存されたメニューはありません</div>`;
    return;
  }
  const avgRate = (menus.reduce((a, b) => a + parseFloat(b.rate || 0), 0) / menus.length).toFixed(1);
  summary.style.display = 'block';
  summary.innerHTML = `<div class="total-bar"><div><div class="total-bar-label">登録メニュー数</div><div class="total-bar-value">${menus.length} 品目</div></div><div style="text-align:right"><div class="total-bar-label">平均原価率</div><div class="total-bar-value">${avgRate}%</div></div></div>`;

  list.innerHTML = menus.map((m, idx) => {
    const memoHtml = m.memo ? `<div class="menu-item-memo">📝 ${m.memo}</div>` : '';
    const perServingHtml = m.perServingCost ? `<div class="mstat">${m.servingCount}杯分 <span>1杯 ¥${m.perServingCost}</span></div>` : '';
    const profitHtml = m.targetPrice > 0 ? (() => {
      const cost = m.perServingCost || m.totalCost;
      const diff = m.targetPrice - cost;
      return diff >= 0
        ? `<div class="mstat" style="color:var(--green);">粗利 <span style="color:var(--green);">+¥${diff.toLocaleString()}</span></div>`
        : `<div class="mstat" style="color:#D32F2F;">損失 <span style="color:#D32F2F;">−¥${Math.abs(diff).toLocaleString()}</span></div>`;
    })() : '';

    return `
    <div class="menu-item">
      <div class="menu-item-accent"></div>
      <div class="menu-item-body">
        <div class="menu-item-name">${m.name}</div>
        ${memoHtml}
        <div class="menu-item-stats">
          <div class="mstat">原価 <span>${m.totalCost}円</span></div>
          <div class="mstat">原価率 <span>${m.rate}%</span></div>
          <div class="mstat">${m.type === 'coffee' ? 'COFFEE' : 'OTHER'}</div>
          ${perServingHtml}
          ${profitHtml}
        </div>
      </div>
      <div class="menu-item-price">
        <div class="menu-price-label">推奨価格</div>
        <div class="menu-price">¥${m.price.toLocaleString()}</div>
      </div>
      <div class="menu-item-actions">
        ${_useSupabase && m.id ? `<button class="menu-action-btn history" onclick="openVersionModal('${m.id}','${m.name.replace(/'/g,"\\'")}')">🕐</button>` : ''}
        <button class="menu-action-btn" onclick="loadMenuIntoForm(${idx})" title="編集">✎</button>
        <button class="menu-action-btn del" onclick="confirmDeleteMenu(${idx})" title="削除">×</button>
      </div>
    </div>`;
  }).join('');
}

function switchTab(tab) {
  document.getElementById('tab-recipe').style.display = tab === 'recipe' ? 'block' : 'none';
  document.getElementById('tab-menu').style.display = tab === 'menu' ? 'block' : 'none';
  document.querySelectorAll('.tab').forEach((t, i) => {
    t.classList.toggle('active', (i === 0 && tab === 'recipe') || (i === 1 && tab === 'menu'));
  });
  if (tab === 'menu') renderMenu();
}

// ── 初期化 ──
loadCustomSku();
updateExcelStatus();
addIngredient();
(async function() {
  await initSupabase();
  await loadMenus();
  renderMenu();
})();
</script>
</body>
</html>
