<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mega Finance - Analyst Dashboard</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #2563eb; --primary-dark: #1e40af;
            --secondary: #f59e0b;
            --success: #10b981; --danger: #ef4444;
            --bg: #f3f4f6; --card-bg: #ffffff;
            --text-main: #1f2937; --text-sub: #6b7280;
            --shadow: 0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05);
        }

        * { box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background-color: var(--bg); color: var(--text-main); margin: 0; line-height: 1.5; scroll-behavior: smooth; }

        /* HEADER */
        header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-dark) 100%);
            color: white;
            padding: 40px 20px 60px 20px;
            text-align: center;
            border-bottom-left-radius: 30px;
            border-bottom-right-radius: 30px;
            box-shadow: var(--shadow);
        }
        header h1 { margin: 0; font-weight: 800; font-size: 2rem; letter-spacing: -0.5px; }
        header p { margin: 10px 0 0; opacity: 0.9; font-weight: 300; font-size: 1rem; }

        /* NAVIGASI */
        nav {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: -30px;
            margin-bottom: 40px;
            padding: 0 20px;
            position: sticky;
            top: 20px;
            z-index: 100;
        }
        .nav-btn {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 12px 25px;
            border-radius: 50px;
            text-decoration: none;
            color: var(--text-main);
            font-weight: 600;
            font-size: 0.9rem;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            transition: 0.3s;
            border: 1px solid white;
        }
        .nav-btn:hover, .nav-btn.active {
            background: var(--secondary);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 10px 15px rgba(245, 158, 11, 0.3);
        }

        /* CONTAINER */
        .container { max-width: 900px; margin: 0 auto; padding: 0 20px 50px 20px; }

        /* CARD STYLE */
        .card {
            background: var(--card-bg);
            border-radius: 20px;
            box-shadow: var(--shadow);
            padding: 40px;
            margin-bottom: 40px;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeUp 0.6s ease-out forwards;
        }
        .card:nth-child(2) { animation-delay: 0.2s; }
        
        .card-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 30px;
            border-bottom: 2px solid #f3f4f6;
            padding-bottom: 20px;
        }
        .card-icon {
            width: 50px; height: 50px;
            background: #eff6ff; color: var(--primary);
            border-radius: 12px;
            display: flex; align-items: center; justify-content: center;
            font-size: 1.5rem;
        }
        .card-title h2 { margin: 0; font-size: 1.5rem; color: var(--text-main); }
        .card-title p { margin: 5px 0 0; font-size: 0.85rem; color: var(--text-sub); }

        /* FORM ELEMENTS */
        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
        .input-group { margin-bottom: 20px; }
        label { display: block; font-weight: 600; margin-bottom: 8px; font-size: 0.9rem; color: var(--text-main); }
        input, select, textarea {
            width: 100%; padding: 14px;
            border: 1px solid #e5e7eb; border-radius: 10px;
            font-size: 1rem; transition: 0.2s;
            font-family: inherit; background: #f9fafb;
        }
        input:focus, select:focus, textarea:focus {
            background: white; border-color: var(--primary); outline: none;
            box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
        }
        textarea { resize: vertical; min-height: 100px; }

        /* BUTTONS */
        .btn-action {
            background: var(--primary); color: white;
            border: none; padding: 16px; width: 100%;
            border-radius: 12px; font-weight: 700; font-size: 1rem;
            cursor: pointer; transition: 0.2s;
            display: flex; align-items: center; justify-content: center; gap: 8px;
        }
        .btn-action:hover { background: var(--primary-dark); transform: translateY(-1px); }
        .btn-action:disabled { background: #cbd5e1; cursor: not-allowed; }

        /* RESULT BOX */
        .result-panel {
            background: #1f2937; color: white;
            border-radius: 16px; padding: 30px;
            margin-top: 25px; text-align: center;
            display: none; animation: popIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        .res-label { text-transform: uppercase; font-size: 0.75rem; letter-spacing: 1px; opacity: 0.7; margin-bottom: 5px; }
        .res-value { font-size: 2.5rem; font-weight: 800; color: var(--secondary); margin: 0; line-height: 1.2; }
        .res-sub { font-size: 0.9rem; margin-top: 10px; padding-top: 10px; border-top: 1px solid rgba(255,255,255,0.1); opacity: 0.9; }
        
        .badge { display: inline-block; padding: 8px 16px; border-radius: 50px; font-size: 0.85rem; font-weight: 700; margin-top: 15px; }
        .bg-ok { background: #dcfce7; color: #166534; }
        .bg-fail { background: #fee2e2; color: #991b1b; }

        /* MANUAL MODE */
        #manualMode { display: none; background: #fff7ed; border: 1px dashed var(--secondary); padding: 20px; border-radius: 12px; margin-top: 20px; }
        .link-map { color: var(--secondary); font-weight: 700; text-decoration: none; display: block; margin-bottom: 10px; font-size: 0.9rem; }

        /* RESPONSIVE */
        @media (max-width: 768px) {
            .grid-2 { grid-template-columns: 1fr; }
            header h1 { font-size: 1.5rem; }
            nav { flex-wrap: wrap; }
        }

        /* ANIMATIONS */
        @keyframes fadeUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes popIn { from { transform: scale(0.9); opacity: 0; } to { transform: scale(1); opacity: 1; } }
    </style>
</head>
<body>

<header>
    <h1>Credit Analyst Workspace</h1>
    <p>Integrated Tools • Mega Finance MDP</p>
</header>

<nav>
    <a href="#coverage" class="nav-btn active" onclick="switchTab(this)">📍 Cek Coverage</a>
    <a href="#padi" class="nav-btn" onclick="switchTab(this)">🌾 Kalkulator Padi</a>
</nav>

<div class="container">

    <div id="coverage" class="card">
        <div class="card-header">
            <div class="card-icon">📍</div>
            <div class="card-title">
                <h2>Smart Coverage Check</h2>
                <p>Cek jarak otomatis dengan deep-parsing alamat.</p>
            </div>
        </div>

        <div class="input-group">
            <label>1. Pilih Kantor Cabang</label>
            <select id="cabangSelect" onchange="resetCoverageUI()">
                <option value="">-- Cari Nama Kota/Cabang --</option>
            </select>
            <small id="cabangAddress" style="display:block; margin-top:5px; color:var(--text-sub); font-size:0.85rem;">-</small>
        </div>

        <div class="input-group">
            <label>2. Alamat Lengkap Konsumen</label>
            <textarea id="alamatInput" placeholder="Contoh: PERUM. BUKIT SEJAHTERA BLOK BX-18 RT. 017 RW. 004 Kel. Karang Jaya Kec. Gandus Kota Palembang" oninput="resetCoverageUI()"></textarea>
        </div>

        <button id="btnProses" onclick="prosesCoverageSmart()" class="btn-action">🚀 Hitung Jarak</button>

        <div id="resultBox" class="result-panel">
            <div class="res-label">Jarak Garis Lurus</div>
            <div id="txtJarak" class="res-value">0 Km</div>
            <div class="res-sub">Batas Max: <span id="txtLimit">-</span></div>
            <div id="badgeStatus" class="badge">STATUS</div>
        </div>

        <div id="manualMode">
            <strong>⚠️ Lokasi Sulit Ditemukan</strong><br>
            <small style="color:var(--text-sub)">Gunakan Google Maps untuk akurasi:</small><br><br>
            <a id="linkGmaps" href="#" target="_blank" class="link-map">🗺️ Buka Google Maps & Copy Koordinat</a>
            <input type="text" id="coordInput" placeholder="Paste: -6.xxxx, 106.xxxx" style="margin-bottom:10px;">
            <button onclick="hitungManual()" class="btn-action" style="background:var(--secondary);">Hitung Manual</button>
        </div>
    </div>


    <div id="padi" class="card">
        <div class="card-header">
            <div class="card-icon">🌾</div>
            <div class="card-title">
                <h2>Kalkulator Kapasitas Bayar</h2>
                <p>Estimasi pendapatan petani padi berdasarkan data BPS.</p>
            </div>
        </div>

        <div class="grid-2">
            <div class="input-group">
                <label>Wilayah (Kab/Kota)</label>
                <select id="kota" onchange="isiProduktivitas()">
                    <option value="">-- Pilih Wilayah --</option>
                </select>
            </div>
            <div class="input-group">
                <label>Produktivitas (Kg/Ha)</label>
                <input type="number" id="prod" readonly style="background:#f3f4f6;">
            </div>
            <div class="input-group">
                <label>Luas Lahan (Ha)</label>
                <input type="number" id="luas" value="1" step="0.1" oninput="hitungPadi()">
            </div>
            <div class="input-group">
                <label>Harga Gabah (Rp/Kg)</label>
                <input type="number" id="harga" value="6500" oninput="hitungPadi()">
            </div>
            <div class="input-group">
                <label>Panen per Tahun</label>
                <input type="number" id="siklus" value="2" oninput="hitungPadi()">
            </div>
            <div class="input-group">
                <label>Margin Bersih (%)</label>
                <input type="number" id="margin" value="40" oninput="hitungPadi()">
            </div>
        </div>

        <div id="padiResult" class="result-panel" style="display:block; margin-top:10px; background:#f0fdf4; color:var(--text-main); border:1px solid #bbf7d0;">
            <div class="res-label" style="color:var(--text-sub);">ESTIMASI INCOME BERSIH / BULAN</div>
            <div id="txtIncomeBulan" class="res-value" style="color:#15803d;">Rp 0</div>
            <div class="res-sub" style="border-top-color:#dcfce7;">
                Omset per Panen: <strong id="txtOmset">Rp 0</strong>
            </div>
        </div>
    </div>

</div>

<footer>© 2026 Mega Finance MDP Tools</footer>

<script>
    /* =========================================
       LOGIKA UMUM (TABS)
    ========================================= */
    function switchTab(el) {
        // Reset active class
        document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
        el.classList.add('active');
    }

    /* =========================================
       LOGIKA 1: SMART COVERAGE (V.10)
    ========================================= */
    const dbCabang = [
        { kode: "BGZ", nama: "BOGOR (BGZ)", region: "JABODETABEK", lat: -6.597623, lon: 106.790326, alamat: "Jl. Veteran no.49 ABC Panaragan,Bogor" },
        { kode: "BEZ", nama: "BEKASI (BEZ)", region: "JABODETABEK", lat: -6.240432, lon: 106.995632, alamat: "JL. Sersan Aswan no.9 Bekasi" },
        { kode: "DPZ", nama: "DEPOK (DPZ)", region: "JABODETABEK", lat: -6.375321, lon: 106.833412, alamat: "JL. Margonda raya no.299 A Depok" },
        { kode: "FMZ", nama: "FATMAWATI (FMZ)", region: "JABODETABEK", lat: -6.275123, lon: 106.797231, alamat: "Jl. Fatmawati Raya No 52 B-C" },
        { kode: "CBZ", nama: "CIBUBUR (CBZ)", region: "JABODETABEK", lat: -6.371234, lon: 106.890123, alamat: "Jl. Raya Kranggan Ruko Villa The Lima Kav. 15. Kel. Jatisampurna, Kec. Jatisampurna, Kota Bekasi 17433" },
        { kode: "BDZ", nama: "BANDUNG & PADALARANG ELE (BDZ)", region: "JAWA", lat: -6.925621, lon: 107.636512, alamat: "Jl. Gatot Subroto No. 278 Kec Batununggal Kel Lingkungan Gumuruh, Kota Bandung, Jawa Barat" },
        { kode: "SBZ", nama: "SURABAYA ELEKTRONIK (SBZ)", region: "JAWA", lat: -7.283123, lon: 112.765432, alamat: "Jl. Kertajaya Indah Timur Blk 16A-03, Komplek Ruko Mega Galaxy, Kel. Klampisngasem, Kec. Sukolilo Surabaya" },
        { kode: "SEZ", nama: "SEMARANG (SEZ)", region: "JAWA", lat: -7.001234, lon: 110.432123, alamat: "Ruko Metroplaza Blok C10 C11, Jl. MT. Haryono No. 970, Kel. Lamper Kidul, Kec. Semarang Selatan, Semarang" },
        { kode: "AMK", nama: "AIR MOLEK (AMK)", region: "LUAR JAWA", lat: -0.369200, lon: 102.293500, alamat: "Jl. Jend. Sudirman No.10, Air Molek II, Riau" },
        { kode: "BPZ", nama: "BALIKPAPAN (BPZ)", region: "LUAR JAWA", lat: -1.246700, lon: 116.866500, alamat: "Ruko Palace 8 Jl. MT Haryono, Balikpapan" },
        { kode: "BLZ", nama: "BANDAR LAMPUNG (BLZ)", region: "LUAR JAWA", lat: -5.433600, lon: 105.274200, alamat: "Jl. Jend. Gatot Subroto No 41B, Bandar Lampung" },
        { kode: "BJZ", nama: "BANJARMASIN (BJZ)", region: "LUAR JAWA", lat: -3.364900, lon: 114.638400, alamat: "Jl. Ahmad Yani Km 8,8, Banjarmasin" },
        { kode: "BWZ", nama: "BANYUWANGI (BWZ)", region: "JAWA", lat: -8.307222, lon: 114.298333, alamat: "Jl. Raya Jember-Banyuwangi, Banyuwangi" },
        { kode: "BAZ", nama: "BATAM (BAZ)", region: "LUAR JAWA", lat: 1.045626, lon: 103.957589, alamat: "Komplek Ruko Sinar Jaya No. 08, Batam" },
        { kode: "BKZ", nama: "BENGKULU (BKZ)", region: "LUAR JAWA", lat: -3.819777, lon: 102.304494, alamat: "Jl. Pangeran Natadirja KM 6,5, Bengkulu" },
        { kode: "BTZ", nama: "BLITAR (BTZ)", region: "JAWA", lat: -8.101417, lon: 112.167222, alamat: "JL. Anjasmoro No 5C, Blitar" },
        { kode: "BRZ", nama: "BOJONEGORO (BRZ)", region: "JAWA", lat: -7.164167, lon: 111.886389, alamat: "Jl. Veteran, Bojonegoro" },
        { kode: "BOZ", nama: "BONE (BOZ)", region: "LUAR JAWA", lat: -4.542222, lon: 120.318056, alamat: "Jl. Gn Jayawijaya Ruko C, Bone" },
        { kode: "CMZ", nama: "CIAMIS (CMZ)", region: "JAWA", lat: -7.327500, lon: 108.336111, alamat: "Jl. Jendral Sudirman No. 02, Ciamis" },
        { kode: "CJZ", nama: "CIANJUR (CJZ)", region: "JAWA", lat: -6.816600, lon: 107.136400, alamat: "Jl. KH. Abdullah Bin Nuh No. 175, Cianjur" },
        { kode: "CNZ", nama: "CIKARANG (CNZ)", region: "JABODETABEK", lat: -6.257500, lon: 107.065500, alamat: "Ruko Tambun City, Bekasi" },
        { kode: "CKZ", nama: "CIKUPA (CKZ)", region: "JABODETABEK", lat: -6.241900, lon: 106.522200, alamat: "Citra Raya, Tangerang" },
        { kode: "CLZ", nama: "CILACAP (CLZ)", region: "JAWA", lat: -7.720800, lon: 109.006900, alamat: "Jl. Gatot Subroto No.25, Cilacap" },
        { kode: "SRZ", nama: "CILEGON (SRZ)", region: "JAWA", lat: -6.108300, lon: 106.137500, alamat: "Jl. Raya Cilegon No. 4, Serang" },
        { kode: "CIZ", nama: "CILEUNGSI (CIZ)", region: "JABODETABEK", lat: -6.481100, lon: 106.851900, alamat: "Jl. Raya Bogor KM 42, Bogor" },
        { kode: "CRZ", nama: "CIREBON (CRZ)", region: "JAWA", lat: -6.721400, lon: 108.553900, alamat: "Komplek Ruko CSB Gold Sunset 11, Cirebon" },
        { kode: "DAZ", nama: "DAYA MM (DAZ)", region: "LUAR JAWA", lat: -5.114700, lon: 119.507500, alamat: "Jl. Perintis Kemerdekaan, Makassar" },
        { kode: "DNZ", nama: "DENPASAR (DNZ)", region: "LUAR JAWA", lat: -8.670458, lon: 115.212629, alamat: "Jl. Imam Bonjol No 341A, Denpasar" },
        { kode: "DUZ", nama: "DUMAI (DUZ)", region: "LUAR JAWA", lat: 1.670300, lon: 101.435500, alamat: "Jl. Sultan Hasanuddin No. 99, Dumai" },
        { kode: "DRZ", nama: "DURI (DRZ)", region: "LUAR JAWA", lat: 1.285000, lon: 101.215000, alamat: "Jl. Hangtuah, Duri" },
        { kode: "GRZ", nama: "GORONTALO (GRZ)", region: "LUAR JAWA", lat: 0.537500, lon: 123.063300, alamat: "Jl. Prof. Dr. HB Jassin, Gorontalo" },
        { kode: "GGZ", nama: "GREEN GARDEN (GGZ)", region: "JABODETABEK", lat: -6.195500, lon: 106.765800, alamat: "Jl. Srengseng Raya, Jakarta Barat" },
        { kode: "GSZ", nama: "GRESIK (GSZ)", region: "JAWA", lat: -7.165800, lon: 112.635500, alamat: "Jl. Dr. Wahidin Sudirohusodo, Gresik" },
        { kode: "JBZ", nama: "JAMBI (JBZ)", region: "LUAR JAWA", lat: -1.615200, lon: 103.605500, alamat: "Jl. Hayam Wuruk Ruko No. 18, Jambi" },
        { kode: "JMZ", nama: "JOMBANG (JMZ)", region: "JAWA", lat: -7.555200, lon: 112.235500, alamat: "Jl. Soekarno Hatta, Jombang" },
        { kode: "KMZ", nama: "KALIMALANG (KMZ)", region: "JABODETABEK", lat: -6.235500, lon: 106.905500, alamat: "Jl. Pahlawan Revolusi, Jakarta Timur" },
        { kode: "KRZ", nama: "KARAWANG (KRZ)", region: "JAWA", lat: -6.305500, lon: 107.285500, alamat: "Grand Taruma, Karawang" },
        { kode: "KIZ", nama: "KEDIRI (KIZ)", region: "JAWA", lat: -7.805500, lon: 112.015500, alamat: "Jl. Dandangan Gendis, Kediri" },
        { kode: "KND", nama: "KENDARI (KND)", region: "LUAR JAWA", lat: -4.025551, lon: 122.508282, alamat: "Jl. Di Panjaitan No.7, Kendari" },
        { kode: "KLZ", nama: "KLATEN (KLZ)", region: "JAWA", lat: -7.703000, lon: 110.601000, alamat: "Jl. Bhayangkara No.26, Klaten" },
        { kode: "KUZ", nama: "KUDUS (KUZ)", region: "JAWA", lat: -6.806000, lon: 110.842000, alamat: "Jl. Pramuka No.368, Kudus" },
        { kode: "KNZ", nama: "KUNINGAN (KNZ)", region: "JAWA", lat: -6.968359, lon: 108.489710, alamat: "Jl. RE. Martadinata No.32, Kuningan" },
        { kode: "LBZ", nama: "LUBUK LINGGAU (LBZ)", region: "LUAR JAWA", lat: -3.267800, lon: 102.937100, alamat: "Jl. Yos Sudarso, Lubuk Linggau" },
        { kode: "LPZ", nama: "LUBUK PAKAM (LPZ)", region: "LUAR JAWA", lat: 3.558000, lon: 98.875000, alamat: "Jl. Jend. Sudirman, Lubuk Pakam" },
        { kode: "MAZ", nama: "MADIUN (MAZ)", region: "JAWA", lat: -7.595800, lon: 111.541100, alamat: "Jl. Raya Nglames, Madiun" },
        { kode: "MGZ", nama: "MAJALENGKA (MGZ)", region: "JAWA", lat: -6.821359, lon: 108.242852, alamat: "Jl. Raya Cicenang, Majalengka" },
        { kode: "MKZ", nama: "MAKASAR (MKZ)", region: "JAWA", lat: -5.147665, lon: 119.432731, alamat: "Jl. Pelita Raya, Makassar" },
        { kode: "MLZ", nama: "MALANG (MLZ)", region: "JAWA", lat: -7.942000, lon: 112.617000, alamat: "Jl. Soekarno Hatta, Malang" },
        { kode: "MTZ", nama: "MATARAM (MTZ)", region: "LUAR JAWA", lat: -8.588000, lon: 116.105000, alamat: "Jl. Airlangga No. 25E, Mataram" },
        { kode: "MDZ", nama: "MEDAN (MDZ)", region: "LUAR JAWA", lat: 3.567000, lon: 98.650000, alamat: "Jl. Setia Budi, Medan" },
        { kode: "MJZ", nama: "MOJOKERTO (MJZ)", region: "JAWA", lat: -7.466400, lon: 112.443700, alamat: "Jl. Pekayon I No. 25, Mojokerto" },
        { kode: "MRZ", nama: "MUARA ENIM (MRZ)", region: "LUAR JAWA", lat: -3.654700, lon: 103.775300, alamat: "Jl. Jendral Sudirman No. 33, Muara Enim" },
        { kode: "PDZ", nama: "PADALARANG (PDZ)", region: "JAWA", lat: -6.867500, lon: 107.517500, alamat: "Jl. Gadobangkong, Padalarang" },
        { kode: "PGY", nama: "PAGUYAMAN (PGY)", region: "LUAR JAWA", lat: 0.710000, lon: 122.580000, alamat: "Jl. Poros Sukamakmur, Gorontalo" },
        { kode: "PGZ", nama: "PALEMBANG (PGZ)", region: "LUAR JAWA", lat: -2.955000, lon: 104.720000, alamat: "Jl Soekarno Hatta No 7-8, Palembang" },
        { kode: "PLZ", nama: "PALOPO (PLZ)", region: "LUAR JAWA", lat: -2.978000, lon: 120.195000, alamat: "Jl. Batara Lattu, Palopo" },
        { kode: "PAZ", nama: "PALU (PAZ)", region: "LUAR JAWA", lat: -0.905000, lon: 119.875000, alamat: "Jl. Jendral Basuki Rahmat, Palu" },
        { kode: "PMZ", nama: "PAMULANG (PMZ)", region: "JABODETABEK", lat: -6.342800, lon: 106.752700, alamat: "Jl. Puspitek No.49, Tangerang Selatan" },
        { kode: "PNZ", nama: "PANDAAN (PNZ)", region: "JAWA", lat: -7.660000, lon: 112.680000, alamat: "Jl. Raya Surabaya - Malang, Pasuruan" },
        { kode: "PRZ", nama: "PARE-PARE (PRZ)", region: "LUAR JAWA", lat: -4.016700, lon: 119.623600, alamat: "Jl. H. Agussalim, Parepare" },
        { kode: "PKZ", nama: "PEKALONGAN (PKZ)", region: "JAWA", lat: -6.889000, lon: 109.675000, alamat: "Ruko Dupan Square, Pekalongan" },
        { kode: "PBZ", nama: "PEKANBARU (PBZ)", region: "LUAR JAWA", lat: 0.485000, lon: 101.430000, alamat: "Jl. Arifin Ahmad, Pekanbaru" },
        { kode: "PMS", nama: "PEMATANG SIANTAR (PMS)", region: "LUAR JAWA", lat: 2.960000, lon: 99.080000, alamat: "Jl. Asahan KM 3, Pematang Siantar" },
        { kode: "PZZ", nama: "PURWAKARTA (PZZ)", region: "JAWA", lat: -6.556100, lon: 107.442800, alamat: "Jl. Ipik Gandhamanah, Purwakarta" },
        { kode: "PUZ", nama: "PURWODADI (PUZ)", region: "JAWA", lat: -7.086400, lon: 110.915800, alamat: "Ruko Cahaya Griya Mandiri, Purwodadi" },
        { kode: "PWZ", nama: "PURWOKERTO (PWZ)", region: "JAWA", lat: -7.424400, lon: 109.239200, alamat: "Jl. Kol Sugiono No. 41, Purwokerto" },
        { kode: "RWZ", nama: "RAWASARI (RWZ)", region: "JABODETABEK", lat: -6.168300, lon: 106.876400, alamat: "Komp. Ruko Mega Grosir Cempaka Mas, Jakarta" },
        { kode: "STZ", nama: "SALATIGA (STZ)", region: "JAWA", lat: -7.330500, lon: 110.508400, alamat: "Ruko Emperium Centra Niaga, Semarang" },
        { kode: "SAZ", nama: "SAMARINDA (SAZ)", region: "LUAR JAWA", lat: -0.491700, lon: 117.145800, alamat: "Jl. Abd. Wahab Syahrani, Samarinda" },
        { kode: "SGT", nama: "SANGATTA (SGT)", region: "LUAR JAWA", lat: 0.505000, lon: 117.538300, alamat: "Jl. Pendidikan, Sangatta" },
        { kode: "SEZ", nama: "SEMARANG (SEZ)", region: "JAWA", lat: -7.001234, lon: 110.432123, alamat: "Jl. MT. Haryono, Semarang" },
        { kode: "SGZ", nama: "SEMARANG 2 (SGZ)", region: "JAWA", lat: -7.027000, lon: 110.505000, alamat: "Jl. Raya Bandung Rejo, Demak" },
        { kode: "TGZ", nama: "TANGERANG (TGZ)", region: "JABODETABEK", lat: -6.213300, lon: 106.619200, alamat: "Jl. Imam Bonjol, Tangerang" },
        { kode: "SPZ", nama: "SERPONG (SPZ)", region: "JABODETABEK", lat: -6.242500, lon: 106.626700, alamat: "Ruko Fluorite Blok FR, Tangerang" },
        { kode: "SDZ", nama: "SIDOARJO (SDZ)", region: "JAWA", lat: -7.447800, lon: 112.718300, alamat: "Komplek Ruko Tiara Town Square, Sidoarjo" },
        { kode: "SLZ", nama: "SOLO (SLZ)", region: "JAWA", lat: -7.604200, lon: 110.816700, alamat: "New Mariposa Blok FH, Sukoharjo" },
        { kode: "SSZ", nama: "STABAT (SSZ)", region: "LUAR JAWA", lat: 3.733300, lon: 98.450000, alamat: "Jl. Proklamasi No. 11, Langkat" },
        { kode: "SKZ", nama: "SUKABUMI (SKZ)", region: "JAWA", lat: -6.921400, lon: 106.903300, alamat: "Jl. Raya Cibolang, Sukabumi" },
        { kode: "SMZ", nama: "SUMEDANG (SMZ)", region: "JAWA", lat: -6.858600, lon: 107.920800, alamat: "Jl. Swadaya, Sumedang" },
        { kode: "SBZ", nama: "SURABAYA (SBZ)", region: "JAWA", lat: -7.283123, lon: 112.765432, alamat: "Jl. Kertajaya Indah Timur, Surabaya" },
        { kode: "SUZ", nama: "SURABAYA 2 (SUZ)", region: "JAWA", lat: -7.295000, lon: 112.715000, alamat: "Ruko Villa Bukit Mas, Surabaya" },
        { kode: "TPZ", nama: "TAMAN PALEM (TPZ)", region: "JABODETABEK", lat: -6.137500, lon: 106.702500, alamat: "Citra Garden 7, Jakarta Barat" },
        { kode: "TGM", nama: "PASAR KEMIS (TGM)", region: "JABODETABEK", lat: -6.170800, lon: 106.561100, alamat: "Jl. Raya Pasar Kemis, Tangerang" },
        { kode: "TJG", nama: "TANJUNG (TJG)", region: "LUAR JAWA", lat: -2.175000, lon: 115.383300, alamat: "Jl. Ir. PHM Noor, Tabalong" },
        { kode: "TAR", nama: "TARAKAN (TAR)", region: "LUAR JAWA", lat: 3.315000, lon: 117.580000, alamat: "Jl. Bhayangkara, Tarakan" },
        { kode: "TEZ", nama: "TEGAL (TEZ)", region: "JAWA", lat: -6.874400, lon: 109.143000, alamat: "Ruko Palm Town House, Tegal" },
        { kode: "TGO", nama: "TENGGARONG (TGO)", region: "LUAR JAWA", lat: -0.435000, lon: 116.985000, alamat: "Jl. Pesut No. 22, Tenggarong" },
        { kode: "TMU", nama: "TUGU MULYO (TMU)", region: "LUAR JAWA", lat: -3.868000, lon: 104.888000, alamat: "Jl. Lintas Timur, Ogan Komering Ilir" },
        { kode: "WRZ", nama: "WARU (WRZ)", region: "JAWA", lat: -7.363000, lon: 112.726000, alamat: "Ruko Juanda Business Centre, Sidoarjo" },
        { kode: "YGZ", nama: "YOGYAKARTA (YGZ)", region: "JAWA", lat: -7.795580, lon: 110.369490 }
    ];

    const select = document.getElementById('cabangSelect');
    dbCabang.sort((a,b) => a.nama.localeCompare(b.nama)).forEach(c => {
        let opt = document.createElement('option');
        opt.value = c.kode; opt.text = c.nama; select.add(opt);
    });

    function resetCoverageUI() {
        document.getElementById('resultBox').style.display = 'none';
        document.getElementById('manualMode').style.display = 'none';
        let cab = dbCabang.find(c => c.kode === select.value);
        document.getElementById('cabangAddress').innerText = cab ? cab.alamat : "-";
    }

    async function prosesCoverageSmart() {
        let kode = select.value;
        let addr = document.getElementById('alamatInput').value.trim();
        if(!kode || !addr) { alert("Lengkapi data cabang dan alamat!"); return; }

        let btn = document.getElementById('btnProses');
        btn.disabled = true;
        btn.innerText = "⏳ Menganalisa...";
        document.getElementById('resultBox').style.display = 'none';
        document.getElementById('manualMode').style.display = 'none';

        try {
            let cab = dbCabang.find(c => c.kode === kode);
            let coordCab = { lat: cab.lat, lon: cab.lon };
            
            // SMART PARSING
            let query = smartParser(addr) || addr.replace(/RT.*?RW.*? /gi, ""); 
            let coordKons = await getCoordinates(query);
            
            if(!coordKons) throw new Error("Lokasi tidak ditemukan");

            let dist = haversine(coordCab.lat, coordCab.lon, coordKons.lat, coordKons.lon);
            tampilkanHasilCoverage(dist, cab.region);

        } catch (e) {
            document.getElementById('manualMode').style.display = 'block';
            let link = `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(addr)}`;
            document.getElementById('linkGmaps').href = link;
        } finally {
            btn.disabled = false;
            btn.innerHTML = "🚀 Hitung Jarak";
        }
    }

    function smartParser(text) {
        let kel = text.match(/(?:kel\.|kelurahan|desa)\s+([a-z0-9\s]+?)(?=\s+(?:kec|kab|kota|$))/i);
        let kec = text.match(/(?:kec\.|kecamatan)\s+([a-z0-9\s]+?)(?=\s+(?:kab|kota|$))/i);
        let kota = text.match(/(?:kab\/kota\.|kab\.|kabupaten|kota)\s+([a-z0-9\s]+?)(?=\s+(?:prov|$))/i);
        let parts = [];
        if(kel) parts.push(kel[1].trim());
        if(kec) parts.push(kec[1].trim());
        if(kota) parts.push(kota[1].trim());
        return parts.length >= 2 ? parts.join(", ") : null;
    }

    async function getCoordinates(addr) {
        try {
            let url = `https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(addr)}&limit=1`;
            let res = await fetch(url);
            let data = await res.json();
            return data.length > 0 ? { lat: parseFloat(data[0].lat), lon: parseFloat(data[0].lon) } : null;
        } catch(e) { return null; }
    }

    function hitungManual() {
        let input = document.getElementById('coordInput').value;
        let parts = input.split(",");
        if(parts.length !== 2) { alert("Format Salah!"); return; }
        
        let cab = dbCabang.find(c => c.kode === select.value);
        let dist = haversine(cab.lat, cab.lon, parseFloat(parts[0]), parseFloat(parts[1]));
        tampilkanHasilCoverage(dist, cab.region);
        document.getElementById('manualMode').style.display = 'none';
    }

    function haversine(lat1, lon1, lat2, lon2) {
        const R = 6371; 
        const dLat = (lat2 - lat1) * Math.PI / 180;
        const dLon = (lon2 - lon1) * Math.PI / 180;
        const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
                  Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * Math.sin(dLon/2) * Math.sin(dLon/2);
        return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
    }

    function tampilkanHasilCoverage(dist, region) {
        let limit = (region === "JABODETABEK") ? 30 : (region === "JAWA") ? 40 : 60;
        let isOk = dist <= limit;
        
        document.getElementById('txtJarak').innerText = dist.toFixed(2) + " Km";
        document.getElementById('txtLimit').innerText = `${limit} Km (${region})`;
        
        let badge = document.getElementById('badgeStatus');
        badge.className = isOk ? "badge bg-ok" : "badge bg-fail";
        badge.innerText = isOk ? "✅ APPROVED (MASUK AREA)" : "❌ REJECT (DILUAR JANGKAUAN)";
        
        document.getElementById('resultBox').style.display = 'block';
    }

    /* =========================================
       LOGIKA 2: KALKULATOR PADI
    ========================================= */
    const dbWilayah = {
        "Aceh Barat": 5482, "Bandung": 6433, "Bekasi": 4946, "Bogor": 5973, 
        "Cianjur": 5991, "Cilacap": 6123, "Cirebon": 6045, "Gresik": 6085, 
        "Karawang": 7200, "Subang": 6800, "Indramayu": 6573, "Sragen": 6400,
        "Ngawi": 6300, "Lamongan": 6200, "Banyuwangi": 5884
        // ... (Tambahkan data lengkap BPS di sini jika perlu)
    };

    const elmKota = document.getElementById('kota');
    Object.keys(dbWilayah).sort().forEach(k => {
        let opt = document.createElement('option');
        opt.value = dbWilayah[k]; opt.text = k; elmKota.add(opt);
    });

    function isiProduktivitas() {
        let val = document.getElementById('kota').value;
        if(val) document.getElementById('prod').value = val;
        hitungPadi();
    }

    function hitungPadi() {
        let prod = parseFloat(document.getElementById('prod').value) || 0;
        let luas = parseFloat(document.getElementById('luas').value) || 0;
        let harga = parseFloat(document.getElementById('harga').value) || 0;
        let siklus = parseFloat(document.getElementById('siklus').value) || 0;
        let margin = parseFloat(document.getElementById('margin').value) || 0;

        let omsetPerPanen = prod * luas * harga;
        let incomeTahunan = omsetPerPanen * siklus * (margin / 100);
        let incomeBulan = incomeTahunan / 12;

        document.getElementById('txtOmset').innerText = "Rp " + Math.round(omsetPerPanen).toLocaleString('id-ID');
        document.getElementById('txtIncomeBulan').innerText = "Rp " + Math.round(incomeBulan).toLocaleString('id-ID');
    }
</script>

</body>
</html>
