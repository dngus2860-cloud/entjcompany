[reward_index.html](https://github.com/user-attachments/files/22911210/reward_index.html)
<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<title>엔티제컴퍼니 · 작업 대시보드</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<style>
  :root{
    --bg:#0f0f23; --panel:#1a1a2e; --card:#16213e; --card2:#1f2d47;
    --text:#e4e8f0; --muted:#8b94a8; --border:#2a3551; --chip:#2d3a5c;
    --primary:#6366f1; --primary-light:#818cf8; --green:#22c55e; --red:#ef4444; --amber:#f59e0b;
    --shadow:0 20px 60px rgba(0,0,0,0.5);
    --gradient-1:linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  }
  [data-theme="light"]{
    --bg:#f5f7fa; --panel:#ffffff; --card:#ffffff; --card2:#f8fafc;
    --text:#1e293b; --muted:#64748b; --border:#e2e8f0; --chip:#e0e7ff;
    --primary:#6366f1; --primary-light:#818cf8; --green:#16a34a; --red:#dc2626; --amber:#d97706;
    --shadow:0 10px 40px rgba(0,0,0,0.08);
  }
  *{box-sizing:border-box; margin:0; padding:0}
  body{
    background:var(--bg); color:var(--text);
    font-family:'Pretendard',-apple-system,system-ui,sans-serif; line-height:1.6;
  }
  .wrap{max-width:1400px; margin:0 auto; padding:24px; display:grid; gap:20px}
  
  header{
    background:var(--panel); border:1px solid var(--border); border-radius:20px;
    padding:24px 32px; box-shadow:var(--shadow); display:flex;
    align-items:center; justify-content:space-between; position:relative; overflow:hidden;
  }
  header::before{content:''; position:absolute; top:0; left:0; right:0; height:4px; background:var(--gradient-1)}
  .brand{display:flex; align-items:center; gap:16px}
  .brand .icon{font-size:36px; filter:drop-shadow(0 4px 8px rgba(99,102,241,0.4))}
  .brand h1{font-size:24px; font-weight:800; background:var(--gradient-1); -webkit-background-clip:text; -webkit-text-fill-color:transparent}
  .chip{background:var(--chip); border:1px solid var(--border); color:var(--primary-light);
    padding:6px 14px; border-radius:999px; font-size:12px; font-weight:600}
  
  .btn{background:var(--primary); color:white; border:none; padding:10px 20px;
    border-radius:12px; cursor:pointer; font-weight:600; font-size:14px;
    transition:all 0.3s; box-shadow:0 4px 12px rgba(99,102,241,0.3)}
  .btn:hover{background:var(--primary-light); transform:translateY(-2px)}
  .btn.outline{background:transparent; color:var(--text); border:2px solid var(--border); box-shadow:none}
  .btn.outline:hover{border-color:var(--primary); color:var(--primary); background:rgba(99,102,241,0.1)}
  .btn.danger{background:#ef4444}
  .btn.small{padding:8px 14px; font-size:13px}
  
  .panel{background:var(--panel); border:1px solid var(--border); border-radius:20px; box-shadow:var(--shadow); overflow:hidden}
  .panel .h{padding:20px 28px; border-bottom:1px solid var(--border); display:flex;
    align-items:center; justify-content:space-between; background:linear-gradient(to right, rgba(99,102,241,0.05), transparent)}
  .panel .h strong{font-size:18px; font-weight:700}
  .panel .b{padding:24px 28px}
  
  .filters{display:grid; grid-template-columns:1.4fr repeat(3,1fr) 0.9fr 1fr; gap:12px}
  @media (max-width:1100px){.filters{grid-template-columns:1fr 1fr}}
  
  .filter-input-wrapper{position:relative}
  input, select{width:100%; padding:12px 16px; border-radius:12px; border:2px solid var(--border);
    background:var(--card); color:var(--text); font-size:14px; font-weight:500; transition:all 0.3s}
  input:focus, select:focus{outline:none; border-color:var(--primary); box-shadow:0 0 0 3px rgba(99,102,241,0.1)}
  
  .autocomplete-dropdown{position:absolute; top:100%; left:0; right:0; background:var(--panel);
    border:2px solid var(--border); border-radius:12px; margin-top:4px; max-height:250px;
    overflow-y:auto; z-index:100; box-shadow:var(--shadow); display:none}
  .autocomplete-dropdown.active{display:block}
  .autocomplete-item{padding:10px 16px; cursor:pointer; display:flex; align-items:center;
    justify-content:space-between; transition:all 0.2s; border-bottom:1px solid var(--border)}
  .autocomplete-item:last-child{border-bottom:none}
  .autocomplete-item:hover{background:var(--card2)}
  .autocomplete-text{flex:1; font-size:14px}
  .autocomplete-delete{width:20px; height:20px; border-radius:50%; background:transparent;
    color:var(--muted); border:none; cursor:pointer; font-size:16px; opacity:0; transition:all 0.2s}
  .autocomplete-item:hover .autocomplete-delete{opacity:1}
  .autocomplete-delete:hover{background:var(--red); color:white; transform:scale(1.1)}
  
  .date-filter-wrap{display:flex; flex-direction:column; gap:6px}
  .period-btns{display:flex; gap:6px}
  .period-btn{flex:1; padding:6px 10px; font-size:12px; font-weight:600; border-radius:8px;
    border:2px solid var(--border); background:var(--card); color:var(--muted);
    cursor:pointer; transition:all 0.3s}
  .period-btn.active{background:var(--primary); color:white; border-color:var(--primary)}
  .period-btn:hover:not(.active){background:var(--card2); border-color:var(--primary-light)}
  
  .filter-reset-all{grid-column:1/-1; display:flex; justify-content:flex-end; margin-top:-4px}
  .toolbar{display:flex; gap:10px; align-items:center; flex-wrap:wrap}
  .sep{height:1px; background:var(--border); margin:20px 0}
  .muted{color:var(--muted); font-size:13px}
  
  table{width:100%; border-collapse:separate; border-spacing:0}
  thead th{position:sticky; top:0; background:var(--card2); z-index:2; border-bottom:2px solid var(--border);
    padding:14px 12px; font-size:13px; font-weight:700; text-transform:uppercase;
    letter-spacing:0.5px; color:var(--muted); white-space:nowrap}
  tbody tr{transition:all 0.2s; border-bottom:1px solid var(--border)}
  tbody tr:hover{background:var(--card2); transform:scale(1.001)}
  td{padding:14px 12px; font-size:14px; vertical-align:middle}
  .num{text-align:right; font-variant-numeric:tabular-nums; font-weight:600}
  .center{text-align:center}
  .nowrap{white-space:nowrap; overflow:hidden; text-overflow:ellipsis; max-width:260px}
  .url a{color:var(--primary-light); text-decoration:none; font-weight:600; transition:color 0.2s}
  .url a:hover{color:var(--primary); text-decoration:underline}
  .select-row{width:18px; height:18px; cursor:pointer}
  
  tr.deadline-warning{border-left:4px solid var(--amber); background:rgba(245,158,11,0.1);
    animation:pulse 2s ease-in-out infinite}
  @keyframes pulse{0%,100%{background:rgba(245,158,11,0.1)} 50%{background:rgba(245,158,11,0.2)}}
  
  .cell-editor{width:100%; border-radius:8px; padding:8px 12px; border:2px solid var(--primary);
    background:var(--card); color:var(--text); font-size:14px}
  .badge-yes{color:var(--green); font-weight:700}
  .badge-no{color:var(--amber); font-weight:700}
  
  .bulk-edit-bar{background:linear-gradient(135deg, rgba(99,102,241,0.1), rgba(139,92,246,0.1));
    border:2px solid var(--primary); border-radius:16px; padding:16px 20px; margin-bottom:16px;
    display:none; align-items:center; gap:16px; animation:slideDown 0.3s}
  .bulk-edit-bar.active{display:flex}
  @keyframes slideDown{from{opacity:0; transform:translateY(-10px)} to{opacity:1; transform:translateY(0)}}
  
  .pagination-wrapper{display:flex; align-items:center; justify-content:space-between;
    padding:20px 0; gap:16px; flex-wrap:wrap}
  .pagination{display:flex; gap:8px; align-items:center}
  .page-btn{min-width:36px; height:36px; padding:0 12px; border:2px solid var(--border);
    background:var(--card); color:var(--text); border-radius:10px; cursor:pointer;
    font-weight:600; transition:all 0.3s}
  .page-btn:hover:not(.active):not(:disabled){border-color:var(--primary); background:var(--card2)}
  .page-btn.active{background:var(--primary); color:white; border-color:var(--primary)}
  .page-btn:disabled{opacity:0.3; cursor:not-allowed}
  .per-page-selector{display:flex; align-items:center; gap:10px}
  .per-page-selector select{width:auto; padding:8px 12px}
  
  .kpis{display:grid; grid-template-columns:repeat(4,1fr); gap:12px}
  .kpi{padding:18px; background:var(--card2); border:2px solid var(--border); border-radius:16px;
    transition:all 0.3s; position:relative; overflow:hidden}
  .kpi::before{content:''; position:absolute; top:0; left:0; right:0; height:3px;
    background:var(--gradient-1); opacity:0; transition:opacity 0.3s}
  .kpi:hover{border-color:var(--primary); transform:translateY(-2px)}
  .kpi:hover::before{opacity:1}
  .kpi .v{font-weight:900; font-size:24px; background:var(--gradient-1);
    -webkit-background-clip:text; -webkit-text-fill-color:transparent}
  
  .bars{display:flex; align-items:flex-end; gap:10px; height:180px; padding:12px;
    border:2px solid var(--border); border-radius:16px; background:var(--card2)}
  .bars .col{flex:1; display:flex; flex-direction:column; align-items:center; gap:8px; min-width:60px}
  .bars .bar{width:100%; max-width:50px; background:var(--gradient-1); border:2px solid var(--border);
    border-radius:12px 12px 6px 6px; height:10%; box-shadow:0 4px 12px rgba(99,102,241,0.2); transition:all 0.3s}
  .bars .col:hover .bar{transform:translateY(-4px)}
  .bar-value{font-size:13px; font-weight:700; color:var(--text); margin-bottom:4px}
  .note{font-size:11px; color:var(--muted); font-weight:600}
  
  .product-멘토스 .bar{background:linear-gradient(180deg, #8B5CF6, #7C3AED) !important}
  .product-호올스 .bar{background:linear-gradient(180deg, #C4B5FD, #A78BFA) !important}
  .product-말차 .bar{background:linear-gradient(180deg, #10B981, #059669) !important}
  
  .monthly-summary{font-size:14px; line-height:1.8}
  .month-item{margin-bottom:8px; padding:8px 12px; background:var(--card2); border-radius:10px;
    border-left:3px solid var(--primary); transition:all 0.2s; cursor:pointer;
    display:flex; justify-content:space-between; align-items:center}
  .month-item:hover{background:var(--card); transform:translateX(4px)}
  .month-item-icon{color:var(--primary); font-size:16px; opacity:0.6; transition:opacity 0.2s}
  .month-item:hover .month-item-icon{opacity:1}
  
  .modal-back{position:fixed; inset:0; background:rgba(0,0,0,0.7); backdrop-filter:blur(4px);
    display:none; align-items:center; justify-content:center; z-index:1000}
  .modal-back.active{display:flex}
  .modal{background:var(--panel); border:2px solid var(--border); border-radius:24px;
    box-shadow:0 30px 80px rgba(0,0,0,0.6); width:800px; max-width:94vw;
    max-height:90vh; overflow:auto; animation:modalIn 0.3s}
  @keyframes modalIn{from{opacity:0; transform:scale(0.9) translateY(20px)}
    to{opacity:1; transform:scale(1) translateY(0)}}
  .modal .mh{padding:24px 28px; border-bottom:2px solid var(--border);
    display:flex; align-items:center; justify-content:space-between;
    background:linear-gradient(to right, rgba(99,102,241,0.05), transparent)}
  .modal .mh strong{font-size:20px; font-weight:800}
  .modal .mb{padding:28px}
  .form-grid{display:grid; grid-template-columns:1fr 1fr; gap:16px}
  .form-grid .full{grid-column:1/-1}
  .form-grid label{display:block; font-size:13px; font-weight:600; color:var(--muted);
    margin-bottom:6px; text-transform:uppercase; letter-spacing:0.5px}
  
  .chart-modal-content{width:600px}
  .chart-modal-close{position:absolute; top:20px; right:20px; width:32px; height:32px;
    border-radius:50%; background:var(--card2); border:2px solid var(--border);
    color:var(--text); cursor:pointer; display:flex; align-items:center; justify-content:center;
    font-size:18px; transition:all 0.2s}
  .chart-modal-close:hover{background:var(--red); color:white; transform:rotate(90deg)}
  .chart-title{font-size:20px; font-weight:800; margin-bottom:24px; text-align:center}
  .pie-chart-container{display:flex; justify-content:center; margin:20px 0}
  .pie-legend{display:grid; grid-template-columns:repeat(2,1fr); gap:12px; margin-top:24px}
  .pie-legend-item{display:flex; align-items:center; gap:10px; padding:8px 12px;
    background:var(--card2); border-radius:10px; transition:all 0.2s}
  .pie-legend-item:hover{background:var(--card); transform:translateX(2px)}
  .pie-legend-color{width:20px; height:20px; border-radius:6px; border:2px solid var(--border)}
  .pie-legend-info{flex:1}
  .pie-legend-label{font-size:13px; font-weight:600}
  .pie-legend-value{font-size:12px; color:var(--muted)}
</style>
</head>
<body>
<div class="wrap" id="app">
  <header>
    <div class="brand">
      <span class="icon">💼</span>
      <div>
        <h1>엔티제컴퍼니</h1>
        <span class="chip">작업 대시보드</span>
      </div>
    </div>
    <div class="toolbar">
      <button class="btn outline" id="exportExcelBtn">📊 엑셀 내보내기</button>
      <button class="btn" id="themeBtn">🌓 테마 전환</button>
    </div>
  </header>

  <section class="panel">
    <div class="h">
      <strong>🔍 필터 & 등록</strong>
      <div class="toolbar">
        <button class="btn small" id="openAddModalBtn">➕ 새 항목</button>
        <button class="btn danger small" id="removeSelectedBtn">🗑️ 선택 삭제</button>
      </div>
    </div>
    <div class="b">
      <div class="filters">
        <div class="filter-input-wrapper" id="qWrapper">
          <input id="q" placeholder="🔎 검색: 업체명/키워드/mid" autocomplete="off">
          <div class="autocomplete-dropdown" id="qAutocomplete"></div>
        </div>
        <select id="fProduct">
          <option value="all">상품: 전체</option>
          <option>멘토스</option><option>말차</option><option>호올스</option><option>신규</option>
        </select>
        <select id="fCategory">
          <option value="all">구분: 전체</option>
          <option>트래픽</option><option>저장</option><option>길찾기</option><option>영수증</option>
        </select>
        <select id="fTax">
          <option value="all">세금계산서: 전체</option>
          <option value="yes">발행</option><option value="no">미발행</option>
        </select>
        <div class="date-filter-wrap">
          <input type="date" id="fDate" autocomplete="off">
          <div class="period-btns">
            <button class="period-btn active" data-period="day">일별</button>
            <button class="period-btn" data-period="week">주별</button>
            <button class="period-btn" data-period="month">월별</button>
          </div>
        </div>
        <select id="sortSel">
          <option value="date_desc">정렬: 최신일자</option>
          <option value="date_asc">정렬: 오래된일자</option>
          <option value="cost_desc">정렬: 작업비 ⬇</option>
          <option value="cost_asc">정렬: 작업비 ⬆</option>
          <option value="client_asc">정렬: 업체명 (가→하)</option>
        </select>
        <div class="filter-reset-all">
          <button class="btn small outline" id="resetFiltersBtn">🔄 필터 초기화</button>
        </div>
      </div>
      <div class="sep"></div>
      <div class="muted">💡 표 셀 클릭 시 바로 수정 (Enter=저장, Esc=취소)</div>
    </div>
  </section>

  <section class="panel">
    <div class="h">
      <strong>📋 작업 목록</strong>
      <span class="muted" id="countInfo">0건</span>
    </div>
    <div class="b">
      <div class="bulk-edit-bar" id="bulkEditBar">
        <span class="muted" id="selectedCount">0개 선택</span>
        <button class="btn small" id="bulkEditBtn">✏️ 일괄수정</button>
      </div>
      <table id="tbl">
        <thead>
          <tr>
            <th class="center"><input type="checkbox" id="chkAll" class="select-row"></th>
            <th>상품</th><th>구분</th><th>업체명</th><th>mid</th><th>키워드</th>
            <th>시작일</th><th>종료일</th><th class="num">일타</th><th class="num">일수</th>
            <th class="num">단가</th><th class="num">총량</th><th class="num">작업비</th>
            <th class="center">세금</th><th>비고</th><th class="center">⚙️</th>
          </tr>
        </thead>
        <tbody id="tbody"></tbody>
      </table>
      <div class="pagination-wrapper">
        <div class="per-page-selector">
          <span class="muted">페이지당 표시:</span>
          <select id="perPageSelect">
            <option value="30" selected>30개</option>
            <option value="50">50개</option>
            <option value="100">100개</option>
          </select>
        </div>
        <div class="pagination" id="pagination"></div>
      </div>
    </div>
  </section>

  <section class="panel">
    <div class="h"><strong>📊 데이터 요약</strong></div>
    <div class="b">
      <div class="kpis">
        <div class="kpi"><div class="muted">총 건수</div><div class="v" id="kpiCount">0</div></div>
        <div class="kpi"><div class="muted">총 작업량</div><div class="v" id="kpiQty">0</div></div>
        <div class="kpi"><div class="muted">총 작업비</div><div class="v" id="kpiCost">₩0</div></div>
        <div class="kpi"><div class="muted">평균 단가</div><div class="v" id="kpiAvg">₩0</div></div>
      </div>
      <div class="sep"></div>
      <div class="toolbar"><strong>📈 상품별 총 작업비</strong></div>
      <div class="bars" id="barsByProduct"></div>
      <div class="sep"></div>
      <div class="toolbar"><strong>💰 월 총 작업비</strong><span class="note">(클릭하여 상세 보기)</span></div>
      <div class="monthly-summary" id="monthlyCost"></div>
    </div>
  </section>
</div>

<div class="modal-back" id="chartModal">
  <div class="modal chart-modal-content">
    <button class="chart-modal-close" id="closeChartModal">✕</button>
    <div class="chart-title" id="chartTitle">월별 상품 분석</div>
    <div class="pie-chart-container">
      <canvas id="pieChart" width="300" height="300"></canvas>
    </div>
    <div class="pie-legend" id="pieLegend"></div>
  </div>
</div>

<div class="modal-back" id="addModalBack">
  <div class="modal">
    <div class="mh">
      <strong>➕ 새 항목 등록</strong>
      <button class="btn small outline" onclick="closeAddModal()">✕ 닫기</button>
    </div>
    <div class="mb">
      <form id="addForm" class="form-grid">
        <div><label>상품</label>
          <select id="aProduct" required>
            <option value="">선택</option>
            <option>멘토스</option><option>말차</option><option>호올스</option><option>신규</option>
          </select>
        </div>
        <div><label>구분</label>
          <select id="aCategory" required>
            <option value="">선택</option>
            <option>트래픽</option><option>저장</option><option>길찾기</option><option>영수증</option>
          </select>
        </div>
        <div class="full"><label>업체명</label><input id="aClient" required></div>
        <div class="full"><label>mid(URL)</label><input id="aMid" type="url"></div>
        <div class="full"><label>키워드</label><input id="aKeywords"></div>
        <div><label>시작일</label><input id="aStartDate" type="date" required></div>
        <div><label>종료일</label><input id="aEndDate" type="date"></div>
        <div><label>일타</label><input id="aUnits" type="number" min="0"></div>
        <div><label>일수</label><input id="aDays" type="number" min="0"></div>
        <div><label>단가</label><input id="aUnitPrice" type="number" step="any" min="0"></div>
        <div><label>세금계산서</label>
          <select id="aTax"><option>미발행</option><option>발행</option></select>
        </div>
        <div class="full"><label>비고</label><input id="aNote"></div>
        <div class="full" style="display:flex; gap:12px; justify-content:flex-end; margin-top:8px">
          <button type="button" class="btn outline" onclick="resetAddForm()">🔄 초기화</button>
          <button type="submit" class="btn">✓ 등록</button>
        </div>
      </form>
    </div>
  </div>
</div>

<div class="modal-back" id="bulkEditModalBack">
  <div class="modal">
    <div class="mh">
      <strong>✏️ 일괄수정</strong>
      <button class="btn small outline" onclick="closeBulkEditModal()">✕ 닫기</button>
    </div>
    <div class="mb">
      <div class="muted" style="margin-bottom:16px">선택된 <strong id="bulkCountText">0</strong>개 항목을 일괄 수정합니다.</div>
      <form id="bulkEditForm" class="form-grid">
        <div><label>상품</label>
          <select id="bProduct"><option value="">변경 안함</option>
            <option>멘토스</option><option>말차</option><option>호올스</option><option>신규</option>
          </select>
        </div>
        <div><label>구분</label>
          <select id="bCategory"><option value="">변경 안함</option>
            <option>트래픽</option><option>저장</option><option>길찾기</option><option>영수증</option>
          </select>
        </div>
        <div class="full"><label>업체명</label><input id="bClient"></div>
        <div class="full"><label>mid(URL)</label><input id="bMid" type="url"></div>
        <div class="full"><label>키워드</label><input id="bKeywords"></div>
        <div><label>시작일</label><input id="bStartDate" type="date"></div>
        <div><label>종료일</label><input id="bEndDate" type="date"></div>
        <div><label>일타</label><input id="bUnits" type="number" min="0"></div>
        <div><label>일수</label><input id="bDays" type="number" min="0"></div>
        <div><label>단가</label><input id="bUnitPrice" type="number" step="any" min="0"></div>
        <div><label>세금계산서</label>
          <select id="bTax"><option value="">변경 안함</option><option>발행</option><option>미발행</option></select>
        </div>
        <div class="full"><label>비고</label><input id="bNote"></div>
        <div class="full" style="display:flex; gap:12px; justify-content:flex-end; margin-top:8px">
          <button type="button" class="btn outline" onclick="resetBulkForm()">🔄 초기화</button>
          <button type="submit" class="btn">✓ 일괄 적용</button>
        </div>
      </form>
    </div>
  </div>
</div>

<script>
const $ = s => document.querySelector(s);
const $$ = s => document.querySelectorAll(s);
const fmtMoney = n => '₩' + Math.round(n||0).toLocaleString('ko-KR');
const parseNum = v => v===''||v==null? null : (isNaN(Number(v))? null : Number(v));
const toISO = d => {if(!d)return''; const z=new Date(d); return new Date(z-z.getTimezoneOffset()*60000).toISOString().slice(0,10)};
const today = () => toISO(new Date());

const storageKey = 'ntz-company-ops-v4';
const searchHistoryKey = 'ntz-search-history';
const state = {
  rows: [],
  prefs: {theme:'auto'},
  filters: {q:'', product:'all', category:'all', tax:'all', date:'', periodType:'day', sort:'date_desc'},
  selected: new Set(),
  pagination: {currentPage:1, perPage:30}
};

function getWeekNumber(d){
  const date=new Date(d); date.setHours(0,0,0,0);
  date.setDate(date.getDate()+4-(date.getDay()||7));
  const yearStart=new Date(date.getFullYear(),0,1);
  return {year:date.getFullYear(), week:Math.ceil((((date-yearStart)/86400000)+1)/7)};
}
function getYearMonth(d){const date=new Date(d); return `${date.getFullYear()}-${String(date.getMonth()+1).padStart(2,'0')}`}

function mk(p){
  const id='r_'+Math.random().toString(36).slice(2,9);
  const createdAt=new Date().toISOString();
  const row={id, createdAt, ...p};
  row.totalQty=(row.units||0)*(row.days||0);
  row.cost=row.totalQty*(row.unitPrice||0);
  return row;
}

function seed(){
  state.rows=[
    mk({product:'멘토스',category:'트래픽',client:'아이프린스 울산점',mid:'https://place.naver.com/place/zzz',
      keywords:'동탄 휴대폰',startDate:'2025-09-10',endDate:'2025-09-17',units:100,days:7,unitPrice:27.5,tax:'발행'}),
    mk({product:'호올스',category:'저장',client:'청주에어컨설치',mid:'https://place.naver.com/place/abc',
      keywords:'청주 에어컨',startDate:'2025-09-12',endDate:'2025-09-19',units:100,days:7,unitPrice:27.5,tax:'미발행'}),
    mk({product:'말차',category:'길찾기',client:'부산치과',mid:'https://place.naver.com/hospital/yyy',
      keywords:'송파치과',startDate:'2025-09-16',endDate:'2025-10-06',units:5,days:20,unitPrice:2090,tax:'미발행'}),
    mk({product:'신규',category:'영수증',client:'브루브루',mid:'https://place.naver.com/restaurant/xxx',
      keywords:'포토리뷰',startDate:'2025-10-06',endDate:'2025-11-05',units:5,days:30,unitPrice:2090,tax:'미발행'})
  ];
}

function load(){
  try{
    const raw=localStorage.getItem(storageKey);
    if(raw){const obj=JSON.parse(raw); state.rows=obj.rows||[]; state.prefs={...state.prefs,...(obj.prefs||{})}}
    if(!state.rows.length)seed();
  }catch(e){seed()}
  applyTheme(state.prefs.theme||'auto');
  if(!localStorage.getItem('ntz-ac-cleared')){
    localStorage.removeItem(searchHistoryKey);
    localStorage.setItem('ntz-ac-cleared','true');
  }
}

function save(){localStorage.setItem(storageKey,JSON.stringify({rows:state.rows,prefs:state.prefs}))}

function getSearchHistory(){try{return JSON.parse(localStorage.getItem(searchHistoryKey)||'[]')}catch(e){return[]}}
function saveSearchHistory(q){
  if(!q||!q.trim())return;
  let h=getSearchHistory().filter(x=>x!==q);
  h.unshift(q); h=h.slice(0,20);
  localStorage.setItem(searchHistoryKey,JSON.stringify(h));
}
function deleteSearchHistoryItem(q){
  let h=getSearchHistory().filter(x=>x!==q);
  localStorage.setItem(searchHistoryKey,JSON.stringify(h));
  renderAutocomplete();
}
function renderAutocomplete(){
  const dd=$('#qAutocomplete'), h=getSearchHistory();
  if(!h.length){dd.innerHTML='<div style="padding:16px;text-align:center;color:var(--muted)">검색 기록 없음</div>'; return}
  dd.innerHTML=h.map(q=>`<div class="autocomplete-item" data-query="${q.replace(/"/g,'&quot;')}">
    <span class="autocomplete-text">${q}</span><button class="autocomplete-delete">✕</button></div>`).join('');
  $$('.autocomplete-item').forEach(item=>{
    const q=item.dataset.query;
    item.querySelector('.autocomplete-text').onclick=e=>{
      e.stopPropagation(); $('#q').value=q; state.filters.q=q;
      state.pagination.currentPage=1; dd.classList.remove('active'); render();
    };
    item.querySelector('.autocomplete-delete').onclick=e=>{e.stopPropagation(); deleteSearchHistoryItem(q)};
  });
}

function applyFilters(rows){
  let arr=rows.slice(); const f=state.filters;
  if(f.q){const q=f.q.toLowerCase(); arr=arr.filter(r=>[r.client,r.keywords,r.mid,r.product,r.category,r.note].join(' ').toLowerCase().includes(q))}
  if(f.product!=='all')arr=arr.filter(r=>r.product===f.product);
  if(f.category!=='all')arr=arr.filter(r=>r.category===f.category);
  if(f.tax!=='all')arr=arr.filter(r=>(f.tax==='yes'?r.tax==='발행':r.tax==='미발행'));
  if(f.date){
    if(f.periodType==='day')arr=arr.filter(r=>r.startDate===f.date);
    else if(f.periodType==='week'){const tw=getWeekNumber(f.date); arr=arr.filter(r=>{if(!r.startDate)return false; const rw=getWeekNumber(r.startDate); return rw.year===tw.year&&rw.week===tw.week})}
    else if(f.periodType==='month'){const tm=getYearMonth(f.date); arr=arr.filter(r=>r.startDate&&getYearMonth(r.startDate)===tm)}
  }
  const[sf,dir]=f.sort.split('_'), mul=dir==='asc'?1:-1;
  arr.sort((a,b)=>{
    if(sf==='date')return(a.startDate||'').localeCompare(b.startDate||'')*mul;
    if(sf==='cost')return((a.cost||0)-(b.cost||0))*mul;
    if(sf==='client')return(a.client||'').localeCompare(b.client||'','ko')*mul;
    return 0;
  });
  return arr;
}

function render(){renderTable(); renderKpis(); updateBulkEditBar(); save()}

function renderTable(){
  const tbody=$('#tbody'); tbody.innerHTML='';
  const allRows=applyFilters(state.rows);
  const{currentPage,perPage}=state.pagination;
  const totalPages=Math.ceil(allRows.length/perPage);
  const startIdx=(currentPage-1)*perPage;
  const rows=allRows.slice(startIdx,startIdx+perPage);
  $('#countInfo').textContent=`${allRows.length}건 (${currentPage}/${totalPages} 페이지)`;
  
  const today=new Date(); today.setHours(0,0,0,0);
  const tomorrow=new Date(today); tomorrow.setDate(tomorrow.getDate()+1);
  
  rows.forEach(r=>{
    const tr=document.createElement('tr');
    if(r.endDate){const ed=new Date(r.endDate); ed.setHours(0,0,0,0); if(ed.getTime()===tomorrow.getTime())tr.classList.add('deadline-warning')}
    
    const tdSel=document.createElement('td'); tdSel.className='center';
    const cb=document.createElement('input'); cb.type='checkbox'; cb.className='select-row';
    cb.checked=state.selected.has(r.id);
    cb.onchange=()=>{if(cb.checked)state.selected.add(r.id); else state.selected.delete(r.id); updateBulkEditBar()};
    tdSel.appendChild(cb); tr.appendChild(tdSel);
    
    addCell(tr,r,'product'); addCell(tr,r,'category'); addCell(tr,r,'client',null,'nowrap');
    const tdMid=document.createElement('td'); tdMid.className='url nowrap';
    if(r.mid){const a=document.createElement('a'); a.href=r.mid; a.target='_blank'; a.textContent='열기'; tdMid.appendChild(a)}
    tdMid.onclick=()=>startEdit(tdMid,r,'mid'); tr.appendChild(tdMid);
    addCell(tr,r,'keywords',null,'nowrap'); addCell(tr,r,'startDate'); addCell(tr,r,'endDate');
    addCell(tr,r,'units',null,'num'); addCell(tr,r,'days',null,'num'); addCell(tr,r,'unitPrice',null,'num');
    const tdQty=document.createElement('td'); tdQty.className='num'; tdQty.textContent=r.totalQty||''; tr.appendChild(tdQty);
    const tdCost=document.createElement('td'); tdCost.className='num'; tdCost.textContent=fmtMoney(r.cost||0); tr.appendChild(tdCost);
    const tdTax=document.createElement('td'); tdTax.className='center '+(r.tax==='발행'?'badge-yes':'badge-no');
    tdTax.textContent=r.tax||''; tdTax.onclick=()=>startEdit(tdTax,r,'tax'); tr.appendChild(tdTax);
    addCell(tr,r,'note',null,'nowrap');
    
    const tdAct=document.createElement('td'); tdAct.className='center';
    const dup=document.createElement('button'); dup.className='btn small outline'; dup.textContent='복제';
    dup.onclick=()=>{const c=JSON.parse(JSON.stringify(r)); c.id='r_'+Math.random().toString(36).slice(2,9); c.createdAt=new Date().toISOString(); state.rows.unshift(c); render()};
    const del=document.createElement('button'); del.className='btn small danger'; del.textContent='삭제';
    del.onclick=()=>{if(confirm('삭제할까요?')){state.rows=state.rows.filter(x=>x.id!==r.id); state.selected.delete(r.id); render()}};
    tdAct.append(dup,' ',del); tr.appendChild(tdAct);
    tbody.appendChild(tr);
  });
  
  const allFiltered=applyFilters(state.rows);
  $('#chkAll').checked=allFiltered.length>0&&allFiltered.every(r=>state.selected.has(r.id));
  renderPagination(totalPages);
}

function addCell(tr,row,field,customRenderer,extraClass,readOnly){
  const val=row[field];
  if(customRenderer){const td=customRenderer(val); if(!readOnly)td.onclick=()=>startEdit(td,row,field); tr.appendChild(td); return}
  const td=document.createElement('td'); if(extraClass)td.className=extraClass;
  td.textContent=val??''; if(!readOnly)td.onclick=()=>startEdit(td,row,field); tr.appendChild(td);
}

function startEdit(td,row,field){
  const cols={product:{type:'select',options:['멘토스','말차','호올스','신규']},category:{type:'select',options:['트래픽','저장','길찾기','영수증']},
    client:{type:'text'},mid:{type:'url'},keywords:{type:'text'},startDate:{type:'date'},endDate:{type:'date'},
    units:{type:'number'},days:{type:'number'},unitPrice:{type:'number'},tax:{type:'select',options:['발행','미발행']},note:{type:'text'}};
  const col=cols[field]; if(!col)return;
  const old=row[field]??'';
  let el;
  if(col.type==='select'){
    el=document.createElement('select'); el.className='cell-editor';
    if(field==='tax')el.innerHTML='<option>발행</option><option>미발행</option>';
    else el.innerHTML=col.options.map(v=>`<option>${v}</option>`).join('');
    el.value=old;
  }else{
    el=document.createElement('input'); el.className='cell-editor';
    el.type=col.type==='text'?'text':col.type; el.value=old;
    if(col.type==='number')el.step='any';
  }
  td.innerHTML=''; td.appendChild(el); el.focus(); if(el.select)el.select();
  const commit=()=>{
    let v=el.value;
    if(col.type==='number')v=parseNum(v);
    row[field]=v||'';
    row.totalQty=(row.units||0)*(row.days||0);
    row.cost=row.totalQty*(row.unitPrice||0);
    render();
  };
  el.onkeydown=e=>{if(e.key==='Enter'){e.preventDefault();commit()}else if(e.key==='Escape'){e.preventDefault();render()}};
  el.onblur=commit;
}

function renderPagination(totalPages){
  const c=$('#pagination'); c.innerHTML=''; if(totalPages<=1)return;
  const{currentPage}=state.pagination;
  const prev=document.createElement('button'); prev.className='page-btn'; prev.textContent='‹';
  prev.disabled=currentPage===1; prev.onclick=()=>{state.pagination.currentPage--;render()}; c.appendChild(prev);
  
  let start=Math.max(1,currentPage-3), end=Math.min(totalPages,start+6);
  if(end-start<6)start=Math.max(1,end-6);
  if(start>1){const f=document.createElement('button'); f.className='page-btn'; f.textContent='1'; f.onclick=()=>{state.pagination.currentPage=1;render()}; c.appendChild(f);
    if(start>2){const e=document.createElement('span'); e.textContent='...'; e.style.padding='0 8px'; e.style.color='var(--muted)'; c.appendChild(e)}}
  for(let i=start;i<=end;i++){const b=document.createElement('button'); b.className='page-btn'+(i===currentPage?' active':'');
    b.textContent=i; b.onclick=()=>{state.pagination.currentPage=i;render()}; c.appendChild(b)}
  if(end<totalPages){if(end<totalPages-1){const e=document.createElement('span'); e.textContent='...'; e.style.padding='0 8px'; e.style.color='var(--muted)'; c.appendChild(e)}
    const l=document.createElement('button'); l.className='page-btn'; l.textContent=totalPages; l.onclick=()=>{state.pagination.currentPage=totalPages;render()}; c.appendChild(l)}
  
  const next=document.createElement('button'); next.className='page-btn'; next.textContent='›';
  next.disabled=currentPage===totalPages; next.onclick=()=>{state.pagination.currentPage++;render()}; c.appendChild(next);
}

function updateBulkEditBar(){
  $('#bulkEditBar').classList.toggle('active',state.selected.size>0);
  $('#selectedCount').textContent=`${state.selected.size}개 선택`;
}

function openBulkEditModal(){
  if(!state.selected.size)return alert('항목을 선택하세요');
  $('#bulkCountText').textContent=state.selected.size;
  const sel=state.rows.filter(r=>state.selected.has(r.id));
  const getVal=f=>{const v=sel.map(r=>r[f]); return v.every(x=>x===v[0])?v[0]:''};
  $('#bProduct').value=getVal('product')||''; $('#bCategory').value=getVal('category')||'';
  $('#bClient').value=getVal('client')||''; $('#bMid').value=getVal('mid')||'';
  $('#bKeywords').value=getVal('keywords')||''; $('#bStartDate').value=getVal('startDate')||'';
  $('#bEndDate').value=getVal('endDate')||''; $('#bUnits').value=getVal('units')??'';
  $('#bDays').value=getVal('days')??''; $('#bUnitPrice').value=getVal('unitPrice')??'';
  $('#bTax').value=getVal('tax')||''; $('#bNote').value=getVal('note')||'';
  $('#bulkEditModalBack').classList.add('active');
}

function closeBulkEditModal(){$('#bulkEditModalBack').classList.remove('active'); resetBulkForm()}
function resetBulkForm(){$('#bulkEditForm').reset(); $('#bProduct').value=''; $('#bCategory').value=''; $('#bTax').value=''}

function applyBulkEdit(e){
  e.preventDefault();
  const u={};
  if($('#bProduct').value)u.product=$('#bProduct').value;
  if($('#bCategory').value)u.category=$('#bCategory').value;
  if($('#bClient').value.trim())u.client=$('#bClient').value.trim();
  if($('#bMid').value.trim())u.mid=$('#bMid').value.trim();
  if($('#bKeywords').value.trim())u.keywords=$('#bKeywords').value.trim();
  if($('#bStartDate').value)u.startDate=$('#bStartDate').value;
  if($('#bEndDate').value)u.endDate=$('#bEndDate').value;
  if($('#bUnits').value)u.units=parseNum($('#bUnits').value);
  if($('#bDays').value)u.days=parseNum($('#bDays').value);
  if($('#bUnitPrice').value)u.unitPrice=parseNum($('#bUnitPrice').value);
  if($('#bTax').value)u.tax=$('#bTax').value;
  if($('#bNote').value.trim())u.note=$('#bNote').value.trim();
  if(!Object.keys(u).length)return alert('변경할 내용을 입력하세요');
  state.rows.forEach(r=>{if(state.selected.has(r.id)){Object.assign(r,u); r.totalQty=(r.units||0)*(r.days||0); r.cost=r.totalQty*(r.unitPrice||0)}});
  render(); closeBulkEditModal(); alert(`${state.selected.size}개 수정 완료`);
}

function renderKpis(){
  const rows=applyFilters(state.rows);
  const cnt=rows.length, qty=rows.reduce((s,r)=>s+(r.totalQty||0),0), cost=rows.reduce((s,r)=>s+(r.cost||0),0);
  $('#kpiCount').textContent=cnt.toLocaleString('ko-KR');
  $('#kpiQty').textContent=qty.toLocaleString('ko-KR');
  $('#kpiCost').textContent=fmtMoney(cost);
  $('#kpiAvg').textContent=fmtMoney(qty>0?cost/qty:0);
  
  const byProduct={};
  state.rows.forEach(r=>{const p=r.product||'미분류'; byProduct[p]=(byProduct[p]||0)+(r.cost||0)});
  const pData=Object.entries(byProduct).map(([p,c])=>({label:p,value:c})).sort((a,b)=>b.value-a.value);
  drawBars($('#barsByProduct'),pData,true);
  
  const byMonth={};
  state.rows.forEach(r=>{if(!r.startDate)return; const ym=getYearMonth(r.startDate); byMonth[ym]=(byMonth[ym]||0)+(r.cost||0)});
  const mData=Object.entries(byMonth).sort((a,b)=>b[0].localeCompare(a[0]));
  const mc=$('#monthlyCost');
  if(!mData.length){mc.innerHTML='<div class="muted">월별 데이터 없음</div>'; return}
  mc.innerHTML=mData.map(([ym,c])=>{const[y,m]=ym.split('-'); return`<div class="month-item" data-month="${ym}">
    <span>${y}년 ${m}월: <strong>${fmtMoney(c)}</strong></span><span class="month-item-icon">📊</span></div>`}).join('');
  $$('.month-item').forEach(i=>i.onclick=()=>showMonthlyChart(i.dataset.month));
}

function drawBars(c,data,money){
  c.innerHTML=''; const max=Math.max(1,...data.map(d=>d.value));
  data.forEach(d=>{
    const col=document.createElement('div'); col.className='col';
    if(d.label&&d.label!=='미분류')col.classList.add(`product-${d.label}`);
    const bar=document.createElement('div'); bar.className='bar'; bar.style.height=Math.round(d.value/max*100)+'%';
    const val=document.createElement('div'); val.className='bar-value'; val.textContent=money?fmtMoney(d.value):d.value.toLocaleString('ko-KR');
    const lab=document.createElement('div'); lab.className='note'; lab.textContent=d.label||'미분류';
    col.append(bar,val,lab); c.appendChild(col);
  });
}

function showMonthlyChart(ym){
  const[y,m]=ym.split('-'); const pc={};
  state.rows.forEach(r=>{if(!r.startDate||getYearMonth(r.startDate)!==ym)return; const p=r.product||'미분류'; pc[p]=(pc[p]||0)+(r.cost||0)});
  const data=Object.entries(pc).map(([p,c])=>({product:p,cost:c,percentage:0}));
  const total=data.reduce((s,i)=>s+i.cost,0);
  data.forEach(i=>i.percentage=total>0?i.cost/total*100:0);
  data.sort((a,b)=>b.cost-a.cost);
  $('#chartTitle').textContent=`${y}년 ${m}월 상품별 작업비`;
  drawPieChart(data); renderPieLegend(data);
  $('#chartModal').classList.add('active');
}

function drawPieChart(data){
  const cv=$('#pieChart'), ctx=cv.getContext('2d'), cx=cv.width/2, cy=cv.height/2, r=120;
  ctx.clearRect(0,0,cv.width,cv.height);
  if(!data.length){ctx.fillStyle='#8b94a8'; ctx.font='16px sans-serif'; ctx.textAlign='center'; ctx.fillText('데이터 없음',cx,cy); return}
  const colors={멘토스:'#8B5CF6',호올스:'#A78BFA',말차:'#10B981',신규:'#6366f1',미분류:'#64748b'};
  const theme=document.documentElement.getAttribute('data-theme');
  const bc=theme==='light'?'#e2e8f0':'#2a3551';
  let ca=-Math.PI/2;
  data.forEach(i=>{
    const sa=i.percentage/100*2*Math.PI, ea=ca+sa;
    ctx.beginPath(); ctx.moveTo(cx,cy); ctx.arc(cx,cy,r,ca,ea); ctx.closePath();
    ctx.fillStyle=colors[i.product]||'#64748b'; ctx.fill();
    ctx.strokeStyle=bc; ctx.lineWidth=2; ctx.stroke();
    if(i.percentage>=5){const ta=ca+sa/2, tx=cx+Math.cos(ta)*(r*0.7), ty=cy+Math.sin(ta)*(r*0.7);
      ctx.fillStyle='#fff'; ctx.font='bold 14px sans-serif'; ctx.textAlign='center'; ctx.textBaseline='middle';
      ctx.fillText(`${i.percentage.toFixed(1)}%`,tx,ty)}
    ca=ea;
  });
}

function renderPieLegend(data){
  const colors={멘토스:'#8B5CF6',호올스:'#A78BFA',말차:'#10B981',신규:'#6366f1',미분류:'#64748b'};
  $('#pieLegend').innerHTML=data.map(i=>`<div class="pie-legend-item">
    <div class="pie-legend-color" style="background:${colors[i.product]||'#64748b'}"></div>
    <div class="pie-legend-info">
      <div class="pie-legend-label">${i.product}</div>
      <div class="pie-legend-value">${fmtMoney(i.cost)} (${i.percentage.toFixed(1)}%)</div>
    </div></div>`).join('');
}

function openAddModal(){$('#addModalBack').classList.add('active'); $('#aStartDate').value=today()}
function closeAddModal(){$('#addModalBack').classList.remove('active')}
function resetAddForm(){$('#addForm').reset(); $('#aProduct').value=''; $('#aCategory').value=''; $('#aTax').value='미발행'; $('#aStartDate').value=today()}

function exportExcel(){
  if(!state.selected.size)return alert('항목을 선택하세요');
  const sel=state.rows.filter(r=>state.selected.has(r.id));
  const data=sel.map(r=>({ID:r.id,상품:r.product||'',구분:r.category||'',업체명:r.client||'',MID:r.mid||'',
    키워드:r.keywords||'',시작일:r.startDate||'',종료일:r.endDate||'',일타:r.units??'',일수:r.days??'',
    단가:r.unitPrice??'',총량:r.totalQty??'',작업비:r.cost??'',세금계산서:r.tax||'',비고:r.note||'',생성일시:r.createdAt||''}));
  const ws=XLSX.utils.json_to_sheet(data), wb=XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb,ws,'작업목록');
  XLSX.writeFile(wb,`엔티제컴퍼니_작업목록_${today()}.xlsx`);
  alert(`${sel.length}개 항목 내보내기 완료`);
}

function applyTheme(m){
  const html=document.documentElement;
  if(m==='auto'){const pd=matchMedia&&matchMedia('(prefers-color-scheme: dark)').matches; html.setAttribute('data-theme',pd?'dark':'light')}
  else html.setAttribute('data-theme',m==='dark'?'dark':'light');
}

function bind(){
  const qIn=$('#q'), qDd=$('#qAutocomplete');
  qIn.onfocus=()=>{renderAutocomplete(); qDd.classList.add('active')};
  qIn.oninput=e=>{state.filters.q=e.target.value; state.pagination.currentPage=1; render()};
  qIn.onkeydown=e=>{if(e.key==='Enter'&&qIn.value.trim()){saveSearchHistory(qIn.value.trim()); qDd.classList.remove('active')}};
  document.onclick=e=>{if(!$('#qWrapper').contains(e.target))qDd.classList.remove('active')};
  
  $('#fProduct').onchange=e=>{state.filters.product=e.target.value; state.pagination.currentPage=1; render()};
  $('#fCategory').onchange=e=>{state.filters.category=e.target.value; state.pagination.currentPage=1; render()};
  $('#fTax').onchange=e=>{state.filters.tax=e.target.value; state.pagination.currentPage=1; render()};
  $('#fDate').onchange=e=>{state.filters.date=e.target.value; state.pagination.currentPage=1; render()};
  $('#sortSel').onchange=e=>{state.filters.sort=e.target.value; state.pagination.currentPage=1; render()};
  $('#perPageSelect').onchange=e=>{state.pagination.perPage=parseInt(e.target.value); state.pagination.currentPage=1; render()};
  
  $$('.period-btn').forEach(b=>b.onclick=()=>{$$('.period-btn').forEach(x=>x.classList.remove('active'));
    b.classList.add('active'); state.filters.periodType=b.dataset.period; state.pagination.currentPage=1; render()});
  
  $('#resetFiltersBtn').onclick=()=>{$('#q').value=''; $('#fDate').value=''; $('#fProduct').value='all';
    $('#fCategory').value='all'; $('#fTax').value='all'; $('#sortSel').value='date_desc';
    state.filters={q:'',product:'all',category:'all',tax:'all',date:'',periodType:'day',sort:'date_desc'};
    state.pagination.currentPage=1; $$('.period-btn').forEach(b=>b.classList.remove('active'));
    $('.period-btn[data-period="day"]').classList.add('active'); qDd.classList.remove('active'); render()};
  
  $('#chkAll').onchange=e=>{const all=applyFilters(state.rows);
    if(e.target.checked)all.forEach(r=>state.selected.add(r.id)); else all.forEach(r=>state.selected.delete(r.id));
    renderTable(); updateBulkEditBar()};
  
  $('#bulkEditBtn').onclick=openBulkEditModal;
  $('#bulkEditForm').onsubmit=applyBulkEdit;
  $('#openAddModalBtn').onclick=openAddModal;
  $('#addForm').onsubmit=e=>{e.preventDefault(); const n=mk({product:$('#aProduct').value,category:$('#aCategory').value,
    client:$('#aClient').value.trim(),mid:$('#aMid').value.trim(),keywords:$('#aKeywords').value.trim(),
    startDate:$('#aStartDate').value||today(),endDate:$('#aEndDate').value||'',units:parseNum($('#aUnits').value),
    days:parseNum($('#aDays').value),unitPrice:parseNum($('#aUnitPrice').value),tax:$('#aTax').value||'미발행',
    note:$('#aNote').value.trim()}); state.rows.unshift(n); render(); closeAddModal()};
  $('#removeSelectedBtn').onclick=()=>{if(!state.selected.size)return alert('항목을 선택하세요');
    if(confirm(`${state.selected.size}건 삭제?`)){state.rows=state.rows.filter(r=>!state.selected.has(r.id));
      state.selected.clear(); render()}};
  $('#themeBtn').onclick=()=>{const cur=document.documentElement.getAttribute('data-theme');
    const next=cur==='dark'?'light':'dark'; applyTheme(next); state.prefs.theme=next; save()};
  $('#exportExcelBtn').onclick=exportExcel;
  $('#closeChartModal').onclick=()=>$('#chartModal').classList.remove('active');
  $('#chartModal').onclick=e=>{if(e.target===$('#chartModal'))$('#chartModal').classList.remove('active')};
  document.onkeydown=e=>{if(e.key==='Escape'){closeAddModal(); closeBulkEditModal(); $('#chartModal').classList.remove('active')}};
}

load(); bind(); render();
</script>
</body>
</html>
