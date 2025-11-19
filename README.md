<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Voltage Stability Limit?", "id": "Batas Daya Sebelum Tegangan Runtuh." },
  { "en": "Apa Itu Power System Stabilizer?", "id": "Alat Peredam Osilasi Frekuensi Rendah." },
  { "en": "Apa Itu Automatic Generation Control?", "id": "Pengatur Beban Generator Secara Terpusat." },
  { "en": "Apa Itu Spinning Reserve Requirement?", "id": "Cadangan Daya Wajib Untuk Keandalan." },
  { "en": "Apa Itu Frequency Response Grid?", "id": "Respon Sistem Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Synthetic Inertia?", "id": "Inersia Buatan Dari Inverter Elektronik." },
  { "en": "Apa Itu Under Voltage Load Shedding?", "id": "Melepas Beban Saat Tegangan Runtuh." },
  { "en": "Apa Itu Power Frequency Withstand Voltage?", "id": "Tahan Tegangan Frekuensi Kerja Semenit." },
  { "en": "Apa Itu Impulse Withstand Voltage?", "id": "Tahan Tegangan Surja Petir." },
  { "en": "Apa Itu Creepage Distance Ratio?", "id": "Jarak Rambat Per Kilovolt Tegangan." },
  { "en": "Apa Itu Unified Power Quality Conditioner? (UPQC)", "id": "Kompensasi Kualitas Daya Seri Paralel." },
  { "en": "Apa Itu Dynamic Voltage Restorer? (DVR)", "id": "Inverter Seri Pemulih Tegangan Sag." },
  { "en": "Apa Itu Solid State Transformer? (SST)", "id": "Trafo Berbasis Konverter Elektronika Daya." },
  { "en": "Apa Itu Wireless Power Transfer?", "id": "Transfer Daya Tanpa Kontak Fisik." },
  { "en": "Apa Itu Resonant Inductive Coupling?", "id": "Prinsip Transfer Daya Nirkabel." },
  { "en": "Apa Itu Skin Effect Factor?", "id": "Rasio Tahanan AC Terhadap DC." },
  { "en": "Apa Itu Bundled Conductor Spacer?", "id": "Penjaga Jarak Antar Kabel Berkas." },
  { "en": "Apa Itu Vibration Damper Placement?", "id": "Posisi Pemasangan Peredam Getaran." },
  { "en": "Apa Itu Counterpoise Wire?", "id": "Kawat Tanah Paralel Jalur Transmisi." },
  { "en": "Apa Itu Shield Wire Angle?", "id": "Sudut Lindung Kawat Tanah." },
  { "en": "Apa Itu Back Flashover Mechanism?", "id": "Loncatan Api Dari Menara Ke Fasa." },
  { "en": "Apa Itu Isokeraunic Level?", "id": "Tingkat Aktivitas Petir Suatu Daerah." },
  { "en": "Apa Itu Insulation Coordination Study?", "id": "Studi Penentuan Level Isolasi Peralatan." },
  { "en": "Apa Itu Lightning Arrestor Class?", "id": "Klasifikasi Kemampuan Energi Arrester." },
  { "en": "Apa Itu Station Class Arrester?", "id": "Arrester Kapasitas Energi Paling Tinggi." },
  { "en": "Apa Itu Distribution Class Arrester?", "id": "Arrester Kapasitas Energi Menengah." },
  { "en": "Apa Itu Secondary Class Arrester?", "id": "Arrester Tegangan Rendah Pelanggan." },
  { "en": "Apa Itu Polymer Housed Arrester?", "id": "Arrester Dengan Selubung Karet Silikon." },
  { "en": "Apa Itu Porcelain Housed Arrester?", "id": "Arrester Dengan Selubung Keramik." },
  { "en": "Apa Itu Pressure Relief Vent?", "id": "Katup Pelepasan Tekanan Gas Arrester." },
  { "en": "Apa Itu Leakage Current Monitor?", "id": "Alat Pantau Kondisi Arrester." },
  { "en": "Apa Itu Third Harmonic Leakage?", "id": "Indikator Penuaan Blok Metal Oxide." },
  { "en": "Apa Itu Wattmetric Method?", "id": "Metode Ukur Rugi Resistif Arrester." },
  { "en": "Apa Itu Field Testing Arrester?", "id": "Pengujian Arrester Di Lapangan." },
  { "en": "Apa Itu Relay Diferensial Digital?", "id": "Relay Diferensial Berbasis Mikroprosesor." },
  { "en": "Apa Itu Percentage Bias Differential?", "id": "Setting Kemiringan Kurva Relay Diferensial." },
  { "en": "Apa Itu Harmonic Restraint Logic?", "id": "Logika Penahan Arus Inrush." },
  { "en": "Apa Itu High Impedance Differential?", "id": "Diferensial Stabil Terhadap Saturasi CT." },
  { "en": "Apa Itu Low Impedance Differential?", "id": "Diferensial Menggunakan Algoritma Perhitungan." },
  { "en": "Apa Itu Busbar Check Zone?", "id": "Zona Verifikasi Gangguan Busbar." },
  { "en": "Apa Itu CT Supervision Relay?", "id": "Mendeteksi Kabel CT Putus." },
  { "en": "Apa Itu End Fault Protection?", "id": "Proteksi Gangguan Antara CT Breaker." },
  { "en": "Apa Itu Blind Spot Protection?", "id": "Proteksi Area Tak Tercover CT." },
  { "en": "Apa Itu Reverse Interlocking?", "id": "Koordinasi Cepat Proteksi Busbar." },
  { "en": "Apa Itu Zone Selective Interlocking?", "id": "Koordinasi Logika Zona Proteksi." },
  { "en": "Apa Itu Arc Flash Relay?", "id": "Relay Deteksi Cahaya Busur Api." },
  { "en": "Apa Itu Point Sensor Arc Flash?", "id": "Sensor Cahaya Titik Tunggal." },
  { "en": "Apa Itu Fiber Loop Sensor?", "id": "Sensor Cahaya Serat Optik Panjang." },
  { "en": "Apa Itu Light And Current Logic?", "id": "Trip Jika Ada Cahaya Dan Arus." },
  { "en": "Apa Itu Current Transformer Saturation?", "id": "Kondisi Inti CT Jenuh Magnetik." },
  { "en": "Apa Itu Remanence Flux CT?", "id": "Sisa Magnetisme Pada Inti CT." },
  { "en": "Apa Itu Transient Performance CT?", "id": "Respon CT Terhadap Komponen DC." },
  { "en": "Apa Itu Accuracy Limit Factor?", "id": "Batas Akurasi CT Proteksi." },
  { "en": "Apa Itu Rated Burden CT?", "id": "Beban Nominal Impedansi Sekunder." },
  { "en": "Apa Itu Composite Error CT?", "id": "Total Kesalahan Arus Dan Fasa." },
  { "en": "Apa Itu Phase Displacement CT?", "id": "Pergeseran Sudut Arus Primer Sekunder." },
  { "en": "Apa Itu Excitation Curve CT?", "id": "Kurva Tegangan Lawan Arus Eksitasi." },
  { "en": "Apa Itu Class X CT?", "id": "Kelas CT Untuk Proteksi Diferensial." },
  { "en": "Apa Itu Class PX CT?", "id": "Standar IEC Untuk CT Proteksi." },
  { "en": "Apa Itu Class TPS CT?", "id": "CT Saturasi Rendah Fluks Remanen." },
  { "en": "Apa Itu Rogowski Coil Sensor?", "id": "Sensor Arus Tanpa Inti Besi." },
  { "en": "Apa Itu Ferroresonance Damping?", "id": "Peredaman Osilasi Pada VT." },
  { "en": "Apa Itu Open Delta Connection?", "id": "Hubungan VT Deteksi Tegangan Nol." },
  { "en": "Apa Itu Broken Delta Connection?", "id": "Nama Lain Open Delta VT." },
  { "en": "Apa Itu Residual Voltage VT?", "id": "Tegangan Urutan Nol Pada VT." },
  { "en": "Apa Itu Fuse Failure Relay?", "id": "Deteksi Sekring VT Putus." },
  { "en": "Apa Itu VT Supervision?", "id": "Pengawasan Integritas Rangkaian VT." },
  { "en": "Apa Itu Breaking Capacity MCB?", "id": "Kapasitas Pemutusan Arus Hubung Singkat." },
  { "en": "Apa Itu Service Breaking Capacity? (Ics)", "id": "Kapasitas Pemutusan Bisa Pakai Lagi." },
  { "en": "Apa Itu Ultimate Breaking Capacity? (Icu)", "id": "Kapasitas Pemutusan Maksimum Sekali Pakai." },
  { "en": "Apa Itu Short Time Withstand? (Icw)", "id": "Ketahanan Arus Sesaat Tanpa Trip." },
  { "en": "Apa Itu Making Capacity Breaker?", "id": "Kemampuan Menutup Pada Arus Gangguan." },
  { "en": "Apa Itu Thermal Magnetic Trip?", "id": "Mekanisme Trip Panas Dan Magnetik." },
  { "en": "Apa Itu Electronic Trip Unit?", "id": "Unit Trip Berbasis Mikroprosesor." },
  { "en": "Apa Itu Long Time Delay? (LTD)", "id": "Setting Waktu Trip Beban Lebih." },
  { "en": "Apa Itu Short Time Delay? (STD)", "id": "Setting Waktu Trip Hubung Singkat." },
  { "en": "Apa Itu Instantaneous Trip?", "id": "Trip Seketika Tanpa Tunda Waktu." },
  { "en": "Apa Itu Ground Fault Trip?", "id": "Trip Akibat Gangguan Ke Tanah." },
  { "en": "Apa Itu Shunt Trip Coil?", "id": "Kumparan Pembuka Breaker Jarak Jauh." },
  { "en": "Apa Itu Under Voltage Release?", "id": "Membuka Breaker Saat Tegangan Hilang." },
  { "en": "Apa Itu Motor Operator Breaker?", "id": "Motor Pengisi Pegas Breaker Otomatis." },
  { "en": "Apa Itu Racking Mechanism?", "id": "Mekanisme Keluar Masuk Breaker Drawout." },
  { "en": "Apa Itu Safety Shutter Panel?", "id": "Penutup Otomatis Terminal Tegangan Tinggi." },
  { "en": "Apa Itu Arc Resistant Switchgear?", "id": "Panel Tahan Ledakan Busur Api." },
  { "en": "Apa Itu Internal Arc Classification?", "id": "Rating Ketahanan Panel Terhadap Ledakan." },
  { "en": "Apa Itu Pressure Relief Flap?", "id": "Katup Buang Tekanan Ledakan Panel." },
  { "en": "Apa Itu Arc Duct?", "id": "Saluran Buang Gas Panas Ledakan." },
  { "en": "Apa Itu Maintenance Earthing Switch?", "id": "Saklar Tanah Untuk Aman Kerja." },
  { "en": "Apa Itu High Speed Earthing Switch?", "id": "Saklar Tanah Mampu Tutup Gangguan." },
  { "en": "Apa Itu Make Proof Switch?", "id": "Saklar Tahan Menutup Arus Gangguan." },
  { "en": "Apa Itu Primary Injection Test?", "id": "Uji Arus Tinggi Sisi Primer." },
  { "en": "Apa Itu Secondary Injection Test?", "id": "Uji Arus Rendah Sisi Relay." },
  { "en": "Apa Itu Relay Logic Test?", "id": "Menguji Fungsi Logika Pemrograman Relay." },
  { "en": "Apa Itu Trip Timing Test?", "id": "Mengukur Kecepatan Waktu Trip Relay." },
  { "en": "Apa Itu CT Polarity Check?", "id": "Memastikan Arah Arus CT Benar." },
  { "en": "Apa Itu CT Ratio Test?", "id": "Mengukur Perbandingan Arus Primer Sekunder." },
  { "en": "Apa Itu CT Kneepoint Test?", "id": "Mencari Titik Jenuh Kurva CT." },
  { "en": "Apa Itu Contact Resistance Test?", "id": "Mengukur Tahanan Kontak Pemutus Tenaga." },
  { "en": "Apa Itu Circuit Breaker Timing Test?", "id": "Mengukur Waktu Buka Tutup Kontak." },
  { "en": "Apa Itu Dielectric Absorption?", "id": "Penyerapan Muatan Oleh Bahan Isolator." },
  { "en": "Apa Itu Polarization Current?", "id": "Arus Penyearahan Dipol Dalam Isolator." },
  { "en": "Apa Itu Surface Tracking?", "id": "Jalur Konduktif Karbon Permukaan Isolator." },
  { "en": "Apa Itu Erosion Isolator?", "id": "Pengikisan Material Akibat Busur Api." },
  { "en": "Apa Itu Hydrophobicity Class?", "id": "Kelas Kemampuan Menolak Air Isolator." },
  { "en": "Apa Itu Local Mode Oscillation?", "id": "Osilasi Generator Tunggal Terhadap Sistem." },
  { "en": "Apa Itu Inter-Area Mode Oscillation?", "id": "Osilasi Antar Kelompok Generator Terpisah." },
  { "en": "Apa Itu Intra-Plant Mode?", "id": "Osilasi Antar Unit Dalam Pembangkit." },
  { "en": "Apa Itu Damping Ratio?", "id": "Ukuran Kecepatan Peluruhan Osilasi." },
  { "en": "Apa Itu Negative Damping?", "id": "Osilasi Yang Semakin Membesar." },
  { "en": "Apa Itu Circuit Breaker Analyzer?", "id": "Alat Uji Waktu Dan Gerak Breaker." },
  { "en": "Apa Itu Motion Travel Curve?", "id": "Grafik Pergerakan Kontak Terhadap Waktu." },
  { "en": "Apa Itu Damping Pot Breaker?", "id": "Peredam Kejut Mekanis Saat Buka." },
  { "en": "Apa Itu Latch Mechanism?", "id": "Mekanisme Pengunci Posisi Breaker." },
  { "en": "Apa Itu Trip Free Mechanism?", "id": "Breaker Bisa Trip Meski Sedang Closing." },
  { "en": "Apa Itu Peukert Exponent?", "id": "Konstanta Perhitungan Kapasitas Baterai Beban." },
  { "en": "Apa Itu Battery Internal Resistance?", "id": "Hambatan Dalam Menurunkan Tegangan Baterai." },
  { "en": "Apa Itu Inter-Cell Connector?", "id": "Penghubung Antar Sel Baterai Seri." },
  { "en": "Apa Itu Battery Rack?", "id": "Rak Penyangga Bank Baterai." },
  { "en": "Apa Itu Spill Containment Baterai?", "id": "Bak Penampung Tumpahan Asam Baterai." },
  { "en": "Apa Itu Sheath Circulating Current?", "id": "Arus Induksi Pada Pelindung Kabel." },
  { "en": "Apa Itu Cross Bonding Box?", "id": "Kotak Sambungan Silang Sheath Kabel." },
  { "en": "Apa Itu Link Box Arrester?", "id": "Pelindung Tegangan Lebih Sheath Kabel." },
  { "en": "Apa Itu Transposition Joint?", "id": "Sambungan Pertukaran Posisi Fasa Kabel." },
  { "en": "Apa Itu Oil Feeding Tank?", "id": "Tangki Suplai Minyak Kabel OF." },
  { "en": "Apa Itu Hydrogen Purity Meter?", "id": "Alat Ukur Kemurnian Gas Pendingin." },
  { "en": "Apa Itu Seal Oil System?", "id": "Mencegah Hidrogen Bocor Lewat Poros." },
  { "en": "Apa Itu Stator Water Cooling?", "id": "Pendinginan Air Dalam Batang Stator." },
  { "en": "Apa Itu Hollow Conductor?", "id": "Konduktor Berlubang Aliran Air Pendingin." },
  { "en": "Apa Itu End Winding Vibration?", "id": "Getaran Pada Ujung Lilitan Stator." },
  { "en": "Apa Itu CT Saturation Detector?", "id": "Algoritma Deteksi Jenuh Inti CT." },
  { "en": "Apa Itu Adaptive Restraint?", "id": "Bias Diferensial Berubah Sesuai Kondisi." },
  { "en": "Apa Itu Phase Comparison Protection?", "id": "Membandingkan Sudut Fasa Arus Ujung." },
  { "en": "Apa Itu Alpha Plane Protection?", "id": "Metode Diferensial Berbasis Rasio Fasor." },
  { "en": "Apa Itu Travelling Wave Protection?", "id": "Proteksi Deteksi Gelombang Berjalan Cepat." },
  { "en": "Apa Itu Solar Tracker?", "id": "Mekanisme Panel Mengikuti Gerak Matahari." },
  { "en": "Apa Itu Single Axis Tracker?", "id": "Pelacak Matahari Satu Sumbu Putar." },
  { "en": "Apa Itu Dual Axis Tracker?", "id": "Pelacak Matahari Dua Sumbu Putar." },
  { "en": "Apa Itu Backtracking Algorithm?", "id": "Mencegah Bayangan Antar Baris Panel." },
  { "en": "Apa Itu Wind Turbine Brake?", "id": "Rem Mekanis Penahan Putaran Turbin." },
  { "en": "Apa Itu Transformer Oil Sampling?", "id": "Pengambilan Sampel Minyak Uji Lab." },
  { "en": "Apa Itu Syringe Sampling?", "id": "Sampling Minyak Menggunakan Suntikan Kaca." },
  { "en": "Apa Itu Online DGA Monitor?", "id": "Alat Pantau Gas Terlarut Real-Time." },
  { "en": "Apa Itu Moisture in Oil Sensor?", "id": "Sensor Kadar Air Minyak Online." },
  { "en": "Apa Itu Paper Aging Marker?", "id": "Indikator Kimia Penuaan Kertas Isolasi." },
  { "en": "Apa Itu Partial Discharge Locator?", "id": "Alat Penentu Lokasi Sumber PD." },
  { "en": "Apa Itu Time of Flight Method?", "id": "Metode Lokasi PD Berbasis Waktu." },
  { "en": "Apa Itu Acoustic Triangulation?", "id": "Menentukan Lokasi PD Dengan Suara." },
  { "en": "Apa Itu Electromagnetic Scanning?", "id": "Memindai Radiasi Gelombang Dari PD." },
  { "en": "Apa Itu RIV Test?", "id": "Uji Tegangan Gangguan Radio Isolator." },
  { "en": "Apa Itu Salt Fog Test?", "id": "Uji Kabut Garam Pada Isolator." },
  { "en": "Apa Itu Steep Front Wave?", "id": "Gelombang Impuls Muka Sangat Curam." },
  { "en": "Apa Itu Standard Lightning Impulse?", "id": "Gelombang Petir Standar 1.2/50 Mikrodetik." },
  { "en": "Apa Itu Standard Switching Impulse?", "id": "Gelombang Hubung Standar 250/2500 Mikrodetik." },
  { "en": "Apa Itu Chopped Impulse?", "id": "Gelombang Impuls Dipotong Tiba-Tiba." },
  { "en": "Apa Itu Full Wave Impulse?", "id": "Gelombang Impuls Penuh Tanpa Potongan." },
  { "en": "Apa Itu Impulse Generator Stage?", "id": "Tingkatan Kapasitor Pembangkit Impuls." },
  { "en": "Apa Itu Trigger Spark Gap?", "id": "Sela Bola Pemicu Pelepasan Impuls." },
  { "en": "Apa Itu Charging Resistor?", "id": "Resistor Pengisi Kapasitor Generator Impuls." },
  { "en": "Apa Itu Wavefront Resistor?", "id": "Resistor Pengatur Waktu Muka Gelombang." },
  { "en": "Apa Itu Wavetail Resistor?", "id": "Resistor Pengatur Waktu Ekor Gelombang." },
  { "en": "Apa Itu Voltage Divider?", "id": "Pembagi Tegangan Untuk Pengukuran HV." },
  { "en": "Apa Itu Resistive Divider?", "id": "Pembagi Tegangan Menggunakan Resistor." },
  { "en": "Apa Itu Capacitive Divider?", "id": "Pembagi Tegangan Menggunakan Kapasitor." },
  { "en": "Apa Itu Rogowski Coil Integrator?", "id": "Pengolah Sinyal Output Rogowski Coil." },
  { "en": "Apa Itu Hall Effect Transducer?", "id": "Transduser Arus DC Dan AC." },
  { "en": "Apa Itu Optical Voltage Transducer?", "id": "Transduser Tegangan Berbasis Serat Optik." },
  { "en": "Apa Itu Ferroelectric Effect?", "id": "Polarisasi Spontan Material Kristal." },
  { "en": "Apa Itu Piezoelectric Effect?", "id": "Listrik Dari Tekanan Mekanis Kristal." },
  { "en": "Apa Itu Pyroelectric Effect?", "id": "Listrik Dari Perubahan Suhu Kristal." },
  { "en": "Apa Itu Triboelectric Effect?", "id": "Listrik Statis Akibat Gesekan Benda." },
  { "en": "Apa Itu Thermoelectric Generator? (TEG)", "id": "Pembangkit Listrik Beda Suhu Pasif." },
  { "en": "Apa Itu Radioisotope Generator? (RTG)", "id": "Pembangkit Nuklir Mini Satelit." },
  { "en": "Apa Itu MHD Generator?", "id": "Magneto Hydro Dynamic Generator." },
  { "en": "Apa Prinsip Generator MHD?", "id": "Fluida Konduktif Lewat Medan Magnet." },
  { "en": "Apa Itu Superconducting Generator?", "id": "Generator Dengan Lilitan Superkonduktor." },
  { "en": "Apa Itu Cryogenic Cooling?", "id": "Pendinginan Suhu Sangat Rendah." },
  { "en": "Apa Itu Liquid Nitrogen Cooling?", "id": "Pendingin Kabel HTS Suhu 77K." },
  { "en": "Apa Itu Liquid Helium Cooling?", "id": "Pendingin Superkonduktor Suhu 4K." },
  { "en": "Apa Itu Quench Superkonduktor?", "id": "Hilangnya Sifat Superkonduktor Tiba-Tiba." },
  { "en": "Apa Itu Critical Current Density?", "id": "Batas Rapat Arus Superkonduktor." },
  { "en": "Apa Itu Critical Magnetic Field?", "id": "Batas Medan Magnet Superkonduktor." },
  { "en": "Apa Itu Critical Temperature Tc?", "id": "Suhu Transisi Fase Superkonduktor." },
  { "en": "Apa Itu Meissner Effect?", "id": "Penolakan Medan Magnet Oleh Superkonduktor." },
  { "en": "Apa Itu Flux Pinning?", "id": "Penguncian Garis Fluks Superkonduktor." },
  { "en": "Apa Itu Maglev Train?", "id": "Kereta Levitasi Magnetik Supercepat." },
  { "en": "Apa Itu Linear Induction Motor?", "id": "Motor Induksi Gerakan Lurus." },
  { "en": "Apa Itu Linear Synchronous Motor?", "id": "Motor Sinkron Gerakan Lurus." },
  { "en": "Apa Itu Traction Inverter?", "id": "Inverter Penggerak Motor Kendaraan Listrik." },
  { "en": "Apa Itu Battery Management System?", "id": "Sistem Pengaman Dan Penyeimbang Baterai." },
  { "en": "Apa Itu Active Cell Balancing?", "id": "Transfer Energi Antar Sel Baterai." },
  { "en": "Apa Itu Passive Cell Balancing?", "id": "Membuang Energi Lebih Ke Resistor." },
  { "en": "Apa Itu Thermal Management System?", "id": "Sistem Pendingin Baterai Mobil Listrik." },
  { "en": "Apa Itu Liquid Cooling Battery?", "id": "Pendingin Cair Plat Baterai EV." },
  { "en": "Apa Itu Air Cooling Battery?", "id": "Pendingin Udara Kipas Baterai EV." },
  { "en": "Apa Itu Immersion Cooling?", "id": "Merendam Baterai Dalam Cairan Dielektrik." },
  { "en": "Apa Itu Wireless Charging EV?", "id": "Pengisian Mobil Listrik Tanpa Kabel." },
  { "en": "Apa Itu Dynamic Wireless Charging?", "id": "Pengisian Mobil Saat Bergerak." },
  { "en": "Apa Itu Koordinasi Relay Arus Lebih?", "id": "Pengaturan Waktu Trip Relay Berjenjang." },
  { "en": "Apa Itu Time Dial Setting? (TDS)", "id": "Pengaturan Pengali Waktu Tunda Relay." },
  { "en": "Apa Itu Setelan Instantaneous Relay?", "id": "Batas Arus Trip Tanpa Tunda." },
  { "en": "Apa Itu Kurva Definite Time?", "id": "Waktu Trip Tetap Berapapun Arusnya." },
  { "en": "Apa Itu Kurva Standard Inverse?", "id": "Waktu Trip Berbanding Terbalik Arus." },
  { "en": "Apa Itu Kurva Very Inverse?", "id": "Waktu Trip Lebih Cepat Dari Standar." },
  { "en": "Apa Itu Kurva Extremely Inverse?", "id": "Waktu Trip Sangat Cepat Arus Tinggi." },
  { "en": "Apa Itu Kurva Long Time Inverse?", "id": "Waktu Tunda Trip Sangat Panjang." },
  { "en": "Apa Itu Reset Time Relay?", "id": "Waktu Relay Kembali Ke Posisi Awal." },
  { "en": "Apa Itu Overshoot Time Relay?", "id": "Waktu Operasi Tambahan Inersia Relay." },
  { "en": "Apa Itu Pengukuran Burden Relay?", "id": "Mengukur Beban Impedansi Rangkaian Relay." },
  { "en": "Apa Itu Tegangan Saturasi CT?", "id": "Tegangan Saat Inti CT Mulai Jenuh." },
  { "en": "Apa Itu Arus Eksitasi CT?", "id": "Arus Magnetisasi Inti Trafo Arus." },
  { "en": "Apa Itu Tanda Polaritas CT?", "id": "Penanda Arah Arus Primer Sekunder." },
  { "en": "Apa Itu Terminal P1 Dan P2?", "id": "Terminal Sisi Primer Trafo Arus." },
  { "en": "Apa Itu Terminal S1 Dan S2?", "id": "Terminal Sisi Sekunder Trafo Arus." },
  { "en": "Apa Itu Titik Bintang CT?", "id": "Titik Hubung Netral Rangkaian CT." },
  { "en": "Apa Itu Hubungan Delta CT?", "id": "Koneksi Segitiga Rangkaian Sekunder CT." },
  { "en": "Apa Itu Filter Urutan Nol CT?", "id": "Rangkaian CT Menjumlahkan Arus Tanah." },
  { "en": "Apa Itu Summation CT?", "id": "CT Menjumlahkan Arus Beberapa Sirkuit." },
  { "en": "Apa Itu Interposing CT?", "id": "Trafo Arus Bantu Penyesuai Rasio." },
  { "en": "Apa Itu Core Balance CT? (CBCT)", "id": "CT Melingkari Ketiga Fasa Kabel." },
  { "en": "Apa Fungsi Utama CBCT?", "id": "Mendeteksi Arus Bocor Tanah Sensitif." },
  { "en": "Apa Itu Kerapatan Fluks Jenuh?", "id": "Batas Maksimum Garis Gaya Magnet." },
  { "en": "Apa Itu Loop Histeresis Inti?", "id": "Kurva Siklus Magnetisasi Bahan Inti." },
  { "en": "Apa Itu Rugi Arus Eddy Inti?", "id": "Panas Akibat Arus Pusar Besi." },
  { "en": "Apa Itu Inti Laminasi?", "id": "Inti Berlapis Tipis Kurangi Eddy." },
  { "en": "Apa Itu Baja Grain Oriented? (CRGO)", "id": "Baja Silikon Arah Butiran Teratur." },
  { "en": "Apa Keuntungan Baja CRGO?", "id": "Permeabilitas Tinggi Pada Arah Gilingan." },
  { "en": "Apa Itu Baja Non-Oriented?", "id": "Baja Silikon Sifat Magnet Segala Arah." },
  { "en": "Apa Itu Stacking Factor Inti?", "id": "Rasio Volume Besi Efektif Total." },
  { "en": "Apa Itu Sambungan Butt Joint?", "id": "Sambungan Inti Trafo Tegak Lurus." },
  { "en": "Apa Itu Sambungan Mitred Joint?", "id": "Sambungan Inti Miring 45 Derajat." },
  { "en": "Apa Itu Step Lap Joint?", "id": "Sambungan Bertingkat Mengurangi Kebisingan Trafo." },
  { "en": "Apa Itu Window Area Trafo?", "id": "Lubang Inti Besi Tempat Lilitan." },
  { "en": "Apa Itu Space Factor Lilitan?", "id": "Rasio Luas Tembaga Terhadap Jendela." },
  { "en": "Apa Itu Lilitan Silinder?", "id": "Lilitan Bentuk Tabung Sederhana." },
  { "en": "Apa Itu Lilitan Piringan? (Disc)", "id": "Lilitan Bentuk Cakram Bertumpuk." },
  { "en": "Apa Itu Lilitan Helical?", "id": "Lilitan Spiral Untuk Arus Besar." },
  { "en": "Apa Itu Lilitan Foil?", "id": "Lilitan Menggunakan Lembaran Tembaga Lebar." },
  { "en": "Apa Itu Transposisi Lilitan?", "id": "Menukar Posisi Kawat Dalam Lilitan." },
  { "en": "Apa Tujuan Transposisi Lilitan?", "id": "Meratakan Arus Dan Mengurangi Rugi." },
  { "en": "Apa Itu Kabel CTC?", "id": "Kabel Transposisi Kontinu Banyak Kawat." },
  { "en": "Apa Kepanjangan CTC? (Continuously Transposed Cable)", "id": "Kabel Transposisi Terus Menerus." },
  { "en": "Apa Itu Kertas Kraft?", "id": "Kertas Isolasi Standar Trafo Minyak." },
  { "en": "Apa Itu Kertas Thermally Upgraded?", "id": "Kertas Isolasi Tahan Suhu Tinggi." },
  { "en": "Apa Itu Spacer Pressboard?", "id": "Penyekat Antar Lilitan Aliran Minyak." },
  { "en": "Apa Itu Saluran Minyak Lilitan?", "id": "Celah Pendingin Dalam Gulungan Trafo." },
  { "en": "Apa Itu Cincin Klem? (Clamping Ring)", "id": "Cincin Penekan Struktur Lilitan Trafo." },
  { "en": "Apa Itu Tie Rod Trafo?", "id": "Batang Besi Pengikat Inti Trafo." },
  { "en": "Apa Itu Core Grounding Strap?", "id": "Pita Tembaga Menanahkan Inti Besi." },
  { "en": "Apa Bahaya Core Floating?", "id": "Timbul Tegangan Statis Dan Arcing." },
  { "en": "Apa Itu Shielding Tangki Trafo?", "id": "Pelindung Magnetik Dinding Tangki." },
  { "en": "Apa Fungsi Shielding Tangki?", "id": "Mengurangi Panas Akibat Fluks Liar." },
  { "en": "Apa Itu Bladder Konservator?", "id": "Kantong Karet Dalam Tangki Konservator." },
  { "en": "Apa Itu Air Cell Konservator?", "id": "Nama Lain Dari Bladder Konservator." },
  { "en": "Apa Itu Magnetic Oil Gauge? (MOG)", "id": "Penunjuk Level Minyak Sistem Magnetik." },
  { "en": "Apa Itu Dehydrating Breather?", "id": "Pernapasan Trafo Dengan Penyerap Lembab." },
  { "en": "Apa Itu Vacuum Drying Trafo?", "id": "Pengeringan Isolasi Dalam Ruang Hampa." },
  { "en": "Apa Itu Vapor Phase Drying?", "id": "Pengeringan Menggunakan Uap Minyak Tanah." },
  { "en": "Apa Itu Vacuum Filling?", "id": "Pengisian Minyak Trafo Kondisi Vakum." },
  { "en": "Apa Itu Voltage Ratio Error?", "id": "Persentase Kesalahan Rasio Tegangan Trafo." },
  { "en": "Apa Itu Phase Displacement Check?", "id": "Memeriksa Geseran Sudut Fasa Trafo." },
  { "en": "Apa Itu No Load Loss Test?", "id": "Uji Rugi Inti Besi Trafo." },
  { "en": "Apa Itu Load Loss Test?", "id": "Uji Rugi Tembaga Beban Penuh." },
  { "en": "Apa Itu Zero Sequence Test?", "id": "Uji Impedansi Urutan Nol Trafo." },
  { "en": "Apa Itu Double Voltage Frequency? (DVDF)", "id": "Uji Tahan Tegangan Induksi Trafo." },
  { "en": "Apa Itu Separate Source Test?", "id": "Uji Tahan Tegangan Sumber Terpisah." },
  { "en": "Apa Itu Switching Surge Test?", "id": "Uji Ketahanan Surja Hubung Trafo." },
  { "en": "Apa Itu Leak Pressure Test?", "id": "Uji Kebocoran Tangki Tekanan Positif." },
  { "en": "Apa Itu Polarity Test Trafo?", "id": "Menentukan Arah Lilitan Kutub Trafo." },
  { "en": "Apa Itu RIV Test Trafo?", "id": "Uji Interferensi Radio Tegangan Tinggi." },
  { "en": "Apa Itu Frequency Domain Spectroscopy? (FDS)", "id": "Uji Diagnostik Isolasi Domain Frekuensi." },
  { "en": "Apa Itu Particle Count Test?", "id": "Menghitung Partikel Kontaminan Dalam Minyak." },
  { "en": "Apa Itu Relative Saturation Minyak?", "id": "Persentase Kejenuhan Air Dalam Minyak." },
  { "en": "Apa Itu Oxidation Stability Test?", "id": "Uji Ketahanan Minyak Terhadap Oksidasi." },
  { "en": "Apa Itu Sludge Content Test?", "id": "Uji Kandungan Lumpur Dalam Minyak." },
  { "en": "Apa Itu Copper Sulfide Deposit?", "id": "Endapan Korosi Tembaga Pada Isolasi." },
  { "en": "Apa Itu Inhibited Oil?", "id": "Minyak Trafo Dengan Zat Anti-Oksidan." },
  { "en": "Apa Itu Stray Gassing Minyak?", "id": "Gas Muncul Bukan Karena Gangguan." },
  { "en": "Apa Itu Fault Gas?", "id": "Gas Terlarut Indikasi Gangguan Internal." },
  { "en": "Apa Itu Hydrogen Gas Trafo?", "id": "Indikasi Partial Discharge Atau Arcing." },
  { "en": "Apa Itu Acetylene Gas Trafo?", "id": "Indikasi Arcing Energi Tinggi." },
  { "en": "Apa Itu Ethylene Gas Trafo?", "id": "Indikasi Pemanasan Berlebih Minyak Trafo." },
  { "en": "Apa Itu Carbon Monoxide Trafo?", "id": "Indikasi Pemanasan Berlebih Kertas Isolasi." },
  { "en": "Apa Itu Duval Triangle?", "id": "Metode Grafis Analisis Gas DGA." },
  { "en": "Apa Itu Rogers Ratio?", "id": "Metode Rasio Analisis Gas DGA." },
  { "en": "Apa Itu Key Gas Method?", "id": "Analisis Gas Berdasarkan Komponen Dominan." },
  { "en": "Apa Itu Moisture Equilibrium?", "id": "Keseimbangan Kadar Air Minyak Kertas." },
  { "en": "Apa Itu Transformer Life Expectancy?", "id": "Perkiraan Sisa Umur Operasi Trafo." },
  { "en": "Apa Itu Hot Spot Temperature?", "id": "Suhu Tertinggi Di Dalam Lilitan." },
  { "en": "Apa Itu Aging Factor Trafo?", "id": "Faktor Percepatan Penuaan Isolasi Trafo." },
  { "en": "Apa Itu Loss of Life?", "id": "Berkurangnya Umur Trafo Akibat Beban." },
  { "en": "Apa Itu Overload Capability?", "id": "Kemampuan Beban Lebih Waktu Singkat." },
  { "en": "Apa Itu Cyclic Loading?", "id": "Pembebanan Siklus Naik Turun Harian." },
  { "en": "Apa Itu Transformer Cooling Class?", "id": "Kode Metode Pendinginan (ONAN, ONAF)." },
  { "en": "Apa Itu Oil Natural Air Natural? (ONAN)", "id": "Pendinginan Alami Minyak Dan Udara." },
  { "en": "Apa Itu Oil Natural Air Forced? (ONAF)", "id": "Pendinginan Kipas Angin Pada Radiator." },
  { "en": "Apa Itu Oil Forced Air Forced? (OFAF)", "id": "Pendinginan Pompa Minyak Kipas Angin." },
  { "en": "Apa Itu Oil Directed Air Forced? (ODAF)", "id": "Minyak Diarahkan Belitan Kipas Angin." },
  { "en": "Apa Itu Dry Type Transformer?", "id": "Trafo Tanpa Minyak Pendingin Udara." },
  { "en": "Apa Itu Cast Resin Transformer?", "id": "Kumparan Dicor Resin Epoksi Keras." },
  { "en": "Apa Keuntungan Trafo Cast Resin?", "id": "Tahan Api Dan Bebas Perawatan." },
  { "en": "Apa Itu Vacuum Cast Coil?", "id": "Pengecoran Resin Dalam Ruang Hampa." },
  { "en": "Apa Itu Transformer Class F?", "id": "Isolasi Tahan Suhu 155 Derajat." },
  { "en": "Apa Itu Transformer Class H?", "id": "Isolasi Tahan Suhu 180 Derajat." },
  { "en": "Apa Itu Fan Cooling System?", "id": "Sistem Pendingin Kipas Trafo Kering." },
  { "en": "Apa Itu Sensor PT100 Trafo?", "id": "Sensor Suhu Kumparan Trafo Kering." },
  { "en": "Apa Itu Safety Helmet Class E?", "id": "Helm Tahan Tegangan Tinggi Listrik." },
  { "en": "Apa Itu Safety Shoes Electrical?", "id": "Sepatu Isolasi Tanpa Besi Terbuka." },
  { "en": "Apa Itu Insulating Gloves Class 0?", "id": "Sarung Tangan Tahan 1000 Volt." },
  { "en": "Apa Itu Insulating Gloves Class 4?", "id": "Sarung Tangan Tahan 36000 Volt." },
  { "en": "Apa Itu Leather Protector Gloves?", "id": "Pelindung Mekanis Sarung Tangan Karet." },
  { "en": "Apa Itu Arc Flash Suit?", "id": "Pakaian Pelindung Panas Busur Api." },
  { "en": "Apa Itu Calorie Rating APD?", "id": "Batas Energi Panas Tahan Pakaian." },
  { "en": "Apa Itu Lock Box LOTO?", "id": "Kotak Kunci Prosedur Isolasi Energi." },
  { "en": "Apa Itu Voltage Detector Stick?", "id": "Tongkat Deteksi Tegangan Jarak Jauh." },
  { "en": "Apa Itu Discharge Stick?", "id": "Tongkat Pembuang Muatan Listrik Sisa." },
  { "en": "Apa Rumus Reaktansi Induktif XL?", "id": "Dua Pi Kali Frekuensi Induktansi." },
  { "en": "Apa Rumus Reaktansi Kapasitif XC?", "id": "Satu Per Dua Pi FC." },
  { "en": "Apa Itu Resonansi Seri RLC?", "id": "Impedansi Minimum Arus Maksimum." },
  { "en": "Apa Itu Resonansi Paralel RLC?", "id": "Impedansi Maksimum Arus Minimum." },
  { "en": "Apa Itu Faktor Daya Lagging?", "id": "Arus Tertinggal Dari Tegangan Induktif." },
  { "en": "Apa Itu Faktor Daya Leading?", "id": "Arus Mendahului Tegangan Kapasitif." },
  { "en": "Apa Itu Segitiga Daya?", "id": "Hubungan Daya Aktif Reaktif Semu." },
  { "en": "Apa Itu Kompensasi Daya Reaktif?", "id": "Memperbaiki Faktor Daya Dengan Kapasitor." },
  { "en": "Apa Itu Disconnecting Switch Break?", "id": "Jarak Buka Kontak Pemisah Udara." },
  { "en": "Apa Itu Pantograph Disconnector?", "id": "Pemisah Model Gunting Hemat Lahan." },
  { "en": "Apa Itu Center Break Disconnector?", "id": "Pemisah Membuka Dari Tengah Lengan." },
  { "en": "Apa Itu Double Break Disconnector?", "id": "Pemisah Dengan Dua Titik Putus." },
  { "en": "Apa Itu Earthing Switch Interlock?", "id": "Mencegah Menutup Saat Ada Tegangan." },
  { "en": "Apa Itu Motor Operated Mechanism?", "id": "Penggerak Saklar Menggunakan Motor Listrik." },
  { "en": "Apa Itu Corona Detection Camera?", "id": "Kamera UV Deteksi Pelepasan Korona." },
  { "en": "Apa Itu Thermographic Camera?", "id": "Kamera Inframerah Deteksi Panas Lebih." },
  { "en": "Apa Itu Power Quality Analyzer?", "id": "Alat Analisis Kualitas Gelombang Listrik." },
  { "en": "Apa Itu Phase Sequence Indicator?", "id": "Alat Cek Urutan Fasa RST." },
  { "en": "Apa Itu Insulation Resistance Tester?", "id": "Alat Ukur Tahanan Isolasi Megger." },
  { "en": "Apa Itu Earth Resistance Tester?", "id": "Alat Ukur Tahanan Pentanahan Tanah." },
  { "en": "Apa Itu Micro Ohm Meter?", "id": "Alat Ukur Tahanan Kontak Rendah." },
  { "en": "Apa Itu Circuit Breaker Analyzer?", "id": "Alat Uji Waktu Gerak Breaker." },
  { "en": "Apa Itu Transformer Turns Ratio Meter?", "id": "Alat Ukur Perbandingan Lilitan Trafo." },
  { "en": "Apa Itu Winding Resistance Meter?", "id": "Alat Ukur Tahanan Lilitan Tembaga." },
  { "en": "Apa Itu Sweep Frequency Response Analysis?", "id": "Deteksi Pergeseran Mekanis Lilitan Trafo." },
  { "en": "Apa Itu Partial Discharge Detector?", "id": "Alat Deteksi Peluahan Listrik Parsial." },
  { "en": "Apa Itu VLF Hipot Tester?", "id": "Uji Kabel Tegangan Frekuensi Rendah." },
  { "en": "Apa Itu DC Hipot Tester?", "id": "Uji Tegangan Tinggi Arus Searah." },
  { "en": "Apa Itu Oil Dielectric Tester?", "id": "Alat Uji Tegangan Tembus Minyak." },
  { "en": "Apa Itu Battery Impedance Tester?", "id": "Alat Uji Kesehatan Sel Baterai." },
  { "en": "Apa Itu Power System Stability?", "id": "Kemampuan Sistem Kembali Normal Gangguan." },
  { "en": "Apa Itu Transient Stability?", "id": "Kestabilan Saat Gangguan Besar Tiba-Tiba." },
  { "en": "Apa Itu Dynamic Stability?", "id": "Kestabilan Terhadap Gangguan Kecil Osilasi." },
  { "en": "Apa Itu Steady State Stability?", "id": "Kestabilan Pada Kondisi Beban Normal." },
  { "en": "Apa Itu Voltage Stability?", "id": "Kemampuan Menjaga Tegangan Tetap Stabil." },
  { "en": "Apa Itu Frequency Stability?", "id": "Kemampuan Menjaga Frekuensi Tetap Stabil." },
  { "en": "Apa Itu Rotor Angle Stability?", "id": "Kestabilan Sudut Fasa Generator Sinkron." },
  { "en": "Apa Itu Swing Equation?", "id": "Persamaan Gerak Rotor Generator Sinkron." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Analisis Kestabilan Transien Generator." },
  { "en": "Apa Itu Critical Clearing Angle?", "id": "Sudut Maksimum Pemutusan Agar Stabil." },
  { "en": "Apa Itu Lightning Arrester Class 1?", "id": "Arrester Untuk Sambaran Petir Langsung." },
  { "en": "Apa Itu Lightning Arrester Class 2?", "id": "Arrester Untuk Sambaran Petir Induksi." },
  { "en": "Apa Itu Lightning Arrester Class 3?", "id": "Arrester Pelindung Peralatan Elektronik Sensitif." },
  { "en": "Apa Itu Metal Oxide Varistor? (MOV)", "id": "Komponen Utama Penahan Surja Arrester." },
  { "en": "Apa Itu Discharge Current Rating?", "id": "Kapasitas Arus Buang Maksimum Arrester." },
  { "en": "Apa Itu Maximum Continuous Operating Voltage?", "id": "Tegangan Kerja Terus Menerus Arrester." },
  { "en": "Apa Itu Residual Voltage Arrester?", "id": "Tegangan Sisa Saat Arrester Bekerja." },
  { "en": "Apa Itu Surge Counter?", "id": "Alat Hitung Frekuensi Kerja Arrester." },
  { "en": "Apa Itu Disconnector Arrester?", "id": "Pemisah Arrester Jika Rusak Total." },
  { "en": "Apa Itu Ground Lead Arrester?", "id": "Kabel Penghubung Arrester Ke Tanah." },
  { "en": "Apa Itu Busbar Protection 87B?", "id": "Proteksi Diferensial Untuk Rel Busbar." },
  { "en": "Apa Itu Transformer Differential 87T?", "id": "Proteksi Diferensial Untuk Transformator Daya." },
  { "en": "Apa Itu Generator Differential 87G?", "id": "Proteksi Diferensial Untuk Stator Generator." },
  { "en": "Apa Itu Line Differential 87L?", "id": "Proteksi Diferensial Saluran Transmisi Kabel." },
  { "en": "Apa Itu Restricted Earth Fault 64REF?", "id": "Proteksi Gangguan Tanah Terbatas Trafo." },
  { "en": "Apa Itu Standby Earth Fault 51N?", "id": "Proteksi Cadangan Gangguan Tanah Netral." },
  { "en": "Apa Itu Thermal Overload 49?", "id": "Proteksi Panas Berlebih Lilitan Peralatan." },
  { "en": "Apa Itu Negative Phase Sequence 46?", "id": "Proteksi Ketidakseimbangan Beban Fasa Terbalik." },
  { "en": "Apa Itu Over Excitation 24?", "id": "Proteksi Fluks Berlebih Pada Generator." },
  { "en": "Apa Itu Reverse Power 32?", "id": "Proteksi Daya Balik Ke Generator." },
  { "en": "Apa Itu Loss of Field 40?", "id": "Proteksi Hilang Medan Penguat Generator." },
  { "en": "Apa Itu Breaker Failure 50BF?", "id": "Proteksi Kegagalan Pemutus Tenaga Trip." },
  { "en": "Apa Itu Trip Circuit Supervision 74?", "id": "Pengawasan Kesiapan Rangkaian Trip Coil." },
  { "en": "Apa Itu Lockout Relay 86?", "id": "Relay Pengunci Setelah Gangguan Berat." },
  { "en": "Apa Itu Check Synchronizing 25?", "id": "Cek Sinkronisasi Sebelum Menutup Breaker." },
  { "en": "Apa Itu Under Frequency 81U?", "id": "Proteksi Frekuensi Rendah Lepas Beban." },
  { "en": "Apa Itu Over Frequency 81O?", "id": "Proteksi Frekuensi Tinggi Lepas Pembangkit." },
  { "en": "Apa Itu Rate of Change Frequency?", "id": "Proteksi Laju Perubahan Frekuensi Cepat." },
  { "en": "Apa Itu Auto Reclose 79?", "id": "Penutup Balik Otomatis Saluran Transmisi." },
  { "en": "Apa Itu Distance Protection 21?", "id": "Proteksi Jarak Berdasarkan Impedansi Saluran." },
  { "en": "Apa Itu Directional Overcurrent 67?", "id": "Proteksi Arus Lebih Dengan Arah." },
  { "en": "Apa Itu Directional Earth Fault 67N?", "id": "Proteksi Gangguan Tanah Dengan Arah." },
  { "en": "Apa Itu Phase Selection Logic?", "id": "Logika Memilih Fasa Yang Terganggu." },
  { "en": "Apa Itu Power Swing Blocking?", "id": "Blokir Trip Saat Ayunan Daya." },
  { "en": "Apa Itu Switch Onto Fault?", "id": "Trip Seketika Menutup Ke Gangguan." },
  { "en": "Apa Itu Stub Bus Protection?", "id": "Proteksi Sisa Busbar Saat Off." },
  { "en": "Apa Itu Fuse Failure Supervision?", "id": "Deteksi Sekring Trafo Tegangan Putus." },
  { "en": "Apa Itu CT Saturation Detection?", "id": "Deteksi Jenuh Inti Trafo Arus." },
  { "en": "Apa Itu Harmonic Restraint?", "id": "Tahan Trip Saat Ada Harmonisa." },
  { "en": "Apa Itu Detuning Factor Reaktor?", "id": "Perbandingan Reaktansi Induktor Dan Kapasitor." },
  { "en": "Apa Itu Resonansi Paralel Kapasitor?", "id": "Impedansi Tinggi Pada Frekuensi Resonansi." },
  { "en": "Apa Itu Resonansi Seri Kapasitor?", "id": "Impedansi Rendah Pada Frekuensi Resonansi." },
  { "en": "Apa Itu Breaking Time Pemutus?", "id": "Waktu Buka Kontak Tambah Busur." },
  { "en": "Apa Itu Opening Time Pemutus?", "id": "Waktu Dari Perintah Hingga Kontak Pisah." },
  { "en": "Apa Itu Arcing Time Pemutus?", "id": "Durasi Busur Api Saat Kontak Buka." },
  { "en": "Apa Itu Make Time Pemutus?", "id": "Waktu Kontak Bersentuhan Setelah Perintah." },
  { "en": "Apa Itu Pre-Arcing Time Fuse?", "id": "Waktu Sebelum Elemen Lebur Mencair." },
  { "en": "Apa Itu Total Clearing Time Fuse?", "id": "Waktu Pre-Arcing Tambah Waktu Busur." },
  { "en": "Apa Itu Current Limiting Fuse?", "id": "Membatasi Puncak Arus Hubung Singkat." },
  { "en": "Apa Itu Cut-Off Current Fuse?", "id": "Arus Maksimum Sesaat Yang Lewat." },
  { "en": "Apa Itu Fuse Blowing Point?", "id": "Titik Leleh Elemen Sekring." },
  { "en": "Apa Itu Relay Pickup Current?", "id": "Arus Minimum Relay Mulai Bekerja." },
  { "en": "Apa Itu Relay Dropout Current?", "id": "Arus Relay Kembali Ke Posisi Awal." },
  { "en": "Apa Itu Reset Ratio Relay?", "id": "Rasio Arus Dropout Terhadap Pickup." },
  { "en": "Apa Itu Plug Setting Relay?", "id": "Memilih Tap Arus Kerja Relay." },
  { "en": "Apa Itu Instantaneous Overcurrent Element?", "id": "Elemen Trip Tanpa Tunda Waktu." },
  { "en": "Apa Itu Definite Time Element?", "id": "Elemen Trip Dengan Waktu Tetap." },
  { "en": "Apa Itu Inverse Time Element?", "id": "Elemen Trip Tergantung Besar Arus." },
  { "en": "Apa Itu Electromechanical Relay Disc?", "id": "Piringan Induksi Berputar Pada Relay Lama." },
  { "en": "Apa Itu Spiral Spring Relay?", "id": "Pegas Penahan Torsi Piringan Relay." },
  { "en": "Apa Itu Shading Ring Relay?", "id": "Cincin Pembelah Fasa Fluks Magnet." },
  { "en": "Apa Itu Permanent Magnet Moving Coil?", "id": "Prinsip Kerja Meter Analog DC." },
  { "en": "Apa Itu Moving Iron Instrument?", "id": "Prinsip Kerja Meter Analog AC." },
  { "en": "Apa Itu Dynamometer Instrument?", "id": "Alat Ukur Daya (Wattmeter) Analog." },
  { "en": "Apa Itu Parallax Error Meter?", "id": "Kesalahan Baca Sudut Pandang Jarum." },
  { "en": "Apa Itu Burden Trafo Arus?", "id": "Beban Impedansi Total Sisi Sekunder." },
  { "en": "Apa Itu Accuracy Class 0.5?", "id": "Kesalahan Maksimum Nol Koma Lima Persen." },
  { "en": "Apa Itu Accuracy Class 5P10?", "id": "Error 5 Persen Pada 10 Kali Arus." },
  { "en": "Apa Itu Instrument Security Factor? (FS)", "id": "Faktor Keamanan Melindungi Alat Ukur." },
  { "en": "Apa Itu Rated Dynamic Current?", "id": "Arus Puncak Maksimum Tahan Mekanis." },
  { "en": "Apa Itu Rated Thermal Current?", "id": "Arus Tahan Panas Satu Detik." },
  { "en": "Apa Itu Open Circuit Voltage CT?", "id": "Tegangan Tinggi Bahaya Saat CT Terbuka." },
  { "en": "Apa Itu Shorting Block CT?", "id": "Terminal Hubung Singkat CT Aman." },
  { "en": "Apa Itu Capacitive Voltage Divider?", "id": "Pembagi Tegangan Menggunakan Kapasitor Seri." },
  { "en": "Apa Itu Inductive Voltage Transformer?", "id": "Trafo Tegangan Menggunakan Inti Besi." },
  { "en": "Apa Itu Ferroresonance VT?", "id": "Osilasi Antara Induktansi VT Dan Kapasitansi." },
  { "en": "Apa Itu Damping Resistor VT?", "id": "Resistor Peredam Osilasi Ferroresonansi." },
  { "en": "Apa Itu Residual Voltage Winding?", "id": "Lilitan Tambahan Deteksi Tegangan Nol." },
  { "en": "Apa Itu Gas Collection Time?", "id": "Waktu Terkumpulnya Gas Buchholz." },
  { "en": "Apa Itu Oil Surge Velocity?", "id": "Kecepatan Minyak Memicu Trip Buchholz." },
  { "en": "Apa Itu Pressure Relief Device Operation?", "id": "Pin Terangkat Saat Tekanan Berlebih." },
  { "en": "Apa Itu Oil Level Indicator?", "id": "Penunjuk Tinggi Permukaan Minyak Konservator." },
  { "en": "Apa Itu Winding Temperature Indicator?", "id": "Thermometer Simulasi Suhu Hotspot Lilitan." },
  { "en": "Apa Itu Oil Temperature Indicator?", "id": "Thermometer Suhu Minyak Bagian Atas." },
  { "en": "Apa Itu Cooling Fan Control?", "id": "Mengatur Nyala Kipas Berdasarkan Suhu." },
  { "en": "Apa Fungsi Oil Seal Breather?", "id": "Mencegah Udara Luar Masuk Langsung." },
  { "en": "Apa Itu Oil Sampling Valve?", "id": "Katup Pengambilan Contoh Minyak Uji." },
  { "en": "Apa Itu Drain Valve Trafo?", "id": "Katup Pembuangan Minyak Paling Bawah." },
  { "en": "Apa Itu Filter Press Valve?", "id": "Katup Sirkulasi Pemurnian Minyak." },
  { "en": "Apa Itu Explosion Vent Trafo?", "id": "Pipa Pelepasan Tekanan Trafo Lama." },
  { "en": "Apa Itu Conservator Air Bag?", "id": "Kantong Karet Pemisah Minyak Udara." },
  { "en": "Apa Itu Radiator Bank?", "id": "Kumpulan Sirip Pendingin Terpisah." },
  { "en": "Apa Itu Transformer Tank Shield?", "id": "Pelindung Magnetik Dinding Tangki Dalam." },
  { "en": "Apa Itu Core Grounding Bushing?", "id": "Terminal Pentanahan Inti Besi Luar." },
  { "en": "Apa Itu Tap Changer Mechanism?", "id": "Mekanisme Penggerak Kontak Pengubah Sadapan." },
  { "en": "Apa Itu Diverter Switch Oil?", "id": "Minyak Khusus Ruang Kontak Diverter." },
  { "en": "Apa Itu Online Oil Filter OLTC?", "id": "Filter Sirkulasi Minyak Tap Changer." },
  { "en": "Apa Itu Vacuum Bottle OLTC?", "id": "Kontak Arus OLTC Dalam Vakum." },
  { "en": "Apa Itu Tie-Line Power Flow?", "id": "Aliran Daya Antar Area Sistem." },
  { "en": "Apa Itu Inter-Area Oscillation?", "id": "Ayunan Daya Antar Dua Area." },
  { "en": "Apa Itu Local Mode Oscillation?", "id": "Ayunan Generator Terhadap Sisa Sistem." },
  { "en": "Apa Itu Power System Damping?", "id": "Kemampuan Meredam Osilasi Daya." },
  { "en": "Apa Itu Negative Damping?", "id": "Osilasi Yang Terus Membesar (Tidak Stabil)." },
  { "en": "Apa Itu Critical Clearing Time?", "id": "Waktu Maksimum Hapus Gangguan Stabil." },
  { "en": "Apa Itu Multi-Machine Stability?", "id": "Kestabilan Melibatkan Banyak Generator." },
  { "en": "Apa Itu Voltage Collapse Point?", "id": "Titik Tegangan Runtuh Pada Kurva PV." },
  { "en": "Apa Itu Load Tap Changer Control?", "id": "Pengaturan Tegangan Trafo Otomatis." },
  { "en": "Apa Itu Line Drop Compensation?", "id": "Kompensasi Tegangan Jatuh Ujung Saluran." },
  { "en": "Apa Itu Reactive Power Compensation?", "id": "Menyuntik Daya Reaktif Jaga Tegangan." },
  { "en": "Apa Itu Shunt Capacitor Bank?", "id": "Kapasitor Paralel Menaikkan Tegangan." },
  { "en": "Apa Itu Shunt Reactor Bank?", "id": "Reaktor Paralel Menurunkan Tegangan." },
  { "en": "Apa Itu Synchronous Condenser?", "id": "Motor Sinkron Pengatur Daya Reaktif." },
  { "en": "Apa Itu Static Var Compensator? (SVC)", "id": "Kompensator Reaktif Thyristor Cepat." },
  { "en": "Apa Itu Thyristor Controlled Reactor? (TCR)", "id": "Reaktor Variabel Kontrol Sudut Firing." },
  { "en": "Apa Itu Thyristor Switched Capacitor? (TSC)", "id": "Kapasitor Disaklar Oleh Thyristor." },
  { "en": "Apa Itu Harmonic Filter Bank?", "id": "Kapasitor Dan Reaktor Seri Resonansi." },
  { "en": "Apa Itu Detuned Reactor Factor?", "id": "Persen Tegangan Pada Reaktor Seri." },
  { "en": "Apa Itu C-Type Filter?", "id": "Filter Harmonisa Rugi Daya Rendah." },
  { "en": "Apa Itu High Pass Filter?", "id": "Meredam Semua Harmonisa Frekuensi Tinggi." },
  { "en": "Apa Itu Active Harmonic Filter?", "id": "Inverter Pembatal Arus Harmonisa." },
  { "en": "Apa Itu Interharmonic Frequency?", "id": "Frekuensi Bukan Kelipatan Bulat Fundamental." },
  { "en": "Apa Itu Voltage Flicker Source?", "id": "Beban Berfluktuasi Cepat (Arc Furnace)." },
  { "en": "Apa Itu Voltage Sag Duration?", "id": "Waktu Tegangan Turun Dibawah 90 Persen." },
  { "en": "Apa Itu Voltage Swell Duration?", "id": "Waktu Tegangan Naik Diatas 110 Persen." },
  { "en": "Apa Itu Transient Overvoltage?", "id": "Lonjakan Tegangan Sangat Cepat (Mikrodetik)." },
  { "en": "Apa Itu Voltage Notch?", "id": "Cacat Tegangan Akibat Komutasi Thyristor." },
  { "en": "Apa Itu DC Offset Current?", "id": "Komponen Searah Pada Gelombang Arus." },
  { "en": "Apa Itu Total Harmonic Distortion? (THD)", "id": "Ukuran Cacat Gelombang Akibat Harmonisa." },
  { "en": "Apa Itu Point Of Common Coupling?", "id": "Titik Sambung Pelanggan Dengan PLN." },
  { "en": "Apa Itu Short Circuit Ratio? (SCR)", "id": "Kekuatan Sistem Di Titik Sambung." },
  { "en": "Apa Itu Weak Grid System?", "id": "Sistem Impedansi Tinggi Mudah Goyang." },
  { "en": "Apa Itu Strong Grid System?", "id": "Sistem Impedansi Rendah Tegangan Stabil." },
  { "en": "Apa Itu Frequency Nadir?", "id": "Titik Terendah Frekuensi Saat Gangguan." },
  { "en": "Apa Itu Rate Of Change Of Frequency?", "id": "Kecepatan Perubahan Frekuensi (Hz/detik)." },
  { "en": "Apa Itu Load Shedding Scheme?", "id": "Pelepasan Beban Bertahap Selamatkan Sistem." },
  { "en": "Apa Itu Under Frequency Relay Setting?", "id": "Batas Frekuensi Memicu Lepas Beban." },
  { "en": "Apa Itu Islanding Detection?", "id": "Mendeteksi Pembangkit Terpisah Dari Grid." },
  { "en": "Apa Itu Rate of Change of Frequency? (ROCOF)", "id": "Laju Perubahan Frekuensi Terhadap Waktu." },
  { "en": "Apa Itu ROCOF Relay?", "id": "Relay Deteksi Perubahan Cepat Frekuensi." },
  { "en": "Apa Itu Power System Stabilizer? (PSS)", "id": "Alat Peredam Osilasi Generator Sinkron." },
  { "en": "Apa Itu Automatic Generation Control? (AGC)", "id": "Pengaturan Frekuensi Beban Terpusat Otomatis." },
  { "en": "Apa Itu Black Start Capability?", "id": "Kemampuan Start Tanpa Pasokan Luar." },
  { "en": "Apa Itu Synchrophasor?", "id": "Pengukuran Fasor Sinkron Waktu GPS." },
  { "en": "Apa Itu Phasor Data Concentrator? (PDC)", "id": "Pengumpul Data Dari Banyak PMU." },
  { "en": "Apa Itu Wide Area Monitoring System? (WAMS)", "id": "Sistem Pantau Grid Area Luas." },
  { "en": "Apa Itu Transient Stability Limit?", "id": "Batas Daya Transfer Kestabilan Transien." },
  { "en": "Apa Itu Voltage Collapse?", "id": "Runtuhnya Tegangan Akibat Kurang Reaktif." },
  { "en": "Apa Itu Kurva PV?", "id": "Kurva Daya Aktif Lawan Tegangan." },
  { "en": "Apa Itu Kurva QV?", "id": "Kurva Daya Reaktif Lawan Tegangan." },
  { "en": "Apa Itu Load Shedding Scheme?", "id": "Skema Lepas Beban Frekuensi Rendah." },
  { "en": "Apa Itu Islanding Detection?", "id": "Deteksi Operasi Terpisah Dari Grid." },
  { "en": "Apa Itu Non-Detection Zone? (NDZ)", "id": "Area Buta Deteksi Anti-Islanding." },
  { "en": "Apa Itu Active Anti-Islanding?", "id": "Inverter Sengaja Menggeser Frekuensi Jaringan." },
  { "en": "Apa Itu Passive Anti-Islanding?", "id": "Memantau Parameter Tegangan Dan Frekuensi." },
  { "en": "Apa Itu Low Voltage Ride Through? (LVRT)", "id": "Bertahan Saat Tegangan Jaringan Turun." },
  { "en": "Apa Itu High Voltage Ride Through? (HVRT)", "id": "Bertahan Saat Tegangan Jaringan Naik." },
  { "en": "Apa Itu Grid Code?", "id": "Aturan Teknis Koneksi Pembangkit Jaringan." },
  { "en": "Apa Itu Ancillary Services?", "id": "Layanan Pendukung Keandalan Sistem Tenaga." },
  { "en": "Apa Itu Spinning Reserve?", "id": "Cadangan Daya Generator Sinkron Berputar." },
  { "en": "Apa Itu Cold Reserve?", "id": "Cadangan Pembangkit Dalam Kondisi Mati." },
  { "en": "Apa Itu Load Following?", "id": "Pembangkit Menyesuaikan Output Sesuai Beban." },
  { "en": "Apa Itu Base Load Plant?", "id": "Pembangkit Operasi Beban Dasar Konstan." },
  { "en": "Apa Itu Peaking Plant?", "id": "Pembangkit Operasi Saat Beban Puncak." },
  { "en": "Apa Itu Ramp Rate Generator?", "id": "Kecepatan Naik Turun Daya Generator." },
  { "en": "Apa Itu Minimum Stable Level?", "id": "Batas Daya Terendah Operasi Stabil." },
  { "en": "Apa Itu Heat Rate Pembangkit?", "id": "Efisiensi Konversi Bahan Bakar Listrik." },
  { "en": "Apa Itu Capacity Factor?", "id": "Rasio Produksi Aktual Terhadap Maksimum." },
  { "en": "Apa Itu Availability Factor?", "id": "Persentase Waktu Pembangkit Siap Operasi." },
  { "en": "Apa Itu Forced Outage Rate?", "id": "Tingkat Gangguan Paksa Pembangkit Listrik." },
  { "en": "Apa Itu Planned Outage?", "id": "Pemadaman Terencana Untuk Pemeliharaan Rutin." },
  { "en": "Apa Itu Economic Dispatch?", "id": "Pembagian Beban Biaya Paling Murah." },
  { "en": "Apa Itu Unit Commitment?", "id": "Penjadwalan Unit Pembangkit Yang Aktif." },
  { "en": "Apa Itu Incremental Cost?", "id": "Biaya Tambahan Per Satu Megawatt." },
  { "en": "Apa Itu Penalty Factor Transmisi?", "id": "Faktor Koreksi Rugi-Rugi Saluran Transmisi." },
  { "en": "Apa Itu Locational Marginal Price? (LMP)", "id": "Harga Listrik Di Lokasi Tertentu." },
  { "en": "Apa Itu Congestion Management?", "id": "Mengelola Kemacetan Aliran Daya Transmisi." },
  { "en": "Apa Itu Transmission Right of Way?", "id": "Jalur Tanah Bebas Bawah Transmisi." },
  { "en": "Apa Itu N-1 Contingency?", "id": "Sistem Aman Satu Komponen Hilang." },
  { "en": "Apa Itu N-2 Contingency?", "id": "Sistem Aman Dua Komponen Hilang." },
  { "en": "Apa Itu State Estimation?", "id": "Estimasi Kondisi Sistem Dari Pengukuran." },
  { "en": "Apa Itu Power Flow Study?", "id": "Analisis Aliran Daya Kondisi Normal." },
  { "en": "Apa Itu Short Circuit Study?", "id": "Analisis Arus Saat Hubung Singkat." },
  { "en": "Apa Itu Harmonic Analysis?", "id": "Analisis Distorsi Gelombang Listrik Sistem." },
  { "en": "Apa Itu Transient Stability Study?", "id": "Analisis Kestabilan Saat Gangguan Besar." },
  { "en": "Apa Itu Motor Starting Study?", "id": "Analisis Jatuh Tegangan Start Motor." },
  { "en": "Apa Itu Protection Coordination Study?", "id": "Pengaturan Selektivitas Relay Proteksi Sistem." },
  { "en": "Apa Itu Arc Flash Analysis?", "id": "Perhitungan Energi Panas Busur Api." },
  { "en": "Apa Itu Grounding Grid Design?", "id": "Perancangan Sistem Pentanahan Gardu Induk." },
  { "en": "Apa Itu Soil Resistivity Test?", "id": "Pengukuran Tahanan Jenis Tanah Lokasi." },
  { "en": "Apa Itu Step Voltage?", "id": "Tegangan Antara Dua Kaki Manusia." },
  { "en": "Apa Itu Touch Voltage?", "id": "Tegangan Antara Tangan Dan Kaki." },
  { "en": "Apa Itu Mesh Voltage?", "id": "Tegangan Sentuh Maksimum Dalam Grid." },
  { "en": "Apa Itu Gravel Layer?", "id": "Lapisan Batu Koral Tahanan Tinggi." },
  { "en": "Apa Fungsi Batu Koral Gardu?", "id": "Meningkatkan Tahanan Kontak Kaki Tanah." },
  { "en": "Apa Itu Grounding Rod?", "id": "Batang Tembaga Ditanam Vertikal Tanah." },
  { "en": "Apa Itu Grounding Grid Conductor?", "id": "Kawat Tembaga Ditanam Horizontal Tanah." },
  { "en": "Apa Itu Exothermic Weld?", "id": "Sambungan Las Kimia Kabel Grounding." },
  { "en": "Apa Itu Compression Connector?", "id": "Konektor Jepit Kabel Grounding Mekanis." },
  { "en": "Apa Itu Wenner 4-Point Method?", "id": "Metode Ukur Resistivitas Tanah Standar." },
  { "en": "Apa Itu Fall of Potential Method?", "id": "Metode Ukur Tahanan Pentanahan Elektroda." },
  { "en": "Apa Itu Clamp-On Ground Tester?", "id": "Ukur Grounding Tanpa Lepas Koneksi." },
  { "en": "Apa Itu Stray Current?", "id": "Arus Bocor Mengalir Di Tanah." },
  { "en": "Apa Itu Cathodic Protection?", "id": "Mencegah Korosi Logam Dalam Tanah." },
  { "en": "Apa Itu Sacrificial Anode?", "id": "Logam Korban Pelindung Katodik Pasif." },
  { "en": "Apa Itu Impressed Current Protection?", "id": "Proteksi Katodik Sumber Listrik DC." },
  { "en": "Apa Itu Rectifier Cathodic Protection?", "id": "Penyearah Suplai Arus Proteksi Katodik." },
  { "en": "Apa Itu Reference Electrode?", "id": "Elektroda Acuan Ukur Potensial Tanah." },
  { "en": "Apa Itu Copper Sulfate Electrode?", "id": "Jenis Elektroda Referensi Paling Umum." },
  { "en": "Apa Itu Interference AC Corrosion?", "id": "Korosi Pipa Akibat Induksi Listrik." },
  { "en": "Apa Itu Polarization Cell?", "id": "Membuang Arus AC Menahan DC." },
  { "en": "Apa Itu Decoupler Solid State?", "id": "Pengganti Polarization Cell Berbasis Semikonduktor." },
  { "en": "Apa Itu Pipeline Current Mapper?", "id": "Alat Pelacak Arus Pipa Bawah Tanah." },
  { "en": "Apa Itu Holiday Detector?", "id": "Alat Deteksi Lubang Coating Pipa." },
  { "en": "Apa Itu Pearson Survey?", "id": "Survei Deteksi Kerusakan Coating Pipa." },
  { "en": "Apa Itu DCVG Survey?", "id": "Direct Current Voltage Gradient Survey." },
  { "en": "Apa Itu Close Interval Survey?", "id": "Survei Potensial Pipa Jarak Rapat." },
  { "en": "Apa Itu Current Interrupter?", "id": "Alat Pemutus Arus Siklus Survei." },
  { "en": "Apa Itu Soil Box Test?", "id": "Uji Resistivitas Sampel Tanah Kotak." },
  { "en": "Apa Itu pH Tanah?", "id": "Tingkat Keasaman Tanah Pengaruhi Korosi." },
  { "en": "Apa Itu Anaerobic Bacteria Corrosion?", "id": "Korosi Bakteri Tanpa Oksigen Tanah." },
  { "en": "Apa Itu Sulfate Reducing Bacteria? (SRB)", "id": "Bakteri Penyebab Korosi Besi Pipa." },
  { "en": "Apa Itu Biocide Injection?", "id": "Menyuntik Kimia Pembunuh Bakteri Korosi." },
  { "en": "Apa Itu Coupon Test Station?", "id": "Titik Uji Sampel Logam Korosi." },
  { "en": "Apa Itu Bond Box?", "id": "Kotak Sambungan Kabel Negatif Pipa." },
  { "en": "Apa Itu Shunt Resistor Bond?", "id": "Resistor Ukur Arus Sambungan Pipa." },
  { "en": "Apa Itu Flange Insulation Kit?", "id": "Isolator Pemisah Elektrik Sambungan Pipa." },
  { "en": "Apa Itu Monolithic Isolation Joint?", "id": "Sambungan Isolasi Pabrikan Tanpa Baut." },
  { "en": "Apa Itu Spark Gap Protector?", "id": "Pelindung Isolasi Flange Dari Petir." },
  { "en": "Apa Itu Zinc Grounding Cell?", "id": "Elektroda Seng Pembuang Arus AC." },
  { "en": "Apa Itu Magnesium Anode?", "id": "Anoda Korban Tanah Resistivitas Tinggi." },
  { "en": "Apa Itu Zinc Anode?", "id": "Anoda Korban Tanah Resistivitas Rendah." },
  { "en": "Apa Itu Aluminium Anode?", "id": "Anoda Korban Khusus Air Laut." },
  { "en": "Apa Itu Backfill Anode?", "id": "Campuran Kimia Pembungkus Anoda Tanah." },
  { "en": "Apa Fungsi Gypsum Backfill?", "id": "Menjaga Kelembaban Sekitar Anoda Korban." },
  { "en": "Apa Itu Ring Main Unit? (RMU)", "id": "Panel Saklar Tegangan Menengah Sistem Cincin." },
  { "en": "Apa Fungsi Utama RMU?", "id": "Membagi Dan Melindungi Jalur Distribusi Listrik." },
  { "en": "Apa Itu Load Break Switch? (LBS)", "id": "Saklar Pemutus Arus Beban Nominal." },
  { "en": "Apa Beda LBS Dan Breaker?", "id": "LBS Tidak Bisa Memutus Arus Gangguan." },
  { "en": "Apa Itu Fuse Switch Disconnector?", "id": "Saklar LBS Dilengkapi Sekring Pengaman." },
  { "en": "Apa Itu Feeder Distribusi?", "id": "Saluran Penyalur Listrik Dari Gardu Induk." },
  { "en": "Apa Itu Sistem Distribusi Radial?", "id": "Satu Jalur Pasokan Tanpa Loop Balik." },
  { "en": "Apa Kelemahan Sistem Radial?", "id": "Keandalan Rendah, Padam Jika Kabel Putus." },
  { "en": "Apa Itu Sistem Distribusi Loop?", "id": "Jalur Pasokan Melingkar (Dua Arah Suplai)." },
  { "en": "Apa Keuntungan Sistem Loop?", "id": "Keandalan Tinggi, Suplai Alternatif Tersedia." },
  { "en": "Apa Itu Sistem Distribusi Spindel?", "id": "Konfigurasi Kabel Ujung Bertemu Di Gardu." },
  { "en": "Apa Itu Express Feeder?", "id": "Penyulang Langsung Ke Pusat Beban Tanpa Cabang." },
  { "en": "Apa Itu Technical Loss Distribusi?", "id": "Rugi Daya Akibat Fisik Peralatan (Panas)." },
  { "en": "Apa Itu Non-Technical Loss?", "id": "Rugi Akibat Pencurian Atau Kesalahan Meter." },
  { "en": "Apa Itu Fault Location Isolation Restoration? (FLISR)", "id": "Sistem Otomatis Melokalisir Gangguan Distribusi." },
  { "en": "Apa Itu Smart Grid Gateway?", "id": "Gerbang Komunikasi Antara Jaringan Dan Meter." },
  { "en": "Apa Itu Head End System? (HES)", "id": "Pusat Pengumpul Data Meter AMI." },
  { "en": "Apa Itu Meter Data Management? (MDMS)", "id": "Sistem Pengelola Data Besar Meter Pintar." },
  { "en": "Apa Itu Tamper Switch Meter?", "id": "Saklar Deteksi Pembukaan Paksa Tutup Meter." },
  { "en": "Apa Itu Earth Fault Indicator? (EFI)", "id": "Alat Deteksi Lokasi Gangguan Tanah Kabel." },
  { "en": "Apa Itu Short Circuit Indicator? (SCI)", "id": "Alat Deteksi Lokasi Hubung Singkat Kabel." },
  { "en": "Apa Itu Pole Mounted Switch?", "id": "Saklar LBS Yang Dipasang Di Tiang." },
  { "en": "Apa Itu Sectionalizer Otomatis?", "id": "Pemisah Seksi Jaringan Saat Jaringan Mati." },
  { "en": "Apa Itu Recloser Control Box?", "id": "Kotak Kontrol Elektronik Untuk Recloser." },
  { "en": "Apa Itu Trafo CSP? (Completely Self Protected)", "id": "Trafo Distribusi Dengan Proteksi Internal Lengkap." },
  { "en": "Apa Itu Trafo Inti Amorf?", "id": "Trafo Distribusi Rugi Besi Sangat Rendah." },
  { "en": "Apa Itu Pad Mounted Transformer?", "id": "Trafo Distribusi Duduk Di Atas Beton." },
  { "en": "Apa Itu Gardu Beton?", "id": "Gardu Distribusi Bangunan Sipil Permanen." },
  { "en": "Apa Itu Gardu Kiosk?", "id": "Gardu Distribusi Prefabrikasi Dari Logam." },
  { "en": "Apa Itu Gardu Cantol?", "id": "Trafo Distribusi Digantung Pada Satu Tiang." },
  { "en": "Apa Itu Gardu Portal?", "id": "Trafo Distribusi Duduk Di Antara Dua Tiang." },
  { "en": "Apa Itu Gardu Sisipan?", "id": "Gardu Tambahan Mengatasi Beban Lebih." },
  { "en": "Apa Itu Low Voltage Board? (LV Board)", "id": "Panel Pembagi Jaringan Tegangan Rendah." },
  { "en": "Apa Itu NH Fuse? (Knife Fuse)", "id": "Sekring Tegangan Rendah Kapasitas Tinggi." },
  { "en": "Apa Itu Fuse Puller?", "id": "Alat Pencabut Sekring NH Dengan Aman." },
  { "en": "Apa Itu Operating Stick? (Opstik)", "id": "Tongkat Isolasi Pengoperasian Saklar Tiang." },
  { "en": "Apa Itu Joint Sleeve?", "id": "Selongsong Sambungan Kabel Tegangan Rendah." },
  { "en": "Apa Itu Parallel Groove Clamp?", "id": "Klem Sambungan Cabang Kabel Udara." },
  { "en": "Apa Itu Strain Clamp?", "id": "Klem Penarik Kabel Di Tiang Ujung." },
  { "en": "Apa Itu Suspension Clamp?", "id": "Klem Penggantung Kabel Di Tiang Lurus." },
  { "en": "Apa Itu Guy Insulator?", "id": "Isolator Pemisah Pada Kawat Trekskor." },
  { "en": "Apa Itu Kabel TIC? (Twisted Insulated Cable)", "id": "Kabel Udara Tegangan Rendah Berpilin." },
  { "en": "Apa Itu Insulation Piercing Connector?", "id": "Konektor Tusuk Kabel Berisolasi (TIC)." },
  { "en": "Apa Itu Spacer Cable System?", "id": "Sistem Kabel Udara Berjarak Rapat." },
  { "en": "Apa Itu Tree Wire?", "id": "Kabel Udara Berisolasi Tahan Gesekan Pohon." },
  { "en": "Apa Batas Tegangan Jatuh Distribusi?", "id": "Maksimal 5 Persen Dari Tegangan Nominal." },
  { "en": "Apa Itu Capacitor Bank Tiang?", "id": "Kapasitor Perbaikan Tegangan Di Jaringan." },
  { "en": "Apa Itu Step Voltage Regulator?", "id": "Trafo Pengatur Tegangan Otomatis Di Jaringan." },
  { "en": "Apa Itu Buck-Boost Transformer?", "id": "Trafo Kecil Penurun Atau Penaik Tegangan." },
  { "en": "Apa Itu Rotary UPS?", "id": "UPS Menggunakan Generator Motor Berputar." },
  { "en": "Apa Itu Diesel Rotary UPS? (DRUPS)", "id": "UPS Putar Terkopel Mesin Diesel." },
  { "en": "Apa Itu Static Transfer Switch? (STS)", "id": "Saklar Pindah Sumber Sangat Cepat." },
  { "en": "Apa Itu Manual Transfer Switch? (MTS)", "id": "Saklar Pindah Sumber Dioperasikan Tangan." },
  { "en": "Apa Itu Paralleling Switchgear?", "id": "Panel Sinkronisasi Beberapa Genset Bersamaan." },
  { "en": "Apa Itu Load Sharing Module?", "id": "Modul Pembagi Beban Genset Paralel." },
  { "en": "Apa Itu Peak Shaving Genset?", "id": "Genset Operasi Saat Beban Puncak Saja." },
  { "en": "Apa Itu Prime Power Rating?", "id": "Rating Daya Genset Beban Bervariasi." },
  { "en": "Apa Itu Continuous Power Rating?", "id": "Rating Daya Genset Beban Konstan 100%." },
  { "en": "Apa Itu Standby Power Rating?", "id": "Rating Daya Genset Cadangan Darurat." },
  { "en": "Apa Itu Power Usage Effectiveness? (PUE)", "id": "Ukuran Efisiensi Energi Data Center." },
  { "en": "Apa Itu Tier 1 Data Center?", "id": "Pusat Data Tanpa Redundansi Komponen." },
  { "en": "Apa Itu Tier 4 Data Center?", "id": "Pusat Data Toleransi Kesalahan Penuh." },
  { "en": "Apa Itu Redundansi N+1?", "id": "Satu Komponen Cadangan Untuk Sistem." },
  { "en": "Apa Itu Redundansi 2N?", "id": "Sistem Ganda Penuh (Dua Jalur Suplai)." },
  { "en": "Apa Itu Rack PDU?", "id": "Distribusi Daya Di Rak Server." },
  { "en": "Apa Itu Trafo Faktor K?", "id": "Trafo Tahan Panas Akibat Harmonisa." },
  { "en": "Apa Itu Welding Transformer?", "id": "Trafo Arus Tinggi Untuk Mesin Las." },
  { "en": "Apa Karakteristik Trafo Las?", "id": "Reaktansi Kebocoran Tinggi, Tegangan Jatuh." },
  { "en": "Apa Itu Electric Arc Furnace?", "id": "Tungku Peleburan Baja Busur Listrik." },
  { "en": "Apa Masalah Utama Arc Furnace?", "id": "Menyebabkan Flicker Tegangan Dan Harmonisa." },
  { "en": "Apa Itu Electrostatic Precipitator?", "id": "Penyaring Debu Cerobong Asap Listrik." },
  { "en": "Apa Prinsip Kerja Electrostatic Precipitator?", "id": "Menarik Debu Bermuatan Ke Pelat Pengumpul." },
  { "en": "Apa Itu Cathodic Protection Rectifier?", "id": "Penyearah Suplai Arus Proteksi Karat." },
  { "en": "Apa Itu Induction Heating?", "id": "Pemanasan Logam Menggunakan Arus Eddy." },
  { "en": "Apa Itu Dielectric Heating?", "id": "Pemanasan Bahan Isolator (Microwave)." },
  { "en": "Apa Itu Variable Speed Drive? (VSD)", "id": "Alat Pengatur Kecepatan Motor Listrik." },
  { "en": "Apa Itu Soft Starter Motor?", "id": "Alat Start Motor Mengurangi Lonjakan Arus." },
  { "en": "Apa Beda Soft Starter Dan VFD?", "id": "Soft Starter Tidak Mengatur Kecepatan Operasi." },
  { "en": "Apa Itu Bypass Contactor?", "id": "Kontaktor Pintas Setelah Soft Start Selesai." },
  { "en": "Apa Itu Ramp Up Time?", "id": "Waktu Motor Mencapai Kecepatan Penuh." },
  { "en": "Apa Itu Ramp Down Time?", "id": "Waktu Motor Berhenti Secara Perlahan." },
  { "en": "Apa Itu Kick Start?", "id": "Torsi Awal Tinggi Sesaat Untuk Beban Berat." },
  { "en": "Apa Itu Current Limit Start?", "id": "Membatasi Arus Maksimum Saat Start." },
  { "en": "Apa Itu Torque Control VFD?", "id": "Mengatur Torsi Motor, Bukan Kecepatan." },
  { "en": "Apa Itu Regenerative Drive?", "id": "VFD Mengembalikan Energi Pengereman Ke Jaringan." },
  { "en": "Apa Itu Braking Chopper?", "id": "Saklar Pembuang Energi Ke Resistor Rem." },
  { "en": "Apa Itu EMC Filter?", "id": "Filter Kompatibilitas Elektromagnetik VFD." },
  { "en": "Apa Itu RFI Filter?", "id": "Filter Interferensi Frekuensi Radio." },
  { "en": "Apa Itu Output Choke?", "id": "Induktor Output VFD Lindungi Kabel." },
  { "en": "Apa Itu Motor Cable Shielded?", "id": "Kabel Motor VFD Berpelindung Ground." },
  { "en": "Apa Itu Pulse Width Modulation? (PWM)", "id": "Teknik Modulasi Sinyal Kontrol VFD." },
  { "en": "Apa Itu Carrier Frequency VFD?", "id": "Frekuensi Switching Transistor Inverter." },
  { "en": "Apa Itu Scalar Control?", "id": "Kontrol V/F Sederhana Untuk Motor." },
  { "en": "Apa Itu Vector Control?", "id": "Kontrol Motor Presisi Tinggi (FOC)." },
  { "en": "Apa Itu Flux Vector Control?", "id": "Nama Lain Dari Vector Control." },
  { "en": "Apa Itu Slip Compensation?", "id": "Menjaga Kecepatan Motor Saat Beban Naik." },
  { "en": "Apa Itu Auto Tuning Motor?", "id": "VFD Membaca Parameter Motor Otomatis." },
  { "en": "Apa Itu Dynamic Braking?", "id": "Pengereman Membuang Panas Ke Resistor." },
  { "en": "Apa Itu Ladder Diagram PLC?", "id": "Bahasa Pemrograman PLC Logika Tangga." },
  { "en": "Apa Itu Function Block Diagram? (FBD)", "id": "Bahasa PLC Blok Fungsi Logika." },
  { "en": "Apa Itu Structured Text PLC?", "id": "Bahasa PLC Mirip Pascal Atau C." },
  { "en": "Apa Itu Instruction List PLC?", "id": "Bahasa PLC Mirip Assembly Low-Level." },
  { "en": "Apa Itu Sequential Function Chart?", "id": "Bahasa PLC Alur Proses Berurutan." },
  { "en": "Apa Itu HMI? (Human Machine Interface)", "id": "Layar Antarmuka Operator Dan Mesin." },
  { "en": "Apa Itu Tag Database HMI?", "id": "Daftar Variabel Penghubung PLC HMI." },
  { "en": "Apa Itu Trending HMI?", "id": "Grafik Riwayat Data Proses Waktu." },
  { "en": "Apa Itu Alarm Management HMI?", "id": "Sistem Notifikasi Gangguan Pada Layar." },
  { "en": "Apa Itu Remote I/O PLC?", "id": "Modul Input Output Jarak Jauh." },
  { "en": "Apa Itu Redundant PLC?", "id": "Dua PLC Bekerja Saling Mem-Backup." },
  { "en": "Apa Itu Hot Standby PLC?", "id": "PLC Cadangan Siap Ambil Alih." },
  { "en": "Apa Itu Cold Standby PLC?", "id": "PLC Cadangan Perlu Dinyalakan Manual." },
  { "en": "Apa Itu PID Loop Control?", "id": "Kontrol Umpan Balik Presisi Proses." },
  { "en": "Apa Itu Tuning PID Otomatis?", "id": "PLC Mencari Parameter PID Sendiri." },
  { "en": "Apa Itu Analog Input 4-20mA?", "id": "Standar Sinyal Sensor Industri Arus." },
  { "en": "Apa Itu Analog Output 0-10V?", "id": "Standar Sinyal Kontrol Tegangan." },
  { "en": "Apa Itu Resolusi Bit ADC?", "id": "Ketelitian Konversi Sinyal Analog Digital." },
  { "en": "Apa Itu Sink Input PLC?", "id": "Input Menerima Arus (Common Positif)." },
  { "en": "Apa Itu Source Input PLC?", "id": "Input Mengeluarkan Arus (Common Negatif)." },
  { "en": "Apa Itu Relay Output PLC?", "id": "Output Kontak Kering Saklar Mekanis." },
  { "en": "Apa Itu Transistor Output PLC?", "id": "Output Saklar Elektronik Kecepatan Tinggi." },
  { "en": "Apa Itu Pulse Train Output?", "id": "Output Pulsa Pengendali Motor Stepper." },
  { "en": "Apa Itu High Speed Counter?", "id": "Input Penghitung Pulsa Cepat Encoder." },
  { "en": "Apa Itu Ex d? (Flameproof)", "id": "Peralatan Tahan Ledakan Casing Tebal." },
  { "en": "Apa Itu Ex e? (Increased Safety)", "id": "Peralatan Didesain Tidak Memercik Api." },
  { "en": "Apa Itu Ex i? (Intrinsic Safety)", "id": "Energi Dibatasi Tidak Memicu Ledakan." },
  { "en": "Apa Itu Ex p? (Pressurized)", "id": "Casing Bertekanan Gas Mencegah Gas Masuk." },
  { "en": "Apa Itu Zener Barrier?", "id": "Pembatas Energi Rangkaian Ex i." },
  { "en": "Apa Itu Hazardous Area Zone 0?", "id": "Gas Meledak Selalu Ada Terus-Menerus." },
  { "en": "Apa Itu Hazardous Area Zone 1?", "id": "Gas Meledak Mungkin Ada Operasi Normal." },
  { "en": "Apa Itu Hazardous Area Zone 2?", "id": "Gas Meledak Jarang Ada (Bocoran)." },
  { "en": "Apa Itu Temperatur Kelas T1?", "id": "Suhu Permukaan Maksimum 450 Derajat." },
  { "en": "Apa Itu Temperatur Kelas T6?", "id": "Suhu Permukaan Maksimum 85 Derajat." },
  { "en": "Apa Itu Gas Group IIC?", "id": "Gas Paling Mudah Meledak (Hidrogen)." },
  { "en": "Apa Itu Gas Group IIA?", "id": "Gas Kurang Sensitif (Propana)." },
  { "en": "Apa Itu ATEX Directive?", "id": "Standar Eropa Peralatan Area Ledakan." },
  { "en": "Apa Itu Sertifikasi IECEx?", "id": "Standar Internasional Peralatan Area Ledakan." },
  { "en": "Apa Itu Cable Gland Ex d?", "id": "Klem Kabel Tahan Ledakan Api." },
  { "en": "Apa Itu Stopping Plug?", "id": "Penutup Lubang Panel Tidak Terpakai." },
  { "en": "Apa Itu Breather Drain Ex?", "id": "Pembuang Air Embun Panel Ex." },
  { "en": "Apa Itu IP Rating IP65?", "id": "Tahan Debu Dan Semprotan Air." },
  { "en": "Apa Itu IP Rating IP68?", "id": "Tahan Debu Dan Rendam Terus." },
  { "en": "Apa Angka Pertama Kode IP?", "id": "Perlindungan Terhadap Benda Padat (Debu)." },
  { "en": "Apa Angka Kedua Kode IP?", "id": "Perlindungan Terhadap Benda Cair (Air)." },
  { "en": "Apa Itu NEMA 4X Enclosure?", "id": "Box Tahan Karat Dan Air." },
  { "en": "Apa Itu IK Rating?", "id": "Ketahanan Terhadap Benturan Mekanis Fisik." },
  { "en": "Apa Itu IK10 Rating?", "id": "Tahan Benturan Energi 20 Joule." },
  { "en": "Apa Itu Lampu High Bay?", "id": "Lampu Gantung Untuk Plafon Tinggi." },
  { "en": "Apa Itu Lampu Flood Light?", "id": "Lampu Sorot Area Luas." },
  { "en": "Apa Itu Lampu Emergency Exit?", "id": "Lampu Tanda Keluar Darurat Baterai." },
  { "en": "Apa Itu Ballast Elektronik?", "id": "Pengendali Lampu Neon Frekuensi Tinggi." },
  { "en": "Apa Itu Starter Lampu TL?", "id": "Pemicu Awal Nyala Lampu Neon." },
  { "en": "Apa Itu Fitting E27?", "id": "Dudukan Lampu Ulir Standar Rumah." },
  { "en": "Apa Itu Fitting E40?", "id": "Dudukan Lampu Ulir Besar Industri." },
  { "en": "Apa Itu Lumen Per Watt?", "id": "Ukuran Efisiensi Energi Lampu." },
  { "en": "Apa Itu Lux Meter Digital?", "id": "Alat Ukur Intensitas Cahaya Ruangan." },
  { "en": "Apa Itu Kabel NYA?", "id": "Kabel Tembaga Tunggal Isolasi PVC." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Tembaga Isolasi PVC Putih." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Tembaga Isolasi PVC Hitam." },
  { "en": "Apa Itu Kabel NYFGBY?", "id": "Kabel Tanah Perisai Plat Baja." },
  { "en": "Apa Itu Kabel N2XSY?", "id": "Kabel Tegangan Menengah Isolasi XLPE." },
  { "en": "Apa Itu Kabel Coaxial RG6?", "id": "Kabel Antena TV Atau CCTV." },
  { "en": "Apa Itu Kabel Fire Resistant? (FRC)", "id": "Kabel Tahan Api Tetap Operasi." },
  { "en": "Apa Itu Kabel Low Smoke Zero Halogen?", "id": "Kabel Bakar Tidak Asap Beracun." },
  { "en": "Apa Itu Cable Tray Perforated?", "id": "Rak Kabel Plat Besi Berlubang." },
  { "en": "Apa Itu Cable Ladder Hot Dip?", "id": "Tangga Kabel Lapis Seng Tebal." },
  { "en": "Apa Itu Busduct Sandwich?", "id": "Busbar Padat Tanpa Celah Udara." },
  { "en": "Apa Itu Busduct Air Insulated?", "id": "Busbar Dengan Pemisah Udara." },
  { "en": "Apa Itu Megger Test 1 Menit?", "id": "Standar Waktu Uji Tahanan Isolasi." },
  { "en": "Apa Itu Polarization Index (PI)?", "id": "Rasio Megger 10 Menit 1 Menit." },
  { "en": "Apa Itu Dielectric Absorption Ratio (DAR)?", "id": "Rasio Megger 60 Detik 30 Detik." },
  { "en": "Apa Arti Nilai PI Diatas 2?", "id": "Isolasi Kondisi Bagus Dan Kering." },
  { "en": "Apa Arti Nilai PI Dibawah 1?", "id": "Isolasi Rusak Atau Sangat Lembab." },
  { "en": "Apa Itu Contact Resistance Test?", "id": "Ukur Tahanan Kontak Breaker (Ductor)." },
  { "en": "Apa Satuan Tahanan Kontak?", "id": "Mikro Ohm." },
  { "en": "Apa Itu Tang Ampere True RMS?", "id": "Ukur Arus Akurat Meski Terdistorsi." },
  { "en": "Apa Itu Phase Rotation Meter?", "id": "Alat Cek Urutan Fasa R-S-T." },
  { "en": "Apa Itu Earth Tester 3 Pole?", "id": "Alat Ukur Grounding Tiga Kabel." },
  { "en": "Apa Itu Loop Impedance Tester?", "id": "Ukur Impedansi Lingkaran Gangguan." },
  { "en": "Apa Itu RCD Tester?", "id": "Alat Uji Trip ELCB." },
  { "en": "Apa Itu Thermal Imager?", "id": "Kamera Deteksi Panas Sambungan Kendor." },
  { "en": "Apa Itu Vibration Meter?", "id": "Alat Ukur Getaran Motor Bearing." },
  { "en": "Apa Itu Laser Tachometer?", "id": "Alat Ukur RPM Tanpa Sentuh." },
  { "en": "Apa Itu Power Analyzer 3 Fasa?", "id": "Alat Rekam Daya Dan Harmonisa." },
  { "en": "Apa Itu Battery Load Tester?", "id": "Alat Uji Kemampuan Arus Aki." },
  { "en": "Apa Itu Oil BDV Tester?", "id": "Alat Uji Tembus Minyak Trafo." },
  { "en": "Apa Itu Relay Test Set?", "id": "Alat Injeksi Uji Relay Proteksi." },
  { "en": "Apa Itu Primary Injection Kit?", "id": "Sumber Arus Besar Uji CT." },
  { "en": "Apa Itu High Voltage Probe?", "id": "Probe Ukur Tegangan Tinggi Langsung." },
  { "en": "Apa Itu LCR Meter?", "id": "Alat Ukur Induktansi Kapasitansi Resistansi." },
  { "en": "Apa Itu Function Generator?", "id": "Pembangkit Gelombang Sinyal Uji." },
  { "en": "Apa Itu Oscilloscope Digital?", "id": "Alat Lihat Bentuk Gelombang Listrik." },
  { "en": "Apa Itu Soldering Station?", "id": "Solder Dengan Pengatur Suhu Presisi." },
  { "en": "Apa Itu Desoldering Pump?", "id": "Penyedot Timah Solder Cair." },
  { "en": "Apa Itu Crimping Tool Skun?", "id": "Tang Press Sepatu Kabel." },
  { "en": "Apa Itu Hydraulic Crimper?", "id": "Tang Press Hidrolik Kabel Besar." },
  { "en": "Apa Itu Wire Stripper?", "id": "Tang Pengupas Isolasi Kabel." },
  { "en": "Apa Itu Motor Shaded Pole?", "id": "Motor Induksi Satu Fasa Torsi Rendah." },
  { "en": "Apa Itu Motor Universal?", "id": "Motor Seri AC DC Kecepatan Tinggi." },
  { "en": "Apa Itu Motor Repulsi?", "id": "Motor AC Rotor Sikat Terhubung Singkat." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor Bergerak Dalam Langkah Sudut Diskrit." },
  { "en": "Apa Itu Motor Reluktansi?", "id": "Motor Bekerja Prinsip Torsi Reluktansi Minimum." },
  { "en": "Apa Itu Motor Histeresis?", "id": "Motor Sinkron Rotor Baja Keras Halus." },
  { "en": "Apa Itu Motor Induksi Linier?", "id": "Motor Menghasilkan Gaya Gerak Lurus." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Umpan Balik Kontrol Presisi." },
  { "en": "Apa Itu Encoder Motor?", "id": "Sensor Umpan Balik Kecepatan Dan Posisi." },
  { "en": "Apa Itu Resolver Motor?", "id": "Trafo Putar Analog Umpan Balik Posisi." },
  { "en": "Apa Itu Tachogenerator?", "id": "Generator Tegangan Sebanding Kecepatan Putar." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Motor Jadi Generator Kembalikan Energi Listrik." },
  { "en": "Apa Itu Pengereman Dinamik?", "id": "Energi Motor Dibuang Ke Resistor Panas." },
  { "en": "Apa Itu Pengereman Plugging?", "id": "Membalik Urutan Fasa Untuk Hentikan Motor." },
  { "en": "Apa Itu Pengereman Injeksi DC?", "id": "Menyuntik Arus DC Ke Stator Motor." },
  { "en": "Apa Itu Carrier Frequency VFD?", "id": "Frekuensi Switching Transistor IGBT Inverter." },
  { "en": "Apa Itu Kontrol V/Hz?", "id": "Metode Kontrol Kecepatan Fluks Konstan." },
  { "en": "Apa Itu Vector Control?", "id": "Kontrol Terpisah Torsi Dan Fluks Motor." },
  { "en": "Apa Itu Direct Torque Control? (DTC)", "id": "Kontrol Histeresis Torsi Dan Fluks Langsung." },
  { "en": "Apa Itu Slip Compensation?", "id": "Menambah Frekuensi Saat Beban Motor Naik." },
  { "en": "Apa Itu IR Compensation?", "id": "Meningkatkan Tegangan Saat Kecepatan Rendah." },
  { "en": "Apa Itu Bypass Contactor Soft Starter?", "id": "Kontaktor Pintas Setelah Motor Kecepatan Penuh." },
  { "en": "Apa Itu Ramp Up Time?", "id": "Waktu Akselerasi Menuju Kecepatan Setpoint." },
  { "en": "Apa Itu Ramp Down Time?", "id": "Waktu Deselerasi Menuju Berhenti." },
  { "en": "Apa Itu Coast To Stop?", "id": "Memutus Daya Membiarkan Motor Berhenti Sendiri." },
  { "en": "Apa Itu Service Factor Motor?", "id": "Pengali Kapasitas Beban Lebih Motor." },
  { "en": "Apa Itu Locked Rotor Current?", "id": "Arus Saat Rotor Motor Diam Terkunci." },
  { "en": "Apa Itu Breakdown Torque?", "id": "Torsi Maksimum Sebelum Motor Mogok." },
  { "en": "Apa Itu Pull-Up Torque?", "id": "Torsi Minimum Selama Proses Akselerasi." },
  { "en": "Apa Itu Cogging Torque?", "id": "Torsi Riak Akibat Interaksi Slot Stator." },
  { "en": "Apa Itu Crawling Motor?", "id": "Motor Berjalan Stabil Di Kecepatan Rendah." },
  { "en": "Apa Itu Single Phasing?", "id": "Motor Tiga Fasa Kehilangan Satu Fasa." },
  { "en": "Apa Itu Phase Imbalance?", "id": "Tegangan Tidak Seimbang Pada Tiga Fasa." },
  { "en": "Apa Itu Isolasi Kelas F?", "id": "Tahan Suhu Hotspot 155 Derajat Celcius." },
  { "en": "Apa Itu Isolasi Kelas H?", "id": "Tahan Suhu Hotspot 180 Derajat Celcius." },
  { "en": "Apa Itu Bearing Fluting?", "id": "Erosi Alur Bearing Akibat Arus Poros." },
  { "en": "Apa Itu Shaft Voltage?", "id": "Tegangan Terinduksi Pada Poros Motor." },
  { "en": "Apa Itu Grounding Brush?", "id": "Sikat Menghubungkan Poros Ke Tanah." },
  { "en": "Apa Itu Insulated Bearing?", "id": "Bearing Isolasi Mencegah Aliran Arus." },
  { "en": "Apa Itu Grease Relief Valve?", "id": "Katup Mencegah Tekanan Gemuk Berlebih." },
  { "en": "Apa Itu Enclosure TEFC? (Totally Enclosed Fan Cooled)", "id": "Motor Tertutup Rapat Pendingin Kipas Luar." },
  { "en": "Apa Itu Enclosure ODP? (Open Drip Proof)", "id": "Motor Terbuka Tahan Tetesan Air." },
  { "en": "Apa Itu Explosion Proof Motor?", "id": "Motor Menahan Ledakan Internal Dengan Aman." },
  { "en": "Apa Itu Flameproof Ex d?", "id": "Casing Tahan Tekanan Ledakan Gas Internal." },
  { "en": "Apa Itu Increased Safety Ex e?", "id": "Desain Mencegah Percikan Api Dan Busur." },
  { "en": "Apa Itu Non-Sparking Ex n?", "id": "Peralatan Normal Tidak Memercikkan Api." },
  { "en": "Apa Itu Intrinsic Safety Ex i?", "id": "Energi Dibatasi Dibawah Ambang Ledakan." },
  { "en": "Apa Itu Thermistor Protection?", "id": "Sensor PTC Ditanam Dalam Lilitan Motor." },
  { "en": "Apa Itu Sensor RTD Motor?", "id": "Sensor Suhu Presisi Lilitan Motor." },
  { "en": "Apa Itu Space Heater Motor?", "id": "Pemanas Mencegah Embun Saat Motor Mati." },
  { "en": "Apa Itu Form Wound Coil?", "id": "Lilitan Kawat Persegi Motor Besar." },
  { "en": "Apa Itu Random Wound Coil?", "id": "Lilitan Kawat Bulat Motor Kecil." },
  { "en": "Apa Itu Vacuum Pressure Impregnation? (VPI)", "id": "Proses Resin Vakum Hilangkan Rongga Udara." },
  { "en": "Apa Itu Dip And Bake?", "id": "Celup Pernis Dan Panggang Sederhana." },
  { "en": "Apa Itu Surge Test Motor?", "id": "Uji Hubung Singkat Antar Lilitan." },
  { "en": "Apa Itu Hi-Pot Test Motor?", "id": "Uji Tegangan Tinggi Isolasi Ke Ground." },
  { "en": "Apa Itu Growler Test?", "id": "Uji Hubung Singkat Lilitan Armature." },
  { "en": "Apa Itu Drop Test Armature?", "id": "Uji Lilitan Putus Pada Armature." },
  { "en": "Apa Itu Commutator Undercutting?", "id": "Memotong Isolasi Mika Antara Segmen Komutator." },
  { "en": "Apa Itu Commutator Turning?", "id": "Membubut Permukaan Komutator Agar Rata." },
  { "en": "Apa Itu Carbon Brush Grade?", "id": "Tingkat Kekerasan Dan Konduktivitas Sikat." },
  { "en": "Apa Itu Brush Seating?", "id": "Membentuk Sikat Sesuai Lengkung Komutator." },
  { "en": "Apa Itu Neutral Plane Setting?", "id": "Posisi Sikat Percikan Api Minimum." },
  { "en": "Apa Itu Interpole Winding?", "id": "Lilitan Bantu Perbaiki Komutasi Motor DC." },
  { "en": "Apa Itu Compensating Winding?", "id": "Lilitan Netralkan Reaksi Jangkar Kutub Utama." },
  { "en": "Apa Karakteristik Motor Seri?", "id": "Torsi Start Tinggi Kecepatan Tanpa Batas." },
  { "en": "Apa Karakteristik Motor Shunt?", "id": "Kecepatan Konstan Saat Beban Berubah." },
  { "en": "Apa Itu Motor Kompon?", "id": "Gabungan Karakteristik Motor Seri Dan Shunt." },
  { "en": "Apa Itu Ward Leonard System?", "id": "Kontrol Kecepatan DC Menggunakan Generator." },
  { "en": "Apa Itu Thyristor Drive?", "id": "Kontrol Motor DC Menggunakan Penyearah SCR." },
  { "en": "Apa Itu Chopper Drive?", "id": "Kontrol Motor DC Menggunakan DC-DC Converter." },
  { "en": "Apa Itu Four Quadrant Operation?", "id": "Bisa Maju Mundur Motor Dan Rem." },
  { "en": "Apa Itu Cycloconverter?", "id": "Pengubah Frekuensi AC Ke AC Langsung." },
  { "en": "Apa Itu Matrix Converter?", "id": "Konverter AC-AC Tanpa Kapasitor DC Link." },
  { "en": "Apa Itu CSI Drive? (Current Source Inverter)", "id": "Inverter Sumber Arus Konstan." },
  { "en": "Apa Itu VSI Drive? (Voltage Source Inverter)", "id": "Inverter Sumber Tegangan Konstan." },
  { "en": "Apa Itu PWM Inverter?", "id": "Inverter Modulasi Lebar Pulsa." },
  { "en": "Apa Itu SHE-PWM?", "id": "PWM Eliminasi Harmonisa Selektif." },
  { "en": "Apa Itu Space Vector PWM?", "id": "PWM Vektor Ruang Optimalkan Tegangan Bus." },
  { "en": "Apa Itu Common Mode Voltage?", "id": "Tegangan Antara Netral Virtual Dan Tanah." },
  { "en": "Apa Itu Differential Mode Voltage?", "id": "Tegangan Antara Dua Fasa." },
  { "en": "Apa Itu dV/dt Filter?", "id": "Filter Batasi Laju Kenaikan Tegangan." },
  { "en": "Apa Itu Sine Wave Filter?", "id": "Filter Hasilkan Gelombang Sinus Murni." },
  { "en": "Apa Itu Line Reactor?", "id": "Induktor Sisi Input Lindungi Drive." },
  { "en": "Apa Itu DC Link Choke?", "id": "Induktor Perata Arus Bus DC." },
  { "en": "Apa Itu Active Front End? (AFE)", "id": "Rectifier Regeneratif Rendah Harmonisa." },
  { "en": "Apa Itu 12-Pulse Rectifier?", "id": "Penyearah Mengurangi Harmonisa Arus Input." },
  { "en": "Apa Itu 18-Pulse Rectifier?", "id": "Penyearah Harmonisa Sangat Rendah." },
  { "en": "Apa Itu Phase Shift Transformer?", "id": "Trafo Geser Fasa Untuk Multi-Pulse Rectifier." },
  { "en": "Apa Itu Flying Capacitor Inverter?", "id": "Inverter Multilevel Menggunakan Kapasitor Terbang." },
  { "en": "Apa Itu Diode Clamped Inverter?", "id": "Inverter Multilevel Menggunakan Dioda Jepit." },
  { "en": "Apa Itu Cascaded H-Bridge?", "id": "Inverter Multilevel Modul Seri H-Bridge." },
  { "en": "Apa Itu Modular Multilevel Converter? (MMC)", "id": "Konverter HVDC Modul Half-Bridge Seri." },
  { "en": "Apa Itu Gate Driver Circuit?", "id": "Rangkaian Penguat Sinyal Gerbang IGBT." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Pelindung Lonjakan Tegangan Saklar." },
  { "en": "Apa Itu Crowbar Circuit?", "id": "Rangkaian Proteksi Hubung Singkat Output." },
  { "en": "Apa Itu Desaturation Detection?", "id": "Deteksi Hubung Singkat Arus Lebih IGBT." },
  { "en": "Apa Itu Dead Time Inverter?", "id": "Jeda Waktu Mencegah Hubung Singkat Bus." },
  { "en": "Apa Itu Shoot-Through Fault?", "id": "Hubung Singkat Bus DC Melalui Inverter." },
  { "en": "Apa Itu Thermal Interface Material?", "id": "Pasta Penghantar Panas Ke Heatsink." },
  { "en": "Apa Itu Thermal Resistance Heatsink?", "id": "Hambatan Aliran Panas Ke Udara." },
  { "en": "Apa Itu Junction Temperature?", "id": "Suhu Chip Silikon Semikonduktor." },
  { "en": "Apa Itu Safe Operating Area? (SOA)", "id": "Batas Aman Operasi Tegangan Arus." },
  { "en": "Apa Itu Reverse Recovery Time?", "id": "Waktu Dioda Berubah Jadi Memblokir." },
  { "en": "Apa Itu Tail Current IGBT?", "id": "Arus Sisa Saat IGBT Mati." },
  { "en": "Apa Itu Miller Clamp?", "id": "Mencegah Nyala Palsu Akibat Tegangan." },
  { "en": "Apa Itu Gate Resistor?", "id": "Mengatur Kecepatan Switching IGBT MOSFET." },
  { "en": "Apa Itu Kelvin Connection?", "id": "Koneksi Empat Kabel Ukur Presisi." },
  { "en": "Apa Itu Shunt Resistor Low Inductance?", "id": "Resistor Ukur Arus Pulsa Cepat." },
  { "en": "Apa Itu Fluxgate Sensor?", "id": "Sensor Arus DC Akurasi Tinggi." },
  { "en": "Apa Itu Closed Loop Hall Sensor?", "id": "Sensor Arus Umpan Balik Akurat." },
  { "en": "Apa Itu Open Loop Hall Sensor?", "id": "Sensor Arus Tanpa Lilitan Kompensasi." },
  { "en": "Apa Itu Burden Resistor CT?", "id": "Resistor Beban Sekunder Trafo Arus." },
  { "en": "Apa Itu Accuracy Class 0.2?", "id": "Kesalahan Maksimum Nol Koma Dua." },
  { "en": "Apa Itu Instrument Transformer Polarity?", "id": "Arah Aliran Arus Primer Sekunder." },
  { "en": "Apa Itu Test Block CT?", "id": "Terminal Uji Dengan Fasilitas Shorting." },
  { "en": "Apa Itu Revenue Metering?", "id": "Pengukuran Energi Untuk Tagihan Listrik." },
  { "en": "Apa Itu Time of Use Metering?", "id": "Tarif Listrik Berbeda Berdasarkan Waktu." },
  { "en": "Apa Itu Maximum Demand Meter?", "id": "Mengukur Permintaan Daya Rata-Rata Tertinggi." },
  { "en": "Apa Itu Sliding Window Demand?", "id": "Perhitungan Demand Geser Interval Waktu." },
  { "en": "Apa Itu Block Interval Demand?", "id": "Perhitungan Demand Reset Tiap Interval." },
  { "en": "Apa Itu Reactive Power Meter?", "id": "Mengukur Energi VAR Jam (kVARh)." },
  { "en": "Apa Itu Four Quadrant Meter?", "id": "Mengukur Impor Ekspor Aktif Reaktif." },
  { "en": "Apa Itu Pulse Output Meter?", "id": "Keluaran Sinyal Pulsa Sebanding Energi." },
  { "en": "Apa Itu Protokol DLMS/COSEM?", "id": "Standar Komunikasi Meter Listrik Pintar." },
  { "en": "Apa Itu OBIS Code?", "id": "Kode Identifikasi Data Meter Listrik." },
  { "en": "Apa Itu AMR System?", "id": "Pembacaan Meter Otomatis Satu Arah." },
  { "en": "Apa Itu AMI System?", "id": "Infrastruktur Meter Cerdas Dua Arah." },
  { "en": "Apa Itu Meter Data Concentrator?", "id": "Pengumpul Data Dari Banyak Meter." },
  { "en": "Apa Itu Power Line Communication Meter?", "id": "Kirim Data Meter Lewat Kabel." },
  { "en": "Apa Itu GPRS Meter?", "id": "Meter Menggunakan Jaringan Seluler GSM." },
  { "en": "Apa Itu NB-IoT Meter?", "id": "Meter Menggunakan Jaringan IoT Narrowband." },
  { "en": "Apa Itu Prepaid Meter Token?", "id": "Kode Angka 20 Digit Listrik." },
  { "en": "Apa Itu Credit Meter?", "id": "Meter Pascabayar Tagihan Bulanan." },
  { "en": "Apa Itu Tamper Detection Meter?", "id": "Deteksi Upaya Pencurian Listrik Fisik." },
  { "en": "Apa Itu Magnetic Tamper?", "id": "Gangguan Meter Menggunakan Magnet Kuat." },
  { "en": "Apa Itu Reverse Current Tamper?", "id": "Membalik Arah Arus Masuk Meter." },
  { "en": "Apa Itu Neutral Disturbance Tamper?", "id": "Memutus Netral Agar Meter Mati." },
  { "en": "Apa Itu Load Profile Data?", "id": "Rekaman Pemakaian Listrik Per Interval." },
  { "en": "Apa Itu Sag Swell Recording?", "id": "Merekam Kejadian Kualitas Daya Buruk." },
  { "en": "Apa Itu Individual Harmonic Distortion?", "id": "Distorsi Satu Frekuensi Harmonisa Spesifik." },
  { "en": "Apa Itu Interharmonic Emission?", "id": "Emisi Frekuensi Non-Integer Dari Beban." },
  { "en": "Apa Itu Voltage Unbalance NEMA?", "id": "Rumus Deviasi Maksimum Rata-Rata." },
  { "en": "Apa Itu Voltage Unbalance IEC?", "id": "Rumus Komponen Negatif Bagi Positif." },
  { "en": "Apa Itu Harmonic Derating Factor?", "id": "Faktor Penurunan Beban Akibat Harmonisa." },
  { "en": "Apa Itu Skin Effect Loss?", "id": "Rugi Daya Akibat Arus Permukaan." },
  { "en": "Apa Itu Proximity Effect Loss?", "id": "Rugi Daya Akibat Medan Tetangga." },
  { "en": "Apa Itu Stray Loss Trafo?", "id": "Rugi Arus Eddy Di Tangki." },
  { "en": "Apa Itu Hysteresis Loss Formula?", "id": "Rugi Berbanding Lurus Frekuensi." },
  { "en": "Apa Itu Eddy Current Loss Formula?", "id": "Rugi Berbanding Lurus Kuadrat Frekuensi." },
  { "en": "Apa Itu Steinmetz Equation?", "id": "Rumus Menghitung Rugi Histeresis Magnetik." },
  { "en": "Apa Itu Magnetic Saturation Curve?", "id": "Kurva Hubungan B Dan H." },
  { "en": "Apa Itu Relative Permeability?", "id": "Permeabilitas Bahan Banding Ruang Hampa." },
  { "en": "Apa Itu Diamagnetic Material?", "id": "Bahan Menolak Medan Magnet Lemah." },
  { "en": "Apa Itu Paramagnetic Material?", "id": "Bahan Menarik Medan Magnet Lemah." },
  { "en": "Apa Itu Ferromagnetic Material?", "id": "Bahan Menarik Medan Magnet Kuat." },
  { "en": "Apa Itu Hard Magnetic Material?", "id": "Bahan Magnet Permanen Susah Demagnetisasi." },
  { "en": "Apa Itu Soft Magnetic Material?", "id": "Bahan Mudah Magnetisasi Dan Demagnetisasi." },
  { "en": "Apa Itu B-H Curve Loop?", "id": "Loop Histeresis Bahan Magnetik." },
  { "en": "Apa Itu Residual Magnetism?", "id": "Magnetisme Tersisa Setelah Medan Hilang." },
  { "en": "Apa Itu Coercive Force?", "id": "Medan Diperlukan Menghilangkan Magnetisme Sisa." },
  { "en": "Apa Itu Magnetostriction Noise?", "id": "Suara Dengung Akibat Perubahan Dimensi." },
  { "en": "Apa Itu Amorphous Core Loss?", "id": "Rugi Inti Besi Sangat Rendah." },
  { "en": "Apa Itu Nanocrystalline Core?", "id": "Bahan Inti Magnetik Frekuensi Tinggi." },
  { "en": "Apa Itu Powdered Iron Core?", "id": "Inti Serbuk Besi Induktor DC." },
  { "en": "Apa Itu Ferrite Bead Impedance?", "id": "Impedansi Resistif Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Differential Mode Choke?", "id": "Induktor Meredam Noise Satu Jalur." },
  { "en": "Apa Itu X-Capacitor?", "id": "Kapasitor Filter Antar Fasa Netral." },
  { "en": "Apa Itu Y-Capacitor?", "id": "Kapasitor Filter Antar Fasa Ground." },
  { "en": "Apa Itu Leakage Current Y-Cap?", "id": "Arus Bocor Ke Tanah Kapasitor." },
  { "en": "Apa Itu EMI Filter Stage?", "id": "Tahapan Penyaringan Gangguan Elektromagnetik." },
  { "en": "Apa Itu Insertion Loss Filter?", "id": "Penurunan Sinyal Akibat Pasang Filter." },
  { "en": "Apa Itu Radiated Emission?", "id": "Gangguan EMI Melalui Udara." },
  { "en": "Apa Itu Conducted Emission?", "id": "Gangguan EMI Melalui Kabel Listrik." },
  { "en": "Apa Itu LISN? (Line Impedance Stabilization Network)", "id": "Alat Standar Pengujian Conducted Emission." },
  { "en": "Apa Itu Anechoic Chamber?", "id": "Ruang Kedap Gelombang Elektromagnetik." },
  { "en": "Apa Itu EMC Testing?", "id": "Uji Kompatibilitas Elektromagnetik Perangkat." },
  { "en": "Apa Itu ESD Immunity Test?", "id": "Uji Ketahanan Terhadap Listrik Statis." },
  { "en": "Apa Itu Surge Immunity Test?", "id": "Uji Ketahanan Terhadap Lonjakan Petir." },
  { "en": "Apa Itu EFT/Burst Test?", "id": "Uji Ketahanan Transien Cepat Berulang." },
  { "en": "Apa Itu Voltage Dip Test?", "id": "Uji Ketahanan Terhadap Kedipan Tegangan." },
  { "en": "Apa Itu Harmonic Emission Test?", "id": "Uji Arus Harmonisa Yang Dihasilkan." },
  { "en": "Apa Itu Flicker Emission Test?", "id": "Uji Fluktuasi Tegangan Yang Dihasilkan." },
  { "en": "Apa Itu Ground Bond Test?", "id": "Uji Koneksi Grounding Body Alat." },
  { "en": "Apa Itu Leakage Current Test?", "id": "Uji Arus Bocor Saat Operasi." },
  { "en": "Apa Itu Functional Test?", "id": "Uji Fungsi Alat Sesuai Spesifikasi." },
  { "en": "Apa Itu Burn-In Test?", "id": "Uji Nyala Durasi Lama Stress." },
  { "en": "Apa Itu Environmental Test?", "id": "Uji Suhu Dan Kelembaban Ekstrem." },
  { "en": "Apa Itu Vibration Test Shaker?", "id": "Meja Getar Simulasi Transportasi Operasi." },
  { "en": "Apa Itu Drop Test Package?", "id": "Uji Jatuh Kemasan Produk." },
  { "en": "Apa Itu HALT? (Highly Accelerated Life Test)", "id": "Uji Percepatan Umur Ekstrem." },
  { "en": "Apa Itu HASS? (Highly Accelerated Stress Screen)", "id": "Skrining Cacat Produksi Cepat." },
  { "en": "Apa Itu MTBF? (Mean Time Between Failures)", "id": "Rata-Rata Waktu Antar Kegagalan." },
  { "en": "Apa Itu MTTF? (Mean Time To Failure)", "id": "Rata-Rata Waktu Sampai Rusak Total." },
  { "en": "Apa Itu MTTR? (Mean Time To Repair)", "id": "Rata-Rata Waktu Perbaikan Alat." },
  { "en": "Apa Itu Availability System?", "id": "Persentase Waktu Sistem Siap Pakai." },
  { "en": "Apa Itu Reliability Bathtub Curve?", "id": "Kurva Laju Kegagalan Produk Elektronik." },
  { "en": "Apa Itu Infant Mortality?", "id": "Kegagalan Dini Produk Baru." },
  { "en": "Apa Itu Random Failure?", "id": "Kegagalan Acak Selama Masa Pakai." },
  { "en": "Apa Itu Wear Out Failure?", "id": "Kegagalan Akibat Usia Komponen Tua." },
  { "en": "Apa Itu Derating Component?", "id": "Mengurangi Beban Komponen Perpanjang Umur." },
  { "en": "Apa Itu Hot Swappable?", "id": "Ganti Modul Tanpa Matikan Sistem." },
  { "en": "Apa Itu Fault Tolerance?", "id": "Sistem Tetap Jalan Meski Rusak." },
  { "en": "Apa Itu Fail Safe Design?", "id": "Desain Gagal Ke Kondisi Aman." },
  { "en": "Apa Itu Probability of Failure on Demand?", "id": "Peluang Gagal Saat Dibutuhkan." },
  { "en": "Apa Itu SIF? (Safety Instrumented Function)", "id": "Fungsi Instrumen Pengaman Khusus." },
  { "en": "Apa Itu BPCS? (Basic Process Control System)", "id": "Sistem Kontrol Proses Normal Harian." },
  { "en": "Apa Itu Safety Integrity Level 1?", "id": "Probabilitas Gagal Antara E-1 Dan E-2." },
  { "en": "Apa Itu Safety Integrity Level 2?", "id": "Probabilitas Gagal Antara E-2 Dan E-3." },
  { "en": "Apa Itu Safety Integrity Level 3?", "id": "Probabilitas Gagal Antara E-3 Dan E-4." },
  { "en": "Apa Itu Voting Logic 2oo3?", "id": "Dua Dari Tiga Sensor Harus Trip." },
  { "en": "Apa Itu Partial Stroke Test?", "id": "Uji Gerak Katup Sebagian Tanpa Stop." },
  { "en": "Apa Itu Solenoid Valve Reset?", "id": "Mengembalikan Katup Ke Posisi Normal." },
  { "en": "Apa Itu Emergency Stop Button?", "id": "Tombol Merah Pemicu Berhenti Darurat." },
  { "en": "Apa Itu Pull Cord Switch?", "id": "Saklar Tali Penghenti Konveyor Darurat." },
  { "en": "Apa Itu Belt Sway Switch?", "id": "Deteksi Sabuk Konveyor Miring Keluar." },
  { "en": "Apa Itu Zero Speed Switch?", "id": "Deteksi Poros Berhenti Atau Slip." },
  { "en": "Apa Itu Vibration Switch?", "id": "Saklar Trip Akibat Getaran Berlebih." },
  { "en": "Apa Itu Bearing Temperature Detector?", "id": "Sensor Suhu Bantalan Logam (RTD)." },
  { "en": "Apa Itu Motor Winding Heater?", "id": "Pemanas Anti Embun Saat Motor Mati." },
  { "en": "Apa Itu Cable Tray Ladder?", "id": "Rak Kabel Bentuk Tangga Ventilasi Baik." },
  { "en": "Apa Itu Cable Tray Solid?", "id": "Rak Kabel Plat Penuh Tanpa Lubang." },
  { "en": "Apa Itu Cable Tray Wire Mesh?", "id": "Rak Kabel Jaring Kawat Ringan." },
  { "en": "Apa Itu Conduit GIP?", "id": "Pipa Pelindung Kabel Besi Galvanis." },
  { "en": "Apa Itu Conduit PVC?", "id": "Pipa Pelindung Kabel Plastik Ringan." },
  { "en": "Apa Itu Flexible Conduit?", "id": "Pipa Pelindung Kabel Bisa Ditekuk." },
  { "en": "Apa Itu Cable Gland Armoured?", "id": "Klem Kabel Khusus Kabel Perisai Baja." },
  { "en": "Apa Itu Cable Lug Bimetal?", "id": "Sepatu Kabel Sambung Tembaga Aluminium." },
  { "en": "Apa Itu Ferrule Kabel?", "id": "Pin Terminal Ujung Kabel Serabut." },
  { "en": "Apa Itu Busbar Heat Shrink?", "id": "Selongsong Isolasi Warna Warni Busbar." },
  { "en": "Apa Itu Busbar Support Insulator?", "id": "Isolator Penyangga Batang Busbar Panel." },
  { "en": "Apa Itu Panel Incoming Feeder?", "id": "Panel Penerima Daya Dari Trafo." },
  { "en": "Apa Itu Panel Outgoing Feeder?", "id": "Panel Pembagi Daya Ke Beban." },
  { "en": "Apa Itu Panel Capacitor Bank?", "id": "Panel Kompensasi Daya Reaktif Otomatis." },
  { "en": "Apa Itu Panel Synchronizing?", "id": "Panel Paralel Genset Atau Trafo." },
  { "en": "Apa Itu Panel ATS AMF?", "id": "Panel Otomatis Pindah PLN Ke Genset." },
  { "en": "Apa Itu Pilot Lamp Panel?", "id": "Lampu Indikator Status Fasa R-S-T." },
  { "en": "Apa Itu Ampere Meter Selector?", "id": "Saklar Pilih Pembacaan Arus Fasa." },
  { "en": "Apa Itu Volt Meter Selector?", "id": "Saklar Pilih Pembacaan Tegangan Fasa." },
  { "en": "Apa Itu Emergency Key Box?", "id": "Kotak Kunci Kaca Pecah Darurat." },
  { "en": "Apa Itu Single Line Diagram?", "id": "Gambar Garis Tunggal Sistem Listrik." },
  { "en": "Apa Itu Wiring Diagram Panel?", "id": "Gambar Pengawatan Kabel Rinci Panel." },
  { "en": "Apa Itu Layout Diagram Panel?", "id": "Gambar Tata Letak Komponen Fisik." },
  { "en": "Apa Itu Bill Of Material?", "id": "Daftar Jumlah Dan Spesifikasi Komponen." },
  { "en": "Apa Itu Short Circuit Calculation?", "id": "Perhitungan Arus Hubung Singkat Maksimum." },
  { "en": "Apa Itu Voltage Drop Calculation?", "id": "Perhitungan Jatuh Tegangan Kabel Jarak." },
  { "en": "Apa Itu Cable Sizing Calculation?", "id": "Perhitungan Ukuran Kabel Sesuai Beban." },
  { "en": "Apa Itu Derating Factor Kabel?", "id": "Faktor Pengurang Kapasitas Arus Kabel." },
  { "en": "Apa Itu Grouping Factor Kabel?", "id": "Derating Akibat Kabel Berhimpitan Panas." },
  { "en": "Apa Itu Ambient Temperature Factor?", "id": "Derating Akibat Suhu Lingkungan Tinggi." },
  { "en": "Apa Itu Soil Thermal Resistivity?", "id": "Tahanan Panas Tanah Buang Panas." },
  { "en": "Apa Itu Direct Buried Cable?", "id": "Kabel Ditanam Langsung Dalam Tanah." },
  { "en": "Apa Itu Cable In Duct?", "id": "Kabel Ditanam Dalam Pipa Beton." },
  { "en": "Apa Itu Cable Tunnel?", "id": "Terowongan Khusus Jalur Kabel Bawah Tanah." },
  { "en": "Apa Itu Manhole Kabel?", "id": "Lubang Akses Perawatan Kabel Tanah." },
  { "en": "Apa Itu Handhole Kabel?", "id": "Lubang Akses Kecil Tarik Kabel." },
  { "en": "Apa Itu Cable Pulling Lubricant?", "id": "Pelumas Licin Memudah Tarik Kabel." },
  { "en": "Apa Itu Cable Grip (Stocking)?", "id": "Anyaman Kawat Penarik Ujung Kabel." },
  { "en": "Apa Itu Swivel Link?", "id": "Sambungan Putar Anti Kabel Melintir." },
  { "en": "Apa Itu Maximum Pulling Tension?", "id": "Batas Gaya Tarik Kabel Aman." },
  { "en": "Apa Itu Minimum Bending Radius?", "id": "Batas Tekukan Kabel Agar Tidak Rusak." },
  { "en": "Apa Itu Side Wall Pressure?", "id": "Tekanan Samping Kabel Saat Belok." },
  { "en": "Apa Itu Cable Cleat?", "id": "Klem Penahan Kabel Saat Hubung Singkat." },
  { "en": "Apa Itu Trefoil Formation?", "id": "Susunan Tiga Kabel Fasa Segitiga." },
  { "en": "Apa Itu Flat Formation?", "id": "Susunan Tiga Kabel Fasa Sejajar." },
  { "en": "Apa Keuntungan Trefoil Formation?", "id": "Impedansi Seimbang Dan Tegangan Induksi Rendah." },
  { "en": "Apa Keuntungan Flat Formation?", "id": "Pendinginan Lebih Baik Tapi Impedansi Beda." },
  { "en": "Apa Itu Sheath Cross Bonding?", "id": "Menghilangkan Arus Sirkulasi Pelindung Kabel." },
  { "en": "Apa Itu Link Box Kabel?", "id": "Kotak Terminal Grounding Pelindung Kabel." },
  { "en": "Apa Itu Sheath Voltage Limiter?", "id": "Arrester Pelindung Tegangan Lebih Sheath." },
  { "en": "Apa Itu Impulse Test Kabel?", "id": "Uji Ketahanan Isolasi Tegangan Petir." },
  { "en": "Apa Itu Cycle Heating Test?", "id": "Uji Siklus Panas Dingin Kabel." },
  { "en": "Apa Itu Water Penetration Test?", "id": "Uji Ketahanan Kabel Terhadap Air." },
  { "en": "Apa Itu Fire Propagation Test?", "id": "Uji Perambatan Api Pada Kabel." },
  { "en": "Apa Itu Smoke Density Test?", "id": "Uji Kepadatan Asap Saat Terbakar." },
  { "en": "Apa Itu Acid Gas Test?", "id": "Uji Gas Asam Saat Kabel Terbakar." },
  { "en": "Apa Itu Oxygen Index Test?", "id": "Kadar Oksigen Minimum Bahan Terbakar." },
  { "en": "Apa Itu Generator Hydro Hydrogen?", "id": "Generator Pendingin Hidrogen Tekanan Tinggi." },
  { "en": "Apa Itu Stator Water Cooling?", "id": "Pendinginan Air Langsung Dalam Konduktor." },
  { "en": "Apa Itu Hydrogen Seal Oil?", "id": "Minyak Penyekat Gas Hidrogen Poros." },
  { "en": "Apa Itu Purging Gas Generator?", "id": "Mengganti Udara Dengan CO2 Lalu H2." },
  { "en": "Apa Itu Purity Monitor Hidrogen?", "id": "Alat Pantau Kemurnian Gas Pendingin." },
  { "en": "Apa Bahaya Hidrogen Tidak Murni?", "id": "Resiko Ledakan Dan Pemanasan Berlebih." },
  { "en": "Apa Itu Liquid Detector Generator?", "id": "Deteksi Kebocoran Air Atau Minyak." },
  { "en": "Apa Itu Core Monitor Generator?", "id": "Deteksi Partikel Overheating Isolasi Inti." },
  { "en": "Apa Itu Partial Discharge Coupler?", "id": "Kapasitor Deteksi PD Pada Terminal." },
  { "en": "Apa Itu Rotor Flux Probe?", "id": "Sensor Deteksi Short Turn Rotor." },
  { "en": "Apa Itu Shaft Voltage Monitor?", "id": "Pantau Tegangan Poros Cegah Bearing Rusak." },
  { "en": "Apa Itu End Winding Vibration?", "id": "Getaran Kepala Kumparan Stator Generator." },
  { "en": "Apa Itu Bump Test Generator?", "id": "Uji Respons Frekuensi Alami Stator." },
  { "en": "Apa Itu El-Cid Test?", "id": "Uji Kerusakan Isolasi Antar Laminasi Inti." },
  { "en": "Apa Itu Wedge Tightness Detector?", "id": "Alat Cek Kekencangan Pasak Slot Stator." },
  { "en": "Apa Itu Retaining Ring Rotor?", "id": "Cincin Penahan Lilitan Rotor Putaran Tinggi." },
  { "en": "Apa Itu Slip Ring Generator?", "id": "Cincin Kolektor Arus Medan Rotor." },
  { "en": "Apa Itu Brush Rigging?", "id": "Dudukan Pemegang Sikat Karbon Generator." },
  { "en": "Apa Itu Constant Pressure Spring?", "id": "Pegas Penekan Sikat Karbon Stabil." },
  { "en": "Apa Itu Brush Wear Monitor?", "id": "Alat Pantau Keausan Sikat Karbon." },
  { "en": "Apa Itu Excitation Transformer?", "id": "Trafo Suplai Daya Sistem Eksitasi." },
  { "en": "Apa Itu Field Flashing?", "id": "Memberi Arus Awal Sisa Magnet Hilang." },
  { "en": "Apa Itu De-Excitation Switch?", "id": "Saklar Pembuang Energi Medan Cepat." },
  { "en": "Apa Itu Field Discharge Resistor?", "id": "Resistor Penyerap Energi Medan Saat Trip." },
  { "en": "Apa Itu AVR Manual Channel?", "id": "Mode Kontrol Arus Medan Tetap." },
  { "en": "Apa Itu AVR Auto Channel?", "id": "Mode Kontrol Tegangan Terminal Tetap." },
  { "en": "Apa Itu Follower Mode AVR?", "id": "Kanal Manual Mengikuti Kanal Otomatis." },
  { "en": "Apa Itu Null Balance Meter?", "id": "Meter Penunjuk Selisih Auto Dan Manual." },
  { "en": "Apa Itu V/Hz Limiter AVR?", "id": "Pembatas Fluks Berlebih Pada Generator." },
  { "en": "Apa Itu OEL? (Over Excitation Limiter)", "id": "Pembatas Arus Medan Maksimum Termal." },
  { "en": "Apa Itu UEL Pada Generator?", "id": "Pembatas Eksitasi Rendah (Under Excitation)." },
  { "en": "Apa Fungsi UEL Generator?", "id": "Mencegah Panas Berlebih Ujung Stator." },
  { "en": "Apa Itu OEL Pada Generator?", "id": "Pembatas Eksitasi Lebih (Over Excitation)." },
  { "en": "Apa Fungsi OEL Generator?", "id": "Mencegah Panas Berlebih Lilitan Rotor." },
  { "en": "Apa Itu V/Hz Limiter?", "id": "Pembatas Rasio Tegangan Terhadap Frekuensi." },
  { "en": "Apa Fungsi V/Hz Limiter?", "id": "Mencegah Saturasi Inti Besi Generator." },
  { "en": "Apa Itu Konduktivitas Air Stator?", "id": "Ukuran Kemurnian Air Pendingin Stator." },
  { "en": "Apa Bahaya Konduktivitas Air Tinggi?", "id": "Arus Bocor Melalui Air Pendingin." },
  { "en": "Apa Itu Kemurnian Hidrogen Generator?", "id": "Persentase Gas Hidrogen Dalam Casing." },
  { "en": "Apa Itu Sistem Minyak Penyekat?", "id": "Minyak Penahan Gas Hidrogen Bocor." },
  { "en": "Apa Itu Kurva Kapabilitas Generator?", "id": "Batas Operasi Daya Aktif Reaktif." },
  { "en": "Apa Itu Reaksi Jangkar Generator?", "id": "Medan Stator Mengganggu Medan Rotor." },
  { "en": "Apa Itu Reaktansi Sumbu Langsung? (Xd)", "id": "Impedansi Arah Kutub Rotor Utama." },
  { "en": "Apa Itu Reaktansi Sumbu Kuadratur? (Xq)", "id": "Impedansi Arah Antar Kutub Rotor." },
  { "en": "Apa Itu Reaktansi Subtransien?", "id": "Impedansi Saat Awal Hubung Singkat." },
  { "en": "Apa Itu Reaktansi Transien?", "id": "Impedansi Beberapa Siklus Setelah Gangguan." },
  { "en": "Apa Itu Reaktansi Sinkron?", "id": "Impedansi Kondisi Operasi Stabil (Steady)." },
  { "en": "Apa Itu Short Circuit Ratio? (SCR)", "id": "Rasio Arus Medan Hubung Singkat." },
  { "en": "Apa Karakteristik Generator SCR Tinggi?", "id": "Ukuran Besar, Stabil, Harga Mahal." },
  { "en": "Apa Karakteristik Generator SCR Rendah?", "id": "Ukuran Kecil, Kurang Stabil, Murah." },
  { "en": "Apa Itu Kumparan Peredam? (Damper)", "id": "Meredam Osilasi Rotor (Hunting)." },
  { "en": "Apa Itu Pemanasan Urutan Negatif?", "id": "Arus Induksi Memanaskan Permukaan Rotor." },
  { "en": "Apa Itu Kapabilitas I2t Generator?", "id": "Batas Tahan Panas Rotor (Unbalance)." },
  { "en": "Apa Itu Kenaikan Suhu Kelas B?", "id": "Batas Kenaikan Suhu 80 Derajat." },
  { "en": "Apa Itu Sensor RTD Stator?", "id": "Detektor Suhu Berbasis Tahanan Logam." },
  { "en": "Apa Itu Sensor Termokopel?", "id": "Detektor Suhu Berbasis Beda Tegangan." },
  { "en": "Apa Itu Monitor Getaran Poros?", "id": "Mendeteksi Ketidakseimbangan Putaran Rotor." },
  { "en": "Apa Itu Flux Probe Rotor?", "id": "Sensor Deteksi Hubung Singkat Lilitan." },
  { "en": "Apa Itu Turning Gear?", "id": "Memutar Rotor Lambat Saat Pendinginan." },
  { "en": "Apa Fungsi Jacking Oil Pump?", "id": "Mengangkat Poros Dengan Tekanan Minyak." },
  { "en": "Apa Itu Pendingin Hidrogen?", "id": "Alat Tukar Panas Gas Ke Air." },
  { "en": "Apa Itu Trafo Eksitasi?", "id": "Trafo Suplai Daya Sistem Eksitasi." },
  { "en": "Apa Itu PMG Eksitasi?", "id": "Generator Magnet Permanen Suplai AVR." },
  { "en": "Apa Itu Sistem Eksitasi Statis?", "id": "Rectifier Dan Sikat Karbon Eksternal." },
  { "en": "Apa Itu Sistem Eksitasi Brushless?", "id": "Rectifier Dioda Berputar Di Poros." },
  { "en": "Apa Itu Kegagalan Dioda Berputar?", "id": "Dioda Putus Atau Hubung Singkat." },
  { "en": "Apa Indikasi Dioda Putus?", "id": "Riak Arus Medan Meningkat Tajam." },
  { "en": "Apa Itu Field Discharge Resistor?", "id": "Penyerap Energi Magnet Saat Trip." },
  { "en": "Apa Itu Field Breaker?", "id": "Pemutus Arus Medan Magnet Rotor." },
  { "en": "Apa Itu Crowbar Circuit Rotor?", "id": "Pelindung Tegangan Lebih Lilitan Rotor." },
  { "en": "Apa Itu De-Excitation?", "id": "Proses Menghilangkan Medan Magnet Cepat." },
  { "en": "Apa Itu Automatic Voltage Regulator? (AVR)", "id": "Pengatur Tegangan Terminal Generator Otomatis." },
  { "en": "Apa Itu Mode Manual AVR?", "id": "Mengatur Arus Medan Secara Langsung." },
  { "en": "Apa Itu Kompensasi Droop Reaktif?", "id": "Berbagi Beban Reaktif Generator Paralel." },
  { "en": "Apa Itu Kompensasi Arus Silang?", "id": "Metode Sharing Var Tanpa Jatuh Tegangan." },
  { "en": "Apa Itu Governor Turbin?", "id": "Pengatur Putaran Atau Daya Turbin." },
  { "en": "Apa Itu Load Limit Governor?", "id": "Batas Maksimum Bukaan Katup Uap." },
  { "en": "Apa Itu Free Governor Mode?", "id": "Respon Otomatis Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Dead Band Frekuensi?", "id": "Rentang Frekuensi Governor Tidak Merespon." },
  { "en": "Apa Itu Speed Droop Setting?", "id": "Persentase Turun Kecepatan Per Beban." },
  { "en": "Apa Itu Mode Isokron Governor?", "id": "Frekuensi Tetap Konstan Berapapun Beban." },
  { "en": "Apa Itu Cranking Motor?", "id": "Motor Pemutar Awal Turbin Gas." },
  { "en": "Apa Itu Purging Cycle?", "id": "Membuang Sisa Gas Sebelum Start." },
  { "en": "Apa Itu Jendela Sinkronisasi?", "id": "Batas Aman Parameter Menutup Breaker." },
  { "en": "Apa Itu Beda Sudut Fasa?", "id": "Selisih Sudut Tegangan Generator Grid." },
  { "en": "Apa Itu Frekuensi Slip Sinkron?", "id": "Selisih Frekuensi Generator Dan Grid." },
  { "en": "Apa Itu Breaker Closing Time?", "id": "Waktu Tunda Mekanis Menutup Kontak." },
  { "en": "Apa Itu Metode Lampu Gelap?", "id": "Lampu Padam Saat Fasa Sinkron." },
  { "en": "Apa Itu Metode Lampu Terang?", "id": "Lampu Nyala Terang Saat Sinkron." },
  { "en": "Apa Itu Metode Lampu Berputar?", "id": "Urutan Nyala Menunjukkan Kecepatan Relatif." },
  { "en": "Apa Itu Proteksi Frame Leakage?", "id": "Deteksi Hubung Singkat Ke Rangka." },
  { "en": "Apa Itu Oil Surge Relay?", "id": "Deteksi Aliran Minyak Cepat OLTC." },
  { "en": "Apa Itu Tank Ground Protection?", "id": "Proteksi Cadangan Arus Bocor Tangki." },
  { "en": "Apa Itu Neutral Grounding Resistor? (NGR)", "id": "Membatasi Arus Gangguan Fasa Tanah." },
  { "en": "Apa Itu Trafo Distribusi?", "id": "Menurunkan Tegangan Menengah Ke Rendah." },
  { "en": "Apa Itu Trafo Daya?", "id": "Trafo Kapasitas Besar Gardu Induk." },
  { "en": "Apa Itu Trafo Rectifier?", "id": "Trafo Khusus Untuk Penyearah Arus." },
  { "en": "Apa Itu Trafo Furnace?", "id": "Trafo Arus Sangat Besar Peleburan." },
  { "en": "Apa Itu Trafo Traksi?", "id": "Trafo Suplai Listrik Kereta Api." },
  { "en": "Apa Itu Optical Voltage Transducer?", "id": "Sensor Tegangan Menggunakan Efek Pockels." },
  { "en": "Apa Itu Optical Current Transducer?", "id": "Sensor Arus Menggunakan Efek Faraday." },
  { "en": "Apa Itu Merging Unit? (MU)", "id": "Antarmuka Analog Ke Digital IEC61850." },
  { "en": "Apa Itu Process Bus?", "id": "Jaringan Data Sampled Value Digital." },
  { "en": "Apa Itu Station Bus?", "id": "Jaringan Kontrol Dan Monitoring Digital." },
  { "en": "Apa Itu Standar IEC 62351?", "id": "Keamanan Siber Protokol Sistem Kuasa." },
  { "en": "Apa Itu Role Based Access?", "id": "Hak Akses Berdasarkan Peran User." },
  { "en": "Apa Itu Deep Packet Inspection?", "id": "Analisis Isi Paket Data Industri." },
  { "en": "Apa Itu Data Diode?", "id": "Perangkat Keras Komunikasi Satu Arah." },
  { "en": "Apa Itu Security Gateway?", "id": "Gerbang Akses Aman Jaringan Luar." },
  { "en": "Apa Itu Patch Management?", "id": "Proses Pembaruan Perangkat Lunak Aman." },
  { "en": "Apa Itu Kabel OPGW?", "id": "Kabel Tanah Berisi Serat Optik." },
  { "en": "Apa Itu Joint Box OPGW?", "id": "Kotak Sambungan Fiber Di Tower." },
  { "en": "Apa Itu Vibration Damper OPGW?", "id": "Peredam Getaran Khusus Kabel Optik." },
  { "en": "Apa Itu Suspension Unit OPGW?", "id": "Klem Gantung Khusus Kabel OPGW." },
  { "en": "Apa Itu Tension Unit OPGW?", "id": "Klem Tarik Khusus Kabel OPGW." },
  { "en": "Apa Itu Downlead Clamp?", "id": "Klem Kabel Turun Di Tower." },
  { "en": "Apa Itu Slack Loop OPGW?", "id": "Gulungan Sisa Kabel Di Tower." },
  { "en": "Apa Itu Fusion Splicer?", "id": "Alat Sambung Fiber Optik (Las)." },
  { "en": "Apa Itu Cleaver Fiber?", "id": "Alat Potong Rata Ujung Fiber." },
  { "en": "Apa Itu Protection Sleeve Fiber?", "id": "Pelindung Sambungan Las Serat Optik." },
  { "en": "Apa Itu Tray Fiber Optik?", "id": "Wadah Penyimpan Sambungan Fiber Optik." },
  { "en": "Apa Itu OTDR Trace?", "id": "Grafik Hasil Ukur Refleksi Optik." },
  { "en": "Apa Itu Dead Zone OTDR?", "id": "Jarak Awal Tidak Terbaca OTDR." },
  { "en": "Apa Itu Launch Cable OTDR?", "id": "Kabel Dummy Hilangkan Dead Zone." },
  { "en": "Apa Itu Optical Loss Test Set?", "id": "Alat Ukur Rugi Daya Optik." },
  { "en": "Apa Itu Return Loss Fiber?", "id": "Cahaya Pantul Balik Ke Sumber." },
  { "en": "Apa Itu Macrobending Fiber?", "id": "Tekukan Fisik Besar Kabel Fiber." },
  { "en": "Apa Itu Microbending Fiber?", "id": "Tekukan Kecil Pada Inti Fiber." },
  { "en": "Apa Itu Cut-Off Wavelength?", "id": "Panjang Gelombang Minimum Mode Tunggal." },
  { "en": "Apa Itu Mode Field Diameter?", "id": "Diameter Efektif Distribusi Cahaya Fiber." },
  { "en": "Apa Itu Dispersion Shifted Fiber?", "id": "Fiber Dispersi Nol Digeser." },
  { "en": "Apa Itu Non-Zero Dispersion Shifted?", "id": "Fiber Optimasi WDM Jarak Jauh." },
  { "en": "Apa Itu Polarization Mode Dispersion?", "id": "Dispersi Akibat Beda Kecepatan Polarisasi." },
  { "en": "Apa Itu Four Wave Mixing?", "id": "Intermodulasi Sinyal Cahaya WDM." },
  { "en": "Apa Itu Stimulated Brillouin Scattering?", "id": "Hamburan Cahaya Akibat Gelombang Akustik." },
  { "en": "Apa Itu Stimulated Raman Scattering?", "id": "Hamburan Cahaya Transfer Energi Gelombang." },
  { "en": "Apa Itu Optical Isolator?", "id": "Komponen Cahaya Satu Arah Saja." },
  { "en": "Apa Itu Optical Circulator?", "id": "Mengarahkan Cahaya Ke Port Berikutnya." },
  { "en": "Apa Itu Optical Switch?", "id": "Saklar Pemindah Jalur Cahaya." },
  { "en": "Apa Itu MEMS Optical Switch?", "id": "Saklar Optik Cermin Mikro Elektromekanis." },
  { "en": "Apa Itu Thermo-Optic Switch?", "id": "Saklar Optik Berbasis Perubahan Suhu." },
  { "en": "Apa Itu Electro-Optic Switch?", "id": "Saklar Optik Berbasis Medan Listrik." },
  { "en": "Apa Itu Attenuator Fiber Optic?", "id": "Alat Peredam Sinyal Cahaya." },
  { "en": "Apa Itu Variable Optical Attenuator?", "id": "Peredam Cahaya Nilai Bisa Diubah." },
  { "en": "Apa Itu Fixed Optical Attenuator?", "id": "Peredam Cahaya Nilai Tetap." },
  { "en": "Apa Itu Erbium Doped Fiber?", "id": "Fiber Penguat Sinyal Optik (Amplifier)." },
  { "en": "Apa Itu Pump Laser Diode?", "id": "Laser Pemberi Energi EDFA." },
  { "en": "Apa Itu Gain Flattening Filter?", "id": "Meratakan Penguatan Sinyal EDFA." },
  { "en": "Apa Itu Optical Add-Drop Multiplexer?", "id": "Menambah Atau Mengambil Sinyal WDM." },
  { "en": "Apa Itu ROADM?", "id": "OADM Bisa Diatur Jarak Jauh." },
  { "en": "Apa Itu Wavelength Selective Switch?", "id": "Saklar Pemilih Panjang Gelombang Spesifik." },
  { "en": "Apa Itu Transponder Optik?", "id": "Pengubah Sinyal Listrik Ke Optik." },
  { "en": "Apa Itu Muxponder Optik?", "id": "Menggabungkan Banyak Sinyal Ke Satu." },
  { "en": "Apa Itu Forward Error Correction?", "id": "Kode Koreksi Kesalahan Transmisi Data." },
  { "en": "Apa Itu Bit Error Rate Test?", "id": "Uji Tingkat Kesalahan Bit Data." },
  { "en": "Apa Itu Eye Diagram Analysis?", "id": "Analisis Kualitas Sinyal Digital Osiloskop." },
  { "en": "Apa Itu Jitter Sinyal Digital?", "id": "Variasi Waktu Kedatangan Sinyal." },
  { "en": "Apa Itu Wander Sinyal Digital?", "id": "Jitter Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Clock Recovery Circuit?", "id": "Mengambil Sinyal Detak Dari Data." },
  { "en": "Apa Itu Phase Locked Loop?", "id": "Rangkaian Sinkronisasi Fasa Sinyal." },
  { "en": "Apa Itu Voltage Controlled Oscillator?", "id": "Osilator Frekuensi Diatur Tegangan." },
  { "en": "Apa Itu Direct Digital Synthesis?", "id": "Pembangkit Sinyal Digital Presisi." },
  { "en": "Apa Itu Digital Signal Processor?", "id": "Prosesor Khusus Operasi Matematika Sinyal." },
  { "en": "Apa Itu Microcontroller Watchdog?", "id": "Reset Sistem Jika Program Macet." },
  { "en": "Apa Itu Brownout Detector?", "id": "Reset Sistem Jika Tegangan Turun." },
  { "en": "Apa Itu Power On Reset?", "id": "Reset Sistem Saat Awal Nyala." },
  { "en": "Apa Itu Decoupling Capacitor?", "id": "Kapasitor Stabilkan Tegangan Suplai Chip." },
  { "en": "Apa Itu Bypass Capacitor?", "id": "Mengalirkan Noise Frekuensi Tinggi Ground." },
  { "en": "Apa Itu Pull-Up Resistor?", "id": "Menahan Pin Input Ke Tegangan." },
  { "en": "Apa Itu Pull-Down Resistor?", "id": "Menahan Pin Input Ke Ground." },
  { "en": "Apa Itu Debounce Circuit?", "id": "Penghilang Getaran Mekanis Saklar." },
  { "en": "Apa Itu Schmitt Trigger Input?", "id": "Input Histeresis Tahan Noise." },
  { "en": "Apa Itu Open Drain Output?", "id": "Output Perlu Resistor Pull-Up Eksternal." },
  { "en": "Apa Itu Push-Pull Output?", "id": "Output Bisa Source Dan Sink." },
  { "en": "Apa Itu Tristate Output?", "id": "Output Bisa Kondisi Impedansi Tinggi." },
  { "en": "Apa Itu Pulse Width Modulation?", "id": "Modulasi Lebar Pulsa Kontrol Daya." },
  { "en": "Apa Itu Analog To Digital?", "id": "Mengubah Tegangan Menjadi Data Digital." },
  { "en": "Apa Itu Digital To Analog?", "id": "Mengubah Data Digital Menjadi Tegangan." },
  { "en": "Apa Itu Sampling Rate ADC?", "id": "Kecepatan Pengambilan Sampel Sinyal." },
  { "en": "Apa Itu Resolution ADC?", "id": "Ketelitian Nilai Konversi Digital." },
  { "en": "Apa Itu Quantization Error?", "id": "Kesalahan Pembulatan Nilai Digital." },
  { "en": "Apa Itu Aliasing Effect?", "id": "Sinyal Palsu Akibat Sampling Lambat." },
  { "en": "Apa Itu Anti-Aliasing Filter?", "id": "Filter Low Pass Sebelum ADC." },
  { "en": "Apa Itu Nyquist Theorem?", "id": "Sampling Minimal Dua Kali Frekuensi." },
  { "en": "Apa Itu SAR ADC?", "id": "Tipe ADC Paling Umum Mikrokontroler." },
  { "en": "Apa Itu Sigma-Delta ADC?", "id": "Tipe ADC Resolusi Tinggi Lambat." },
  { "en": "Apa Itu Flash ADC?", "id": "Tipe ADC Sangat Cepat Mahal." },
  { "en": "Apa Itu R-2R Ladder DAC?", "id": "Rangkaian Resistor Pengubah Digital Analog." },
  { "en": "Apa Itu Operational Amplifier?", "id": "Penguat Sinyal Analog Serbaguna." },
  { "en": "Apa Itu Inverting Amplifier?", "id": "Penguat Pembalik Fasa Sinyal." },
  { "en": "Apa Itu Non-Inverting Amplifier?", "id": "Penguat Tanpa Balik Fasa Sinyal." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat Penyangga Impedansi Tinggi." },
  { "en": "Apa Itu Comparator Circuit?", "id": "Pembanding Dua Level Tegangan." },
  { "en": "Apa Itu Differential Amplifier?", "id": "Penguat Selisih Dua Tegangan Input." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Itu Common Mode Rejection?", "id": "Kemampuan Menolak Noise Sinyal Bersama." },
  { "en": "Apa Itu Slew Rate Op-Amp?", "id": "Kecepatan Perubahan Tegangan Output." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Perkalian Penguatan Dan Lebar Pita." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Error Input Op-Amp." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Masuk Kaki Input Op-Amp." },
  { "en": "Apa Itu Virtual Ground?", "id": "Titik Referensi Semu Rangkaian Op-Amp." },
  { "en": "Apa Itu Active Filter?", "id": "Filter Menggunakan Op-Amp Dan RC." },
  { "en": "Apa Itu Passive Filter?", "id": "Filter Menggunakan RLC Tanpa Penguat." },
  { "en": "Apa Itu Low Pass Filter?", "id": "Meloloskan Frekuensi Rendah Saja." },
  { "en": "Apa Itu High Pass Filter?", "id": "Meloloskan Frekuensi Tinggi Saja." },
  { "en": "Apa Itu Band Pass Filter?", "id": "Meloloskan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Band Stop Filter?", "id": "Menahan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Notch Filter?", "id": "Menahan Satu Frekuensi Spesifik." },
  { "en": "Apa Itu Cut-Off Frequency?", "id": "Frekuensi Sinyal Turun 3 dB." },
  { "en": "Apa Itu Roll-Off Rate?", "id": "Kemiringan Kurva Redaman Filter." },
  { "en": "Apa Itu Butterworth Filter?", "id": "Filter Respon Frekuensi Datar." },
  { "en": "Apa Itu Chebyshev Filter?", "id": "Filter Redaman Curam Bergelombang." },
  { "en": "Apa Itu Bessel Filter?", "id": "Filter Respon Fasa Linier." },
  { "en": "Apa Itu Elliptic Filter?", "id": "Filter Redaman Paling Curam." },
  { "en": "Apa Itu Quality Factor Q?", "id": "Ukuran Selektivitas Filter Resonansi." },
  { "en": "Apa Itu Damping Factor?", "id": "Ukuran Redaman Osilasi Sistem." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Sistem Umpan Balik." },
  { "en": "Apa Itu Gain Margin?", "id": "Batas Penguatan Sebelum Tidak Stabil." },
  { "en": "Apa Itu Bode Plot?", "id": "Grafik Respon Frekuensi Dan Fasa." },
  { "en": "Apa Itu Root Locus?", "id": "Grafik Lokasi Pole Sistem Kontrol." },
  { "en": "Apa Itu PID Controller?", "id": "Kontroler Proporsional Integral Derivatif." },
  { "en": "Apa Itu Proportional Term?", "id": "Respon Terhadap Besarnya Error Saat Ini." },
  { "en": "Apa Itu Integral Term?", "id": "Respon Terhadap Akumulasi Error Lalu." },
  { "en": "Apa Itu Derivative Term PID?", "id": "Respon Terhadap Laju Perubahan Error." },
  { "en": "Apa Itu Tuning PID?", "id": "Proses Mencari Parameter Pengendali Optimal." },
  { "en": "Apa Itu Metode Ziegler Nichols?", "id": "Metode Tuning PID Eksperimental Klasik." },
  { "en": "Apa Itu Setpoint Variable?", "id": "Nilai Target Yang Diinginkan Sistem." },
  { "en": "Apa Itu Process Variable?", "id": "Nilai Aktual Terukur Dari Sensor." },
  { "en": "Apa Itu Control Variable?", "id": "Sinyal Keluaran Pengendali Ke Aktuator." },
  { "en": "Apa Itu Feedback Loop?", "id": "Sinyal Balik Dari Output Ke Input." },
  { "en": "Apa Itu Open Loop Control?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Closed Loop Control?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Steady State Error?", "id": "Selisih Target Dan Aktual Saat Stabil." },
  { "en": "Apa Itu Overshoot Response?", "id": "Lonjakan Sinyal Melebihi Target Sesaat." },
  { "en": "Apa Itu Settling Time?", "id": "Waktu Mencapai Kondisi Stabil." },
  { "en": "Apa Itu Rise Time?", "id": "Waktu Sinyal Naik Mencapai Target." },
  { "en": "Apa Itu Relay Ladder Logic?", "id": "Bahasa Pemrograman Grafis Standar PLC." },
  { "en": "Apa Itu Normally Open Contact?", "id": "Kontak Terbuka Saat Tidak Aktif." },
  { "en": "Apa Itu Normally Closed Contact?", "id": "Kontak Tertutup Saat Tidak Aktif." },
  { "en": "Apa Itu Holding Circuit?", "id": "Rangkaian Pengunci Relay Otomatis." },
  { "en": "Apa Itu Interlock Logic?", "id": "Syarat Logika Mencegah Operasi Terlarang." },
  { "en": "Apa Itu Timer On Delay?", "id": "Menunda Aktif Setelah Sinyal Masuk." },
  { "en": "Apa Itu Timer Off Delay?", "id": "Menunda Mati Setelah Sinyal Hilang." },
  { "en": "Apa Itu Counter Up?", "id": "Menghitung Naik Setiap Pulsa Masuk." },
  { "en": "Apa Itu Counter Down?", "id": "Menghitung Turun Setiap Pulsa Masuk." },
  { "en": "Apa Itu PLC Scan Time?", "id": "Waktu Siklus Proses Program PLC." },
  { "en": "Apa Itu Watchdog Timer PLC?", "id": "Deteksi Kemacetan Program PLC." },
  { "en": "Apa Itu HMI Screen?", "id": "Layar Sentuh Antarmuka Operator Mesin." },
  { "en": "Apa Itu SCADA Master Station?", "id": "Pusat Kontrol Pengumpul Data Lapangan." },
  { "en": "Apa Itu RTU Remote Terminal?", "id": "Unit Terminal Data Di Lapangan." },
  { "en": "Apa Itu Communication Protocol Modbus?", "id": "Protokol Serial Standar Industri Terbuka." },
  { "en": "Apa Itu Modbus TCP/IP?", "id": "Modbus Via Jaringan Ethernet." },
  { "en": "Apa Itu Modbus RTU?", "id": "Modbus Via Serial RS485." },
  { "en": "Apa Itu DNP3 Protocol?", "id": "Protokol Telemetri Utilitas Listrik." },
  { "en": "Apa Itu IEC 60870-5-104?", "id": "Protokol Telecontrol Via TCP/IP." },
  { "en": "Apa Itu IEC 61850 Standard?", "id": "Standar Komunikasi Gardu Induk Digital." },
  { "en": "Apa Itu GOOSE Message?", "id": "Sinyal Cepat Antar Relay Proteksi." },
  { "en": "Apa Itu MMS Protocol?", "id": "Komunikasi Data Ke SCADA Pusat." },
  { "en": "Apa Itu Sampled Values?", "id": "Data Digital Arus Tegangan Real-Time." },
  { "en": "Apa Itu Merging Unit?", "id": "Pengubah Analog Ke Digital IEC61850." },
  { "en": "Apa Itu Ethernet Switch Industrial?", "id": "Switch Jaringan Tahan Lingkungan Kasar." },
  { "en": "Apa Itu Fiber Optic Ring?", "id": "Topologi Jaringan Cincin Redundan." },
  { "en": "Apa Itu Cyber Security Grid?", "id": "Perlindungan Sistem Dari Serangan Siber." },
  { "en": "Apa Itu Firewall Industrial?", "id": "Penyaring Paket Data Jaringan Kontrol." },
  { "en": "Apa Itu Air Gap Security?", "id": "Isolasi Fisik Jaringan Dari Internet." },
  { "en": "Apa Itu Relay Differential Trafo?", "id": "Membandingkan Arus Primer Dan Sekunder." },
  { "en": "Apa Itu Relay Buchholz Trafo?", "id": "Deteksi Gas Akibat Gangguan Internal." },
  { "en": "Apa Itu Relay Jansen Tap Changer?", "id": "Deteksi Tekanan Minyak Tap Changer." },
  { "en": "Apa Itu Relay Sudden Pressure?", "id": "Deteksi Kenaikan Tekanan Tangki Cepat." },
  { "en": "Apa Itu Relay Thermal Overload?", "id": "Proteksi Panas Berlebih Pada Lilitan." },
  { "en": "Apa Itu Relay Overcurrent Instant?", "id": "Proteksi Arus Lebih Tanpa Tunda." },
  { "en": "Apa Itu Relay Overcurrent Time?", "id": "Proteksi Arus Lebih Dengan Waktu." },
  { "en": "Apa Itu Relay Earth Fault?", "id": "Proteksi Gangguan Hubung Tanah." },
  { "en": "Apa Itu Relay Standby Earth Fault?", "id": "Cadangan Proteksi Gangguan Tanah." },
  { "en": "Apa Itu Relay Restricted Earth Fault?", "id": "Proteksi Tanah Zona Terbatas Trafo." },
  { "en": "Apa Itu Relay Negative Sequence?", "id": "Proteksi Beban Tidak Seimbang." },
  { "en": "Apa Itu Relay Under Voltage?", "id": "Proteksi Tegangan Turun Dibawah Batas." },
  { "en": "Apa Itu Relay Over Voltage?", "id": "Proteksi Tegangan Naik Diatas Batas." },
  { "en": "Apa Itu Relay Frequency?", "id": "Proteksi Penyimpangan Frekuensi Sistem." },
  { "en": "Apa Itu Relay Reverse Power?", "id": "Mencegah Daya Balik Ke Generator." },
  { "en": "Apa Itu Relay Loss of Excitation?", "id": "Proteksi Hilang Medan Magnet Rotor." },
  { "en": "Apa Itu Relay Out of Step?", "id": "Proteksi Lepas Sinkron Generator." },
  { "en": "Apa Itu Relay Vector Shift?", "id": "Deteksi Perubahan Sudut Fasa Mendadak." },
  { "en": "Apa Itu Relay ROCOF?", "id": "Deteksi Laju Perubahan Frekuensi." },
  { "en": "Apa Itu Auto Recloser?", "id": "Penutup Balik Otomatis Saluran Udara." },
  { "en": "Apa Itu Sectionalizer?", "id": "Pemisah Seksi Jaringan Saat Mati." },
  { "en": "Apa Itu Fault Indicator?", "id": "Penunjuk Lokasi Gangguan Kabel Tanah." },
  { "en": "Apa Itu Fuse Cut Out?", "id": "Sekring Lebur Pelindung Trafo Tiang." },
  { "en": "Apa Itu Lightning Arrester?", "id": "Pelindung Surja Petir Tegangan Tinggi." },
  { "en": "Apa Itu Disconnecting Switch?", "id": "Pemisah Rangkaian Tanpa Beban." },
  { "en": "Apa Itu Load Break Switch?", "id": "Pemutus Rangkaian Berbeban Nominal." },
  { "en": "Apa Itu Circuit Breaker?", "id": "Pemutus Rangkaian Arus Gangguan Besar." },
  { "en": "Apa Itu Vacuum Circuit Breaker?", "id": "Pemutus Arus Dalam Ruang Hampa." },
  { "en": "Apa Itu SF6 Circuit Breaker?", "id": "Pemutus Arus Media Gas SF6." },
  { "en": "Apa Itu Air Blast Breaker?", "id": "Pemutus Arus Media Udara Tekan." },
  { "en": "Apa Itu Oil Circuit Breaker?", "id": "Pemutus Arus Media Minyak Isolasi." },
  { "en": "Apa Itu Minimum Oil Breaker?", "id": "Pemutus Minyak Volume Sedikit." },
  { "en": "Apa Itu Operating Mechanism Breaker?", "id": "Penggerak Kontak Buka Tutup Breaker." },
  { "en": "Apa Itu Spring Charge Motor?", "id": "Motor Pengisi Pegas Energi Breaker." },
  { "en": "Apa Itu Trip Coil?", "id": "Kumparan Pemicu Buka Breaker." },
  { "en": "Apa Itu Closing Coil?", "id": "Kumparan Pemicu Tutup Breaker." },
  { "en": "Apa Itu Anti Pumping Relay?", "id": "Mencegah Breaker Tutup Buka Berulang." },
  { "en": "Apa Itu Lockout Relay?", "id": "Mengunci Breaker Setelah Trip Gangguan." },
  { "en": "Apa Itu Trip Circuit Supervision?", "id": "Monitor Kesiapan Rangkaian Trip Coil." },
  { "en": "Apa Itu DC Supply Supervision?", "id": "Monitor Tegangan Baterai Kontrol." },
  { "en": "Apa Itu CT Polarity?", "id": "Arah Aliran Arus Trafo Arus." },
  { "en": "Apa Itu CT Ratio?", "id": "Perbandingan Arus Primer Dan Sekunder." },
  { "en": "Apa Itu CT Burden?", "id": "Beban Impedansi Rangkaian Sekunder CT." },
  { "en": "Apa Itu CT Saturation?", "id": "Kondisi Inti Besi CT Jenuh." },
  { "en": "Apa Itu CT Class P?", "id": "Kelas Proteksi Trafo Arus." },
  { "en": "Apa Itu CT Class 0.5?", "id": "Kelas Pengukuran Energi Akurasi Tinggi." },
  { "en": "Apa Itu Knee Point Voltage?", "id": "Titik Jenuh Kurva Eksitasi CT." },
  { "en": "Apa Itu PT Ratio?", "id": "Perbandingan Tegangan Primer Sekunder." },
  { "en": "Apa Itu PT Burden?", "id": "Beban Impedansi Rangkaian Sekunder PT." },
  { "en": "Apa Itu Capacitive Voltage Transformer?", "id": "Trafo Tegangan Pembagi Kapasitor." },
  { "en": "Apa Itu Inductive Voltage Transformer?", "id": "Trafo Tegangan Kumparan Magnetik." },
  { "en": "Apa Itu Ferroresonance?", "id": "Osilasi Resonansi Inti Besi Kapasitor." },
  { "en": "Apa Itu Power Factor Correction?", "id": "Perbaikan Faktor Daya Kapasitor Bank." },
  { "en": "Apa Itu Automatic Power Factor Controller?", "id": "Pengendali Langkah Kapasitor Otomatis." },
  { "en": "Apa Itu Capacitor Bank Step?", "id": "Tahapan Daya Reaktif Kapasitor." },
  { "en": "Apa Itu Inrush Current Capacitor?", "id": "Lonjakan Arus Saat Kapasitor Masuk." },
  { "en": "Apa Itu Detuned Reactor?", "id": "Reaktor Penahan Harmonisa Kapasitor." },
  { "en": "Apa Itu Harmonic Filter?", "id": "Penyaring Frekuensi Harmonisa Spesifik." },
  { "en": "Apa Itu Active Power Filter?", "id": "Filter Harmonisa Elektronik Umpan Balik." },
  { "en": "Apa Itu Passive Power Filter?", "id": "Filter Harmonisa Komponen RLC Pasif." },
  { "en": "Apa Itu Hybrid Power Filter?", "id": "Gabungan Filter Aktif Dan Pasif." },
  { "en": "Apa Itu Tuned Filter?", "id": "Filter Pasif Disetel Frekuensi Tertentu." },
  { "en": "Apa Itu Damped Filter?", "id": "Filter Pasif Respon Frekuensi Lebar." },
  { "en": "Apa Itu Ripple Control Signal?", "id": "Sinyal Kontrol Frekuensi Audio Jaringan." },
  { "en": "Apa Itu Flicker Emission?", "id": "Emisi Kedipan Tegangan Dari Beban." },
  { "en": "Apa Itu Commutation Notch?", "id": "Tegangan Nol Saat Thyristor Pindah." },
  { "en": "Apa Itu IEEE 519 Standard?", "id": "Standar Harmonisa Sistem Tenaga Listrik." },
  { "en": "Apa Itu IEC 61000-3-2?", "id": "Standar Emisi Harmonisa Peralatan Kecil." },
  { "en": "Apa Itu IEC 61000-3-12?", "id": "Standar Emisi Harmonisa Peralatan Besar." },
  { "en": "Apa Itu Point of Common Coupling? (PCC)", "id": "Titik Sambung Pelanggan Dan Jaringan." },
  { "en": "Apa Itu Short Circuit Ratio? (SCR)", "id": "Rasio Kekuatan Hubung Singkat PCC." },
  { "en": "Apa Itu Weak Grid?", "id": "Jaringan Dengan Impedansi Hubung Tinggi." },
  { "en": "Apa Itu Strong Grid?", "id": "Jaringan Dengan Impedansi Hubung Rendah." },
  { "en": "Apa Itu Resonansi Paralel Sistem?", "id": "Impedansi Sistem Sangat Tinggi Harmonisa." },
  { "en": "Apa Itu Resonansi Seri Sistem?", "id": "Impedansi Sistem Sangat Rendah Harmonisa." },
  { "en": "Apa Itu Harmonic Power Flow?", "id": "Aliran Daya Pada Frekuensi Harmonisa." },
  { "en": "Apa Itu Triplen Harmonics?", "id": "Harmonisa Kelipatan Tiga (Urutan Nol)." },
  { "en": "Apa Itu Sequence Component Harmonisa?", "id": "Urutan Fasa Positif Negatif Nol." },
  { "en": "Apa Itu K-Factor Transformer?", "id": "Trafo Tahan Panas Arus Harmonisa." },
  { "en": "Apa Itu Phase Shifting Transformer?", "id": "Trafo Penggeser Fasa Pembatal Harmonisa." },
  { "en": "Apa Itu Zigzag Transformer Filter?", "id": "Trafo Pemblokir Harmonisa Urutan Nol." },
  { "en": "Apa Itu 12-Pulse Rectifier?", "id": "Penyearah Pembatal Harmonisa Orde Rendah." },
  { "en": "Apa Itu 18-Pulse Rectifier?", "id": "Penyearah Kualitas Daya Sangat Tinggi." },
  { "en": "Apa Itu Active Front End? (AFE)", "id": "Penyearah IGBT Sinusoidal Faktor Daya Satu." },
  { "en": "Apa Itu Power Factor Displacement?", "id": "Faktor Daya Akibat Beda Fasa." },
  { "en": "Apa Itu Power Factor Distortion?", "id": "Faktor Daya Akibat Bentuk Gelombang." },
  { "en": "Apa Itu True Power Factor?", "id": "Gabungan Faktor Daya Displacement Distorsi." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Nilai Puncak Terhadap RMS." },
  { "en": "Apa Itu Form Factor?", "id": "Rasio Nilai RMS Terhadap Rata-Rata." },
  { "en": "Apa Itu Skin Effect Harmonic?", "id": "Kenaikan Resistansi Kabel Frekuensi Tinggi." },
  { "en": "Apa Itu Transformer Derating?", "id": "Penurunan Kapasitas Trafo Akibat Harmonisa." },
  { "en": "Apa Itu Capacitor Derating?", "id": "Penurunan Kapasitas Kapasitor Akibat Harmonisa." },
  { "en": "Apa Itu Cable Derating?", "id": "Penurunan Kapasitas Kabel Akibat Panas." },
  { "en": "Apa Itu Motor Derating?", "id": "Penurunan Daya Motor Akibat Harmonisa." },
  { "en": "Apa Itu Generator Derating?", "id": "Penurunan Daya Generator Akibat Pemanasan." },
  { "en": "Apa Itu Harmonic Spectrum?", "id": "Grafik Amplitudo Harmonisa Domain Frekuensi." },
  { "en": "Apa Itu Fast Fourier Transform? (FFT)", "id": "Algoritma Analisis Spektrum Sinyal Digital." },
  { "en": "Apa Itu Sampling Window?", "id": "Durasi Waktu Pengambilan Sampel Sinyal." },
  { "en": "Apa Itu Aliasing Frequency?", "id": "Frekuensi Palsu Akibat Sampling Rendah." },
  { "en": "Apa Itu Nyquist Frequency?", "id": "Setengah Dari Frekuensi Sampling ADC." },
  { "en": "Apa Itu Anti-Aliasing Filter?", "id": "Filter Low Pass Sebelum Sampling." },
  { "en": "Apa Itu Subharmonics?", "id": "Frekuensi Di Bawah Frekuensi Fundamental." },
  { "en": "Apa Itu Voltage Sag?", "id": "Penurunan Tegangan Sesaat Durasi Pendek." },
  { "en": "Apa Itu Voltage Swell?", "id": "Kenaikan Tegangan Sesaat Durasi Pendek." },
  { "en": "Apa Itu Voltage Interruption?", "id": "Hilangnya Tegangan Total Sesaat." },
  { "en": "Apa Itu Transient Overvoltage?", "id": "Lonjakan Tegangan Sangat Cepat." },
  { "en": "Apa Itu Impulsive Transient?", "id": "Transien Satu Arah (Seperti Petir)." },
  { "en": "Apa Itu Oscillatory Transient?", "id": "Transien Bolak-Balik Frekuensi Tinggi." },
  { "en": "Apa Itu Voltage Fluctuation?", "id": "Perubahan Tegangan Terus Menerus (Flicker)." },
  { "en": "Apa Itu Short Term Flicker? (Pst)", "id": "Indeks Flicker Sepuluh Menit." },
  { "en": "Apa Itu Long Term Flicker? (Plt)", "id": "Indeks Flicker Dua Jam." },
  { "en": "Apa Itu Voltage Unbalance?", "id": "Ketidakseimbangan Magnitudo Atau Sudut Fasa." },
  { "en": "Apa Itu Frequency Variation?", "id": "Penyimpangan Frekuensi Dari Nilai Nominal." },
  { "en": "Apa Itu CBEMA Curve?", "id": "Kurva Toleransi Tegangan Peralatan IT." },
  { "en": "Apa Itu ITIC Curve?", "id": "Kurva Pengganti CBEMA Lebih Baru." },
  { "en": "Apa Itu SEMI F47?", "id": "Standar Sag Industri Semikonduktor." },
  { "en": "Apa Itu Power Quality Analyzer?", "id": "Alat Ukur Kualitas Daya Listrik." },
  { "en": "Apa Itu Class A PQ Meter?", "id": "Meter Kualitas Daya Akurasi Tertinggi." },
  { "en": "Apa Itu Class S PQ Meter?", "id": "Meter Kualitas Daya Akurasi Standar." },
  { "en": "Apa Itu Dip/Swell Trigger?", "id": "Batas Pemicu Perekaman Kejadian PQ." },
  { "en": "Apa Itu Waveform Capture?", "id": "Perekaman Bentuk Gelombang Saat Trigger." },
  { "en": "Apa Itu Aggregation Interval?", "id": "Periode Penggabungan Data Ukur PQ." },
  { "en": "Apa Itu Flagging Concept?", "id": "Menandai Data Ukur Tidak Valid." },
  { "en": "Apa Itu Voltage Ride Through?", "id": "Kemampuan Alat Bertahan Saat Sag." },
  { "en": "Apa Itu Dynamic Voltage Restorer? (DVR)", "id": "Inverter Seri Kompensasi Tegangan Sag." },
  { "en": "Apa Itu UPS Online?", "id": "UPS Konversi Ganda Terus Menerus." },
  { "en": "Apa Itu UPS Line Interactive?", "id": "UPS Dengan Trafo Regulator Tegangan." },
  { "en": "Apa Itu UPS Offline?", "id": "UPS Hanya Aktif Saat Padam." },
  { "en": "Apa Itu Flywheel UPS?", "id": "UPS Penyimpan Energi Roda Gila." },
  { "en": "Apa Itu Isolation Transformer?", "id": "Trafo Pemisah Grounding Dan Noise." },
  { "en": "Apa Itu Noise Cut Transformer?", "id": "Trafo Peredam Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Surge Protection Device? (SPD)", "id": "Alat Pelindung Lonjakan Tegangan." },
  { "en": "Apa Itu Metal Oxide Varistor? (MOV)", "id": "Komponen Utama Penahan Surja." },
  { "en": "Apa Itu Gas Discharge Tube? (GDT)", "id": "Tabung Gas Pembuang Arus Petir." },
  { "en": "Apa Itu Silicon Avalanche Diode? (SAD)", "id": "Dioda Pelindung Surja Sangat Cepat." },
  { "en": "Apa Itu Clamping Voltage?", "id": "Tegangan Maksimum Yang Diizinkan SPD." },
  { "en": "Apa Itu Let-Through Voltage?", "id": "Tegangan Sisa Setelah Diredam SPD." },
  { "en": "Apa Itu Surge Current Rating?", "id": "Kapasitas Arus Maksimum SPD." },
  { "en": "Apa Itu Joule Rating SPD?", "id": "Energi Maksimum Yang Diserap SPD." },
  { "en": "Apa Itu Cascaded Surge Protection?", "id": "Proteksi Surja Bertingkat (Zona)." },
  { "en": "Apa Itu Grounding Bonding?", "id": "Menyambung Semua Ground Jadi Satu." },
  { "en": "Apa Itu Equipotential Plane?", "id": "Area Kerja Dengan Tegangan Seragam." },
  { "en": "Apa Itu Isolated Ground?", "id": "Grounding Khusus Terpisah Dari Panel." },
  { "en": "Apa Itu Signal Reference Grid?", "id": "Grid Grounding Frekuensi Tinggi." },
  { "en": "Apa Itu Ground Loop?", "id": "Arus Sirkulasi Antar Titik Ground." },
  { "en": "Apa Itu Common Mode Noise?", "id": "Noise Pada Kedua Kabel Fasa-Netral." },
  { "en": "Apa Itu Normal Mode Noise?", "id": "Noise Antara Kabel Fasa Dan Netral." },
  { "en": "Apa Itu Electromagnetic Interference? (EMI)", "id": "Gangguan Sinyal Elektromagnetik Luar." },
  { "en": "Apa Itu Radio Frequency Interference? (RFI)", "id": "Gangguan Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Permissive Underreach Transfer Trip? (PUTT)", "id": "Trip Diizinkan Sinyal Zona 1 Lawan." },
  { "en": "Apa Itu Permissive Overreach Transfer Trip? (POTT)", "id": "Trip Diizinkan Sinyal Zona 2 Lawan." },
  { "en": "Apa Itu Direct Underreach Transfer Trip? (DUTT)", "id": "Trip Langsung Sinyal Zona 1 Lawan." },
  { "en": "Apa Itu Blocking Scheme Protection?", "id": "Trip Diblokir Jika Lawan Deteksi Gangguan." },
  { "en": "Apa Itu Weak Infeed Logic?", "id": "Logika Trip Saat Sumber Arus Lemah." },
  { "en": "Apa Itu Echo Logic Protection?", "id": "Memantulkan Sinyal Trip Ke Gardu Pengirim." },
  { "en": "Apa Itu Power Swing Blocking?", "id": "Mencegah Trip Saat Ayunan Daya Sistem." },
  { "en": "Apa Itu Out of Step Tripping?", "id": "Trip Terkontrol Saat Generator Lepas Sinkron." },
  { "en": "Apa Itu Stub Bus Protection?", "id": "Proteksi Area Buta Antara CT Breaker." },
  { "en": "Apa Itu Capacitive Voltage Transformer? (CVT)", "id": "Trafo Tegangan Menggunakan Pembagi Kapasitor." },
  { "en": "Apa Itu Ferroresonance Suppression Circuit?", "id": "Rangkaian Peredam Osilasi Trafo Tegangan." },
  { "en": "Apa Itu Carrier Coupling Capacitor?", "id": "Kapasitor Penyuntik Sinyal Komunikasi PLC." },
  { "en": "Apa Itu Wave Trap Tuning?", "id": "Penyetelan Frekuensi Blokir Perangkap Gelombang." },
  { "en": "Apa Itu Line Matching Unit? (LMU)", "id": "Penyesuai Impedansi Sinyal PLC Dan Kabel." },
  { "en": "Apa Itu Grading Capacitor Breaker?", "id": "Kapasitor Pemerata Tegangan Antar Kontak." },
  { "en": "Apa Itu Pre-Insertion Resistor?", "id": "Resistor Peredam Arus Awal Menutup Breaker." },
  { "en": "Apa Itu Closing Resistor Breaker?", "id": "Resistor Seri Saat Proses Menutup Kontak." },
  { "en": "Apa Itu Gas Density Monitor?", "id": "Alat Pantau Kerapatan Gas SF6." },
  { "en": "Apa Itu Vacuum Tap Changer?", "id": "Pengubah Tap Menggunakan Botol Vakum." },
  { "en": "Apa Itu Resistor Transition Diverter?", "id": "Diverter Switch Menggunakan Resistor Transisi." },
  { "en": "Apa Itu Reactor Transition Diverter?", "id": "Diverter Switch Menggunakan Reaktor Transisi." },
  { "en": "Apa Itu Tie-in Resistor?", "id": "Resistor Pengikat Potensial Tap Changer." },
  { "en": "Apa Itu Tap Selector Switch?", "id": "Saklar Pemilih Tap Tanpa Beban." },
  { "en": "Apa Itu Diverter Switch Mechanism?", "id": "Mekanisme Pindah Beban Cepat Tap Changer." },
  { "en": "Apa Itu Motor Drive Unit? (MDU)", "id": "Motor Penggerak Mekanik Tap Changer." },
  { "en": "Apa Itu Step-by-Step Mechanism?", "id": "Mekanisme Pindah Satu Tap Per Operasi." },
  { "en": "Apa Itu Tap Position Indicator?", "id": "Penunjuk Posisi Tap Trafo Saat Ini." },
  { "en": "Apa Itu Remote Tap Changer Control?", "id": "Panel Kontrol Tap Changer Jarak Jauh." },
  { "en": "Apa Itu Sector Shaped Conductor?", "id": "Konduktor Kabel Bentuk Juring Lingkaran." },
  { "en": "Apa Itu Milliken Conductor?", "id": "Konduktor Segmen Terisolasi Cegah Skin Effect." },
  { "en": "Apa Itu Water Blocking Tape?", "id": "Pita Penyerap Air Dalam Kabel." },
  { "en": "Apa Itu Semiconducting Glaze?", "id": "Lapisan Glasir Konduktif Pada Isolator." },
  { "en": "Apa Itu Lead Alloy Sheath?", "id": "Pelindung Timbal Tahan Korosi Kabel." },
  { "en": "Apa Itu Corrugated Aluminum Sheath?", "id": "Pelindung Aluminium Bergelombang Kabel HV." },
  { "en": "Apa Itu HDPE Outer Sheath?", "id": "Jaket Luar Kabel Polietilena Keras." },
  { "en": "Apa Itu Graphite Coating Cable?", "id": "Lapisan Grafit Uji Jaket Kabel." },
  { "en": "Apa Itu Cable Cleat Spacing?", "id": "Jarak Antar Klem Penahan Kabel." },
  { "en": "Apa Itu Trefoil Cable Formation?", "id": "Susunan Tiga Kabel Bentuk Segitiga." },
  { "en": "Apa Itu Step Voltage Potential?", "id": "Beda Potensial Antara Dua Kaki." },
  { "en": "Apa Itu Touch Voltage Potential?", "id": "Beda Potensial Antara Tangan Dan Kaki." },
  { "en": "Apa Itu Mesh Voltage Potential?", "id": "Tegangan Sentuh Maksimum Dalam Grid." },
  { "en": "Apa Itu Transferred Potential?", "id": "Tegangan Tanah Pindah Lewat Konduktor." },
  { "en": "Apa Itu Ground Potential Rise?", "id": "Kenaikan Tegangan Tanah Saat Gangguan." },
  { "en": "Apa Itu Soil Ionization?", "id": "Tanah Menjadi Konduktif Saat Arus Tinggi." },
  { "en": "Apa Itu Counterpoise Grounding?", "id": "Kawat Tanah Paralel Transmisi Resistivitas Tinggi." },
  { "en": "Apa Itu Bentonite Grounding?", "id": "Lempung Penurun Tahanan Jenis Tanah." },
  { "en": "Apa Itu Chemical Ground Rod?", "id": "Elektroda Pipa Berisi Garam Kimia." },
  { "en": "Apa Itu Voltage Dip Duration?", "id": "Lama Waktu Tegangan Turun Sesaat." },
  { "en": "Apa Itu Voltage Swell Magnitude?", "id": "Besarnya Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Transient Recovery Voltage? (TRV)", "id": "Tegangan Transien Saat Kontak Membuka." },
  { "en": "Apa Itu Rate of Rise TRV?", "id": "Kecepatan Kenaikan Tegangan Pulih Transien." },
  { "en": "Apa Itu Chopped Wave Impulse?", "id": "Gelombang Petir Terpotong Flashover." },
  { "en": "Apa Itu Full Wave Impulse?", "id": "Gelombang Petir Standar Tanpa Potongan." },
  { "en": "Apa Itu Switching Impulse Level?", "id": "Level Ketahanan Terhadap Surja Hubung." },
  { "en": "Apa Itu Basic Impulse Level? (BIL)", "id": "Level Ketahanan Dasar Terhadap Petir." },
  { "en": "Apa Itu Power Frequency Withstand?", "id": "Tahan Tegangan Frekuensi Kerja Semenit." },
  { "en": "Apa Itu Wet Power Frequency Test?", "id": "Uji Tahan Tegangan Kondisi Basah." },
  { "en": "Apa Itu Slip Recovery System?", "id": "Sistem Pengembalian Energi Slip Rotor." },
  { "en": "Apa Itu Doubly Fed Induction Generator?", "id": "Generator Rotor Dan Stator Terhubung Grid." },
  { "en": "Apa Itu Wound Rotor Motor?", "id": "Motor Induksi Rotor Belitan Tembaga." },
  { "en": "Apa Itu Liquid Resistance Starter?", "id": "Starter Motor Menggunakan Larutan Elektrolit." },
  { "en": "Apa Itu Grid Rotor Resistance?", "id": "Resistor Logam Pengendali Kecepatan Rotor." },
  { "en": "Apa Itu Shorting Ring Rotor?", "id": "Cincin Penghubung Singkat Batang Rotor." },
  { "en": "Apa Itu Double Cage Rotor?", "id": "Rotor Sangkar Ganda Torsi Start Tinggi." },
  { "en": "Apa Itu Skewed Rotor Slot?", "id": "Alur Rotor Miring Kurangi Dengung." },
  { "en": "Apa Itu Magnetic Locking Motor?", "id": "Rotor Terkunci Akibat Harmoni Slot." },
  { "en": "Apa Itu Remote Terminal Unit? (RTU)", "id": "Perangkat Pengumpul Data Lapangan SCADA." },
  { "en": "Apa Itu Human Machine Interface? (HMI)", "id": "Layar Kendali Operator Sistem Otomasi." },
  { "en": "Apa Itu Sequence of Events? (SOE)", "id": "Rekaman Kronologis Kejadian Sangat Presisi." },
  { "en": "Apa Itu Disturbance Fault Recorder?", "id": "Perekam Bentuk Gelombang Saat Gangguan." },
  { "en": "Apa Itu GPS Time Sync?", "id": "Sinkronisasi Waktu Menggunakan Satelit GPS." },
  { "en": "Apa Itu IRIG-B Time Code?", "id": "Kode Standar Sinkronisasi Waktu Peralatan." },
  { "en": "Apa Itu SNTP Time Protocol?", "id": "Protokol Waktu Jaringan Sederhana." },
  { "en": "Apa Itu PTP Time Protocol?", "id": "Protokol Waktu Presisi Mikro Detik." },
  { "en": "Apa Itu Fiber Optic Ring?", "id": "Topologi Jaringan Serat Optik Cincin." },
  { "en": "Apa Itu Ethernet Switch Managed?", "id": "Switch Jaringan Bisa Dikonfigurasi." },
  { "en": "Apa Itu Valve Regulated Lead Acid?", "id": "Aki Kering Bebas Perawatan." },
  { "en": "Apa Itu Absorbed Glass Mat? (AGM)", "id": "Teknologi Aki Elektrolit Serap Kaca." },
  { "en": "Apa Itu Gel Electrolyte Battery?", "id": "Aki Dengan Elektrolit Berbentuk Gel." },
  { "en": "Apa Itu Flooded Lead Acid?", "id": "Aki Basah Konvensional Isi Air." },
  { "en": "Apa Itu Nickel Cadmium Battery?", "id": "Baterai NiCd Tahan Suhu Ekstrem." },
  { "en": "Apa Itu Lithium Iron Phosphate?", "id": "Baterai LiFePO4 Aman Dan Awet." },
  { "en": "Apa Itu Battery Cell Balancing?", "id": "Menyamakan Tegangan Seluruh Sel Baterai." },
  { "en": "Apa Itu Battery Internal Resistance?", "id": "Hambatan Dalam Penentu Kesehatan Baterai." },
  { "en": "Apa Itu Battery Strap Resistance?", "id": "Tahanan Konektor Antar Terminal Baterai." },
  { "en": "Apa Itu Inter-Tier Connector?", "id": "Penghubung Antar Rak Baterai Bertingkat." },
  { "en": "Apa Itu Hydrogen Cooling System?", "id": "Sistem Pendingin Generator Gas Hidrogen." },
  { "en": "Apa Itu Seal Oil System?", "id": "Minyak Penyekat Gas Hidrogen Poros." },
  { "en": "Apa Itu Excitation Transformer?", "id": "Trafo Suplai Daya Penguat Medan." },
  { "en": "Apa Itu Field Discharge Switch?", "id": "Saklar Pembuang Energi Medan Magnet." },
  { "en": "Apa Itu Generator Circuit Breaker?", "id": "Pemutus Tenaga Utama Output Generator." },
  { "en": "Apa Itu Isolated Phase Busduct?", "id": "Busbar Fasa Terpisah Isolasi Udara." },
  { "en": "Apa Itu Neutral Grounding Transformer?", "id": "Trafo Pentanahan Titik Netral Generator." },
  { "en": "Apa Itu Generator Step-Up Transformer?", "id": "Trafo Penaik Tegangan Utama Pembangkit." },
  { "en": "Apa Itu Unit Auxiliary Transformer?", "id": "Trafo Pemakaian Sendiri Dari Generator." },
  { "en": "Apa Itu Open Circuit Characteristic? (OCC)", "id": "Kurva Tegangan Lawan Arus Medan." },
  { "en": "Apa Itu Short Circuit Characteristic? (SCC)", "id": "Kurva Arus Jangkar Lawan Medan." },
  { "en": "Apa Itu Air Gap Line?", "id": "Garis Linear Awal Kurva Saturasi." },
  { "en": "Apa Itu Zero Power Factor Test?", "id": "Uji Pemanasan Rotor Generator." },
  { "en": "Apa Itu Potier Triangle?", "id": "Metode Grafis Menentukan Reaktansi Bocor." },
  { "en": "Apa Itu Synchronous Impedance Method?", "id": "Metode Hitung Regulasi Tegangan Generator." },
  { "en": "Apa Itu Ampere-Turn Method?", "id": "Metode MMF Hitung Regulasi Tegangan." },
  { "en": "Apa Itu Zero Power Factor Method?", "id": "Metode Potier Hitung Regulasi Tegangan." },
  { "en": "Apa Itu Blondel's Two Reaction Theory?", "id": "Teori Analisis Mesin Kutub Menonjol." },
  { "en": "Apa Itu Power Angle Delta?", "id": "Sudut Antara Rotor Dan Stator." },
  { "en": "Apa Itu Infinite Busbar?", "id": "Sumber Tegangan Dan Frekuensi Konstan." },
  { "en": "Apa Itu Synchronizing Torque?", "id": "Torsi Penjaga Keserempakan Putaran." },
  { "en": "Apa Itu Damping Torque?", "id": "Torsi Peredam Osilasi Rotor." },
  { "en": "Apa Itu Hunting Motor Sinkron?", "id": "Osilasi Kecepatan Sekitar Titik Sinkron." },
  { "en": "Apa Itu Pull-In Torque?", "id": "Torsi Masuk Ke Kecepatan Sinkron." },
  { "en": "Apa Itu Pull-Out Torque?", "id": "Torsi Maksimum Sebelum Lepas Sinkron." },
  { "en": "Apa Itu V-Curve Motor Sinkron?", "id": "Grafik Arus Jangkar Lawan Medan." },
  { "en": "Apa Itu Inverted V-Curve?", "id": "Grafik Faktor Daya Lawan Medan." },
  { "en": "Apa Itu Over-Excited Motor?", "id": "Motor Sinkron Menyuplai Daya Reaktif." },
  { "en": "Apa Itu Under-Excited Motor?", "id": "Motor Sinkron Menyerap Daya Reaktif." },
  { "en": "Apa Itu Auto-Transformer Starter?", "id": "Starter Mengurangi Tegangan Pakai Trafo." },
  { "en": "Apa Itu Star-Delta Timer?", "id": "Mengatur Waktu Pindah Bintang Segitiga." },
  { "en": "Apa Itu Closed Transition Starter?", "id": "Pindah Star-Delta Tanpa Putus Arus." },
  { "en": "Apa Itu Part Winding Starter?", "id": "Start Menggunakan Sebagian Lilitan Stator." },
  { "en": "Apa Itu Korndorfer Starter?", "id": "Varian Auto-Trafo Starter Tanpa Jeda." },
  { "en": "Apa Itu Primary Resistor Starter?", "id": "Resistor Seri Stator Kurangi Tegangan." },
  { "en": "Apa Itu Secondary Resistor Starter?", "id": "Resistor Seri Rotor Motor Cincin." },
  { "en": "Apa Itu Soft Starter Bypass?", "id": "Kontaktor Pintas Setelah Kecepatan Penuh." },
  { "en": "Apa Itu DC Link Voltage?", "id": "Tegangan Bus DC Dalam VFD." },
  { "en": "Apa Itu Brake Chopper Unit?", "id": "Modul Saklar Pembuang Energi Rem." },
  { "en": "Apa Itu Dynamic Braking Resistor?", "id": "Resistor Pengubah Energi Rem Panas." },
  { "en": "Apa Itu Motor Service Factor?", "id": "Faktor Pengali Kemampuan Beban Lebih." },
  { "en": "Apa Itu Motor Duty S4?", "id": "Start-Stop Sering Dengan Pendinginan." },
  { "en": "Apa Itu Motor Duty S6?", "id": "Operasi Kontinu Dengan Beban Intermiten." },
  { "en": "Apa Itu Brushless DC Motor? (BLDC)", "id": "Motor DC Komutasi Elektronik." },
  { "en": "Apa Itu Universal Motor?", "id": "Motor Seri Bisa AC DC." },
  { "en": "Apa Itu Cable Charging Current?", "id": "Arus Kapasitif Kabel Tanpa Beban." },
  { "en": "Apa Itu Dielectric Loss Angle?", "id": "Sudut Delta Pada Uji Isolasi." },
  { "en": "Apa Itu Water Treeing Kabel?", "id": "Degradasi Isolasi Akibat Kelembaban." },
  { "en": "Apa Itu Electrical Treeing Kabel?", "id": "Jalur Tembus Permanen Dalam Isolasi." },
  { "en": "Apa Itu Partial Discharge Inception?", "id": "Tegangan Mulai Munculnya PD." },
  { "en": "Apa Itu Partial Discharge Extinction?", "id": "Tegangan Berhentinya PD Saat Turun." },
  { "en": "Apa Itu Cable Sheath Bonding?", "id": "Metode Pentanahan Pelindung Logam Kabel." },
  { "en": "Apa Itu Single Point Bonding?", "id": "Pentanahan Sheath Hanya Satu Ujung." },
  { "en": "Apa Itu Both Ends Bonding?", "id": "Pentanahan Sheath Di Kedua Ujung." },
  { "en": "Apa Itu IDMT Relay Curve?", "id": "Kurva Waktu Terbalik Standar." },
  { "en": "Apa Itu Definite Time Relay?", "id": "Relay Waktu Trip Tetap." },
  { "en": "Apa Itu Instantaneous Overcurrent?", "id": "Relay Trip Tanpa Tunda Waktu." },
  { "en": "Apa Itu Pickup Current Relay?", "id": "Arus Ambang Batas Mulai Operasi." },
  { "en": "Apa Itu Drop-off Current Relay?", "id": "Arus Reset Kembali Posisi Awal." },
  { "en": "Apa Itu Reset Ratio Protection?", "id": "Perbandingan Arus Reset Dan Pickup." },
  { "en": "Apa Itu Time Multiplier Setting? (TMS)", "id": "Pengali Waktu Geser Kurva IDMT." },
  { "en": "Apa Itu Plug Setting Multiplier? (PSM)", "id": "Rasio Arus Gangguan Terhadap Setting." },
  { "en": "Apa Itu CT Spill Current?", "id": "Arus Ketidakseimbangan Sirkuit Diferensial." },
  { "en": "Apa Itu Neutral Displacement Voltage?", "id": "Pergeseran Tegangan Titik Netral." },
  { "en": "Apa Itu Impulse Ratio Isolator?", "id": "Rasio Tembus Impuls Dan AC." },
  { "en": "Apa Itu Critical Flashover Voltage?", "id": "Tegangan 50 Persen Peluang Tembus." },
  { "en": "Apa Itu Withstand Voltage Level?", "id": "Tegangan Aman Tanpa Tembus." },
  { "en": "Apa Itu Dry Arcing Distance?", "id": "Jarak Loncatan Api Kondisi Kering." },
  { "en": "Apa Itu Leakage Distance Isolator?", "id": "Jarak Rambat Permukaan Isolator." },
  { "en": "Apa Itu Ferranti Effect Rise?", "id": "Kenaikan Tegangan Ujung Beban Nol." },
  { "en": "Apa Itu Corona Power Loss?", "id": "Daya Hilang Akibat Ionisasi Udara." },
  { "en": "Apa Itu Total Harmonic Distortion? (THD)", "id": "Persentase Total Cacat Gelombang." },
  { "en": "Apa Itu Voltage Flicker Index?", "id": "Ukuran Kedipan Cahaya Akibat Tegangan." },
  { "en": "Apa Itu Displacement Power Factor?", "id": "Faktor Daya Hanya Fundamental." },
  { "en": "Apa Itu True Power Factor?", "id": "Faktor Daya Termasuk Harmonisa." },
  { "en": "Apa Itu K-Factor Rating?", "id": "Kemampuan Trafo Tangani Harmonisa." },
  { "en": "Apa Itu Detuned Reactor Bank?", "id": "Kapasitor Seri Induktor Anti Resonansi." },
  { "en": "Apa Itu Battery Float Voltage?", "id": "Tegangan Jaga Baterai Penuh." },
  { "en": "Apa Itu Battery Equalize Voltage?", "id": "Tegangan Penyeimbang Sel Baterai." },
  { "en": "Apa Itu Peukert's Law Battery?", "id": "Kapasitas Turun Jika Arus Naik." },
  { "en": "Apa Itu Depth of Discharge? (DOD)", "id": "Persen Kapasitas Terpakai Dari Penuh." },
  { "en": "Apa Itu State of Charge? (SOC)", "id": "Persen Sisa Kapasitas Baterai." },
  { "en": "Apa Itu Battery Sulfation?", "id": "Kristal Timbal Sulfat Keras." },
  { "en": "Apa Itu Nickel Cadmium Cell?", "id": "Baterai Alkalin Tahan Banting." },
  { "en": "Apa Itu Boiler Feed Pump?", "id": "Pompa Air Umpan Ketel Uap." },
  { "en": "Apa Itu Steam Turbine Governor?", "id": "Katup Pengatur Aliran Uap Turbin." },
  { "en": "Apa Itu Condenser Vacuum Pump?", "id": "Pompa Hampa Udara Kondensor Uap." },
  { "en": "Apa Itu Cooling Tower Fill?", "id": "Media Perluas Kontak Air Udara." },
  { "en": "Apa Itu Penstock Surge Tank?", "id": "Peredam Tekanan Air Pipa Pesat." },
  { "en": "Apa Itu Trash Rack Hydro?", "id": "Saringan Sampah Intake Turbin Air." },
  { "en": "Apa Itu Draft Tube Hydro?", "id": "Pipa Buang Air Keluar Turbin." },
  { "en": "Apa Itu Spillway Gate Dam?", "id": "Pintu Pelimpah Air Waduk." },
  { "en": "Apa Itu Coal Pulverizer Mill?", "id": "Mesin Penggiling Batubara Jadi Debu." },
  { "en": "Apa Itu Electrostatic Precipitator? (ESP)", "id": "Penangkap Debu Listrik Tegangan Tinggi." },
  { "en": "Apa Itu Flue Gas Desulfurization?", "id": "Penyerap Sulfur Gas Buang PLTU." },
  { "en": "Apa Itu Bottom Ash Hopper?", "id": "Penampung Abu Bawah Boiler." },
  { "en": "Apa Itu Deaerator Tank?", "id": "Tangki Pembuang Oksigen Air Umpan." },
  { "en": "Apa Itu Economizer Boiler?", "id": "Pemanas Air Awal Gas Buang." },
  { "en": "Apa Itu Superheater Boiler?", "id": "Pemanas Uap Kering Lanjut." },
  { "en": "Apa Itu Reheater Boiler?", "id": "Pemanas Ulang Uap Antar Turbin." },
  { "en": "Apa Itu Air Preheater? (APH)", "id": "Pemanas Udara Masuk Ruang Bakar." },
  { "en": "Apa Itu Forced Draft Fan? (FD Fan)", "id": "Kipas Peniup Udara Ke Boiler." },
  { "en": "Apa Itu Induced Draft Fan? (ID Fan)", "id": "Kipas Penyedot Gas Buang Boiler." },
  { "en": "Apa Itu Primary Air Fan? (PA Fan)", "id": "Kipas Peniup Serbuk Batubara." },
  { "en": "Apa Itu Steam Drum?", "id": "Bejana Pemisah Air Dan Uap." },
  { "en": "Apa Itu Blowdown Valve?", "id": "Katup Buang Kotoran Air Boiler." },
  { "en": "Apa Itu Safety Valve Boiler?", "id": "Katup Pengaman Tekanan Lebih." },
  { "en": "Apa Itu Soot Blower?", "id": "Pembersih Jelaga Pipa Boiler." },
  { "en": "Apa Itu Blocked Overcurrent Protection?", "id": "Proteksi Arus Lebih Terblokir Sinyal." },
  { "en": "Apa Itu Thermal Image Relay?", "id": "Simulasi Panas Lilitan Berbasis Arus." },
  { "en": "Apa Itu Negative Sequence Voltage?", "id": "Tegangan Urutan Fasa Terbalik." },
  { "en": "Apa Itu Dead Zone Protection?", "id": "Proteksi Area Antara CT Breaker." },
  { "en": "Apa Itu Breaker Fail Timer?", "id": "Waktu Tunda Sebelum Trip Busbar." },
  { "en": "Apa Itu Ductor Test?", "id": "Uji Tahanan Kontak Arus Tinggi." },
  { "en": "Apa Itu Tip-Up Test Tan Delta?", "id": "Uji Kenaikan Tan Delta Tegangan." },
  { "en": "Apa Itu Phase Resolved PD Pattern?", "id": "Pola PD Terhadap Fasa Tegangan." },
  { "en": "Apa Itu Duval Triangle Ratio?", "id": "Metode Analisis Gas Terlarut Minyak." },
  { "en": "Apa Itu SFRA Trace Deviation?", "id": "Penyimpangan Grafik Respon Frekuensi Trafo." },
  { "en": "Apa Itu Line Trap Bandwidth?", "id": "Rentang Frekuensi Blokir Perangkap Gelombang." },
  { "en": "Apa Itu CVT Ferroresonance?", "id": "Osilasi Tegangan Tinggi Pada Kapasitor." },
  { "en": "Apa Itu Station Class Arrester?", "id": "Arrester Kapasitas Energi Paling Besar." },
  { "en": "Apa Itu Center Break Disconnector?", "id": "Pemisah Membuka Di Tengah Lengan." },
  { "en": "Apa Itu Pantograph Reach?", "id": "Jangkauan Lengan Gunting Pemisah." },
  { "en": "Apa Itu Inertia Constant H?", "id": "Energi Kinetik Rotor Per MVA." },
  { "en": "Apa Itu Damping Ratio Zeta?", "id": "Ukuran Kecepatan Hilangnya Osilasi." },
  { "en": "Apa Itu Swing Equation Generator?", "id": "Persamaan Gerak Rotor Saat Gangguan." },
  { "en": "Apa Itu RCM Maintenance?", "id": "Pemeliharaan Berbasis Keandalan Aset." },
  { "en": "Apa Itu CBM Maintenance?", "id": "Pemeliharaan Berbasis Kondisi Aktual." },
  { "en": "Apa Itu TBM Maintenance?", "id": "Pemeliharaan Berbasis Jadwal Waktu." },
  { "en": "Apa Itu Delta-T Thermography?", "id": "Beda Suhu Objek Dan Referensi." },
  { "en": "Apa Itu Cross Bonding Link Box?", "id": "Kotak Sambungan Silang Sheath Kabel." },
  { "en": "Apa Itu SVL Link Box?", "id": "Arrester Pelindung Sheath Dalam Box." },
  { "en": "Apa Itu Battery Sulfation Reversal?", "id": "Proses Desulfator Pulsa Frekuensi Tinggi." },
  { "en": "Apa Itu Equalization Voltage Level?", "id": "Tegangan Charging Lebih Tinggi Normal." },
  { "en": "Apa Itu Specific Gravity Correction?", "id": "Koreksi Berat Jenis Terhadap Suhu." },
  { "en": "Apa Itu Pilot Wire Protection?", "id": "Proteksi Diferensial Kabel Pilot." },
  { "en": "Apa Itu Power Line Carrier?", "id": "Komunikasi Lewat Kabel Tegangan Tinggi." },
  { "en": "Apa Itu Frequency Shift Keying?", "id": "Modulasi Frekuensi Sinyal Digital PLC." },
  { "en": "Apa Itu Blocking Coil?", "id": "Induktor Penahan Frekuensi Tinggi PLC." },
  { "en": "Apa Itu Coupling Capacitor PLC?", "id": "Kapasitor Penyuntik Sinyal Ke Jaringan." },
  { "en": "Apa Itu Drain Coil PLC?", "id": "Mengalirkan Arus Bocor Frekuensi 50Hz." },
  { "en": "Apa Itu Tuning Unit PLC?", "id": "Penyesuai Impedansi Alat Dan Jaringan." },
  { "en": "Apa Itu Hybrid Coupler?", "id": "Penggabung Dua Transmitter Satu Kabel." },
  { "en": "Apa Itu Four Wire Measurement?", "id": "Metode Kelvin Ukur Tahanan Rendah." },
  { "en": "Apa Itu Current Injection Kit?", "id": "Alat Suntik Arus Uji Relay." },
  { "en": "Apa Itu Secondary Injection?", "id": "Suntik Arus Ke Terminal Relay." },
  { "en": "Apa Itu Primary Injection?", "id": "Suntik Arus Ke Busbar Utama." },
  { "en": "Apa Itu Phantom Load?", "id": "Beban Buatan Uji KWH Meter." },
  { "en": "Apa Itu Meter Constant?", "id": "Jumlah Putaran Per KWH Energi." },
  { "en": "Apa Itu Creep Test Meter?", "id": "Uji Putaran Tanpa Beban Meter." },
  { "en": "Apa Itu Starting Current Meter?", "id": "Arus Minimum Meter Mulai Hitung." },
  { "en": "Apa Itu Tamper Evidence?", "id": "Bukti Fisik Gangguan Segel Meter." },
  { "en": "Apa Itu Meter Seal?", "id": "Segel Pengaman Penutup KWH Meter." },
  { "en": "Apa Itu Optical Port Meter?", "id": "Port Komunikasi Data Inframerah Meter." },
  { "en": "Apa Itu DLMS Protocol?", "id": "Protokol Komunikasi Standar Meter Pintar." },
  { "en": "Apa Itu Load Profile?", "id": "Rekaman Pemakaian Energi Per Interval." },
  { "en": "Apa Itu Maximum Demand Register?", "id": "Pencatat Permintaan Daya Tertinggi." },
  { "en": "Apa Itu Time of Use?", "id": "Tarif Listrik Berbeda Berdasarkan Waktu." },
  { "en": "Apa Itu Prepaid Meter?", "id": "Meter Listrik Bayar Di Muka." },
  { "en": "Apa Itu Token Listrik?", "id": "Kode 20 Digit Isi Ulang." },
  { "en": "Apa Itu Keypad Meter?", "id": "Tombol Input Token Pada Meter." },
  { "en": "Apa Itu Contactor Meter?", "id": "Saklar Pemutus Arus Dalam Meter." },
  { "en": "Apa Itu Credit Status?", "id": "Sisa KWH Dalam Meter Prabayar." },
  { "en": "Apa Itu Low Credit Alarm?", "id": "Peringatan Sisa KWH Hampir Habis." },
  { "en": "Apa Itu Friendly Hours?", "id": "Waktu Tidak Boleh Padam (Malam)." },
  { "en": "Apa Itu Emergency Credit?", "id": "Hutang KWH Darurat Meter Prabayar." },
  { "en": "Apa Itu Clear Tamper Token?", "id": "Kode Reset Status Tamper Meter." },
  { "en": "Apa Itu Power Limit?", "id": "Batas Daya Maksimum Pelanggan." },
  { "en": "Apa Itu Over Power Trip?", "id": "Meter Padam Karena Kelebihan Daya." },
  { "en": "Apa Itu Under Voltage Trip?", "id": "Meter Padam Tegangan Terlalu Rendah." },
  { "en": "Apa Itu Over Voltage Trip?", "id": "Meter Padam Tegangan Terlalu Tinggi." },
  { "en": "Apa Itu Earth Fault Trip?", "id": "Meter Padam Ada Arus Bocor." },
  { "en": "Apa Itu Reverse Energy?", "id": "Energi Mengalir Balik Ke Jaringan." },
  { "en": "Apa Itu Import Energy?", "id": "Energi Masuk Dari Jaringan." },
  { "en": "Apa Itu Export Energy?", "id": "Energi Keluar Ke Jaringan (Surya)." },
  { "en": "Apa Itu Net Metering?", "id": "Selisih Ekspor Impor Energi Listrik." },
  { "en": "Apa Itu Bi-Directional Meter?", "id": "Meter Ukur Dua Arah Aliran." },
  { "en": "Apa Itu Instrument Transformer Meter?", "id": "Meter Menggunakan CT Dan PT." },
  { "en": "Apa Itu Direct Connected Meter?", "id": "Meter Terhubung Langsung Tanpa CT." },
  { "en": "Apa Itu Class 0.2S Meter?", "id": "Meter Akurasi Tinggi Rentang Lebar." },
  { "en": "Apa Itu Class 1.0 Meter?", "id": "Meter Akurasi Standar Rumah Tangga." },
  { "en": "Apa Itu Active Power P?", "id": "Daya Nyata Satuan Watt." },
  { "en": "Apa Itu Reactive Power Q?", "id": "Daya Reaktif Satuan VAR." },
  { "en": "Apa Itu Apparent Power S?", "id": "Daya Semu Satuan VA." },
  { "en": "Apa Itu Power Factor PF?", "id": "Rasio Daya Aktif Bagi Semu." },
  { "en": "Apa Itu Lagging PF?", "id": "Arus Tertinggal (Beban Induktif)." },
  { "en": "Apa Itu Leading PF?", "id": "Arus Mendahului (Beban Kapasitif)." },
  { "en": "Apa Itu Unity PF?", "id": "Faktor Daya Satu (Resistif)." },
  { "en": "Apa Itu Phase Angle?", "id": "Sudut Antara Tegangan Dan Arus." },
  { "en": "Apa Itu Frequency Hz?", "id": "Jumlah Siklus Gelombang Per Detik." },
  { "en": "Apa Itu Period T?", "id": "Waktu Satu Siklus Gelombang Penuh." },
  { "en": "Apa Itu Amplitude?", "id": "Nilai Puncak Gelombang Sinus." },
  { "en": "Apa Itu Peak-to-Peak?", "id": "Selisih Nilai Puncak Atas Bawah." },
  { "en": "Apa Itu RMS Value?", "id": "Nilai Efektif Setara Panas DC." },
  { "en": "Apa Itu Average Value?", "id": "Rata-Rata Nilai Gelombang Rectified." },
  { "en": "Apa Itu Form Factor?", "id": "Rasio RMS Bagi Average." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Peak Bagi RMS." },
  { "en": "Apa Itu Harmonic Distortion?", "id": "Cacat Gelombang Akibat Frekuensi Lain." },
  { "en": "Apa Itu THD-V?", "id": "Total Distorsi Harmonisa Tegangan." },
  { "en": "Apa Itu THD-I?", "id": "Total Distorsi Harmonisa Arus." },
  { "en": "Apa Itu Individual Harmonic?", "id": "Komponen Harmonisa Frekuensi Tunggal." },
  { "en": "Apa Itu Fundamental Frequency?", "id": "Frekuensi Dasar Sistem (50Hz)." },
  { "en": "Apa Itu DC Component?", "id": "Nilai Rata-Rata Sinyal AC Non-Simetris." },
  { "en": "Apa Itu Sistem Katenari Kereta?", "id": "Sistem Kabel Listrik Atas Kereta Api." },
  { "en": "Apa Itu Pantograf Kereta Listrik?", "id": "Alat Pengambil Listrik Dari Kabel Atas." },
  { "en": "Apa Itu Tegangan Traksi 1500V DC?", "id": "Standar Tegangan Kereta Listrik Commuter." },
  { "en": "Apa Itu Tegangan Traksi 25kV AC?", "id": "Standar Tegangan Kereta Cepat Jarak Jauh." },
  { "en": "Apa Itu Third Rail System?", "id": "Rel Ketiga Penyuplai Listrik Di Bawah." },
  { "en": "Apa Itu Stray Current Corrosion?", "id": "Korosi Pipa Akibat Arus Bocor Tanah." },
  { "en": "Apa Itu Regenerative Braking Train?", "id": "Kereta Mengembalikan Listrik Saat Pengereman." },
  { "en": "Apa Itu Traction Substation?", "id": "Gardu Induk Penyuplai Listrik Kereta." },
  { "en": "Apa Itu Rectifier Transformer Traksi?", "id": "Trafo Khusus Penyearah Gardu DC." },
  { "en": "Apa Itu High Speed Circuit Breaker?", "id": "Pemutus Arus DC Kecepatan Tinggi." },
  { "en": "Apa Itu DC Arrestor Traksi?", "id": "Pelindung Surja Petir Sistem DC." },
  { "en": "Apa Itu Rail Return Current?", "id": "Arus Balik Melalui Rel Kereta." },
  { "en": "Apa Itu Impedance Bond Rel?", "id": "Pemisah Sinyal Dan Arus Traksi Rel." },
  { "en": "Apa Itu Chopper Control Train?", "id": "Pengendali Motor DC Kereta Lama." },
  { "en": "Apa Itu VVVF Inverter Train?", "id": "Pengendali Motor AC Kereta Modern." },
  { "en": "Apa Itu Traction Motor DC Series?", "id": "Motor DC Torsi Awal Tinggi Kereta." },
  { "en": "Apa Itu Traction Motor AC Induction?", "id": "Motor AC Tahan Banting Kereta Modern." },
  { "en": "Apa Itu Overhead Catenary System? (OCS)", "id": "Sistem Tiang Dan Kabel Listrik Atas." },
  { "en": "Apa Itu Messenger Wire Katenari?", "id": "Kabel Penyangga Utama Di Atas." },
  { "en": "Apa Itu Contact Wire Katenari?", "id": "Kabel Bawah Yang Digesek Pantograf." },
  { "en": "Apa Bahan Contact Wire?", "id": "Tembaga Keras Beralur Khusus." },
  { "en": "Apa Itu Dropper Katenari?", "id": "Kawat Penggantung Contact Wire Ke Messenger." },
  { "en": "Apa Itu Tensioning Device Katenari?", "id": "Penarik Kabel Agar Tetap Kencang." },
  { "en": "Apa Itu Balance Weight Katenari?", "id": "Pemberat Penarik Kabel Katenari Otomatis." },
  { "en": "Apa Itu Section Insulator Katenari?", "id": "Pemisah Listrik Antar Bagian Kabel." },
  { "en": "Apa Itu Neutral Section Katenari?", "id": "Bagian Kabel Tanpa Tegangan Pemisah Fasa." },
  { "en": "Apa Fungsi Neutral Section?", "id": "Mencegah Hubung Singkat Antar Gardu." },
  { "en": "Apa Itu Mast Katenari?", "id": "Tiang Penyangga Sistem Kabel Atas." },
  { "en": "Apa Itu Cantilever Katenari?", "id": "Lengan Penyangga Kabel Di Tiang." },
  { "en": "Apa Itu Steady Arm Katenari?", "id": "Lengan Penahan Posisi Contact Wire." },
  { "en": "Apa Itu Pantograph Carbon Strip?", "id": "Batang Karbon Gesek Pada Pantograf." },
  { "en": "Apa Itu Hard Spot Katenari?", "id": "Titik Keras Penyebab Pantograf Loncat." },
  { "en": "Apa Itu Sparking Pantograf?", "id": "Percikan Api Akibat Kontak Buruk." },
  { "en": "Apa Itu Zig-Zag Katenari?", "id": "Pola Kabel Agar Pantograf Aus Merata." },
  { "en": "Apa Itu Return Conductor Rail?", "id": "Kabel Penguat Arus Balik Rel." },
  { "en": "Apa Itu Negative Feeder?", "id": "Kabel Negatif Gantung Sistem AT." },
  { "en": "Apa Itu Rankine Cycle Efficiency?", "id": "Efisiensi Termodinamika Siklus Uap Air." },
  { "en": "Apa Itu Carnot Cycle Efficiency?", "id": "Efisiensi Teoritis Maksimum Mesin Panas." },
  { "en": "Apa Itu Supercritical Boiler?", "id": "Boiler Di Atas Tekanan Kritis Air." },
  { "en": "Apa Itu Ultra-Supercritical Boiler?", "id": "Boiler Tekanan Dan Suhu Sangat Tinggi." },
  { "en": "Apa Itu Main Steam Temperature?", "id": "Suhu Uap Masuk Turbin Tekanan Tinggi." },
  { "en": "Apa Itu Main Steam Pressure?", "id": "Tekanan Uap Masuk Turbin." },
  { "en": "Apa Itu Turbine Heat Rate?", "id": "Konsumsi Panas Per KWH Turbin." },
  { "en": "Apa Itu Gross Plant Heat Rate?", "id": "Efisiensi Pembangkit Tanpa Pemakaian Sendiri." },
  { "en": "Apa Itu Net Plant Heat Rate?", "id": "Efisiensi Pembangkit Bersih Ke Grid." },
  { "en": "Apa Itu Coal Calorific Value?", "id": "Nilai Energi Panas Per Kg Batubara." },
  { "en": "Apa Itu Gross Calorific Value? (GCV)", "id": "Nilai Kalor Termasuk Panas Uap Air." },
  { "en": "Apa Itu Net Calorific Value? (NCV)", "id": "Nilai Kalor Tanpa Panas Uap Air." },
  { "en": "Apa Itu Moisture Content Coal?", "id": "Kadar Air Dalam Batubara." },
  { "en": "Apa Itu Ash Content Coal?", "id": "Kadar Abu Sisa Pembakaran Batubara." },
  { "en": "Apa Itu Volatile Matter Coal?", "id": "Zat Terbang Mudah Terbakar Batubara." },
  { "en": "Apa Itu Fixed Carbon Coal?", "id": "Karbon Tetap Sisa Pemanasan Batubara." },
  { "en": "Apa Itu Hardgrove Grindability Index? (HGI)", "id": "Ukuran Kemudahan Batubara Digiling Halus." },
  { "en": "Apa Itu Ash Fusion Temperature?", "id": "Suhu Leleh Abu Batubara." },
  { "en": "Apa Itu Slagging Boiler?", "id": "Kerak Abu Cair Di Dinding Boiler." },
  { "en": "Apa Itu Fouling Boiler?", "id": "Kerak Abu Kering Di Pipa Superheater." },
  { "en": "Apa Itu Loss on Ignition? (LOI)", "id": "Persentase Karbon Tidak Terbakar Di Abu." },
  { "en": "Apa Itu Unburned Carbon Ash?", "id": "Sisa Batubara Mentah Dalam Abu." },
  { "en": "Apa Itu Excess Air Combustion?", "id": "Udara Lebih Untuk Pembakaran Sempurna." },
  { "en": "Apa Itu Stoichiometric Ratio?", "id": "Perbandingan Udara Bahan Bakar Ideal." },
  { "en": "Apa Itu Flue Gas Oxygen?", "id": "Sisa Oksigen Dalam Gas Buang." },
  { "en": "Apa Itu Flue Gas CO?", "id": "Indikasi Pembakaran Tidak Sempurna (Bahaya)." },
  { "en": "Apa Itu NOx Emission Boiler?", "id": "Polutan Nitrogen Oksida Gas Buang." },
  { "en": "Apa Itu SOx Emission Boiler?", "id": "Polutan Sulfur Oksida Gas Buang." },
  { "en": "Apa Itu Selective Catalytic Reduction? (SCR)", "id": "Sistem Pengurang NOx Pakai Katalis." },
  { "en": "Apa Itu Ammonia Injection SCR?", "id": "Menyuntik Amonia Untuk Menetralkan NOx." },
  { "en": "Apa Itu Baghouse Filter?", "id": "Filter Kain Penyaring Debu Cerobong." },
  { "en": "Apa Itu Cooling Tower Range?", "id": "Selisih Suhu Air Masuk Dan Keluar." },
  { "en": "Apa Itu Cooling Tower Approach?", "id": "Selisih Suhu Air Keluar Dan Udara." },
  { "en": "Apa Itu Wet Bulb Temperature?", "id": "Suhu Terendah Pendinginan Evaporasi Air." },
  { "en": "Apa Itu Condenser Back Pressure?", "id": "Tekanan Vakum Balik Exhaust Turbin." },
  { "en": "Apa Itu Vacuum Ejector?", "id": "Alat Pembuat Vakum Menggunakan Uap." },
  { "en": "Apa Itu Ball Cleaning System?", "id": "Bola Karet Pembersih Pipa Kondensor." },
  { "en": "Apa Itu Deaerator Pegging Steam?", "id": "Uap Pemanas Air Di Deaerator." },
  { "en": "Apa Itu EPR Insulation Material?", "id": "Karet Ethylene Propylene Rubber Fleksibel." },
  { "en": "Apa Itu PVC Insulation Temp Rating?", "id": "Batas Suhu Operasi PVC 70 Derajat." },
  { "en": "Apa Itu Silicon Rubber Insulator?", "id": "Isolator Polimer Tahan Air Hujan." },
  { "en": "Apa Itu EPDM Rubber?", "id": "Karet Sintetis Tahan Cuaca Panas." },
  { "en": "Apa Itu Epoxy Resin Insulator?", "id": "Bahan Isolator Keras Dalam Ruangan." },
  { "en": "Apa Itu Toughened Glass Insulator?", "id": "Isolator Kaca Tempered Tahan Benturan." },
  { "en": "Apa Itu Mineral Oil Dielectric?", "id": "Minyak Isolasi Berbasis Minyak Bumi." },
  { "en": "Apa Itu Synthetic Ester Oil?", "id": "Minyak Isolasi Sintetis Tahan Api." },
  { "en": "Apa Itu Natural Ester Oil?", "id": "Minyak Nabati Ramah Lingkungan." },
  { "en": "Apa Itu Biodegradable Oil?", "id": "Minyak Yang Bisa Terurai Alam." },
  { "en": "Apa Itu SF6 Gas Dielectric Strength?", "id": "Tiga Kali Lebih Kuat Dari Udara." },
  { "en": "Apa Itu Vacuum Dielectric Strength?", "id": "Sangat Tinggi Jarak Celah Pendek." },
  { "en": "Apa Itu Mica Tape Insulation?", "id": "Isolasi Tahan Panas Lilitan Generator." },
  { "en": "Apa Itu Kapton Tape Insulation?", "id": "Isolasi Film Polimida Suhu Tinggi." },
  { "en": "Apa Itu Nomex Paper Insulation?", "id": "Kertas Aramid Tahan Panas Trafo." },
  { "en": "Apa Itu Pressboard Insulation?", "id": "Papan Kertas Tebal Isolasi Trafo." },
  { "en": "Apa Itu Enamel Wire Coating?", "id": "Lapisan Tipis Isolasi Kawat Magnet." },
  { "en": "Apa Itu Kabel XLPE?", "id": "Kabel Isolasi Cross-Linked Polyethylene." },
  { "en": "Apa Keuntungan Isolasi XLPE?", "id": "Tahan Suhu Tinggi Dan Kuat." },
  { "en": "Apa Itu Kabel Oil Filled?", "id": "Kabel Berisolasi Kertas Berisi Minyak." },
  { "en": "Apa Itu Kabel Gas Pressure?", "id": "Kabel Berisolasi Gas Nitrogen Tekanan." },
  { "en": "Apa Itu Kabel PILC?", "id": "Paper Insulated Lead Covered Cable." },
  { "en": "Apa Itu Kabel MICC?", "id": "Mineral Insulated Copper Clad Cable." },
  { "en": "Apa Keuntungan Kabel MICC?", "id": "Sangat Tahan Api Dan Panas." },
  { "en": "Apa Itu Fire Survival Cable?", "id": "Kabel Tetap Operasi Saat Kebakaran." },
  { "en": "Apa Itu Flame Retardant Cable?", "id": "Kabel Menghambat Perambatan Api." },
  { "en": "Apa Itu Low Smoke Zero Halogen?", "id": "Kabel Terbakar Sedikit Asap Racun." },
  { "en": "Apa Itu Konduktor Aluminium ACSR?", "id": "Aluminium Conductor Steel Reinforced." },
  { "en": "Apa Itu Konduktor Aluminium AAAC?", "id": "All Aluminium Alloy Conductor." },
  { "en": "Apa Itu Konduktor Aluminium AAC?", "id": "All Aluminium Conductor." },
  { "en": "Apa Itu Konduktor ACSS?", "id": "Aluminium Conductor Steel Supported." },
  { "en": "Apa Keuntungan Konduktor ACSS?", "id": "Tahan Suhu Operasi Sangat Tinggi." },
  { "en": "Apa Itu Konduktor ACCC?", "id": "Aluminium Conductor Composite Core." },
  { "en": "Apa Keuntungan Konduktor ACCC?", "id": "Ringan, Kuat, Dan Rendah Sag." },
  { "en": "Apa Itu Konduktor Tembaga Hard Drawn?", "id": "Tembaga Keras Untuk Saluran Udara." },
  { "en": "Apa Itu Konduktor Tembaga Annealed?", "id": "Tembaga Lunak Untuk Kabel Tanah." },
  { "en": "Apa Itu Konduktor Berongga?", "id": "Konduktor Pipa Mengurangi Efek Kulit." },
  { "en": "Apa Itu Konduktor Terpilin? (Stranded)", "id": "Konduktor Fleksibel Banyak Kawat Kecil." },
  { "en": "Apa Itu Konduktor Padat? (Solid)", "id": "Konduktor Kawat Tunggal Kaku." },
  { "en": "Apa Itu Skin Effect Pada Kabel?", "id": "Arus Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Proximity Effect Kabel?", "id": "Arus Terdistribusi Tidak Merata." },
  { "en": "Apa Itu Geometric Mean Radius? (GMR)", "id": "Radius Efektif Menghitung Induktansi Kabel." },
  { "en": "Apa Itu Geometric Mean Distance? (GMD)", "id": "Jarak Rata-Rata Antar Fasa." },
  { "en": "Apa Itu Induktansi Saluran Transmisi?", "id": "Sifat Menghambat Perubahan Arus AC." },
  { "en": "Apa Itu Kapasitansi Saluran Transmisi?", "id": "Sifat Menyimpan Muatan Antar Fasa." },
  { "en": "Apa Itu Reaktansi Induktif Saluran?", "id": "Hambatan AC Akibat Induktansi Kabel." },
  { "en": "Apa Itu Reaktansi Kapasitif Saluran?", "id": "Hambatan AC Akibat Kapasitansi Kabel." },
  { "en": "Apa Itu Impedansi Surja Karakteristik?", "id": "Impedansi Saluran Tanpa Rugi-Rugi." },
  { "en": "Apa Itu Surge Impedance Loading? (SIL)", "id": "Daya Alami Saluran Transmisi." },
  { "en": "Apa Itu Voltage Regulation Saluran?", "id": "Perubahan Tegangan Kirim Dan Terima." },
  { "en": "Apa Itu Efisiensi Transmisi?", "id": "Perbandingan Daya Terima Dan Kirim." },
  { "en": "Apa Itu Rugi Korona Saluran?", "id": "Energi Hilang Akibat Ionisasi Udara." },
  { "en": "Apa Itu Critical Disruptive Voltage?", "id": "Tegangan Mulai Terjadinya Korona." },
  { "en": "Apa Itu Visual Critical Voltage?", "id": "Tegangan Korona Mulai Terlihat Mata." },
  { "en": "Apa Itu Faktor Cuaca Korona?", "id": "Pengaruh Hujan Terhadap Rugi Korona." },
  { "en": "Apa Itu Faktor Permukaan Korona?", "id": "Pengaruh Kasar Konduktor Terhadap Korona." },
  { "en": "Apa Itu Bundle Conductor?", "id": "Menggunakan Beberapa Kabel Per Fasa." },
  { "en": "Apa Keuntungan Bundle Conductor?", "id": "Mengurangi Korona Dan Reaktansi Induktif." },
  { "en": "Apa Itu Transposisi Saluran?", "id": "Menukar Posisi Fasa Sepanjang Jalur." },
  { "en": "Apa Tujuan Transposisi?", "id": "Menyeimbangkan Tegangan Dan Arus Induksi." },
  { "en": "Apa Itu Saluran Transmisi Pendek?", "id": "Panjang Kurang Dari 80 Kilometer." },
  { "en": "Apa Itu Saluran Transmisi Menengah?", "id": "Panjang Antara 80 Hingga 240 Kilometer." },
  { "en": "Apa Itu Saluran Transmisi Panjang?", "id": "Panjang Lebih Dari 240 Kilometer." },
  { "en": "Apa Itu Model Pi Saluran?", "id": "Representasi Kapasitansi Di Kedua Ujung." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
