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


  { "en": "Apa Fungsi Utama Kabel?", "id": "Menghantarkan Arus Listrik Dengan Aman." },
  { "en": "Bagian Utama Kabel Listrik?", "id": "Konduktor Isolator Dan Pelindung Luar." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Mudah Menghantarkan Arus Listrik." },
  { "en": "Bahan Konduktor Paling Umum?", "id": "Tembaga Dan Aluminium." },
  { "en": "Kenapa Tembaga Sering Digunakan?", "id": "Konduktivitas Sangat Baik Dan Tahan Korosi." },
  { "en": "Kenapa Aluminium Digunakan?", "id": "Lebih Ringan Dan Lebih Murah Dari Tembaga." },
  { "en": "Kelemahan Aluminium Apa?", "id": "Konduktivitas Lebih Rendah Dan Mudah Oksidasi." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Yang Sulit Menghantarkan Arus Listrik." },
  { "en": "Fungsi Isolator Pada Kabel?", "id": "Mencegah Kebocoran Arus Dan Hubungan Singkat." },
  { "en": "Bahan Isolator Paling Umum?", "id": "Plastik Seperti PVC Dan Karet." },
  { "en": "Apa Kepanjangan PVC?", "id": "Polyvinyl Chloride." },
  { "en": "Apa Fungsi Pelindung Luar?", "id": "Melindungi Kabel Dari Kerusakan Fisik." },
  { "en": "Pelindung Luar Disebut Juga?", "id": "Selubung Luar Atau Jacket Kabel." },
  { "en": "Bahan Pelindung Luar?", "id": "Biasanya Terbuat Dari Bahan PVC." },
  { "en": "Apa Itu Kabel Serabut?", "id": "Konduktor Terdiri Dari Banyak Kawat Halus." },
  { "en": "Keuntungan Kabel Serabut?", "id": "Sangat Fleksibel Dan Mudah Dibengkokkan." },
  { "en": "Apa Itu Kabel Pejal?", "id": "Konduktor Terdiri Dari Satu Kawat Tunggal." },
  { "en": "Keuntungan Kabel Pejal?", "id": "Lebih Kuat Dan Tahan Getaran." },
  { "en": "Di Mana Kabel Serabut Digunakan?", "id": "Peralatan Portabel Dan Elektronik." },
  { "en": "Di Mana Kabel Pejal Digunakan?", "id": "Instalasi Listrik Tetap Di Dinding." },
  { "en": "Apa Satuan Ukuran Konduktor?", "id": "Luas Penampang Dalam Milimeter Persegi (mm²)." },
  { "en": "Ukuran Konduktor Mempengaruhi Apa?", "id": "Kemampuan Hantar Arus (KHA) Kabel." },
  { "en": "Konduktor Lebih Besar KHA Nya?", "id": "Semakin Besar Dan Tahanan Semakin Kecil." },
  { "en": "Apa Itu KHA?", "id": "Kemampuan Hantar Arus Maksimum Kabel." },
  { "en": "Apa Satuan KHA?", "id": "Ampere (A)." },
  { "en": "Apa Yang Terjadi Jika KHA Terlampaui?", "id": "Kabel Menjadi Panas Dan Bisa Terbakar." },
  { "en": "Apa Itu Kode Warna Kabel?", "id": "Standar Warna Isolator Untuk Identifikasi." },
  { "en": "Fungsi Kode Warna?", "id": "Membedakan Kabel Fasa Netral Dan Ground." },
  { "en": "Warna Kabel Fasa Di Indonesia?", "id": "Hitam Coklat Atau Abu-abu." },
  { "en": "Warna Kabel Netral Di Indonesia?", "id": "Biru." },
  { "en": "Warna Kabel Ground Di Indonesia?", "id": "Hijau-Kuning." },
  { "en": "Apa Itu Kabel Fasa?", "id": "Kabel Yang Membawa Tegangan Listrik." },
  { "en": "Apa Itu Kabel Netral?", "id": "Kabel Kembali Untuk Arus Listrik." },
  { "en": "Apa Itu Kabel Ground?", "id": "Kabel Pengaman Yang Terhubung Ke Tanah." },
  { "en": "Fungsi Kabel Ground?", "id": "Melindungi Dari Sengatan Listrik Akibat Kebocoran." },
  { "en": "Apa Itu Tegangan Tembus Isolator?", "id": "Batas Tegangan Sebelum Isolator Rusak." },
  { "en": "Isolator Yang Baik Punya Tegangan Tembus?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Kabel NYA?", "id": "Kabel Pejal Berisolasi PVC Satu Lapis." },
  { "en": "Penggunaan Kabel NYA?", "id": "Instalasi Dalam Pipa Pelindung." },
  { "en": "Kenapa Harus Pakai Pipa?", "id": "Karena Isolasinya Hanya Satu Lapis." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Dengan Beberapa Inti Dan Selubung PVC." },
  { 'en': 'Berapa Inti Kabel NYM?', 'id': 'Dua Tiga Atau Empat Inti.' },
  { "en": "Penggunaan Kabel NYM?", "id": "Instalasi Dalam Ruangan Tanpa Pipa." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Dengan Isolasi Tahan Cuaca." },
  { "en": "Penggunaan Kabel NYY?", "id": "Instalasi Luar Ruangan Atau Bawah Tanah." },
  { "en": "Apa Itu Kabel Coaxial?", "id": "Kabel Untuk Sinyal Frekuensi Tinggi." },
  { "en": "Contoh Penggunaan Kabel Coaxial?", "id": "Antena Televisi Dan Jaringan Komputer." },
  { "en": "Struktur Kabel Coaxial?", "id": "Konduktor Pusat Dielektrik Dan Pelindung Logam." },
  { "en": "Fungsi Pelindung Logam (Shield)?", "id": "Melindungi Sinyal Dari Interferensi Elektromagnetik." },
  { "en": "Apa Itu Kabel UTP?", "id": "Unshielded Twisted Pair." },
  { "en": "Penggunaan Kabel UTP?", "id": "Jaringan Ethernet Komputer Dan Telepon." },
  { "en": "Kenapa Kawatnya Dipilin (Twisted)?", "id": "Untuk Mengurangi Crosstalk Dan Interferensi." },
  { "en": "Apa Itu Kabel STP?", "id": "Shielded Twisted Pair." },
  { "en": "Bedanya Dengan UTP?", "id": "STP Memiliki Lapisan Pelindung Tambahan." },
  { "en": "Apa Itu Kabel Fleksibel?", "id": "Kabel Yang Didesain Untuk Sering Bergerak." },
  { "en": "Contoh Kabel Fleksibel?", "id": "Kabel Setrika Atau Kabel Earphone." },
  { "en": "Apa Itu Kabel Optik?", "id": "Kabel Yang Menghantarkan Cahaya." },
  { "en": "Inti Kabel Optik Terbuat Dari?", "id": "Serat Kaca Atau Plastik Sangat Tipis." },
  { "en": "Keuntungan Kabel Optik?", "id": "Kapasitas Data Sangat Besar Kebal Interferensi." },
  { "en": "Apa Itu Skin Effect?", "id": "Kecenderungan Arus AC Mengalir Di Permukaan." },
  { "en": "Skin Effect Terjadi Pada Frekuensi?", "id": "Frekuensi Tinggi." },
  { "en": "Apa Itu Resistansi Kabel?", "id": "Hambatan Aliran Arus Dalam Konduktor." },
  { "en": "Satuan Resistansi Kabel?", "id": "Ohm (Ω)." },
  { "en": "Resistansi Menyebabkan Apa?", "id": "Kehilangan Daya Dalam Bentuk Panas." },
  { "en": "Kabel Lebih Panjang Resistansinya?", "id": "Semakin Besar." },
  { "en": "Kabel Lebih Tebal Resistansinya?", "id": "Semakin Kecil." },
  { "en": "Apa Itu Jatuh Tegangan (Voltage Drop)?", "id": "Penurunan Tegangan Di Sepanjang Kabel." },
  { "en": "Penyebab Jatuh Tegangan?", "id": "Resistansi Internal Dari Kabel." },
  { "en": "Jatuh Tegangan Yang Baik?", "id": "Harus Dijaga Sekecil Mungkin." },
  { "en": "Cara Mengurangi Jatuh Tegangan?", "id": "Menggunakan Kabel Dengan Ukuran Lebih Besar." },
  { "en": "Apa Itu Terminal Kabel?", "id": "Komponen Untuk Menyambung Kabel Ke Alat." },
  { "en": "Contoh Terminal Kabel?", "id": "Skun Kabel Atau Cable Lug." },
  { "en": "Fungsi Terminal Kabel?", "id": "Memastikan Koneksi Yang Kuat Dan Aman." },
  { "en": "Apa Itu Penyambungan Kabel?", "id": "Menghubungkan Dua Ujung Kabel." },
  { "en": "Penyambungan Harus Dilakukan?", "id": "Dengan Benar Untuk Hindari Masalah." },
  { "en": "Apa Itu Kotak Sambung (Junction Box)?", "id": "Kotak Pelindung Untuk Sambungan Kabel." },
  { "en": "Apa Itu Kabel Tray?", "id": "Struktur Penopang Untuk Jalur Kabel." },
  { "en": "Apa Itu Kabel Ladder?", "id": "Jenis Kabel Tray Berbentuk Seperti Tangga." },
  { "en": "Fungsi Kabel Tray?", "id": "Merutekan Dan Melindungi Kabel Secara Teratur." },
  { "en": "Apa Itu Kabel Tahan Api?", "id": "Kabel Yang Tetap Berfungsi Saat Terbakar." },
  { "en": "Di Mana Kabel Tahan Api Digunakan?", "id": "Sistem Darurat Seperti Alarm Kebakaran." },
  { "en": "Apa Itu Kabel Bawah Laut?", "id": "Kabel Untuk Komunikasi Antar Pulau." },
  { "en": "Apa Itu Kabel Udara?", "id": "Kabel Yang Digantung Di Tiang Listrik." },
  { "en": "Apa Itu Standar Kabel Di Indonesia?", "id": "Standar Nasional Indonesia (SNI)." },
  { "en": "Kenapa Penting Menggunakan Kabel SNI?", "id": "Untuk Menjamin Keamanan Dan Kualitas Kabel." },
  { "en": "Apa Itu Pemanasan Kabel?", "id": "Kenaikan Suhu Akibat Aliran Arus." },
  { "en": "Pemanasan Berlebih Bisa Merusak?", "id": "Isolasi Kabel Dan Menyebabkan Kebakaran." },
  { "en": "Faktor Yang Mempengaruhi Pemanasan?", "id": "Besar Arus Resistansi Dan Suhu Sekitar." },
  { "en": "Apa Itu Konduktivitas Termal?", "id": "Kemampuan Bahan Menghantarkan Panas." },
  { "en": "Apa Itu Induktansi Kabel?", "id": "Sifat Kabel Menghasilkan Medan Magnet." },
  { "en": "Apa Itu Kapasitansi Kabel?", "id": "Sifat Kabel Menyimpan Energi Listrik." },
  { "en": "Induktansi Dan Kapasitansi Penting Pada?", "id": "Sinyal Frekuensi Tinggi Dan Saluran Transmisi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Rasio Tegangan Dan Arus Gelombang." },
  { "en": "Impedansi Penting Untuk Kabel?", "id": "Kabel Sinyal Seperti Coaxial Dan UTP." },
  { "en": "Apa Itu Crosstalk?", "id": "Gangguan Sinyal Antar Pasang Kawat Berdekatan." },
  { "en": "Apa Itu Kabel Inti Tunggal?", "id": "Kabel Yang Hanya Memiliki Satu Konduktor." },
  { "en": "Apa Itu Kabel Multi-Inti?", "id": "Kabel Dengan Dua Atau Lebih Konduktor." },
  { "en": "Isolator Setiap Inti Punya Warna?", "id": "Ya Untuk Identifikasi Masing-Masing Inti." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Pasangan Kawat Yang Dipilin Bersama." },
  { "en": "Tujuan Pilinan Kawat?", "id": "Mengurangi Interferensi Elektromagnetik (EMI)." },
  { "en": "Apa Itu EMI?", "id": "Gangguan Dari Medan Elektromagnetik Eksternal." },
  { "en": "Apa Itu Kabel Data?", "id": "Kabel Yang Dirancang Untuk Transmisi Data." },
  { "en": "Contoh Kabel Data?", "id": "Kabel UTP USB Dan HDMI." },
  { "en": "Apa Itu Kabel Listrik (Power Cable)?", "id": "Kabel Untuk Distribusi Tenaga Listrik." },
  { "en": "Tegangan Kabel Listrik Biasanya?", "id": "Lebih Tinggi Daripada Kabel Data." },
  { "en": "Apa Itu Kabel Tegangan Rendah?", "id": "Kabel Untuk Tegangan Di Bawah 1000 Volt." },
  { "en": "Apa Itu Kabel Tegangan Menengah?", "id": "Kabel Untuk Tegangan Antara 1kV Dan 35kV." },
  { "en": "Apa Itu Kabel Tegangan Tinggi?", "id": "Kabel Untuk Tegangan Di Atas 35kV." },
  { "en": "Semakin Tinggi Tegangan Isolasinya?", "id": "Harus Semakin Tebal Dan Kuat." },
  { "en": "Apa Itu AWG?", "id": "American Wire Gauge." },
  { "en": "AWG Adalah Standar Ukuran?", "id": "Ukuran Diameter Kawat." },
  { "en": "Angka AWG Lebih Kecil Berarti?", "id": "Diameter Kawat Lebih Besar." },
  { "en": "Angka AWG Lebih Besar Berarti?", "id": "Diameter Kawat Lebih Kecil." },
  { "en": "Apa Itu Kabel Shielded?", "id": "Kabel Dengan Lapisan Pelindung Elektromagnetik." },
  { "en": "Lapisan Shield Terbuat Dari?", "id": "Aluminium Foil Atau Jalinan Tembaga." },
  { "en": "Fungsi Shield?", "id": "Melindungi Sinyal Dari Gangguan Noise." },
  { "en": "Kapan Kabel Shielded Digunakan?", "id": "Di Lingkungan Dengan Banyak Gangguan Listrik." },
  { "en": "Apa Itu Kabel Unshielded?", "id": "Kabel Tanpa Lapisan Pelindung Elektromagnetik." },
  { "en": "Contoh Kabel Unshielded?", "id": "Kabel UTP Untuk Jaringan Ethernet." },
  { "en": "Apa Itu Dielektrik?", "id": "Bahan Isolator Di Antara Konduktor." },
  { "en": "Fungsi Dielektrik Di Kabel Coaxial?", "id": "Menjaga Jarak Konstan Antar Konduktor." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Resistansi Efektif Kabel Untuk Sinyal AC." },
  { "en": "Impedansi Kabel Coaxial TV?", "id": "Biasanya 75 Ohm." },
  { "en": "Impedansi Kabel UTP Ethernet?", "id": "Biasanya 100 Ohm." },
  { "en": "Pentingnya Mencocokkan Impedansi?", "id": "Untuk Mencegah Pantulan Sinyal." },
  { "en": "Pantulan Sinyal Menyebabkan?", "id": "Kehilangan Sinyal Dan Error Data." },
  { "en": "Apa Itu Connector?", "id": "Komponen Di Ujung Kabel Untuk Sambungan." },
  { "en": "Contoh Connector?", "id": "Konektor RJ45 USB Atau BNC." },
  { "en": "Apa Itu Kabel Speaker?", "id": "Kabel Untuk Menghubungkan Amplifier Ke Speaker." },
  { "en": "Fitur Kabel Speaker?", "id": "Biasanya Terdiri Dari Dua Konduktor Serabut." },
  { "en": "Apa Itu Kabel Optik?", "id": "Menghantarkan Sinyal Dalam Bentuk Cahaya." },
  { "en": "Apa Itu Core Kabel Optik?", "id": "Bagian Tengah Tempat Cahaya Merambat." },
  { "en": "Apa Itu Cladding Kabel Optik?", "id": "Lapisan Yang Memantulkan Cahaya Kembali Ke Core." },
  { "en": "Prinsip Kerja Kabel Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Kabel Single-Mode?", "id": "Kabel Optik Dengan Core Sangat Kecil." },
  { "en": "Keuntungan Kabel Single-Mode?", "id": "Untuk Jarak Sangat Jauh Dan Bandwidth Tinggi." },
  { "en": "Apa Itu Kabel Multi-Mode?", "id": "Kabel Optik Dengan Core Lebih Besar." },
  { "en": "Keuntungan Kabel Multi-Mode?", "id": "Lebih Murah Dan Mudah Disambungkan." },
  { "en": "Kekurangan Kabel Multi-Mode?", "id": "Hanya Untuk Jarak Pendek." },
  { "en": "Apa Itu Attenuation (Atenuasi)?", "id": "Pelemahan Kekuatan Sinyal Sepanjang Kabel." },
  { "en": "Satuan Atenuasi?", "id": "Desibel Per Meter (dB/m)." },
  { "en": "Atenuasi Lebih Rendah Berarti?", "id": "Sinyal Bisa Merambat Lebih Jauh." },
  { "en": "Frekuensi Lebih Tinggi Atenuasinya?", "id": "Biasanya Semakin Besar." },
  { "en": "Apa Itu Kabel Plenum?", "id": "Kabel Dengan Jaket Tahan Api." },
  { "en": "Di Mana Kabel Plenum Digunakan?", "id": "Di Ruang Plenum (Ventilasi Udara)." },
  { "en": "Tujuannya Untuk?", "id": "Mencegah Penyebaran Api Dan Asap Beracun." },
  { "en": "Apa Itu Kabel Ribbon?", "id": "Kabel Dengan Banyak Konduktor Berjejer Datar." },
  { "en": "Penggunaan Kabel Ribbon?", "id": "Koneksi Internal Di Komputer Dan Elektronik." },
  { "en": "Apa Itu Kabel Power Over Ethernet (PoE)?", "id": "Menyalurkan Data Dan Listrik Sekaligus." },
  { "en": "PoE Menggunakan Kabel Apa?", "id": "Kabel Jaringan UTP." },
  { "en": "Apa Itu Kapasitansi Kabel?", "id": "Kemampuan Kabel Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktansi Kabel?", "id": "Kemampuan Kabel Menghasilkan Medan Magnet." },
  { "en": "Keduanya Mempengaruhi Sinyal?", "id": "Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Kode Nomenklatur Kabel?", "id": "Sistem Penamaan Standar Untuk Jenis Kabel." },
  { "en": "Contoh Nomenklatur?", "id": "NYA NYM NYY." },
  { "en": 'Huruf "N" Berarti?', 'id': 'Kabel Standar Dengan Konduktor Tembaga.' },
  { "en": 'Huruf "Y" Berarti?', 'id': 'Isolasi Terbuat Dari Bahan PVC.' },
  { "en": 'Huruf "A" Berarti?', 'id': 'Kabel Tunggal Atau Kawat.' },
  { "en": 'Huruf "M" Berarti?', 'id': 'Memiliki Banyak Inti (Multi-core).' },
  { "en": "Apa Itu Pengupas Kabel (Stripper)?", "id": "Alat Untuk Melepas Isolasi Dari Konduktor." },
  { "en": "Apa Itu Crimping Tool?", "id": "Alat Untuk Memasang Konektor Ke Kabel." },
  { "en": "Apa Itu Heatshrink Tube?", "id": "Tabung Plastik Yang Mengerut Saat Dipanaskan." },
  { "en": "Fungsi Heatshrink Tube?", "id": "Melindungi Dan Mengisolasi Sambungan Kabel." },
  { "en": "Apa Itu Kabel Tanah (Ground Wire)?", "id": "Sama Dengan Kabel Ground Atau Pembumian." },
  { "en": "Kenapa Kabel Ground Penting?", "id": "Untuk Keselamatan Manusia Dari Sengatan Listrik." },
  { "en": "Apa Itu Short Circuit (Hubungan Singkat)?", "id": "Arus Mengalir Langsung Antara Fasa Netral." },
  { "en": "Penyebab Hubungan Singkat?", "id": "Isolasi Kabel Rusak Atau Gagal." },
  { "en": "Apa Itu Open Circuit (Rangkaian Terbuka)?", "id": "Kabel Putus Sehingga Arus Tidak Mengalir." },
  { "en": "Apa Itu Radius Tikungan (Bend Radius)?", "id": "Radius Minimum Kabel Boleh Dibengkokkan." },
  { "en": "Melanggar Radius Tikungan Menyebabkan?", "id": "Kerusakan Internal Pada Kabel." },
  { "en": "Apa Itu Strain Relief?", "id": "Komponen Untuk Mencegah Kabel Tertarik." },
  { "en": "Di Mana Strain Relief Ditemukan?", "id": "Di Dekat Konektor Atau Titik Masuk." },
  { "en": "Apa Itu Konduktor Terdampar?", "id": "Nama Lain Untuk Konduktor Serabut." },
  { "en": "Apa Itu Konduktor Padat?", "id": "Nama Lain Untuk Konduktor Pejal." },
  { "en": "Apa Itu Kabel Armored?", "id": "Kabel Dengan Pelindung Mekanis Logam." },
  { "en": "Fungsi Armor?", "id": "Melindungi Dari Gigitan Binatang Atau Benturan." },
  { "en": "Apa Itu Halogen Free Cable?", "id": "Kabel Yang Tidak Mengeluarkan Asap Beracun." },
  { "en": "Digunakan Di Mana?", "id": "Area Publik Seperti Terowongan Dan Stasiun." },
  { "en": "Apa Itu Resistivitas?", "id": "Hambatan Jenis Suatu Bahan Konduktor." },
  { "en": "Bahan Dengan Resistivitas Rendah?", "id": "Adalah Konduktor Listrik Yang Baik." },
  { "en": "Contohnya Adalah?", "id": "Perak Tembaga Emas Dan Aluminium." },
  { "en": "Apa Itu Dielectric Strength?", "id": "Kekuatan Isolator Menahan Medan Listrik." },
  { "en": "Satuan Dielectric Strength?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Itu Kode IP (IP Rating)?", "id": "Standar Proteksi Terhadap Debu Dan Air." },
  { "en": "Konektor Kabel Bisa Punya IP Rating?", "id": "Ya Untuk Penggunaan Luar Ruangan." },
  { "en": "Apa Itu Kabel Termokopel?", "id": "Kabel Khusus Untuk Sensor Suhu Termokopel." },
  { "en": "Terbuat Dari Apa?", "id": "Dua Jenis Logam Yang Berbeda." },
  { "en": "Prinsip Kerjanya?", "id": "Menghasilkan Tegangan Berdasarkan Perbedaan Suhu." },
  { "en": "Apa Itu Pemanasan Induksi?", "id": "Pemanasan Konduktor Akibat Medan Magnet." },
  { "en": "Apa Itu Gelombang Berjalan (Traveling Wave)?", "id": "Sinyal Yang Merambat Sepanjang Kabel." },
  { "en": "Kecepatan Rambat Tergantung?", "id": "Sifat Dielektrik Dari Isolator Kabel." },
  { "en": "Apa Itu Konduktor Terdampar?", "id": "Sama Dengan Konduktor Berjenis Serabut." },
  { "en": "Apa Itu Konduktor Padat?", "id": "Sama Dengan Konduktor Berjenis Pejal." },
  { "en": "Fleksibilitas Mana Yang Lebih Baik?", "id": "Konduktor Terdampar Atau Serabut." },
  { "en": "Ketahanan Mekanis Mana Yang Lebih Baik?", "id": "Konduktor Padat Atau Pejal." },
  { "en": "Apa Itu Selubung Dalam (Inner Sheath)?", "id": "Lapisan Pelindung Di Bawah Armor." },
  { "en": "Fungsi Selubung Dalam?", "id": "Sebagai Bantalan Untuk Lapisan Armor." },
  { "en": "Apa Itu Armor Kabel?", "id": "Lapisan Pelindung Mekanis Dari Logam." },
  { "en": "Bahan Armor Kabel?", "id": "Pita Baja Atau Kawat Baja." },
  { "en": "Kabel Armored Digunakan Untuk?", "id": "Instalasi Tanam Langsung Di Tanah." },
  { "en": "Tujuannya Melindungi Dari?", "id": "Tekanan Mekanis Dan Gigitan Hewan." },
  { "en": "Apa Itu Jacket Kabel?", "id": "Lapisan Paling Luar Dari Kabel." },
  { "en": "Fungsi Jacket Kabel?", "id": "Melindungi Dari Cuaca Lembab Dan Bahan Kimia." },
  { "en": "Apa Itu Kode Warna Internasional?", "id": "Standar IEC (International Electrotechnical Commission)." },
  { "en": "Apa Itu Kode Warna Lokal?", "id": "Standar PUIL Di Indonesia." },
  { "en": "Apa Kepanjangan PUIL?", "id": "Persyaratan Umum Instalasi Listrik." },
  { "en": "Kabel Untuk Listrik Satu Fasa?", "id": "Minimal Memiliki Dua Inti." },
  { "en": "Dua Inti Tersebut Adalah?", "id": "Kabel Fasa Dan Kabel Netral." },
  { "en": "Disarankan Menggunakan Berapa Inti?", "id": "Tiga Inti Termasuk Dengan Ground." },
  { "en": "Kabel Untuk Listrik Tiga Fasa?", "id": "Bisa Tiga Atau Empat Inti." },
  { "en": "Jika Tiga Inti Terdiri Dari?", "id": "Tiga Kabel Fasa (R S T)." },
  { "en": "Jika Empat Inti Terdiri Dari?", "id": "Tiga Fasa Dan Satu Netral." },
  { "en": "Untuk Tiga Fasa Seimbang Netralnya?", "id": "Tidak Mengalirkan Arus Listrik." },
  { "en": "Apa Itu Kawat Email?", "id": "Kawat Tembaga Dengan Lapisan Isolasi Tipis." },
  { "en": "Di Mana Kawat Email Digunakan?", "id": "Gulungan Motor Transformator Dan Solenoid." },
  { "en": "Isolasinya Tahan Terhadap Apa?", "id": "Suhu Tinggi." },
  { "en": "Apa Itu Kabel HDMI?", "id": "High-Definition Multimedia Interface." },
  { "en": "HDMI Menghantarkan Sinyal Apa?", "id": "Video Dan Audio Digital." },
  { "en": "HDMI Menggunakan Kabel Jenis?", "id": "Twisted Pair Dengan Pelindung (Shield)." },
  { "en": "Apa Itu Kabel USB?", "id": "Universal Serial Bus." },
  { "en": "Kabel USB Terdiri Dari?", "id": "Pasangan Data Dan Saluran Daya." },
  { "en": "Apa Itu Kabel Fleksibel Datar (FFC)?", "id": "Kabel Sangat Tipis Dan Fleksibel." },
  { "en": "Digunakan Di Mana?", "id": "Peralatan Elektronik Portabel Seperti Laptop." },
  { "en": "Apa Itu Pipa Konduit?", "id": "Pipa Pelindung Untuk Jalur Kabel." },
  { "en": "Bahan Pipa Konduit?", "id": "Logam Atau Plastik (PVC)." },
  { "en": "Tujuan Pipa Konduit?", "id": "Melindungi Kabel Dari Gangguan Mekanis." },
  { "en": "Apa Itu Ferit (Ferrite Bead)?", "id": "Komponen Pasif Di Sekitar Kabel." },
  { "en": "Bentuk Ferit Biasanya?", "id": "Silinder Kecil Dekat Ujung Kabel." },
  { "en": "Fungsi Ferit?", "id": "Menekan Gangguan Frekuensi Tinggi (EMI)." },
  { "en": "Ferit Bekerja Seperti Filter?", "id": "Filter Low-Pass Sederhana." },
  { "en": "Apa Itu Atenuasi Sinyal?", "id": "Pelemahan Kekuatan Sinyal Sepanjang Kabel." },
  { "en": "Penyebab Atenuasi?", "id": "Resistansi Dan Efek Dielektrik." },
  { "en": "Atenuasi Lebih Buruk Pada?", "id": "Frekuensi Yang Lebih Tinggi." },
  { "en": "Apa Itu Kecepatan Propagasi?", "id": "Seberapa Cepat Sinyal Merambat Di Kabel." },
  { "en": "Kecepatan Propagasi Selalu?", "id": "Lebih Lambat Dari Kecepatan Cahaya." },
  { "en": "Ditentukan Oleh Konstanta Apa?", "id": "Konstanta Dielektrik Bahan Isolator." },
  { "en": "Isolator Udara Punya Kecepatan?", "id": "Paling Cepat Mendekati Kecepatan Cahaya." },
  { "en": "Apa Itu Kabel Las?", "id": "Kabel Untuk Menyalurkan Arus Pengelasan." },
  { "en": "Fitur Kabel Las?", "id": "Sangat Fleksibel Dan Tahan Suhu Tinggi." },
  { "en": "Konduktornya Berjenis Apa?", "id": "Serabut Sangat Halus." },
  { "en": "Apa Itu Kabel Tahan Panas?", "id": "Kabel Dengan Isolasi Tahan Suhu Tinggi." },
  { "en": "Contoh Bahan Isolasinya?", "id": "Silikon Atau Fiberglass." },
  { "en": "Apa Itu Kabel Audio?", "id": "Kabel Untuk Menghantarkan Sinyal Suara." },
  { "en": "Jenis Kabel Audio?", "id": "Analog (RCA XLR) Dan Digital (Toslink)." },
  { "en": "Apa Itu Kabel Coaxial Digital?", "id": "Kabel Coaxial 75 Ohm Untuk Audio Digital." },
  { "en": "Apa Itu Kabel Toslink?", "id": "Kabel Serat Optik Untuk Audio Digital." },
  { "en": "Apa Itu Kabel Bus?", "id": "Kabel Untuk Komunikasi Data Serial." },
  { "en": "Contoh Sistem Bus?", "id": "CAN Bus (Otomotif) Modbus (Industri)." },
  { "en": "Apa Itu Kabel Tray?", "id": "Rak Terbuka Untuk Menopang Jalur Kabel." },
  { "en": "Apa Itu Kabel Trunking?", "id": "Saluran Tertutup Untuk Menopang Jalur Kabel." },
  { "en": "Apa Itu Kabel Gland?", "id": "Perangkat Untuk Memasang Kabel Ke Panel." },
  { "en": "Fungsi Kabel Gland?", "id": "Memberi Kekuatan Mekanis Dan Segel." },
  { "en": "Apa Itu Lugs Kabel?", "id": "Terminal Untuk Menyambung Kabel Ke Baut." },
  { "en": "Lugs Dipasang Dengan Alat?", "id": "Tang Crimping." },
  { "en": "Apa Itu Splicing?", "id": "Menyambung Dua Ujung Kabel Secara Permanen." },
  { "en": "Apa Itu Tapping?", "id": "Membuat Cabang Dari Kabel Utama." },
  { "en": "Apa Itu Kabel Antena?", "id": "Kabel Yang Menghubungkan Antena Ke Penerima." },
  { "en": "Jenisnya Biasanya Adalah?", "id": "Kabel Coaxial." },
  { "en": "Apa Itu Kabel TC?", "id": "Konduktor Tembaga Dilapisi Timah (Tinned Copper)." },
  { "en": "Tujuan Pelapisan Timah?", "id": "Mencegah Oksidasi Dan Memudahkan Penyolderan." },
  { "en": "Apa Itu Kode IP?", "id": "Ingress Protection Rating." },
  { "en": "Kode IP Menunjukkan Proteksi Terhadap?", "id": "Benda Padat Dan Cairan." },
  { "en": "Angka Pertama Untuk?", "id": "Proteksi Terhadap Benda Padat (Debu)." },
  { "en": "Angka Kedua Untuk?", "id": "Proteksi Terhadap Cairan (Air)." },
  { "en": "Apa Itu Rating Tegangan Kabel?", "id": "Tegangan Maksimum Yang Aman Untuk Kabel." },
  { "en": "Contoh Rating Tegangan?", "id": "300/500 Volt." },
  { "en": "Angka Pertama (300V) Adalah?", "id": "Tegangan Antara Inti Dan Tanah." },
  { "en": "Angka Kedua (500V) Adalah?", "id": "Tegangan Antara Dua Inti Fasa." },
  { "en": "Apa Itu Uji Megger?", "id": "Mengukur Tahanan Isolasi Kabel." },
  { "en": "Hasil Uji Yang Baik?", "id": "Tahanan Isolasi Sangat Tinggi." },
  { "en": "Apa Itu Kabel Twinax?", "id": "Seperti Coaxial Tapi Dengan Dua Konduktor Pusat." },
  { "en": "Digunakan Untuk Sinyal?", "id": "Sinyal Diferensial Kecepatan Tinggi." },
  { "en": "Apa Itu Sinyal Diferensial?", "id": "Sinyal Dibawa Oleh Dua Kawat Berlawanan." },
  { "en": "Keuntungan Sinyal Diferensial?", "id": "Sangat Tahan Terhadap Gangguan Noise." },
  { "en": "Contoh Penggunaannya?", "id": "USB Ethernet Dan HDMI." },
  { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Beberapa Kawat Disusun Berdampingan." },
  { "en": "Apa Itu Kabel Bawah Tanah?", "id": "Kabel Yang Dirancang Untuk Dikubur Langsung." },
  { "en": "Perlindungannya Harus Kuat?", "id": "Ya Biasanya Dengan Armor Baja." },
  { "en": "Apa Itu Jalur Kabel (Cable Duct)?", "id": "Pipa Bawah Tanah Untuk Menarik Kabel." },
  { "en": "Apa Itu Polaritas Kabel?", "id": "Identifikasi Konduktor Positif Dan Negatif." },
  { "en": "Penting Untuk Sistem Apa?", "id": "Sistem DC Dan Speaker Audio." },
  { "en": "Bagaimana Menandai Polaritas?", "id": "Warna Garis Atau Bentuk Isolator." },
  { "en": "Apa Itu Hambatan Jenis?", "id": "Sama Dengan Resistivitas." },
  { "en": "Apa Itu Konduktansi?", "id": "Kebalikan Dari Resistansi." },
  { "en": "Satuan Konduktansi?", "id": "Siemens (S)." },
  { "en": "Apa Itu Kabel Konsentris?", "id": "Konduktor Netral Mengelilingi Konduktor Fasa." },
  { "en": "Penggunaan Kabel Konsentris?", "id": "Kabel Sambungan Layanan Ke Rumah." },
  { "en": "Apa Itu Kabel Instrumentasi?", "id": "Kabel Untuk Mengirim Sinyal Kontrol Presisi." },
  { "en": "Fitur Kabel Instrumentasi?", "id": "Selalu Memiliki Pelindung (Shield) Yang Baik." },
  { "en": "Tujuannya Melindungi Sinyal Dari?", "id": "Interferensi Dan Crosstalk." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Pasangan Konduktor Yang Dipilin." },
  { "en": "Kenapa Konduktornya Dipilin?", "id": "Untuk Mengurangi Gangguan Elektromagnetik." },
  { "en": "Pilinan Menyebabkan Pembatalan?", "id": "Gangguan Yang Diterima Kedua Kawat." },
  { "en": "Apa Itu Kabel Kategori 5e (Cat 5e)?", "id": "Standar Kabel UTP Untuk Jaringan Ethernet." },
  { "en": "Kecepatan Maksimum Cat 5e?", "id": "Satu Gigabit Per Detik." },
  { "en": "Apa Itu Kabel Kategori 6 (Cat 6)?", "id": "Standar UTP Dengan Performa Lebih Tinggi." },
  { "en": "Keuntungan Cat 6?", "id": "Lebih Tahan Terhadap Crosstalk." },
  { "en": "Apa Itu Kabel Serat Optik?", "id": "Kabel Yang Menghantarkan Sinyal Cahaya." },
  { "en": "Apa Keuntungan Utama Serat Optik?", "id": "Kekebalan Total Terhadap Interferensi Elektromagnetik." },
  { "en": "Bandwidth Serat Optik?", "id": "Sangat Besar Jauh Melebihi Kabel Tembaga." },
  { "en": "Apa Itu Cladding?", "id": "Lapisan Kaca Yang Mengelilingi Inti Optik." },
  { "en": "Indeks Bias Cladding?", "id": "Lebih Rendah Dari Indeks Bias Inti." },
  { "en": "Perbedaan Indeks Bias Menyebabkan?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Coating Kabel Optik?", "id": "Lapisan Plastik Pelindung Paling Luar." },
  { "en": "Apa Itu Kabel Simplex?", "id": "Kabel Optik Dengan Satu Serat." },
  { "en": "Apa Itu Kabel Duplex?", "id": "Kabel Optik Dengan Dua Serat." },
  { "en": "Digunakan Untuk Komunikasi?", "id": "Dua Arah Secara Bersamaan." },
  { "en": "Apa Itu Tegangan Operasi?", "id": "Tegangan Normal Saat Kabel Digunakan." },
  { "en": "Apa Itu Tegangan Rating?", "id": "Tegangan Maksimum Yang Diizinkan Pabrikan." },
  { "en": "Tegangan Rating Harus Selalu?", "id": "Lebih Tinggi Dari Tegangan Operasi." },
  { "en": "Apa Itu Faktor Keamanan (Safety Factor)?", "id": "Rasio Antara Rating Dan Kondisi Operasi." },
  { "en": "Apa Itu Kulit Konduktor (Skin Effect)?", "id": "Arus AC Cenderung Mengalir Di Permukaan." },
  { "en": "Skin Effect Meningkatkan Apa?", "id": "Resistansi Efektif Dari Konduktor." },
  { "en": "Efek Ini Signifikan Pada?", "id": "Frekuensi Yang Sangat Tinggi." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Tidak Merata Akibat Konduktor Berdekatan." },
  { "en": "Efek Ini Juga Meningkatkan?", "id": "Resistansi Efektif Dan Kehilangan Daya." },
  { "en": "Apa Itu Kabel Litz?", "id": "Kabel Terdiri Dari Banyak Kawat Terisolasi." },
  { "en": "Tujuan Kabel Litz?", "id": "Mengurangi Skin Effect Dan Proximity Effect." },
  { "en": "Digunakan Pada Aplikasi?", "id": "Frekuensi Tinggi Seperti Pemanas Induksi." },
  { "en": "Apa Itu Kabel Tray?", "id": "Struktur Untuk Menopang Dan Merutekan Kabel." },
  { "en": "Apa Itu Tangga Kabel (Cable Ladder)?", "id": "Jenis Kabel Tray Berbentuk Seperti Tangga." },
  { "en": "Apa Itu Conduit?", "id": "Pipa Pelindung Untuk Kabel Listrik." },
  { "en": "Apa Itu Trunking?", "id": "Saluran Pelindung Persegi Untuk Kabel." },
  { "en": "Apa Itu Kabel Bawah Air?", "id": "Sama Dengan Kabel Bawah Laut." },
  { "en": "Isolasinya Harus Tahan Terhadap?", "id": "Tekanan Air Dan Korosi Air Garam." },
  { "en": "Apa Itu Sambungan Las (Welded Splice)?", "id": "Menyambung Konduktor Dengan Proses Pengelasan." },
  { "en": "Apa Itu Sambungan Tekan (Compression Splice)?", "id": "Menyambung Konduktor Menggunakan Tekanan Mekanis." },
  { "en": "Apa Itu Isolasi Kertas Berisi Minyak?", "id": "Isolasi Tradisional Untuk Kabel Tegangan Tinggi." },
  { "en": "Apa Itu Isolasi XLPE?", "id": "Cross-Linked Polyethylene." },
  { "en": "Keuntungan XLPE?", "id": "Tahan Suhu Lebih Tinggi Dari PVC." },
  { "en": "Apa Itu Kabel LSZH?", "id": "Low Smoke Zero Halogen." },
  { "en": "Keuntungan Kabel LSZH?", "id": "Tidak Mengeluarkan Asap Beracun Saat Terbakar." },
  { "en": "Digunakan Di Area Publik?", "id": "Ya Seperti Kapal Dan Gedung Bertingkat." },
  { "en": "Apa Itu Temperatur Rating Kabel?", "id": "Suhu Operasi Maksimum Yang Diizinkan." },
  { "en": "Melebihi Temperatur Rating Menyebabkan?", "id": "Degradasi Dan Kerusakan Isolasi." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Kecil Yang Mengalir Melalui Isolator." },
  { "en": "Isolator Sempurna Punya Arus Bocor?", "id": "Nol (Tidak Ada Di Dunia Nyata)." },
  { "en": "Apa Itu Uji Hi-Pot?", "id": "High Potential Test." },
  { "en": "Tujuan Uji Hi-Pot?", "id": "Menguji Kekuatan Dielektrik Isolasi." },
  { "en": "Apa Itu Konektor RJ45?", "id": "Konektor Standar Untuk Kabel Ethernet." },
  { "en": "Apa Itu Konektor BNC?", "id": "Konektor Untuk Kabel Coaxial." },
  { "en": "Apa Itu Konektor F?", "id": "Konektor Untuk Kabel Antena TV." },
  { "en": "Apa Itu Kabel Koil?", "id": "Kabel Berbentuk Spiral Seperti Kabel Telepon." },
  { "en": "Apa Itu Pengujian Kontinuitas?", "id": "Memastikan Tidak Ada Kabel Yang Putus." },
  { "en": "Alat Untuk Uji Kontinuitas?", "id": "Multimeter Dalam Mode Ohm." },
  { "en": "Hasil Yang Baik Menunjukkan Resistansi?", "id": "Sangat Rendah Mendekati Nol Ohm." },
  { "en": "Apa Itu Penandaan Kabel (Cable Marking)?", "id": "Informasi Tercetak Di Selubung Luar Kabel." },
  { "en": "Informasi Apa Yang Ada?", "id": "Jenis Ukuran Standar Dan Nama Pabrikan." },
  { "en": "Apa Itu Derating Kabel?", "id": "Pengurangan KHA Akibat Kondisi Instalasi." },
  { "en": "Contoh Kondisi Derating?", "id": "Suhu Sekitar Tinggi Atau Banyak Kabel Berdekatan." },
  { "en": "Kenapa Banyak Kabel Berdekatan Mengurangi KHA?", "id": "Karena Panas Sulit Terbuang." },
  { "en": "Apa Itu Kode NEMA?", "id": "Standar Amerika Untuk Peralatan Listrik." },
  { "en": "Apa Itu Kode IEC?", "id": "Standar Internasional Untuk Peralatan Listrik." },
  { "en": "Apa Itu Kabel Listrik Portabel?", "id": "Sama Dengan Kabel Fleksibel." },
  { "en": "Apa Itu Penuaan Isolasi (Aging)?", "id": "Degradasi Sifat Isolator Seiring Waktu." },
  { "en": "Penyebab Utama Penuaan?", "id": "Panas Oksigen Dan Tegangan Listrik." },
  { "en": "Umur Kabel Tergantung Pada?", "id": "Kualitas Isolasi Dan Kondisi Operasi." },
  { "en": "Apa Itu Sambungan Dingin (Cold Joint)?", "id": "Sambungan Solder Yang Tidak Sempurna." },
  { "en": "Apa Itu Sambungan Kering (Dry Joint)?", "id": "Sama Dengan Sambungan Dingin." },
  { "en": "Sambungan Dingin Menyebabkan Resistansi?", "id": "Resistansi Tinggi Dan Bisa Gagal." },
  { "en": "Apa Itu Kabel Triplex?", "id": "Kabel Udara Dengan Tiga Konduktor Dipilin." },
  { "en": "Terdiri Dari Apa Saja?", "id": "Dua Fasa Terisolasi Dan Satu Netral Telanjang." },
  { "en": "Fungsi Netral Telanjang?", "id": "Sebagai Penopang Mekanis (Messenger Wire)." },
  { "en": "Apa Itu Kabel Quadruplex?", "id": "Kabel Udara Dengan Empat Konduktor Dipilin." },
  { "en": "Terdiri Dari Apa Saja?", "id": "Tiga Fasa Terisolasi Dan Satu Netral." },
  { "en": "Apa Itu Tahanan Kontak?", "id": "Resistansi Pada Titik Sambungan Dua Konduktor." },
  { "en": "Tahanan Kontak Yang Baik?", "id": "Harus Sangat Rendah." },
  { "en": "Tahanan Kontak Tinggi Menyebabkan?", "id": "Pemanasan Berlebih Pada Titik Sambungan." },
  { "en": "Apa Itu Pelumas Kabel (Cable Lubricant)?", "id": "Cairan Untuk Memudahkan Penarikan Kabel." },
  { "en": "Di Mana Digunakan?", "id": "Saat Menarik Kabel Panjang Dalam Pipa Konduit." },
  { "en": "Apa Itu Kabel Hibrida?", "id": "Kabel Yang Menggabungkan Beberapa Jenis Kabel." },
  { "en": "Contoh Kabel Hibrida?", "id": "Kabel Yang Berisi Serat Optik Dan Tembaga." },
  { "en": "Tujuannya Untuk?", "id": "Menyalurkan Data Dan Listrik Dalam Satu Kabel." },
  { "en": "Apa Itu Kabel Serabut Terdampar Konsentris?", "id": "Jenis Serabut Dengan Pola Pilinan Tertentu." },
  { "en": "Apa Itu Oksidasi Konduktor?", "id": "Reaksi Kimia Konduktor Dengan Oksigen." },
  { "en": "Oksidasi Menyebabkan Apa?", "id": "Meningkatkan Resistansi Pada Permukaan." },
  { "en": "Aluminium Lebih Mudah Teroksidasi?", "id": "Ya Dibandingkan Dengan Tembaga." },
  { "en": "Sambungan Aluminium Membutuhkan?", "id": "Pasta Anti-Oksidan." },
  { "en": "Apa Itu Kabel Listrik?", "id": "Media Untuk Menyalurkan Energi Listrik." },
  { "en": "Bahan Utama Konduktor?", "id": "Tembaga Atau Aluminium." },
  { "en": "Bahan Utama Isolator?", "id": "PVC Karet Atau XLPE." },
  { "en": "Apa Itu Kabel Pejal?", "id": "Konduktor Tunggal Yang Solid." },
  { "en": "Apa Itu Kabel Serabut?", "id": "Kumpulan Kawat Halus Yang Dipilin." },
  { "en": "Kabel Mana Yang Lebih Fleksibel?", "id": "Kabel Serabut." },
  { "en": "Apa Itu KHA?", "id": "Kemampuan Hantar Arus Suatu Kabel." },
  { "en": "Satuan KHA Adalah?", "id": "Ampere (A)." },
  { "en": "Ukuran Kabel Lebih Besar KHA-nya?", "id": "Semakin Besar." },
  { "en": "Apa Itu Jatuh Tegangan?", "id": "Penurunan Tegangan Sepanjang Kabel." },
  { "en": "Penyebab Jatuh Tegangan?", "id": "Resistansi Dari Kabel." },
  { "en": "Cara Menguranginya?", "id": "Memperbesar Ukuran Penampang Kabel." },
  { "en": "Warna Kabel Grounding Standar?", "id": "Loreng Hijau-Kuning." },
  { "en": "Warna Kabel Netral Standar?", "id": "Biru." },
  { "en": "Warna Kabel Fasa Standar?", "id": "Hitam Coklat Abu-abu." },
  { "en": "Fungsi Kabel Grounding?", "id": "Sebagai Pengaman Dari Kebocoran Arus." },
  { "en": "Apa Itu Kabel NYA?", "id": "Kabel Tunggal Pejal Isolasi PVC." },
  { "en": "Instalasi Kabel NYA Perlu?", "id": "Menggunakan Pipa Pelindung." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Multi-Inti Dengan Selubung PVC Putih." },
  { "en": "Penggunaan Kabel NYM?", "id": "Instalasi Dalam Bangunan." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Multi-Inti Dengan Selubung PVC Hitam." },
  { "en": "Kelebihan Selubung NYY?", "id": "Tahan Terhadap Kondisi Cuaca." },
  { "en": "Penggunaan Kabel NYY?", "id": "Instalasi Luar Ruangan Atau Bawah Tanah." },
  { "en": "Apa Itu Kabel Coaxial?", "id": "Kabel Untuk Sinyal Frekuensi Radio." },
  { "en": "Struktur Kabel Coaxial?", "id": "Inti Dielektrik Jalinan Logam Dan Jaket." },
  { "en": "Fungsi Jalinan Logam (Shield)?", "id": "Melindungi Sinyal Dari Gangguan EMI." },
  { "en": "Apa Itu Kabel UTP?", "id": "Kabel Jaringan Tanpa Pelindung (Shield)." },
  { "en": "Kenapa Kawatnya Dipilin?", "id": "Untuk Mengurangi Crosstalk Dan Interferensi." },
  { "en": "Apa Itu Serat Optik?", "id": "Kabel Yang Menghantarkan Sinyal Cahaya." },
  { "en": "Keuntungan Utama Serat Optik?", "id": "Kebal Gangguan EMI Dan Kapasitas Sangat Besar." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
  { "en": "Efek Ini Signifikan Pada?", "id": "Frekuensi Yang Sangat Tinggi." },
  { "en": "Apa Itu Connector?", "id": "Komponen Penyambung Di Ujung Kabel." },
  { "en": "Contoh Connector Jaringan?", "id": "RJ45." },
  { "en": "Contoh Connector TV?", "id": "Konektor F Atau Konektor BNC." },
  { "en": "Apa Itu Crimping?", "id": "Proses Pemasangan Konektor Ke Kabel." },
  { "en": "Apa Itu Stripping?", "id": "Proses Mengupas Lapisan Isolasi Kabel." },
  { "en": "Apa Itu Kabel Tahan Api?", "id": "Kabel Yang Isolasinya Tidak Mudah Terbakar." },
  { "en": "Apa Itu Kabel Armored?", "id": "Kabel Dengan Pelindung Mekanis Logam." },
  { "en": "Tujuan Armor Kabel?", "id": "Melindungi Dari Gigitan Tikus Atau Benturan." },
  { "en": "Apa Itu Standar SNI?", "id": "Standar Nasional Indonesia Untuk Produk." },
  { "en": "Kenapa Harus Pilih Kabel SNI?", "id": "Terjamin Keamanan Dan Kualitasnya." },
  { "en": "Apa Itu Kabel Ladder?", "id": "Rak Kabel Terbuka Berbentuk Tangga." },
  { "en": "Apa Itu Kabel Tray?", "id": "Rak Kabel Terbuka Berbentuk Baki." },
  { "en": "Apa Itu Conduit?", "id": "Pipa Untuk Melindungi Jalur Kabel." },
  { "en": "Apa Itu Tegangan Tembus?", "id": "Kekuatan Maksimum Isolator Menahan Tegangan." },
  { "en": "Apa Itu Induktansi Kabel?", "id": "Menyebabkan Jatuh Tegangan Pada Arus AC." },
  { "en": "Apa Itu Kapasitansi Kabel?", "id": "Menyebabkan Arus Bocor Pada Arus AC." },
  { "en": "Dua Efek Ini Penting Pada?", "id": "Saluran Transmisi Yang Sangat Panjang." },
  { "en": "Apa Itu AWG?", "id": "Standar Ukuran Kawat Amerika." },
  { "en": "Angka AWG Kecil Berarti Kawat?", "id": "Semakin Tebal." },
  { "en": "Angka AWG Besar Berarti Kawat?", "id": "Semakin Tipis." },
  { "en": "Apa Itu Kabel LSZH?", "id": "Kabel Yang Mengeluarkan Sedikit Asap." },
  { "en": "Kepanjangan LSZH?", "id": "Low Smoke Zero Halogen." },
  { "en": "Pemanasan Kabel Disebabkan Oleh?", "id": "Aliran Arus Dan Resistansi Konduktor." },
  { "en": "Rumus Daya Hilang Di Kabel?", "id": "I Kuadrat Dikalikan R (I²R)." },
  { "en": "Apa Itu Skun Kabel?", "id": "Terminal Untuk Sambungan Baut." },
  { "en": "Apa Itu Junction Box?", "id": "Kotak Untuk Melindungi Sambungan Kabel." },
  { "en": "Apa Itu Kabel Hibrida?", "id": "Menggabungkan Konduktor Tembaga Dan Serat Optik." },
  { "en": "Apa Itu Kabel Pengelasan?", "id": "Sangat Fleksibel Dan Tahan Panas." },
  { "en": "Apa Itu Oksidasi?", "id": "Reaksi Logam Dengan Oksigen." },
  { "en": "Oksidasi Pada Sambungan Menyebabkan?", "id": "Resistansi Tinggi Dan Pemanasan." },
  { "en": "Bagaimana Mencegah Oksidasi Aluminium?", "id": "Menggunakan Pasta Anti Oksidan." },
  { "en": "Apa Itu Kabel Triplex?", "id": "Kabel Udara Untuk Jaringan Distribusi." },
  { "en": "Terdiri Dari Dua Fasa Dan?", "id": "Satu Kawat Netral Penggantung." },
  { "en": "Apa Itu Proximity Effect?", "id": "Distribusi Arus Tidak Merata Akibat Kedekatan." },
  { "en": "Efeknya Meningkatkan Apa?", "id": "Resistansi Dan Kerugian Daya." },
  { "en": "Apa Itu Kode IP?", "id": "Tingkat Proteksi Terhadap Debu Dan Air." },
  { "en": "Angka Pertama IP Menunjukkan?", "id": "Proteksi Terhadap Benda Padat." },
  { "en": "Angka Kedua IP Menunjukkan?", "id": "Proteksi Terhadap Cairan." },
  { "en": "IP68 Berarti Apa?", "id": "Tahan Debu Total Dan Tahan Terendam Air." },
  { "en": "Apa Itu Kabel Ribbon?", "id": "Kabel Datar Dengan Banyak Jalur Paralel." },
  { "en": "Apa Itu Kabel Data Serial?", "id": "Mengirim Data Bit Demi Bit." },
  { "en": "Apa Itu Kabel Data Paralel?", "id": "Mengirim Banyak Bit Secara Bersamaan." },
  { "en": "Kabel Mana Yang Lebih Cepat?", "id": "Kabel Serial Modern Jauh Lebih Cepat." },
  { "en": "Contoh Kabel Serial?", "id": "USB SATA Dan Ethernet." },
  { "en": "Apa Itu Rating Temperatur?", "id": "Suhu Maksimum Isolasi Bisa Bertahan." },
  { "en": "Apa Itu Kabel Termokopel?", "id": "Terbuat Dari Dua Logam Berbeda." },
  { "en": "Fungsinya Sebagai Sensor Apa?", "id": "Sensor Suhu." },
  { "en": "Apa Itu Uji Kontinuitas?", "id": "Memastikan Konduktor Tidak Putus." },
  { "en": "Hasil Baiknya Adalah Resistansi?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Uji Isolasi?", "id": "Memastikan Isolator Tidak Bocor." },
  { "en": "Hasil Baiknya Adalah Resistansi?", "id": "Sangat Tinggi (Megaohm)." },
  { "en": "Alat Uji Isolasi Disebut?", "id": "Megger Atau Insulation Tester." },
  { "en": "Apa Itu Kabel Koil?", "id": "Kabel Berbentuk Spiral Untuk Fleksibilitas." },
  { "en": "Apa Itu Kabel Gland?", "id": "Penyambung Kabel Ke Boks Panel." },
  { "en": "Fungsinya Memberi Segel Dan?", "id": "Pereda Ketegangan (Strain Relief)." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Peredam Gangguan Frekuensi Tinggi." },
  { "en": "Bekerja Seperti Filter Apa?", "id": "Filter Low-Pass." },
  { "en": "Konduktor Tembaga Dilapisi Timah Disebut?", "id": "Tinned Copper (TC)." },
  { "en": "Tujuannya Untuk Mencegah?", "id": "Oksidasi Dan Memudahkan Solder." },
  { "en": "Kabel Single-Mode Digunakan Untuk?", "id": "Jarak Jauh." },
  { "en": "Kabel Multi-Mode Digunakan Untuk?", "id": "Jarak Pendek." },
  { "en": "Kabel Mana Yang Lebih Mahal?", "id": "Kabel Single-Mode Dan Perangkatnya." },
  { "en": "Apa Itu Splicing Serat Optik?", "id": "Menyambung Dua Serat Optik Secara Permanen." },
  { "en": "Membutuhkan Alat Apa?", "id": "Fusion Splicer." },
  { "en": "Apa Itu Jaket Plenum?", "id": "Tahan Api Dan Mengeluarkan Sedikit Asap." },
  { "en": "Penting Untuk Instalasi Di?", "id": "Ruang Ventilasi Gedung." },
  { "en": "Apa Beda Konduktor Dan Isolator?", "id": "Konduktor Hantarkan Isolator Hambat Listrik." },
  { "en": "Apa Beda Kabel Pejal Dan Serabut?", "id": "Pejal Kaku Serabut Fleksibel." },
  { "en": "Apa Beda Kabel NYM Dan NYY?", "id": "NYM Untuk Dalam NYY Untuk Luar." },
  { "en": "Apa Beda Kabel UTP Dan STP?", "id": "STP Punya Lapisan Pelindung Tambahan." },
  { "en": "Apa Beda Serat Optik Dan Kabel Tembaga?", "id": "Optik Bawa Cahaya Tembaga Bawa Listrik." },
  { "en": "Apa Beda Tegangan Rating Dan Operasi?", "id": "Rating Batas Aman Operasi Tegangan Kerja." },
  { "en": "Apa Singkatan Dari KHA?", "id": "Kemampuan Hantar Arus." },
  { "en": "Apa Yang Ditentukan Oleh KHA?", "id": "Arus Maksimum Yang Aman Untuk Kabel." },
  { "en": "Apa Akibat Melebihi Batas KHA?", "id": "Kabel Panas Isolasi Meleleh Terbakar." },
  { "en": "Apa Itu Jatuh Tegangan Di Kabel?", "id": "Kehilangan Tegangan Di Sepanjang Kabel." },
  { "en": "Apa Penyebab Utama Jatuh Tegangan?", "id": "Resistansi Dari Konduktor Kabel." },
  { "en": "Bagaimana Panjang Kabel Mempengaruhi Jatuh Tegangan?", "id": "Kabel Lebih Panjang Jatuh Tegangan Lebih Besar." },
  { "en": "Bagaimana Arus Mempengaruhi Jatuh Tegangan?", "id": "Arus Lebih Besar Jatuh Tegangan Lebih Besar." },
  { "en": "Apa Fungsi Kode Warna Pada Kabel?", "id": "Identifikasi Fungsi Setiap Inti Kabel." },
  { "en": "Apa Warna Standar Untuk Kabel Grounding?", "id": "Hijau-Kuning." },
  { "en": "Apa Warna Standar Untuk Kabel Netral?", "id": "Biru." },
  { "en": "Apa Warna Standar Untuk Kabel Fasa?", "id": "Hitam Coklat Abu-abu." },
  { "en": "Apa Itu Kabel Berpelindung (Shielded)?", "id": "Kabel Dengan Pelindung Anti-Interferensi." },
  { "en": "Apa Nama Lapisan Pelindung Tersebut?", "id": "Shield Atau Jalinan Logam." },
  { "en": "Apa Itu Crosstalk Pada Kabel?", "id": "Gangguan Sinyal Antar Pasangan Kawat." },
  { "en": "Bagaimana Pilinan Kawat Mengurangi Crosstalk?", "id": "Dengan Membatalkan Efek Medan Magnet." },
  { "en": "Kenapa Impedansi Karakteristik Itu Penting?", "id": "Untuk Transfer Sinyal Frekuensi Tinggi." },
  { "en": "Apa Akibat Impedansi Tidak Cocok?", "id": "Menyebabkan Pantulan Sinyal Dan Kehilangan Daya." },
  { "en": "Apa Itu Atenuasi Sinyal?", "id": "Pelemahan Sinyal Seiring Bertambahnya Jarak." },
  { "en": "Apakah Atenuasi Bergantung Pada Frekuensi?", "id": "Ya Atenuasi Lebih Buruk Di Frekuensi Tinggi." },
  { "en": "Apa Itu Radius Tikungan Minimum?", "id": "Batas Kelengkungan Paling Tajam Untuk Kabel." },
  { "en": "Apa Resiko Membengkokkan Kabel Terlalu Tajam?", "id": "Merusak Konduktor Dan Lapisan Isolasi." },
  { "en": "Apa Itu Konduktivitas Listrik?", "id": "Kemampuan Bahan Menghantarkan Listrik." },
  { "en": "Logam Apa Dengan Konduktivitas Terbaik?", "id": "Perak." },
  { "en": "Kenapa Tembaga Lebih Umum Dari Perak?", "id": "Karena Tembaga Jauh Lebih Murah." },
  { "en": "Apa Itu Kekuatan Dielektrik?", "id": "Kekuatan Isolator Menahan Tegangan Tembus." },
  { "en": "Apa Fungsi Kabel Daya (Power Cable)?", "id": "Untuk Menyalurkan Tenaga Listrik." },
  { "en": "Apa Fungsi Kabel Sinyal?", "id": "Untuk Membawa Informasi Atau Data." },
  { "en": "Apa Fokus Desain Kabel Daya?", "id": "KHA Dan Jatuh Tegangan." },
  { "en": "Apa Fokus Desain Kabel Sinyal?", "id": "Impedansi Dan Perlindungan Terhadap Gangguan." },
  { "en": "Apa Kelebihan Isolasi Silikon?", "id": "Sangat Fleksibel Dan Tahan Suhu Tinggi." },
  { "en": "Apa Fungsi Kabel Tahan Api?", "id": "Tetap Berfungsi Selama Terjadi Kebakaran." },
  { "en": "Apa Kelebihan Kabel LSZH?", "id": "Tidak Mengeluarkan Asap Beracun Saat Terbakar." },
  { "en": "Di Mana Kabel LSZH Penting?", "id": "Di Ruang Publik Tertutup." },
  { "en": "Apa Efek Oksidasi Pada Sambungan?", "id": "Menyebabkan Pemanasan Lokal Dan Resiko Kebakaran." },
  { "en": "Apa Nama Alat Untuk Mengupas Kabel?", "id": "Pengupas Kabel Atau Wire Stripper." },
  { "en": "Apa Nama Alat Untuk Memasang Skun?", "id": "Tang Crimping Atau Crimping Tool." },
  { "en": "Apa Fungsi Timah Solder?", "id": "Menyambungkan Komponen Elektronik Secara Elektrikal." },
  { "en": "Apa Syarat Proses Menyolder?", "id": "Membutuhkan Panas Dan Permukaan Yang Bersih." },
  { "en": "Apa Fitur Kabel Bawah Tanah?", "id": "Memiliki Perlindungan Mekanis Yang Sangat Kuat." },
  { "en": "Apa Contoh Perlindungan Mekanis Tersebut?", "id": "Lapisan Armor Baja." },
  { "en": "Apa Arti Rating IP67 Pada Konektor?", "id": "Tahan Debu Dan Tahan Terendam Sementara." },
  { "en": "Serat Optik Multi-Mode Untuk Jarak Apa?", "id": "Jarak Pendek (Dalam Gedung)." },
  { "en": "Serat Optik Single-Mode Untuk Jarak Apa?", "id": "Jarak Sangat Jauh (Antar Kota)." },
  { "en": "Apa Sumber Cahaya Serat Multi-Mode?", "id": "LED (Light Emitting Diode)." },
  { "en": "Apa Sumber Cahaya Serat Single-Mode?", "id": "Laser Diode." },
  { "en": "Apa Itu Dispersi Modal?", "id": "Penyebaran Pulsa Cahaya Di Serat Multi-Mode." },
  { "en": "Apa Akibat Dispersi Modal?", "id": "Membatasi Bandwidth Dan Jarak Maksimum." },
  { "en": "Apakah Serat Single-Mode Mengalami Dispersi Modal?", "id": "Tidak Karena Hanya Membawa Satu Mode Cahaya." },
  { "en": "Apa Itu Kabel Audio Balanced?", "id": "Menggunakan Tiga Konduktor Untuk Satu Sinyal." },
  { "en": "Apa Nama Konektor Kabel Balanced?", "id": "Konektor XLR Atau TRS." },
  { "en": "Apa Keuntungan Utama Kabel Balanced?", "id": "Sangat Baik Dalam Menolak Gangguan (Noise)." },
  { "en": "Apa Itu Kabel Audio Unbalanced?", "id": "Menggunakan Dua Konduktor Untuk Satu Sinyal." },
  { "en": "Apa Nama Konektor Kabel Unbalanced?", "id": "Konektor RCA Atau TS." },
  { "en": "Kabel Mana Yang Lebih Umum Di Audio Konsumen?", "id": "Kabel Unbalanced." },
  { "en": "Apa Itu Pemanasan Dielektrik?", "id": "Pemanasan Isolator Akibat Medan Listrik AC." },
  { "en": "Kapan Pemanasan Dielektrik Menjadi Signifikan?", "id": "Pada Kabel Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Jalur Transmisi?", "id": "Kabel Dimana Panjangnya Sebanding Panjang Gelombang." },
  { "en": "Kapan Kabel Dianggap Jalur Transmisi?", "id": "Pada Frekuensi Sangat Tinggi (RF)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Ukuran Tingkat Ketidakcocokan Impedansi." },
  { "en": "Berapa Nilai SWR Yang Ideal?", "id": "Satu Banding Satu (1:1)." },
  { "en": "Apa Akibat Nilai SWR Yang Tinggi?", "id": "Banyak Daya Sinyal Yang Dipantulkan Kembali." },
  { "en": "Apa Kelebihan Kabel Superkonduktor?", "id": "Menghantarkan Listrik Tanpa Kehilangan Daya." },
  { "en": "Apa Kekurangan Kabel Superkonduktor?", "id": "Membutuhkan Pendinginan Suhu Sangat Rendah." },
  { "en": "Apa Fitur Utama Kabel Panel Surya?", "id": "Tahan Terhadap Sinar UV Dan Cuaca Ekstrem." },
  { "en": "Jenis Arus Pada Kabel Panel Surya?", "id": "Arus Searah (DC)." },
  { "en": "Apa Itu Kabel Pita Pemanas?", "id": "Kabel Yang Dirancang Untuk Menjadi Panas." },
  { "en": "Apa Aplikasi Kabel Pita Pemanas?", "id": "Mencegah Pembekuan Pipa Di Iklim Dingin." },
  { "en": "Apa Itu Kabel Non-Logam (NM Cable)?", "id": "Istilah Amerika Untuk Kabel Berbungkus Plastik." },
  { "en": "Apa Akibat Dari Corona Discharge?", "id": "Kehilangan Energi Dan Suara Berdesis." },
  { "en": "Apa Itu Tahanan Kontak?", "id": "Resistansi Yang Muncul Pada Titik Sambungan." },
  { "en": "Kenapa Tahanan Kontak Harus Rendah?", "id": "Untuk Mencegah Pemanasan Berlebih." },
  { "en": "Apa Itu Kabel Hibrida?", "id": "Satu Kabel Berisi Tembaga Dan Serat Optik." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Sama Dengan Kabel Coaxial." },
  { "en": "Apa Itu Konduktor Tembaga Kaleng?", "id": "Tembaga Yang Dilapisi Dengan Timah." },
  { "en": "Apa Keuntungan Tembaga Kaleng?", "id": "Tahan Korosi Dan Mudah Disolder." },
  { "en": "Apa Itu Isolator PVC?", "id": "Polyvinyl Chloride." },
  { "en": "Apa Itu Isolator XLPE?", "id": "Cross-Linked Polyethylene." },
  { "en": "Mana Yang Tahan Panas Lebih Baik?", "id": "XLPE Tahan Panas Lebih Baik Dari PVC." },
  { "en": "Apa Itu Kabel Submersible?", "id": "Kabel Untuk Pompa Air Bawah Air." },
  { "en": "Isolasinya Harus Tahan Terhadap Apa?", "id": "Air Dan Tekanan." },
  { "en": "Apa Itu Kawat Jumper?", "id": "Kabel Pendek Untuk Koneksi Sementara." },
  { "en": "Apa Itu Pigtail?", "id": "Kabel Pendek Untuk Menyambung Dari Perangkat." },
  { "en": "Apa Fungsi Klem Kabel?", "id": "Menjepit Dan Menahan Posisi Kabel." },
  { "en": "Apa Itu Tegangan Fasa-ke-Netral?", "id": "Tegangan Antara Kabel Fasa Dan Netral." },
  { "en": "Apa Itu Tegangan Fasa-ke-Fasa?", "id": "Tegangan Antara Dua Kabel Fasa." },
  { "en": "Di Sistem Tiga Fasa Mana Yang Lebih Tinggi?", "id": "Tegangan Fasa-ke-Fasa." },
  { "en": "Berapa Faktornya?", "id": "Akar Tiga (√3)." },
  { "en": "Apa Itu Kabel Twisted Shielded Pair (STP)?", "id": "Kabel Pilin Dengan Lapisan Pelindung." },
  { "en": "Apa Itu Kabel Twisted Unshielded Pair (UTP)?", "id": "Kabel Pilin Tanpa Lapisan Pelindung." },
  { "en": "Apa Itu Ukuran Milimeter Persegi?", "id": "Satuan Luas Penampang Konduktor." },
  { "en": "Kabel Untuk Motor Listrik Butuh Ukuran?", "id": "Cukup Besar Untuk Arus Awal Tinggi." },
  { "en": "Apa Itu Arus Inrush?", "id": "Arus Awal Yang Sangat Tinggi." },
  { "en": "Apa Beda Resistansi DC Dan AC?", "id": "Resistansi AC Lebih Tinggi Akibat Skin Effect." },
  { "en": "Apa Fungsi Utama Konektor Kabel?", "id": "Menghubungkan Dan Memutuskan Sirkuit Dengan Mudah." },
  { "en": "Apa Itu Kabel Serat Optik Plastik?", "id": "Serat Optik Dengan Inti Terbuat Dari Plastik." },
  { "en": "Apa Keuntungan Serat Optik Plastik?", "id": "Lebih Murah Fleksibel Dan Tahan Banting." },
  { "en": "Apa Kekurangan Serat Optik Plastik?", "id": "Atenuasi Lebih Tinggi Dari Serat Kaca." },
  { "en": "Apa Itu Kabel Composite?", "id": "Kabel Yang Membawa Beberapa Jenis Sinyal." },
  { "en": "Contoh Kabel Composite?", "id": "Kabel Video Komposit (Kuning Merah Putih)." },
  { "en": "Kabel Kuning Untuk Sinyal Apa?", "id": "Sinyal Video." },
  { "en": "Kabel Merah Dan Putih Untuk Apa?", "id": "Sinyal Audio Stereo (Kanan Dan Kiri)." },
  { "en": "Apa Itu Kabel Coaxial Hard-Line?", "id": "Kabel Coaxial Kaku Dengan Pelindung Pipa Logam." },
  { "en": "Digunakan Di Mana?", "id": "Aplikasi Daya Tinggi Seperti Pemancar Radio." },
  { "en": "Apa Itu Kabel Twin-Lead?", "id": "Kabel Datar Dengan Dua Konduktor Paralel." },
  { "en": "Penggunaan Kabel Twin-Lead?", "id": "Antena TV Lama." },
  { "en": "Impedansi Kabel Twin-Lead?", "id": "Biasanya 300 Ohm." },
  { "en": "Apa Itu Balun?", "id": "Perangkat Untuk Mencocokkan Impedansi Berbeda." },
  { "en": "Balun Menghubungkan Apa?", "id": "Kabel Balanced Dan Unbalanced." },
  { "en": "Apa Itu Kabel Tinsel?", "id": "Kabel Sangat Fleksibel Dengan Benang Tembaga." },
  { "en": "Digunakan Pada Apa?", "id": "Gagang Telepon Atau Headphone." },
  { "en": "Apa Itu Temperatur Ambient?", "id": "Suhu Lingkungan Di Sekitar Kabel." },
  { "en": "Temperatur Ambient Mempengaruhi Apa?", "id": "Kemampuan Hantar Arus (KHA) Kabel." },
  { "en": "Suhu Ambient Lebih Tinggi KHA?", "id": "Menjadi Lebih Rendah." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Kondisi Pemanasan Kabel Yang Tak Terkendali." },
  { "en": "Bisa Menyebabkan Apa?", "id": "Kegagalan Isolasi Dan Kebakaran." },
  { "en": "Apa Itu Kabel Penggantung (Messenger Wire)?", "id": "Kawat Penopang Untuk Kabel Udara." },
  { "en": "Apa Itu Lashing Kabel?", "id": "Mengikat Kabel Ke Kawat Penggantung." },
  { "en": "Apa Itu Kode NFPA?", "id": "Standar Keamanan Listrik Amerika Serikat." },
  { "en": "NFPA Singkatan Dari?", "id": "National Fire Protection Association." },
  { "en": "Apa Itu NEC?", "id": "National Electrical Code (Bagian Dari NFPA)." },
  { "en": "Apa Itu Konduktor Tembaga Annealed?", "id": "Tembaga Yang Dilunakkan Untuk Fleksibilitas." },
  { "en": "Apa Itu Konduktor Bare Copper?", "id": "Konduktor Tembaga Telanjang Tanpa Lapisan." },
  { "en": "Apa Itu Konduktor Tinned Copper?", "id": "Konduktor Tembaga Yang Dilapisi Timah." },
  { "en": "Apa Itu Arus Menerus (Continuous Current)?", "id": "Arus Yang Mengalir Konstan." },
  { "en": "Apa Itu Arus Intermiten?", "id": "Arus Yang Mengalir Secara Putus-putus." },
  { "en": "KHA Didasarkan Pada Arus?", "id": "Arus Menerus Dalam Kondisi Tertentu." },
  { "en": "Apa Itu Kabel Tahan Minyak?", "id": "Kabel Dengan Jaket Tahan Terhadap Oli." },
  { "en": "Digunakan Di Mana?", "id": "Lingkungan Industri Pabrik Dan Bengkel." },
  { "en": "Apa Itu Kabel Tahan Sinar UV?", "id": "Kabel Dengan Jaket Tahan Radiasi Ultraviolet." },
  { "en": "Penting Untuk Instalasi Apa?", "id": "Instalasi Luar Ruangan Yang Terkena Matahari." },
  { "en": "Sinar UV Dapat Merusak Apa?", "id": "Membuat Isolasi PVC Menjadi Rapuh." },
  { "en": "Apa Itu Titik Sambungan?", "id": "Tempat Dua Atau Lebih Kabel Terhubung." },
  { "en": "Kenapa Titik Sambungan Rawan Masalah?", "id": "Potensi Resistansi Tinggi Dan Pemanasan." },
  { "en": "Sambungan Harus Selalu Dilakukan Di?", "id": "Dalam Kotak Sambung (Junction Box)." },
  { "en": "Apa Itu Kelenjar Kabel (Cable Gland)?", "id": "Perangkat Penyambung Kabel Ke Panel." },
  { "en": "Fungsi Kelenjar Kabel?", "id": "Memberi Segel Dan Mencegah Kabel Tertarik." },
  { "en": "Apa Itu Kabel Audio Digital AES/EBU?", "id": "Standar Profesional Untuk Audio Digital." },
  { "en": "Impedansinya Berapa?", "id": "110 Ohm Menggunakan Kabel Twisted Pair." },
  { "en": "Apa Itu Kabel S/PDIF?", "id": "Standar Konsumen Untuk Audio Digital." },
  { "en": "Jenis Kabelnya Apa?", "id": "Coaxial 75 Ohm Atau Serat Optik." },
  { "en": "Apa Itu Pelindung Kepang (Braid Shield)?", "id": "Pelindung Terbuat Dari Jalinan Kawat Tembaga." },
  { "en": "Keuntungannya Apa?", "id": "Kuat Fleksibel Dan Memberi Cakupan Baik." },
  { "en": "Apa Itu Pelindung Foil?", "id": "Pelindung Terbuat Dari Lapisan Tipis Aluminium." },
  { "en": "Keuntungannya Apa?", "id": "Memberi Cakupan 100 Persen." },
  { "en": "Kabel Kualitas Tinggi Sering Menggunakan?", "id": "Kombinasi Pelindung Foil Dan Kepang." },
  { "en": "Apa Itu Kawat Drain (Drain Wire)?", "id": "Kawat Telanjang Kontak Dengan Pelindung Foil." },
  { "en": "Fungsi Kawat Drain?", "id": "Memudahkan Terminasi Ground Untuk Pelindung." },
  { "en": "Apa Itu Kabel Kontrol?", "id": "Kabel Multi-Inti Untuk Sinyal Kontrol Industri." },
  { "en": "Apa Itu Kabel Tray Berventilasi?", "id": "Kabel Tray Dengan Lubang Untuk Sirkulasi Udara." },
  { "en": "Tujuannya Untuk Membantu?", "id": "Pelesapan Panas Dari Kabel." },
  { "en": "Apa Itu Umur Pakai (Service Life) Kabel?", "id": "Estimasi Waktu Kabel Bisa Berfungsi Baik." },
  { "en": "Apa Faktor Yang Mempengaruhinya?", "id": "Suhu Beban Arus Dan Lingkungan." },
  { "en": "Apa Itu Kabel Koaksial Leaky?", "id": "Kabel Coaxial Yang Sengaja Dibuat Bocor." },
  { "en": "Tujuannya Untuk Berfungsi Sebagai?", "id": "Antena Panjang." },
  { "en": "Digunakan Di Mana?", "id": "Terowongan Tambang Atau Area Sulit Sinyal." },
  { "en": "Apa Itu Kabel Heliax?", "id": "Jenis Kabel Coaxial Kaku Kualitas Tinggi." },
  { "en": "Dielektriknya Seringkali Berupa?", "id": "Udara Atau Busa." },
  { "en": "Atenuasinya Sangat Rendah?", "id": "Ya Cocok Untuk Saluran Pemancar." },
  { "en": "Apa Itu TDR?", "id": "Time Domain Reflectometer." },
  { "en": "Fungsi TDR?", "id": "Mencari Lokasi Kerusakan Kabel." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Mengirim Pulsa Dan Menganalisis Pantulannya." },
  { "en": "Waktu Pantulan Menunjukkan?", "id": "Jarak Ke Lokasi Kerusakan." },
  { "en": "Bentuk Pantulan Menunjukkan?", "id": "Jenis Kerusakan (Putus Atau Hubung Singkat)." },
  { "en": "Apa Itu Kabel Bus Lapangan (Fieldbus)?", "id": "Kabel Untuk Jaringan Kontrol Industri." },
  { "en": "Apa Itu Kabel Data Pair?", "id": "Satu Pasang Kawat Untuk Komunikasi." },
  { "en": "Apa Itu Kabel Data Quad?", "id": "Empat Kawat Dipilin Bersama." },
  { "en": "Apa Itu Konduktivitas Relatif Tembaga?", "id": "Standar IACS (International Annealed Copper Standard)." },
  { "en": "Nilainya Ditetapkan Berapa?", "id": "100% IACS." },
  { "en": "Konduktivitas Aluminium Berapa?", "id": "Sekitar 61% IACS." },
  { "en": "Artinya Untuk Arus Sama Kabel Aluminium?", "id": "Harus Lebih Besar Dari Kabel Tembaga." },
  { "en": "Apa Itu Kabel Datar Bawah Karpet?", "id": "Kabel Sangat Datar Untuk Instalasi Tersembunyi." },
  { "en": "Apa Itu Kawat Tie?", "id": "Kawat Pengikat Untuk Merapikan Kabel." },
  { "en": "Apa Itu Cable Clamp?", "id": "Penjepit Untuk Menahan Kabel Di Tempatnya." },
  { "en": "Apa Itu Pipa Fleksibel?", "id": "Pipa Konduit Yang Bisa Dibengkokkan." },
  { "en": "Bahan Pipa Fleksibel?", "id": "Logam Atau Plastik Bergelombang." },
  { "en": "Apa Itu Konstanta Dielektrik (εr)?", "id": "Ukuran Kemampuan Isolator Menyimpan Energi." },
  { "en": "Konstanta Dielektrik Mempengaruhi?", "id": "Kapasitansi Dan Kecepatan Rambat Sinyal." },
  { "en": "Nilai Lebih Rendah Kecepatan?", "id": "Semakin Cepat." },
  { "en": "Apa Itu Faktor Disipasi?", "id": "Ukuran Kehilangan Energi Dalam Dielektrik." },
  { "en": "Nilai Yang Baik Adalah?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Gelombang Berdiri?", "id": "Pola Gelombang Akibat Pantulan Sinyal." },
  { "en": "Apa Itu Mismatch Impedansi?", "id": "Ketidakcocokan Impedansi Antara Kabel Dan Beban." },
  { "en": "Penyebab Utama Pantulan Adalah?", "id": "Mismatch Impedansi." },
  { "en": "Bagaimana Cara Mengidentifikasi Kabel?", "id": "Membaca Penandaan Di Selubung Luar." },
  { "en": "Apa Beda Arus AC Dan DC?", "id": "AC Bolak-balik DC Searah." },
  { "en": "Apakah Skin Effect Terjadi Pada Arus DC?", "id": "Tidak Hanya Terjadi Pada Arus AC." },
  { "en": "Apa Itu Kabel Serabut?", "id": "Konduktor Terdiri Dari Banyak Kawat Tipis." },
  { "en": "Apa Itu Kabel Pejal?", "id": "Konduktor Terdiri Dari Satu Kawat Padat." },
  { "en": "Mana Yang Lebih Baik Untuk Frekuensi Tinggi?", "id": "Kabel Serabut Karena Mengurangi Skin Effect." },
  { "en": "Apa Itu Konektor Hermetis?", "id": "Konektor Yang Kedap Udara Dan Air." },
  { "en": "Apa Itu Kabel Pigtail?", "id": "Kabel Pendek Dengan Konektor Di Satu Sisi." },
  { "en": "Digunakan Untuk Menyambung Apa?", "id": "Menyambung Kabel Ke Perangkat Elektronik." },
  { "en": "Apa Itu Tahanan Isolasi?", "id": "Ukuran Seberapa Baik Bahan Menahan Listrik." },
  { "en": "Satuan Tahanan Isolasi?", "id": "Megaohm Atau Gigaohm." },
  { "en": "Tahanan Isolasi Diukur Dengan?", "id": "Megohmmeter Atau Insulation Tester." },
  { "en": "Apa Itu Tembaga Bebas Oksigen (OFC)?", "id": "Tembaga Murni Untuk Kualitas Audio Tinggi." },
  { "en": "Apa Keuntungan OFC?", "id": "Konduktivitas Sedikit Lebih Baik." },
  { "en": "Apa Itu Kabel Speaker?", "id": "Kabel Untuk Menghubungkan Amplifier Ke Speaker." },
  { "en": "Ukuran Kabel Speaker Mempengaruhi Apa?", "id": "Faktor Redaman (Damping Factor)." },
  { "en": "Kabel Lebih Besar Faktor Redamannya?", "id": "Semakin Baik." },
  { "en": "Apa Itu Kabel Fleksibel (Flex Cable)?", "id": "Sama Dengan Kabel Serabut." },
  { "en": "Apa Itu Kabel Keras (Solid Cable)?", "id": "Sama Dengan Kabel Pejal." },
  { "en": "Apa Itu Kode Warna Resistor?", "id": "Bukan Standar Untuk Kabel Listrik." },
  { "en": "Kabel Listrik Menggunakan Standar Warna?", "id": "Standar Warna Isolator Inti." },
  { "en": "Apa Itu Kabel Bundled?", "id": "Beberapa Kabel Diikat Menjadi Satu." },
  { "en": "Tujuannya Untuk Apa?", "id": "Manajemen Kabel Agar Terlihat Rapi." },
  { "en": "Apa Itu Sleeving Kabel?", "id": "Selubung Tambahan Untuk Estetika Dan Proteksi." },
  { "en": "Apa Itu Konstanta Propagasi?", "id": "Parameter Yang Menggambarkan Perambatan Gelombang." },
  { "en": "Terdiri Dari Apa Saja?", "id": "Konstanta Atenuasi Dan Konstanta Fasa." },
  { "en": "Konstanta Atenuasi Menentukan?", "id": "Pelemahan Sinyal." },
  { "en": "Konstanta Fasa Menentukan?", "id": "Perubahan Fasa Sinyal." },
  { "en": "Apa Itu Kabel Koaksial Triaxial (Triax)?", "id": "Coaxial Dengan Satu Pelindung Tambahan." },
  { "en": "Gunanya Untuk Apa?", "id": "Aplikasi Pengukuran Sangat Sensitif." },
  { "en": "Apa Itu Return Loss?", "id": "Ukuran Sinyal Yang Dipantulkan Kembali." },
  { "en": "Return Loss Yang Baik Bernilai?", "id": "Sangat Tinggi (Dalam Desibel)." },
  { "en": "Apa Itu Insertion Loss?", "id": "Ukuran Sinyal Yang Hilang Melewati Komponen." },
  { "en": "Insertion Loss Yang Baik Bernilai?", "id": "Sangat Rendah (Dalam Desibel)." },
  { "en": "Apa Itu Kabel Direct Burial?", "id": "Kabel Yang Bisa Dikubur Langsung." },
  { "en": "Kabel NYY Termasuk Jenis Ini?", "id": "Ya Jika Memiliki Perlindungan Yang Cukup." },
  { "en": "Apa Itu Kabel PVC?", "id": "Kabel Dengan Isolasi Polyvinyl Chloride." },
  { "en": "Apa Itu Kabel XLPE?", "id": "Kabel Dengan Isolasi Cross-Linked Polyethylene." },
  { "en": "Mana Yang Lebih Tahan Panas?", "id": "XLPE Tahan Panas Lebih Baik." },
  { "en": "Apa Itu Kabel Karet (Rubber Cable)?", "id": "Kabel Dengan Isolasi Karet." },
  { "en": "Keuntungan Kabel Karet?", "id": "Sangat Fleksibel Dan Tahan Air." },
  { "en": "Digunakan Pada Apa?", "id": "Peralatan Tambang Atau Pompa Submersible." },
  { "en": "Apa Itu Kabel Spiral?", "id": "Sama Dengan Kabel Koil." },
  { "en": "Apa Itu Kabel Telepon?", "id": "Kabel Twisted Pair Untuk Saluran Telepon." },
  { "en": "Biasanya Terdiri Dari Berapa Pasang?", "id": "Satu Atau Dua Pasang Kawat." },
  { "en": "Apa Itu Kabel Hook-up?", "id": "Kabel Inti Tunggal Untuk Rangkaian Internal." },
  { "en": "Apa Itu Kapasitansi Sendiri (Self-Capacitance)?", "id": "Kapasitansi Internal Sebuah Komponen." },
  { "en": "Apa Itu Induktansi Sendiri (Self-Inductance)?", "id": "Induktansi Internal Sebuah Komponen." },
  { "en": "Apa Itu Titik Api (Flash Point) Isolator?", "id": "Suhu Dimana Uapnya Bisa Terbakar." },
  { "en": "Apa Itu Pelindung Magnetik?", "id": "Pelindung Efektif Untuk Frekuensi Rendah." },
  { "en": "Bahannya Terbuat Dari?", "id": "Logam Permeabilitas Tinggi (Mu-metal)." },
  { "en": "Apa Itu Pelindung Elektrik?", "id": "Pelindung Efektif Untuk Frekuensi Tinggi." },
  { "en": "Bahannya Terbuat Dari?", "id": "Logam Konduktivitas Tinggi (Tembaga)." },
  { "en": "Shield Kabel Coaxial Adalah Pelindung?", "id": "Pelindung Elektrik." },
  { "en": "Apa Itu Kabel Data Kategori 7 (Cat 7)?", "id": "Standar STP Untuk Kinerja Sangat Tinggi." },
  { "en": "Setiap Pasang Kawat Memiliki?", "id": "Pelindung Foil Sendiri." },
  { "en": "Apa Itu TDR?", "id": "Time Domain Reflectometer." },
  { "en": "TDR Dapat Mendeteksi Apa?", "id": "Lokasi Dan Jenis Kerusakan Kabel." },
  { "en": "Apa Itu OTDR?", "id": "Optical Time Domain Reflectometer." },
  { "en": "OTDR Digunakan Untuk Kabel?", "id": "Kabel Serat Optik." },
  { "en": "Apa Itu Fusion Splicer?", "id": "Alat Untuk Menyambung Serat Optik." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Melelehkan Dua Ujung Serat Dengan Listrik." },
  { "en": "Apa Itu Cleaver Serat Optik?", "id": "Alat Untuk Memotong Serat Optik Rata." },
  { "en": "Penting Untuk Sambungan?", "id": "Sangat Penting Untuk Sambungan Rendah Rugi." },
  { "en": "Apa Itu Firestop?", "id": "Bahan Untuk Menutup Celah Jalur Kabel." },
  { "en": "Tujuannya Untuk Mencegah?", "id": "Penyebaran Api Antar Ruangan." },
  { "en": "Apa Itu Kabel Tray Vertikal?", "id": "Kabel Tray Untuk Jalur Naik Turun." },
  { "en": "Apa Itu Service Loop?", "id": "Kelebihan Kabel Yang Digulung Dekat Terminasi." },
  { "en": "Tujuan Service Loop?", "id": "Cadangan Untuk Perbaikan Atau Perubahan." },
  { "en": "Apa Itu Kabel Berisolasi Mineral (MI)?", "id": "Kabel Sangat Tahan Api Dan Panas." },
  { "en": "Isolasinya Terbuat Dari?", "id": "Bubuk Magnesium Oksida." },
  { "en": "Selubungnya Terbuat Dari?", "id": "Pipa Tembaga." },
  { "en": "Apa Itu Kabel Kontrol Lift?", "id": "Kabel Datar Multi-Inti Yang Sangat Kuat." },
  { "en": "Apa Itu Kabel Derek (Crane Cable)?", "id": "Kabel Fleksibel Dan Kuat Untuk Derek." },
  { "en": "Apa Itu Kabel Robotik?", "id": "Kabel Tahan Tekukan Berulang Jutaan Kali." },
  { "en": "Apa Itu Faktor Pengisian Konduit?", "id": "Persentase Ruang Konduit Yang Boleh Diisi." },
  { "en": "Tidak Boleh 100 Persen Karena?", "id": "Masalah Pemanasan Dan Kemudahan Penarikan." },
  { "en": "Apa Itu Kabel Audio Seimbang?", "id": "Sama Dengan Kabel Balanced." },
  { "en": "Apa Itu Kabel Audio Tidak Seimbang?", "id": "Sama Dengan Kabel Unbalanced." },
  { "en": "Apa Itu Common Mode Rejection?", "id": "Kemampuan Sistem Balanced Menolak Noise." },
  { "en": "CMRR Yang Baik Bernilai?", "id": "Sangat Tinggi (Dalam Desibel)." },
  { "en": "Apa Itu Konduktor Berongga?", "id": "Konduktor Berbentuk Pipa." },
  { "en": "Digunakan Untuk Apa?", "id": "Saluran Busbar Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Ikatan Kabel (Cable Tie)?", "id": "Pengikat Plastik Untuk Merapikan Kabel." },
  { "en": "Bahan Ikatan Kabel?", "id": "Nilon." },
  { "en": "Apa Itu Kabel Koaksial RG-58?", "id": "Kabel Coaxial 50 Ohm Umum." },
  { "en": "Apa Itu Kabel Koaksial RG-59?", "id": "Kabel Coaxial 75 Ohm Umum." },
  { "en": "RG Singkatan Dari?", "id": "Radio Guide." },
  { "en": "Apa Itu Velositas Propagasi?", "id": "Kecepatan Rambat Sinyal Relatif Cahaya." },
  { "en": "Nilainya Selalu Kurang Dari?", "id": "100 Persen." },
  { "en": "Apa Itu Tang Amper (Clamp Meter)?", "id": "Mengukur Arus Tanpa Memutus Sirkuit." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Mengukur Medan Magnet Di Sekitar Kabel." },
  { "en": "Apa Itu Kabel Tahan Air?", "id": "Kabel Untuk Penggunaan Terendam Air." },
  { "en": "Apa Itu Kabel Tahan Kimia?", "id": "Kabel Dengan Jaket Tahan Bahan Kimia." },
  { "en": "Apa Itu Kabel Fleksibel Berpelindung?", "id": "Kabel Serabut Dengan Lapisan Shield." },
  { "en": "Digunakan Pada Apa?", "id": "Motor Dengan Drive Frekuensi Variabel (VFD)." },
  { "en": "Tujuan Pelindung?", "id": "Mencegah Kebisingan EMI Dari Drive." },
  { "en": "Apa Itu Kabel Serabut?", "id": "Konduktor Terdiri Dari Banyak Kawat Halus." },
  { "en": "Apa Itu Kabel Pejal?", "id": "Konduktor Terdiri Dari Satu Kawat Tunggal." },
  { "en": "Mana Yang Lebih Fleksibel?", "id": "Kabel Serabut Lebih Fleksibel." },
  { "en": "Mana Yang Tahan Getaran?", "id": "Kabel Pejal Lebih Tahan Getaran." },
  { "en": "Apa Itu KHA?", "id": "Kemampuan Hantar Arus Maksimum Kabel." },
  { "en": "Satuan KHA Adalah?", "id": "Ampere." },
  { "en": "Apa Efek Melebihi KHA?", "id": "Kabel Panas Dan Isolasi Rusak." },
  { "en": "Apa Itu Jatuh Tegangan?", "id": "Kehilangan Tegangan Di Sepanjang Kabel." },
  { "en": "Penyebab Jatuh Tegangan?", "id": "Resistansi Konduktor Kabel." },
  { "en": "Bagaimana Mengurangi Jatuh Tegangan?", "id": "Menggunakan Kabel Dengan Ukuran Lebih Besar." },
  { "en": "Apa Fungsi Warna Isolasi?", "id": "Untuk Identifikasi Fungsi Inti Kabel." },
  { "en": "Apa Warna Kabel Ground?", "id": "Hijau-Kuning." },
  { "en": "Apa Warna Kabel Netral?", "id": "Biru." },
  { "en": "Apa Warna Kabel Fasa?", "id": "Hitam Coklat Atau Abu-abu." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Multi-Inti Untuk Instalasi Dalam Ruangan." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Multi-Inti Untuk Instalasi Luar Ruangan." },
  { "en": "Apa Itu Kabel NYA?", "id": "Kabel Inti Tunggal Untuk Pemasangan Pipa." },
  { "en": "Apa Itu Kabel Coaxial?", "id": "Kabel Untuk Sinyal Frekuensi Tinggi." },
  { "en": "Fungsi Pelindung Di Kabel Coaxial?", "id": "Melindungi Sinyal Dari Gangguan EMI." },
  { "en": "Apa Itu Kabel UTP?", "id": "Kabel Pilin Tanpa Pelindung." },
  { "en": "Apa Fungsi Pilinan Pada UTP?", "id": "Mengurangi Crosstalk Antar Pasangan Kawat." },
  { "en": "Apa Itu Serat Optik?", "id": "Menghantarkan Data Menggunakan Sinyal Cahaya." },
  { "en": "Keuntungan Utama Serat Optik?", "id": "Kebal Terhadap Gangguan Elektromagnetik." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
  { "en": "Kapan Skin Effect Signifikan?", "id": "Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Konektor Kabel?", "id": "Komponen Untuk Menyambung Kabel Ke Perangkat." },
  { "en": "Contoh Konektor Jaringan?", "id": "RJ45." },
  { "en": "Apa Itu Stripping Kabel?", "id": "Proses Mengupas Lapisan Isolasi." },
  { "en": "Apa Itu Crimping Kabel?", "id": "Proses Memasang Konektor Pada Kabel." },
  { "en": "Apa Itu Kabel Armored?", "id": "Kabel Dengan Pelindung Mekanis Logam." },
  { "en": "Apa Fungsi Armor Kabel?", "id": "Melindungi Dari Benturan Fisik." },
  { "en": "Apa Itu Kabel Tahan Api?", "id": "Kabel Yang Isolasinya Sulit Terbakar." },
  { "en": "Apa Itu Kabel LSZH?", "id": "Kabel Rendah Asap Tanpa Halogen." },
  { "en": "Kenapa Kabel LSZH Penting?", "id": "Lebih Aman Saat Terjadi Kebakaran." },
  { "en": "Apa Itu Kabel Tray?", "id": "Struktur Penopang Jalur Kabel." },
  { "en": "Apa Itu Pipa Conduit?", "id": "Pipa Pelindung Untuk Kabel." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Penting Untuk Kabel Sinyal." },
  { "en": "Apa Akibat Impedansi Tidak Cocok?", "id": "Terjadi Pantulan Sinyal." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal Seiring Jarak." },
  { "en": "Atenuasi Lebih Besar Pada Frekuensi?", "id": "Lebih Tinggi." },
  { "en": "Apa Itu AWG?", "id": "Standar Amerika Untuk Ukuran Kawat." },
  { "en": "Nomor AWG Kecil Berarti Kawat?", "id": "Lebih Tebal." },
  { "en": "Apa Itu Tegangan Tembus?", "id": "Batas Kekuatan Isolator." },
  { "en": "Apa Itu Oksidasi Pada Sambungan?", "id":- "Menyebabkan Resistansi Tinggi Dan Panas." },
  { "en": "Bagaimana Mencegahnya Pada Aluminium?", "id": "Menggunakan Pasta Anti-Oksidan." },
  { "en": "Apa Itu Kabel Triplex?", "id": "Kabel Udara Untuk Jaringan Listrik." },
  { "en": "Apa Itu Proximity Effect?", "id": "Meningkatkan Resistansi Efektif Kabel." },
  { "en": "Apa Itu Rating IP?", "id": "Tingkat Proteksi Terhadap Debu Air." },
  { "en": "IP68 Berarti?", "id": "Tahan Debu Total Dan Tahan Air." },
  { "en": "Apa Itu Kabel Ribbon?", "id": "Kabel Datar Dengan Banyak Konduktor." },
  { "en": "Apa Itu Uji Kontinuitas?", "id": "Memastikan Konduktor Tidak Putus." },
  { "en": "Apa Itu Uji Isolasi?", "id": "Memastikan Isolator Tidak Bocor." },
  { "en": "Alat Uji Isolasi?", "id": "Megohmmeter Atau Insulation Tester." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Meredam Gangguan Frekuensi Tinggi." },
  { "en": "Apa Itu Tinned Copper?", "id": "Tembaga Dilapisi Timah." },
  { "en": "Tujuannya Untuk Mencegah?", "id": "Oksidasi Dan Memudahkan Solder." },
  { "en": "Kabel Fiber Single-Mode Untuk?", "id": "Transmisi Jarak Sangat Jauh." },
  { "en": "Kabel Fiber Multi-Mode Untuk?", "id": "Transmisi Jarak Pendek." },
  { "en": "Apa Itu Splicing Fiber?", "id": "Menyambung Dua Ujung Serat Optik." },
  { "en": "Apa Itu Jacket Plenum?", "id": "Jaket Kabel Tahan Api." },
  { "en": "Apa Itu Kabel Balanced?", "id": "Sangat Tahan Terhadap Gangguan." },
  { "en": "Contoh Konektor Balanced?", "id": "XLR." },
  { "en": "Apa Itu Kabel Unbalanced?", "id": "Lebih Rentan Terhadap Gangguan." },
  { "en": "Contoh Konektor Unbalanced?", "id": "RCA." },
  { "en": "Apa Itu TDR?", "id": "Alat Untuk Mencari Lokasi Kerusakan Kabel." },
  { "en": "Apa Itu OTDR?", "id": "TDR Untuk Kabel Serat Optik." },
  { "en": "Apa Itu Service Loop?", "id": "Gulungan Kabel Cadangan Untuk Perbaikan." },
  { "en": "Apa Itu Kabel MI?", "id": "Kabel Berisolasi Mineral Tahan Api." },
  { "en": "Apa Itu Common Mode Rejection?", "id": "Kemampuan Kabel Balanced Menolak Noise." },
  { "en": "Apa Itu Kabel Tie?", "id": "Pengikat Plastik Untuk Merapikan Kabel." },
  { "en": "Apa Itu Velositas Propagasi?", "id": "Kecepatan Sinyal Merambat Di Kabel." },
  { "en": "Apa Itu Clamp Meter?", "id": "Mengukur Arus Tanpa Memutus Kabel." },
  { "en": "Apa Itu Kabel Fleksibel Berpelindung?", "id": "Untuk Motor VFD Mencegah Gangguan EMI." },
  { "en": "Apa Itu Kabel Composite Video?", "id": "Kabel Analog Dengan Tiga Warna." },
  { "en": "Kuning Untuk Apa?", "id": "Video." },
  { "en": "Merah Dan Putih Untuk Apa?", "id": "Audio Stereo." },
  { "en": "Apa Itu Balun?", "id": "Mengubah Sinyal Balanced Ke Unbalanced." },
  { "en": "Apa Itu Derating Kabel?", "id": "Pengurangan KHA Akibat Kondisi Lingkungan." },
  { "en": "Suhu Panas Akan?", "id": "Mengurangi KHA Kabel." },
  { "en": "Banyak Kabel Berdekatan Akan?", "id": "Mengurangi KHA Setiap Kabel." },
  { "en": "Apa Itu Isolasi XLPE?", "id": "Tahan Panas Lebih Baik Dari PVC." },
  { "en": "Apa Itu Kawat Jumper?", "id": "Kabel Pendek Untuk Koneksi Papan Sirkuit." },
  { "en": "Apa Itu Uji Hi-Pot?", "id": "Menguji Kekuatan Isolasi Pada Tegangan Tinggi." },
  { "en": "Apa Itu Kabel Bawah Karpet?", "id": "Kabel Sangat Datar Untuk Instalasi Tersembunyi." },
  { "en": "Apa Itu Kabel Koil Tesla?", "id": "Kabel Untuk Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Superkonduktor?", "id": "Konduktor Tanpa Resistansi Sama Sekali." },
  { "en": "Membutuhkan Suhu Berapa?", "id": "Suhu Sangat Dingin." },
  { "en": "Apa Itu Kabel Solar?", "id": "Kabel Tahan Sinar UV Untuk Panel Surya." },
  { "en": "Apa Itu Konduktor Berongga?", "id": "Mengurangi Berat Dan Skin Effect." },
  { "en": "Apa Itu Ikatan Kabel?", "id": "Sama Dengan Kabel Tie." },
  { "en": "Apa Itu Kabel Koaksial RG-6?", "id": "Standar Umum Untuk TV Kabel." },
  { "en": "Apa Itu Pemanas Induksi?", "id": "Memanfaatkan Medan Magnet Dari Kabel." },
  { "en": "Apa Itu Tang Amper?", "id": "Sama Dengan Clamp Meter." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Mempengaruhi Kapasitansi Dan Kecepatan Sinyal." },
  { "en": "Nilai Rendah Berarti Kecepatan?", "id": "Lebih Cepat." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Penyebaran Pulsa Cahaya Di Serat Optik." },
  { "en": "Membatasi Apa?", "id": "Kecepatan Data Dan Jarak." },
  { "en": "Apa Itu Kabel Audio Digital?", "id": "Mengirim Audio Dalam Bentuk Bit." },
  { "en": "Contoh Kabel Audio Digital?", "id": "Toslink Atau Coaxial Digital." },
  { "en": "Apa Beda Tembaga Dan Aluminium?", "id": "Tembaga Konduktor Lebih Baik Dari Aluminium." },
  { "en": "Apa Beda Isolasi PVC Dan XLPE?", "id": "XLPE Tahan Suhu Lebih Tinggi Dari PVC." },
  { "en": "Apa Beda Shield Braid Dan Foil?", "id": "Braid Fleksibel Foil Memberi Cakupan Penuh." },
  { "en": "Apa Beda KHA Dan Rating Tegangan?", "id": "KHA Batas Arus Rating Batas Tegangan." },
  { "en": "Apa Beda Atenuasi Dan Jatuh Tegangan?", "id": "Atenuasi Untuk Sinyal Jatuh Tegangan Listrik." },
  { "en": "Apa Itu Kekuatan Tarik (Tensile Strength)?", "id": "Kekuatan Kabel Menahan Gaya Tarikan." },
  { "en": "Penting Untuk Instalasi Kabel Apa?", "id": "Kabel Udara Dan Kabel Bawah Laut." },
  { "en": "Apa Itu Elastisitas Kabel?", "id": "Kemampuan Kabel Kembali Ke Bentuk Semula." },
  { "en": "Penting Untuk Jenis Kabel Apa?", "id": "Kabel Fleksibel Dan Kabel Robotik." },
  { "en": "Apa Itu Koefisien Ekspansi Termal?", "id": "Ukuran Perubahan Panjang Akibat Suhu." },
  { "en": "Kabel Di Udara Akan Memuai Atau Menyusut?", "id": "Memuai Saat Panas Menyusut Saat Dingin." },
  { "en": "Ini Mempengaruhi Ketegangan Kabel?", "id": "Ya Sangat Penting Untuk Desain Kabel Udara." },
  { "en": "Apa Itu Ketahanan Abrasi?", "id": "Kemampuan Jaket Kabel Menahan Gesekan." },
  { "en": "Apa Itu Ketahanan Kimia?", "id": "Kemampuan Jaket Kabel Menahan Bahan Kimia." },
  { "en": "Penting Untuk Lingkungan Apa?", "id": "Industri Kimia Dan Pabrik." },
  { "en": "Apa Itu Ketahanan Kelembaban?", "id": "Kemampuan Kabel Mencegah Masuknya Air." },
  { "en": "Apa Itu Water Blocking Tape?", "id": "Pita Yang Mengembang Saat Terkena Air." },
  { "en": "Tujuannya Untuk Mencegah?", "id": "Air Merambat Di Sepanjang Kabel." },
  { "en": "Apa Itu Kabel Filled?", "id": "Kabel Diisi Dengan Gel Anti Air." },
  { "en": "Digunakan Pada Kabel Apa?", "id": "Kabel Komunikasi Bawah Tanah." },
  { "en": "Apa Itu Kode Kategori Pada Kabel LAN?", "id": "Menunjukkan Kinerja Dan Bandwidth Kabel." },
  { "en": "Kategori Lebih Tinggi Berarti?", "id": "Performa Lebih Baik Dan Frekuensi Lebih Tinggi." },
  { "en": "Apa Itu Kabel Cross-Over?", "id": "Kabel Jaringan Untuk Menghubungkan Dua Perangkat Sejenis." },
  { "en": "Apa Beda Dengan Kabel Straight-Through?", "id": "Urutan Kawat Di Salah Satu Ujung Berbeda." },
  { "en": "Apa Itu Auto MDI-X?", "id": "Fitur Jaringan Yang Otomatis Mendeteksi Kabel." },
  { "en": "Membuat Kabel Cross-Over Tidak Lagi Diperlukan?", "id": "Benar Untuk Perangkat Jaringan Modern." },
  { "en": "Apa Itu Kabel Coaxial Twin?", "id": "Sama Dengan Kabel Twinax." },
  { "en": "Apa Itu Kabel Quad-Pair?", "id": "Kabel UTP Dengan Empat Pasang Kawat." },
  { "en": "Berapa Jumlah Total Konduktornya?", "id": "Delapan Konduktor." },
  { "en": "Digunakan Untuk Apa?", "id": "Jaringan Ethernet Modern." },
  { "en": "Apa Itu Kabel Siamese?", "id": "Dua Kabel Berbeda Yang Menempel." },
  { "en": "Contoh Kabel Siamese?", "id": "Kabel Coaxial Dan Kabel Listrik." },
  { "en": "Digunakan Untuk Instalasi Apa?", "id": "Kamera Keamanan CCTV." },
  { "en": "Apa Itu Insertion Loss?", "id": "Pelemahan Sinyal Saat Melewati Konektor." },
  { "en": "Nilai Yang Baik Adalah?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Return Loss?", "id": "Ukuran Sinyal Yang Dipantulkan Oleh Konektor." },
  { "en": "Nilai Yang Baik Adalah?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Kabel Patch?", "id": "Kabel Pendek Untuk Koneksi Di Patch Panel." },
  { "en": "Apa Itu Kabel Backbone?", "id": "Kabel Utama Berkapasitas Tinggi Antar Gedung." },
  { "en": "Biasanya Menggunakan Kabel Apa?", "id": "Serat Optik." },
  { "en": "Apa Itu Kabel Last Mile?", "id": "Kabel Dari Jaringan Utama Ke Pelanggan." },
  { "en": "Apa Itu Tembaga Annealed?", "id": "Tembaga Yang Dipanaskan Untuk Menjadi Lunak." },
  { "en": "Tujuannya Untuk Meningkatkan?", "id": "Fleksibilitas Dan Kemudahan Instalasi." },
  { "en": "Apa Itu Peringkat UL?", "id": "Sertifikasi Keamanan Produk Dari Underwriters Laboratories." },
  { "en": "Peringkat UL Penting Di Mana?", "id": "Terutama Di Pasar Amerika Utara." },
  { "en": "Apa Itu Tanda CE?", "id": "Sertifikasi Produk Untuk Pasar Uni Eropa." },
  { "en": "Apa Itu RoHS?", "id": "Restriction of Hazardous Substances." },
  { "en": "Artinya Kabel Tidak Mengandung?", "id": "Bahan Berbahaya Seperti Timbal Dan Merkuri." },
  { "en": "Apa Itu Tang Ampere (Clamp Meter)?", "id": "Mengukur Arus Listrik Tanpa Kontak Langsung." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Mengukur Medan Magnet Di Sekitar Kabel." },
  { "en": "Apa Itu Kabel Koaksial Radiating?", "id": "Sama Dengan Kabel Koaksial Leaky." },
  { "en": "Apa Itu Kabel Tahan Gigitan Pengerat?", "id": "Kabel Dengan Lapisan Pelindung Khusus." },
  { "en": "Apa Itu Kabel Lay-Flat?", "id": "Kabel Datar Untuk Pompa Submersible." },
  { "en": "Apa Itu Umbilical Cable?", "id": "Kabel Kompleks Untuk Robot Bawah Air." },
  { "en": "Membawa Apa Saja?", "id": "Daya Sinyal Dan Kadang Fluida." },
  { "en": "Apa Itu Titik Lebur Konduktor?", "id": "Suhu Dimana Konduktor Meleleh." },
  { "en": "Penting Untuk Analisis Apa?", "id": "Analisis Kondisi Hubungan Singkat." },
  { "en": "Apa Itu Arus Hubungan Singkat?", "id": "Arus Sangat Besar Saat Terjadi Korsleting." },
  { "en": "Kabel Harus Mampu Menahan?", "id": "Gaya Mekanis Dan Panas Arus Hubung Singkat." },
  { "en": "Apa Itu Kabel Berisolasi Udara?", "id": "Konduktor Telanjang Yang Digantung Di Udara." },
  { "en": "Isolatornya Adalah?", "id": "Udara Di Sekitarnya." },
  { "en": "Contohnya Adalah?", "id": "Saluran Udara Tegangan Ekstra Tinggi (SUTET)." },
  { "en": "Apa Itu Spacer Kabel?", "id": "Menjaga Jarak Antar Konduktor Udara." },
  { "en": "Apa Itu Dempul Isolasi (Putty)?", "id": "Bahan Lunak Untuk Mengisi Celah Isolasi." },
  { "en": "Apa Itu Pita Isolasi (Electrical Tape)?", "id": "Pita Perekat Berbahan Vinyl Untuk Isolasi." },
  { "en": "Apa Itu Konektor IDC?", "id": "Insulation Displacement Connector." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Menembus Isolasi Tanpa Perlu Dikupas." },
  { "en": "Digunakan Pada Kabel Apa?", "id": "Kabel Telepon Dan Kabel Pita." },
  { "en": "Apa Itu Kabel Senjata (Hook-up Wire)?", "id": "Kabel Inti Tunggal Untuk Rangkaian Internal." },
  { "en": "Apa Itu Kawat Bus?", "id": "Konduktor Telanjang Untuk Titik Ground Bersama." },
  { "en": "Apa Itu Kabel Audio Interconnect?", "id": "Kabel Untuk Menghubungkan Komponen Audio." },
  { "en": "Contoh Kabel Interconnect?", "id": "Kabel RCA Atau Kabel XLR." },
  { "en": "Apa Itu Kabel Listrik AC?", "id": "Kabel Dari Stop Kontak Ke Perangkat." },
  { "en": "Apa Itu Colokan (Plug)?", "id": "Konektor Jantan Di Ujung Kabel AC." },
  { "en": "Apa Itu Stop Kontak (Socket)?", "id": "Konektor Betina Di Dinding." },
  { "en": "Apa Itu Kabel Ekstensi?", "id": "Kabel Untuk Memperpanjang Jangkauan Kabel Listrik." },
  { "en": "Harus Digunakan Secara Permanen?", "id": "Tidak Hanya Untuk Penggunaan Sementara." },
  { "en": "Kenapa Tidak Boleh Permanen?", "id": "Resiko Kelebihan Beban Dan Kebakaran." },
  { "en": "Apa Itu Kabel Bertegangan?", "id": "Kabel Yang Sedang Mengalirkan Arus Listrik." },
  { "en": "Sangat Berbahaya Untuk Disentuh?", "id": "Ya Bisa Menyebabkan Kematian." },
  { "en": "Alat Untuk Mengecek Tegangan?", "id": "Test Pen Atau Voltmeter." },
  { "en": "Apa Itu Kabel Koaksial Terisi Busa?", "id": "Dielektriknya Berupa Busa Polietilen." },
  { "en": "Keuntungannya Apa?", "id": "Lebih Ringan Dan Atenuasi Rendah." },
  { "en": "Apa Itu Kabel Tahan Radiasi?", "id": "Kabel Untuk Digunakan Di Lingkungan Nuklir." },
  { "en": "Isolasinya Terbuat Dari?", "id": "Bahan Khusus Yang Tahan Radiasi." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Tegangan Ujung Penerima Lebih Tinggi Dari Pengirim." },
  { "en": "Terjadi Pada Saluran Apa?", "id": "Saluran Transmisi Panjang Tanpa Beban." },
  { "en": "Penyebabnya Adalah?", "id": "Efek Kapasitansi Saluran." },
  { "en": "Apa Itu Reaktor Shunt?", "id": "Induktor Untuk Mengkompensasi Efek Kapasitansi." },
  { "en": "Apa Itu Kapasitor Seri?", "id": "Kapasitor Untuk Mengkompensasi Efek Induktansi." },
  { "en": "Tujuannya Untuk Meningkatkan?", "id": "Kapasitas Transfer Daya Saluran." },
  { "en": "Apa Itu Kode IPXX?", "id": "X Berarti Tidak Diuji Untuk Kategori Itu." },
  { "en": "Contoh IPX7 Berarti?", "id": "Tahan Terendam Air Tidak Diuji Debu." },
  { "en": "Apa Itu Konduktor Berkas (Bundled Conductor)?", "id": "Beberapa Konduktor Per Fasa Di Saluran Udara." },
  { "en": "Tujuannya Mengurangi Apa?", "id": "Corona Discharge Dan Induktansi Saluran." },
  { "en": "Apa Itu Kabel Zero Halogen?", "id": "Sama Dengan Kabel LSZH." },
  { "en": "Apa Itu Kabel Self-Supporting?", "id": "Kabel Udara Tanpa Membutuhkan Kawat Penggantung." },
  { "en": "Apa Peran Utama Konduktor?", "id": "Menghantarkan Arus Listrik Dengan Mudah." },
  { "en": "Apa Peran Utama Isolator?", "id": "Mencegah Kebocoran Arus Listrik." },
  { "en": "Apa Peran Utama Jaket Kabel?", "id": "Memberikan Perlindungan Terhadap Lingkungan." },
  { "en": "Apa Peran Utama Armor Kabel?", "id": "Memberikan Perlindungan Terhadap Benturan Mekanis." },
  { "en": "Apa Peran Utama Pelindung (Shield)?", "id": "Melindungi Sinyal Dari Gangguan EMI." },
  { "en": "Apa Beda Kabel Daya Dan Sinyal?", "id": "Daya Bawa Energi Sinyal Bawa Informasi." },
  { "en": "Apa Beda Kabel Tembaga Dan Optik?", "id": "Tembaga Bawa Elektron Optik Bawa Foton." },
  { "en": "Apa Beda Konektor Dan Terminal?", "id": "Konektor Bisa Dilepas Terminal Biasanya Tetap." },
  { "en": "Apa Itu Kabel Fleksibel?", "id": "Didesain Untuk Dibengkokkan Berulang Kali." },
  { "en": "Apa Itu Kabel Kaku?", "id": "Didesain Untuk Instalasi Tetap." },
  { "en": "Konduktor Fleksibel Berjenis?", "id": "Serabut." },
  { "en": "Konduktor Kaku Berjenis?", "id": "Pejal." },
  { "en": "Apa Itu Standar NEC?", "id": "Standar Kelistrikan Amerika Utara." },
  { "en": "Apa Itu Standar IEC?", "id": "Standar Kelistrikan Internasional." },
  { "en": "Apa Itu Standar SNI?", "id": "Standar Nasional Indonesia." },
  { "en": "Kenapa Standar Itu Penting?", "id": "Untuk Keselamatan Kompatibilitas Dan Kualitas." },
  { "en": "Apa Itu Kabel Patch Cord?", "id": "Kabel Pendek Fleksibel Dengan Konektor." },
  { "en": "Apa Itu Kabel Bulk?", "id": "Kabel Panjang Dalam Gulungan Tanpa Konektor." },
  { "en": "Apa Itu Peringkat Api (Fire Rating)?", "id": "Kemampuan Kabel Menahan Penyebaran Api." },
  { "en": "Apa Itu Kabel Riser?", "id": "Kabel Untuk Instalasi Vertikal Antar Lantai." },
  { "en": "Peringkat Apinya Harus?", "id": "Mencegah Api Merambat Ke Atas." },
  { "en": "Apa Itu Kabel Plenum?", "id": "Kabel Untuk Instalasi Di Ruang Ventilasi." },
  { "en": "Peringkat Apinya Harus?", "id": "Rendah Asap Dan Tidak Beracun." },
  { "en": "Apa Itu Konduktor Bare?", "id": "Konduktor Telanjang Tanpa Isolasi." },
  { "en": "Contoh Konduktor Bare?", "id": "Kabel Grounding Atau Kawat Penangkal Petir." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Dengan Level Tegangan Diskrit." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Dengan Level Tegangan Kontinu." },
  { "en": "Kabel Ethernet Membawa Sinyal?", "id": "Digital." },
  { "en": "Kabel Audio RCA Membawa Sinyal?", "id": "Analog." },
  { "en": "Apa Itu Pembumian (Grounding)?", "id": "Menghubungkan Sistem Ke Bumi." },
  { "en": "Apa Tujuannya?", "id": "Keselamatan Dan Referensi Tegangan Nol." },
  { "en": "Apa Itu Ikatan (Bonding)?", "id": "Menghubungkan Semua Bagian Logam Non-Listrik." },
  { "en": "Tujuannya Untuk Menyamakan?", "id": "Potensial Listrik Mencegah Kejutan." },
  { "en": "Apa Itu Kabel Coaxial Quad-Shield?", "id": "Coaxial Dengan Empat Lapisan Pelindung." },
  { "en": "Memberikan Perlindungan Terbaik?", "id": "Ya Terhadap Gangguan Frekuensi Tinggi." },
  { "en": "Apa Itu Kabel Speaker OFC?", "id": "Oxygen-Free Copper." },
  { "en": "Apa Itu Tahanan DC?", "id": "Resistansi Kabel Terhadap Arus DC." },
  { "en": "Apa Itu Tahanan AC?", "id": "Resistansi Kabel Terhadap Arus AC." },
  { "en": "Mana Yang Selalu Lebih Tinggi?", "id": "Tahanan AC Karena Skin Effect." },
  { "en": "Apa Itu Kode Warna Pasangan Pilin?", "id": "Standar Untuk Kabel Jaringan (TIA/EIA 568)." },
  { "en": "Contoh Pasangan Warna?", "id": "Biru/Putih-Biru Oranye/Putih-Oranye." },
  { "en": "Apa Itu Kabel Straight-Through?", "id": "Urutan Warna Sama Di Kedua Ujung." },
  { "en": "Digunakan Untuk Menghubungkan?", "id": "Perangkat Berbeda (PC Ke Switch)." },
  { "en": "Apa Itu Kabel Cross-Over?", "id": "Urutan Kirim Terima Dibalik." },
  { "en": "Digunakan Untuk Menghubungkan?", "id": "Perangkat Sejenis (PC Ke PC)." },
  { "en": "Apa Itu Penuaan Kabel?", "id": "Degradasi Kinerja Kabel Seiring Waktu." },
  { "en": "Faktor Utama Penuaan?", "id": "Panas Oksigen Sinar UV." },
  { "en": "Isolasi Menjadi Rapuh?", "id": "Ya Dan Bisa Retak." },
  { "en": "Apa Itu Kabel Konduktor Kompak?", "id": "Konduktor Serabut Yang Dipadatkan." },
  { "en": "Tujuannya Mengurangi Diameter?", "id": "Ya Tanpa Mengurangi Luas Penampang." },
  { "en": "Apa Itu Kabel Self-Healing?", "id": "Kabel Dengan Kemampuan Memperbaiki Diri." },
  { "en": "Bagaimana Caranya?", "id": "Material Polimer Khusus Mengalir Menutup Retakan." },
  { "en": "Apa Itu Kabel Festoon?", "id": "Kabel Datar Untuk Sistem Derek Bergerak." },
  { "en": "Apa Itu Kabel Umbilical?", "id": "Kabel Multi-Fungsi Untuk Aplikasi Bawah Laut." },
  { "en": "Apa Itu Gelombang Mikro (Microwave)?", "id": "Sinyal Frekuensi Sangat Tinggi." },
  { "en": "Kabel Untuk Gelombang Mikro?", "id": "Kabel Coaxial Kualitas Sangat Tinggi." },
  { "en": "Apa Itu Waveguide?", "id": "Pipa Logam Berongga Untuk Saluran Microwave." },
  { "en": "Bisa Dianggap Kabel?", "id": "Ya Kabel Tanpa Konduktor Pusat." },
  { "en": "Apa Itu Kabel Tahan Temperatur Ekstrem?", "id": "Menggunakan Isolasi Seperti Mika Atau Keramik." },
  { "en": "Apa Itu Stripline?", "id": "Jalur Transmisi Datar Di Papan Sirkuit Cetak." },
  { "en": "Apa Itu Microstrip?", "id": "Jenis Lain Jalur Transmisi Di PCB." },
  { "en": "Apa Itu Konektor N-Type?", "id": "Konektor Coaxial Kinerja Tinggi." },
  { "en": "Apa Itu Konektor SMA?", "id": "Konektor Coaxial Kecil Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu Splicing?", "id": "Metode Penyambungan Kabel." },
  { "en": "Apa Itu Terminasi?", "id": "Proses Pemasangan Konektor Di Ujung Kabel." },
  { "en": "Terminasi Yang Buruk Menyebabkan?", "id": "Pantulan Sinyal Dan Kinerja Buruk." },
  { "en": "Apa Itu Pengujian Kabel Jaringan?", "id": "Memverifikasi Peta Kawat Panjang Dan Kinerja." },
  { "en": "Apa Itu Peta Kawat (Wire Map)?", "id": "Memeriksa Apakah Semua Kawat Terhubung Benar." },
  { "en": "Apa Itu NEXT?", "id": "Near-End Crosstalk." },
  { "en": "Mengukur Gangguan Di Ujung?", "id": "Ujung Yang Sama Dengan Sumber Sinyal." },
  { "en": "Apa Itu FEXT?", "id": "Far-End Crosstalk." },
  { "en": "Mengukur Gangguan Di Ujung?", "id": "Ujung Yang Berlawanan Dari Sumber Sinyal." },
  { "en": "Apa Itu Return Loss?", "id": "Ukuran Pantulan Sinyal Akibat Mismatch." },
  { "en": "Nilai Return Loss Yang Baik?", "id": "Besar (Dalam dB)." },
  { "en": "Apa Itu Insertion Loss?", "id": "Pelemahan Sinyal Saat Melewati Kabel." },
  { "en": "Nilai Insertion Loss Yang Baik?", "id": "Kecil (Dalam dB)." },
  { "en": "Apa Itu Propagation Delay?", "id": "Waktu Sinyal Merambat Dari Ujung Ke Ujung." },
  { "en": "Apa Itu Delay Skew?", "id": "Perbedaan Waktu Tiba Antar Pasangan Kawat." },
  { "en": "Delay Skew Tinggi Menyebabkan?", "id": "Error Bit Pada Kecepatan Tinggi." },
  { "en": "Apa Itu Kabel Plenum Rated?", "id": "Kabel Yang Aman Untuk Ruang Ventilasi." },
  { "en": "Apa Itu Kabel Riser Rated?", "id": "Kabel Yang Aman Untuk Jalur Vertikal." },
  { "en": "Apa Itu Kabel CM Rated?", "id": "Kabel Untuk Penggunaan Umum." },
  { "en": "Standar Ini Berasal Dari?", "id": "NEC (National Electrical Code) Amerika." },
  { "en": "Apa Itu Kabel Koaksial 50 Ohm?", "id": "Umumnya Untuk Jaringan Data Dan RF." },
  { "en": "Apa Itu Kabel Koaksial 75 Ohm?", "id": "Umumnya Untuk Video Dan TV Kabel." },
  { "en": "Apa Itu Kabel Audio Balanced?", "id": "Menggunakan Sinyal Diferensial Untuk Menolak Noise." },
  { "en": "Apa Itu Kabel Audio Unbalanced?", "id": "Lebih Rentan Terhadap Gangguan Noise." },
  { "en": "Apa Itu Konduktor Tembaga Murni?", "id": "Disebut Juga Bare Copper." },
  { "en": "Apa Itu Konduktor CCA?", "id": "Copper Clad Aluminum." },
  { "en": "Artinya Aluminium Dilapisi?", "id": "Lapisan Tipis Tembaga." },
  { "en": "Performanya Dibanding Tembaga Murni?", "id": "Lebih Buruk." },
  { "en": "Harganya Dibanding Tembaga Murni?", "id": "Lebih Murah." },
  { "en": "Apa Itu Kabel Drop Wire?", "id": "Kabel Dari Tiang Telepon Ke Rumah." },
  { "en": "Apa Itu Kabel Feeder?", "id": "Kabel Utama Yang Menyuplai Area Luas." },
  { "en": "Apa Itu Kabel Distribusi?", "id": "Kabel Yang Bercabang Dari Kabel Feeder." },
  { "en": "Apa Itu Sambungan Layanan?", "id": "Kabel Final Dari Jaringan Ke Pelanggan." },
  { "en": "Apa Beda Kabel Dan Kawat?", "id": "Kabel Kumpulan Kawat Kawat Satu Konduktor." },
  { "en": "Apa Beda Tegangan Dan Arus?", "id": "Tegangan Dorongan Arus Adalah Aliran." },
  { "en": "Apa Beda Resistansi Dan Impedansi?", "id": "Resistansi DC Impedansi AC." },
  { "en": "Apa Beda Shield Dan Armor?", "id": "Shield Lindungi Sinyal Armor Lindungi Fisik." },
  { "en": "Apa Beda Konduit Dan Trunking?", "id": "Konduit Pipa Bulat Trunking Kotak Persegi." },
  { "en": "Apa Itu Panas Joule?", "id": "Panas Akibat Arus Melewati Resistansi." },
  { "en": "Ini Adalah Prinsip Dasar?", "id": "Pemanasan Kabel Listrik." },
  { "en": "Apa Itu Kabel Berpendingin Cairan?", "id": "Kabel Daya Tinggi Dengan Cairan Pendingin." },
  { "en": "Apa Itu Kabel Busbar?", "id": "Batang Konduktor Kaku Untuk Distribusi Daya." },
  { "en": "Apa Itu Kabel Antistatis?", "id": "Kabel Didesain Mencegah Listrik Statis." },
  { "en": "Penting Di Lingkungan Apa?", "id": "Mudah Terbakar Atau Penuh Elektronik Sensitif." },
  { "en": "Apa Itu Tahanan Tanah?", "id": "Resistansi Antara Elektroda Tanah Dan Bumi." },
  { "en": "Nilai Yang Baik Adalah?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Sistem Pembumian TT?", "id": "Titik Netral Dan Bodi Terpisah." },
  { "en": "Apa Itu Sistem Pembumian TN?", "id": "Titik Netral Dan Bodi Terhubung." },
  { "en": "Sistem Mana Yang Umum Di Perumahan?", "id": "Sistem TN." },
  { "en": "Apa Itu Kode IP Angka Ketiga?", "id": "Tidak Ada Hanya Dua Angka." },
  { "en": "Apa Itu Kabel Koaksial RG-11?", "id": "Coaxial 75 Ohm Dengan Atenuasi Rendah." },
  { "en": "Digunakan Untuk Jarak?", "id": "Lebih Jauh Dari Kabel RG-6." },
  { "en": "Apa Itu Kabel Fire Alarm?", "id": "Kabel Khusus Untuk Sirkuit Alarm Kebakaran." },
  { "en": "Warnanya Biasanya Apa?", "id": "Merah." },
  { "en": "Harus Tahan Api?", "id": "Ya Untuk Keandalan Sistem." },
  { "en": "Apa Itu Kabel Lift?", "id": "Kabel Pita Datar Fleksibel." },
  { "en": "Apa Itu Kabel Kapal (Marine Cable)?", "id": "Kabel Tahan Korosi Air Garam." },
  { "en": "Apa Itu Kabel Pertambangan?", "id": "Kabel Sangat Kuat Dan Tahan Abrasi." },
  { "en": "Apa Itu Kabel Bandara?", "id": "Kabel Untuk Pencahayaan Landasan Pacu." },
  { "en": "Tegangannya Biasanya Tinggi?", "id": "Ya Untuk Mengurangi Jatuh Tegangan." },
  { "en": "Apa Itu Tahanan Gelombang?", "id": "Sama Dengan Impedansi Karakteristik." },
  { "en": "Apa Itu Kecepatan Grup?", "id": "Kecepatan Perambatan Energi Atau Informasi." },
  { "en": "Apa Itu Kecepatan Fasa?", "id": "Kecepatan Perambatan Fasa Gelombang." },
  { "en": "Di Media Dispersif Keduanya?", "id": "Tidak Sama." },
  { "en": "Apa Itu Efek Memori Dielektrik?", "id": "Isolator Menyimpan Sebagian Muatan." },
  { "en": "Apa Itu Standar UL?", "id": "Standar Keamanan Dari Underwriters Laboratories." },
  { "en": "Apa Itu Standar CSA?", "id": "Standar Keamanan Kanada." },
  { "en": "Apa Itu Standar VDE?", "id": "Standar Kelistrikan Jerman." },
  { "en": "Apa Itu Tanda HAR?", "id": "Tanda Harmonisasi Kabel Eropa." },
  { "en": "Apa Itu Penuaan Dipercepat?", "id": "Uji Laboratorium Untuk Mensimulasikan Umur Kabel." },
  { "en": "Bagaimana Caranya?", "id": "Meningkatkan Suhu Dan Paparan Oksigen." },
  { "en": "Apa Itu Uji Tekukan Dingin (Cold Bend)?", "id": "Menguji Fleksibilitas Kabel Di Suhu Rendah." },
  { "en": "Apa Itu Uji Ketahanan Sinar Matahari?", "id": "Menguji Degradasi Jaket Akibat Sinar UV." },
  { "en": "Apa Itu Gel Filled Cable?", "id": "Kabel Diisi Gel Untuk Mencegah Air." },
  { "en": "Apa Itu Dry Core Cable?", "id": "Kabel Udara Bertekanan Untuk Mencegah Lembab." },
  { "en": "Apa Itu Kabel Koaksial Bocor?", "id": "Sengaja Dibuat Berlubang Untuk Jadi Antena." },
  { "en": "Apa Itu Skin Depth?", "id": "Kedalaman Penetrasi Arus AC Di Konduktor." },
  { "en": "Frekuensi Naik Skin Depth?", "id": "Menurun." },
  { "en": "Apa Itu Kabel Komunikasi Pasangan?", "id": "Sama Dengan Kabel Twisted Pair." },
  { "en": "Apa Itu Kabel Quad?", "id": "Empat Konduktor Dipilin Bersama." },
  { "en": "Digunakan Dalam Sistem Telepon?", "id": "Ya Sistem Telepon Lama." },
  { "en": "Apa Itu Kabel Video Komponen?", "id": "Mengirim Sinyal Video Terpisah (Y Pb Pr)." },
  { "en": "Kualitasnya Lebih Baik Dari Komposit?", "id": "Ya Karena Sinyal Warna Kecerahan Terpisah." },
  { "en": "Apa Itu Kabel S-Video?", "id": "Memisahkan Sinyal Kecerahan Dan Warna." },
  { "en": "Apa Itu Kabel VGA?", "id": "Kabel Analog Untuk Monitor Komputer." },
  { "en": "Apa Itu Kabel DVI?", "id": "Bisa Membawa Sinyal Video Analog Digital." },
  { "en": "Apa Itu DisplayPort?", "id": "Standar Antarmuka Video Digital Modern." },
  { "en": "Apa Itu Kabel Thunderbolt?", "id": "Antarmuka Berkecepatan Sangat Tinggi." },
  { "en": "Thunderbolt Membawa Sinyal Apa?", "id": "Data PCIe Dan Video DisplayPort." },
  { "en": "Apa Itu Kabel SATA?", "id": "Kabel Untuk Menghubungkan Hard Drive Internal." },
  { "en": "Apa Itu Kabel SAS?", "id": "Versi Perusahaan Dari Kabel SATA." },
  { "en": "Apa Itu Kabel FireWire?", "id": "Antarmuka Serial Kecepatan Tinggi Lama." },
  { "en": "Apa Itu Kabel Coaxial Heliax?", "id": "Kabel Coaxial Kaku Dengan Atenuasi Sangat Rendah." },
  { "en": "Apa Itu Kabel Tahan Panas?", "id": "Kabel Dengan Isolasi Tahan Suhu Tinggi." },
  { "en": "Contoh Isolasi Tahan Panas?", "id": "Teflon Silikon Mika." },
  { "en": "Apa Itu Kabel Cryogenic?", "id": "Kabel Untuk Suhu Sangat Dingin." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Prinsip Kerja Termokopel." },
  { "en": "Apa Itu Efek Peltier?", "id": "Kebalikan Dari Efek Seebeck." },
  { "en": "Apa Itu Efek Thomson?", "id": "Konsep Termodinamika Terkait." },
  { "en": "Apa Itu Kabel Tahan Potong?", "id": "Kabel Dengan Lapisan Tahan Benda Tajam." },
  { "en": "Apa Itu Pulling Eye?", "id": "Alat Bantu Untuk Menarik Kabel Berat." },
  { "en": "Apa Itu Tensioner Kabel?", "id": "Mesin Untuk Menjaga Ketegangan Kabel Udara." },
  { "en": "Apa Itu Sag Kabel?", "id": "Lengkungan Kabel Udara Antara Dua Tiang." },
  { "en": "Sag Terlalu Kecil Menyebabkan?", "id": "Ketegangan Mekanis Terlalu Besar." },
  { "en": "Sag Terlalu Besar Menyebabkan?", "id": "Kabel Terlalu Rendah Berbahaya." },
  { "en": "Apa Itu Galloping Kabel?", "id": "Osilasi Frekuensi Rendah Kabel Udara." },
  { "en": "Penyebabnya Adalah?", "id": "Angin Dan Tumpukan Es." },
  { "en": "Apa Itu Peredam Getaran (Vibration Damper)?", "id": "Alat Untuk Meredam Getaran Kabel Udara." },
  { "en": "Bentuknya Seperti Apa?", "id": "Seperti Barbel Kecil Di Dekat Tiang." },
  { "en": "Apa Itu Kabel Berisolasi Gas (GIL)?", "id": "Saluran Transmisi Dengan Isolasi Gas SF6." },
  { "en": "Keuntungannya Apa?", "id": "Kapasitas Daya Sangat Besar." },
  { "en": "Apa Itu Kabel Berinti Aluminium Berbaju Tembaga?", "id": "CCA (Copper Clad Aluminum)." },
  { "en": "Konduktivitasnya Lebih Baik Dari Aluminium?", "id": "Ya Karena Efek Kulit." },
  { "en": "Apa Itu Kabel CCAW?", "id": "Copper Clad Aluminum and Winding." },
  { "en": "Apa Itu Kabel CCS?", "id": "Copper Clad Steel." },
  { "en": "Kekuatan Mekanisnya Lebih Baik?", "id": "Ya Karena Inti Baja." },
  { "en": "Konduktivitasnya Lebih Buruk?", "id": "Ya Dari Tembaga Murni." },
  { "en": "Digunakan Pada Kabel Apa?", "id": "Kabel Coaxial (Inti Pusat)." },
  { "en": "Apa Itu Pembumian Satu Titik?", "id": "Pelindung Kabel Ditanahkan Hanya Di Satu Ujung." },
  { "en": "Tujuannya Mencegah Apa?", "id": "Arus Sirkulasi Di Lapisan Pelindung." },
  { "en": "Apa Itu Ground Loop?", "id": "Arus Tak Diinginkan Akibat Beda Potensial Tanah." },
  { "en": "Menyebabkan Gangguan Apa?", "id": "Dengung (Hum) Pada Sistem Audio." },
  { "en": "Isolator Galvanik Berguna Untuk?", "id": "Memutus Ground Loop." },
  { "en": "Apa Itu Kabel Zero-Bezel?", "id": "Konsep Desain Bukan Jenis Kabel." },
  { "en": "Apa Itu Konduktor Transposisi?", "id": "Posisi Kawat Dalam Bundel Diubah Periodik." },
  { "en": "Tujuannya Untuk Menyeimbangkan?", "id": "Impedansi Antar Fasa." },
  { "en": "Apa Itu Kabel Inti Bersektor?", "id": "Konduktor Berbentuk Sektor Bukan Lingkaran." },
  { "en": "Tujuannya Untuk Membuat Kabel?", "id": "Lebih Kompak Dan Bulat." },
  { "en": "Apa Beda Kabel Serabut Dan Litz?", "id": "Kawat Litz Setiap Serabutnya Terisolasi." },
  { "en": "Apa Beda Armor Dan Shield?", "id": "Armor Untuk Fisik Shield Untuk Elektrik." },
  { "en": "Apa Beda Jatuh Tegangan Dan Atenuasi?", "id": "Jatuh Tegangan Daya Atenuasi Sinyal." },
  { "en": "Apa Beda KHA Dan Rating Suhu?", "id": "KHA Batas Arus Rating Batas Suhu." },
  { "en": "Apa Beda Konduit Dan Kabel Tray?", "id": "Konduit Pipa Tertutup Tray Rak Terbuka." },
  { "en": "Apa Itu Terminasi Kabel?", "id": "Proses Pemasangan Konektor Atau Lug." },
  { "en": "Apa Itu Splicing Kabel?", "id": "Proses Penyambungan Dua Kabel." },
  { "en": "Terminasi Yang Baik Harus?", "id": "Rendah Resistansi Dan Kuat Mekanis." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Struktur Paling Umum Kabel RF." },
  { "en": "Apa Itu Twisted Pair?", "id": "Struktur Paling Umum Kabel Data." },
  { "en": "Apa Itu Serat Optik?", "id": "Media Paling Cepat Untuk Jarak Jauh." },
  { "en": "Apa Itu Kabel Listrik?", "id": "Media Paling Umum Untuk Distribusi Daya." },
  { "en": "Kenapa Tembaga Menjadi Pilihan Utama?", "id": "Kombinasi Terbaik Konduktivitas Biaya Fleksibilitas." },
  { "en": "Kenapa Aluminium Alternatif Yang Baik?", "id": "Jauh Lebih Ringan Dan Lebih Murah." },
  { "en": "Kelemahan Utama Aluminium?", "id": "Mudah Teroksidasi Dan Kurang Kuat." },
  { "en": "Apa Itu Isolasi Termoplastik?", "id": "Meleleh Saat Dipanaskan (Contoh PVC)." },
  { "en": "Apa Itu Isolasi Termoset?", "id": "Tidak Meleleh Saat Dipanaskan (Contoh XLPE)." },
  { "en": "Mana Yang Lebih Stabil Pada Suhu Tinggi?", "id": "Isolasi Termoset Seperti XLPE." },
  { "en": "Apa Itu PoE?", "id": "Power over Ethernet." },
  { "en": "Artinya Kabel Ethernet Membawa?", "id": "Data Dan Daya Listrik DC." },
  { "en": "Apa Itu Mode Tunggal (Single-Mode)?", "id": "Serat Optik Dengan Satu Jalur Cahaya." },
  { "en": "Apa Itu Mode Ganda (Multi-Mode)?", "id": "Serat Optik Dengan Banyak Jalur Cahaya." },
  { "en": "Dispersi Terjadi Pada Serat?", "id": "Multi-Mode." },
  { "en": "Apa Itu Jaket Kabel?", "id": "Lapisan Terluar Yang Melindungi Kabel." },
  { "en": "Apa Itu Inti Kabel (Core)?", "id": "Bagian Konduktif Yang Membawa Arus." },
  { "en": "Apa Itu Pasangan Kabel (Pair)?", "id": "Dua Konduktor Yang Bekerja Bersama." },
  { "en": "Kabel UTP Terdiri Dari?", "id": "Empat Pasangan Kawat." },
  { "en": "Setiap Pasangan Punya Laju Pilin Berbeda?", "id": "Ya Untuk Mengurangi Crosstalk." },
  { "en": "Apa Itu Near-End Crosstalk (NEXT)?", "id": "Gangguan Diukur Di Ujung Yang Sama." },
  { "en": "Apa Itu Far-End Crosstalk (FEXT)?", "id": "Gangguan Diukur Di Ujung Yang Berlawanan." },
  { "en": "Apa Itu Kode Warna TIA/EIA-568A?", "id": "Salah Satu Standar Pengkabelan Ethernet." },
  { "en": "Apa Itu Kode Warna TIA/EIA-568B?", "id": "Standar Pengkabelan Ethernet Yang Lebih Umum." },
  { "en": "Kabel Straight Menggunakan Standar?", "id": "Sama Di Kedua Ujung (568B)." },
  { "en": "Kabel Cross Menggunakan Standar?", "id": "568A Di Satu Ujung 568B Lainnya." },
  { "en": "Apa Itu Tegangan Breakdown?", "id": "Sama Dengan Tegangan Tembus." },
  { "en": "Apa Itu Resistivitas Volume?", "id": "Ukuran Tahanan Internal Isolator." },
  { "en": "Apa Itu Resistivitas Permukaan?", "id": "Ukuran Tahanan Di Permukaan Isolator." },
  { "en": "Kelembaban Mempengaruhi Resistivitas Mana?", "id": "Resistivitas Permukaan Secara Signifikan." },
  { "en": "Apa Itu Pohon Air (Water Treeing)?", "id": "Jalur Degradasi Dalam Isolasi Kabel." },
  { "en": "Penyebabnya Adalah?", "id": "Kelembaban Dan Stres Medan Listrik." },
  { "en": "Bisa Menyebabkan Kegagalan Kabel?", "id": "Ya Dalam Jangka Panjang." },
  { "en": "Apa Itu Pohon Listrik (Electrical Treeing)?", "id": "Jalur Kegagalan Akibat Pelepasan Sebagian." },
  { "en": "Ini Proses Yang Cepat Atau Lambat?", "id": "Sangat Cepat Menuju Kegagalan Total." },
  { "en": "Apa Itu Partial Discharge (PD)?", "id": "Pelepasan Sebagian Muatan Listrik Di Isolasi." },
  { "en": "Pengujian PD Berguna Untuk?", "id": "Mendeteksi Cacat Produksi Kabel." },
  { "en": "Apa Itu Kabel Coaxial Foam?", "id": "Dielektriknya Berupa Busa." },
  { "en": "Tujuannya Untuk Menurunkan?", "id": "Konstanta Dielektrik Dan Atenuasi." },
  { "en": "Apa Itu Stripline?", "id": "Jalur Transmisi Datar Di Antara Dua Lapisan Ground." },
  { "en": "Apa Itu Microstrip?", "id": "Jalur Transmisi Datar Di Atas Satu Lapisan Ground." },
  { "en": "Keduanya Adalah Bentuk?", "id": "Jalur Transmisi Planar Di PCB." },
  { "en": "Apa Itu Kabel Antena Leaky Feeder?", "id": "Sama Dengan Kabel Koaksial Bocor." },
  { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Beberapa Kawat Disusun Berdampingan Dalam Pita." },
  { "en": "Apa Itu Konduktor Terdampar Konsentris?", "id": "Serabut Dipilin Dalam Lapisan Konsentris." },
  { "en": "Apa Itu Konduktor Terdampar Berkas?", "id": "Beberapa Grup Serabut Dipilin Bersama." },
  { "en": "Apa Itu Stand Off (Penyangga)?", "id": "Isolator Untuk Menjauhkan Kabel Dari Struktur." },
  { "en": "Apa Itu Corona Ring?", "id": "Cincin Logam Untuk Mengontrol Medan Listrik." },
  { "en": "Digunakan Pada Terminasi Kabel?", "id": "Kabel Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Stress Cone?", "id": "Komponen Terminasi Untuk Mengurangi Stres Listrik." },
  { "en": "Apa Itu Kabel Messenger?", "id": "Kawat Penopang Dalam Kabel Udara." },
  { "en": "Apa Itu Kabel Figure-8?", "id": "Kabel Dengan Messenger Menyatu." },
  { "en": "Apa Itu Kabel Tahan Hidrokarbon?", "id": "Kabel Untuk Industri Minyak Dan Gas." },
  { "en": "Apa Itu Pengujian Siklus Panas?", "id": "Menguji Perilaku Kabel Di Bawah Beban Panas." },
  { "en": "Apa Itu Kabel Kompensasi Termokopel?", "id": "Kabel Ekstensi Untuk Sensor Termokopel." },
  { "en": "Bahannya Harus Mirip?", "id": "Ya Dengan Bahan Termokopel Asli." },
  { "en": "Apa Itu Peringkat Api FT4?", "id": "Standar Uji Bakar Vertikal." },
  { "en": "Apa Itu Peringkat Api FT6?", "id": "Standar Uji Bakar Horizontal Yang Lebih Ketat." },
  { "en": "Apa Itu Kabel Shielded Secara Individual?", "id": "Setiap Pasangan Kawat Punya Shield Sendiri." },
  { "en": "Memberikan Perlindungan Crosstalk Terbaik?", "id": "Ya." },
  { "en": "Apa Itu Kabel Koaksial Super-Fleksibel?", "id": "Konduktor Luarnya Berbentuk Gelombang." },
  { "en": "Apa Itu Konektor Push-On?", "id": "Konektor Yang Terhubung Tanpa Diputar." },
  { "en": "Apa Itu Konektor Threaded?", "id": "Konektor Yang Terhubung Dengan Ulir." },
  { "en": "Mana Yang Lebih Aman?", "id": "Konektor Threaded." },
  { "en": "Apa Itu Kabel Berpendingin Air?", "id": "Kabel Daya Ekstrem Dengan Air Pendingin." },
  { "en": "Apa Itu Kabel Cryogenic?", "id": "Kabel Untuk Suhu Sangat Dingin." },
  { "en": "Bisa Menjadi Superkonduktor?", "id": "Ya Beberapa Didesain Untuk Itu." },
  { "en": "Apa Itu Peringkat Suhu Penyimpanan?", "id": "Batas Suhu Saat Kabel Tidak Digunakan." },
  { "en": "Apa Itu Peringkat Suhu Instalasi?", "id": "Batas Suhu Saat Kabel Boleh Dipasang." },
  { "en": "Kenapa Ada Batas Suhu Instalasi?", "id": "Isolasi Bisa Retak Jika Terlalu Dingin." },
  { "en": "Apa Itu Strippability?", "id": "Kemudahan Mengupas Isolasi Dari Konduktor." },
  { "en": "Apa Itu Kabel Lapis Ganda?", "id": "Kabel Dengan Dua Lapis Isolasi." },
  { "en": "Disebut Juga Kabel?", "id": "Kabel Berinsulasi Ganda." },
  { "en": "Apa Simbolnya?", "id": "Kotak Di Dalam Kotak." },
  { "en": "Apakah Membutuhkan Grounding?", "id": "Tidak Karena Sangat Aman." },
  { "en": "Contoh Perangkat Berinsulasi Ganda?", "id": "Bor Listrik Dan Pengering Rambut." },
  { "en": "Apa Itu Harmonik Arus?", "id": "Distorsi Arus Akibat Beban Non-Linier." },
  { "en": "Harmonik Menyebabkan Pemanasan Tambahan Di?", "id": "Kabel Terutama Kabel Netral." },
  { "en": "Untuk Beban Non-Linier Kabel Netral Perlu?", "id": "Ukuran Lebih Besar Dari Kabel Fasa." },
  { "en": "Apa Itu Faktor Kulit (Skin Factor)?", "id": "Rasio Resistansi AC Terhadap DC." },
  { "en": "Nilainya Selalu?", "id": "Lebih Besar Dari Satu." },
  { "en": "Apa Itu Konduktor Milliken?", "id": "Konduktor Bersektor Terisolasi Untuk Mengurangi Efek Kulit." },
  { "en": "Digunakan Pada Kabel?", "id": "Kabel Daya Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Isolator Polimer?", "id": "Isolator Terbuat Dari Silikon Atau EPDM." },
  { "en": "Apa Itu Isolator Porselen?", "id": "Isolator Keramik Tradisional." },
  { "en": "Mana Yang Lebih Ringan?", "id": "Isolator Polimer." },
  { "en": "Apa Itu Sifat Hidrofobik?", "id": "Kemampuan Permukaan Menolak Air." },
  { "en": "Penting Untuk Isolator?", "id": "Ya Di Area Berpolusi Tinggi." },
  { "en": "Isolator Polimer Punya Sifat Ini?", "id": "Ya Sangat Baik." },
  { "en": "Apa Beda Konduktor, Isolator, Semikonduktor?", "id": "Berdasarkan Kemampuan Menghantarkan Listrik." },
  { "en": "Apa Beda Kabel Data Analog Dan Digital?", "id": "Analog Bawa Gelombang Kontinu Digital Bawa Bit." },
  { "en": "Apa Beda Kabel Indoor Dan Outdoor?", "id": "Outdoor Punya Perlindungan Cuaca Dan UV." },
  { "en": "Apa Beda Kabel Fleksibel Dan Kaku?", "id": "Fleksibel Untuk Gerak Kaku Untuk Tetap." },
  { "en": "Apa Beda Sambungan Solder Dan Crimping?", "id": "Solder Pakai Panas Crimping Pakai Tekanan." },
  { "en": "Apa Peran Dielektrik Dalam Kapasitansi?", "id": "Konstanta Dielektrik Menentukan Nilai Kapasitansi." },
  { "en": "Apa Peran Jarak Antar Konduktor?", "id": "Mempengaruhi Kapasitansi Dan Induktansi." },
  { "en": "Jarak Lebih Dekat Kapasitansi?", "id": "Semakin Besar." },
  { "en": "Apa Itu 'Kabel Yang Baik'?", "id": "Kabel Yang Sesuai Aplikasi Dan Standar." },
  { "en": "Apa Itu 'Ukuran Yang Tepat'?", "id": "Ukuran Cukup Untuk Arus Tanpa Panas Berlebih." },
  { "en": "Apa Itu 'Instalasi Yang Benar'?", "id": "Instalasi Sesuai Aturan Keselamatan." },
  { "en": "Apa Itu Kabel Telemetri?", "id": "Kabel Untuk Mengirim Data Pengukuran Jarak Jauh." },
  { "en": "Apa Itu Kabel Koaksial Radiating?", "id": "Berfungsi Ganda Sebagai Kabel Dan Antena." },
  { "en": "Apa Itu Kabel Tahan Momen Putir (Torsion)?", "id": "Kabel Untuk Aplikasi Robotik Berputar." },
  { "en": "Apa Itu Selubung PVC?", "id": "Polyvinyl Chloride." },
  { "en": "Apa Itu Selubung PE?", "id": "Polyethylene." },
  { "en": "Apa Itu Selubung PUR?", "id": "Polyurethane." },
  { "en": "PUR Tahan Terhadap Apa?", "id": "Abrasi Dan Bahan Kimia." },
  { "en": "PE Tahan Terhadap Apa?", "id": "Kelembaban Dan Cuaca." },
  { "en": "PVC Adalah Pilihan?", "id": "Paling Umum Dan Ekonomis." },
  { "en": "Apa Itu Konstanta Atenuasi (α)?", "id": "Bagian Riil Konstanta Propagasi." },
  { "en": "Apa Itu Konstanta Fasa (β)?", "id": "Bagian Imajiner Konstanta Propagasi." },
  { "en": "Panjang Gelombang Di Kabel Dihitung Dari?", "id": "Konstanta Fasa (2π/β)." },
  { "en": "Apa Itu Kabel Pita Datar Fleksibel (FFC)?", "id": "Kabel Sangat Tipis Untuk Elektronik." },
  { "en": "Apa Itu Kabel Sirkuit Fleksibel (FPC)?", "id": "Sirkuit Tercetak Di Atas Substrat Fleksibel." },
  { "en": "Apa Itu Kabel Berinti Udara?", "id": "Kabel Koaksial Dengan Udara Sebagai Dielektrik." },
  { "en": "Atenuasinya Sangat Rendah?", "id": "Ya." },
  { "en": "Apa Itu Kabel Berisi Busa?", "id": "Kabel Koaksial Dengan Busa Sebagai Dielektrik." },
  { "en": "Apa Itu Stripping Termal?", "id": "Mengupas Isolasi Menggunakan Panas." },
  { "en": "Digunakan Untuk Isolasi Apa?", "id": "Teflon Dan Isolasi Keras Lainnya." },
  { "en": "Apa Itu Uji Spark?", "id": "Pengujian Integritas Isolasi Selama Produksi." },
  { "en": "Bagaimana Cara Kerjanya?", "id": "Melewatkan Kabel Melalui Elektroda Tegangan Tinggi." },
  { "en": "Jika Isolasi Cacat Apa Yang Terjadi?", "id": "Terjadi Percikan Api." },
  { "en": "Apa Itu Peringkat CM, CMR, CMP?", "id": "Peringkat Tahan Api Standar Amerika." },
  { "en": "CMP Adalah Peringkat Tertinggi?", "id": "Ya Untuk Penggunaan Di Ruang Plenum." },
  { "en": "CMR Adalah Peringkat Untuk?", "id": "Instalasi Vertikal (Riser)." },
  { "en": "CM Adalah Peringkat Untuk?", "id": "Penggunaan Umum." },
  { "en": "Apa Itu Kabel Komposit?", "id": "Kabel Dengan Beberapa Jenis Komponen." },
  { "en": "Contohnya Adalah?", "id": "Kabel Berisi Twisted Pair Dan Coaxial." },
  { "en": "Apa Itu Kabel Siamese?", "id": "Dua Kabel Yang Digabung Berdampingan." },
  { "en": "Apa Itu Kabel Audio OFC?", "id": "Oxygen-Free Copper." },
  { "en": "Tujuannya Mengurangi Oksidasi?", "id": "Ya Di Antara Kristal Tembaga." },
  { "en": "Apa Itu Konduktor Terdampar Tali?", "id": "Beberapa Bundel Serabut Dipilin Bersama." },
  { "en": "Memberikan Fleksibilitas Ekstra?", "id": "Ya." },
  { "en": "Apa Itu Kabel Non-Kontaminasi?", "id": "Jaket Yang Tidak Merusak Permukaan Lain." },
  { "en": "Apa Itu Tegangan rms?", "id": "Root Mean Square." },
  { "en": "Rating Tegangan Kabel Dinyatakan Dalam?", "id": "Tegangan rms." },
  { "en": "Apa Itu Tegangan Puncak?", "id": "Nilai Maksimum Sesaat Gelombang AC." },
  { "en": "Hubungannya Dengan rms?", "id": "Tegangan Puncak Adalah rms Kali Akar Dua." },
  { "en": "Isolasi Harus Menahan Tegangan?", "id": "Tegangan Puncak." },
  { "en": "Apa Itu Uji Partial Discharge?", "id": "Menguji Pelepasan Muatan Sebagian Di Isolasi." },
  { "en": "Mengindikasikan Adanya Cacat?", "id": "Ya Seperti Rongga Udara Di Isolasi." },
  { "en": "Apa Itu Kabel Shielded Secara Global?", "id": "Satu Shield Mengelilingi Semua Inti." },
  { "en": "Apa Itu Kabel Shielded Individual?", "id": "Setiap Pasangan Inti Punya Shield Sendiri." },
  { "en": "Mana Yang Lebih Baik Untuk Crosstalk?", "id": "Shielded Individual." },
  { "en": "Apa Itu Grounding Shield?", "id": "Menghubungkan Shield Ke Tanah." },
  { "en": "Harus Ditanahkan Di Berapa Ujung?", "id": "Biasanya Hanya Di Satu Ujung." },
  { "en": "Tujuannya Mencegah Apa?", "id": "Arus Ground Loop Di Shield." },
  { "en": "Apa Itu Pigtail Shield?", "id": "Kawat Pendek Untuk Menyambung Shield." },
  { "en": "Pigtail Panjang Buruk Untuk?", "id": "Frekuensi Tinggi." },
  { "en": "Kenapa Buruk?", "id": "Karena Punya Induktansi Tinggi." },
  { "en": "Terminasi Shield Yang Baik?", "id": "Menyambung 360 Derajat Ke Konektor." },
  { "en": "Apa Itu Transfer Impedance?", "id": "Ukuran Efektivitas Pelindung (Shield)." },
  { "en": "Nilai Yang Baik Adalah?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Konduktor Bersektor?", "id": "Konduktor Berbentuk Sektor Lingkaran." },
  { "en": "Tujuannya Membuat Kabel?", "id": "Lebih Padat Dan Bulat." },
  { "en": "Apa Itu Kabel Berisi Udara Kering?", "id": "Udara Bertekanan Mencegah Kelembaban Masuk." },
  { "en": "Digunakan Pada Kabel Telepon?", "id": "Ya Kabel Telepon Jarak Jauh." },
  { "en": "Apa Itu Kabel Tahan Hidrolisis?", "id": "Kabel Tahan Terhadap Air Panas Uap." },
  { "en": "Apa Itu Kabel Tahan Mikroba?", "id": "Kabel Untuk Industri Makanan." },
  { "en": "Apa Itu Penuaan UV?", "id": "Degradasi Jaket Akibat Sinar Matahari." },
  { "en": "Jaket Menjadi Keras Dan?", "id": "Rapuh." },
  { "en": "Warna Jaket Mempengaruhi Ketahanan UV?", "id": "Ya Hitam Biasanya Paling Tahan." },
  { "en": "Kenapa Warna Hitam Tahan UV?", "id": "Karena Mengandung Carbon Black." },
  { "en": "Apa Itu Kawat Magnet?", "id": "Sama Dengan Kawat Email." },
  { "en": "Apa Itu Kawat Litz?", "id": "Setiap Serabut Diisolasi Untuk Kurangi Skin Effect." },
  { "en": "Apa Itu Kabel Fire Survival?", "id": "Sama Dengan Kabel Tahan Api." },
  { "en": "Apa Itu Standar BS 6387?", "id": "Standar Inggris Untuk Kinerja Kabel Api." },
  { "en": "Menguji Ketahanan Terhadap?", "id": "Api Benturan Dan Semprotan Air." },
  { "en": "Apa Itu Kabel Pengaman Intrinsik (IS)?", "id": "Kabel Untuk Area Berbahaya (Mudah Meledak)." },
  { "en": "Warnanya Biasanya Apa?", "id": "Biru Terang." },
  { "en": "Energi Listriknya Dibatasi?", "id": "Ya Agar Tidak Bisa Memicu Percikan Api." },
  { "en": "Apa Itu Hambatan Karakteristik?", "id": "Sama Dengan Impedansi Karakteristik." },
  { "en": "Apa Itu Kabel Seimbang?", "id": "Sama Dengan Kabel Balanced." },
  { "en": "Apa Itu Kabel Tidak Seimbang?", "id": "Sama Dengan Kabel Unbalanced." },
  { "en": "Coaxial Adalah Kabel?", "id": "Tidak Seimbang." },
  { "en": "Twisted Pair Adalah Kabel?", "id": "Seimbang." },
  { "en": "Balun Mengubah Seimbang Ke?", "id": "Tidak Seimbang Dan Sebaliknya." },
  { "en": "Apa Itu Return Current Path?", "id": "Jalur Kembali Untuk Arus Listrik." },
  { "en": "Pada Kabel Coaxial Jalur Kembali?", "id": "Adalah Lapisan Pelindung (Shield)." },
  { "en": "Pada Twisted Pair Jalur Kembali?", "id": "Adalah Kawat Kedua Dalam Pasangan." },
  { "en": "Area Loop Arus Yang Baik?", "id": "Harus Sekecil Mungkin." },
  { "en": "Kenapa Harus Kecil?", "id": "Untuk Mengurangi Emisi Dan Serapan Gangguan." },
  { "en": "Twisted Pair Punya Area Loop?", "id": "Sangat Kecil." },
  { "en": "Apa Itu Mode Diferensial?", "id": "Sinyal Normal Pada Kabel Balanced." },
  { "en": "Apa Itu Mode Bersama (Common Mode)?", "id": "Gangguan Yang Muncul Sama Di Kedua Kawat." },
  { "en": "Sistem Balanced Menolak Sinyal?", "id": "Mode Bersama." },
  { "en": "Apa Elemen Dasar Kabel?", "id": "Konduktor Dan Isolator." },
  { "en": "Apa Fungsi Konduktor?", "id": "Membawa Arus Listrik." },
  { "en": "Apa Fungsi Isolator?", "id": "Membatasi Arus Pada Konduktor." },
  { "en": "Apa Pembeda Utama Antar Kabel?", "id": "Material Konstruksi Dan Aplikasi." },
  { "en": "Tembaga Adalah Konduktor Yang?", "id": "Sangat Baik Dan Umum." },
  { "en": "Aluminium Adalah Konduktor Yang?", "id": "Lebih Ringan Dan Lebih Murah." },
  { "en": "PVC Adalah Isolator Yang?", "id": "Paling Umum Dan Serbaguna." },
  { "en": "XLPE Adalah Isolator Untuk?", "id": "Suhu Lebih Tinggi Dan Performa Baik." },
  { "en": "Kabel Fleksibel Menggunakan Konduktor?", "id": "Serabut." },
  { "en": "Kabel Instalasi Tetap Menggunakan Konduktor?", "id": "Pejal." },
  { "en": "Apa Itu Pelindung (Shield)?", "id": "Melindungi Sinyal Dari Gangguan Luar." },
  { "en": "Apa Itu Armor?", "id": "Melindungi Kabel Dari Kerusakan Fisik." },
  { "en": "Kabel Data Modern Menggunakan?", "id": "Pasangan Kawat Yang Dipilin." },
  { "en": "Pilinan Mengurangi Apa?", "id": "Crosstalk Dan Serapan Gangguan." },
  { "en": "Kabel Optik Menggunakan Apa?", "id": "Serat Kaca Untuk Menghantarkan Cahaya." },
  { "en": "Kabel Optik Kebal Terhadap?", "id": "Semua Jenis Interferensi Elektromagnetik." },
  { "en": "Apa Itu KHA Kabel?", "id": "Batas Arus Aman Berkelanjutan." },
  { "en": "Apa Itu Jatuh Tegangan Kabel?", "id": "Kehilangan Potensial Listrik Akibat Resistansi." },
  { "en": "Kabel Lebih Besar Akan Mengurangi?", "id": "Jatuh Tegangan Dan Pemanasan." },
  { "en": "Apa Itu Kode Warna?", "id": "Untuk Keselamatan Dan Kemudahan Instalasi." },
  { "en": "Apa Itu Konektor?", "id": "Antarmuka Antara Kabel Dan Peralatan." },
  { "en": "Konektor Yang Baik Harus?", "id": "Memiliki Resistansi Kontak Rendah." },
  { "en": "Apa Itu Impedansi?", "id": "Penting Untuk Kabel Frekuensi Tinggi." },
  { "en": "Ketidakcocokan Impedansi Menyebabkan?", "id": "Pantulan Sinyal." },
  { "en": "Pantulan Sinyal Buruk Untuk?", "id": "Integritas Sinyal Digital Dan Video." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal Seiring Jarak." },
  { "en": "Atenuasi Membatasi Apa?", "id": "Panjang Maksimum Sebuah Kabel." },
  { "en": "Apa Itu Kabel Berinsulasi Ganda?", "id": "Peralatan Tanpa Membutuhkan Koneksi Ground." },
  { "en": "Simbolnya Adalah?", "id": "Dua Kotak Konsentris." },
  { "en": "Apa Itu Standar Kabel?", "id": "Menjamin Kinerja Dan Keamanan." },
  { "en": "Apa Itu Peringkat Api?", "id": "Menunjukkan Perilaku Kabel Saat Terbakar." },
  { "en": "Kabel LSZH Adalah Pilihan?", "id": "Aman Untuk Area Publik." },
  { "en": "Apa Itu Sinyal Balanced?", "id": "Menggunakan Dua Kawat Untuk Menolak Gangguan." },
  { "en": "Apa Itu Sinyal Unbalanced?", "id": "Menggunakan Satu Kawat Dan Ground." },
  { "en": "Mana Yang Lebih Baik Untuk Audio Profesional?", "id": "Sinyal Balanced." },
  { "en": "Apa Itu Stripping?", "id": "Mengupas Isolasi." },
  { "en": "Apa Itu Crimping?", "id": "Memasang Terminal Dengan Tekanan." },
  { "en": "Apa Itu Soldering?", "id": "Menyambung Dengan Timah Panas." },
  { "en": "Apa Itu Conduit?", "id": "Pipa Pelindung Kabel." },
  { "en": "Apa Itu Trunking?", "id": "Saluran Kabel Persegi." },
  { "en": "Apa Itu Kabel Tray?", "id": "Nampan Terbuka Untuk Jalur Kabel." },
  { "en": "Apa Itu Strain Relief?", "id": "Mencegah Kabel Tertarik Dari Konektor." },
  { "en": "Apa Itu Bend Radius?", "id": "Batas Aman Kelengkungan Kabel." },
  { "en": "Apa Itu Oksidasi?", "id": "Penyebab Umum Kegagalan Sambungan." },
  { "en": "Apa Itu Tinned Copper?", "id": "Membantu Mencegah Oksidasi." },
  { "en": "Apa Itu Skin Effect?", "id": "Meningkatkan Resistansi Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Kabel Litz?", "id": "Didesain Untuk Melawan Skin Effect." },
  { "en": "Apa Itu Single-Mode Fiber?", "id": "Untuk Jarak Jauh Kecepatan Tinggi." },
  { "en": "Apa Itu Multi-Mode Fiber?", "id": "Untuk Jarak Pendek Biaya Lebih Rendah." },
  { "en": "Apa Itu Fusion Splicing?", "id": "Metode Penyambungan Fiber Optik Terbaik." },
  { "en": "Apa Itu TDR?", "id": "Mencari Lokasi Putus Kabel." },
  { "en": "Apa Itu OTDR?", "id": "TDR Untuk Kabel Fiber Optik." },
  { "en": "Apa Itu Kabel Hibrida?", "id": "Menggabungkan Tembaga Dan Fiber." },
  { "en": "Apa Itu Derating?", "id": "Penyesuaian KHA Berdasarkan Kondisi." },
  { "en": "Suhu Tinggi Akan?", "id": "Menurunkan Nilai KHA." },
  { "en": "Kabel Yang Dibundel Akan?", "id": "Menurunkan Nilai KHA." },
  { "en": "Apa Itu Jalur Kembali Arus?", "id": "Harus Selalu Dekat Dengan Jalur Pergi." },
  { "en": "Tujuannya Mengurangi?", "id": "Induktansi Dan Emisi Gangguan." },
  { "en": "Pada Coaxial Jalur Kembali Adalah?", "id": "Shield." },
  { "en": "Pada Twisted Pair Jalur Kembali Adalah?", "id": "Kawat Pasangannya." },
  { "en": "Apa Itu Common Mode Current?", "id": "Arus Gangguan Yang Mengalir Searah." },
  { "en": "Ferrite Bead Efektif Meredam?", "id": "Arus Common Mode." },
  { "en": "Apa Itu Kabel Spiral?", "id": "Kabel Fleksibel Yang Bisa Memanjang." },
  { "en": "Apa Itu Kabel Pita?", "id": "Kabel Datar Untuk Koneksi Internal." },
  { "en": "Apa Itu Penuaan Isolasi?", "id": "Degradasi Isolator Seiring Waktu." },
  { "en": "Penyebab Utamanya Adalah?", "id": "Panas." },
  { "en": "Apa Itu Rating Suhu?", "id": "Batas Suhu Aman Pengoperasian Kabel." },
  { "en": "Apa Itu Kabel Tahan UV?", "id": "Penting Untuk Instalasi Luar Ruangan." },
  { "en": "Apa Itu Harmonik?", "id": "Menyebabkan Pemanasan Tambahan Di Kabel Netral." },
  { "en": "Apa Itu Kabel Kontrol?", "id": "Kabel Multi-Inti Untuk Sinyal Kontrol." },
  { "en": "Biasanya Memiliki Pelindung?", "id": "Ya Untuk Mencegah Gangguan." },
  { "en": "Apa Itu Ground Loop?", "id": "Penyebab Dengung Di Sistem Audio." },
  { "en": "Solusinya Bisa Menggunakan?", "id": "Isolator Audio Atau Koneksi Balanced." },
  { "en": "Apa Itu Firestop?", "id": "Material Untuk Mencegah Api Lewat Tembusan Kabel." },
  { "en": "Apa Itu Kabel Bawah Laut?", "id": "Tulang Punggung Internet Global." },
  { "en": "Biasanya Berjenis Apa?", "id": "Kabel Serat Optik." },
  { "en": "Memiliki Penguat Sinyal?", "id": "Ya Di Sepanjang Jalurnya." },
  { "en": "Apa Itu Kabel Tahan Radiasi?", "id": "Untuk Pembangkit Listrik Tenaga Nuklir." },
  { "en": "Apa Itu Kabel Cryogenic?", "id": "Untuk Aplikasi Suhu Sangat Rendah." },
  { "en": "Apa Itu Kabel Superkonduktor?", "id": "Kabel Dengan Nol Resistansi." },
  { "en": "Memerlukan Pendinginan Ekstrem?", "id": "Ya Menggunakan Nitrogen Atau Helium Cair." },
  { "en": "Apa Itu Kawat Thermocouple?", "id": "Menghasilkan Tegangan Berdasarkan Suhu." },
  { "en": "Terbuat Dari Dua Logam?", "id": "Berbeda Yang Disatukan." },
  { "en": "Apa Itu Kabel Instrumen?", "id": "Kabel Terlindung Untuk Sinyal Pengukuran." },
  { "en": "Apa Itu Kabel Data Kategori 8?", "id": "Standar Ethernet Untuk Kecepatan Sangat Tinggi." },
  { "en": "Kecepatannya Mencapai?", "id": "40 Gigabit Per Detik." },
  { "en": "Selalu Berjenis Shielded?", "id": "Ya Selalu STP." },
  { "en": "Apa Itu Kabel Listrik Fleksibel?", "id": "Umum Digunakan Untuk Peralatan Portabel." },
  { "en": "Apa Itu Kabel Instalasi Tetap?", "id": "Umum Digunakan Di Dalam Dinding." },
  { "en": "Apa Itu Kode Kabel SNI?", "id": "Menunjukkan Spesifikasi Dan Keamanan Kabel." },
  { "en": "Membeli Kabel Selalu Pilih?", "id": "Yang Berlogo SNI." },
  { "en": "Untuk Keamanan Dan?", "id": "Kinerja Yang Andal." },
  { "en": "Apa Itu Kabel Power Cord?", "id": "Kabel AC Yang Bisa Dilepas." },
  { "en": "Apa Itu Kabel Hardwired?", "id": "Kabel Yang Tersambung Permanen Ke Perangkat." },
  { "en": "Inti Dari Semua Kabel?", "id": "Menyalurkan Energi Atau Informasi." },
  { "en": "Dengan Cara Yang?", "id": "Efisien Aman Dan Andal." },
  { "en": "Pemilihan Kabel Yang Tepat?", "id": "Sangat Penting Untuk Kinerja Sistem." },
  { "en": "Apa Dua Komponen Utama Kabel?", "id": "Konduktor Dan Isolator." },
  { "en": "Apa Fungsi Dasar Konduktor?", "id": "Membawa Aliran Elektron (Arus)." },
  { "en": "Apa Fungsi Dasar Isolator?", "id": "Mencegah Aliran Elektron Bocor." },
  { "en": "Apa Beda Utama Tembaga Dan Aluminium?", "id": "Tembaga Punya Konduktivitas Lebih Tinggi." },
  { "en": "Apa Beda Utama Kabel Fleksibel Dan Kaku?", "id": "Jenis Konduktor (Serabut Vs Pejal)." },
  { "en": "Apa Beda Utama Kabel Indoor Dan Outdoor?", "id": "Ketahanan Jaket Terhadap Cuaca." },
  { "en": "Apa Beda Utama Kabel Daya Dan Data?", "id": "Fokus Desain (Arus Vs Integritas Sinyal)." },
  { "en": "Apa Beda Utama Kabel Tembaga Dan Optik?", "id": "Media Transmisi (Elektron Vs Foton)." },
  { "en": "Apa Itu KHA?", "id": "Batas Arus Maksimum Yang Aman." },
  { "en": "Apa Itu Jatuh Tegangan?", "id": "Kehilangan Tegangan Akibat Resistansi." },
  { "en": "Bagaimana Cara Memilih Ukuran Kabel?", "id": "Berdasarkan KHA Dan Batas Jatuh Tegangan." },
  { "en": "Apa Tujuan Utama Grounding?", "id": "Keselamatan Manusia Dari Sengatan Listrik." },
  { "en": "Apa Tujuan Utama Shielding?", "id": "Melindungi Sinyal Dari Gangguan Luar." },
  { "en": "Apa Tujuan Utama Pilinan Kawat?", "id": "Mengurangi Gangguan Antar Pasangan Kawat." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Penting Untuk Mencegah Pantulan Sinyal." },
  { "en": "Apa Itu Atenuasi?", "id": "Membatasi Jarak Maksimum Transmisi Sinyal." },
  { "en": "Standar Di Indonesia Menggunakan?", "id": "SNI (Standar Nasional Indonesia)." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Putih Untuk Dalam Ruangan." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Hitam Untuk Luar Ruangan." },
  { "en": "Apa Itu Kabel NYA?", "id": "Harus Dipasang Di Dalam Pipa." },
  { "en": "Apa Itu Kabel UTP?", "id": "Paling Umum Untuk Jaringan Ethernet." },
  { "en": "Apa Itu Kabel Coaxial?", "id": "Paling Umum Untuk Sinyal TV." },
  { "en": "Apa Itu Serat Optik?", "id": "Paling Umum Untuk Internet Jarak Jauh." },
  { "en": "Pemanasan Kabel Disebabkan Oleh?", "id": "Kerugian I²R." },
  { "en": "Apa Itu Oksidasi?", "id": "Musuh Utama Sambungan Kabel." },
  { "en": "Bagaimana Cara Terminasi Yang Baik?", "id": "Bersih Kencang Dan Tahanan Rendah." },
  { "en": "Apa Itu Stripper?", "id": "Alat Untuk Mengupas Isolasi." },
  { "en": "Apa Itu Crimper?", "id": "Alat Untuk Memasang Terminal." },
  { "en": "Apa Itu Solder?", "id": "Timah Untuk Sambungan Elektronik." },
  { "en": "Apa Itu Kabel LSZH?", "id": "Lebih Aman Saat Kebakaran." },
  { "en": "Apa Itu Kabel Tahan Api?", "id": "Untuk Sistem Darurat." },
  { "en": "Apa Itu Sinyal Balanced?", "id": "Menggunakan Tiga Kawat." },
  { "en": "Apa Keuntungannya?", "id": "Sangat Tahan Gangguan." },
  { "en": "Apa Itu Sinyal Unbalanced?", "id": "Menggunakan Dua Kawat." },
  { "en": "Apa Itu Radius Tekuk?", "id": "Batas Aman Pembengkokan Kabel." },
  { "en": "Apa Itu Strain Relief?", "id": "Pereda Tegangan Tarik." },
  { "en": "Apa Itu TDR?", "id": "Mencari Lokasi Kabel Putus." },
  { "en": "Apa Itu Megger?", "id": "Mengukur Kualitas Isolasi." },
  { "en": "Apa Itu Single-Mode Fiber?", "id": "Untuk Jarak Jauh." },
  { "en": "Apa Itu Multi-Mode Fiber?", "id": "Untuk Jarak Pendek." },
  { "en": "Apa Itu Dispersi?", "id": "Penyebab Pembatasan Bandwidth Fiber." },
  { "en": "Apa Itu Kode Kategori (CAT)?", "id": "Standar Kinerja Kabel Jaringan." },
  { "en": "Kategori Lebih Tinggi Berarti?", "id": "Lebih Cepat." },
  { "en": "Apa Itu PoE?", "id": "Daya Listrik Lewat Kabel Ethernet." },
  { "en": "Apa Itu Armor?", "id": "Untuk Instalasi Direct Burial." },
  { "en": "Apa Itu Conduit?", "id": "Memberi Perlindungan Mekanis Tambahan." },
  { "en": "Apa Itu Ferrite?", "id": "Meredam Gangguan Frekuensi Tinggi." },
  { "en": "Apa Itu Skin Effect?", "id": "Meningkatkan Resistansi AC." },
  { "en": "Apa Itu Kabel Litz?", "id": "Melawan Skin Effect." },
  { "en": "Apa Itu Derating?", "id": "Penyesuaian KHA Akibat Panas." },
  { "en": "Penyebab Panas Eksternal?", "id": "Suhu Ambient Atau Kabel Berdekatan." },
  { "en": "Apa Itu Ground Loop?", "id": "Penyebab Dengung Audio." },
  { "en": "Solusinya Bisa Dengan Kabel?", "id": "Balanced Atau Isolator." },
  { "en": "Apa Itu Kabel Hibrida?", "id": "Gabungan Tembaga Dan Fiber Optik." },
  { "en": "Apa Itu Jalur Kembali Arus?", "id": "Harus Selalu Dekat Jalur Pergi." },
  { "en": "Tujuannya Mengurangi?", "id": "Loop Induktansi." },
  { "en": "Loop Induktansi Menyebabkan?", "id": "Emisi Dan Serapan Gangguan." },
  { "en": "Pilinan Kawat Membuat Loop?", "id": "Sangat Kecil Dan Saling Membatalkan." },
  { "en": "Shield Pada Coaxial Berfungsi Sebagai?", "id": "Jalur Kembali Arus." },
  { "en": "Apa Itu CCA?", "id": "Aluminium Berbaju Tembaga." },
  { "en": "Kualitasnya Lebih Rendah Dari?", "id": "Tembaga Murni." },
  { "en": "Apa Itu Konduktor Berkas?", "id": "Mengurangi Efek Corona Di SUTET." },
  { "en": "Apa Itu Harmonik?", "id": "Bisa Memanaskan Kabel Netral." },
  { "en": "Untuk Beban Komputer Kabel Netral Perlu?", "id": "Ukuran Yang Lebih Besar." },
  { "en": "Apa Itu Konektor RJ11?", "id": "Untuk Kabel Telepon." },
  { "en": "Apa Itu Konektor RCA?", "id": "Untuk Audio/Video Analog." },
  { "en": "Apa Itu Konektor XLR?", "id": "Untuk Audio Profesional Balanced." },
  { "en": "Apa Itu Konektor BNC?", "id": "Untuk Video Profesional Atau RF." },
  { "en": "Apa Itu Kabel Tahan UV?", "id": "Jaket Tidak Cepat Rusak Oleh Matahari." },
  { "en": "Penting Untuk Kabel Warna?", "id": "Selain Warna Hitam." },
  { "en": "Apa Itu F-Connector?", "id": "Konektor Ulir Untuk TV Kabel." },
  { "en": "Apa Itu Fusion Splicing?", "id": "Menyambung Fiber Dengan Melelehkannya." },
  { "en": "Menghasilkan Sambungan Dengan Rugi?", "id": "Sangat Rendah." },
  { "en": "Apa Itu Mechanical Splicing?", "id": "Menyambung Fiber Secara Mekanis." },
  { "en": "Ruginya Lebih Besar?", "id": "Ya Dibanding Fusion Splicing." },
  { "en": "Apa Itu Kabel Patch Fiber?", "id": "Kabel Fiber Pendek Dengan Konektor." },
  { "en": "Apa Itu Pigtail Fiber?", "id": "Kabel Fiber Dengan Konektor Di Satu Sisi." },
  { "en": "Apa Itu Atenuator Optik?", "id": "Melemahkan Sinyal Cahaya." },
  { "en": "Apa Itu Amplifier Optik?", "id": "Menguatkan Sinyal Cahaya." },
  { "en": "Apa Itu WDM?", "id": "Wavelength Division Multiplexing." },
  { "en": "Artinya Satu Fiber Membawa?", "id": "Banyak Warna Cahaya (Kanal)." },
  { "en": "Ini Meningkatkan Kapasitas?", "id": "Sangat Besar." },
  { "en": "Apa Itu Kabel Tanam Langsung?", "id": "Sama Dengan Kabel Direct Burial." },
  { "en": "Apa Itu Kabel Udara?", "id": "Kabel Yang Digantung Antar Tiang." },
  { "en": "Apa Itu Kabel Laut?", "id": "Kabel Yang Diletakkan Di Dasar Laut." },
  { "en": "Perlindungannya Sangat Kuat?", "id": "Ya Berlapis-lapis Armor Dan Isolasi." },
  { "en": "Pemilihan Kabel Adalah Langkah Kritis?", "id": "Ya Dalam Desain Sistem Elektronik." },
  { "en": "Kabel Yang Salah Menyebabkan?", "id": "Kinerja Buruk Dan Bahaya Keamanan." },
  { "en": "Selalu Periksa Standar Dan?", "id": "Spesifikasi Yang Sesuai." },
  { "en": "Kabel Adalah Tulang Punggung?", "id": "Dunia Modern Yang Terhubung." },
  { "en": "Dari Pembangkit Listrik Hingga?", "id": "Ponsel Di Tangan Anda." },
  { "en": "Memahami Dasar Kabel Adalah?", "id": "Ilmu Fundamental Bagi Insinyur Elektro." },
  { "en": "Konduktor Membawa Sinyal?", "id": "Isolator Melindungi Sinyal." },
  { "en": "Dua Fungsi Sederhana Ini?", "id": "Mendasari Teknologi Yang Sangat Kompleks." }



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
