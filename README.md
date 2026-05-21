<!DOCTYPE html>
<html lang="id">    
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Form Data Mahasiswa</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background: #070004;
        }
        form {
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        /* PERBAIKAN BUG: Menggunakan :not([type="radio"]) agar tombol radio Jenis Kelamin tidak rusak/melebar */
        input:not([type="radio"]), textarea, select {
            width: 100%;
            padding: 8px;
            margin: 5px 0 15px;
            box-sizing: border-box; /* Mencegah elemen keluar dari kotak form */
        }
        button {
            padding: 10px;
            margin-right: 5px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }
        .submit { background: green; color: white; }
        .reset { background: rgb(252, 12, 28); color: white; }
        
        /* STYLE BARU: Desain Tombol Export */
        .btn-excel { background: #217346; color: white; }
        .btn-pdf { background: #E04443; color: white; }
        
        table {
            width: 100%;
            border-collapse: collapse;
            background: #fff;
            margin-top: 10px;
        }
        th, td {
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }
    </style>
</head>
<body>
    <h2>Form Data Mahasiswa</h2>
    <form id="formMahasiswa">
        <label for="nim">NIM</label>
        <input type="text" id="nim" required>
        
        <label for="nama">Nama</label>
        <input type="text" id="nama" required>
        
        <label for="alamat">Alamat</label>
        <textarea id="alamat"></textarea>
        
        <label>Jenis Kelamin</label><br>
        <input type="radio" name="jk" value="Pria" id="pria" required> <label for="pria">Pria</label>
        <input type="radio" name="jk" value="Wanita" id="wanita"> <label for="wanita">Wanita</label>
        <br><br>
        
        <label>Tanggal Lahir</label>
        <div style="display: flex; gap: 10px;">
            <select id="tanggal" required></select>
            <select id="bulan" required>
                <option value="01">Januari</option>
                <option value="02">Februari</option>
                <option value="03">Maret</option>
                <option value="04">April</option>
                <option value="05">Mei</option>
                <option value="06">Juni</option>
                <option value="07">Juli</option>
                <option value="08">Agustus</option>
                <option value="09">September</option>
                <option value="10">Oktober</option>
                <option value="11">November</option>
                <option value="12">Desember</option>
            </select>
            <select id="tahun" required></select>
        </div>
        
        <label for="password">Password</label>
        <input type="password" id="password" required>
        
        <button type="submit" class="submit">Submit</button>
        <button type="reset" class="reset">Reset</button>
    </form>
    
    <h2>Data Mahasiswa</h2>
    <div style="margin-bottom: 15px;">
        <button type="button" class="btn-excel" onclick="exportKeExcel()">Export ke Excel</button>
        <button type="button" class="btn-pdf" onclick="exportKePDF()">Export ke PDF</button>
    </div>

    <table id="tabelMahasiswa">
        <thead>
            <tr>
                <th>NIM</th>
                <th>Nama</th>
                <th>Alamat</th>
                <th>JK</th>
                <th>TTL</th>
                <th>Password</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>

    <script>
        // 1. Mengisi Opsi Tanggal (1-31) secara otomatis
        const tanggalSelect = document.getElementById('tanggal');
        for (let i = 1; i <= 31; i++) {
            let nilai = i < 10 ? '0' + i : i;
            tanggalSelect.innerHTML += `<option value="${nilai}">${i}</option>`;
        }

        // 2. Mengisi Opsi Tahun (Dari tahun sekarang mundur ke 1970) secara otomatis
        const tahunSelect = document.getElementById('tahun');
        const tahunSekarang = new Date().getFullYear();
        for (let i = tahunSekarang; i >= 1970; i--) {
            tahunSelect.innerHTML += `<option value="${i}">${i}</option>`;
        }

        // 3. Logika Menyimpan Input Form ke dalam Tabel HTML
        document.getElementById('formMahasiswa').addEventListener('submit', function(e) {
            e.preventDefault(); // Mencegah muat ulang halaman (refresh)

            const nim = document.getElementById('nim').value;
            const nama = document.getElementById('nama').value;
            const alamat = document.getElementById('alamat').value;
            
            const jkTerpilih = document.querySelector('input[name="jk"]:checked');
            const jk = jkTerpilih ? jkTerpilih.value : '-';
            
            const tanggal = document.getElementById('tanggal').value;
            const bulan = document.getElementById('bulan').value;
            const tahun = document.getElementById('tahun').value;
            const ttl = `${tanggal}/${bulan}/${tahun}`;

            const tableBody = document.getElementById('tableBody');
            const barisBaru = tableBody.insertRow();

            barisBaru.innerHTML = `
                <td>${nim}</td>
                <td>${nama}</td>
                <td>${alamat}</td>
                <td>${jk}</td>
                <td>${ttl}</td>
                <td>******</td> <td><button style="background: red; color: white; padding: 5px 10px;" onclick="this.parentElement.parentElement.remove()">Hapus</button></td>
            `;

            this.reset(); // Mengosongkan form kembali setelah submit berhasil
        });

        // 4. FITUR: Fungsi Export Data ke file Excel (.xlsx)
        function exportKeExcel() {
            const tabelOriginal = document.getElementById("tabelMahasiswa");
            const salinanTabel = tabelOriginal.cloneNode(true);
            
            // Logika Bisnis: Menghapus kolom 'Aksi' (tombol hapus) pada file unduhan agar rapi
            const baris = salinanTabel.querySelectorAll("tr");
            baris.forEach(b => {
                if(b.lastElementChild) b.removeChild(b.lastElementChild);
            });

            const workbook = XLSX.utils.table_to_book(salinanTabel, { sheet: "Data Mahasiswa" });
            XLSX.writeFile(workbook, "Data_Mahasiswa.xlsx");
        }

        // 5. FITUR: Fungsi Export Data ke berkas PDF (.pdf)
        function exportKePDF() {
            const tabelOriginal = document.getElementById("tabelMahasiswa");
            const salinanTabel = tabelOriginal.cloneNode(true);
            
            // Menghapus kolom 'Aksi' pada file laporan PDF
            const baris = salinanTabel.querySelectorAll("tr");
            baris.forEach(b => {
                if(b.lastElementChild) b.removeChild(b.lastElementChild);
            });

            // Membuat container pembungkus dokumen PDF agar ada Judul Laporannya
            const areaCetak = document.createElement("div");
            areaCetak.innerHTML = "<h2 style='text-align: center; font-family: Arial; margin-bottom: 20px;'>LAPORAN DATA MAHASISWA</h2>";
            areaCetak.appendChild(salinanTabel);

            const opsiKonfigurasi = {
                margin:       15,
                filename:     'Data_Mahasiswa.pdf',
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2 },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            html2pdf().set(opsiKonfigurasi).from(areaCetak).save();
        }
    </script>
</body>
</html>
