<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Credit Analyst Dashboard - Mega Finance</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <style>
        /* --- VARIABEL WARNA & RESET --- */
        :root {
            --primary-color: #0f172a; /* Corporate Navy */
            --accent-color: #f59e0b; /* Mega Finance Orange/Gold Hint */
            --success-color: #10b981; /* Emerald Green */
            --danger-color: #ef4444; /* Red for Alert */
            --bg-color: #f1f5f9;
            --card-bg: #ffffff;
            --text-main: #334155;
            --text-sub: #64748b;
            --border-color: #e2e8f0;
            --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
            --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }

        * { box-sizing: border-box; }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
        }

        /* --- HEADER --- */
        header {
            background: var(--primary-color);
            color: white;
            padding: 40px 20px 60px 20px;
            text-align: center;
            background-image: radial-gradient(circle at top right, #1e293b, transparent);
        }

        header h1 { margin: 0; font-weight: 700; font-size: 1.8rem; letter-spacing: -0.5px; }
        header p { color: #94a3b8; margin-top: 8px; font-size: 0.95rem; }

        /* --- NAVIGASI --- */
        nav {
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap; /* Agar rapi di HP */
            margin-top: -30px;
            margin-bottom: 30px;
            padding: 0 20px;
            position: sticky;
            top: 20px;
            z-index: 100;
        }

        .nav-pill {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            padding: 12px 20px;
            border-radius: 50px;
            box-shadow: var(--shadow-md);
            text-decoration: none;
            color: var(--text-main);
            font-weight: 600;
            font-size: 0.85rem;
            transition: all 0.3s ease;
            border: 1px solid white;
        }

        .nav-pill:hover, .nav-pill.active {
            background: var(--accent-color);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(245, 158, 11, 0.3);
        }

        /* --- CONTAINER --- */
        .container { max-width: 1000px; margin: 0 auto; padding: 0 20px; }

        /* --- CARDS --- */
        .card {
            background: var(--card-bg);
            border-radius: 16px;
            box-shadow: var(--shadow-sm);
            padding: 40px;
            margin-bottom: 30px;
            border: 1px solid var(--border-color);
            scroll-margin-top: 100px;
        }

        .card h2 {
            margin-top: 0;
            color: var(--primary-color);
            font-size: 1.5rem;
            display: flex;
            align-items: center;
            gap: 10px;
            border-bottom: 2px solid var(--bg-color);
            padding-bottom: 15px;
            margin-bottom: 25px;
        }

        .section-desc { color: var(--text-sub); margin-bottom: 30px; }

        /* --- FORM GRID --- */
        .calc-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 24px;
        }

        .input-group { display: flex; flex-direction: column; }

        label {
            font-size: 0.85rem;
            font-weight: 600;
            color: var(--text-main);
            margin-bottom: 8px;
        }

        input, select {
            padding: 14px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            font-size: 1rem;
            font-family: 'Inter', sans-serif;
            transition: all 0.2s;
            background-color: #f8fafc;
            width: 100%;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary-color);
            background-color: #fff;
            box-shadow: 0 0 0 3px rgba(15, 23, 42, 0.1);
        }

        input[readonly] {
            background-color: #e2e8f0;
            color: var(--text-sub);
            cursor: not-allowed;
        }

        .note { font-size: 0.75rem; color: var(--text-sub); margin-top: 6px; }

        /* --- RESULT BOX --- */
        .result-box {
            grid-column: 1 / -1;
            background: var(--primary-color);
            color: white;
            padding: 30px;
            border-radius: 12px;
            margin-top: 20px;
            position: relative;
            overflow: hidden;
        }
        
        /* Tema khusus untuk box usia agar beda sedikit */
        .result-box.blue-theme { background: #1e293b; }

        .result-box::before {
            content: '';
            position: absolute;
            top: -50px;
            right: -50px;
            width: 150px;
            height: 150px;
            background: rgba(255,255,255,0.05);
            border-radius: 50%;
        }

        .result-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .result-row span:first-child { color: #94a3b8; font-size: 0.95rem; }
        .result-row span:last-child { font-weight: 600; font-size: 1.1rem; }

        .result-row.total {
            border-top: 1px solid rgba(255,255,255,0.2);
            border-bottom: none;
            margin-top: 15px;
            padding-top: 20px;
        }

        .result-row.total span:first-child {
            color: var(--accent-color);
            font-weight: 700;
            text-transform: uppercase;
            font-size: 0.9rem;
            letter-spacing: 1px;
        }

        .result-row.total span:last-child {
            font-size: 1.5rem; /* Sedikit lebih kecil dari uang */
            color: var(--success-color);
            font-weight: 700;
        }

        button {
            background-color: rgba(255,255,255,0.1);
            color: white;
            border: 1px solid rgba(255,255,255,0.2);
            padding: 12px;
            width: 100%;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            margin-top: 25px;
            transition: 0.3s;
        }

        button:hover { background-color: white; color: var(--primary-color); }

        /* --- LIST & FOOTER --- */
        ul { padding-left: 20px; }
        li { margin-bottom: 12px; color: var(--text-main); }
        
        footer {
            text-align: center;
            padding: 40px;
            color: var(--text-sub);
            font-size: 0.85rem;
            border-top: 1px solid var(--border-color);
            margin-top: 40px;
            background: white;
        }

        /* --- RESPONSIVE --- */
        @media (max-width: 600px) {
            .nav-pill { padding: 10px 14px; font-size: 0.75rem; }
            .card { padding: 25px; }
            .result-row.total { flex-direction: column; align-items: flex-start; gap: 5px; }
        }
    </style>
</head>
<body>

<header>
    <h1>Credit Analyst Workspace</h1>
    <p>Management Development Program (MDP) &bull; Mega Finance</p>
</header>

<nav>
    <a href="#kalkulator-usia" class="nav-pill active">🎂 Cek Usia</a>
    <a href="#kalkulator-padi" class="nav-pill">🌾 Kalkulator Padi</a>
    <a href="#proyek" class="nav-pill">🚀 Inovasi</a>
    <a href="#profil" class="nav-pill">👤 Profil</a>
</nav>

<div class="container">

    <div id="kalkulator-usia" class="card">
        <h2><span>📅</span> Analisis Usia & Tenor</h2>
        <p class="section-desc">Hitung otomatis usia debitur dan pasangan saat pengajuan (Current) dan saat kredit lunas (Maturity).</p>
        
        <div class="calc-grid">
            <div class="input-group">
                <label>Tgl Lahir Pengaju (Debitur)</label>
                <input type="date" id="dobPengaju" onchange="hitungUsia()">
            </div>

            <div class="input-group">
                <label>Tgl Lahir Pasangan</label>
                <input type="date" id="dobPasangan" onchange="hitungUsia()">
                <p class="note">Biarkan kosong jika single/cerai</p>
            </div>

            <div class="input-group">
                <label>Tenor Kredit (Bulan)</label>
                <input type="number" id="tenorBulan" value="12" min="1" oninput="hitungUsia()">
            </div>

            <div class="result-box blue-theme">
                <div class="result-row">
                    <span>Usia Pengaju (Saat Ini)</span>
                    <span id="txtUsiaPengaju">-</span>
                </div>
                <div class="result-row">
                    <span>Usia Pasangan (Saat Ini)</span>
                    <span id="txtUsiaPasangan">-</span>
                </div>
                <div style="border-bottom: 1px dashed rgba(255,255,255,0.3); margin: 10px 0;"></div>
                
                <div class="result-row">
                    <span>Usia Pengaju + Tenor (Lunas)</span>
                    <span id="txtUsiaPengajuEnd" style="color: var(--accent-color);">-</span>
                </div>
                <div class="result-row">
                    <span>Usia Pasangan + Tenor (Lunas)</span>
                    <span id="txtUsiaPasanganEnd" style="color: var(--accent-color);">-</span>
                </div>

                <button onclick="resetUsia()">⟳ Reset Data Usia</button>
            </div>
        </div>
    </div>
    <div id="kalkulator-padi" class="card">
        <h2>
            <span>🧮</span> Kalkulator Kapasitas Bayar
        </h2>
        <p class="section-desc">Alat estimasi pendapatan petani padi berdasarkan database produktivitas wilayah BPS dan harga GKG terkini.</p>
        
        <div class="calc-grid">
            <div class="input-group">
                <label>Kabupaten/Kota</label>
                <select id="kota" onchange="isiProduktivitas()">
                    <option value="">-- Pilih Wilayah --</option>
                </select>
                <p class="note">Data produktivitas terintegrasi BPS</p>
            </div>

            <div class="input-group">
                <label>Produktivitas (Kg/Ha)</label>
                <input type="number" id="prod" placeholder="0" readonly>
            </div>

            <div class="input-group">
                <label>Luas Lahan (Ha)</label>
                <input type="number" id="luas" value="1" step="0.1" oninput="hitung()">
            </div>

            <div class="input-group">
                <label>Harga Gabah (Rp/Kg)</label>
                <input type="number" id="harga" value="6500" oninput="hitung()">
                <p class="note">Update HPP GKG: 15 Jan 2025</p>
            </div>
            
            <div class="input-group">
                <label>Siklus Panen per Tahun</label>
                <input type="number" id="siklus" value="2" oninput="hitung()">
            </div>

            <div class="input-group">
                <label>Margin Bersih (%)</label>
                <input type="number" id="margin" value="40" oninput="hitung()">
                <p class="note">Net Income setelah biaya operasional</p>
            </div>

            <div class="result-box">
                <div class="result-row">
                    <span>Omset Kotor (Per Panen)</span>
                    <span id="txtOmset">Rp 0</span>
                </div>
                <div class="result-row">
                    <span>Pendapatan Bersih (Per Panen)</span>
                    <span id="txtBersihPanen">Rp 0</span>
                </div>
                <div class="result-row total">
                    <span>Estimasi Disposable Income / Bulan</span>
                    <span id="txtIncomeBulan">Rp 0</span>
                </div>
                <button onclick="resetForm()">⟳ Reset Perhitungan Padi</button>
            </div>
        </div>
    </div>

    <div id="proyek" class="card">
        <h2><span>🚀</span> Terobosan Strategis</h2>
        <ul>
            <li><strong>Automasi Rekening Koran (Gemini AI):</strong> Ekstraksi data mutasi bank dari PDF ke Excel secara instan.</li>
            <li><strong>Validasi Lahan (Spatial Analysis):</strong> Integrasi data produktivitas BPS dengan Citra Satelit (ATR/BPN) untuk verifikasi lokasi.</li>
            <li><strong>Excel Tools Development:</strong> Dashboard monitoring fasilitas aktif SLIK/Pefindo.</li>
        </ul>
    </div>

    <div id="profil" class="card">
        <h2><span>👤</span> Analyst Profile</h2>
        <div style="display: grid; grid-template-columns: 100px 1fr; gap: 20px; align-items: center;">
            <div style="width: 80px; height: 80px; background: #e2e8f0; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 2rem;">👨‍💼</div>
            <div>
                <p style="margin: 0; font-weight: bold; font-size: 1.1rem;">Credit Analyst (MDP)</p>
                <p style="margin: 5px 0; color: var(--text-sub);">Fokus: Analisis risiko kredit mikro, Turlap, & Pengembangan Tools.</p>
            </div>
        </div>
    </div>

</div>

<footer>
    &copy; 2026 Credit Analyst Dashboard system.<br>Developed for Mega Finance MDP.
</footer>

<script>
    /* =======================================
       SCRIPT 1: LOGIKA KALKULATOR USIA (BARU)
       ======================================= */
    function hitungUsia() {
        const tglLahirPengaju = document.getElementById('dobPengaju').value;
        const tglLahirPasangan = document.getElementById('dobPasangan').value;
        const tenorBulan = parseInt(document.getElementById('tenorBulan').value) || 0;

        // Hitung Pengaju
        if (tglLahirPengaju) {
            const hasilPengaju = kalkulasiDetail(tglLahirPengaju, tenorBulan);
            document.getElementById('txtUsiaPengaju').innerText = hasilPengaju.usiaSaatIni;
            document.getElementById('txtUsiaPengajuEnd').innerText = hasilPengaju.usiaAkhir;
        } else {
            document.getElementById('txtUsiaPengaju').innerText = "-";
            document.getElementById('txtUsiaPengajuEnd').innerText = "-";
        }

        // Hitung Pasangan
        if (tglLahirPasangan) {
            const hasilPasangan = kalkulasiDetail(tglLahirPasangan, tenorBulan);
            document.getElementById('txtUsiaPasangan').innerText = hasilPasangan.usiaSaatIni;
            document.getElementById('txtUsiaPasanganEnd').innerText = hasilPasangan.usiaAkhir;
        } else {
            document.getElementById('txtUsiaPasangan').innerText = "-";
            document.getElementById('txtUsiaPasanganEnd').innerText = "-";
        }
    }

    function kalkulasiDetail(tglLahirStr, tenorBulan) {
        const today = new Date();
        const dob = new Date(tglLahirStr);
        
        // 1. Hitung Usia Saat Ini
        let years = today.getFullYear() - dob.getFullYear();
        let months = today.getMonth() - dob.getMonth();
        if (months < 0 || (months === 0 && today.getDate() < dob.getDate())) {
            years--;
            months += 12;
        }
        if (today.getDate() < dob.getDate()) { months--; }
        if (months < 0) { months += 12; }
        
        const usiaSaatIniStr = `${years} Thn ${months} Bln`;

        // 2. Hitung Usia Akhir Tenor (Maturity Age)
        const maturityDate = new Date(today);
        maturityDate.setMonth(today.getMonth() + tenorBulan);

        let endYears = maturityDate.getFullYear() - dob.getFullYear();
        let endMonths = maturityDate.getMonth() - dob.getMonth();
        
        if (endMonths < 0 || (endMonths === 0 && maturityDate.getDate() < dob.getDate())) {
            endYears--;
            endMonths += 12;
        }
        if (maturityDate.getDate() < dob.getDate()) { endMonths--; }
        if (endMonths < 0) { endMonths += 12; }

        const usiaAkhirStr = `${endYears} Thn ${endMonths} Bln`;

        return {
            usiaSaatIni: usiaSaatIniStr,
            usiaAkhir: usiaAkhirStr
        };
    }

    function resetUsia() {
        document.getElementById('dobPengaju').value = "";
        document.getElementById('dobPasangan').value = "";
        document.getElementById('tenorBulan').value = "12";
        hitungUsia();
    }


    /* =======================================
       SCRIPT 2: LOGIKA KALKULATOR PADI (LAMA)
       ======================================= */
    const dbWilayah = {"Aceh Barat": 5482, "Aceh Barat Daya": 5515, "Aceh Besar": 5182, "Aceh Jaya": 5386, "Aceh Selatan": 4644, "Aceh Singkil": 3790, "Aceh Tamiang": 4748, "Aceh Tengah": 5483, "Aceh Tenggara": 6225, "Aceh Timur": 5155, "Aceh Utara": 5609, "Agam": 4839, "Alor": 3045, "Asahan": 5956, "Badung": 6930, "Balangan": 3624, "Bandung": 6433, "Bandung Barat": 5398, "Banggai": 4112, "Banggai Kepulauan": 3037, "Banggai Laut": 4176, "Bangkalan": 4782, "Bangli": 5209, "Banjar": 3779, "Banjarnegara": 5690, "Bantaeng": 4566, "Banyu Asin": 5118, "Banyumas": 5280, "Banyuwangi": 5884, "Barito Kuala": 3736, "Barito Selatan": 3127, "Barito Timur": 3817, "Barito Utara": 2569, "Barru": 5398, "Batang": 5479, "Batang Hari": 3785, "Batu Bara": 5820, "Bekasi": 4946, "Belu": 3144, "Bener Meriah": 4893, "Bengkalis": 3867, "Bengkayang": 3421, "Bengkulu Selatan": 4498, "Bengkulu Tengah": 3647, "Bengkulu Utara": 4379, "Berau": 2942, "Bima": 4850, "Bireuen": 6071, "Blitar": 5559, "Blora": 5002, "Bogor": 5973, "Bojonegoro": 5415, "Bolaang Mongondow": 4738, "Bolaang Mongondow Selatan": 4203, "Bolaang Mongondow Timur": 4176, "Bolaang Mongondow Utara": 4203, "Bombana": 4479, "Bondowoso": 5127, "Bone": 4779, "Boyolali": 5570, "Brebes": 5501, "Buleleng": 5504, "Bulukumba": 5056, "Bulungan": 3590, "Bungo": 4398, "Buol": 4107, "Buru": 3849, "Buton": 3625, "Buton Selatan": 3454, "Buton Tengah": 3159, "Buton Utara": 3500, "Ciamis": 5332, "Cianjur": 5991, "Cilacap": 6123, "Cirebon": 6045, "Dairi": 5420, "Deli Serdang": 6244, "Demak": 5936, "Dharmasraya": 4536, "Dompu": 4678, "Donggala": 4849, "Empat Lawang": 4912, "Ende": 4429, "Enrekang": 4559, "Flores Timur": 2036, "Garut": 6351, "Gayo Lues": 5146, "Gianyar": 6106, "Gowa": 4857, "Gresik": 6085, "Grobogan": 6175, "Gunung Mas": 2051, "Hulu Sungai Selatan": 4711, "Hulu Sungai Tengah": 4753, "Hulu Sungai Utara": 5229, "Humbang Hasundutan": 4389, "Indragiri Hilir": 3597, "Indragiri Hulu": 4246, "Indramayu": 6573, "Jember": 5191, "Jembrana": 6317, "Jeneponto": 4775, "Jepara": 5190, "Jombang": 6182, "Kampar": 4101, "Kapuas": 3315, "Kapuas Hulu": 2908, "Karang Asem": 6010, "Karanganyar": 6083, "Karawang": 5689, "Karo": 5456, "Katingan": 3201, "Kaur": 4333, "Kayong Utara": 3582, "Kebumen": 5178, "Kediri": 5708, "Kendal": 5998, "Kepahiang": 4589, "Kepulauan Mentawai": 2651, "Kepulauan Meranti": 3271, "Kepulauan Sangihe": 4303, "Kepulauan Selayar": 4004, "Kepulauan Talaud": 4448, "Kepulauan Tanimbar": 1554, "Kerinci": 5314, "Ketapang": 3750, "Klaten": 5623, "Klungkung": 6235, "Kolaka": 4597, "Kolaka Timur": 4428, "Kolaka Utara": 4069, "Konawe": 4058, "Konawe Kepulauan": 3769, "Konawe Selatan": 3986, "Konawe Utara": 3529, "Kota Balikpapan": 2534, "Kota Banda Aceh": 5079, "Kota Bandar Lampung": 5120, "Kota Bandung": 6959, "Kota Banjar": 6046, "Kota Banjar Baru": 3117, "Kota Banjarmasin": 4257, "Kota Baru": 3989, "Kota Batu": 7272, "Kota Baubau": 4889, "Kota Bekasi": 4286, "Kota Bengkulu": 4894, "Kota Bima": 5389, "Kota Binjai": 4497, "Kota Bitung": 4431, "Kota Blitar": 6484, "Kota Bogor": 5305, "Kota Bukittinggi": 6311, "Kota Cilegon": 5307, "Kota Cimahi": 6138, "Kota Cirebon": 6394, "Kota Denpasar": 6809, "Kota Depok": 5624, "Kota Dumai": 2356, "Kota Gunungsitoli": 4486, "Kota Jambi": 4185, "Kota Kediri": 6319, "Kota Kendari": 4116, "Kota Kotamobagu": 4775, "Kota Kupang": 5050, "Kota Langsa": 5038, "Kota Lhokseumawe": 4317, "Kota Lubuklinggau": 5226, "Kota Madiun": 5758, "Kota Magelang": 5023, "Kota Makassar": 5697, "Kota Malang": 6516, "Kota Manado": 4224, "Kota Mataram": 6614, "Kota Medan": 6029, "Kota Metro": 5705, "Kota Mojokerto": 6040, "Kota Padang": 4861, "Kota Padang Panjang": 5886, "Kota Padangsidimpuan": 5316, "Kota Pagar Alam": 5077, "Kota Palembang": 4558, "Kota Palopo": 5068, "Kota Palu": 4701, "Kota Parepare": 5191, "Kota Pariaman": 4889, "Kota Pasuruan": 5502, "Kota Payakumbuh": 4757, "Kota Pekalongan": 6475, "Kota Pekanbaru": 4111, "Kota Pematang Siantar": 5823, "Kota Pontianak": 3517, "Kota Prabumulih": 3854, "Kota Probolinggo": 6106, "Kota Salatiga": 6644, "Kota Samarinda": 4244, "Kota Sawah Lunto": 5455, "Kota Semarang": 5504, "Kota Serang": 5349, "Kota Singkawang": 3610, "Kota Solok": 5678, "Kota Subulussalam": 4378, "Kota Sukabumi": 5858, "Kota Sungai Penuh": 6181, "Kota Surabaya": 5554, "Kota Surakarta": 5759, "Kota Tangerang": 6217, "Kota Tanjung Balai": 4848, "Kota Tarakan": 3200, "Kota Tasikmalaya": 5419, "Kota Tebing Tinggi": 5868, "Kota Tegal": 6084, "Kota Tomohon": 5938, "Kotawaringin Barat": 3806, "Kotawaringin Timur": 3351, "Kuantan Singingi": 4705, "Kubu Raya": 2894, "Kudus": 5868, "Kuningan": 5381, "Kupang": 5040, "Kutai Barat": 3001, "Kutai Kartanegara": 4093, "Kutai Timur": 3678, "Labuhan Batu": 5254, "Labuhan Batu Selatan": 4129, "Labuhan Batu Utara": 4536, "Lahat": 5097, "Lamandau": 2976, "Lamongan": 5941, "Lampung Barat": 4703, "Lampung Selatan": 5491, "Lampung Tengah": 5701, "Lampung Timur": 5172, "Lampung Utara": 4186, "Landak": 3296, "Langkat": 4858, "Lebak": 4823, "Lebong": 6248, "Lembata": 2934, "Lima Puluh Kota": 4358, "Lombok Barat": 3432, "Lombok Tengah": 5051, "Lombok Timur": 5366, "Lombok Utara": 6078, "Lumajang": 5407, "Luwu": 5359, "Luwu Timur": 5802, "Luwu Utara": 5419, "Madiun": 5958, "Magelang": 5070, "Magetan": 6272, "Mahakam Ulu": 2407, "Majalengka": 5550, "Majene": 5267, "Malaka": 3825, "Malang": 6211, "Malinau": 3751, "Maluku Tengah": 4079, "Mamasa": 3999, "Mamuju": 4920, "Mamuju Tengah": 4709, "Mandailing Natal": 4224, "Manggarai": 4623, "Manggarai Barat": 4475, "Manggarai Timur": 4600, "Maros": 4804, "Melawi": 2994, "Mempawah": 3186, "Merangin": 3836, "Mesuji": 4816, "Minahasa": 4802, "Minahasa Selatan": 4131, "Minahasa Tenggara": 4361, "Minahasa Utara": 3858, "Mojokerto": 6041, "Morowali": 4015, "Morowali Utara": 4161, "Muara Enim": 5111, "Muaro Jambi": 3372, "Mukomuko": 5782, "Muna": 3514, "Muna Barat": 3997, "Murung Raya": 1254, "Musi Banyuasin": 4964, "Musi Rawas": 5833, "Musi Rawas Utara": 3919, "Nagan Raya": 3865, "Nagekeo": 5072, "Ngada": 4925, "Nganjuk": 5811, "Ngawi": 6221, "Nias": 4532, "Nias Barat": 4459, "Nias Selatan": 3533, "Nias Utara": 3553, "Nunukan": 3631, "Ogan Ilir": 4977, "Ogan Komering Ilir": 5917, "Ogan Komering Ulu": 4694, "Ogan Komering Ulu Selatan": 6230, "Ogan Komering Ulu Timur": 6565, "Pacitan": 4770, "Padang Lawas": 3992, "Padang Lawas Utara": 3957, "Padang Pariaman": 4452, "Pakpak Bharat": 4134, "Palangka Raya": 3750, "Pamekasan": 5169, "Pandeglang": 5352, "Pangandaran": 4898, "Pangkajene Dan Kepulauan": 4141, "Parigi Moutong": 4742, "Pasaman": 4404, "Pasaman Barat": 4647, "Pasangkayu": 5272, "Paser": 4663, "Pasuruan": 5429, "Pati": 5463, "Pekalongan": 5502, "Pelalawan": 3974, "Pemalang": 5707, "Penajam Paser Utara": 3636, "Penukal Abab Lematang Ilir": 4307, "Pesawaran": 5291, "Pesisir Barat": 4654, "Pesisir Selatan": 4878, "Pidie": 6262, "Pidie Jaya": 6770, "Pinrang": 6107, "Polewali Mandar": 5521, "Ponorogo": 5982, "Poso": 3685, "Pringsewu": 6353, "Probolinggo": 5353, "Pulang Pisau": 4086, "Purbalingga": 5470, "Purwakarta": 5890, "Purworejo": 5387, "Rejang Lebong": 4686, "Rembang": 4898, "Rokan Hilir": 4435, "Rokan Hulu": 4378, "Rote Ndao": 3889, "Sabu Raijua": 3854, "Sambas": 3057, "Samosir": 5997, "Sampang": 4413, "Sanggau": 2567, "Sarolangun": 4116, "Sekadau": 2890, "Seluma": 4437, "Semarang": 5964, "Seram Bagian Barat": 3229, "Seram Bagian Timur": 3299, "Serang": 5384, "Serdang Bedagai": 6517, "Seruyan": 2760, "Siak": 4415, "Sidenreng Rappang": 5175, "Sidoarjo": 6509, "Sigi": 4709, "Sijunjung": 3856, "Sikka": 3708, "Simalungun": 5742, "Simeulue": 3422, "Sinjai": 4680, "Sintang": 2691, "Situbondo": 5420, "Solok": 4728, "Solok Selatan": 3831, "Soppeng": 5336, "Sragen": 6556, "Subang": 5912, "Sukabumi": 5720, "Sukamara": 3803, "Sukoharjo": 7531, "Sumba Barat": 3682, "Sumba Barat Daya": 3021, "Sumba Tengah": 4271, "Sumba Timur": 3657, "Sumbawa": 5532, "Sumbawa Barat": 4899, "Sumedang": 5500, "Sumenep": 5155, "Tabalong": 4729, "Tabanan": 6066, "Takalar": 4189, "Tana Tidung": 3242, "Tana Toraja": 4528, "Tanah Bumbu": 4026, "Tanah Datar": 4787, "Tanah Laut": 4273, "Tangerang": 5117, "Tanggamus": 5639, "Tanjung Jabung Barat": 4681, "Tanjung Jabung Timur": 3888, "Tapanuli Selatan": 4919, "Tapanuli Tengah": 3950, "Tapanuli Utara": 5129, "Tapin": 4146, "Tasikmalaya": 5329, "Tebo": 4624, "Tegal": 5339, "Temanggung": 6476, "Timor Tengah Selatan": 4439, "Timor Tengah Utara": 3758, "Toba Samosir": 6014, "Tojo Una-Una": 4408, "Toli-Toli": 4698, "Toraja Utara": 5012, "Trenggalek": 5749, "Tuban": 6211, "Tulang Bawang Barat": 4056, "Tulangbawang": 4979, "Tulungagung": 6287, "Wajo": 4766, "Way Kanan": 4789, "Wonogiri": 5276, "Wonosobo": 4984};

    const elmKota = document.getElementById('kota');
    
    // Populasi Dropdown dengan data yang diurutkan
    Object.keys(dbWilayah).sort().forEach(kota => {
        let opt = document.createElement('option');
        opt.value = dbWilayah[kota]; // Nilai adalah produktivitas (Kg)
        opt.text = kota;
        elmKota.add(opt);
    });

    // Fungsi: Mengisi Produktivitas Otomatis
    function isiProduktivitas() {
        let val = elmKota.value;
        if (val) {
            document.getElementById('prod').value = val;
            hitung();
        } else {
            document.getElementById('prod').value = "";
            document.getElementById('txtOmset').innerText = "Rp 0";
            document.getElementById('txtBersihPanen').innerText = "Rp 0";
            document.getElementById('txtIncomeBulan').innerText = "Rp 0";
        }
    }

    // Fungsi: Menghitung Logika Finansial
    function hitung() {
        // Ambil nilai dari form
        let luas = parseFloat(document.getElementById('luas').value) || 0;
        let prod = parseFloat(document.getElementById('prod').value) || 0;
        let harga = parseFloat(document.getElementById('harga').value) || 0;
        let siklus = parseFloat(document.getElementById('siklus').value) || 0;
        let margin = parseFloat(document.getElementById('margin').value) || 0;

        // Rumus 1: Omset (Kotor) = Luas x Prod (Kg) x Harga
        let omset = luas * prod * harga;

        // Rumus 2: Pendapatan Bersih (Per Panen) = Omset x Margin%
        let bersihPanen = omset * (margin / 100);
        
        // Rumus 3: Income per Bulan = (Bersih Panen x Jumlah Siklus) / 12 Bulan
        let incomeBulan = (bersihPanen * siklus) / 12;

        // Tampilkan Hasil (Format Rupiah)
        document.getElementById('txtOmset').innerText = formatRp(omset);
        document.getElementById('txtBersihPanen').innerText = formatRp(bersihPanen);
        document.getElementById('txtIncomeBulan').innerText = formatRp(incomeBulan);
    }

    // Helper: Format Rupiah
    function formatRp(num) {
        return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits:0 }).format(num);
    }

    // Fungsi: Reset Form Padi
    function resetForm() {
        elmKota.value = "";
        document.getElementById('prod').value = "";
        document.getElementById('luas').value = 1;
        document.getElementById('harga').value = 6500;
        document.getElementById('siklus').value = 2;
        document.getElementById('margin').value = 40;
        
        document.getElementById('txtOmset').innerText = "Rp 0";
        document.getElementById('txtBersihPanen').innerText = "Rp 0";
        document.getElementById('txtIncomeBulan').innerText = "Rp 0";
    }
</script>

</body>
</html>
