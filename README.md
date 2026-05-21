<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form Data Mahasiswa</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
  <!-- Export Libraries -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --pink:       #f472b6;
      --pink-light: #fce7f3;
      --pink-dark:  #be185d;
      --rose:       #fb7185;
      --white:      #ffffff;
      --gray-50:    #fdf2f8;
      --gray-100:   #f3e8ee;
      --gray-300:   #d1b8c7;
      --gray-500:   #9c6d84;
      --gray-700:   #5e3354;
      --green:      #10b981;
      --red:        #ef4444;
      --amber:      #f59e0b;
      --shadow-sm:  0 1px 3px rgba(190,24,93,.12);
      --shadow-md:  0 4px 16px rgba(190,24,93,.16);
      --shadow-lg:  0 8px 32px rgba(190,24,93,.20);
      --radius:     14px;
    }

    body {
      font-family: 'Plus Jakarta Sans', sans-serif;
      min-height: 100vh;
      background: linear-gradient(135deg, #fce7f3 0%, #ffe4e6 40%, #fdf2f8 100%);
      padding: 32px 20px;
      color: var(--gray-700);
    }

    /* ── HEADER ── */
    .page-header {
      text-align: center;
      margin-bottom: 32px;
    }
    .page-header h1 {
      font-size: 2rem;
      font-weight: 700;
      background: linear-gradient(135deg, var(--pink-dark), var(--rose));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .page-header p {
      color: var(--gray-500);
      font-size: .9rem;
      margin-top: 4px;
    }

    /* ── CARD ── */
    .card {
      background: var(--white);
      border-radius: var(--radius);
      box-shadow: var(--shadow-md);
      padding: 28px;
      margin-bottom: 28px;
      border: 1px solid var(--gray-100);
    }
    .card-title {
      font-size: 1.1rem;
      font-weight: 700;
      color: var(--pink-dark);
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .card-title::before {
      content: '';
      display: block;
      width: 4px; height: 20px;
      background: linear-gradient(to bottom, var(--pink), var(--rose));
      border-radius: 4px;
    }

    /* ── FORM GRID ── */
    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px 24px;
    }
    .form-group { display: flex; flex-direction: column; gap: 6px; }
    .form-group.full { grid-column: 1 / -1; }
    label {
      font-size: .8rem;
      font-weight: 600;
      color: var(--gray-500);
      text-transform: uppercase;
      letter-spacing: .05em;
    }
    input[type="text"],
    input[type="password"],
    textarea,
    select {
      width: 100%;
      padding: 10px 14px;
      border: 1.5px solid var(--gray-100);
      border-radius: 8px;
      font-family: inherit;
      font-size: .92rem;
      color: var(--gray-700);
      background: var(--gray-50);
      transition: border-color .2s, box-shadow .2s;
      outline: none;
    }
    input:focus, textarea:focus, select:focus {
      border-color: var(--pink);
      box-shadow: 0 0 0 3px rgba(244,114,182,.15);
      background: var(--white);
    }
    textarea { resize: vertical; min-height: 80px; }

    /* ── RADIO GROUP ── */
    .radio-group { display: flex; gap: 20px; padding-top: 4px; }
    .radio-option {
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
      font-size: .92rem;
      font-weight: 500;
    }
    .radio-option input[type="radio"] {
      width: 18px; height: 18px;
      accent-color: var(--pink-dark);
      cursor: pointer;
    }

    /* ── DATE ROW ── */
    .date-row { display: flex; gap: 10px; }
    .date-row select { flex: 1; }

    /* ── BUTTONS ── */
    .btn-row { display: flex; gap: 10px; margin-top: 8px; justify-content: flex-end; }
    .btn {
      padding: 10px 24px;
      border: none;
      border-radius: 8px;
      font-family: inherit;
      font-size: .92rem;
      font-weight: 600;
      cursor: pointer;
      transition: transform .15s, box-shadow .15s, opacity .15s;
      display: flex; align-items: center; gap: 6px;
    }
    .btn:hover { transform: translateY(-1px); box-shadow: var(--shadow-md); }
    .btn:active { transform: translateY(0); }
    .btn-submit { background: linear-gradient(135deg, var(--pink-dark), var(--rose)); color: #fff; }
    .btn-reset  { background: var(--gray-100); color: var(--gray-700); }

    /* ── STATS ── */
    .stats { display: flex; gap: 12px; margin-bottom: 20px; flex-wrap: wrap; }
    .stat-box {
      background: var(--pink-light);
      border-radius: 10px;
      padding: 10px 18px;
      font-size: .85rem;
      font-weight: 600;
      color: var(--pink-dark);
      border: 1px solid rgba(244,114,182,.25);
    }
    .stat-box span { font-size: 1.3rem; display: block; }

    /* ── TABLE ── */
    .table-wrap { overflow-x: auto; }
    table { width: 100%; border-collapse: collapse; font-size: .875rem; }
    thead tr { background: linear-gradient(135deg, var(--pink-dark), var(--rose)); }
    th {
      padding: 12px 14px;
      text-align: left;
      color: #fff;
      font-weight: 600;
      font-size: .8rem;
      text-transform: uppercase;
      letter-spacing: .05em;
      white-space: nowrap;
    }
    tbody tr {
      border-bottom: 1px solid var(--gray-100);
      transition: background .15s;
    }
    tbody tr:hover { background: var(--gray-50); }
    tbody tr.editing { background: #fff9c4; }
    td { padding: 12px 14px; color: var(--gray-700); }
    .badge {
      display: inline-block;
      padding: 3px 10px;
      border-radius: 20px;
      font-size: .78rem;
      font-weight: 600;
    }
    .badge-pria   { background: #dbeafe; color: #1d4ed8; }
    .badge-wanita { background: #fce7f3; color: var(--pink-dark); }
    .dots { letter-spacing: .15em; font-size: .9rem; }
    .empty-state {
      text-align: center;
      padding: 48px 20px;
      color: var(--gray-300);
    }
    .empty-state p { font-size: 2rem; margin-bottom: 8px; }

    /* ── ACTION BUTTONS ── */
    .action-btn {
      padding: 5px 12px;
      border: none;
      border-radius: 6px;
      font-family: inherit;
      font-size: .78rem;
      font-weight: 600;
      cursor: pointer;
      transition: opacity .15s, transform .15s;
      margin-right: 4px;
    }
    .action-btn:hover { opacity: .85; transform: scale(1.05); }
    .btn-edit   { background: var(--amber); color: #fff; }
    .btn-delete { background: var(--red); color: #fff; }

    /* ── TOAST ── */
    #toast {
      position: fixed;
      bottom: 28px; right: 28px;
      padding: 12px 22px;
      border-radius: 10px;
      color: #fff;
      font-weight: 600;
      font-size: .88rem;
      box-shadow: var(--shadow-lg);
      opacity: 0;
      transform: translateY(12px);
      transition: opacity .3s, transform .3s;
      z-index: 999;
      pointer-events: none;
    }
    #toast.show { opacity: 1; transform: translateY(0); }
    #toast.success { background: var(--green); }
    #toast.error   { background: var(--red); }
    #toast.info    { background: var(--pink-dark); }

    .btn-excel  { background: #16a34a; color: #fff; }
    .btn-pdf    { background: #dc2626; color: #fff; }
    .export-row { display: flex; gap: 10px; margin-bottom: 16px; flex-wrap: wrap; }

    @media (max-width: 600px) {
      .form-grid { grid-template-columns: 1fr; }
      .form-group.full { grid-column: 1; }
    }
  </style>
</head>
<body>

  <div class="page-header">
    <h1>📋 Data Mahasiswa</h1>
    <p>Sistem Manajemen Data Mahasiswa</p>
  </div>

  <!-- FORM -->
  <div class="card">
    <div class="card-title" id="formTitle">Tambah Mahasiswa</div>
    <form id="formMahasiswa" novalidate>
      <input type="hidden" id="editIndex" value="-1">
      <div class="form-grid">

        <div class="form-group">
          <label for="nim">NIM <span style="color:var(--rose)">*</span></label>
          <input type="text" id="nim" placeholder="Contoh: 12345678" required>
        </div>

        <div class="form-group">
          <label for="nama">Nama Lengkap <span style="color:var(--rose)">*</span></label>
          <input type="text" id="nama" placeholder="Nama mahasiswa" required>
        </div>

        <div class="form-group full">
          <label for="alamat">Alamat</label>
          <textarea id="alamat" placeholder="Alamat lengkap..."></textarea>
        </div>

        <div class="form-group">
          <label>Jenis Kelamin <span style="color:var(--rose)">*</span></label>
          <div class="radio-group">
            <label class="radio-option">
              <input type="radio" name="jk" value="Pria"> Pria
            </label>
            <label class="radio-option">
              <input type="radio" name="jk" value="Wanita"> Wanita
            </label>
          </div>
        </div>

        <div class="form-group">
          <label>Tanggal Lahir</label>
          <div class="date-row">
            <select id="tanggal"></select>
            <select id="bulan">
              <option value="01">Jan</option>
              <option value="02">Feb</option>
              <option value="03">Mar</option>
              <option value="04">Apr</option>
              <option value="05">Mei</option>
              <option value="06">Jun</option>
              <option value="07">Jul</option>
              <option value="08">Agu</option>
              <option value="09">Sep</option>
              <option value="10">Okt</option>
              <option value="11">Nov</option>
              <option value="12">Des</option>
            </select>
            <select id="tahun"></select>
          </div>
        </div>

        <div class="form-group">
          <label for="password">Password</label>
          <input type="password" id="password" placeholder="••••••••">
        </div>

      </div><!-- /form-grid -->

      <div class="btn-row" style="margin-top:20px">
        <button type="button" class="btn btn-reset" onclick="resetForm()">✕ Reset</button>
        <button type="submit" class="btn btn-submit" id="submitBtn">＋ Simpan</button>
      </div>
    </form>
  </div>

  <!-- TABLE -->
  <div class="card">
    <div class="card-title">Daftar Mahasiswa</div>

    <div class="stats">
      <div class="stat-box"><span id="statTotal">0</span>Total</div>
      <div class="stat-box"><span id="statPria">0</span>Pria</div>
      <div class="stat-box"><span id="statWanita">0</span>Wanita</div>
    </div>

    <div class="export-row">
      <button class="btn btn-excel" onclick="exportExcel()">📊 Export Excel</button>
      <button class="btn btn-pdf"   onclick="exportPDF()">📄 Export PDF</button>
    </div>

    <div class="table-wrap">
      <table>
        <thead>
          <tr>
            <th>#</th>
            <th>NIM</th>
            <th>Nama</th>
            <th>Alamat</th>
            <th>J/K</th>
            <th>Tgl Lahir</th>
            <th>Password</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody id="tableBody">
          <tr id="emptyRow">
            <td colspan="8">
              <div class="empty-state">
                <p>🎓</p>
                Belum ada data mahasiswa. Silakan tambahkan melalui form di atas.
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div id="toast"></div>

<script>
  /* ─── DATA ─── */
  let mahasiswaList = [];

  /* ─── INIT DROPDOWN ─── */
  function initDropdowns() {
    const selTgl = document.getElementById('tanggal');
    for (let i = 1; i <= 31; i++) {
      const o = document.createElement('option');
      o.value = String(i).padStart(2, '0');
      o.textContent = i;
      selTgl.appendChild(o);
    }

    const selThn = document.getElementById('tahun');
    const now = new Date().getFullYear();
    for (let y = now; y >= 1970; y--) {
      const o = document.createElement('option');
      o.value = y;
      o.textContent = y;
      selThn.appendChild(o);
    }
  }

  /* ─── TOAST ─── */
  function showToast(msg, type = 'success') {
    const t = document.getElementById('toast');
    t.textContent = msg;
    t.className = `show ${type}`;
    clearTimeout(t._timer);
    t._timer = setTimeout(() => { t.className = ''; }, 2800);
  }

  /* ─── RENDER TABLE ─── */
  const BULAN = ['','Januari','Februari','Maret','April','Mei','Juni',
                 'Juli','Agustus','September','Oktober','November','Desember'];

  function renderTable() {
    const tbody = document.getElementById('tableBody');
    tbody.innerHTML = '';

    if (mahasiswaList.length === 0) {
      tbody.innerHTML = `<tr id="emptyRow"><td colspan="8">
        <div class="empty-state"><p>🎓</p>Belum ada data mahasiswa.</div>
      </td></tr>`;
      updateStats();
      return;
    }

    mahasiswaList.forEach((m, i) => {
      const bulanNama = BULAN[parseInt(m.bulan)];
      const tgl = `${parseInt(m.tanggal)} ${bulanNama} ${m.tahun}`;
      const badgeClass = m.jk === 'Pria' ? 'badge-pria' : 'badge-wanita';
      const passDots = '●'.repeat(Math.min(m.password.length, 8)) || '-';

      const tr = document.createElement('tr');
      tr.dataset.index = i;
      tr.innerHTML = `
        <td>${i + 1}</td>
        <td><strong>${m.nim}</strong></td>
        <td>${m.nama}</td>
        <td>${m.alamat || '-'}</td>
        <td><span class="badge ${badgeClass}">${m.jk}</span></td>
        <td>${tgl}</td>
        <td class="dots" title="Klik mata untuk lihat">${passDots}</td>
        <td>
          <button class="action-btn btn-edit"   onclick="editData(${i})">✏️ Edit</button>
          <button class="action-btn btn-delete" onclick="deleteData(${i})">🗑️ Hapus</button>
        </td>`;
      tbody.appendChild(tr);
    });

    updateStats();
  }

  /* ─── STATS ─── */
  function updateStats() {
    document.getElementById('statTotal').textContent  = mahasiswaList.length;
    document.getElementById('statPria').textContent   = mahasiswaList.filter(m => m.jk === 'Pria').length;
    document.getElementById('statWanita').textContent = mahasiswaList.filter(m => m.jk === 'Wanita').length;
  }

  /* ─── SUBMIT ─── */
  document.getElementById('formMahasiswa').addEventListener('submit', function(e) {
    e.preventDefault();

    const nim    = document.getElementById('nim').value.trim();
    const nama   = document.getElementById('nama').value.trim();
    const alamat = document.getElementById('alamat').value.trim();
    const jk     = document.querySelector('input[name="jk"]:checked')?.value || '';
    const tgl    = document.getElementById('tanggal').value;
    const bln    = document.getElementById('bulan').value;
    const thn    = document.getElementById('tahun').value;
    const pass   = document.getElementById('password').value;
    const editIdx = parseInt(document.getElementById('editIndex').value);

    /* Validasi */
    if (!nim)  { showToast('NIM wajib diisi!', 'error'); return; }
    if (!nama) { showToast('Nama wajib diisi!', 'error'); return; }
    if (!jk)   { showToast('Pilih jenis kelamin!', 'error'); return; }

    /* Cek NIM duplikat (kecuali saat edit baris yang sama) */
    const dupIdx = mahasiswaList.findIndex(m => m.nim === nim);
    if (dupIdx !== -1 && dupIdx !== editIdx) {
      showToast('NIM sudah terdaftar!', 'error'); return;
    }

    const data = { nim, nama, alamat, jk, tanggal: tgl, bulan: bln, tahun: thn, password: pass };

    if (editIdx >= 0) {
      mahasiswaList[editIdx] = data;
      showToast('Data berhasil diperbarui ✅', 'info');
    } else {
      mahasiswaList.push(data);
      showToast('Mahasiswa berhasil ditambahkan 🎉');
    }

    renderTable();
    resetForm();
  });

  /* ─── EDIT ─── */
  function editData(i) {
    const m = mahasiswaList[i];
    document.getElementById('nim').value    = m.nim;
    document.getElementById('nama').value   = m.nama;
    document.getElementById('alamat').value = m.alamat;
    document.getElementById('tanggal').value = m.tanggal;
    document.getElementById('bulan').value   = m.bulan;
    document.getElementById('tahun').value   = m.tahun;
    document.getElementById('password').value = m.password;
    document.getElementById('editIndex').value = i;

    const radio = document.querySelector(`input[name="jk"][value="${m.jk}"]`);
    if (radio) radio.checked = true;

    document.getElementById('formTitle').textContent  = '✏️ Edit Mahasiswa';
    document.getElementById('submitBtn').textContent  = '💾 Update';

    /* Highlight baris */
    document.querySelectorAll('tbody tr').forEach(r => r.classList.remove('editing'));
    const row = document.querySelector(`tr[data-index="${i}"]`);
    if (row) row.classList.add('editing');

    window.scrollTo({ top: 0, behavior: 'smooth' });
    showToast('Mode edit aktif', 'info');
  }

  /* ─── DELETE ─── */
  function deleteData(i) {
    const nama = mahasiswaList[i].nama;
    if (!confirm(`Hapus data "${nama}"?`)) return;
    mahasiswaList.splice(i, 1);
    renderTable();
    showToast(`Data "${nama}" dihapus`, 'error');
  }

  /* ─── RESET ─── */
  function resetForm() {
    document.getElementById('formMahasiswa').reset();
    document.getElementById('editIndex').value = -1;
    document.getElementById('formTitle').textContent = 'Tambah Mahasiswa';
    document.getElementById('submitBtn').textContent = '＋ Simpan';
    document.querySelectorAll('tbody tr').forEach(r => r.classList.remove('editing'));
  }

  /* ─── EXPORT HELPERS ─── */
  function getExportRows() {
    return mahasiswaList.map((m, i) => {
      const bulanNama = BULAN[parseInt(m.bulan)];
      return [
        i + 1,
        m.nim,
        m.nama,
        m.alamat || '-',
        m.jk,
        `${parseInt(m.tanggal)} ${bulanNama} ${m.tahun}`,
        m.password || '-'
      ];
    });
  }

  /* ─── EXPORT EXCEL ─── */
  function exportExcel() {
    if (mahasiswaList.length === 0) {
      showToast('Tidak ada data untuk diekspor!', 'error'); return;
    }

    const header = [['No','NIM','Nama','Alamat','Jenis Kelamin','Tanggal Lahir','Password']];
    const rows   = getExportRows();
    const wsData = [...header, ...rows];

    const wb = XLSX.utils.book_new();
    const ws = XLSX.utils.aoa_to_sheet(wsData);

    /* Lebar kolom */
    ws['!cols'] = [
      { wch: 4 }, { wch: 14 }, { wch: 22 }, { wch: 28 },
      { wch: 14 }, { wch: 20 }, { wch: 16 }
    ];

    /* Style header (warna) — pakai cell meta */
    const headerStyle = {
      font: { bold: true, color: { rgb: 'FFFFFF' } },
      fill: { fgColor: { rgb: 'BE185D' } },
      alignment: { horizontal: 'center' }
    };
    ['A1','B1','C1','D1','E1','F1','G1'].forEach(cell => {
      if (ws[cell]) ws[cell].s = headerStyle;
    });

    XLSX.utils.book_append_sheet(wb, ws, 'Data Mahasiswa');
    XLSX.writeFile(wb, 'data_mahasiswa.xlsx');
    showToast('File Excel berhasil diunduh 📊');
  }

  /* ─── EXPORT PDF ─── */
  function exportPDF() {
    if (mahasiswaList.length === 0) {
      showToast('Tidak ada data untuk diekspor!', 'error'); return;
    }

    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation: 'landscape', unit: 'mm', format: 'a4' });

    /* Header dokumen */
    doc.setFillColor(190, 24, 93);
    doc.rect(0, 0, 297, 22, 'F');
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(14);
    doc.setFont('helvetica', 'bold');
    doc.text('DATA MAHASISWA', 148, 10, { align: 'center' });
    doc.setFontSize(8);
    doc.setFont('helvetica', 'normal');
    const now = new Date().toLocaleDateString('id-ID', { day:'2-digit', month:'long', year:'numeric' });
    doc.text(`Dicetak: ${now}  |  Total: ${mahasiswaList.length} mahasiswa`, 148, 17, { align: 'center' });

    /* Tabel */
    doc.autoTable({
      startY: 28,
      head: [['No','NIM','Nama','Alamat','J/K','Tgl Lahir','Password']],
      body: getExportRows(),
      styles: {
        font: 'helvetica',
        fontSize: 8,
        cellPadding: 3,
        valign: 'middle',
        textColor: [94, 51, 84]
      },
      headStyles: {
        fillColor: [190, 24, 93],
        textColor: [255, 255, 255],
        fontStyle: 'bold',
        halign: 'center'
      },
      alternateRowStyles: { fillColor: [253, 242, 248] },
      columnStyles: {
        0: { halign: 'center', cellWidth: 10 },
        1: { cellWidth: 28 },
        2: { cellWidth: 38 },
        3: { cellWidth: 55 },
        4: { halign: 'center', cellWidth: 22 },
        5: { cellWidth: 38 },
        6: { cellWidth: 28 }
      },
      margin: { left: 10, right: 10 },
      didDrawPage(data) {
        /* Footer halaman */
        doc.setFontSize(7);
        doc.setTextColor(156, 109, 132);
        doc.text(
          `Halaman ${data.pageNumber}`,
          148, doc.internal.pageSize.height - 6,
          { align: 'center' }
        );
      }
    });

    doc.save('data_mahasiswa.pdf');
    showToast('File PDF berhasil diunduh 📄');
  }

  /* ─── BOOT ─── */
  initDropdowns();
  renderTable();
</script>
</body>
</html>
