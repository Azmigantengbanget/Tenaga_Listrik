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


  { "en": "Satuan tegangan listrik?", "id": "Volt (V)." },
  { "en": "Satuan arus listrik?", "id": "Ampere (A)." },
  { "en": "Satuan daya listrik nyata?", "id": "Watt (W)." },
  { "en": "Satuan daya reaktif?", "id": "Volt Ampere Reactive (VAR)." },
  { "en": "Satuan daya semu?", "id": "Volt Ampere (VA)." },
  { "en": "Rumus dasar hukum Ohm?", "id": "V = I x R." },
  { "en": "Komponen utama PLTA (Pembangkit Listrik Tenaga Air)?", "id": "Turbin dan generator." },
  { "en": "Sumber energi PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Batu bara atau gas." },
  { "en": "Fungsi utama transformator?", "id": "Menaikkan atau menurunkan tegangan." },
  { "en": "Alat pengaman lebur?", "id": "Sekring atau fuse." },
  { "en": "Frekuensi standar kelistrikan Indonesia?", "id": "50 Hertz (Hz)." },
  { "en": "Kepanjangan dari SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Saluran Udara Tegangan Ekstra Tinggi." },
  { "en": "Tegangan pada SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "500 kiloVolt (kV)." },
  { "en": "Bahan utama konduktor listrik?", "id": "Aluminium atau tembaga." },
  { "en": "Fungsi utama isolator jaringan?", "id": "Menyekat bagian bertegangan." },
  { "en": "Kepanjangan dari PLN (Perusahaan Listrik Negara)?", "id": "Perusahaan Listrik Negara." },
  { "en": "Alat pengubah energi mekanik ke listrik?", "id": "Generator." },
  { "en": "Alat pengubah energi listrik ke mekanik?", "id": "Motor listrik." },
  { "en": "Bagian berputar pada motor?", "id": "Rotor." },
  { "en": "Bagian diam pada motor?", "id": "Stator." },
  { "en": "Fungsi CB (Circuit Breaker)?", "id": "Memutus arus gangguan secara otomatis." },
  { "en": "Kepanjangan dari MCB (Miniature Circuit Breaker)?", "id": "Miniature Circuit Breaker." },
  { "en": "Fungsi utama gardu induk?", "id": "Mentransformasi dan mendistribusikan daya." },
  { "en": "Kepanjangan dari JTR (Jaringan Tegangan Rendah)?", "id": "Jaringan Tegangan Rendah." },
  { "en": "Besar tegangan JTR (Jaringan Tegangan Rendah)?", "id": "220/380 Volt." },
  { "en": "Kepanjangan dari JTM (Jaringan Tegangan Menengah)?", "id": "Jaringan Tegangan Menengah." },
  { "en": "Besar tegangan JTM (Jaringan Tegangan Menengah)?", "id": "20 kiloVolt (kV)." },
  { "en": "Sistem pentanahan disebut juga?", "id": "Grounding." },
  { "en": "Fungsi utama sistem pentanahan?", "id": "Mengamankan dari tegangan sentuh." },
  { "en": "Apa itu segitiga daya?", "id": "Hubungan daya nyata, reaktif, semu." },
  { "en": "Faktor daya yang baik nilainya?", "id": "Mendekati satu." },
  { "en": "Alat untuk memperbaiki faktor daya?", "id": "Kapasitor bank." },
  { "en": "Pembangkit yang menggunakan tenaga angin?", "id": "PLTB (Pembangkit Listrik Tenaga Bayu)." },
  { "en": "Pembangkit yang menggunakan tenaga nuklir?", "id": "PLTN (Pembangkit Listrik Tenaga Nuklir)." },
  { "en": "Kepanjangan dari PMT (Pemutus Tenaga)?", "id": "Pemutus Tenaga." },
  { "en": "Fungsi PMT (Pemutus Tenaga)?", "id": "Memutus arus beban atau gangguan." },
  { "en": "Kepanjangan dari PMS (Pemisah)?", "id": "Pemisah." },
  { "en": "Fungsi PMS (Pemisah)?", "id": "Memisahkan instalasi tanpa arus." },
  { "en": "Perbedaan utama PMT dan PMS?", "id": "PMT berbeban, PMS tidak." },
  { "en": "Sumber energi PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Cahaya matahari." },
  { "en": "Komponen utama PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Panel surya dan inverter." },
  { "en": "Alat ukur tegangan listrik?", "id": "Voltmeter." },
  { "en": "Alat ukur arus listrik?", "id": "Amperemeter." },
  { "en": "Alat ukur daya listrik?", "id": "Wattmeter." },
  { "en": "Alat ukur energi listrik di rumah?", "id": "kWh meter." },
  { "en": "Jenis arus listrik?", "id": "Searah (DC) dan bolak-balik (AC)." },
  { "en": "Sistem transmisi listrik Indonesia menggunakan arus?", "id": "Arus bolak-balik (AC)." },
  { "en": "Bahan isolator yang umum?", "id": "Keramik atau polimer." },
  { "en": "Penyebab utama rugi-rugi transmisi?", "id": "Resistansi pada konduktor." },
  { "en": "Bagaimana cara mengurangi rugi transmisi?", "id": "Dengan menaikkan tegangan transmisi." },
  { "en": "Apa itu sistem tiga fasa?", "id": "Sistem listrik tiga kawat AC." },
  { "en": "Sudut beda fasa sistem tiga fasa?", "id": "120 derajat." },
  { "en": "Hubungan belitan pada sistem tiga fasa?", "id": "Hubungan bintang (Y) dan segitiga (Δ)." },
  { "en": "Tegangan fasa ke netral disebut?", "id": "Tegangan fasa." },
  { "en": "Tegangan antar fasa disebut?", "id": "Tegangan line." },
  { "en": "Kepanjangan dari GFR (Ground Fault Relay)?", "id": "Ground Fault Relay." },
  { "en": "Fungsi GFR (Ground Fault Relay)?", "id": "Mendeteksi gangguan tanah." },
  { "en": "Kepanjangan dari OCR (Over Current Relay)?", "id": "Over Current Relay." },
  { "en": "Fungsi OCR (Over Current Relay)?", "id": "Mendeteksi arus lebih." },
  { "en": "Bagian generator yang menghasilkan medan magnet?", "id": "Rotor (sistem eksitasi)." },
  { "en": "Bagian generator tempat timbulnya tegangan?", "id": "Stator (kumparan jangkar)." },
  { "en": "Apa itu sistem interkoneksi?", "id": "Jaringan listrik yang saling terhubung." },
  { "en": "Keuntungan sistem interkoneksi?", "id": "Keandalan sistem meningkat." },
  { "en": "Pendingin utama pada trafo daya besar?", "id": "Oli transformator." },
  { "en": "Fungsi oli pada trafo?", "id": "Sebagai pendingin dan isolasi." },
  { "en": "Bagian trafo untuk mengatur tegangan?", "id": "Tap changer." },
  { "en": "Pembangkit yang cepat untuk start?", "id": "PLTG (Pembangkit Listrik Tenaga Gas)." },
  { "en": "Pembangkit untuk beban puncak?", "id": "PLTG atau PLTA." },
  { "en": "Pembangkit untuk beban dasar?", "id": "PLTU atau PLTN." },
  { "en": "Fungsi lightning arrester?", "id": "Melindungi peralatan dari sambaran petir." },
  { "en": "Apa itu black out?", "id": "Padam total sistem kelistrikan." },
  { "en": "Komponen untuk sinkronisasi generator?", "id": "Synchroscope." },
  { "en": "Syarat utama sinkronisasi generator?", "id": "Tegangan, frekuensi, urutan fasa sama." },
  { "en": "Kepanjangan dari SCADA (Supervisory Control and Data Acquisition)?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Fungsi SCADA pada sistem tenaga?", "id": "Monitoring dan kontrol jarak jauh." },
  { "en": "Jenis tiang transmisi?", "id": "Tiang baja (tower)." },
  { "en": "Jenis tiang distribusi?", "id": "Tiang beton atau besi." },
  { "en": "Fenomena pelepasan muatan listrik di isolator?", "id": "Corona." },
  { "en": "Efek dari corona?", "id": "Menimbulkan rugi daya." },
  { "en": "Jaringan kabel bawah tanah disebut?", "id": "SKTM (Saluran Kabel Tegangan Menengah)." },
  { "en": "Kelebihan kabel bawah tanah?", "id": "Lebih aman dan estetis." },
  { "en": "Kekurangan kabel bawah tanah?", "id": "Biaya mahal, perbaikan sulit." },
  { "en": "Apa itu motor induksi?", "id": "Motor AC yang paling umum." },
  { "en": "Apa itu slip pada motor induksi?", "id": "Perbedaan putaran rotor dan stator." },
  { "en": "Bagaimana cara membalik putaran motor 3 fasa?", "id": "Menukar dua dari tiga fasanya." },
  { "en": "Alat proteksi motor dari beban lebih?", "id": "TOR (Thermal Overload Relay)." },
  { "en": "Fungsi kapasitor pada motor 1 fasa?", "id": "Membantu putaran awal (starting)." },
  { "en": "Apa itu busbar?", "id": "Batang konduktor pengumpul daya." },
  { "en": "Bahan busbar umumnya?", "id": "Tembaga atau aluminium." },
  { "en": "Apa itu gardu distribusi?", "id": "Tempat trafo distribusi ke pelanggan." },
  { "en": "Alat ukur resistansi isolasi?", "id": "Insulation Tester (Megger)." },
  { "en": "Tujuan mengukur resistansi isolasi?", "id": "Mengetahui kondisi isolasi peralatan." },
  { "en": "Apa itu gangguan hubung singkat?", "id": "Hubungan antar fasa atau fasa-tanah." },
  { "en": "Akibat dari hubung singkat?", "id": "Arus sangat besar, merusak alat." },
  { "en": "Sistem eksitasi pada generator berfungsi?", "id": "Menghasilkan medan magnet." },
  { "en": "Kepanjangan dari AVR (Automatic Voltage Regulator)?", "id": "Automatic Voltage Regulator." },
  { "en": "Fungsi AVR (Automatic Voltage Regulator)?", "id": "Menjaga tegangan output generator stabil." },
  { "en": "Apa itu transformator step-up?", "id": "Transformator untuk menaikkan tegangan." },
  { "en": "Apa itu transformator step-down?", "id": "Transformator untuk menurunkan tegangan." },
  { "en": "Apa itu rugi-rugi tembaga?", "id": "Rugi daya akibat resistansi belitan." },
  { "en": "Apa itu rugi-rugi inti besi?", "id": "Rugi akibat histerisis dan eddy." },
  { "en": "Rugi tembaga bergantung pada?", "id": "Arus beban." },
  { "en": "Rugi inti besi bergantung pada?", "id": "Tegangan dan frekuensi." },
  { "en": "Efek Ferranti terjadi pada?", "id": "Saluran transmisi panjang tanpa beban." },
  { "en": "Apa itu Efek Ferranti?", "id": "Tegangan ujung penerima lebih tinggi." },
  { "en": "Apa itu Efek Kulit (Skin Effect)?", "id": "Arus mengalir di permukaan konduktor." },
  { "en": "Komponen pemanas air di PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Boiler atau ketel uap." },
  { "en": "Fungsi kondensor di PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Mengubah uap menjadi air kembali." },
  { "en": "Bahan bakar PLTD (Pembangkit Listrik Tenaga Diesel)?", "id": "Solar (minyak diesel)." },
  { "en": "Kepanjangan dari PLTGGU (Pembangkit Listrik Tenaga Gas Uap)?", "id": "Pembangkit Listrik Tenaga Gas Uap." },
  { "en": "Keunggulan PLTGGU (Pembangkit Listrik Tenaga Gas Uap)?", "id": "Efisiensi termal sangat tinggi." },
  { "en": "Generator yang berputar serempak dengan frekuensi?", "id": "Generator sinkron." },
  { "en": "Generator yang putarannya di bawah sinkron?", "id": "Generator asinkron (induksi)." },
  { "en": "Fungsi kawat tanah di SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Melindungi dari sambaran petir langsung." },
  { "en": "Jenis isolator pada SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Isolator gantung (suspension insulator)." },
  { "en": "Jenis isolator pada JTM (Jaringan Tegangan Menengah)?", "id": "Isolator jenis pin (pin insulator)." },
  { "en": "Kepanjangan dari ROW (Right of Way)?", "id": "Right of Way." },
  { "en": "Fungsi ROW (Right of Way)?", "id": "Area aman di bawah transmisi." },
  { "en": "Alat proteksi yang bekerja membuka-menutup otomatis?", "id": "Recloser (penutup balik otomatis)." },
  { "en": "Fungsi Sectionalizer?", "id": "Memisahkan bagian jaringan yang terganggu." },
  { "en": "Kepanjangan dari LBS (Load Break Switch)?", "id": "Load Break Switch." },
  { "en": "Fungsi LBS (Load Break Switch)?", "id": "Memutus dan menyambung arus beban." },
  { "en": "Konfigurasi jaringan distribusi paling sederhana?", "id": "Jaringan radial." },
  { "en": "Kelemahan jaringan radial?", "id": "Jika ada gangguan, suplai padam." },
  { "en": "Jaringan distribusi dengan dua sumber pasokan?", "id": "Jaringan loop (cincin)." },
  { "en": "Kepanjangan dari CT (Current Transformer)?", "id": "Current Transformer." },
  { "en": "Fungsi CT (Current Transformer)?", "id": "Menurunkan arus untuk pengukuran/proteksi." },
  { "en": "Kepanjangan dari PT (Potential Transformer)?", "id": "Potential Transformer." },
  { "en": "Fungsi PT (Potential Transformer)?", "id": "Menurunkan tegangan untuk pengukuran/proteksi." },
  { "en": "Relay untuk mengamankan trafo dari gangguan internal?", "id": "Relay diferensial." },
  { "en": "Prinsip kerja relay diferensial?", "id": "Membandingkan arus masuk dan keluar." },
  { "en": "Relay untuk proteksi saluran transmisi?", "id": "Relay jarak (distance relay)." },
  { "en": "Kepanjangan dari APD (Alat Pelindung Diri)?", "id": "Alat Pelindung Diri." },
  { "en": "Contoh APD (Alat Pelindung Diri) kelistrikan?", "id": "Helm, sarung tangan, sepatu isolasi." },
  { "en": "Gangguan kelipatan frekuensi fundamental?", "id": "Harmonisa." },
  { "en": "Penyebab utama harmonisa?", "id": "Beban non-linear seperti elektronik." },
  { "en": "Penurunan tegangan sesaat disebut?", "id": "Kedip tegangan (voltage sag)." },
  { "en": "Kenaikan tegangan sesaat disebut?", "id": "Lonjakan tegangan (voltage swell)." },
  { "en": "Lemari panel listrik tegangan menengah?", "id": "Kubikel (Cubicle)." },
  { "en": "Metode starting motor induksi daya besar?", "id": "Star-delta starter." },
  { "en": "Tujuan starting star-delta?", "id": "Mengurangi arus start motor." },
  { "en": "Pendingin trafo ONAN (Oil Natural Air Natural)?", "id": "Pendinginan oli dan udara alami." },
  { "en": "Pendingin trafo ONAF (Oil Natural Air Forced)?", "id": "Pendinginan oli alami, udara paksa." },
  { "en": "Perbedaan ONAF dan ONAN?", "id": "ONAF menggunakan kipas pendingin." },
  { "en": "Informasi spesifikasi motor terdapat di?", "id": "Papan nama (nameplate)." },
  { "en": "Satuan fluks magnetik?", "id": "Weber (Wb)." },
  { "en": "Satuan kerapatan fluks magnetik?", "id": "Tesla (T)." },
  { "en": "Bagian dari sistem proteksi petir eksternal?", "id": "Air terminal (finial)." },
  { "en": "Bagian trafo yang menghubungkan belitan ke luar?", "id": "Bushing." },
  { "en": "Relay yang mendeteksi gelembung gas pada trafo?", "id": "Relay Bucholz." },
  { "en": "Penyebab munculnya gas di trafo?", "id": "Hubung singkat atau pemanasan berlebih." },
  { "en": "Apa itu pemeliharaan prediktif?", "id": "Pemeliharaan berdasarkan kondisi alat." },
  { "en": "Kepanjangan dari APP (Alat Pengukur dan Pembatas)?", "id": "Alat Pengukur dan Pembatas." },
  { "en": "Contoh dari APP (Alat Pengukur dan Pembatas)?", "id": "kWh meter dan MCB." },
  { "en": "Apa itu susut jaringan?", "id": "Selisih energi dikirim dan dijual." },
  { "en": "Pembangkit yang memanfaatkan panas bumi?", "id": "PLTP (Pembangkit Listrik Tenaga Panas Bumi)." },
  { "en": "Pengaturan urutan kerja peralatan proteksi?", "id": "Koordinasi proteksi." },
  { "en": "Tujuan koordinasi proteksi?", "id": "Melokalisir area gangguan seminimal mungkin." },
  { "en": "Sekering dengan kemampuan pemutusan arus tinggi?", "id": "Sekering tipe HRC (High Rupturing Capacity)." },
  { "en": "Simbol fasa R, S, T berwarna?", "id": "Merah, kuning, hitam." },
  { "en": "Warna kabel untuk netral?", "id": "Biru." },
  { "en": "Warna kabel untuk ground/tanah?", "id": "Hijau-kuning." },
  { "en": "Bagian dasar dari tower transmisi?", "id": "Pondasi." },
  { "en": "Jenis pondasi tower disesuaikan dengan?", "id": "Kondisi tanah." },
  { "en": "Apa itu konduktansi?", "id": "Kemampuan menghantarkan arus listrik." },
  { "en": "Apa itu reaktansi?", "id": "Hambatan terhadap arus bolak-balik." },
  { "en": "Reaktansi disebabkan oleh?", "id": "Induktor dan kapasitor." },
  { "en": "Satuan konduktansi, suseptansi, admitansi?", "id": "Siemens (S)." },
  { "en": "Kebalikan dari resistansi?", "id": "Konduktansi." },
  { "en": "Kebalikan dari reaktansi?", "id": "Suseptansi." },
  { "en": "Kebalikan dari impedansi?", "id": "Admitansi." },
  { "en": "Jaringan distribusi dari gardu ke pelanggan?", "id": "Jaringan sekunder." },
  { "en": "Jaringan distribusi antar gardu?", "id": "Jaringan primer." },
  { "en": "Alat untuk menaikkan tegangan DC?", "id": "DC-DC Boost Converter." },
  { "en": "Alat untuk menurunkan tegangan DC?", "id": "DC-DC Buck Converter." },
  { "en": "Apa itu pemeliharaan preventif?", "id": "Pemeliharaan terjadwal untuk mencegah kerusakan." },
  { "en": "Apa itu pemeliharaan korektif?", "id": "Perbaikan setelah terjadi kerusakan." },
  { "en": "Tegangan yang tertera di nameplate disebut?", "id": "Tegangan nominal." },
  { "en": "Arus yang tertera di nameplate disebut?", "id": "Arus nominal." },
  { "en": "Kepanjangan dari LOTO (Lock Out Tag Out)?", "id": "Lock Out Tag Out." },
  { "en": "Tujuan LOTO (Lock Out Tag Out)?", "id": "Mengisolasi sumber energi saat perbaikan." },
  { "en": "Apa itu andongan (sag) kabel?", "id": "Lengkungan kabel di antara tiang." },
  { "en": "Fungsi andongan (sag)?", "id": "Mengurangi tegangan tarik mekanis kabel." },
  { "en": "Bagian motor yang dialiri arus eksitasi?", "id": "Belitan medan (field winding)." },
  { "en": "Sumber eksitasi generator sinkron?", "id": "Exciter." },
  { "en": "Jenis exciter pada generator?", "id": "Statis atau tanpa sikat (brushless)." },
  { "en": "Apa itu drop tegangan?", "id": "Penurunan tegangan di sepanjang saluran." },
  { "en": "Penyebab utama drop tegangan?", "id": "Impedansi saluran dan arus beban." },
  { "en": "Batas toleransi drop tegangan di JTR (Jaringan Tegangan Rendah)?", "id": "Sekitar 5 persen." },
  { "en": "Pembangkit yang ramah lingkungan?", "id": "Pembangkit EBT (Energi Baru Terbarukan)." },
  { "en": "Contoh Pembangkit EBT (Energi Baru Terbarukan)?", "id": "PLTS, PLTB, PLTA." },
  { "en": "Konsep jaringan listrik modern dan cerdas?", "id": "Smart grid." },
  { "en": "Komponen yang menghubungkan panel surya ke grid?", "id": "Grid-tie inverter." },
  { "en": "Sistem proteksi dari kejut listrik?", "id": "ELCB (Earth Leakage Circuit Breaker)." },
  { "en": "Sensitivitas ELCB (Earth Leakage Circuit Breaker) diukur dalam?", "id": "miliAmpere (mA)." },
  { "en": "Instalasi listrik di Indonesia mengacu pada PUIL (Peraturan Umum Instalasi Listrik)?", "id": "Benar, sebagai standar utama." },
  { "en": "Standar produk kelistrikan PLN (Perusahaan Listrik Negara) disebut?", "id": "SPLN (Standar Perusahaan Listrik Negara)." },
  { "en": "Kode numerik relay proteksi diatur oleh standar ANSI (American National Standards Institute)?", "id": "Benar, menggunakan kode angka." },
  { "en": "Angka '51' pada standar ANSI (American National Standards Institute) adalah relay?", "id": "Relay arus lebih (Overcurrent Relay)." },
  { "en": "Representasi sistem tenaga dalam satu garis disebut?", "id": "Single Line Diagram." },
  { "en": "Area luar Gardu Induk disebut?", "id": "Switchyard atau lapangan hubung." },
  { "en": "Pusat kendali Gardu Induk?", "id": "Rumah kontrol (control building)." },
  { "en": "Perangkat antarmuka SCADA di gardu adalah RTU (Remote Terminal Unit)?", "id": "Benar, untuk telemetri dan telekontrol." },
  { "en": "Fungsi utama RTU (Remote Terminal Unit)?", "id": "Mengirim data dan menerima perintah." },
  { "en": "Protokol komunikasi modern untuk gardu induk?", "id": "IEC 61850." },
  { "en": "Apa media isolasi utama pada GIS (Gas Insulated Substation)?", "id": "Gas SF6 (Sulfur Heksafluorida)." },
  { "en": "Keunggulan utama GIS (Gas Insulated Substation)?", "id": "Dimensi lebih kecil, hemat tempat." },
  { "en": "Gas pemadam busur api pada PMT (Pemutus Tenaga) modern?", "id": "Gas SF6 (Sulfur Heksafluorida)." },
  { "en": "Pengatur putaran turbin untuk menjaga frekuensi adalah?", "id": "Governor." },
  { "en": "Fungsi utama governor pada generator?", "id": "Mengatur putaran dan frekuensi." },
  { "en": "Pengujian trafo tanpa beban untuk mengukur?", "id": "Rugi inti besi." },
  { "en": "Pengujian trafo hubung singkat untuk mengukur?", "id": "Rugi tembaga." },
  { "en": "Informasi pergeseran fasa belitan trafo disebut?", "id": "Grup vektor (vector group)." },
  { "en": "Relay yang melindungi dari putaran berlebih?", "id": "Relay kecepatan lebih (overspeed)." },
  { "en": "Fungsi UVR (Under Voltage Relay)?", "id": "Proteksi dari tegangan terlalu rendah." },
  { "en": "Fungsi OFR (Over Frequency Relay)?", "id": "Proteksi dari frekuensi terlalu tinggi." },
  { "en": "Relay arus lebih yang memiliki elemen arah?", "id": "DOCR (Directional Over Current Relay)." },
  { "en": "Bahan isolasi kabel TM (Tegangan Menengah) yang populer?", "id": "XLPE (Cross-linked Polyethylene)." },
  { "en": "Bahan isolasi kabel TR (Tegangan Rendah) yang umum?", "id": "PVC (Polyvinyl Chloride)." },
  { "en": "Batas kemampuan kabel dialiri arus disebut?", "id": "KHA (Kapasitas Hantar Arus)." },
  { "en": "Faktor yang mempengaruhi KHA (Kapasitas Hantar Arus) kabel?", "id": "Suhu lingkungan." },
  { "en": "Ukuran distorsi gelombang akibat harmonisa?", "id": "THD (Total Harmonic Distortion)." },
  { "en": "Batas toleransi THD (Total Harmonic Distortion) tegangan di sistem?", "id": "Biasanya di bawah 5 persen." },
  { "en": "Alat untuk meredam harmonisa?", "id": "Filter harmonisa (pasif/aktif)." },
  { "en": "Fluktuasi tegangan yang menyebabkan lampu berkedip?", "id": "Voltage Flicker." },
  { "en": "Gangguan tegangan sangat cepat dan tinggi?", "id": "Transien (transient)." },
  { "en": "Generator sinkron untuk kompensasi daya reaktif?", "id": "Kondensor sinkron." },
  { "en": "Fungsi utama kondensor sinkron?", "id": "Menyuplai atau menyerap daya reaktif." },
  { "en": "Peralatan elektronik daya untuk mengatur daya reaktif?", "id": "SVC (Static VAR Compensator)." },
  { "en": "Kepanjangan dari FACTS (Flexible AC Transmission Systems)?", "id": "Flexible AC Transmission Systems." },
  { "en": "Tujuan utama perangkat FACTS?", "id": "Meningkatkan kendali dan kapasitas transmisi." },
  { "en": "Metode menghentikan motor dengan membalik fasa?", "id": "Pengereman plugging." },
  { "en": "Jenis pengereman selain mekanik pada motor?", "id": "Pengereman dinamik (dynamic braking)." },
  { "en": "Jenis motor yang kecepatannya paling mudah diatur?", "id": "Motor DC (Direct Current)." },
  { "en": "Komponen motor DC (Direct Current) untuk mengubah arah arus?", "id": "Komutator (commutator)." },
  { "en": "Komponen yang menyalurkan arus ke komutator?", "id": "Sikat arang (carbon brush)." },
  { "en": "Sumber listrik cadangan saat listrik padam?", "id": "UPS (Uninterruptible Power Supply)." },
  { "en": "Komponen utama UPS (Uninterruptible Power Supply)?", "id": "Rectifier, baterai, inverter." },
  { "en": "Fungsi rectifier pada UPS (Uninterruptible Power Supply)?", "id": "Mengubah AC ke DC." },
  { "en": "Fungsi inverter pada UPS (Uninterruptible Power Supply)?", "id": "Mengubah DC ke AC." },
  { "en": "Urutan fasa standar sistem 3 fasa?", "id": "R-S-T." },
  { "en": "Alat untuk memeriksa urutan fasa?", "id": "Phase sequence indicator." },
  { "en": "Impedansi yang dirasakan arus gangguan tanah?", "id": "Impedansi urutan nol." },
  { "en": "Impedansi sistem saat kondisi seimbang?", "id": "Impedansi urutan positif." },
  { "en": "Impedansi yang muncul saat sistem tak seimbang?", "id": "Impedansi urutan negatif." },
  { "en": "Trafo yang menurunkan tegangan ke pelanggan?", "id": "Transformator distribusi." },
  { "en": "Trafo tegangan tinggi di gardu induk?", "id": "Transformator daya (power transformer)." },
  { "en": "Sisi sekunder trafo distribusi umumnya terhubung?", "id": "Hubungan Bintang (Y) dengan netral." },
  { "en": "Tujuan titik netral trafo ditanahkan?", "id": "Sebagai referensi tegangan dan proteksi." },
  { "en": "Sistem pentanahan TT (Terre-Terre) artinya?", "id": "Netral dan bodi ditanahkan terpisah." },
  { "en": "Sistem pentanahan TN (Terre-Neutre) artinya?", "id": "Netral dan bodi ditanahkan bersama." },
  { "en": "Konduktor utama pembagi daya di panel?", "id": "Busbar atau rel daya." },
  { "en": "Satu set peralatan untuk satu saluran di gardu?", "id": "Bay." },
  { "en": "Contoh penamaan bay di gardu induk?", "id": "Bay trafo atau bay penghantar." },
  { "en": "Isolator penyangga konduktor pada trafo?", "id": "Bushing." },
  { "en": "Pengujian oli trafo untuk mengetahui kekuatan isolasi?", "id": "Uji tegangan tembus (breakdown voltage)." },
  { "en": "Tujuan uji tegangan tembus oli trafo?", "id": "Mengukur kekuatan dielektrik oli." },
  { "en": "Bahan penyerap uap air pada trafo?", "id": "Gel silika (silica gel)." },
  { "en": "Warna gel silika yang masih baik?", "id": "Biru." },
  { "en": "Warna gel silika yang sudah jenuh air?", "id": "Merah muda atau putih." },
  { "en": "Bagian trafo untuk sirkulasi udara?", "id": "Pernapasan (breather)." },
  { "en": "Tangki cadangan tempat oli trafo memuai?", "id": "Konservator." },
  { "en": "Jenis APAR (Alat Pemadam Api Ringan) untuk listrik?", "id": "Jenis CO2 atau dry powder." },
  { "en": "APAR (Alat Pemadam Api Ringan) yang tidak boleh untuk listrik?", "id": "Jenis air atau busa." },
  { "en": "Analisis aliran daya dan tegangan di jaringan?", "id": "Studi aliran daya (load flow)." },
  { "en": "Tujuan utama studi aliran daya?", "id": "Mengetahui kondisi operasi sistem." },
  { "en": "Pusat yang mengatur operasi sistem tenaga listrik?", "id": "Pusat Pengatur Beban (Dispatcher)." },
  { "en": "Apa itu cadangan berputar (spinning reserve)?", "id": "Kapasitas generator yang siap dipakai." },
  { "en": "Tujuan adanya spinning reserve?", "id": "Menjaga stabilitas frekuensi saat gangguan." },
  { "en": "Pelepasan beban untuk menyelamatkan sistem?", "id": "Load shedding." },
  { "en": "Kondisi saat permintaan listrik tertinggi?", "id": "Beban puncak (peak load)." },
  { "en": "Rasio antara beban rata-rata dan beban puncak?", "id": "Faktor beban (load factor)." },
  { "en": "Nilai faktor beban yang baik?", "id": "Mendekati 1." },
  { "en": "Rasio daya terpasang dan permintaan serentak?", "id": "Faktor kebutuhan (demand factor)." },
  { "en": "Tegangan transient tinggi akibat switching atau petir?", "id": "Surja (surge)." },
  { "en": "Alat proteksi surja pada jaringan?", "id": "Arrester." },
  { "en": "Kapasitansi liar antara konduktor transmisi?", "id": "Kapasitansi shunt." },
  { "en": "Induktansi yang ada pada sepanjang konduktor?", "id": "Induktansi seri." },
  { "en": "Kabel yang menghubungkan panel ke APP?", "id": "Saluran Utama Pelayanan (SUP)." },
  { "en": "Kabel dari tiang ke rumah pelanggan?", "id": "Saluran Masuk Pelayanan (SMP)." },
  { "en": "Apa itu motor servo?", "id": "Motor untuk kontrol posisi presisi." },
  { "en": "Alat untuk soft starting motor induksi?", "id": "Soft starter." },
  { "en": "Pengatur kecepatan motor AC dengan mengubah frekuensi?", "id": "VFD (Variable Frequency Drive)." },
  { "en": "Kepanjangan VFD (Variable Frequency Drive)?", "id": "Variable Frequency Drive." },
  { "en": "Hubungan kumparan motor untuk tegangan tinggi?", "id": "Hubungan Bintang (Star)." },
  { "en": "Hubungan kumparan motor untuk tegangan rendah?", "id": "Hubungan Segitiga (Delta)." },
  { "en": "Apa itu transformator instrumen?", "id": "CT dan PT." },
  { "en": "Kelas akurasi penting untuk CT (Current Transformer) pengukuran?", "id": "Benar, untuk ketelitian kWh meter." },
  { "en": "Kelas proteksi penting untuk CT (Current Transformer) proteksi?", "id": "Benar, agar tidak cepat jenuh." },
  { "en": "Sisi sekunder CT (Current Transformer) tidak boleh?", "id": "Dibiarkan terbuka saat berbeban." },
  { "en": "Kemampuan sistem kembali normal setelah gangguan?", "id": "Stabilitas sistem tenaga." },
  { "en": "Stabilitas sistem saat gangguan besar tiba-tiba?", "id": "Stabilitas transien." },
  { "en": "Stabilitas sistem saat perubahan beban kecil?", "id": "Stabilitas keadaan tunak (steady-state)." },
  { "en": "Osilasi daya antar generator disebut?", "id": "Ayunan daya (power swing)." },
  { "en": "Alat untuk meredam ayunan daya?", "id": "Power System Stabilizer (PSS)." },
  { "en": "Jenis konduktor transmisi yang umum digunakan?", "id": "ACSR (Aluminum Conductor Steel Reinforced)." },
  { "en": "Fungsi inti baja pada konduktor ACSR?", "id": "Sebagai penguat mekanis." },
  { "en": "Tanduk pada isolator untuk proteksi busur api?", "id": "Tanduk api (arcing horn)." },
  { "en": "Fungsi utama tanduk api (arcing horn)?", "id": "Melindungi isolator dari busur api." },
  { "en": "Apa itu regulasi tegangan?", "id": "Perubahan tegangan antara berbeban dan nol." },
  { "en": "Nilai regulasi tegangan yang baik?", "id": "Nilai yang kecil (mendekati nol)." },
  { "en": "Alat untuk komunikasi data lewat jaringan listrik?", "id": "PLC (Power Line Communication)." },
  { "en": "Fungsi Line Trap atau Wave Trap?", "id": "Mencegah sinyal frekuensi tinggi masuk." },
  { "en": "Trafo tegangan yang menggunakan pembagi kapasitif?", "id": "CVT (Capacitor Voltage Transformer)." },
  { "en": "Keuntungan CVT (Capacitor Voltage Transformer) dibanding PT?", "id": "Lebih ekonomis untuk tegangan tinggi." },
  { "en": "Pembacaan meter pelanggan secara jarak jauh?", "id": "AMR (Automatic Meter Reading)." },
  { "en": "Manfaat utama AMR (Automatic Meter Reading)?", "id": "Efisiensi pencatatan dan akurasi data." },
  { "en": "Gardu distribusi yang dibangun di atas tiang?", "id": "Gardu portal." },
  { "en": "Gardu distribusi yang terpasang di satu tiang?", "id": "Gardu cantol." },
  { "en": "Gardu distribusi berbentuk bangunan kecil?", "id": "Gardu kios atau gardu beton." },
  { "en": "Pekerjaan pada jaringan tanpa memadamkan listrik?", "id": "PDKB (Pekerjaan Dalam Keadaan Bertegangan)." },
  { "en": "Tim yang melakukan PDKB (Pekerjaan Dalam Keadaan Bertegangan) disebut?", "id": "Tim PDKB." },
  { "en": "Jarak minimum aman dari bagian bertegangan?", "id": "Jarak aman (safety distance)." },
  { "en": "Osilasi pada generator sinkron disebut?", "id": "Hunting." },
  { "en": "Penyebab hunting pada generator?", "id": "Perubahan beban yang mendadak." },
  { "en": "Kumparan untuk meredam hunting pada generator?", "id": "Kumparan peredam (damper winding)." },
  { "en": "Metode starting pada motor sinkron?", "id": "Menggunakan motor induksi kecil." },
  { "en": "Batas temperatur operasi pada isolasi motor?", "id": "Kelas isolasi (misal F, H)." },
  { "en": "Kelas isolasi F pada motor berarti?", "id": "Batas suhu 155 derajat Celsius." },
  { "en": "Sistem PLTS (Pembangkit Listrik Tenaga Surya) yang terhubung ke grid?", "id": "Sistem on-grid." },
  { "en": "Sistem PLTS (Pembangkit Listrik Tenaga Surya) yang berdiri sendiri?", "id": "Sistem off-grid." },
  { "en": "Komponen utama sistem PLTS (Pembangkit Listrik Tenaga Surya) off-grid?", "id": "Panel surya, baterai, inverter." },
  { "en": "Sifat tidak menentu dari pembangkit EBT (Energi Baru Terbarukan)?", "id": "Intermittency." },
  { "en": "Kekuatan material isolasi menahan tegangan?", "id": "Kekuatan dielektrik." },
  { "en": "Satuan kekuatan dielektrik?", "id": "kV/mm atau V/m." },
  { "en": "Ukuran hambatan suatu material konduktor?", "id": "Resistivitas atau hambatan jenis." },
  { "en": "Satuan resistivitas?", "id": "Ohm-meter (Ω·m)." },
  { "en": "Fenomena resonansi antara kapasitansi dan induktansi inti trafo?", "id": "Ferroresonance." },
  { "en": "Dampak dari ferroresonance?", "id": "Menimbulkan tegangan lebih berbahaya." },
  { "en": "Kemampuan sistem untuk meredam osilasi?", "id": "Peredaman (damping)." },
  { "en": "Pemisah yang dilengkapi saklar pentanahan?", "id": "PMS (Pemisah) dengan saklar tanah." },
  { "en": "Fungsi saklar tanah (earthing switch)?", "id": "Membuang sisa muatan dan mengamankan." },
  { "en": "Prosedur mengoperasikan PMS (Pemisah) dan PMT (Pemutus Tenaga)?", "id": "Buka PMT dulu, baru PMS." },
  { "en": "Prosedur menutup PMS (Pemisah) dan PMT (Pemutus Tenaga)?", "id": "Tutup PMS dulu, baru PMT." },
  { "en": "Arus hubung singkat terbesar terjadi pada gangguan?", "id": "Gangguan tiga fasa." },
  { "en": "Gangguan yang paling sering terjadi di sistem?", "id": "Gangguan satu fasa ke tanah." },
  { "en": "Baterai yang umum digunakan di gardu induk?", "id": "Baterai asam timbal (lead-acid)." },
  { "en": "Jenis baterai kering bebas perawatan di gardu?", "id": "Baterai VRLA (Valve-Regulated Lead-Acid)." },
  { "en": "Fungsi baterai di gardu induk?", "id": "Sumber daya DC untuk proteksi." },
  { "en": "Alat pengisi daya baterai di gardu?", "id": "Rectifier atau battery charger." },
  { "en": "Apa itu sistem catu daya DC?", "id": "Sumber daya untuk relay dan kontrol." },
  { "en": "Tegangan sistem DC di gardu induk?", "id": "48V atau 110V DC." },
  { "en": "Apa itu arus bocor (leakage current)?", "id": "Arus kecil yang mengalir lewat isolasi." },
  { "en": "Peralatan untuk mengukur tahanan pentanahan?", "id": "Earth Tester." },
  { "en": "Nilai tahanan pentanahan yang baik?", "id": "Sekecil mungkin (di bawah 1 ohm)." },
  { "en": "Cara menurunkan nilai tahanan pentanahan?", "id": "Menambah batang elektroda, perbaikan tanah." },
  { "en": "Sistem proteksi cadangan jika proteksi utama gagal?", "id": "Proteksi cadangan (backup protection)." },
  { "en": "Pembagian daerah proteksi dalam sistem?", "id": "Zona proteksi." },
  { "en": "Prinsip zona proteksi?", "id": "Zona harus saling tumpang tindih." },
  { "en": "Perbedaan level tegangan antara MV (Medium Voltage) dan LV (Low Voltage)?", "id": "MV ribuan Volt, LV ratusan." },
  { "en": "Contoh tegangan MV (Medium Voltage)?", "id": "20 kV." },
  { "en": "Contoh tegangan LV (Low Voltage)?", "id": "220/380 V." },
  { "en": "Apa itu beban seimbang?", "id": "Beban pada tiga fasa sama." },
  { "en": "Apa itu beban tidak seimbang?", "id": "Beban pada tiga fasa berbeda." },
  { "en": "Akibat beban tidak seimbang?", "id": "Timbul arus di kawat netral." },
  { "en": "Relay untuk mendeteksi ketidakseimbangan beban?", "id": "Relay urutan negatif." },
  { "en": "Efisiensi transformator dihitung dari perbandingan?", "id": "Daya output dibagi daya input." },
  { "en": "Kondisi efisiensi trafo maksimum terjadi saat?", "id": "Rugi tembaga sama dengan rugi besi." },
  { "en": "Penyebab panas berlebih pada motor?", "id": "Beban lebih, ventilasi buruk, tegangan." },
  { "en": "Apa itu ventilasi paksa pada motor?", "id": "Menggunakan kipas eksternal untuk pendinginan." },
  { "en": "Apa itu faktor diversitas?", "id": "Rasio jumlah permintaan individu dan maksimum." },
  { "en": "Nilai faktor diversitas selalu?", "id": "Lebih besar dari satu." },
  { "en": "Apa itu transformator Zig-Zag?", "id": "Trafo khusus untuk membuat titik netral." },
  { "en": "Fungsi utama transformator Zig-Zag?", "id": "Untuk keperluan pentanahan sistem." },
  { "en": "Metode penyambungan kabel di bawah tanah?", "id": "Jointing." },
  { "en": "Ujung akhir dari kabel bawah tanah?", "id": "Terminasi." },
  { "en": "Fungsi terminasi kabel?", "id": "Menyambungkan kabel ke peralatan lain." },
  { "en": "Perawatan rutin pada switchyard gardu induk?", "id": "Pembersihan isolator." },
  { "en": "Mengapa isolator perlu dibersihkan?", "id": "Menghindari flashover akibat polusi." },
  { "en": "Fenomena loncatan api pada permukaan isolator?", "id": "Flashover." },
  { "en": "Apa itu tegangan impuls?", "id": "Tegangan naik sangat cepat (mikrodetik)." },
  { "en": "Pengujian peralatan terhadap tegangan impuls petir?", "id": "BIL (Basic Insulation Level)." },
  { "en": "Pengujian peralatan terhadap tegangan impuls hubung-buka?", "id": "SIL (Switching Insulation Level)." },
  { "en": "Apa itu isolasi SF6?", "id": "Gas SF6 sebagai media isolasi." },
  { "en": "Kekuatan dielektrik gas SF6 dibanding udara?", "id": "Jauh lebih tinggi." },
  { "en": "Dampak lingkungan dari gas SF6?", "id": "Gas rumah kaca sangat kuat." },
  { "en": "Proses penuaan pada material isolasi?", "id": "Degradasi isolasi." },
  { "en": "Penyebab degradasi isolasi?", "id": "Panas, tegangan, kelembaban, mekanis." },
  { "en": "Peralatan untuk mendeteksi hotspot pada sambungan?", "id": "Kamera thermovisi (infrared camera)." },
  { "en": "Apa itu hotspot pada sambungan listrik?", "id": "Titik panas akibat resistansi tinggi." },
  { "en": "Sistem suplai daya ganda ke konsumen penting?", "id": "Sistem catur daya." },
  { "en": "Saklar pemindah otomatis antar sumber?", "id": "ATS (Automatic Transfer Switch)." },
  { "en": "Fungsi ATS (Automatic Transfer Switch)?", "id": "Memindahkan suplai daya secara otomatis." },
  { "en": "Saklar pemindah antar sumber dari genset?", "id": "AMF (Automatic Main Failure)." },
  { "en": "Fungsi AMF (Automatic Main Failure)?", "id": "Menghidupkan genset saat PLN padam." },
  { "en": "Sistem pentanahan titik netral melalui tahanan?", "id": "Pentanahan dengan NGR (Neutral Grounding Resistor)." },
  { "en": "Tujuan pentanahan dengan NGR (Neutral Grounding Resistor)?", "id": "Membatasi arus gangguan tanah." },
  { "en": "Sistem pentanahan tanpa impedansi tambahan?", "id": "Pentanahan langsung (solidly grounded)." },
  { "en": "Kompensator busur api tanah?", "id": "Kumparan Petersen (Petersen Coil)." },
  { "en": "Kode kabel tanah berinti tembaga isolasi PVC?", "id": "Kabel NYY." },
  { "en": "Kode kabel dengan pelindung pita baja?", "id": "Kabel NYFGbY." },
  { "en": "Lapisan pelindung mekanis pada kabel?", "id": "Armor atau perisai baja." },
  { "en": "Lapisan untuk meratakan medan listrik pada kabel?", "id": "Screen atau lapisan semi-konduktif." },
  { "en": "Pengujian tegangan tinggi pada kabel?", "id": "Hipot test (High potential test)." },
  { "en": "Pengujian kabel dengan frekuensi sangat rendah?", "id": "Pengujian VLF (Very Low Frequency)." },
  { "en": "Perbaikan faktor daya secara elektronik?", "id": "APF (Active Power Filter)." },
  { "en": "Fungsi utama APF (Active Power Filter)?", "id": "Mengurangi harmonisa dan kompensasi daya reaktif." },
  { "en": "Pusat kontrol utama sistem SCADA?", "id": "Master Station atau Control Center." },
  { "en": "Sistem cadangan yang siap menggantikan sistem utama?", "id": "Sistem redundansi." },
  { "en": "Pembangkit yang beroperasi terus-menerus?", "id": "Pembangkit beban dasar (baseload plant)." },
  { "en": "Pembangkit yang beroperasi saat beban puncak?", "id": "Pembangkit beban puncak (peaker plant)." },
  { "en": "Ukuran efisiensi pembangkit termal?", "id": "Heat rate (laju kalor)." },
  { "en": "Nilai heat rate yang baik?", "id": "Nilai yang rendah." },
  { "en": "Pemanfaatan panas sisa untuk membangkitkan listrik?", "id": "Kogenerasi (cogeneration)." },
  { "en": "Pembangkit yang menerapkan kogenerasi?", "id": "PLTGU (Pembangkit Listrik Tenaga Gas Uap)." },
  { "en": "Biaya rata-rata untuk menghasilkan 1 kWh?", "id": "BPP (Biaya Pokok Produksi)." },
  { "en": "Harga jual listrik ke konsumen?", "id": "TTL (Tarif Tenaga Listrik)." },
  { "en": "Penyaluran tenaga listrik milik pihak lain di jaringan transmisi?", "id": "Wheeling." },
  { "en": "Jarak rambat busur api di permukaan isolator?", "id": "Jarak rambat (creepage distance)." },
  { "en": "Mengapa permukaan isolator dibuat berlekuk?", "id": "Memperpanjang jarak rambat (creepage distance)." },
  { "en": "Tingkat ketahanan isolasi terhadap surja petir?", "id": "BIL (Basic Insulation Level)." },
  { "en": "Tingkat ketahanan isolasi terhadap surja hubung-buka?", "id": "SIL (Switching Insulation Level)." },
  { "en": "Penyesuaian ketahanan isolasi peralatan dalam sistem?", "id": "Koordinasi isolasi." },
  { "en": "Peralatan dengan BIL (Basic Insulation Level) terendah?", "id": "Arrester." },
  { "en": "Fungsi skema proteksi kegagalan pemutus?", "id": "CBFP (Circuit Breaker Failure Protection)." },
  { "en": "Cara kerja CBFP (Circuit Breaker Failure Protection)?", "id": "Membuka PMT di sekitarnya." },
  { "en": "Fungsi yang mencegah PMT menutup-buka berulang kali?", "id": "Relay anti-pumping." },
  { "en": "Skema proteksi utama untuk busbar?", "id": "Proteksi diferensial busbar." },
  { "en": "Arus sesaat yang sangat tinggi saat trafo diberi energi?", "id": "Arus inrush (inrush current)." },
  { "en": "Karakteristik arus inrush trafo?", "id": "Kandungan harmonisa orde kedua tinggi." },
  { "en": "Analisis untuk menghitung arus gangguan?", "id": "Analisis hubung singkat." },
  { "en": "Data yang diperlukan untuk analisis hubung singkat?", "id": "Impedansi sumber dan jaringan." },
  { "en": "Analisis kestabilan sistem pasca-gangguan?", "id": "Analisis stabilitas transien." },
  { "en": "Apa itu sudut rotor generator?", "id": "Posisi sudut rotor terhadap medan putar." },
  { "en": "Stabilitas transien ditentukan oleh ayunan?", "id": "Ayunan sudut rotor." },
  { "en": "Sistem transmisi jarak jauh menggunakan arus searah?", "id": "Transmisi HVDC (High Voltage Direct Current)." },
  { "en": "Keuntungan transmisi HVDC (High Voltage Direct Current)?", "id": "Rugi transmisi lebih kecil." },
  { "en": "Stasiun pengubah AC ke DC di sistem HVDC?", "id": "Stasiun konverter." },
  { "en": "Stasiun pengubah DC ke AC di sistem HVDC?", "id": "Stasiun inverter." },
  { "en": "Alat ukur suhu belitan trafo?", "id": "Winding temperature indicator." },
  { "en": "Alat ukur suhu oli trafo?", "id": "Oil temperature indicator." },
  { "en": "Analisis gas terlarut dalam oli trafo?", "id": "DGA (Dissolved Gas Analysis)." },
  { "en": "Tujuan DGA (Dissolved Gas Analysis)?", "id": "Mendeteksi gangguan internal trafo dini." },
  { "en": "Gas indikasi sparking atau arcing ringan?", "id": "Asetilena (C2H2)." },
  { "en": "Gas indikasi panas berlebih (overheating)?", "id": "Etilena (C2H4) dan Metana (CH4)." },
  { "en": "Gas indikasi degradasi kertas isolasi?", "id": "Karbon dioksida dan monoksida." },
  { "en": "Apa itu partial discharge?", "id": "Pelepasan muatan listrik lokal kecil." },
  { "en": "Dampak dari partial discharge?", "id": "Merusak dan menua-kan isolasi." },
  { "en": "Apa itu rugi-rugi dielektrik?", "id": "Panas yang timbul pada isolasi." },
  { "en": "Ukuran rugi-rugi dielektrik?", "id": "Faktor disipasi (tan delta)." },
  { "en": "Nilai tan delta yang baik untuk isolasi?", "id": "Sangat kecil (mendekati nol)." },
  { "en": "Apa itu konduktor tipe bundle?", "id": "Satu fasa terdiri dari beberapa sub-konduktor." },
  { "en": "Tujuan konduktor tipe bundle?", "id": "Mengurangi efek corona dan reaktansi." },
  { "en": "Pemisah antar sub-konduktor pada tipe bundle?", "id": "Spacer." },
  { "en": "Getaran pada konduktor transmisi akibat angin?", "id": "Aeolian vibration." },
  { "en": "Alat untuk meredam getaran konduktor?", "id": "Vibration damper." },
  { "en": "Apa itu resistansi kaki menara?", "id": "Tahanan pentanahan tower transmisi." },
  { "en": "Tujuan menjaga resistansi kaki menara rendah?", "id": "Mencegah sambaran balik (back flashover)." },
  { "en": "Fenomena petir menyambar kawat tanah?", "id": "Sambaran balik (back flashover)." },
  { "en": "Tingkat keandalan sistem tenaga listrik?", "id": "Reliability." },
  { "en": "Indeks keandalan yang mengukur lama padam rata-rata?", "id": "SAIDI (System Average Interruption Duration Index)." },
  { "en": "Indeks keandalan yang mengukur seringnya padam rata-rata?", "id": "SAIFI (System Average Interruption Frequency Index)." },
  { "en": "Apa itu motor servo AC?", "id": "Motor AC untuk kontrol presisi." },
  { "en": "Apa itu motor stepper?", "id": "Motor yang bergerak per langkah sudut." },
  { "en": "Aplikasi motor stepper?", "id": "Printer, robotik, mesin CNC." },
  { "en": "Apa itu jatuh tegangan (voltage drop)?", "id": "Penurunan tegangan di sepanjang saluran." },
  { "en": "Jatuh tegangan disebabkan oleh?", "id": "Impedansi saluran." },
  { "en": "Pembangkit yang menggunakan energi ombak?", "id": "PLTO (Pembangkit Listrik Tenaga Ombak)." },
  { "en": "Pembangkit yang menggunakan pasang surut air laut?", "id": "PLTPs (Pembangkit Listrik Tenaga Pasang Surut)." },
  { "en": "Bahan semikonduktor utama untuk sel surya?", "id": "Silikon (Silicon)." },
  { "en": "Efisiensi panel surya komersial saat ini?", "id": "Sekitar 20-23 persen." },
  { "en": "Apa itu Maximum Power Point Tracking (MPPT)?", "id": "Algoritma untuk memaksimalkan daya surya." },
  { "en": "Komponen yang menjalankan algoritma MPPT?", "id": "Solar charge controller atau inverter." },
  { "en": "Apa itu sistem hibrida (hybrid system)?", "id": "Gabungan beberapa jenis sumber pembangkit." },
  { "en": "Contoh sistem hibrida?", "id": "PLTS digabung dengan diesel atau baterai." },
  { "en": "Alat yang menyatukan berbagai sumber ke beban?", "id": "Hybrid inverter." },
  { "en": "Apa itu islanding pada PLTS on-grid?", "id": "PLTS tetap menyuplai saat grid padam." },
  { "en": "Mengapa islanding berbahaya?", "id": "Membahayakan pekerja perbaikan jaringan." },
  { "en": "Proteksi yang mencegah islanding?", "id": "Anti-islanding protection." },
  { "en": "Apa itu penyulang (feeder) distribusi?", "id": "Saluran keluar dari gardu induk." },
  { "en": "Beban yang terhubung pada satu penyulang?", "id": "Beban penyulang." },
  { "en": "Manajemen sisi permintaan (demand-side management)?", "id": "Mempengaruhi pola konsumsi listrik pelanggan." },
  { "en": "Tujuan demand-side management?", "id": "Mengurangi beban puncak dan menunda investasi." },
  { "en": "Contoh program demand-side management?", "id": "Pemberlakuan tarif saat beban puncak." },
  { "en": "Tegangan pada baterai saat terisi penuh?", "id": "Tegangan float." },
  { "en": "Tegangan pengisian cepat pada baterai?", "id": "Tegangan boost atau equalize." },
  { "en": "Kapasitas baterai diukur dalam?", "id": "Ampere-hour (Ah)." },
  { "en": "Tingkat kedalaman pengosongan baterai?", "id": "Depth of Discharge (DoD)." },
  { "en": "Umur baterai dipengaruhi oleh?", "id": "Siklus cas-buang dan DoD." },
  { "en": "Pembagian beban ekonomis antar unit pembangkit?", "id": "Economic dispatch." },
  { "en": "Penjadwalan unit pembangkit yang akan beroperasi?", "id": "Unit commitment." },
  { "en": "Pelepasan beban otomatis saat frekuensi turun?", "id": "UFLS (Under-Frequency Load Shedding)." },
  { "en": "Pelepasan beban otomatis saat tegangan turun?", "id": "UVLS (Under-Voltage Load Shedding)." },
  { "en": "Konfigurasi busbar paling sederhana di gardu induk?", "id": "Busbar tunggal (single busbar)." },
  { "en": "Konfigurasi busbar dengan keandalan sangat tinggi?", "id": "Satu setengah pemutus (one-and-a-half breaker)." },
  { "en": "Keuntungan konfigurasi busbar ganda (double busbar)?", "id": "Fleksibilitas operasi dan pemeliharaan mudah." },
  { "en": "Perangkat kontrol dan proteksi untuk satu bay?", "id": "Bay Controller Unit (BCU)." },
  { "en": "Kotak terminal pengumpul kabel kontrol di switchyard?", "id": "Marshalling kiosk." },
  { "en": "Proteksi generator akibat hilangnya medan eksitasi?", "id": "Proteksi hilang eksitasi (loss of field)." },
  { "en": "Kode relay ANSI (American National Standards Institute) untuk hilang eksitasi?", "id": "Kode 40." },
  { "en": "Proteksi generator dari arus tak seimbang?", "id": "Proteksi urutan fasa negatif." },
  { "en": "Proteksi generator dari hubung singkat belitan stator ke tanah?", "id": "Proteksi stator earth fault." },
  { "en": "Proteksi generator dari kondisi motoring?", "id": "Proteksi daya balik (reverse power)." },
  { "en": "Kode relay ANSI (American National Standards Institute) untuk daya balik?", "id": "Kode 32." },
  { "en": "Proses penyambungan dua ujung kabel daya?", "id": "Penyambungan (jointing)." },
  { "en": "Asesoris ujung kabel untuk koneksi?", "id": "Terminasi kabel (cable termination)." },
  { "en": "Komponen terminasi untuk mengendalikan medan listrik?", "id": "Kerucut pereda stres (stress cone)." },
  { "en": "Sepatu kabel untuk koneksi ke terminal?", "id": "Skun kabel (cable lug)." },
  { "en": "Pengujian untuk memverifikasi rasio belitan trafo?", "id": "Uji TTR (Transformer Turn Ratio)." },
  { "en": "Alat untuk uji TTR (Transformer Turn Ratio)?", "id": "TTR meter." },
  { "en": "Alat untuk menguji tahanan isolasi?", "id": "Megger (Insulation Resistance Tester)." },
  { "en": "Satuan hasil pengukuran tahanan isolasi?", "id": "Megaohm (MΩ) atau Gigaohm (GΩ)." },
  { "en": "Perangkat pengaman tekanan lebih pada tangki trafo?", "id": "Pressure relief device." },
  { "en": "Kemampuan motor beroperasi di atas daya nominal?", "id": "Faktor servis (service factor)." },
  { "en": "Kode standar proteksi selungkup peralatan listrik?", "id": "Rating IP (Ingress Protection)." },
  { "en": "Angka pertama pada rating IP (Ingress Protection) menunjukkan?", "id": "Proteksi terhadap benda padat." },
  { "en": "Angka kedua pada rating IP (Ingress Protection) menunjukkan?", "id": "Proteksi terhadap benda cair." },
  { "en": "Metode pengereman motor yang mengembalikan energi ke sistem?", "id": "Pengereman regeneratif." },
  { "en": "Metode analisis gangguan tak seimbang?", "id": "Metode komponen simetris." },
  { "en": "Tiga komponen simetris adalah?", "id": "Urutan positif, negatif, dan nol." },
  { "en": "Komponen yang ada saat sistem seimbang?", "id": "Komponen urutan positif." },
  { "en": "Komponen yang timbul saat gangguan fasa-tanah?", "id": "Urutan positif, negatif, dan nol." },
  { "en": "Impedansi total sistem dilihat dari titik gangguan?", "id": "Impedansi Thevenin." },
  { "en": "Waktu rata-rata antar kerusakan pada suatu alat?", "id": "MTBF (Mean Time Between Failure)." },
  { "en": "Waktu rata-rata untuk memperbaiki kerusakan?", "id": "MTTR (Mean Time To Repair)." },
  { "en": "Strategi pemeliharaan yang berpusat pada keandalan?", "id": "RCM (Reliability Centered Maintenance)." },
  { "en": "Kementerian yang meregulasi sektor kelistrikan Indonesia?", "id": "Kementerian ESDM (Energi dan Sumber Daya Mineral)." },
  { "en": "Badan standardisasi kelistrikan internasional?", "id": "IEC (International Electrotechnical Commission)." },
  { "en": "Standar IEC (International Electrotechnical Commission) untuk panel listrik?", "id": "IEC 61439." },
  { "en": "Arus sirkuler yang timbul pada inti besi?", "id": "Arus eddy (eddy current)." },
  { "en": "Cara mengurangi arus eddy pada inti trafo?", "id": "Menggunakan inti dari pelat laminasi." },
  { "en": "Kerugian daya akibat pembalikan medan magnet?", "id": "Rugi histerisis." },
  { "en": "Lengkungan yang menggambarkan sifat magnetik material?", "id": "Kurva histerisis." },
  { "en": "Tegangan antara benda bertegangan dan tanah?", "id": "Tegangan sentuh (touch voltage)." },
  { "en": "Tegangan antara dua titik di permukaan tanah?", "id": "Tegangan langkah (step voltage)." },
  { "en": "Cara mengurangi tegangan langkah di gardu induk?", "id": "Memasang jaring pentanahan (ground grid)." },
  { "en": "Gabungan mesin diesel dan generator?", "id": "Genset (Generator Set)." },
  { "en": "Panel distribusi utama tegangan rendah?", "id": "LVMDP (Low Voltage Main Distribution Panel)." },
  { "en": "Panel distribusi utama tegangan menengah?", "id": "MVMDP (Medium Voltage Main Distribution Panel)." },
  { "en": "Panel untuk bank kapasitor tegangan rendah?", "id": "Panel kapasitor." },
  { "en": "Pengujian yang dilakukan di pabrik pembuat?", "id": "Uji jenis (type test)." },
  { "en": "Pengujian yang dilakukan setelah instalasi di lokasi?", "id": "Uji komisioning (commissioning test)." },
  { "en": "Peralatan untuk memutar poros turbin saat start awal?", "id": "Turning gear." },
  { "en": "Sistem pelumasan pada bearing turbin/generator?", "id": "Sistem oil-lube." },
  { "en": "Sistem pendingin untuk generator berkapasitas besar?", "id": "Pendingin hidrogen atau air." },
  { "en": "Keuntungan pendingin hidrogen pada generator?", "id": "Kepadatan rendah, pendinginan lebih baik." },
  { "en": "Apa itu black start capability?", "id": "Kemampuan unit pembangkit start tanpa listrik." },
  { "en": "Pembangkit yang umumnya punya kemampuan black start?", "id": "PLTA atau PLTD." },
  { "en": "Sistem proteksi busur api pada panel?", "id": "Arc flash protection." },
  { "en": "Bahaya utama dari busur api (arc flash)?", "id": "Suhu sangat tinggi, ledakan, cahaya." },
  { "en": "Apa itu studi hubung singkat?", "id": "Analisis arus gangguan hubung singkat." },
  { "en": "Tujuan studi hubung singkat?", "id": "Menentukan rating peralatan dan setelan proteksi." },
  { "en": "Tingkat arus gangguan tertinggi yang dapat diputus PMT?", "id": "Kapasitas pemutusan (breaking capacity)." },
  { "en": "Tingkat arus gangguan sesaat yang dapat ditahan peralatan?", "id": "Rating hubung singkat (short-circuit rating)." },
  { "en": "Relay yang mendeteksi arus urutan fasa negatif?", "id": "Relay NPS (Negative Phase Sequence)." },
  { "en": "Tahanan yang dipasang di titik netral trafo/generator?", "id": "NER (Neutral Earthing Resistor)." },
  { "en": "Fungsi NER (Neutral Earthing Resistor)?", "id": "Membatasi arus gangguan fasa-tanah." },
  { "en": "Apa itu sistem SCADA terdistribusi?", "id": "DCS (Distributed Control System)." },
  { "en": "Perbedaan SCADA dan DCS?", "id": "SCADA berorientasi pengawasan, DCS berorientasi proses." },
  { "en": "Proses menghubungkan generator ke grid?", "id": "Sinkronisasi." },
  { "en": "Alat untuk memonitor proses sinkronisasi?", "id": "Synchroscope." },
  { "en": "Kondisi 'lampu gelap' saat sinkronisasi menandakan?", "id": "Beda fasa nol, siap sinkron." },
  { "en": "Aliran daya reaktif dari generator ke sistem?", "id": "Generator over-excited (tereksitasi lebih)." },
  { "en": "Aliran daya reaktif dari sistem ke generator?", "id": "Generator under-excited (tereksitasi kurang)." },
  { "en": "Kurva kemampuan operasi generator?", "id": "Kurva kapabilitas (capability curve)." },
  { "en": "Batas operasi generator ditentukan oleh?", "id": "Batas pemanasan stator dan rotor." },
  { "en": "Apa itu transformator autotransformer?", "id": "Trafo dengan satu belitan saja." },
  { "en": "Keuntungan autotransformer?", "id": "Ukuran lebih kecil, efisiensi tinggi." },
  { "en": "Kelemahan autotransformer?", "id": "Tidak ada isolasi galvanis." },
  { "en": "Aplikasi autotransformer?", "id": "Starter motor, interkoneksi sistem beda tegangan." },
  { "en": "Apa itu transformator grounding?", "id": "Trafo untuk membuat titik netral." },
  { "en": "Perawatan preventif menggunakan data online?", "id": "Condition Based Maintenance (CBM)." },
  { "en": "Contoh CBM (Condition Based Maintenance)?", "id": "Monitoring DGA online pada trafo." },
  { "en": "Analisis kegagalan dan dampaknya?", "id": "FMEA (Failure Mode and Effect Analysis)." },
  { "en": "Kabel tegangan tinggi bawah laut?", "id": "Kabel laut (submarine cable)." },
  { "en": "Tantangan utama pemasangan kabel laut?", "id": "Kedalaman laut, arus, dan lingkungan." },
  { "en": "Lapisan terluar kabel laut untuk proteksi mekanis?", "id": "Steel wire armor." },
  { "en": "Apa itu daya dukung tanah?", "id": "Kemampuan tanah menahan beban pondasi." },
  { "en": "Pentingnya daya dukung tanah untuk tower?", "id": "Menentukan jenis dan kedalaman pondasi." },
  { "en": "Alat pemadam api otomatis di ruang panel?", "id": "Sistem fire suppression." },
  { "en": "Media pemadam pada sistem fire suppression?", "id": "Gas inert (FM200, Novec)." },
  { "en": "Studi untuk menentukan setelan relay proteksi?", "id": "Studi koordinasi proteksi." },
  { "en": "Tujuan studi koordinasi proteksi?", "id": "Memastikan relay bekerja secara selektif." },
  { "en": "Apa itu selektivitas proteksi?", "id": "Hanya relay terdekat gangguan yang bekerja." },
  { "en": "Apa itu stabilitas tegangan?", "id": "Kemampuan sistem menjaga tegangan stabil." },
  { "en": "Keruntuhan tegangan (voltage collapse) disebabkan oleh?", "id": "Kekurangan suplai daya reaktif." },
  { "en": "Sistem eksitasi generator tanpa sikat arang?", "id": "Sistem eksitasi nirsikat (brushless)." },
  { "en": "Sistem eksitasi yang menggunakan penyearah terkontrol?", "id": "Sistem eksitasi statis." },
  { "en": "Fungsi tambahan pada AVR (Automatic Voltage Regulator) untuk meredam osilasi?", "id": "PSS (Power System Stabilizer)." },
  { "en": "Tujuan utama PSS (Power System Stabilizer)?", "id": "Meningkatkan stabilitas dinamik sistem." },
  { "en": "Jenis media pemadam busur api pada PMT (Pemutus Tenaga)?", "id": "Minyak, udara, vakum, gas SF6." },
  { "en": "PMT (Pemutus Tenaga) yang menggunakan ruang hampa?", "id": "Pemutus tenaga vakum (VCB)." },
  { "en": "Arus puncak maksimum yang dapat ditutup oleh PMT (Pemutus Tenaga)?", "id": "Kapasitas penutupan (making capacity)." },
  { "en": "Mekanisme yang mencegah PMT (Pemutus Tenaga) menutup saat ada perintah trip?", "id": "Mekanisme trip-free." },
  { "en": "Ukuran distorsi arus terhadap arus beban maksimum?", "id": "TDD (Total Demand Distortion)." },
  { "en": "Titik di mana konsumen terhubung ke jaringan PLN?", "id": "PCC (Point of Common Coupling)." },
  { "en": "Rating trafo yang dirancang untuk beban harmonisa tinggi?", "id": "K-Factor." },
  { "en": "Nilai K-Factor tinggi menandakan trafo?", "id": "Tahan terhadap pemanasan akibat harmonisa." },
  { "en": "Komponen utama dalam modul GIS (Gas Insulated Substation)?", "id": "PMT, PMS, CT, Busbar." },
  { "en": "Tekanan gas SF6 (Sulfur Hexafluoride) pada GIS dijaga oleh?", "id": "Density monitor (monitor kerapatan)." },
  { "en": "Peralatan untuk mendeteksi kebocoran gas SF6 (Sulfur Hexafluoride)?", "id": "SF6 leak detector." },
  { "en": "Proses pengisian kembali gas SF6 (Sulfur Hexafluoride) disebut?", "id": "Gas filling." },
  { "en": "Metode analisis sistem tenaga dengan nilai dasar?", "id": "Sistem per unit (p.u)." },
  { "en": "Keuntungan menggunakan sistem per unit?", "id": "Menyederhanakan perhitungan dan analisis." },
  { "en": "Diagram rangkaian yang merepresentasikan impedansi sistem?", "id": "Diagram impedansi." },
  { "en": "Ukuran kemampuan sistem menyuplai arus gangguan?", "id": "Tingkat hubung singkat (fault level)." },
  { "en": "Pemanas air umpan sebelum masuk boiler di PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Economizer." },
  { "en": "Pemanas uap jenuh menjadi uap kering di PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Superheater." },
  { "en": "Pemanasan kembali uap setelah melewati turbin tekanan tinggi?", "id": "Reheater." },
  { "en": "Siklus termodinamika dasar untuk PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Siklus Rankine." },
  { "en": "Siklus termodinamika dasar untuk PLTG (Pembangkit Listrik Tenaga Gas)?", "id": "Siklus Brayton." },
  { "en": "Komponen utama VFD (Variable Frequency Drive)?", "id": "Rectifier, DC link, inverter." },
  { "en": "Bagian VFD (Variable Frequency Drive) yang mengubah AC ke DC?", "id": "Rectifier (penyearah)." },
  { "en": "Bagian VFD (Variable Frequency Drive) yang menyimpan energi DC?", "id": "DC link kapasitor." },
  { "en": "Bagian VFD (Variable Frequency Drive) yang mengubah DC ke AC variabel?", "id": "Inverter." },
  { "en": "Cincin pada rotor motor untuk koneksi eksternal?", "id": "Cincin geser (slip ring)." },
  { "en": "Motor induksi yang memiliki slip ring?", "id": "Motor induksi rotor lilit." },
  { "en": "Tujuan rotor lilit pada motor induksi?", "id": "Mengatur tahanan rotor untuk starting." },
  { "en": "Pengelolaan aset fisik perusahaan secara sistematis?", "id": "Manajemen aset (asset management)." },
  { "en": "Estimasi sisa umur pakai suatu peralatan?", "id": "RLA (Remaining Life Assessment)." },
  { "en": "Analisis untuk mencari akar penyebab suatu masalah?", "id": "RCA (Root Cause Analysis)." },
  { "en": "Infrastruktur pengukuran energi cerdas dan dua arah?", "id": "AMI (Advanced Metering Infrastructure)." },
  { "en": "Perbedaan AMR dan AMI (Advanced Metering Infrastructure)?", "id": "AMI mendukung komunikasi dua arah." },
  { "en": "Perangkat yang mengukur fasor tegangan dan arus?", "id": "PMU (Phasor Measurement Unit)." },
  { "en": "Data dari PMU (Phasor Measurement Unit) disinkronkan dengan?", "id": "GPS (Global Positioning System)." },
  { "en": "Sistem pemantauan area luas menggunakan data PMU?", "id": "WAMS (Wide Area Monitoring Systems)." },
  { "en": "Tipe isolator untuk menopang konduktor secara vertikal?", "id": "Isolator jenis post (post insulator)." },
  { "en": "Tipe isolator untuk menahan tarikan konduktor?", "id": "Isolator jenis strain (strain insulator)." },
  { "en": "Isolator yang digantung pada tower transmisi?", "id": "Isolator gantung (suspension insulator)." },
  { "en": "Bahan isolator modern selain keramik dan kaca?", "id": "Polimer atau komposit." },
  { "en": "Keuntungan isolator polimer?", "id": "Lebih ringan, anti-pecah, hidrofobik." },
  { "en": "Sifat hidrofobik pada isolator artinya?", "id": "Sifat menolak air." },
  { "en": "Apa itu tegangan nominal sistem?", "id": "Tegangan referensi untuk desain sistem." },
  { "en": "Apa itu tegangan operasi sistem?", "id": "Tegangan aktual yang terukur." },
  { "en": "Batas tegangan operasi maksimum yang diizinkan?", "id": "Tegangan operasi maksimum." },
  { "en": "Apa itu cadangan dingin (cold reserve)?", "id": "Unit pembangkit yang padam tapi siap." },
  { "en": "Perbedaan cadangan dingin dan cadangan berputar?", "id": "Cadangan berputar sudah sinkron." },
  { "en": "Biaya investasi awal pembangunan pembangkit?", "id": "Capital Expenditure (CAPEX)." },
  { "en": "Biaya operasional dan pemeliharaan pembangkit?", "id": "Operational Expenditure (OPEX)." },
  { "en": "Apa itu kWh ekspor-impor?", "id": "Sistem untuk pelanggan PLTS atap." },
  { "en": "Kelebihan energi PLTS atap yang diekspor ke PLN disebut?", "id": "Energi ekspor." },
  { "en": "Penggunaan listrik dari PLN saat PLTS tidak cukup?", "id": "Energi impor." },
  { "en": "Perangkat lunak untuk simulasi sistem tenaga?", "id": "ETAP, PSS/E, DigSilent." },
  { "en": "Apa itu kontingensi dalam sistem tenaga?", "id": "Kehilangan atau kegagalan satu komponen." },
  { "en": "Analisis untuk menguji keamanan sistem saat kontingensi?", "id": "Analisis kontingensi (N-1)." },
  { "en": "Prinsip 'N-1' dalam keamanan sistem?", "id": "Sistem harus aman jika satu komponen." },
  { "en": "Apa itu FACTS tipe seri?", "id": "Contoh: TCSC (Thyristor Controlled Series Capacitor)." },
  { "en": "Apa itu FACTS tipe shunt?", "id": "Contoh: STATCOM (Static Synchronous Compensator)." },
  { "en": "Fungsi utama TCSC?", "id": "Mengatur reaktansi induktif saluran." },
  { "en": "Fungsi utama STATCOM?", "id": "Menyuntikkan atau menyerap daya reaktif." },
  { "en": "Perangkat gabungan seri-shunt FACTS?", "id": "UPFC (Unified Power Flow Controller)." },
  { "en": "Peralatan pengaman dari tekanan tinggi di dalam GIS?", "id": "Rupture disc." },
  { "en": "Apa itu motor servo hidrolik?", "id": "Motor presisi yang digerakkan oli." },
  { "en": "Aplikasi motor servo hidrolik?", "id": "Mesin industri berat, sistem kontrol." },
  { "en": "Apa itu motor servo pneumatik?", "id": "Motor presisi yang digerakkan udara." },
  { "en": "Aplikasi motor servo pneumatik?", "id": "Robotika, otomasi ringan." },
  { "en": "Apa itu sistem otomasi industri?", "id": "Penggunaan sistem kontrol untuk industri." },
  { "en": "Komponen utama sistem otomasi industri?", "id": "Sensor, aktuator, kontroler." },
  { "en": "Contoh kontroler dalam otomasi industri?", "id": "PLC, DCS, mikrokontroler." },
  { "en": "Jaringan komunikasi antar perangkat industri?", "id": "Fieldbus (contoh: Profibus, Modbus)." },
  { "en": "Panel antarmuka antara manusia dan mesin?", "id": "HMI (Human-Machine Interface)." },
  { "en": "Fungsi HMI (Human-Machine Interface)?", "id": "Visualisasi dan kontrol proses." },
  { "en": "Peralatan untuk memulai putaran turbin gas?", "id": "Starting motor." },
  { "en": "Apa itu 'firing' pada turbin gas?", "id": "Proses penyalaan awal di ruang bakar." },
  { "en": "Suhu gas buang pada turbin gas?", "id": "Sangat tinggi, bisa dimanfaatkan lagi." },
  { "en": "Pemanfaatan panas gas buang turbin gas?", "id": "Untuk memanaskan boiler di PLTGU." },
  { "en": "Komponen PLTG yang menyedot dan menekan udara?", "id": "Kompresor." },
  { "en": "Komponen PLTG tempat pencampuran udara dan bahan bakar?", "id": "Ruang bakar (combustion chamber)." },
  { "en": "Apa itu efisiensi siklus gabungan (combined cycle)?", "id": "Efisiensi total dari PLTGU." },
  { "en": "Keunggulan efisiensi siklus gabungan?", "id": "Sangat tinggi, bisa mencapai 60%." },
  { "en": "Apa itu transformator tipe kering (dry type)?", "id": "Trafo tanpa pendingin oli cair." },
  { "en": "Media pendingin trafo tipe kering?", "id": "Udara atau resin cor." },
  { "en": "Keuntungan trafo tipe kering?", "id": "Lebih aman dari kebakaran, ramah lingkungan." },
  { "en": "Tempat pemasangan trafo tipe kering?", "id": "Dalam gedung, area sensitif kebakaran." },
  { "en": "Apa itu tap changer saat berbeban?", "id": "OLTC (On-Load Tap Changer)." },
  { "en": "Fungsi OLTC (On-Load Tap Changer)?", "id": "Mengubah rasio trafo tanpa padam." },
  { "en": "Apa itu tap changer tanpa beban?", "id": "NLTC (No-Load Tap Changer)." },
  { "en": "Syarat mengubah tap pada NLTC?", "id": "Trafo harus dipadamkan terlebih dahulu." },
  { "en": "Pengujian untuk mengukur kualitas isolasi (rugi dielektrik)?", "id": "Pengujian tan delta." },
  { "en": "Pengujian ketahanan isolasi dengan tegangan tinggi?", "id": "Hi-Pot (High Potential) test." },
  { "en": "Fungsi utama surge arrester?", "id": "Melindungi peralatan dari surja tegangan." },
  { "en": "Alat untuk menguji kinerja surge arrester?", "id": "Surge arrester tester." },
  { "en": "Perangkat FACTS (Flexible AC Transmission Systems) paling komprehensif?", "id": "UPFC (Unified Power Flow Controller)." },
  { "en": "Fungsi utama UPFC (Unified Power Flow Controller)?", "id": "Mengontrol aliran daya aktif dan reaktif." },
  { "en": "Perangkat FACTS (Flexible AC Transmission Systems) tipe shunt untuk kompensasi reaktif?", "id": "STATCOM (Static Synchronous Compensator)." },
  { "en": "Perangkat FACTS (Flexible AC Transmission Systems) tipe seri untuk mengatur reaktansi?", "id": "TCSC (Thyristor Controlled Series Capacitor)." },
  { "en": "Evaluasi penggunaan energi pada suatu fasilitas?", "id": "Audit energi (energy audit)." },
  { "en": "Upaya menggunakan energi lebih sedikit untuk hasil sama?", "id": "Efisiensi energi." },
  { "en": "Grafik yang menunjukkan pola pemakaian beban listrik?", "id": "Kurva beban (load curve)." },
  { "en": "Program yang melibatkan pelanggan untuk mengubah pola beban?", "id": "Respons permintaan (demand response)." },
  { "en": "Pembangkit listrik skala kecil yang terhubung ke jaringan distribusi?", "id": "DG (Distributed Generation)." },
  { "en": "Contoh DG (Distributed Generation)?", "id": "PLTS atap, genset, mikrohidro." },
  { "en": "Jaringan listrik lokal yang bisa beroperasi mandiri?", "id": "Microgrid atau jaringan mikro." },
  { "en": "Mode operasi microgrid saat terhubung ke jaringan utama?", "id": "Mode terhubung grid (grid-connected)." },
  { "en": "Mode operasi microgrid saat terlepas dari jaringan utama?", "id": "Mode pulau (islanded mode)." },
  { "en": "Proses penghapusan monopoli dalam industri listrik?", "id": "Deregulasi." },
  { "en": "Pembangkit listrik yang dimiliki oleh pihak swasta?", "id": "IPP (Independent Power Producer)." },
  { "en": "Perjanjian jual beli listrik antara IPP dan PLN?", "id": "PPA (Power Purchase Agreement)." },
  { "en": "Relay proteksi digital modern disebut?", "id": "IED (Intelligent Electronic Device)." },
  { "en": "Keuntungan IED (Intelligent Electronic Device) dibanding relay elektromekanik?", "id": "Multifungsi, akurat, dan komunikatif." },
  { "en": "Sistem otomasi gardu induk modern?", "id": "SAS (Substation Automation System)." },
  { "en": "Protokol komunikasi cepat antar IED di gardu induk?", "id": "Pesan GOOSE (Generic Object Oriented Substation Event)." },
  { "en": "Standar komunikasi untuk otomasi gardu induk?", "id": "IEC 61850." },
  { "en": "Tahanan untuk membuang energi pengereman VFD (Variable Frequency Drive)?", "id": "Tahanan pengereman (braking resistor)." },
  { "en": "Sensor umpan balik posisi pada motor servo?", "id": "Encoder atau resolver." },
  { "en": "Perbedaan encoder dan resolver?", "id": "Encoder digital, resolver analog." },
  { "en": "Rasio waktu kerja motor terhadap total siklus?", "id": "Siklus kerja (duty cycle)." },
  { "en": "Gangguan gelombang elektromagnetik pada peralatan elektronik?", "id": "EMI (Electromagnetic Interference)." },
  { "en": "Kemampuan alat berfungsi normal di lingkungan elektromagnetik?", "id": "EMC (Electromagnetic Compatibility)." },
  { "en": "Lapisan pelindung kabel dari interferensi elektromagnetik?", "id": "Pelindung (shielding)." },
  { "en": "Analisis untuk menentukan tingkat bahaya busur api?", "id": "Analisis bahaya arc flash." },
  { "en": "Energi termal yang dilepaskan saat arc flash?", "id": "Energi insiden (incident energy)." },
  { "en": "Kategori APD (Alat Pelindung Diri) untuk arc flash ditentukan oleh?", "id": "Tingkat energi insiden." },
  { "en": "Prosedur penguncian sumber energi sebelum perbaikan?", "id": "Prosedur LOTO (Lockout-Tagout)." },
  { "en": "Kunci gembok pada prosedur LOTO (Lockout-Tagout) berfungsi untuk?", "id": "Mengunci isolasi energi." },
  { "en": "Label pada prosedur LOTO (Lockout-Tagout) berfungsi untuk?", "id": "Memberi informasi pekerjaan sedang berlangsung." },
  { "en": "Induktor besar untuk kompensasi kapasitansi saluran?", "id": "Reaktor shunt." },
  { "en": "Fungsi reaktor shunt?", "id": "Menyerap daya reaktif berlebih." },
  { "en": "Induktor yang dipasang seri dengan saluran?", "id": "Reaktor seri." },
  { "en": "Fungsi reaktor seri?", "id": "Membatasi arus hubung singkat." },
  { "en": "Resistor untuk meredam osilasi pada sistem?", "id": "Resistor peredam (damping resistor)." },
  { "en": "Sistem koneksi beberapa sumber DG (Distributed Generation) ke grid?", "id": "Power islanding." },
  { "en": "Apa itu sistem SCADA berbasis web?", "id": "Web-based SCADA." },
  { "en": "Keuntungan sistem SCADA berbasis web?", "id": "Dapat diakses dari mana saja." },
  { "en": "Apa itu sistem manajemen energi?", "id": "EMS (Energy Management System)." },
  { "en": "Tujuan EMS (Energy Management System)?", "id": "Memantau dan mengoptimalkan penggunaan energi." },
  { "en": "Apa itu sistem manajemen distribusi?", "id": "DMS (Distribution Management System)." },
  { "en": "Fungsi DMS (Distribution Management System)?", "id": "Mengelola dan mengontrol jaringan distribusi." },
  { "en": "Apa itu sistem manajemen pemadaman?", "id": "OMS (Outage Management System)." },
  { "en": "Fungsi OMS (Outage Management System)?", "id": "Melacak dan mengelola pemadaman listrik." },
  { "en": "Apa itu sistem informasi geografis?", "id": "GIS (Geographic Information System)." },
  { "en": "Peran GIS (Geographic Information System) di kelistrikan?", "id": "Memetakan dan mengelola aset jaringan." },
  { "en": "Apa itu sistem catu daya redundan?", "id": "Sistem dengan dua sumber independen." },
  { "en": "Tujuan sistem catu daya redundan?", "id": "Meningkatkan keandalan catu daya." },
  { "en": "Apa itu sistem UPS (Uninterruptible Power Supply) online?", "id": "Beban selalu disuplai lewat inverter." },
  { "en": "Keuntungan sistem UPS (Uninterruptible Power Supply) online?", "id": "Proteksi terbaik, tanpa jeda transfer." },
  { "en": "Apa itu sistem UPS (Uninterruptible Power Supply) offline?", "id": "Beban disuplai dari PLN normalnya." },
  { "en": "Kelemahan sistem UPS (Uninterruptible Power Supply) offline?", "id": "Ada jeda waktu transfer." },
  { "en": "Apa itu sistem UPS (Uninterruptible Power Supply) line-interactive?", "id": "Gabungan online dan offline." },
  { "en": "Baterai yang umum dipakai di UPS (Uninterruptible Power Supply)?", "id": "Baterai VRLA (Valve-Regulated Lead-Acid)." },
  { "en": "Apa itu faktor puncak (crest factor)?", "id": "Rasio nilai puncak dan RMS." },
  { "en": "Penyebab crest factor tinggi?", "id": "Beban non-linear." },
  { "en": "Apa itu arus netral triplen?", "id": "Arus harmonisa kelipatan tiga." },
  { "en": "Dampak arus netral triplen?", "id": "Menyebabkan panas berlebih di kawat netral." },
  { "en": "Sumber utama arus netral triplen?", "id": "Komputer, lampu neon." },
  { "en": "Desain kawat netral untuk beban triplen?", "id": "Ukuran lebih besar dari fasa." },
  { "en": "Apa itu grounding terisolasi (isolated ground)?", "id": "Sistem grounding terpisah untuk elektronik." },
  { "en": "Tujuan grounding terisolasi?", "id": "Mengurangi noise listrik." },
  { "en": "Apa itu titik referensi sinyal?", "id": "Signal reference grid." },
  { "en": "Apa itu ikatan ekuipotensial (equipotential bonding)?", "id": "Menghubungkan semua bagian logam." },
  { "en": "Tujuan ikatan ekuipotensial?", "id": "Menghindari beda tegangan berbahaya." },
  { "en": "Apa itu transformator isolasi?", "id": "Trafo dengan rasio 1:1." },
  { "en": "Fungsi transformator isolasi?", "id": "Mengisolasi secara elektrik dan mengurangi noise." },
  { "en": "Pelindung elektrostatik pada trafo disebut?", "id": "Faraday shield." },
  { "en": "Fungsi Faraday shield pada trafo?", "id": "Memblokir noise mode kapasitif." },
  { "en": "Apa itu noise mode bersama (common mode)?", "id": "Noise antara fasa/netral dan ground." },
  { "en": "Apa itu noise mode diferensial (differential mode)?", "id": "Noise antara fasa dan netral." },
  { "en": "Filter untuk menekan EMI (Electromagnetic Interference)?", "id": "Filter EMI/RFI." },
  { "en": "Apa itu rating SCCR peralatan?", "id": "Short Circuit Current Rating." },
  { "en": "Pentingnya rating SCCR (Short Circuit Current Rating)?", "id": "Menjamin alat aman saat hubung singkat." },
  { "en": "Studi untuk menentukan rating SCCR (Short Circuit Current Rating) panel?", "id": "Studi hubung singkat." },
  { "en": "Apa itu sistem proteksi petir terkoordinasi?", "id": "SPM (Surge Protection Measures) terkoordinasi." },
  { "en": "Komponen utama SPM (Surge Protection Measures)?", "id": "Proteksi eksternal dan internal." },
  { "en": "Perangkat pelindung surja di panel listrik?", "id": "SPD (Surge Protective Device)." },
  { "en": "Zona proteksi petir menurut standar?", "id": "LPZ (Lightning Protection Zone)." },
  { "en": "Apa itu kabel twisted pair?", "id": "Kabel dengan pasangan terpilin." },
  { "en": "Tujuan memilin kabel (twisted pair)?", "id": "Mengurangi interferensi elektromagnetik." },
  { "en": "Pelindung pada kabel UTP vs STP?", "id": "STP (Shielded) punya pelindung, UTP tidak." },
  { "en": "Kontroler logika yang dapat diprogram?", "id": "PLC (Programmable Logic Controller)." },
  { "en": "Bahasa pemrograman umum untuk PLC (Programmable Logic Controller)?", "id": "Ladder diagram." },
  { "en": "Aplikasi PLC (Programmable Logic Controller) di sistem tenaga?", "id": "Kontrol genset, ATS, panel." },
  { "en": "Pemeriksaan dan pengujian rutin peralatan?", "id": "Komisioning (commissioning)." },
  { "en": "Dokumen akhir setelah proyek selesai?", "id": "As-built drawing." },
  { "en": "Pentingnya as-built drawing?", "id": "Dokumentasi akurat untuk pemeliharaan." },
  { "en": "Waktu maksimum pemutusan gangguan agar sistem stabil?", "id": "CCT (Critical Clearing Time)." },
  { "en": "Metode grafis untuk analisis stabilitas transien?", "id": "Kriteria sama luas (Equal Area Criterion)." },
  { "en": "Kemampuan mesin berputar untuk menyimpan energi kinetik?", "id": "Inersia sistem." },
  { "en": "Ukuran seberapa cepat osilasi diredam?", "id": "Rasio redaman (damping ratio)." },
  { "en": "Filter harmonisa dari komponen pasif RLC?", "id": "Filter pasif." },
  { "en": "Filter harmonisa yang menggunakan elektronika daya?", "id": "Filter aktif." },
  { "en": "Perbedaan THD (Total Harmonic Distortion) dan TDD (Total Demand Distortion)?", "id": "THD terhadap fundamental, TDD terhadap beban." },
  { "en": "Komponen frekuensi yang bukan kelipatan fundamental?", "id": "Interharmonisa." },
  { "en": "Hukum yang menjelaskan tegangan breakdown pada gas?", "id": "Hukum Paschen." },
  { "en": "Proses kegagalan isolasi pada material?", "id": "Breakdown atau dadal." },
  { "en": "Pelepasan muatan listrik parsial di sekitar konduktor tegangan tinggi?", "id": "Lucutan korona (corona discharge)." },
  { "en": "Dampak dari lucutan korona?", "id": "Rugi daya, noise radio, korosi." },
  { "en": "Sistem otomasi untuk mengatasi gangguan di jaringan distribusi?", "id": "FLISR (Fault Location, Isolation, and Service Restoration)." },
  { "en": "Tujuan sistem FLISR?", "id": "Mempercepat pemulihan dan mengurangi area padam." },
  { "en": "Optimalisasi tegangan dan daya reaktif di jaringan distribusi?", "id": "VVO (Volt-VAR Optimization)." },
  { "en": "Otomatisasi jaringan distribusi secara umum?", "id": "DA (Distribution Automation)." },
  { "en": "Pembangkit listrik yang mengubah energi kimia langsung jadi listrik?", "id": "Sel bahan bakar (fuel cell)." },
  { "en": "Komponen utama PLTP (Pembangkit Listrik Tenaga Panas Bumi)?", "id": "Sumur produksi, separator, turbin, kondensor." },
  { "en": "PLTA (Pembangkit Listrik Tenaga Air) yang memompa air kembali ke atas?", "id": "PLTA tipe pompa-simpan (pumped storage)." },
  { "en": "Fungsi PLTA (Pembangkit Listrik Tenaga Air) tipe pompa-simpan?", "id": "Menyimpan energi saat beban rendah." },
  { "en": "Skema proteksi busbar berdasarkan hukum Kirchhoff?", "id": "Proteksi diferensial." },
  { "en": "Karakteristik relay diferensial trafo untuk mencegah trip salah?", "id": "Karakteristik bias (persentase)." },
  { "en": "Fungsi bias pada relay diferensial trafo?", "id": "Mencegah trip akibat tap changer." },
  { "en": "Proteksi yang mendeteksi kondisi generator ayunan daya?", "id": "Proteksi lepas sinkron (out-of-step)." },
  { "en": "Strategi pemeliharaan berdasarkan tingkat risiko kegagalan?", "id": "RBM (Risk-Based Maintenance)." },
  { "en": "Indikator untuk mengukur kinerja suatu sistem?", "id": "KPI (Key Performance Indicator)." },
  { "en": "Contoh KPI (Key Performance Indicator) keandalan?", "id": "SAIDI dan SAIFI." },
  { "en": "Manajemen seluruh siklus hidup aset?", "id": "Asset lifecycle management." },
  { "en": "Layanan tambahan selain penyaluran energi?", "id": "Layanan pendukung (ancillary services)." },
  { "en": "Contoh layanan pendukung?", "id": "Regulasi frekuensi, cadangan daya reaktif." },
  { "en": "Kondisi kelebihan pembebanan pada saluran transmisi?", "id": "Kemacetan (congestion)." },
  { "en": "Pengujian untuk mendeteksi deformasi belitan trafo?", "id": "SFRA (Sweep Frequency Response Analysis)." },
  { "en": "Prinsip kerja SFRA (Sweep Frequency Response Analysis)?", "id": "Membandingkan respons frekuensi belitan." },
  { "en": "Pengujian untuk mendeteksi lucutan parsial (partial discharge)?", "id": "Pengujian PD (Partial Discharge)." },
  { "en": "Material yang hambatannya nol pada suhu sangat rendah?", "id": "Superkonduktor." },
  { "en": "Saluran transmisi berisolasi gas dalam tabung logam?", "id": "GIL (Gas Insulated Lines)." },
  { "en": "Komponen semikonduktor daya pada perangkat FACTS?", "id": "Thyristor atau IGBT." },
  { "en": "Kepanjangan IGBT?", "id": "Insulated-Gate Bipolar Transistor." },
  { "en": "Klasifikasi tegangan HV (High Voltage)?", "id": "35 kV hingga 230 kV." },
  { "en": "Klasifikasi tegangan EHV (Extra High Voltage)?", "id": "345 kV hingga 765 kV." },
  { "en": "Klasifikasi tegangan UHV (Ultra High Voltage)?", "id": "Di atas 800 kV." },
  { "en": "Kawat tanah transmisi yang memiliki serat optik?", "id": "OPGW (Optical Ground Wire)." },
  { "en": "Fungsi ganda OPGW (Optical Ground Wire)?", "id": "Proteksi petir dan komunikasi data." },
  { "en": "Bantalan poros pada mesin berputar?", "id": "Bantalan (bearing)." },
  { "en": "Jenis bantalan yang umum digunakan?", "id": "Bantalan bola (ball bearing), journal." },
  { "en": "Getaran berlebih pada mesin menandakan?", "id": "Ketidakseimbangan, kerusakan bantalan, misalignment." },
  { "en": "Proses menyejajarkan dua poros mesin?", "id": "Penjajaran (alignment)." },
  { "en": "Apa itu soft starter?", "id": "Alat untuk start motor secara perlahan." },
  { "en": "Keuntungan menggunakan soft starter?", "id": "Mengurangi lonjakan arus dan stres mekanik." },
  { "en": "Sistem distribusi dengan dua penyulang aktif?", "id": "Jaringan spindel." },
  { "en": "Saklar pemindah beban di jaringan distribusi?", "id": "Change Over Switch (COS)." },
  { "en": "Apa itu sistem SCADA terpusat?", "id": "Satu pusat kontrol untuk semua." },
  { "en": "Apa itu sistem SCADA terdistribusi?", "id": "Beberapa pusat kontrol dengan otonomi." },
  { "en": "Peralatan di lapangan yang terhubung ke RTU (Remote Terminal Unit)?", "id": "Sensor, transduser, IED." },
  { "en": "Data yang dikirim RTU (Remote Terminal Unit) ke master?", "id": "Status PMT, pengukuran arus, tegangan." },
  { "en": "Perintah yang dikirim master ke RTU (Remote Terminal Unit)?", "id": "Buka/tutup PMT, ubah setelan." },
  { "en": "Sistem manajemen data pengukuran energi?", "id": "MDMS (Meter Data Management System)." },
  { "en": "Fungsi MDMS (Meter Data Management System)?", "id": "Mengumpulkan, memvalidasi, menyimpan data meter." },
  { "en": "Apa itu cyber security di sistem tenaga?", "id": "Perlindungan sistem dari serangan siber." },
  { "en": "Ancaman siber pada sistem SCADA?", "id": "Akses ilegal, malware, DoS attack." },
  { "en": "Dinding api untuk melindungi jaringan SCADA?", "id": "Firewall industri." },
  { "en": "Apa itu rating MVA trafo?", "id": "Kapasitas daya semu transformator." },
  { "en": "Mengapa rating trafo dalam MVA bukan MW?", "id": "Rugi-rugi bergantung pada arus dan tegangan." },
  { "en": "Beban trafo idealnya berapa persen?", "id": "Sekitar 80% dari kapasitas." },
  { "en": "Apa itu overload pada trafo?", "id": "Beban melebihi kapasitas nominalnya." },
  { "en": "Dampak overload terus-menerus pada trafo?", "id": "Memperpendek umur isolasi (penuaan)." },
  { "en": "Pendingin tambahan pada trafo yang otomatis bekerja?", "id": "Kipas atau pompa oli otomatis." },
  { "en": "Apa itu isolasi kelas A?", "id": "Batas suhu 105 derajat Celsius." },
  { "en": "Apa itu isolasi kelas B?", "id": "Batas suhu 130 derajat Celsius." },
  { "en": "Apa itu isolasi kelas H?", "id": "Batas suhu 180 derajat Celsius." },
  { "en": "Semakin tinggi kelas isolasi, maka?", "id": "Semakin tahan terhadap suhu tinggi." },
  { "en": "Apa itu Partial Discharge Inception Voltage (PDIV)?", "id": "Tegangan awal timbulnya lucutan parsial." },
  { "en": "Apa itu Partial Discharge Extinction Voltage (PDEV)?", "id": "Tegangan saat lucutan parsial padam." },
  { "en": "Idealnya, PDEV (Partial Discharge Extinction Voltage) harus?", "id": "Lebih tinggi dari tegangan operasi." },
  { "en": "Apa itu 'space charge' pada isolasi DC?", "id": "Akumulasi muatan di dalam material isolasi." },
  { "en": "Tantangan isolasi pada sistem HVDC (High Voltage Direct Current)?", "id": "Fenomena space charge dan polarisasi." },
  { "en": "Jenis PMT (Pemutus Tenaga) untuk sistem HVDC?", "id": "PMT DC hibrida atau mekanik." },
  { "en": "Tantangan pemutusan arus DC?", "id": "Tidak ada titik nol alami." },
  { "en": "Apa itu 'crowbar circuit'?", "id": "Rangkaian proteksi dari tegangan lebih." },
  { "en": "Cara kerja 'crowbar circuit'?", "id": "Membuat hubung singkat untuk membuang energi." },
  { "en": "Apa itu 'snubber circuit'?", "id": "Rangkaian untuk meredam lonjakan tegangan." },
  { "en": "Komponen utama 'snubber circuit'?", "id": "Resistor dan kapasitor." },
  { "en": "Aplikasi 'snubber circuit'?", "id": "Melindungi semikonduktor daya (thyristor)." },
  { "en": "Apa itu 'flywheel energy storage'?", "id": "Penyimpanan energi dalam roda berputar." },
  { "en": "Keuntungan 'flywheel energy storage'?", "id": "Respon cepat, siklus hidup panjang." },
  { "en": "Apa itu 'supercapacitor'?", "id": "Kapasitor dengan kepadatan energi tinggi." },
  { "en": "Keuntungan 'supercapacitor'?", "id": "Pengisian sangat cepat, daya tinggi." },
  { "en": "Aplikasi 'supercapacitor' di sistem tenaga?", "id": "Memperbaiki kualitas daya, ride-through." },
  { "en": "Apa itu 'power quality analyzer'?", "id": "Alat ukur parameter kualitas daya." },
  { "en": "Parameter yang diukur 'power quality analyzer'?", "id": "Harmonisa, sag, swell, transien." },
  { "en": "Apa itu 'load shedding' adaptif?", "id": "Pelepasan beban berdasarkan kondisi real-time." },
  { "en": "Apa itu 'wide area stability control'?", "id": "Kontrol stabilitas menggunakan data WAMS." },
  { "en": "Perbedaan antara proteksi dan kontrol?", "id": "Proteksi bereaksi, kontrol mengatur." },
  { "en": "Konsep jaringan listrik yang bisa memperbaiki diri?", "id": "Self-healing grid." },
  { "en": "Teknologi kunci untuk 'self-healing grid'?", "id": "FLISR, DA, komunikasi canggih." },
  { "en": "Pembangkit yang menggunakan perbedaan suhu laut?", "id": "OTEC (Ocean Thermal Energy Conversion)." },
  { "en": "Prinsip kerja OTEC?", "id": "Menggunakan siklus Rankine suhu rendah." },
  { "en": "Jaringan perangkat pintar di sistem tenaga?", "id": "IoT (Internet of Things) untuk grid." },
  { "en": "Analisis data untuk prediksi kegagalan peralatan?", "id": "Analisis prediktif (predictive analytics)." },
  { "en": "Platform gabungan DMS, OMS, dan SCADA?", "id": "ADMS (Advanced Distribution Management Systems)." },
  { "en": "Teknologi buku besar digital untuk transaksi energi?", "id": "Blockchain." },
  { "en": "Sistem penyimpanan energi berbasis baterai?", "id": "BESS (Battery Energy Storage Systems)." },
  { "en": "Jenis baterai dominan pada BESS (Battery Energy Storage Systems) skala besar?", "id": "Baterai Lithium-ion (Li-ion)." },
  { "en": "Jenis baterai dengan elektrolit cair yang dipompa?", "id": "Baterai aliran (flow battery)." },
  { "en": "Penggunaan BESS (Battery Energy Storage Systems) untuk menstabilkan frekuensi?", "id": "Regulasi frekuensi (frequency regulation)." },
  { "en": "Penggunaan BESS (Battery Energy Storage Systems) untuk mengurangi beban puncak?", "id": "Pencukuran puncak (peak shaving)." },
  { "en": "Topologi HVDC (High Voltage Direct Current) dengan satu konduktor?", "id": "Konfigurasi monopolar." },
  { "en": "Topologi HVDC (High Voltage Direct Current) dengan dua konduktor?", "id": "Konfigurasi bipolar." },
  { "en": "Stasiun HVDC (High Voltage Direct Current) yang menghubungkan dua jaringan AC?", "id": "Konfigurasi back-to-back." },
  { "en": "Konverter HVDC (High Voltage Direct Current) berbasis thyristor?", "id": "LCC (Line Commutated Converter)." },
  { "en": "Konverter HVDC (High Voltage Direct Current) berbasis IGBT/GTO?", "id": "VSC (Voltage Source Converter)." },
  { "en": "Tegangan lebih akibat sambaran petir tidak langsung?", "id": "Surja terinduksi (induced overvoltage)." },
  { "en": "Tegangan lebih akibat operasi buka-tutup PMT?", "id": "Surja hubung-buka (switching overvoltage)." },
  { "en": "Tegangan yang muncul melintasi kontak PMT setelah memutus arus?", "id": "TRV (Transient Recovery Voltage)." },
  { "en": "Tantangan bagi PMT jika TRV (Transient Recovery Voltage) terlalu tinggi?", "id": "Risiko kegagalan pemutusan (restrike)." },
  { "en": "Analisis sisa umur kertas isolasi trafo?", "id": "Analisis Furan." },
  { "en": "Analisis getaran pada mesin berputar?", "id": "Analisis vibrasi." },
  { "en": "Tujuan analisis vibrasi?", "id": "Mendeteksi masalah mekanis sejak dini." },
  { "en": "Pengujian akustik untuk mendeteksi lucutan parsial?", "id": "Acoustic partial discharge testing." },
  { "en": "Pasar untuk kapasitas pembangkit yang tersedia?", "id": "Pasar kapasitas (capacity market)." },
  { "en": "Pasar jual beli energi listrik aktual?", "id": "Pasar energi (energy market)." },
  { "en": "Pasar untuk layanan pendukung sistem?", "id": "Pasar layanan pendukung (ancillary services)." },
  { "en": "Strategi untuk mengelola risiko fluktuasi harga listrik?", "id": "Hedging." },
  { "en": "Lokasi gangguan berdasarkan waktu tempuh gelombang?", "id": "Lokasi gangguan gelombang berjalan (traveling wave)." },
  { "en": "Skema proteksi yang menggunakan data PMU?", "id": "Proteksi berbasis synchrophasor." },
  { "en": "Setelan relay proteksi yang bisa berubah otomatis?", "id": "Relaying adaptif." },
  { "en": "Bahan alternatif untuk tower transmisi?", "id": "Material komposit." },
  { "en": "Konduktor yang bisa beroperasi pada suhu tinggi?", "id": "Konduktor HTLS (High-Temperature Low-Sag)." },
  { "en": "Keuntungan konduktor HTLS (High-Temperature Low-Sag)?", "id": "Meningkatkan kapasitas hantar arus." },
  { "en": "Sistem tenaga sebagai gabungan komponen fisik dan siber?", "id": "Sistem siber-fisik (cyber-physical system)." },
  { "en": "Kemampuan sistem untuk bertahan dan pulih dari gangguan?", "id": "Ketahanan (resilience)." },
  { "en": "Kegagalan beruntun yang meluas di sistem?", "id": "Kegagalan kaskade (cascading failure)." },
  { "en": "Organisasi standardisasi profesional teknik elektro global?", "id": "IEEE (Institute of Electrical and Electronics Engineers)." },
  { "en": "Standar IEEE (Institute of Electrical and Electronics Engineers) untuk kelistrikan industri?", "id": "IEEE Color Books." },
  { "en": "Organisasi sistem tenaga listrik besar global?", "id": "CIGRE (International Council on Large Electric Systems)." },
  { "en": "Badan keandalan kelistrikan Amerika Utara?", "id": "NERC (North American Electric Reliability Corporation)." },
  { "en": "Apa itu 'grid inertia'?", "id": "Inersia total dari semua mesin sinkron." },
  { "en": "Dampak penurunan 'grid inertia'?", "id": "Frekuensi lebih cepat berubah (labil)." },
  { "en": "Sumber 'grid inertia' sintetis atau virtual?", "id": "Inverter BESS atau pembangkit EBT." },
  { "en": "Apa itu 'grid forming inverter'?", "id": "Inverter yang bisa menciptakan tegangan sendiri." },
  { "en": "Apa itu 'grid following inverter'?", "id": "Inverter yang mengikuti tegangan grid." },
  { "en": "Satuan daya semu (apparent power)?", "id": "Volt-Ampere (VA) atau MVA." },
  { "en": "Satuan daya aktif (active power)?", "id": "Watt (W) atau MW." },
  { "en": "Satuan daya reaktif (reactive power)?", "id": "VAR atau MVAR." },
  { "en": "Konversi daya dari AC ke AC?", "id": "Cycloconverter atau matrix converter." },
  { "en": "Sistem penyimpanan energi menggunakan udara terkompresi?", "id": "CAES (Compressed Air Energy Storage)." },
  { "en": "Prinsip kerja CAES?", "id": "Menyimpan udara bertekanan di rongga bawah tanah." },
  { "en": "Apa itu 'power-to-gas' (P2G)?", "id": "Mengubah listrik menjadi gas hidrogen." },
  { "en": "Proses dalam P2G (power-to-gas)?", "id": "Elektrolisis air." },
  { "en": "Hidrogen dari P2G (power-to-gas) bisa digunakan untuk?", "id": "Bahan bakar atau dibangkitkan lagi." },
  { "en": "Jaringan DC (Direct Current) tegangan rendah di gedung?", "id": "Microgrid DC." },
  { "en": "Keuntungan microgrid DC?", "id": "Efisiensi lebih tinggi untuk beban DC." },
  { "en": "Apa itu 'solid state transformer' (SST)?", "id": "Transformator berbasis elektronika daya." },
  { "en": "Keuntungan 'solid state transformer' (SST)?", "id": "Kontrol fleksibel, ukuran kompak." },
  { "en": "Tingkat ketersediaan (availability) sistem diukur dalam?", "id": "Persentase waktu operasi." },
  { "en": "Rumus ketersediaan (availability)?", "id": "MTBF dibagi (MTBF + MTTR)." },
  { "en": "Apa itu 'fault ride through' (FRT)?", "id": "Kemampuan pembangkit EBT tetap terhubung saat gangguan." },
  { "en": "Tujuan kapabilitas FRT (fault ride through)?", "id": "Menjaga stabilitas sistem tenaga." },
  { "en": "Kondisi tegangan nol sesaat pada PCC?", "id": "Zero Voltage Ride Through (ZVRT)." },
  { "en": "Kondisi tegangan tinggi sesaat pada PCC?", "id": "High Voltage Ride Through (HVRT)." },
  { "en": "Apa itu 'power oscillation damper' (POD)?", "id": "Fitur pada kontroler untuk meredam osilasi." },
  { "en": "Di mana POD (power oscillation damper) dapat diimplementasikan?", "id": "AVR, FACTS, HVDC control." },
  { "en": "Apa itu 'substation hardening'?", "id": "Memperkuat gardu induk dari ancaman fisik." },
  { "en": "Ancaman yang ditangani 'substation hardening'?", "id": "Bencana alam, serangan, sabotase." },
  { "en": "Apa itu 'phasor data concentrator' (PDC)?", "id": "Alat pengumpul data dari banyak PMU." },
  { "en": "Fungsi PDC (phasor data concentrator)?", "id": "Menyelaraskan dan mengirim data ke pusat." },
  { "en": "Apa itu 'state estimation' di sistem tenaga?", "id": "Estimasi kondisi operasi sistem real-time." },
  { "en": "Data yang digunakan untuk 'state estimation'?", "id": "Data SCADA dan PMU." },
  { "en": "Apa itu 'dynamic line rating' (DLR)?", "id": "Rating saluran transmisi berdasarkan kondisi real-time." },
  { "en": "Faktor yang mempengaruhi DLR (dynamic line rating)?", "id": "Suhu, kecepatan angin, radiasi surya." },
  { "en": "Keuntungan DLR (dynamic line rating)?", "id": "Mengoptimalkan kapasitas transmisi yang ada." },
  { "en": "Apa itu 'grid code'?", "id": "Aturan teknis untuk terhubung ke grid." },
  { "en": "Siapa yang harus mematuhi 'grid code'?", "id": "Pembangkit dan konsumen besar." },
  { "en": "Isi dari 'grid code'?", "id": "Syarat proteksi, kualitas daya, kontrol." },
  { "en": "Apa itu 'isokeraunic level'?", "id": "Jumlah hari guntur per tahun." },
  { "en": "Kegunaan 'isokeraunic level'?", "id": "Memperkirakan probabilitas sambaran petir." },
  { "en": "Gelombang berjalan yang dipantulkan di ujung saluran?", "id": "Gelombang pantul (reflected wave)." },
  { "en": "Gelombang berjalan yang diteruskan melalui titik sambungan?", "id": "Gelombang bias (refracted wave)." },
  { "en": "Koefisien pantul dan bias bergantung pada?", "id": "Impedansi surja saluran." },
  { "en": "Impedansi surja (surge impedance) saluran udara?", "id": "Sekitar 400 ohm." },
  { "en": "Impedansi surja (surge impedance) kabel tanah?", "id": "Sekitar 40-50 ohm." },
  { "en": "Apa itu 'transposition' pada saluran transmisi?", "id": "Pertukaran posisi fasa secara periodik." },
  { "en": "Tujuan 'transposition'?", "id": "Menyeimbangkan impedansi ketiga fasa." },
  { "en": "Dampak impedansi tak seimbang?", "id": "Tegangan dan arus menjadi tak seimbang." },
  { "en": "Apa itu 'conductor galloping'?", "id": "Osilasi frekuensi rendah amplitudo besar." },
  { "en": "Penyebab 'conductor galloping'?", "id": "Angin pada konduktor berlapis es." },
  { "en": "Apa itu 'ring bus' configuration?", "id": "Konfigurasi busbar berbentuk cincin." },
  { "en": "Keuntungan 'ring bus'?", "id": "Ekonomis dan fleksibel untuk gardu kecil." },
  { "en": "Kerugian 'ring bus'?", "id": "Perawatan PMT membuka ring." },
  { "en": "Proses pengeringan isolasi trafo di lokasi?", "id": "Oil filtering dan vacuum drying." },
  { "en": "Apa itu 'dissolved moisture' pada oli trafo?", "id": "Kandungan air terlarut dalam oli." },
  { "en": "Dampak 'dissolved moisture' tinggi?", "id": "Menurunkan kekuatan dielektrik oli." },
  { "en": "Apa itu 'interfacial tension' (IFT) pada oli?", "id": "Ukuran kontaminasi terlarut pada oli." },
  { "en": "Nilai IFT (interfacial tension) yang rendah menandakan?", "id": "Oli sudah terkontaminasi atau tua." },
  { "en": "Representasi digital virtual dari aset fisik grid?", "id": "Kembaran digital (digital twin)." },
  { "en": "Aplikasi AI (Artificial Intelligence) di sistem tenaga?", "id": "Prediksi beban dan deteksi anomali." },
  { "en": "Penggunaan ML (Machine Learning) untuk pemeliharaan?", "id": "Pemeliharaan prediktif (predictive maintenance)." },
  { "en": "Penyimpanan data grid menggunakan server jarak jauh?", "id": "Komputasi awan (cloud computing)." },
  { "en": "Pembatasan produksi energi terbarukan saat suplai berlebih?", "id": "Curtailment." },
  { "en": "Sumber daya pembangkit berbasis inverter?", "id": "IBR (Inverter-Based Resources)." },
  { "en": "Tantangan utama IBR (Inverter-Based Resources)?", "id": "Inersia sistem yang rendah." },
  { "en": "Pembangkit yang menggabungkan beberapa jenis EBT?", "id": "Pembangkit listrik hibrida." },
  { "en": "Skema proteksi yang mengunci trip busbar lain?", "id": "Zone interlocking." },
  { "en": "Proteksi diferensial untuk saluran transmisi?", "id": "Relay 87L." },
  { "en": "Prinsip kerja relay 87L?", "id": "Membandingkan arus di kedua ujung saluran." },
  { "en": "Gangguan antara belitan stator dan rotor generator?", "id": "Stator-to-rotor fault." },
  { "en": "Analisis getaran dan suara pada trafo?", "id": "Analisis vibro-akustik." },
  { "en": "Tujuan analisis vibro-akustik?", "id": "Mendeteksi masalah mekanis inti/belitan." },
  { "en": "Pengujian isolasi pada berbagai frekuensi?", "id": "DFR (Dielectric Frequency Response) testing." },
  { "en": "Sensor untuk memantau kelembaban oli trafo secara online?", "id": "Online moisture sensor." },
  { "en": "Pemberian harga pada emisi karbon?", "id": "Penetapan harga karbon (carbon pricing)." },
  { "en": "Sertifikat yang merepresentasikan produksi 1 MWh dari EBT?", "id": "RECs (Renewable Energy Certificates)." },
  { "en": "Model investasi untuk modernisasi jaringan?", "id": "Grid modernization investment models." },
  { "en": "Arus quasi-DC yang diinduksi badai geomagnetik?", "id": "GIC (Geomagnetically Induced Currents)." },
  { "en": "Dampak GIC (Geomagnetically Induced Currents) pada trafo?", "id": "Pemanasan berlebih dan saturasi inti." },
  { "en": "Strategi pemulihan sistem dari kondisi padam total?", "id": "Strategi pemulihan black start." },
  { "en": "Urutan pemulihan beban saat black start?", "id": "Beban penting dan pembangkit lain." },
  { "en": "Transformator untuk mengontrol sudut fasa tegangan?", "id": "PST (Phase Shifting Transformer)." },
  { "en": "Fungsi utama PST (Phase Shifting Transformer)?", "id": "Mengatur aliran daya aktif." },
  { "en": "Proteksi kapasitor seri dari tegangan lebih?", "id": "MOV (Metal Oxide Varistor)." },
  { "en": "Perbedaan NGR (Neutral Grounding Resistor) dan Reaktor?", "id": "NGR resistif, reaktor induktif." },
  { "en": "Material isolasi yang bisa memperbaiki diri?", "id": "Material isolasi swa-pulih." },
  { "en": "Material superkonduktor untuk kabel daya?", "id": "Kabel superkonduktor." },
  { "en": "Oli trafo yang mudah terurai di alam?", "id": "Oli trafo biodegradabel (ester)." },
  { "en": "Apa itu 'grid edge'?", "id": "Batas antara jaringan utilitas dan konsumen." },
  { "en": "Perangkat yang berada di 'grid edge'?", "id": "Smart meter, PLTS atap, EV charger." },
  { "en": "Sumber daya energi terdistribusi?", "id": "DER (Distributed Energy Resources)." },
  { "en": "Sistem untuk mengelola DER (Distributed Energy Resources)?", "id": "DERMS (DER Management System)." },
  { "en": "Sinkronisasi data PMU (Phasor Measurement Unit) menggunakan?", "id": "Sinyal waktu dari satelit GPS." },
  { "en": "Pemanfaatan data PMU (Phasor Measurement Unit) untuk stabilitas?", "id": "Deteksi ayunan daya dini." },
  { "en": "Protokol komunikasi untuk pertukaran data PMU?", "id": "IEEE C37.118." },
  { "en": "Sistem manajemen energi pada level pusat kontrol?", "id": "EMS (Energy Management System)." },
  { "en": "Sistem yang mengelola baterai pada BESS?", "id": "BMS (Battery Management System)." },
  { "en": "Fungsi utama BMS (Battery Management System)?", "id": "Melindungi baterai, menyeimbangkan sel." },
  { "en": "Penuaan baterai pada BESS disebut?", "id": "Degradasi kapasitas." },
  { "en": "Ukuran kesehatan baterai?", "id": "State of Health (SoH)." },
  { "en": "Ukuran tingkat isian baterai?", "id": "State of Charge (SoC)." },
  { "en": "Apa itu 'ancillary service' regulasi tegangan?", "id": "Menyediakan atau menyerap daya reaktif." },
  { "en": "Pembangkit yang menyediakan 'ancillary service' inersia?", "id": "Pembangkit sinkron (PLTU, PLTA)." },
  { "en": "Apa itu 'voltage source converter' (VSC)?", "id": "Konverter yang bisa menghasilkan tegangan AC." },
  { "en": "Aplikasi VSC (Voltage Source Converter)?", "id": "STATCOM, VSC-HVDC, drive motor." },
  { "en": "Kapasitor yang dipasang seri di saluran transmisi?", "id": "Kapasitor seri." },
  { "en": "Tujuan pemasangan kapasitor seri?", "id": "Mengurangi reaktansi induktif saluran." },
  { "en": "Dampak positif kapasitor seri?", "id": "Meningkatkan kapasitas transfer daya." },
  { "en": "Risiko penggunaan kapasitor seri?", "id": "Sub-Synchronous Resonance (SSR)." },
  { "en": "Apa itu Sub-Synchronous Resonance (SSR)?", "id": "Interaksi osilasi listrik dan mekanik." },
  { "en": "Sistem eksitasi yang sumber dayanya dari terminal generator?", "id": "Sistem eksitasi statis." },
  { "en": "Apa itu 'field flashing' pada generator?", "id": "Memberi tegangan awal ke belitan medan." },
  { "en": "Tujuan 'field flashing'?", "id": "Membangun medan magnet sisa (residual)." },
  { "en": "Apa itu 'de-excitation'?", "id": "Proses menghilangkan medan magnet cepat." },
  { "en": "Peralatan untuk 'de-excitation'?", "id": "Field breaker dan resistor pembuang." },
  { "en": "Apa itu 'runback' pada pembangkit?", "id": "Pengurangan output daya otomatis." },
  { "en": "Penyebab skema 'runback' aktif?", "id": "Gangguan pada peralatan bantu." },
  { "en": "Apa itu 'islanding detection'?", "id": "Deteksi kondisi DG terpisah dari grid." },
  { "en": "Metode 'islanding detection'?", "id": "Pasif (tegangan/frekuensi) dan aktif (injeksi sinyal)." },
  { "en": "Apa itu 'grid-forming' BESS?", "id": "BESS yang dapat membentuk jaringan sendiri." },
  { "en": "Kegunaan 'grid-forming' BESS?", "id": "Black start, menstabilkan microgrid." },
  { "en": "Peralatan yang menghubungkan dua busbar berbeda tegangan?", "id": "Transformator." },
  { "en": "Peralatan yang menghubungkan dua busbar sama tegangan?", "id": "Bus coupler atau bus tie." },
  { "en": "Fungsi bus coupler?", "id": "Memparalel atau memisah busbar." },
  { "en": "Apa itu 'switchgear'?", "id": "Kombinasi peralatan hubung dan proteksi." },
  { "en": "Contoh 'switchgear' tegangan menengah?", "id": "Kubikel 20 kV." },
  { "en": "Jenis 'switchgear' berdasarkan isolasinya?", "id": "Air-insulated, gas-insulated, solid-insulated." },
  { "en": "Apa itu 'arc resistant switchgear'?", "id": "Switchgear tahan terhadap ledakan busur api." },
  { "en": "Tujuan 'arc resistant switchgear'?", "id": "Meningkatkan keselamatan personil." },
  { "en": "Apa itu 'transfer switch'?", "id": "Saklar untuk memindah sumber daya." },
  { "en": "Jenis 'transfer switch'?", "id": "Manual (MTS) dan otomatis (ATS)." },
  { "en": "Kondisi saat generator berjalan sebagai motor?", "id": "Motoring." },
  { "en": "Dampak 'motoring' pada turbin uap?", "id": "Overheating pada sudu turbin." },
  { "en": "Apa itu 'pole slipping' pada generator?", "id": "Rotor kehilangan sinkronisasi dengan stator." },
  { "en": "Proteksi yang mendeteksi 'pole slipping'?", "id": "Relay lepas sinkron (loss of synchronism)." },
  { "en": "Penyebab 'pole slipping'?", "id": "Gangguan berat, eksitasi lemah." },
  { "en": "Kondisi tegangan tidak seimbang di terminal generator?", "id": "Tegangan urutan negatif." },
  { "en": "Dampak tegangan urutan negatif pada generator?", "id": "Pemanasan berlebih pada rotor." },
  { "en": "Kapasitas generator menahan tegangan tak seimbang?", "id": "Kurva kapabilitas urutan negatif." },
  { "en": "Perlindungan trafo dari tekanan lebih mendadak?", "id": "Relay tekanan tiba-tiba (sudden pressure)." },
  { "en": "Prinsip kerja relay Buchholz?", "id": "Mendeteksi akumulasi gas atau aliran oli." },
  { "en": "Alarm pada relay Buchholz menandakan?", "id": "Gangguan ringan atau akumulasi gas." },
  { "en": "Trip pada relay Buchholz menandakan?", "id": "Gangguan berat atau aliran oli deras." },
  { "en": "Pengujian untuk mengecek polaritas belitan trafo?", "id": "Uji polaritas." },
  { "en": "Jenis polaritas pada trafo?", "id": "Aditif dan subtraktif." },
  { "en": "Pentingnya mengetahui polaritas trafo?", "id": "Untuk operasi paralel yang benar." },
  { "en": "Syarat utama paralel trafo?", "id": "Tegangan, polaritas, grup vektor sama." },
  { "en": "Dampak jika grup vektor trafo paralel berbeda?", "id": "Timbul arus sirkulasi sangat besar." },
  { "en": "Apa itu 'load tap changer' (LTC)?", "id": "Sama dengan OLTC (On-Load Tap Changer)." },
  { "en": "Komponen utama LTC (Load Tap Changer)?", "id": "Selector switch dan diverter switch." },
  { "en": "Fungsi 'diverter switch' pada LTC?", "id": "Memindahkan arus beban tanpa terputus." },
  { "en": "Apa itu 'voltage collapse'?", "id": "Keruntuhan tegangan progresif dan tak terkendali." },
  { "en": "Penyebab utama 'voltage collapse'?", "id": "Kekurangan daya reaktif masif." },
  { "en": "Indikator awal 'voltage collapse'?", "id": "Penurunan tegangan yang terus menerus." },
  { "en": "Strategi pengerasan jaringan terhadap cuaca ekstrem?", "id": "Grid hardening." },
  { "en": "Contoh 'grid hardening'?", "id": "Memasang kabel bawah tanah, tiang komposit." },
  { "en": "Dampak kebakaran hutan pada saluran transmisi?", "id": "Flashover akibat asap dan panas." },
  { "en": "Teknologi mobil listrik (EV) menyuplai daya ke grid?", "id": "V2G (Vehicle-to-Grid)." },
  { "en": "Teknologi mobil listrik (EV) menyuplai daya ke rumah?", "id": "V2H (Vehicle-to-Home)." },
  { "en": "Strategi pengisian daya mobil listrik (EV) yang dikontrol utilitas?", "id": "Pengisian daya terkelola (managed charging)." },
  { "en": "Dampak pengisian daya mobil listrik (EV) serentak pada trafo distribusi?", "id": "Beban lebih (overload)." },
  { "en": "Identifikasi beban individu dari data meter terpusat?", "id": "NILM (Non-Intrusive Load Monitoring)." },
  { "en": "Isu utama terkait data smart meter?", "id": "Privasi data pelanggan." },
  { "en": "Penggunaan data synchrophasor untuk kontrol real-time?", "id": "Wide Area Control Systems (WACS)." },
  { "en": "Analisis stabilitas sinyal kecil di sistem tenaga?", "id": "Analisis modal (eigenvalue analysis)." },
  { "en": "Torsi yang menjaga generator tetap sinkron?", "id": "Torsi sinkronisasi." },
  { "en": "Torsi yang meredam osilasi sudut rotor?", "id": "Torsi redaman." },
  { "en": "Jaringan HVDC (High Voltage Direct Current) dengan lebih dari dua terminal?", "id": "Sistem MTDC (Multi-terminal DC)." },
  { "en": "Perangkat FACTS (Flexible AC Transmission Systems) untuk mengontrol aliran daya antar saluran?", "id": "IPFC (Interline Power Flow Controller)." },
  { "en": "Strategi kontrol untuk VSC-HVDC (Voltage Source Converter-HVDC)?", "id": "Vector control." },
  { "en": "Pengujian relay proteksi dengan mensimulasikan kondisi ujung-ke-ujung?", "id": "End-to-end testing." },
  { "en": "Pengujian relay dengan menginjeksi arus/tegangan ke terminal relay?", "id": "Pengujian injeksi sekunder." },
  { "en": "Pengujian dengan menginjeksi arus besar ke sirkuit primer?", "id": "Pengujian injeksi primer." },
  { "en": "Format file standar untuk catatan gangguan (fault record)?", "id": "COMTRADE." },
  { "en": "Penjadwalan unit pembangkit dengan mempertimbangkan keamanan jaringan?", "id": "SCUC (Security-Constrained Unit Commitment)." },
  { "en": "Analisis dampak kegagalan komponen secara real-time?", "id": "Analisis kontingensi real-time." },
  { "en": "Model penentuan harga transmisi listrik?", "id": "Transmission pricing models." },
  { "en": "Batas aman antara rating arrester dan ketahanan isolasi alat?", "id": "Margin proteksi (protective margin)." },
  { "en": "Rasio antara level proteksi arrester dan BIL (Basic Insulation Level) alat?", "id": "Rasio proteksi (protective ratio)." },
  { "en": "Tegangan lebih durasi panjang akibat gangguan atau switching?", "id": "TOV (Temporary Overvoltage)." },
  { "en": "Dampak TOV (Temporary Overvoltage) pada arrester?", "id": "Bisa menyebabkan kerusakan termal." },
  { "en": "Transformator yang mengontrol tegangan dan sudut fasa?", "id": "Quad-booster transformer." },
  { "en": "Transformator yang dirancang untuk aplikasi HVDC (High Voltage Direct Current)?", "id": "Transformator konverter." },
  { "en": "Tantangan desain transformator konverter?", "id": "Harmonisa tinggi dan komponen DC." },
  { "en": "Reaktor yang dipasang paralel dengan saluran?", "id": "Reaktor shunt." },
  { "en": "Standar keselamatan kelistrikan di tempat kerja dari NFPA?", "id": "NFPA 70E." },
  { "en": "Badan pemerintah AS untuk keselamatan kerja?", "id": "OSHA (Occupational Safety and Health Administration)." },
  { "en": "Batas pendekatan aman dari bahaya arc flash?", "id": "Arc flash boundary." },
  { "en": "Batas pendekatan untuk pekerja terkualifikasi?", "id": "Limited approach boundary." },
  { "en": "Operator yang mengelola stasiun pengisian daya mobil listrik (EV)?", "id": "CPO (Charge Point Operator)." },
  { "en": "Sistem manajemen energi di tingkat gedung/industri?", "id": "EMS (Energy Management System)." },
  { "en": "Program respons permintaan (DR) berbasis harga?", "id": "Time-of-Use (TOU) pricing." },
  { "en": "Kemampuan pembangkit EBT bertahan saat tegangan rendah?", "id": "LVRT (Low Voltage Ride Through)." },
  { "en": "Apa itu 'islanding'?", "id": "Bagian jaringan ber-DG terisolasi." },
  { "en": "Apa itu 'blackout restoration'?", "id": "Proses pemulihan dari padam total." },
  { "en": "Pembangkit pertama yang dihidupkan saat black start?", "id": "Black start unit." },
  { "en": "Jalur transmisi pertama yang diberi energi saat pemulihan?", "id": "Cranking path." },
  { "en": "Apa itu 'load forecast'?", "id": "Prediksi permintaan beban listrik." },
  { "en": "Jenis 'load forecast' berdasarkan waktu?", "id": "Jangka pendek, menengah, dan panjang." },
  { "en": "Sistem penyimpanan energi mekanik di bawah tanah?", "id": "Gravitricity." },
  { "en": "Prinsip kerja 'gravitricity'?", "id": "Mengangkat dan menurunkan beban berat." },
  { "en": "Apa itu 'virtual power plant' (VPP)?", "id": "Agregasi DER (Distributed Energy Resources) yang dikontrol terpusat." },
  { "en": "Fungsi 'virtual power plant' (VPP)?", "id": "Berpartisipasi di pasar listrik." },
  { "en": "Apa itu 'transformer derating'?", "id": "Pengurangan kapasitas trafo akibat kondisi non-ideal." },
  { "en": "Penyebab 'transformer derating'?", "id": "Harmonisa, ketinggian, suhu ambien tinggi." },
  { "en": "Uji ketahanan catu daya DC gardu induk?", "id": "Uji kapasitas baterai (discharge test)." },
  { "en": "Kondisi baterai di-charge dengan tegangan rendah konstan?", "id": "Float charging." },
  { "en": "Pengisian baterai dengan tegangan lebih tinggi setelah discharge?", "id": "Equalizing charging." },
  { "en": "Tujuan 'equalizing charging'?", "id": "Menyeimbangkan tegangan sel-sel baterai." },
  { "en": "Peralatan untuk memonitor kondisi PMT (Pemutus Tenaga) secara online?", "id": "Circuit breaker monitoring system." },
  { "en": "Parameter yang dimonitor pada PMT (Pemutus Tenaga)?", "id": "Waktu buka/tutup, tekanan gas SF6." },
  { "en": "Apa itu 'point on wave switching'?", "id": "Operasi switching pada titik gelombang tertentu." },
  { "en": "Tujuan 'point on wave switching'?", "id": "Mengurangi transien saat switching." },
  { "en": "Material kontak pada PMT (Pemutus Tenaga) vakum?", "id": "Tembaga-kromium (CuCr)." },
  { "en": "Apa itu 'current chopping' pada PMT (Pemutus Tenaga) vakum?", "id": "Pemutusan arus sebelum titik nol alami." },
  { "en": "Dampak 'current chopping'?", "id": "Menimbulkan tegangan lebih (overvoltage)." },
  { "en": "Relay yang membandingkan fasa arus?", "id": "Relay perbandingan fasa." },
  { "en": "Proteksi saluran transmisi menggunakan sinyal carrier?", "id": "Carrier-aided protection." },
  { "en": "Skema 'carrier-aided protection'?", "id": "Blocking, permissive, transfer trip." },
  { "en": "Apa itu 'power line carrier communication' (PLCC)?", "id": "Komunikasi menggunakan saluran transmisi itu sendiri." },
  { "en": "Frekuensi sinyal PLCC (Power Line Carrier Communication)?", "id": "Frekuensi radio (kHz)." },
  { "en": "Peralatan untuk menyuntikkan dan menerima sinyal PLCC?", "id": "PLCC terminal." },
  { "en": "Apa itu 'ferroresonance'?", "id": "Resonansi non-linear antara induktor dan kapasitor." },
  { "en": "Kondisi pemicu 'ferroresonance'?", "id": "Switching pada trafo tak berbeban." },
  { "en": "Apa itu 'harmonics filter'?", "id": "Filter untuk meredam frekuensi harmonisa." },
  { "en": "Jenis 'harmonics filter'?", "id": "Pasif, aktif, dan hibrida." },
  { "en": "Filter yang di-tuning ke satu frekuensi harmonisa?", "id": "Single-tuned filter." },
  { "en": "Filter yang meredam beberapa frekuensi harmonisa?", "id": "High-pass filter." },
  { "en": "Apa itu 'neutral displacement'?", "id": "Pergeseran tegangan titik netral." },
  { "en": "Penyebab 'neutral displacement'?", "id": "Beban tak seimbang, gangguan fasa-tanah." },
  { "en": "Proteksi terhadap 'neutral displacement'?", "id": "Relay tegangan lebih netral." },
  { "en": "Apa itu 'cold load pickup'?", "id": "Lonjakan arus saat menyuplai beban padam lama." },
  { "en": "Penyebab 'cold load pickup'?", "id": "Beban pemanas dan motor start serentak." },
  { "en": "Pengaturan relay untuk mengatasi 'cold load pickup'?", "id": "Setelan arus lebih sementara dinaikkan." },
  { "en": "Peralatan untuk mengukur sudut fasa?", "id": "Phasor Measurement Unit (PMU)." },
  { "en": "Pemanfaatan data sudut fasa?", "id": "Memonitor stabilitas dan kongesti sistem." },
  { "en": "Perbedaan sudut fasa antara dua titik di sistem?", "id": "Menunjukkan arah dan besaran aliran daya." },
  { "en": "Apa itu 'cascading outage'?", "id": "Pemadaman beruntun yang menyebar luas." },
  { "en": "Pemicu 'cascading outage'?", "id": "Satu gangguan awal yang tidak terisolasi." },
  { "en": "Skema proteksi untuk mencegah 'cascading outage'?", "id": "System Protection Scheme (SPS)." },
  { "en": "Nama lain 'System Protection Scheme' (SPS)?", "id": "Remedial Action Scheme (RAS)." },
  { "en": "Contoh aksi dari SPS (System Protection Scheme)?", "id": "Generator tripping, load shedding." },
  { "en": "Apa itu 'situational awareness' bagi operator grid?", "id": "Pemahaman kondisi sistem secara real-time." },
  { "en": "Alat bantu 'situational awareness'?", "id": "Layar SCADA, data PMU, alarm." },
  { "en": "Pembangkit yang menggunakan biomassa sebagai bahan bakar?", "id": "PLTBm (Pembangkit Listrik Tenaga Biomassa)." },
  { "en": "Contoh biomassa untuk PLTBm?", "id": "Cangkang sawit, sekam padi, sampah kayu." },
  { "en": "Pembangkit yang menggunakan sampah kota?", "id": "PLTSa (Pembangkit Listrik Tenaga Sampah)." },
  { "en": "Proses utama pada PLTSa?", "id": "Insinerasi (pembakaran) atau gasifikasi." },
  { "en": "Apa itu 'wheeling charge'?", "id": "Biaya sewa jaringan transmisi." },
  { "en": "Siapa yang membayar 'wheeling charge'?", "id": "Pihak ketiga yang menggunakan jaringan." },
  { "en": "Sistem untuk mendeteksi aktivitas mencurigakan di jaringan SCADA?", "id": "IDS (Intrusion Detection System)." },
  { "en": "Database yang menyimpan data historis proses industri?", "id": "Data historian." },
  { "en": "Penggunaan AI (Artificial Intelligence) untuk deteksi anomali lalu lintas jaringan?", "id": "Network anomaly detection." },
  { "en": "Inverter yang meniru perilaku generator sinkron?", "id": "VSM (Virtual Synchronous Machine)." },
  { "en": "Nama lain VSM (Virtual Synchronous Machine)?", "id": "VSG (Virtual Synchronous Generator)." },
  { "en": "Fungsi utama VSM (Virtual Synchronous Machine)?", "id": "Menyediakan inersia virtual ke sistem." },
  { "en": "Manajemen strategis untuk sekelompok transformator?", "id": "Manajemen armada transformator (transformer fleet)." },
  { "en": "Perbedaan RCM (Reliability-Centered Maintenance) dan RBM (Risk-Based Maintenance)?", "id": "RCM fokus fungsi, RBM fokus risiko." },
  { "en": "Penggunaan drone dalam inspeksi jaringan listrik?", "id": "Inspeksi visual, termal, dan korona." },
  { "en": "Robot untuk inspeksi di dalam gardu GIS?", "id": "Robot inspeksi." },
  { "en": "Standar pengisian daya cepat untuk truk listrik?", "id": "MCS (Megawatt Charging System)." },
  { "en": "Teknologi penukaran baterai mobil listrik (EV) kosong dengan yang penuh?", "id": "Tukar baterai (battery swapping)." },
  { "en": "Teknologi pengisian daya mobil listrik (EV) tanpa kabel?", "id": "Wireless EV charging." },
  { "en": "Saluran transmisi berisolasi gas SF6?", "id": "GIL (Gas-Insulated Lines)." },
  { "en": "Perangkat untuk membatasi arus gangguan secara otomatis?", "id": "FCL (Fault Current Limiter)." },
  { "en": "Jenis FCL (Fault Current Limiter) yang menggunakan superkonduktor?", "id": "SFCL (Superconducting FCL)." },
  { "en": "Rating kapasitas trafo berdasarkan kondisi lingkungan real-time?", "id": "DTR (Dynamic Thermal Rating)." },
  { "en": "Skema voting proteksi yang membutuhkan dua dari tiga sinyal?", "id": "Skema dua dari tiga (2-out-of-3)." },
  { "en": "Skema proteksi sistem terintegrasi yang luas?", "id": "SIPS (System Integrity Protection Schemes)." },
  { "en": "Komunikasi antar relay untuk proteksi saluran?", "id": "Teleproteksi." },
  { "en": "Laju perubahan frekuensi sistem tenaga?", "id": "RoCoF (Rate of Change of Frequency)." },
  { "en": "RoCoF (Rate of Change of Frequency) tinggi menandakan?", "id": "Inersia sistem rendah." },
  { "en": "Stabilitas sistem terhadap gangguan kecil?", "id": "Stabilitas sinyal kecil." },
  { "en": "Osilasi frekuensi rendah antar area sistem interkoneksi?", "id": "Osilasi antar-area (inter-area oscillations)." },
  { "en": "Hidrogen yang diproduksi menggunakan energi terbarukan?", "id": "Hidrogen hijau (green hydrogen)." },
  { "en": "Teknologi penangkapan CO2 dari gas buang pembangkit?", "id": "CCUS (Carbon Capture, Utilization, and Storage)." },
  { "en": "Proses mengganti bahan bakar fosil dengan listrik?", "id": "Elektrifikasi." },
  { "en": "Model transaksi energi langsung antar konsumen?", "id": "P2P (Peer-to-Peer) energy trading." },
  { "en": "Sistem pasar energi berbasis sinyal harga real-time?", "id": "Energi transaktif (transactive energy)." },
  { "en": "Model regulasi utilitas berdasarkan kinerja?", "id": "PBR (Performance-Based Regulation)." },
  { "en": "Pengujian untuk mengukur noise radio akibat korona?", "id": "Uji RIV (Radio Influence Voltage)." },
  { "en": "Tingkat interferensi tegangan radio disebut?", "id": "RIV (Radio Influence Voltage) level." },
  { "en": "Pengujian penuaan dipercepat pada material isolasi?", "id": "Accelerated aging tests." },
  { "en": "Apa itu 'grid code compliance'?", "id": "Kepatuhan pembangkit terhadap aturan jaringan." },
  { "en": "Studi untuk memastikan 'grid code compliance'?", "id": "Studi interkoneksi." },
  { "en": "Apa itu 'congestion cost'?", "id": "Biaya tambahan akibat kemacetan transmisi." },
  { "en": "Apa itu 'redispatch'?", "id": "Mengubah output pembangkit untuk mengatasi kongesti." },
  { "en": "Apa itu 'curtailment cost'?", "id": "Kerugian akibat tidak memproduksi energi EBT." },
  { "en": "Sistem pendingin trafo OFWF singkatan dari?", "id": "Oil Forced Water Forced." },
  { "en": "Di mana trafo pendingin OFWF digunakan?", "id": "PLTA (Pembangkit Listrik Tenaga Air)." },
  { "en": "Apa itu 'transformer sound level'?", "id": "Tingkat kebisingan yang dihasilkan trafo." },
  { "en": "Sumber utama kebisingan trafo?", "id": "Getaran inti (magnetostriction) dan kipas." },
  { "en": "Apa itu 'magnetostriction'?", "id": "Perubahan dimensi material akibat medan magnet." },
  { "en": "Dinding peredam suara di sekitar gardu induk?", "id": "Sound barrier atau enclosure." },
  { "en": "Apa itu 'stray flux' pada trafo?", "id": "Fluks magnet yang keluar dari inti." },
  { "en": "Dampak 'stray flux'?", "id": "Menimbulkan pemanasan pada tangki." },
  { "en": "Pelindung magnetik di dalam tangki trafo?", "id": "Magnetic shield." },
  { "en": "Apa itu 'gas-in-oil' analysis?", "id": "Analisis gas terlarut dalam oli (DGA)." },
  { "en": "Segitiga Duval digunakan untuk?", "id": "Mendiagnosis jenis gangguan dari DGA." },
  { "en": "Apa itu 'breaker-and-a-half' scheme?", "id": "Skema satu setengah pemutus." },
  { "en": "Jumlah PMT per sirkuit pada skema 'breaker-and-a-half'?", "id": "1.5 PMT per sirkuit." },
  { "en": "Keandalan skema 'breaker-and-a-half'?", "id": "Sangat tinggi." },
  { "en": "Apa itu 'stuck breaker condition'?", "id": "Kondisi PMT gagal membuka." },
  { "en": "Proteksi yang bekerja saat 'stuck breaker condition'?", "id": "Breaker Failure Protection (BFP)." },
  { "en": "Apa itu 'single-phase tripping'?", "id": "Membuka hanya satu fasa yang terganggu." },
  { "en": "Tujuan 'single-phase tripping'?", "id": "Menjaga sinkronisme pada saluran ganda." },
  { "en": "Kondisi setelah 'single-phase tripping'?", "id": "Sistem berjalan dengan dua fasa." },
  { "en": "Apa itu 'auto-reclosing'?", "id": "Menutup kembali PMT secara otomatis." },
  { "en": "Tujuan 'auto-reclosing'?", "id": "Memulihkan suplai dari gangguan temporer." },
  { "en": "Waktu jeda sebelum 'auto-reclosing'?", "id": "Dead time." },
  { "en": "Tujuan 'dead time'?", "id": "Memberi waktu de-ionisasi media gangguan." },
  { "en": "Reclosing untuk gangguan permanen disebut?", "id": "Reclosing onto a fault." },
  { "en": "Jumlah maksimum upaya reclosing?", "id": "Dapat diatur (biasanya 1-3 kali)." },
  { "en": "Apa itu 'synchro-check'?", "id": "Pemeriksaan kondisi sinkron sebelum menutup PMT." },
  { "en": "Kapan 'synchro-check' diperlukan?", "id": "Saat menutup PMT yang memisahkan sistem." },
  { "en": "Relay yang berfungsi untuk 'synchro-check'?", "id": "Relay 25." },
  { "en": "Apa itu 'dead bus closing'?", "id": "Menutup PMT ke busbar yang padam." },
  { "en": "Apa itu 'live line, dead bus' condition?", "id": "Saluran bertegangan, busbar padam." },
  { "en": "Apa itu 'power swing'?", "id": "Osilasi daya aktif dan reaktif." },
  { "en": "Proteksi yang harus diblokir saat 'power swing'?", "id": "Relay jarak." },
  { "en": "Fungsi 'power swing blocking' (PSB)?", "id": "Mencegah relay jarak trip salah." },
  { "en": "Apa itu 'voltage transformer' (VT)?", "id": "Sama dengan Potential Transformer (PT)." },
  { "en": "Apa itu 'coupling capacitor voltage transformer' (CCVT)?", "id": "Sama dengan CVT." },
  { "en": "Beban maksimum yang boleh terhubung ke CT/PT?", "id": "Burden." },
  { "en": "Dampak 'burden' berlebih pada CT/PT?", "id": "Menurunkan akurasi pengukuran." },
  { "en": "Apa itu 'knee point voltage' pada CT?", "id": "Batas saturasi inti CT." },
  { "en": "Pentingnya 'knee point voltage' untuk proteksi?", "id": "CT tidak boleh jenuh saat gangguan." },
  { "en": "Apa itu CT kelas P?", "id": "CT untuk tujuan proteksi (Protection)." },
  { "en": "Apa itu CT kelas M?", "id": "CT untuk tujuan pengukuran (Metering)." },
  { "en": "Faktor akurasi batas (ALF) pada CT proteksi?", "id": "Accuracy Limit Factor." },
  { "en": "ALF (Accuracy Limit Factor) 10 artinya?", "id": "CT akurat hingga 10x arus nominal." },
  { "en": "Apa itu 'transient response' pada CT?", "id": "Perilaku CT saat terjadi transien DC." },
  { "en": "Apa itu 'remanence' pada inti CT?", "id": "Sisa magnetisme setelah arus hilang." },
  { "en": "Dampak 'remanence'?", "id": "Bisa menyebabkan saturasi lebih cepat." },
  { "en": "Cara menghilangkan 'remanence'?", "id": "Degaussing." },
  { "en": "Rangkaian sekunder VT/PT harus diproteksi dengan?", "id": "MCB atau sekering kecil." },
  { "en": "Sisi sekunder VT/PT harus ditanahkan?", "id": "Ya, untuk keselamatan." },
  { "en": "Apa itu 'ferroresonance suppression circuit' pada VT?", "id": "Rangkaian untuk meredam ferroresonansi." },
  { "en": "Apa itu 'asset information management' (AIM)?", "id": "Manajemen informasi aset teknik." },
  { "en": "Apa itu 'work order management'?", "id": "Manajemen perintah kerja pemeliharaan." },
  { "en": "Sistem manajemen pemeliharaan terkomputerisasi?", "id": "CMMS (Computerized Maintenance Management System)." },
  { "en": "Integrasi CMMS dengan sistem enterprise?", "id": "EAM (Enterprise Asset Management)." },
  { "en": "Apa itu 'root cause failure analysis' (RCFA)?", "id": "Analisis akar penyebab kegagalan." },
  { "en": "Tujuan RCFA (Root Cause Failure Analysis)?", "id": "Mencegah kegagalan serupa terulang." },
  { "en": "Diagram tulang ikan untuk RCFA?", "id": "Diagram Ishikawa." },
  { "en": "Metode '5 Whys' digunakan untuk?", "id": "Menemukan akar penyebab masalah." },
  { "en": "Konsep integrasi sektor listrik, panas, dan transportasi?", "id": "Penggandengan sektor (sector coupling)." },
  { "en": "Perangkat yang menggunakan listrik untuk menghasilkan hidrogen?", "id": "Elektroliser." },
  { "en": "Peran elektroliser dalam penyeimbangan grid?", "id": "Menyerap kelebihan energi terbarukan." },
  { "en": "Amonia yang diproduksi menggunakan hidrogen hijau?", "id": "Amonia hijau (green ammonia)." },
  { "en": "Tantangan utama pemutus tenaga DC (Direct Current)?", "id": "Tidak ada arus titik nol alami." },
  { "en": "Topologi konverter VSC-HVDC (Voltage Source Converter-HVDC) modern?", "id": "MMC (Modular Multi-level Converter)." },
  { "en": "Keuntungan MMC (Modular Multi-level Converter)?", "id": "Harmonisa rendah, skalabilitas, redundansi." },
  { "en": "Penggunaan kontrol HVDC (High Voltage Direct Current) untuk meredam osilasi?", "id": "Power oscillation damping." },
  { "en": "Jaringan komunikasi level proses di gardu digital?", "id": "Bus proses (process bus)." },
  { "en": "Jaringan komunikasi level stasiun di gardu digital?", "id": "Bus stasiun (station bus)." },
  { "en": "Perangkat yang mengubah sinyal CT/PT analog ke digital?", "id": "MU (Merging Unit)." },
  { "en": "Data digital hasil sampling dari MU (Merging Unit)?", "id": "Nilai sampel (Sampled Values - SV)." },
  { "en": "Pesan status antar IED di bus stasiun?", "id": "Pesan GOOSE." },
  { "en": "Menyesuaikan indeks keandalan dengan kondisi cuaca?", "id": "Normalisasi cuaca." },
  { "en": "Penilaian risiko untuk perencanaan grid berbasis probabilitas?", "id": "Probabilistic risk assessment." },
  { "en": "Gardu induk yang bisa dipindahkan (portabel)?", "id": "Gardu induk bergerak (mobile substation)." },
  { "en": "Penyimpanan energi menggunakan panas garam cair?", "id": "Penyimpanan energi termal." },
  { "en": "Penyimpanan energi dalam bentuk gas hidrogen?", "id": "Penyimpanan energi hidrogen." },
  { "en": "Sistem yang menggabungkan beberapa teknologi penyimpanan energi?", "id": "Sistem penyimpanan energi hibrida." },
  { "en": "Contoh sistem penyimpanan energi hibrida?", "id": "Baterai digabung dengan superkapasitor." },
  { "en": "Monitoring lucutan parsial (PD) secara online pada GIS?", "id": "Online PD monitoring." },
  { "en": "Penentuan rating kabel bawah tanah secara dinamis?", "id": "Dynamic cable rating." },
  { "en": "Sensor serat optik untuk memantau suhu konduktor?", "id": "Distributed Temperature Sensing (DTS)." },
  { "en": "Algoritma smart charging untuk mengisi daya mobil listrik (EV) saat tarif murah?", "id": "Valley-filling." },
  { "en": "Pengisian daya dua arah pada mobil listrik (EV)?", "id": "Bidirectional charging (V2G/V2H)." },
  { "en": "Dampak stasiun pengisian cepat pada kualitas daya?", "id": "Harmonisa, sag, dan transien." },
  { "en": "Simulasi sistem tenaga yang berjalan secara real-time?", "id": "RTDS (Real-Time Digital Simulation)." },
  { "en": "Pengujian perangkat keras kontroler dengan simulasi virtual?", "id": "HIL (Hardware-in-the-loop) testing." },
  { "en": "Simulasi gabungan antara sistem tenaga dan jaringan komunikasi?", "id": "Ko-simulasi (co-simulation)." },
  { "en": "Analisis kesalahan operasi pada sistem proteksi?", "id": "Analisis misoperation." },
  { "en": "Kegagalan pada sistem proteksi yang tidak terdeteksi?", "id": "Kegagalan tersembunyi (hidden failures)." },
  { "en": "Skema proteksi yang menggunakan data dari area luas?", "id": "WAP (Wide Area Protection)." },
  { "en": "Pembatas arus gangguan superkonduktor?", "id": "SFCL (Superconducting Fault Current Limiter)." },
  { "en": "Transfer daya listrik tanpa menggunakan kabel?", "id": "Wireless power transfer (WPT)." },
  { "en": "Inverter yang mampu membentuk tegangan dan frekuensi sendiri?", "id": "Grid-forming inverter." },
  { "en": "Kebutuhan utama untuk sistem berinersia rendah?", "id": "Grid-forming inverter." },
  { "en": "Peralatan STATCOM (Static Synchronous Compensator) yang dapat dipindahkan?", "id": "Mobile STATCOM." },
  { "en": "Tujuan Mobile STATCOM?", "id": "Dukungan tegangan temporer." },
  { "en": "Standar IEC (International Electrotechnical Commission) untuk gardu digital?", "id": "IEC 61850." },
  { "en": "Protokol komunikasi pada IEC 61850-9-2?", "id": "Sampled Values (SV)." },
  { "en": "Protokol komunikasi pada IEC 61850-8-1?", "id": "GOOSE dan MMS." },
  { "en": "Sinkronisasi waktu presisi di gardu digital?", "id": "Precision Time Protocol (PTP)." },
  { "en": "Standar untuk PTP (Precision Time Protocol)?", "id": "IEEE 1588." },
  { "en": "Apa itu 'digital twin' untuk sistem proteksi?", "id": "Model virtual untuk pengujian relay." },
  { "en": "Pemanfaatan 'digital twin' proteksi?", "id": "Menguji setelan relay sebelum implementasi." },
  { "en": "Apa itu 'time-domain simulation'?", "id": "Simulasi yang menghitung nilai sesaat." },
  { "en": "Aplikasi 'time-domain simulation'?", "id": "Studi transien elektromagnetik (EMT)." },
  { "en": "Apa itu 'phasor-domain simulation'?", "id": "Simulasi yang menggunakan nilai fasor." },
  { "en": "Aplikasi 'phasor-domain simulation'?", "id": "Studi stabilitas dan aliran daya." },
  { "en": "Apa itu 'electromechanical oscillations'?", "id": "Osilasi antara generator di sistem." },
  { "en": "Frekuensi 'electromechanical oscillations'?", "id": "Biasanya 0.1 hingga 2 Hz." },
  { "en": "Peredam osilasi daya (Power Oscillation Damper - POD) ada di?", "id": "AVR, HVDC, FACTS." },
  { "en": "Apa itu 'subsynchronous torsional interaction' (SSTI)?", "id": "Interaksi antara kontroler dan torsi mekanik." },
  { "en": "Penyebab SSTI (subsynchronous torsional interaction)?", "id": "Kontroler VSC, STATCOM." },
  { "en": "Analisis risiko kegagalan mode jamak (common mode)?", "id": "Common-mode failure analysis." },
  { "en": "Contoh 'common-mode failure'?", "id": "Kegagalan catu daya DC untuk semua relay." },
  { "en": "Strategi mitigasi 'common-mode failure'?", "id": "Redundansi dan diversitas." },
  { "en": "Menggunakan dua jenis relay berbeda untuk proteksi yang sama?", "id": "Prinsip diversitas." },
  { "en": "Apa itu 'line-commutated converter' (LCC)?", "id": "Konverter HVDC yang membutuhkan jaringan kuat." },
  { "en": "Kelemahan LCC (Line-Commutated Converter)?", "id": "Mengonsumsi daya reaktif, risiko gagal komutasi." },
  { "en": "Apa itu 'commutation failure'?", "id": "Kegagalan proses pemindahan arus antar thyristor." },
  { "en": "Penyebab 'commutation failure'?", "id": "Penurunan tegangan AC yang tiba-tiba." },
  { "en": "Reaktor untuk memperhalus arus DC pada HVDC?", "id": "Smoothing reactor." },
  { "en": "Filter pada stasiun konverter HVDC?", "id": "Filter harmonisa AC dan DC." },
  { "en": "Apa itu 'reactive power compensation'?", "id": "Kompensasi daya reaktif." },
  { "en": "Peralatan untuk 'reactive power compensation'?", "id": "Kapasitor, reaktor, STATCOM." },
  { "en": "Apa itu 'voltage regulation'?", "id": "Pengaturan tegangan." },
  { "en": "Peralatan untuk 'voltage regulation'?", "id": "Tap changer trafo, AVR." },
  { "en": "Kemampuan sistem untuk menahan gangguan tanpa kehilangan beban?", "id": "Keamanan sistem (system security)." },
  { "en": "Kemampuan sistem untuk menyuplai beban secara kontinu?", "id": "Kecukupan sistem (system adequacy)." },
  { "en": "Keamanan dan kecukupan adalah dua komponen dari?", "id": "Keandalan sistem (system reliability)." },
  { "en": "Apa itu 'islanding' yang disengaja?", "id": "Intentional islanding (microgrid)." },
  { "en": "Apa itu 'unintentional islanding'?", "id": "Islanding yang tidak diinginkan." },
  { "en": "Kebutuhan proteksi untuk 'unintentional islanding'?", "id": "Anti-islanding protection." },
  { "en": "Relay yang mendeteksi laju perubahan frekuensi?", "id": "Relay 81R (RoCoF)." },
  { "en": "Aplikasi relay 81R?", "id": "Deteksi islanding, load shedding." },
  { "en": "Relay yang mendeteksi pergeseran vektor tegangan?", "id": "Vector shift relay." },
  { "en": "Aplikasi 'vector shift relay'?", "id": "Deteksi islanding untuk DG." },
  { "en": "Apa itu 'probabilistic planning'?", "id": "Perencanaan sistem dengan mempertimbangkan ketidakpastian." },
  { "en": "Contoh ketidakpastian dalam 'probabilistic planning'?", "id": "Variabilitas EBT, pertumbuhan beban." },
  { "en": "Apa itu 'deterministic planning'?", "id": "Perencanaan sistem berdasarkan skenario terburuk." },
  { "en": "Bahan bakar sintetis yang dibuat dari listrik terbarukan?", "id": "E-fuel atau electrofuel." },
  { "en": "Proses pembuatan 'e-fuel'?", "id": "Elektrolisis diikuti sintesis." },
  { "en": "Penggunaan 'e-fuel'?", "id": "Transportasi, industri, penyimpanan energi." },
  { "en": "Apa itu 'demand-side flexibility'?", "id": "Fleksibilitas permintaan dari sisi pelanggan." },
  { "en": "Contoh 'demand-side flexibility'?", "id": "Smart charging EV, pemanas air pintar." },
  { "en": "Apa itu 'non-wires alternative' (NWA)?", "id": "Alternatif selain membangun jaringan baru." },
  { "en": "Contoh NWA (non-wires alternative)?", "id": "BESS, efisiensi energi, demand response." },
  { "en": "Manfaat NWA (non-wires alternative)?", "id": "Menunda investasi, lebih ramah lingkungan." },
  { "en": "Apa itu 'dynamic stability'?", "id": "Stabilitas sistem terhadap gangguan kecil (termasuk kontrol)." },
  { "en": "Apa itu 'transient stability'?", "id": "Stabilitas sistem pasca gangguan besar." },
  { "en": "Batas stabilitas transien ayunan pertama?", "id": "First-swing stability." },
  { "en": "Apa itu 'multi-swing instability'?", "id": "Ketidakstabilan yang terjadi setelah beberapa ayunan." },
  { "en": "Penyebab 'multi-swing instability'?", "id": "Redaman sistem yang negatif." },
  { "en": "Apa itu 'voltage source'?", "id": "Sumber yang tegangannya konstan." },
  { "en": "Apa itu 'current source'?", "id": "Sumber yang arusnya konstan." },
  { "en": "Jaringan distribusi yang membentuk loop tertutup?", "id": "Jaringan meshed atau terhubung." },
  { "en": "Manfaat jaringan distribusi meshed?", "id": "Keandalan tinggi, suplai dari dua arah." },
  { "en": "Mengubah topologi jaringan distribusi secara dinamis?", "id": "Rekonfigurasi jaringan." },
  { "en": "Saluran transmisi yang menghubungkan dua area sistem?", "id": "Saluran dasi (tie-line)." },
  { "en": "Analisis aliran daya untuk frekuensi harmonisa?", "id": "Analisis aliran daya harmonisa." },
  { "en": "Komponen urutan harmonisa yang memutar medan ke depan?", "id": "Urutan positif." },
  { "en": "Komponen urutan harmonisa yang memutar medan ke belakang?", "id": "Urutan negatif." },
  { "en": "Komponen urutan harmonisa pada kelipatan tiga?", "id": "Urutan nol." },
  { "en": "Kondisi impedansi sistem sangat tinggi pada frekuensi harmonisa?", "id": "Resonansi paralel." },
  { "en": "Kondisi impedansi sistem sangat rendah pada frekuensi harmonisa?", "id": "Resonansi seri." },
  { "en": "Kabel yang menggunakan material superkonduktor suhu tinggi?", "id": "Kabel HTS (High-Temperature Superconducting)." },
  { "en": "Keuntungan kabel HTS (High-Temperature Superconducting)?", "id": "Kapasitas sangat tinggi, rugi-rugi rendah." },
  { "en": "Oli trafo sintetis dengan titik nyala tinggi?", "id": "Minyak ester sintetis." },
  { "en": "Oli trafo dari sumber nabati?", "id": "Minyak ester alami." },
  { "en": "Teknologi switchgear bebas gas SF6 (Sulfur Hexafluoride)?", "id": "Menggunakan udara bersih atau campuran gas." },
  { "en": "Sistem kontrol otomatis untuk menyeimbangkan pembangkitan dan beban?", "id": "AGC (Automatic Generation Control)." },
  { "en": "Bagian dari AGC (Automatic Generation Control) yang mengatur frekuensi?", "id": "LFC (Load Frequency Control)." },
  { "en": "Selisih antara aliran daya tie-line aktual dan terjadwal?", "id": "ACE (Area Control Error)." },
  { "en": "Tujuan LFC (Load Frequency Control)?", "id": "Menjaga frekuensi dan pertukaran daya." },
  { "en": "Kemampuan pembangkit untuk mengubah output dayanya?", "id": "Kemampuan ramping (ramping capability)." },
  { "en": "Pengamanan siber untuk IED (Intelligent Electronic Device)?", "id": "Manajemen password, enkripsi, firewall." },
  { "en": "Skema proteksi dengan dua sistem independen?", "id": "Proteksi utama dan cadangan." },
  { "en": "Skema proteksi saluran transmisi menggunakan sinyal blok?", "id": "Directional comparison blocking." },
  { "en": "Kemampuan pembangkit untuk start dari kondisi padam total?", "id": "Kemampuan black start." },
  { "en": "Respon frekuensi sangat cepat dari BESS atau EBT?", "id": "FFR (Fast Frequency Response)." },
  { "en": "Tujuan FFR (Fast Frequency Response)?", "id": "Mencegah RoCoF tinggi di sistem inersia rendah." },
  { "en": "Program simulasi untuk transien elektromagnetik?", "id": "EMTP (Electromagnetic Transient Program)." },
  { "en": "Proses mengurutkan kontingensi berdasarkan tingkat keparahan?", "id": "Peringkingan kontingensi (contingency ranking)." },
  { "en": "Analisis aliran daya yang memperhitungkan ketidakpastian?", "id": "Aliran daya probabilistik." },
  { "en": "Kapasitas maksimum DER (Distributed Energy Resources) yang bisa terhubung ke penyulang?", "id": "Kapasitas penampungan (hosting capacity)." },
  { "en": "Masalah utama penetrasi PV (Photovoltaic) surya tinggi?", "id": "Tegangan lebih di siang hari." },
  { "en": "Aliran daya dari jaringan distribusi kembali ke gardu induk?", "id": "Aliran daya balik (reverse power flow)." },
  { "en": "Kebisingan yang terdengar dari saluran transmisi?", "id": "Audible noise." },
  { "en": "Penyebab utama 'audible noise'?", "id": "Lucutan korona, terutama saat lembab." },
  { "en": "Medan listrik dan magnet di sekitar saluran listrik?", "id": "EMF (Electric and Magnetic Fields)." },
  { "en": "Struktur penampung tumpahan oli di bawah trafo?", "id": "Bak penampung (containment pit)." },
  { "en": "Pengujian peralatan di pabrik sebelum dikirim?", "id": "FAT (Factory Acceptance Test)." },
  { "en": "Pengujian peralatan di lokasi setelah instalasi?", "id": "SAT (Site Acceptance Test)." },
  { "en": "Pengujian faktor daya isolasi (Doble test) mengukur?", "id": "Kualitas dan kontaminasi isolasi." },
  { "en": "Pengujian untuk mengecek koneksi internal belitan trafo?", "id": "Pengukuran tahanan belitan." },
  { "en": "Apa itu 'tap staggering' pada trafo paralel?", "id": "Posisi tap berbeda yang tidak disengaja." },
  { "en": "Dampak 'tap staggering'?", "id": "Menimbulkan arus sirkulasi reaktif." },
  { "en": "Apa itu 'circulating current'?", "id": "Arus yang bersirkulasi antar unit paralel." },
  { "en": "Apa itu 'differential leakage current'?", "id": "Arus diferensial kecil saat kondisi normal." },
  { "en": "Penyebab 'differential leakage current'?", "id": "Ketidakcocokan CT, arus magnetisasi." },
  { "en": "Harmonisa ke-5 termasuk urutan?", "id": "Urutan negatif." },
  { "en": "Dampak harmonisa urutan negatif pada motor?", "id": "Pemanasan berlebih dan torsi negatif." },
  { "en": "Harmonisa ke-7 termasuk urutan?", "id": "Urutan positif." },
  { "en": "Dampak harmonisa urutan positif?", "id": "Tidak separah urutan negatif." },
  { "en": "Apa itu 'point of common coupling' (PCC)?", "id": "Titik di mana beban/pembangkit terhubung ke jaringan." },
  { "en": "Standar kualitas daya IEEE yang umum?", "id": "IEEE 519." },
  { "en": "Fokus dari standar IEEE 519?", "id": "Batasan distorsi harmonisa di PCC." },
  { "en": "Apa itu 'thermal runaway' pada baterai?", "id": "Reaksi berantai pelepasan panas tak terkendali." },
  { "en": "Penyebab 'thermal runaway'?", "id": "Overcharging, hubung singkat internal, suhu tinggi." },
  { "en": "Sistem proteksi terhadap 'thermal runaway'?", "id": "BMS (Battery Management System), pendinginan." },
  { "en": "Apa itu 'second life battery'?", "id": "Baterai mobil listrik (EV) bekas yang digunakan untuk BESS." },
  { "en": "Manfaat 'second life battery'?", "id": "Mengurangi biaya BESS dan limbah baterai." },
  { "en": "Apa itu 'solid-state battery'?", "id": "Baterai yang menggunakan elektrolit padat." },
  { "en": "Keuntungan 'solid-state battery'?", "id": "Lebih aman, kepadatan energi lebih tinggi." },
  { "en": "Tantangan 'grid following' inverter?", "id": "Tidak bisa beroperasi saat grid padam." },
  { "en": "Apa itu 'weak grid' condition?", "id": "Jaringan dengan impedansi tinggi/inersia rendah." },
  { "en": "Masalah IBR (Inverter-Based Resources) pada 'weak grid'?", "id": "Ketidakstabilan kontrol." },
  { "en": "Apa itu 'short circuit ratio' (SCR)?", "id": "Ukuran kekuatan jaringan di titik interkoneksi." },
  { "en": "Nilai SCR (Short Circuit Ratio) rendah menandakan?", "id": "Jaringan lemah (weak grid)." },
  { "en": "Apa itu 'islanding' yang tidak terdeteksi?", "id": "Unintentional islanding." },
  { "en": "Apa itu 'frequency droop' control?", "id": "Kontrol di mana daya output diatur oleh frekuensi." },
  { "en": "Fungsi 'frequency droop'?", "id": "Membagi beban aktif antar generator." },
  { "en": "Apa itu 'voltage droop' control?", "id": "Kontrol di mana daya reaktif diatur oleh tegangan." },
  { "en": "Fungsi 'voltage droop'?", "id": "Membagi beban reaktif antar generator." },
  { "en": "Apa itu 'isochronous control'?", "id": "Mode kontrol frekuensi untuk menjaga frekuensi tetap." },
  { "en": "Kapan 'isochronous control' digunakan?", "id": "Pada sistem terisolasi (island mode)." },
  { "en": "Apa itu 'black start path'?", "id": "Jalur energi dari unit black start ke beban." },
  { "en": "Apa itu 'system restoration plan'?", "id": "Rencana pemulihan sistem dari padam total." },
  { "en": "Apa itu 'energy arbitrage'?", "id": "Membeli listrik saat murah, menjual saat mahal." },
  { "en": "Aplikasi 'energy arbitrage'?", "id": "Menggunakan BESS untuk keuntungan finansial." },
  { "en": "Apa itu 'frequency containment reserve' (FCR)?", "id": "Cadangan untuk menahan deviasi frekuensi awal." },
  { "en": "Respon waktu FCR (Frequency Containment Reserve)?", "id": "Sangat cepat (dalam hitungan detik)." },
  { "en": "Apa itu 'frequency restoration reserve' (FRR)?", "id": "Cadangan untuk mengembalikan frekuensi ke nominal." },
  { "en": "Respon waktu FRR (Frequency Restoration Reserve)?", "id": "Beberapa menit." },
  { "en": "Apa itu 'replacement reserve' (RR)?", "id": "Cadangan untuk menggantikan cadangan lain yang terpakai." },
  { "en": "Studi untuk menentukan kelayakan ekonomi suatu proyek?", "id": "Studi kelayakan (feasibility study)." },
  { "en": "Apa itu 'levelized cost of energy' (LCOE)?", "id": "Biaya energi rata-rata seumur hidup pembangkit." },
  { "en": "Fungsi LCOE (Levelized Cost of Energy)?", "id": "Membandingkan keekonomian berbagai teknologi pembangkit." },
  { "en": "Apa itu 'locational marginal pricing' (LMP)?", "id": "Harga listrik di lokasi spesifik jaringan." },
  { "en": "LMP (Locational Marginal Pricing) dipengaruhi oleh?", "id": "Biaya pembangkitan, kongesti, dan rugi-rugi." },
  { "en": "Apa itu 'financial transmission right' (FTR)?", "id": "Instrumen keuangan untuk hedging kongesti." },
  { "en": "Peralatan untuk menyerap harmonisa dan mengkompensasi daya reaktif?", "id": "Filter aktif (Active Power Filter)." },
  { "en": "Peralatan untuk meningkatkan kualitas tegangan secara dinamis?", "id": "DVR (Dynamic Voltage Restorer)." },
  { "en": "Prinsip kerja DVR (Dynamic Voltage Restorer)?", "id": "Menyuntikkan tegangan seri untuk memperbaiki sag/swell." },
  { "en": "Apa itu 'unified power quality conditioner' (UPQC)?", "id": "Gabungan fitur STATCOM dan DVR." },
  { "en": "Fungsi UPQC (Unified Power Quality Conditioner)?", "id": "Mengatasi masalah kualitas daya komprehensif." },
  { "en": "Material isolasi gas alternatif selain SF6?", "id": "Campuran gas Novec 4710 atau CO2." },
  { "en": "Potensi pemanasan global (GWP) gas SF6?", "id": "Sangat tinggi, ribuan kali CO2." },
  { "en": "Apa itu 'lean management' dalam operasi utilitas?", "id": "Fokus pada efisiensi dan eliminasi pemborosan." },
  { "en": "Apa itu 'agile methodology' dalam pengembangan proyek energi?", "id": "Pendekatan iteratif dan fleksibel." },
  { "en": "Pemanfaatan 'big data' di perusahaan listrik?", "id": "Optimalisasi operasi, pemeliharaan prediktif, analisis pelanggan." },
  { "en": "Penggunaan komputasi kuantum di sistem tenaga?", "id": "Optimalisasi sistem tenaga skala besar." },
  { "en": "Sistem kontrol grid yang beroperasi tanpa intervensi manusia?", "id": "Kontrol grid otonom berbasis AI." },
  { "en": "Konsep jaringan energi terdesentralisasi seperti internet?", "id": "Internet Energi (Internet of Energy)." },
  { "en": "Pembangkit listrik tenaga surya di orbit luar angkasa?", "id": "Pembangkit surya berbasis antariksa." },
  { "en": "Potensi superkonduktor pada suhu ruang?", "id": "Transmisi listrik tanpa rugi sama sekali." },
  { "en": "Aplikasi grafena (graphene) dalam penyimpanan energi?", "id": "Elektroda superkapasitor dan baterai." },
  { "en": "Serangan terkoordinasi pada komponen fisik dan siber?", "id": "Serangan siber-fisik." },
  { "en": "Penggunaan blockchain untuk keamanan SCADA?", "id": "Mencatat perintah kontrol secara aman." },
  { "en": "Strategi keamanan siber yang terus mengubah konfigurasi sistem?", "id": "MTD (Moving Target Defense)." },
  { "en": "Sistem penyimpanan energi baterai yang bisa dipindahkan?", "id": "Penyimpanan energi bergerak (mobile)." },
  { "en": "Perlindungan infrastruktur kritis dari pulsa elektromagnetik ketinggian?", "id": "Proteksi HEMP (High-Altitude Electromagnetic Pulse)." },
  { "en": "Cara melindungi dari HEMP (High-Altitude Electromagnetic Pulse)?", "id": "Faraday cage, filter, grounding." },
  { "en": "Proses pemurnian kembali oli trafo bekas?", "id": "Regenerasi oli transformator." },
  { "en": "Tantangan daur ulang sudu turbin angin?", "id": "Material komposit yang sulit dipisah." },
  { "en": "Analisis dampak lingkungan produk dari awal hingga akhir?", "id": "LCA (Life Cycle Assessment)." },
  { "en": "Keterkaitan antara sistem air dan sistem energi?", "id": "Nexus air-energi (water-energy nexus)." },
  { "en": "Dampak pusat data (data center) skala besar pada grid?", "id": "Beban besar, konstan, butuh keandalan tinggi." },
  { "en": "Metode kontrol sistem tenaga yang memperhitungkan non-linearitas?", "id": "Kontrol non-linear." },
  { "en": "Sistem peredam osilasi area luas menggunakan data PMU?", "id": "WADC (Wide-Area Damping Control)." },
  { "en": "Tantangan stabilitas pada sistem 100% berbasis inverter?", "id": "Kurangnya inersia dan arus gangguan." },
  { "en": "Material semikonduktor modern untuk elektronika daya?", "id": "SiC (Silicon Carbide) dan GaN (Gallium Nitride)." },
  { "en": "Keuntungan SiC (Silicon Carbide) dan GaN (Gallium Nitride)?", "id": "Efisiensi tinggi, frekuensi switching tinggi." },
  { "en": "Pemutus tenaga yang menggunakan semikonduktor daya?", "id": "SSCB (Solid-State Circuit Breaker)." },
  { "en": "Keuntungan SSCB (Solid-State Circuit Breaker)?", "id": "Pemutusan sangat cepat, tanpa busur api." },
  { "en": "Standar model informasi umum untuk pertukaran data utilitas?", "id": "CIM (Common Information Model)." },
  { "en": "Tujuan CIM (Common Information Model)?", "id": "Interoperabilitas antar sistem perangkat lunak." },
  { "en": "Transformator instrumen yang menggunakan prinsip optik?", "id": "Transformator instrumen optik." },
  { "en": "Simulator untuk melatih operator gardu induk/pusat kontrol?", "id": "Simulator pelatihan operator." },
  { "en": "Ilmu desain antarmuka operator untuk mengurangi kesalahan?", "id": "Rekayasa faktor manusia (human factors)." },
  { "en": "Apa itu 'grid-forming stability'?", "id": "Stabilitas yang disediakan oleh inverter grid-forming." },
  { "en": "Apa itu 'grid-connected stability'?", "id": "Stabilitas inverter yang bergantung pada grid." },
  { "en": "Apa itu 'converter-driven stability'?", "id": "Stabilitas yang ditentukan oleh interaksi konverter." },
  { "en": "Apa itu 'dynamic security assessment' (DSA)?", "id": "Penilaian keamanan sistem secara dinamis." },
  { "en": "Tujuan DSA (dynamic security assessment)?", "id": "Memprediksi dan mencegah ketidakstabilan sistem." },
  { "en": "Apa itu 'probabilistic reliability assessment'?", "id": "Penilaian keandalan berbasis probabilitas." },
  { "en": "Apa itu 'adequacy' dalam keandalan?", "id": "Kecukupan pembangkitan untuk memenuhi beban." },
  { "en": "Apa itu 'security' dalam keandalan?", "id": "Kemampuan sistem menahan gangguan mendadak." },
  { "en": "Proses penuaan isolator polimer akibat cuaca?", "id": "Weathering." },
  { "en": "Apa itu 'equivalent salt deposit density' (ESDD)?", "id": "Ukuran tingkat polusi pada isolator." },
  { "en": "Pengujian isolator di laboratorium dengan polusi buatan?", "id": "Clean fog test." },
  { "en": "Pelapisan hidrofobik pada isolator?", "id": "Pelapisan RTV (Room Temperature Vulcanizing) silikon." },
  { "en": "Apa itu 'grading ring' pada isolator?", "id": "Cincin untuk meratakan distribusi tegangan." },
  { "en": "Fungsi lain 'grading ring'?", "id": "Mengurangi korona pada ujung isolator." },
  { "en": "Apa itu 'transient stability margin'?", "id": "Batas keamanan stabilitas transien." },
  { "en": "Peralatan untuk memonitor 'transient stability margin' real-time?", "id": "PMU (Phasor Measurement Unit) dan perangkat lunak." },
  { "en": "Apa itu 'voltage stability margin'?", "id": "Batas keamanan sebelum keruntuhan tegangan." },
  { "en": "Apa itu 'loadability limit'?", "id": "Batas pembebanan maksimum suatu saluran." },
  { "en": "Batas pembebanan yang ditentukan oleh panas?", "id": "Batas termal." },
  { "en": "Batas pembebanan yang ditentukan oleh stabilitas?", "id": "Batas stabilitas." },
  { "en": "Pembangkit yang memanfaatkan gradien salinitas?", "id": "Pembangkit energi osmotik." },
  { "en": "Apa itu 'blue energy'?", "id": "Energi dari pertemuan air tawar dan asin." },
  { "en": "Apa itu 'molten salt reactor' (MSR)?", "id": "Reaktor nuklir yang menggunakan garam cair." },
  { "en": "Keuntungan MSR (molten salt reactor)?", "id": "Keamanan pasif yang lebih baik." },
  { "en": "Apa itu 'small modular reactor' (SMR)?", "id": "Reaktor nuklir kecil dan modular." },
  { "en": "Keuntungan SMR (small modular reactor)?", "id": "Fabrikasi di pabrik, lebih fleksibel." },
  { "en": "Jaringan listrik DC (Direct Current) untuk distribusi lokal?", "id": "Jaringan distribusi DC." },
  { "en": "Keuntungan jaringan distribusi DC?", "id": "Integrasi EBT dan BESS lebih mudah." },
  { "en": "Apa itu 'universal access to energy'?", "id": "Akses universal terhadap energi." },
  { "en": "Tujuan Pembangunan Berkelanjutan (SDG) terkait energi?", "id": "SDG 7: Energi bersih dan terjangkau." },
  { "en": "Apa itu 'just energy transition'?", "id": "Transisi energi yang adil bagi masyarakat." },
  { "en": "Konsep 'prosumer' dalam sistem tenaga?", "id": "Konsumen yang juga memproduksi listrik." },
  { "en": "Platform digital untuk mengelola 'prosumer'?", "id": "Platform agregator atau VPP." },
  { "en": "Apa itu 'behind-the-meter' (BTM) asset?", "id": "Aset energi di sisi pelanggan." },
  { "en": "Contoh BTM (behind-the-meter) asset?", "id": "PLTS atap, baterai rumah." },
  { "en": "Apa itu 'front-of-the-meter' (FTM) asset?", "id": "Aset energi di sisi utilitas." },
  { "en": "Contoh FTM (front-of-the-meter) asset?", "id": "Pembangkit skala besar, BESS grid-scale." },
  { "en": "Analisis yang menggabungkan perencanaan transmisi dan distribusi?", "id": "Integrated resource planning (IRP)." },
  { "en": "Pemanfaatan 'machine learning' untuk prediksi cuaca EBT?", "id": "Prediksi radiasi surya dan angin." },
  { "en": "Pemanfaatan 'computer vision' untuk inspeksi aset?", "id": "Deteksi kerusakan dari gambar drone." },
  { "en": "Apa itu 'augmented reality' (AR) untuk teknisi lapangan?", "id": "Menampilkan data dan panduan virtual." },
  { "en": "Apa itu 'digital substation testing'?", "id": "Pengujian gardu digital menggunakan simulasi." },
  { "en": "Apa itu 'traveling wave relay'?", "id": "Relay proteksi berbasis gelombang berjalan." },
  { "en": "Keuntungan 'traveling wave relay'?", "id": "Lokasi gangguan sangat akurat dan cepat." },
  { "en": "Apa itu 'harmonic resonance'?", "id": "Resonansi yang terjadi pada frekuensi harmonisa." },
  { "en": "Bahaya 'harmonic resonance'?", "id": "Tegangan/arus lebih, kerusakan peralatan." },
  { "en": "Studi untuk mengidentifikasi risiko 'harmonic resonance'?", "id": "Studi harmonisa (harmonic analysis)." },
  { "en": "Perangkat lunak untuk studi harmonisa?", "id": "ETAP, DigSilent, PSS/E." },
  { "en": "Apa itu 'power system restoration'?", "id": "Proses pemulihan sistem tenaga." },
  { "en": "Urutan prioritas pemulihan beban?", "id": "Beban kritis, pembangkit lain, beban industri." },
  { "en": "Apa itu 'islanding capability'?", "id": "Kemampuan suatu area untuk beroperasi mandiri." },
  { "en": "Fungsi 'islanding capability' saat pemulihan?", "id": "Membentuk pulau-pulau stabil sebelum sinkronisasi." },
  { "en": "Apa itu 'resynchronization'?", "id": "Sinkronisasi kembali antar pulau sistem." },
  { "en": "Tantangan 'resynchronization'?", "id": "Perbedaan frekuensi dan sudut fasa besar." },
  { "en": "Apa itu 'wide-area situational awareness' (WASA)?", "id": "Kesadaran situasional area luas." },
  { "en": "Teknologi kunci untuk WASA (wide-area situational awareness)?", "id": "PMU, WAMS, SCADA, GIS." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sirkuit untuk sinkronisasi dengan sinyal AC." },
  { "en": "Peran PLL (phase-locked loop) pada IBR?", "id": "Sinkronisasi inverter dengan grid." },
  { "en": "Ketidakstabilan yang disebabkan oleh interaksi PLL?", "id": "PLL-induced instability." },
  { "en": "Apa itu 'system strength'?", "id": "Ukuran kekuatan jaringan di suatu titik." },
  { "en": "Indikator 'system strength'?", "id": "Short Circuit Ratio (SCR)." },
  { "en": "Solusi untuk 'low system strength'?", "id": "Kondensor sinkron, grid-forming inverter." },
  { "en": "Apa itu 'grid of the future'?", "id": "Jaringan listrik masa depan." },
  { "en": "Karakteristik 'grid of the future'?", "id": "Dekarbonisasi, terdesentralisasi, digitalisasi." },
  { "en": "Apa itu 'energy transition'?", "id": "Transisi dari sistem energi fosil ke terbarukan." },
  { "en": "Tantangan terbesar 'energy transition'?", "id": "Menjaga keandalan dan keterjangkauan." },
  { "en": "Peran sistem tenaga arus kuat di masa depan?", "id": "Tulang punggung masyarakat modern yang berkelanjutan." }



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
