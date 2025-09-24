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


  { "en": "Apa Itu Teori Informasi?", "id": "Studi Kuantifikasi, Penyimpanan, Dan Komunikasi Informasi." },
  { "en": "Siapa Bapak Teori Informasi?", "id": "Claude Shannon." },
  { "en": "Apa Satuan Dasar Informasi?", "id": "Bit." },
  { "en": "Apa Kepanjangan Dari Bit?", "id": "Binary Digit." },
  { "en": "Apa Itu Entropi Dalam Teori Informasi?", "id": "Ukuran Ketidakpastian Atau Kejutan Suatu Variabel." },
  { "en": "Kapan Entropi Mencapai Nilai Maksimum?", "id": "Saat Semua Hasil Memiliki Probabilitas Sama." },
  { "en": "Apa Itu Informasi Timbal Balik (Mutual Information)?", "id": "Ukuran Pengurangan Ketidakpastian Satu Variabel." },
  { "en": "Apa Itu Saluran Komunikasi?", "id": "Media Tempat Informasi Ditransmisikan." },
  { "en": "Sebutkan Contoh Saluran Komunikasi?", "id": "Kabel Tembaga, Serat Optik, Udara." },
  { "en": "Apa Itu Derau (Noise) Dalam Komunikasi?", "id": "Gangguan Yang Merusak Sinyal Informasi." },
  { "en": "Apa Itu Kapasitas Saluran?", "id": "Tingkat Maksimal Data Dapat Dikirim Andal." },
  { "en": "Apa Rumus Kapasitas Saluran Shannon-Hartley?", "id": "C Sama Dengan B Log2 (1+S/N)." },
  { "en": "Apa Itu Pengkodean Sumber (Source Coding)?", "id": "Proses Untuk Mengompresi Data." },
  { "en": "Apa Tujuan Utama Pengkodean Sumber?", "id": "Mengurangi Redundansi Data." },
  { "en": "Apa Itu Redundansi?", "id": "Informasi Berlebih Yang Tidak Diperlukan." },
  { "en": "Apa Itu Kode Awalan (Prefix Code)?", "id": "Kode Dimana Tidak Ada Codeword Awalan." },
  { "en": "Sebutkan Contoh Algoritma Pengkodean Sumber?", "id": "Huffman Coding, Shannon-Fano Coding, Lempel-Ziv." },
  { "en": "Apa Itu Pengkodean Huffman?", "id": "Metode Pengkodean Sumber Berbasis Frekuensi Simbol." },
  { "en": "Apakah Pengkodean Huffman Menghasilkan Kode Optimal?", "id": "Ya, Untuk Pengkodean Simbol-demi-Simbol." },
  { "en": "Apa Itu Pengkodean Saluran (Channel Coding)?", "id": "Proses Menambah Redundansi Untuk Deteksi Kesalahan." },
  { "en": "Apa Tujuan Utama Pengkodean Saluran?", "id": "Melawan Efek Derau Pada Saluran." },
  { "en": "Apa Itu Deteksi Kesalahan?", "id": "Kemampuan Mendeteksi Adanya Kesalahan Transmisi." },
  { "en": "Apa Itu Koreksi Kesalahan?", "id": "Kemampuan Memperbaiki Kesalahan Transmisi." },
  { "en": "Apa Itu Bit Paritas (Parity Bit)?", "id": "Bit Tambahan Untuk Mendeteksi Kesalahan Tunggal." },
  { "en": "Apa Itu Paritas Genap?", "id": "Jumlah Bit 1 Termasuk Paritas Adalah Genap." },
  { "en": "Apa Itu Paritas Ganjil?", "id": "Jumlah Bit 1 Termasuk Paritas Adalah Ganjil." },
  { "en": "Apa Itu Kode Blok?", "id": "Mengkodekan Blok Data Berukuran Tetap." },
  { "en": "Apa Itu Kode Konvolusional?", "id": "Mengkodekan Aliran Data Secara Berkelanjutan." },
  { "en": "Apa Itu Jarak Hamming?", "id": "Jumlah Posisi Bit Yang Berbeda." },
  { "en": "Apa Jarak Hamming Minimum Suatu Kode?", "id": "Jarak Terkecil Antara Dua Codeword Valid." },
  { "en": "Bagaimana Jarak Hamming Terkait Deteksi Kesalahan?", "id": "Dapat Mendeteksi Hingga d(min)-1 Kesalahan." },
  { "en": "Apa Itu Kode Pengulangan (Repetition Code)?", "id": "Mengulang Setiap Bit Data Beberapa Kali." },
  { "en": "Apa Itu Kode Hamming?", "id": "Jenis Kode Koreksi Kesalahan Tunggal." },
  { "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Kode Deteksi Kesalahan Berbasis Polinomial." },
  { "en": "Apakah CRC Dapat Mengoreksi Kesalahan?", "id": "Tidak, CRC Hanya Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu Kode Reed-Solomon?", "id": "Kode Koreksi Kesalahan Non-biner Yang Kuat." },
  { "en": "Di Mana Kode Reed-Solomon Biasa Digunakan?", "id": "CD, DVD, Kode QR, Komunikasi Luar Angkasa." },
  { "en": "Apa Itu Kode Turbo?", "id": "Kode Koreksi Kesalahan Berkinerja Sangat Tinggi." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Code?", "id": "Kode Linier Yang Didefinisikan Matriks Paritas Jarang." },
  { "en": "Apa Itu Modulasi?", "id": "Proses Mengubah Sinyal Informasi Ke Gelombang Pembawa." },
  { "en": "Sebutkan Tiga Jenis Modulasi Dasar?", "id": "Amplitudo, Frekuensi, Dan Fasa." },
  { "en": "Apa Itu Amplitude Shift Keying (ASK)?", "id": "Modulasi Digital Dengan Mengubah Amplitudo." },
  { "en": "Apa Itu Frequency Shift Keying (FSK)?", "id": "Modulasi Digital Dengan Mengubah Frekuensi." },
  { "en": "Apa Itu Phase Shift Keying (PSK)?", "id": "Modulasi Digital Dengan Mengubah Fasa." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Menggabungkan Modulasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Sumber Informasi Diskret?", "id": "Sumber Yang Menghasilkan Simbol Dari Alfabet Terbatas." },
  { "en": "Apa Itu Sumber Informasi Kontinu?", "id": "Sumber Yang Menghasilkan Sinyal Bernilai Kontinu." },
  { "en": "Apa Itu Proses Stokastik?", "id": "Sekumpulan Variabel Acak Yang Diindeks Waktu." },
  { "en": "Apa Itu Sumber Tanpa Memori (Memoryless Source)?", "id": "Simbol Yang Dihasilkan Independen Satu Sama Lain." },
  { "en": "Apa Itu Rantai Markov?", "id": "Model Stokastik Yang Menggambarkan Urutan Peristiwa." },
  { "en": "Apa Itu Kraft's Inequality?", "id": "Syarat Perlu Dan Cukup Eksistensi Kode Awalan." },
  { "en": "Apa Itu Teorema Pengkodean Sumber Shannon?", "id": "Menetapkan Batas Fundamental Kompresi Data." },
  { "en": "Apa Batas Kompresi Data Menurut Shannon?", "id": "Panjang Rata-rata Tidak Bisa Kurang Dari Entropi." },
  { "en": "Apa Itu Pengkodean Aritmetik?", "id": "Metode Pengkodean Sumber Dengan Efisiensi Tinggi." },
  { "en": "Apa Itu Algoritma Lempel-Ziv-Welch (LZW)?", "id": "Algoritma Kompresi Data Berbasis Kamus." },
  { "en": "Apa Itu Kompresi Lossless?", "id": "Kompresi Dimana Data Asli Dapat Dipulihkan." },
  { "en": "Apa Itu Kompresi Lossy?", "id": "Kompresi Dimana Sebagian Data Hilang." },
  { "en": "Berikan Contoh Format Kompresi Lossless?", "id": "ZIP, PNG, FLAC." },
  { "en": "Berikan Contoh Format Kompresi Lossy?", "id": "JPEG, MP3, MPEG." },
  { "en": "Apa Itu Rate-Distortion Theory?", "id": "Mempelajari Trade-off Antara Laju Kompresi Dan Distorsi." },
  { "en": "Apa Itu Saluran Biner Simetris (Binary Symmetric Channel)?", "id": "Model Saluran Diskret Dengan Probabilitas Kesalahan Sama." },
  { "en": "Apa Itu Saluran Biner Hapus (Binary Erasure Channel)?", "id": "Model Saluran Dimana Bit Dapat Dihapus." },
  { "en": "Apa Itu Additive White Gaussian Noise (AWGN) Channel?", "id": "Model Saluran Kontinu Dengan Derau Gaussian." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Daya Sinyal Terhadap Daya Derau." },
  { "en": "Semakin Tinggi SNR, Semakin?", "id": "Baik Kualitas Sinyal Yang Diterima." },
  { "en": "Apa Itu Teorema Pengkodean Saluran Shannon?", "id": "Menyatakan Keberadaan Kode Andal Di Bawah Kapasitas." },
  { "en": "Apa Itu Laju Kode (Code Rate)?", "id": "Rasio Bit Informasi Terhadap Total Bit." },
  { "en": "Apa Itu Decoding Kemungkinan Maksimum (Maximum Likelihood Decoding)?", "id": "Memilih Codeword Yang Paling Mungkin Diterima." },
  { "en": "Apa Itu Diagram Trellis?", "id": "Representasi Grafis Dari Kode Konvolusional." },
  { "en": "Apa Itu Algoritma Viterbi?", "id": "Algoritma Untuk Decoding Kode Konvolusional." },
  { "en": "Apa Itu Kriptografi?", "id": "Ilmu Mengamankan Komunikasi Dari Pihak Ketiga." },
  { "en": "Bagaimana Teori Informasi Berhubungan Dengan Kriptografi?", "id": "Menganalisis Keamanan Sistem Kriptografi." },
  { "en": "Apa Itu Kunci Dalam Kriptografi?", "id": "Informasi Rahasia Untuk Enkripsi Dan Dekripsi." },
  { "en": "Apa Itu Enkripsi Kunci Simetris?", "id": "Menggunakan Kunci Yang Sama Untuk Enkripsi/Dekripsi." },
  { "en": "Apa Itu Enkripsi Kunci Asimetris?", "id": "Menggunakan Kunci Publik Dan Kunci Privat." },
  { "en": "Apa Itu Entropi Diferensial?", "id": "Ukuran Entropi Untuk Variabel Acak Kontinu." },
  { "en": "Apa Itu Divergensi Kullback-Leibler?", "id": "Ukuran Perbedaan Antara Dua Distribusi Probabilitas." },
  { "en": "Apa Itu Kode Polar?", "id": "Kode Koreksi Kesalahan Yang Mencapai Kapasitas Saluran." },
  { "en": "Apa Itu Concatenated Code?", "id": "Menggabungkan Dua Kode Untuk Kinerja Lebih Baik." },
  { "en": "Apa Itu Interleaving?", "id": "Mengubah Urutan Data Untuk Melawan Burst Error." },
  { "en": "Apa Itu Burst Error?", "id": "Kesalahan Yang Terjadi Berurutan Dalam Kelompok." },
  { "en": "Apa Itu Kuantisasi?", "id": "Proses Memetakan Nilai Kontinu Ke Nilai Diskret." },
  { "en": "Apa Itu Sampling?", "id": "Mengubah Sinyal Waktu Kontinu Menjadi Sinyal Diskret." },
  { "en": "Apa Itu Teorema Sampling Nyquist-Shannon?", "id": "Menetapkan Laju Sampling Minimum." },
  { "en": "Apa Itu Aliasing?", "id": "Efek Kesalahan Akibat Laju Sampling Terlalu Rendah." },
  { "en": "Apa Itu Kode Gray?", "id": "Urutan Biner Dimana Dua Nilai Berurutan Berbeda." },
  { "en": "Apa Itu Information Rate?", "id": "Laju Informasi Yang Dihasilkan Sumber." },
  { "en": "Apa Itu Alfabet Sumber?", "id": "Himpunan Semua Simbol Yang Mungkin Dihasilkan." },
  { "en": "Apa Itu Probabilitas Transisi?", "id": "Probabilitas Output Diterima Mengingat Input Dikirim." },
  { "en": "Apa Itu Matriks Transisi Saluran?", "id": "Matriks Yang Berisi Semua Probabilitas Transisi." },
  { "en": "Apa Itu Fading Channel?", "id": "Saluran Komunikasi Dengan Variasi Atenuasi." },
  { "en": "Apa Itu Diversity Technique?", "id": "Teknik Untuk Melawan Efek Fading." },
  { "en": "Apa Itu Kode Fountain?", "id": "Kelas Kode Hapus Yang Efisien." },
  { "en": "Apa Itu Automatic Repeat Request (ARQ)?", "id": "Protokol Kontrol Kesalahan Yang Menggunakan Pengiriman Ulang." },
  { "en": "Apa Itu Hybrid ARQ?", "id": "Menggabungkan ARQ Dengan Koreksi Kesalahan Maju." },
  { "en": "Apa Itu Forward Error Correction (FEC)?", "id": "Menambahkan Redundansi Untuk Koreksi Di Penerima." },
  { "en": "Apa Itu Network Coding?", "id": "Memungkinkan Node Menengah Memanipulasi Paket." },
  { "en": "Apa Itu Joint Source-Channel Coding?", "id": "Menggabungkan Pengkodean Sumber Dan Saluran." },
  { "en": "Apa Itu Entropi Bersyarat (Conditional Entropy)?", "id": "Entropi Variabel Mengingat Variabel Lain." },
  { "en": "Apa Itu Entropi Gabungan (Joint Entropy)?", "id": "Ukuran Ketidakpastian Dari Sepasang Variabel Acak." },
  { "en": "Apa Hubungan Antara Entropi Gabungan Dan Entropi?", "id": "H(X,Y) Kurang Dari Atau Sama Dengan H(X)+H(Y)." },
  { "en": "Apa Itu Aturan Rantai (Chain Rule) Untuk Entropi?", "id": "H(X,Y) Sama Dengan H(X) Ditambah H(Y|X)." },
  { "en": "Apa Itu Entropi Relatif?", "id": "Nama Lain Untuk Divergensi Kullback-Leibler." },
  { "en": "Berapakah Nilai Entropi Dari Hasil Yang Pasti?", "id": "Nol, Karena Tidak Ada Ketidakpastian." },
  { "en": "Apa Itu Pengkodean Shannon-Fano?", "id": "Metode Pengkodean Sumber Berdasarkan Probabilitas Simbol." },
  { "en": "Apa Perbedaan Utama Huffman Dan Shannon-Fano?", "id": "Huffman Optimal, Shannon-Fano Tidak Selalu." },
  { "en": "Apa Itu Pohon Biner?", "id": "Struktur Data Pohon Dengan Maksimal Dua Anak." },
  { "en": "Bagaimana Pohon Biner Digunakan Dalam Pengkodean Huffman?", "id": "Untuk Membangun Codeword Berdasarkan Frekuensi." },
  { "en": "Apa Itu Simbol?", "id": "Unit Dasar Informasi Yang Dihasilkan Sumber." },
  { "en": "Apa Itu Codeword?", "id": "Representasi Biner Dari Suatu Simbol." },
  { "en": "Apa Itu Panjang Codeword Rata-rata?", "id": "Panjang Rata-rata Tertimbang Dari Semua Codeword." },
  { "en": "Apa Itu Efisiensi Kode?", "id": "Rasio Entropi Terhadap Panjang Rata-rata." },
  { "en": "Apa Itu Kode Unik Terdekodekan (Uniquely Decodable Code)?", "id": "Setiap Urutan Codeword Hanya Punya Satu Interpretasi." },
  { "en": "Apakah Semua Kode Awalan Unik Terdekodekan?", "id": "Ya, Semua Kode Awalan Pasti Unik Terdekodekan." },
  { "en": "Apa Itu Kode Instan (Instantaneous Code)?", "id": "Nama Lain Untuk Kode Awalan." },
  { "en": "Apa Itu Kode Golomb?", "id": "Pengkodean Optimal Untuk Distribusi Geometris." },
  { "en": "Apa Itu Checksum?", "id": "Nilai Kecil Untuk Mendeteksi Kesalahan Data." },
  { "en": "Apa Perbedaan CRC Dan Checksum?", "id": "CRC Lebih Kuat Dalam Mendeteksi Kesalahan." },
  { "en": "Apa Itu Matriks Generator (Generator Matrix)?", "id": "Matriks Untuk Menghasilkan Codeword Dalam Kode Linier." },
  { "en": "Apa Itu Matriks Pengecekan Paritas (Parity-Check Matrix)?", "id": "Matriks Untuk Memverifikasi Validitas Suatu Codeword." },
  { "en": "Apa Itu Sindrom (Syndrome) Dalam Pengkodean?", "id": "Hasil Perkalian Vektor Diterima Dengan Matriks Paritas." },
  { "en": "Apa Arti Sindrom Bernilai Nol?", "id": "Tidak Ada Kesalahan Yang Terdeteksi." },
  { "en": "Apa Itu Ruang Kode (Code Space)?", "id": "Himpunan Semua Kemungkinan Codeword yang Valid." },
  { "en": "Apa Itu Kode Linier?", "id": "Kode Dimana Jumlah Dua Codeword Juga Codeword." },
  { "en": "Apa Itu Bobot Hamming (Hamming Weight)?", "id": "Jumlah Bit Bernilai Satu Dalam Vektor." },
  { "en": "Apa Itu Dekoder Hard-Decision?", "id": "Dekoder Yang Menggunakan Nilai Bit Keras (0/1)." },
  { "en": "Apa Itu Dekoder Soft-Decision?", "id": "Dekoder Yang Menggunakan Probabilitas Atau Keandalan Bit." },
  { "en": "Mana Yang Lebih Baik: Dekoder Hard Atau Soft?", "id": "Dekoder Soft-Decision Memberikan Kinerja Lebih Baik." },
  { "en": "Apa Itu Batas Singleton (Singleton Bound)?", "id": "Batas Atas Pada Ukuran Kode Blok." },
  { "en": "Apa Itu Batas Hamming (Hamming Bound)?", "id": "Batas Atas Lainnya Untuk Ukuran Kode." },
  { "en": "Apa Itu Kode Sempurna (Perfect Code)?", "id": "Kode Yang Memenuhi Batas Hamming." },
  { "en": "Apakah Kode Hamming Merupakan Kode Sempurna?", "id": "Ya, Kode Hamming Biner Adalah Sempurna." },
  { "en": "Apa Itu Kode BCH (Bose-Chaudhuri-Hocquenghem)?", "id": "Kelas Kode Siklik Pengoreksi Kesalahan Ganda." },
  { "en": "Apa Itu Kode Siklik?", "id": "Kode Linier Dengan Sifat Pergeseran Siklik." },
  { "en": "Bagaimana Kode Siklik Dihasilkan?", "id": "Menggunakan Polinomial Generator." },
  { "en": "Apa Itu Medan Galois (Galois Field)?", "id": "Medan Finit Dengan Elemen Terbatas." },
  { "en": "Mengapa Medan Galois Penting Dalam Pengkodean?", "id": "Digunakan Untuk Membangun Kode Koreksi Kesalahan." },
  { "en": "Apa Itu Dekode Berlekamp-Massey?", "id": "Algoritma Untuk Mendekode Kode BCH Dan Reed-Solomon." },
  { "en": "Apa Itu Kuantisasi Skalar?", "id": "Mengkuantisasi Setiap Sampel Secara Individual." },
  { "en": "Apa Itu Kuantisasi Vektor?", "id": "Mengkuantisasi Sekelompok Sampel Secara Bersamaan." },
  { "en": "Apa Itu Distorsi?", "id": "Ukuran Ketidakakuratan Setelah Kompresi Atau Transmisi." },
  { "en": "Apa Itu Kesalahan Kuadrat Rata-rata (Mean Squared Error)?", "id": "Ukuran Umum Untuk Distorsi." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Representasi Grafis Dari Skema Modulasi Digital." },
  { "en": "Berapa Banyak Titik Dalam Konstelasi 16-QAM?", "id": "Enam Belas Titik Konstelasi." },
  { "en": "Apa Itu Binary Phase Shift Keying (BPSK)?", "id": "PSK Yang Menggunakan Dua Fasa Berbeda." },
  { "en": "Apa Itu Quadrature Phase Shift Keying (QPSK)?", "id": "PSK Yang Menggunakan Empat Fasa Berbeda." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Rasio Jumlah Bit Salah Terhadap Total Bit." },
  { "en": "Apa Itu Symbol Error Rate (SER)?", "id": "Rasio Jumlah Simbol Salah Terhadap Total Simbol." },
  { "en": "Apa Itu Gray Coding Dalam Modulasi?", "id": "Mengurangi Bit Error Rate Dari Symbol Error." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Digunakan Oleh Sinyal." },
  { "en": "Apa Itu Efisiensi Spektral?", "id": "Laju Data Yang Dapat Dikirim Per Hertz." },
  { "en": "Apa Hubungan Modulasi Dan Efisiensi Spektral?", "id": "Modulasi Orde Tinggi Meningkatkan Efisiensi Spektral." },
  { "en": "Apa Itu Fading Rayleigh?", "id": "Model Statistik Untuk Fading Kanal Nirkabel." },
  { "en": "Apa Itu Fading Rician?", "id": "Model Fading Lainnya Dengan Komponen Dominan." },
  { "en": "Apa Itu Time Diversity?", "id": "Mengirim Sinyal Yang Sama Pada Waktu Berbeda." },
  { "en": "Apa Itu Frequency Diversity?", "id": "Mengirim Sinyal Yang Sama Pada Frekuensi Berbeda." },
  { "en": "Apa Itu Space Diversity?", "id": "Menggunakan Beberapa Antena Di Penerima Atau Pengirim." },
  { "en": "Apa Itu Orthogonal Frequency-Division Multiplexing (OFDM)?", "id": "Teknik Modulasi Untuk Komunikasi Kecepatan Tinggi." },
  { "en": "Apa Keuntungan Utama OFDM?", "id": "Tahan Terhadap Multipath Fading." },
  { "en": "Apa Itu Cyclic Prefix?", "id": "Salinan Akhir Simbol OFDM Di Awal." },
  { "en": "Apa Fungsi Cyclic Prefix Dalam OFDM?", "id": "Menghilangkan Interferensi Antar Simbol." },
  { "en": "Apa Itu Kriptanalisis?", "id": "Ilmu Memecahkan Kode Tanpa Mengetahui Kunci." },
  { "en": "Apa Itu Serangan Brute-Force?", "id": "Mencoba Semua Kemungkinan Kunci." },
  { "en": "Apa Itu Serangan Teks Diketahui (Known-Plaintext Attack)?", "id": "Penyerang Memiliki Akses Ke Teks Asli." },
  { "en": "Apa Itu One-Time Pad?", "id": "Sistem Enkripsi Yang Terbukti Tidak Dapat Dipecahkan." },
  { "en": "Apa Syarat Agar One-Time Pad Aman?", "id": "Kunci Harus Acak, Rahasia, Dan Sama Panjangnya." },
  { "en": "Apa Itu Data Encryption Standard (DES)?", "id": "Standar Enkripsi Simetris Yang Sudah Usang." },
  { "en": "Apa Itu Advanced Encryption Standard (AES)?", "id": "Standar Enkripsi Simetris Yang Digunakan Saat Ini." },
  { "en": "Apa Itu Asymptotic Equipartition Property (AEP)?", "id": "Konsep Penting Dalam Teori Informasi." },
  { "en": "Apa Itu Typical Set?", "id": "Himpunan Urutan Dengan Probabilitas Tertentu." },
  { "en": "Apa Itu Kode Sumber Universal?", "id": "Kompresi Data Tanpa Mengetahui Statistik Sumber." },
  { "en": "Apa Itu Algoritma Lempel-Ziv (LZ77, LZ78)?", "id": "Keluarga Algoritma Kompresi Lossless." },
  { "en": "Apa Itu Transform Coding?", "id": "Mengubah Data Ke Domain Lain Untuk Kompresi." },
  { "en": "Berikan Contoh Transform Coding?", "id": "Discrete Cosine Transform (DCT) Dalam JPEG." },
  { "en": "Apa Itu Run-Length Encoding (RLE)?", "id": "Metode Kompresi Sederhana Untuk Data Berulang." },
  { "en": "Apa Itu Delta Modulation?", "id": "Teknik Konversi Analog Ke Sinyal Digital." },
  { "en": "Apa Itu Pulse-Code Modulation (PCM)?", "id": "Metode Standar Untuk Mendigitalkan Audio." },
  { "en": "Apa Itu Entropi Sumber Gabungan?", "id": "Ketidakpastian Total Dari Beberapa Sumber Informasi." },
  { "en": "Apa Itu Kode Spasi-Waktu (Space-Time Code)?", "id": "Kode Untuk Jaringan MIMO." },
  { "en": "Apa Itu Intersymbol Interference (ISI)?", "id": "Gangguan Satu Simbol Terhadap Simbol Berikutnya." },
  { "en": "Apa Itu Equalizer?", "id": "Filter Untuk Mengurangi Efek Intersymbol Interference." },
  { "en": "Apa Itu Matched Filter?", "id": "Filter Optimal Untuk Memaksimalkan SNR." },
  { "en": "Apa Itu Multiple Access Channel?", "id": "Beberapa Pengguna Mengirim Ke Satu Penerima." },
  { "en": "Apa Itu Broadcast Channel?", "id": "Satu Pengguna Mengirim Ke Banyak Penerima." },
  { "en": "Apa Itu Time-Division Multiple Access (TDMA)?", "id": "Berbagi Saluran Dengan Membagi Waktu." },
  { "en": "Apa Itu Frequency-Division Multiple Access (FDMA)?", "id": "Berbagi Saluran Dengan Membagi Frekuensi." },
  { "en": "Apa Itu Code-Division Multiple Access (CDMA)?", "id": "Berbagi Saluran Dengan Menggunakan Kode Unik." },
  { "en": "Apa Itu Orthogonal Spreading Code?", "id": "Kode Yang Digunakan Dalam Sistem CDMA." },
  { "en": "Apa Itu Walsh-Hadamard Code?", "id": "Contoh Kode Penyebaran Ortogonal." },
  { "en": "Apa Itu Multiple-Input Multiple-Output (MIMO)?", "id": "Sistem Komunikasi Dengan Banyak Antena." },
  { "en": "Apa Keuntungan Sistem MIMO?", "id": "Meningkatkan Kapasitas Dan Keandalan Saluran." },
  { "en": "Apa Itu Multiplexing Gain?", "id": "Peningkatan Laju Data Dari MIMO." },
  { "en": "Apa Itu Diversity Gain?", "id": "Peningkatan Keandalan Dari MIMO." },
  { "en": "Apa Itu Information Bottleneck Method?", "id": "Metode Untuk Mengekstrak Informasi Relevan." },
  { "en": "Apa Itu Fisher Information?", "id": "Ukuran Informasi Tentang Parameter Tidak Dikenal." },
  { "en": "Apa Itu Algorithmic Information Theory?", "id": "Mendefinisikan Informasi Berdasarkan Kompleksitas Kolmogorov." },
  { "en": "Apa Itu Kompleksitas Kolmogorov?", "id": "Panjang Program Komputer Terpendek Untuk Menghasilkan Objek." },
  { "en": "Apa Itu Quantum Information Theory?", "id": "Mempelajari Teori Informasi Dalam Konteks Kuantum." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu Entanglement Kuantum?", "id": "Korelasi Kuat Antara Dua Atau Lebih Qubit." },
  { "en": "Apa Itu Teorema No-Cloning?", "id": "Menyatakan Qubit Sewenang-wenang Tidak Dapat Disalin." },
  { "en": "Apa Itu Teleportasi Kuantum?", "id": "Proses Memindahkan Keadaan Kuantum Ke Lokasi Lain." },
  { "en": "Apa Itu Kriptografi Kuantum?", "id": "Menggunakan Prinsip Kuantum Untuk Komunikasi Aman." },
  { "en": "Apa Itu Quantum Key Distribution (QKD)?", "id": "Protokol Untuk Berbagi Kunci Kriptografi Secara Aman." },
  { "en": "Apa Itu Protokol BB84?", "id": "Contoh Populer Dari Protokol Quantum Key Distribution." },
  { "en": "Apa Itu Pengukuran Von Neumann?", "id": "Jenis Pengukuran Kuantum Paling Dasar." },
  { "en": "Apa Itu Fidelity Dalam Informasi Kuantum?", "id": "Ukuran Seberapa Mirip Dua Keadaan Kuantum." },
  { "en": "Apa Itu Saluran Depolarisasi Kuantum?", "id": "Model Derau Untuk Saluran Komunikasi Kuantum." },
  { "en": "Apa Itu Kode Koreksi Kesalahan Kuantum?", "id": "Melindungi Informasi Kuantum Dari Derau." },
  { "en": "Apa Itu Kode Shor?", "id": "Salah Satu Kode Koreksi Kesalahan Kuantum Pertama." },
  { "en": "Apa Itu Decoding Loop (Loop Decoding)?", "id": "Metode Dekode Iteratif Untuk Kode LDPC." },
  { "en": "Apa Itu Belief Propagation?", "id": "Algoritma Pengiriman Pesan Untuk Inferensi Probabilistik." },
  { "en": "Bagaimana Belief Propagation Digunakan Dalam Pengkodean?", "id": "Untuk Mendekode Kode LDPC Dan Turbo." },
  { "en": "Apa Itu Faktor Graf?", "id": "Representasi Grafis Dari Faktorisasi Suatu Fungsi." },
  { "en": "Apa Itu Batas Varshamov-Gilbert?", "id": "Batas Bawah Pada Ukuran Kode Blok." },
  { "en": "Apa Itu Batas Plotkin?", "id": "Batas Atas Lainnya Pada Ukuran Kode." },
  { "en": "Apa Itu Batas Griesmer?", "id": "Batas Pada Panjang Kode Biner Linier." },
  { "en": "Apa Itu Kode Golay?", "id": "Dua Kode Biner Sempurna Yang Luar Biasa." },
  { "en": "Apa Itu Kode Maksimum Jarak Terpisah (MDS)?", "id": "Kode Yang Memenuhi Batas Singleton." },
  { "en": "Apakah Kode Reed-Solomon Termasuk Kode MDS?", "id": "Ya, Kode Reed-Solomon Adalah Kode MDS." },
  { "en": "Apa Itu Teori Pencarian (Search Theory)?", "id": "Studi Tentang Metode Optimal Mencari Objek." },
  { "en": "Apa Itu Pengujian Hipotesis?", "id": "Metode Statistik Untuk Membuat Keputusan." },
  { "en": "Apa Itu Lemma Neyman-Pearson?", "id": "Memberikan Uji Hipotesis Paling Kuat." },
  { "en": "Apa Itu Deteksi Sinyal?", "id": "Mendeteksi Keberadaan Sinyal Di Tengah Derau." },
  { "en": "Apa Itu Receiver Operating Characteristic (ROC) Curve?", "id": "Grafik Kinerja Untuk Klasifikasi Biner." },
  { "en": "Apa Itu Area Under the Curve (AUC)?", "id": "Ukuran Kinerja Yang Terkait Dengan Kurva ROC." },
  { "en": "Apa Itu Teori Estimasi?", "id": "Cabang Statistik Yang Mengestimasi Nilai Parameter." },
  { "en": "Apa Itu Batas Cramér-Rao?", "id": "Batas Bawah Pada Varians Estimator." },
  { "en": "Apa Itu Estimator Tak Bias (Unbiased Estimator)?", "id": "Estimator Yang Nilai Harapannya Parameter Sebenarnya." },
  { "en": "Apa Itu White Noise?", "id": "Sinyal Acak Dengan Kepadatan Spektral Daya Datar." },
  { "en": "Apa Itu Pink Noise?", "id": "Sinyal Acak Dengan Spektrum Daya Berbanding Terbalik." },
  { "en": "Apa Itu Filter Wiener?", "id": "Filter Optimal Untuk Mengurangi Derau." },
  { "en": "Apa Itu Filter Kalman?", "id": "Filter Rekursif Untuk Mengestimasi Keadaan Sistem." },
  { "en": "Apa Itu Proses Gaussian?", "id": "Proses Stokastik Dimana Setiap Kombinasi Linier." },
  { "en": "Apa Itu Proses Stasioner?", "id": "Proses Stokastik Yang Distribusinya Tidak Berubah." },
  { "en": "Apa Itu Proses Ergodik?", "id": "Proses Stasioner Dimana Rata-rata Waktu Sama." },
  { "en": "Apa Itu Fungsi Autokorelasi?", "id": "Korelasi Sinyal Dengan Versi Tertunda Dirinya." },
  { "en": "Apa Itu Kepadatan Spektral Daya (Power Spectral Density)?", "id": "Distribusi Daya Sinyal Pada Frekuensi." },
  { "en": "Apa Itu Teorema Wiener-Khinchin?", "id": "Menghubungkan Autokorelasi Dengan Kepadatan Spektral Daya." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Mengubah Sinyal Antara Domain Waktu Dan Frekuensi." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Menghitung Transformasi Fourier." },
  { "en": "Apa Itu Jendela (Windowing) Dalam Analisis Sinyal?", "id": "Mengisolasi Sebagian Sinyal Untuk Analisis." },
  { "en": "Sebutkan Contoh Fungsi Jendela?", "id": "Hann, Hamming, Dan Blackman." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematika Pada Dua Fungsi." },
  { "en": "Apa Teorema Konvolusi?", "id": "Transformasi Fourier Dari Konvolusi Adalah Perkalian." },
  { "en": "Apa Itu Impuls Dirac Delta?", "id": "Fungsi Ideal Dengan Nilai Tak Hingga." },
  { "en": "Apa Itu Respon Impuls?", "id": "Output Sistem Saat Diberi Input Impuls." },
  { "en": "Apa Itu Sistem Linear Time-Invariant (LTI)?", "id": "Sistem Yang Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu S-Plane Dalam Teori Kontrol?", "id": "Bidang Kompleks Untuk Analisis Transformasi Laplace." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Transformasi Integral Terkait Dengan Transformasi Fourier." },
  { "en": "Apa Itu Z-Transform?", "id": "Transformasi Untuk Analisis Sinyal Waktu Diskret." },
  { "en": "Apa Itu Filter Digital?", "id": "Sistem Matematis Yang Memodifikasi Sinyal Digital." },
  { "en": "Apa Itu Finite Impulse Response (FIR) Filter?", "id": "Filter Yang Respon Impulsnya Berdurasi Terbatas." },
  { "en": "Apa Itu Infinite Impulse Response (IIR) Filter?", "id": "Filter Yang Respon Impulsnya Berdurasi Tak Terbatas." },
  { "en": "Mana Yang Selalu Stabil: FIR Atau IIR?", "id": "Filter FIR Selalu Stabil." },
  { "en": "Apa Itu Kode Huffman Dinamis (Adaptive Huffman Coding)?", "id": "Memperbarui Pohon Huffman Selama Kompresi." },
  { "en": "Apa Itu Konteks Dalam Kompresi Data?", "id": "Simbol Sebelumnya Yang Mempengaruhi Probabilitas Simbol Berikutnya." },
  { "en": "Apa Itu Model Prediction by Partial Matching (PPM)?", "id": "Model Statistik Adaptif Untuk Kompresi." },
  { "en": "Apa Itu Transformasi Burrows-Wheeler (BWT)?", "id": "Transformasi Blok Data Reversibel." },
  { "en": "Apa Itu Move-to-Front Transform?", "id": "Transformasi Yang Sering Digunakan Setelah BWT." },
  { "en": "Apa Itu Entropy Coding?", "id": "Skema Kompresi Lossless Yang Independen." },
  { "en": "Apa Itu Universal Code?", "id": "Kode Awalan Untuk Bilangan Bulat Positif." },
  { "en": "Apa Itu Elias Gamma Coding?", "id": "Contoh Dari Universal Code." },
  { "en": "Apa Itu Network Information Theory?", "id": "Mempelajari Batas Fundamental Transmisi Informasi." },
  { "en": "Apa Itu Cut-Set Bound?", "id": "Batas Atas Kapasitas Jaringan Komunikasi." },
  { "en": "Apa Itu Relay Channel?", "id": "Saluran Komunikasi Dengan Bantuan Node Relay." },
  { "en": "Apa Itu Interference Channel?", "id": "Saluran Dimana Transmisi Saling Mengganggu." },
  { "en": "Apa Itu Degrees of Freedom (DoF) Dari Kanal?", "id": "Jumlah Sinyal Independen Yang Dapat Dikirim." },
  { "en": "Apa Itu Blind Source Separation?", "id": "Memisahkan Sinyal Sumber Dari Campuran Sinyal." },
  { "en": "Apa Itu Independent Component Analysis (ICA)?", "id": "Metode Komputasi Untuk Memisahkan Sinyal." },
  { "en": "Apa Itu Kode Produk (Product Code)?", "id": "Membangun Kode Baru Dari Kode Yang Ada." },
  { "en": "Apa Itu Kode Tandem?", "id": "Nama Lain Untuk Concatenated Code." },
  { "en": "Apa Itu Iterative Decoding?", "id": "Proses Dekode Berulang Antara Dekoder Komponen." },
  { "en": "Bagaimana Kode Turbo Didekode?", "id": "Menggunakan Prinsip Dekode Iteratif." },
  { "en": "Apa Itu EXIT Chart (Extrinsic Information Transfer Chart)?", "id": "Alat Analisis Untuk Konvergensi Dekoder Iteratif." },
  { "en": "Apa Itu Log-Likelihood Ratio (LLR)?", "id": "Ukuran Keandalan Bit Dalam Dekode." },
  { "en": "Apa Itu Maximum A Posteriori (MAP) Decoding?", "id": "Meminimalkan Probabilitas Kesalahan Simbol Atau Bit." },
  { "en": "Apa Itu BCJR Algorithm?", "id": "Algoritma Untuk Implementasi Dekoder MAP." },
  { "en": "Apa Itu Soft-Output Viterbi Algorithm (SOVA)?", "id": "Modifikasi Algoritma Viterbi Yang Menghasilkan Soft Output." },
  { "en": "Apa Itu Finite Geometry Code?", "id": "Kode Yang Dibangun Menggunakan Geometri Terbatas." },
  { "en": "Apa Itu Algebraic Geometry Code?", "id": "Kode Yang Dibangun Dari Kurva Aljabar." },
  { "en": "Apa Itu List Decoding?", "id": "Dekoder Menghasilkan Daftar Codeword Yang Mungkin." },
  { "en": "Apa Algoritma Sudan?", "id": "Algoritma List Decoding Pertama Untuk Kode Reed-Solomon." },
  { "en": "Apa Itu Fingerprinting (Dalam Konteks Digital)?", "id": "Menanamkan Informasi Unik Ke Dalam Salinan Konten." },
  { "en": "Apa Itu Watermarking?", "id": "Menyembunyikan Pesan Dalam Sinyal Lain." },
  { "en": "Apa Perbedaan Watermarking Dan Steganografi?", "id": "Watermarking Melindungi Sinyal, Steganografi Menyembunyikan Komunikasi." },
  { "en": "Apa Itu Perceptual Coding?", "id": "Kompresi Lossy Berdasarkan Persepsi Manusia." },
  { "en": "Apa Itu Model Psikoakustik?", "id": "Model Ilmiah Tentang Persepsi Suara Manusia." },
  { "en": "Bagaimana MP3 Menggunakan Model Psikoakustik?", "id": "Membuang Informasi Audio Yang Tidak Terdengar." },
  { "en": "Apa Itu Sub-band Coding?", "id": "Membagi Sinyal Menjadi Pita Frekuensi Berbeda." },
  { "en": "Apa Itu Color Subsampling?", "id": "Teknik Kompresi Gambar Berdasarkan Persepsi Warna." },
  { "en": "Apa Itu Chroma Subsampling 4:2:0?", "id": "Skema Subsampling Yang Umum Digunakan." },
  { "en": "Apa Itu Kode Linear Block Code (LBC)?", "id": "Kelas Kode Blok Dengan Sifat Linier." },
  { "en": "Apa Itu Standard Array Decoding?", "id": "Metode Dekode Sistematis Untuk Kode Linier." },
  { "en": "Apa Itu Coset Leader?", "id": "Vektor Dengan Bobot Hamming Terkecil." },
  { "en": "Apa Itu Information Set?", "id": "Posisi Bit Dalam Codeword Yang Sesuai." },
  { "en": "Apa Itu Trellis Coded Modulation (TCM)?", "id": "Menggabungkan Pengkodean Dan Modulasi." },
  { "en": "Siapa Penemu Trellis Coded Modulation?", "id": "Gottfried Ungerboeck." },
  { "en": "Apa Keuntungan Utama Trellis Coded Modulation?", "id": "Meningkatkan Kinerja Tanpa Mengubah Bandwidth." },
  { "en": "Apa Itu Set Partitioning?", "id": "Prinsip Kunci Di Balik Desain TCM." },
  { "en": "Apa Itu Fano's Inequality?", "id": "Menghubungkan Probabilitas Kesalahan Dengan Entropi Bersyarat." },
  { "en": "Apa Itu Data Processing Inequality?", "id": "Menyatakan Informasi Tidak Dapat Diciptakan." },
  { "en": "Apa Itu Markov Chain?", "id": "Urutan Variabel Acak Dengan Sifat Markov." },
  { "en": "Apa Itu Sifat Markov?", "id": "Masa Depan Hanya Bergantung Pada Saat Ini." },
  { "en": "Apa Itu Distribusi Stasioner Rantai Markov?", "id": "Distribusi Probabilitas Yang Tidak Berubah Seiring Waktu." },
  { "en": "Apa Itu Information Geometry?", "id": "Menerapkan Teknik Geometri Diferensial Ke Statistik." },
  { "en": "Apa Itu Manifold Statistik?", "id": "Himpunan Distribusi Probabilitas Yang Membentuk Manifold." },
  { "en": "Apa Itu Metrik Fisher-Rao?", "id": "Metrik Riemann Pada Manifold Statistik." },
  { "en": "Apa Itu Sumber Gaussian?", "id": "Sumber Informasi Yang Menghasilkan Sinyal Gaussian." },
  { "en": "Untuk Laju Tertentu, Kompresi Apa Minimalisasi Distorsi?", "id": "Teori Laju-Distorsi (Rate-Distortion Theory)." },
  { "en": "Apa Fungsi Laju-Distorsi (Rate-Distortion Function)?", "id": "Memberikan Batas Bawah Laju Kompresi." },
  { "en": "Apa Itu Blahut-Arimoto Algorithm?", "id": "Algoritma Iteratif Untuk Menghitung Kapasitas Saluran." },
  { "en": "Apa Itu Saluran Gaussian Vektor?", "id": "Model Saluran AWGN Untuk Sistem MIMO." },
  { "en": "Apa Itu Water-Filling Algorithm?", "id": "Mengalokasikan Daya Optimal Di Saluran Frekuensi-Selektif." },
  { "en": "Apa Itu Frekuensi-Selektif Fading?", "id": "Fading Yang Bervariasi Di Seluruh Bandwidth Sinyal." },
  { "en": "Apa Itu Flat Fading?", "id": "Fading Yang Mempengaruhi Semua Frekuensi Secara Sama." },
  { "en": "Apa Itu Waktu Koherensi (Coherence Time)?", "id": "Durasi Waktu Dimana Respons Kanal Konstan." },
  { "en": "Apa Itu Bandwidth Koherensi (Coherence Bandwidth)?", "id": "Rentang Frekuensi Dimana Kanal Dianggap Datar." },
  { "en": "Apa Itu Secrecy Capacity?", "id": "Laju Maksimum Komunikasi Aman Dan Andal." },
  { "en": "Apa Itu Wiretap Channel?", "id": "Model Saluran Komunikasi Dengan Penyadap." },
  { "en": "Siapa Yang Memperkenalkan Konsep Wiretap Channel?", "id": "Aaron D. Wyner." },
  { "en": "Apa Itu Kode Wyner-Ziv?", "id": "Pengkodean Sumber Dengan Informasi Sampingan." },
  { "en": "Apa Itu Distributed Source Coding?", "id": "Mengkodekan Sumber Terkorelasi Secara Terpisah." },
  { "en": "Apa Itu Teorema Slepian-Wolf?", "id": "Menetapkan Batas Laju Untuk Pengkodean Sumber Terdistribusi." },
  { "en": "Apa Itu Kompresi Video?", "id": "Mengurangi Ukuran Data Video Digital." },
  { "en": "Apa Itu Redundansi Temporal?", "id": "Korelasi Antara Frame Video Yang Berdekatan." },
  { "en": "Apa Itu Redundansi Spasial?", "id": "Korelasi Di Dalam Satu Frame Video." },
  { "en": "Apa Itu Motion Compensation?", "id": "Teknik Untuk Mengurangi Redundansi Temporal." },
  { "en": "Apa Itu Motion Vector?", "id": "Mewakili Gerakan Blok Antar Frame." },
  { "en": "Apa Itu I-frame (Intra-coded frame)?", "id": "Frame Yang Dikodekan Secara Independen." },
  { "en": "Apa Itu P-frame (Predicted frame)?", "id": "Frame Yang Dikodekan Menggunakan Prediksi Dari Frame Sebelumnya." },
  { "en": "Apa Itu B-frame (Bi-predictive frame)?", "id": "Frame Yang Menggunakan Prediksi Dari Frame Sebelumnya Dan Sesudahnya." },
  { "en": "Apa Itu Group of Pictures (GOP)?", "id": "Urutan Frame Dalam Kompresi Video." },
  { "en": "Standar Kompresi Apa Yang Menggunakan Motion Compensation?", "id": "MPEG, H.264, Dan H.265." },
  { "en": "Apa Itu H.264/AVC (Advanced Video Coding)?", "id": "Standar Kompresi Video Yang Sangat Populer." },
  { "en": "Apa Itu H.265/HEVC (High Efficiency Video Coding)?", "id": "Penerus H.264 Dengan Efisiensi Lebih Tinggi." },
  { "en": "Apa Itu Kode Hamming Diperpanjang (Extended Hamming Code)?", "id": "Menambahkan Satu Bit Paritas Ke Kode Hamming." },
  { "en": "Apa Itu Kode Singleton?", "id": "Nama Lain Untuk Kode MDS." },
  { "en": "Apa Itu Kode Hadamard?", "id": "Keluarga Kode Biner Dengan Jarak Besar." },
  { "en": "Apa Itu Matriks Hadamard?", "id": "Matriks Persegi Dengan Entri +1 Dan -1." },
  { "en": "Apa Itu Automorphism Group Dari Sebuah Kode?", "id": "Grup Permutasi Yang Memetakan Kode Ke Dirinya." },
  { "en": "Apa Itu Kode Self-Dual?", "id": "Kode Yang Sama Dengan Kode Dualnya." },
  { "en": "Apa Itu Kode Dual?", "id": "Himpunan Vektor Ortogonal Terhadap Semua Codeword." },
  { "en": "Apa Itu Teorema MacWilliams?", "id": "Menghubungkan Distribusi Bobot Kode Dengan Kode Dualnya." },
  { "en": "Apa Itu Enumerator Bobot (Weight Enumerator)?", "id": "Polinomial Yang Menggambarkan Distribusi Bobot Kode." },
  { "en": "Apa Itu Kode Kuantum?", "id": "Melindungi Keadaan Kuantum Dari Dekokerensi." },
  { "en": "Apa Itu Dekokerensi?", "id": "Kehilangan Koherensi Kuantum Akibat Interaksi." },
  { "en": "Apa Itu Bit Flip Error?", "id": "Jenis Kesalahan Kuantum." },
  { "en": "Apa Itu Phase Flip Error?", "id": "Jenis Kesalahan Kuantum Lainnya." },
  { "en": "Apa Itu Stabilizer Code?", "id": "Kelas Penting Dari Kode Kuantum." },
  { "en": "Apa Itu Kode Topologis?", "id": "Kode Kuantum Dengan Sifat Perlindungan Topologis." },
  { "en": "Apa Itu Surface Code?", "id": "Contoh Terkenal Dari Kode Topologis." },
  { "en": "Apa Itu Channel State Information (CSI)?", "id": "Pengetahuan Tentang Karakteristik Kanal Komunikasi." },
  { "en": "Apa Itu CSI Di Pengirim (CSIT)?", "id": "Informasi Kanal Yang Tersedia Di Pengirim." },
  { "en": "Apa Itu CSI Di Penerima (CSIR)?", "id": "Informasi Kanal Yang Tersedia Di Penerima." },
  { "en": "Bagaimana CSI Biasanya Diperoleh?", "id": "Melalui Transmisi Sinyal Pilot Atau Pelatihan." },
  { "en": "Apa Itu Beamforming?", "id": "Teknik Memfokuskan Sinyal Nirkabel Ke Arah Tertentu." },
  { "en": "Apa Yang Dibutuhkan Untuk Melakukan Beamforming?", "id": "Channel State Information Di Pengirim (CSIT)." },
  { "en": "Apa Itu Massive MIMO?", "id": "Sistem MIMO Dengan Jumlah Antena Sangat Besar." },
  { "en": "Apa Itu Full-Duplex Communication?", "id": "Transmisi Dan Penerimaan Secara Simultan." },
  { "en": "Apa Tantangan Utama Full-Duplex?", "id": "Mengatasi Interferensi Diri (Self-Interference)." },
  { "en": "Apa Itu Index Coding?", "id": "Teknik Pengkodean Untuk Mengurangi Transmisi Broadcast." },
  { "en": "Apa Itu Coded Caching?", "id": "Skema Caching Yang Memanfaatkan Transmisi Terkode." },
  { "en": "Apa Itu Erasure?", "id": "Kesalahan Dimana Posisi Bit Yang Hilang Diketahui." },
  { "en": "Apa Beda Error Dan Erasure?", "id": "Posisi Erasure Diketahui, Posisi Error Tidak." },
  { "en": "Apa Itu Kode Kuantil (Quantile Code)?", "id": "Teknik Untuk Mengkuantisasi Data." },
  { "en": "Apa Itu Companding?", "id": "Mengompresi Dan Mengekspansi Sinyal." },
  { "en": "Apa Itu A-law Dan μ-law?", "id": "Algoritma Companding Yang Digunakan Dalam PCM." },
  { "en": "Apa Itu Kode Elias Delta?", "id": "Contoh Lain Dari Universal Code." },
  { "en": "Apa Itu Fibonacci Coding?", "id": "Sistem Bilangan Untuk Pengkodean Universal." },
  { "en": "Apa Itu T-distribution?", "id": "Distribusi Probabilitas Yang Mirip Distribusi Normal." },
  { "en": "Apa Itu Chi-squared Test?", "id": "Uji Statistik Untuk Menilai Goodness of Fit." },
  { "en": "Apa Itu Hipotesis Nol?", "id": "Pernyataan Default Yang Biasanya Diuji." },
  { "en": "Apa Itu P-value?", "id": "Probabilitas Mendapatkan Hasil Yang Sama Ekstremnya." },
  { "en": "Apa Itu Proses Poisson?", "id": "Model Stokastik Untuk Menghitung Peristiwa." },
  { "en": "Apa Itu Proses Wiener?", "id": "Model Matematika Untuk Gerak Brown." },
  { "en": "Apa Itu Lemma Borel-Cantelli?", "id": "Hasil Dalam Teori Probabilitas." },
  { "en": "Apa Itu Hukum Bilangan Besar?", "id": "Teorema Yang Menjelaskan Hasil Jangka Panjang." },
  { "en": "Apa Itu Teorema Limit Pusat?", "id": "Menjelaskan Distribusi Rata-rata Sampel." },
  { "en": "Apa Itu Variabel Acak?", "id": "Variabel Yang Nilainya Hasil Dari Fenomena Acak." },
  { "en": "Apa Itu Fungsi Distribusi Kumulatif (CDF)?", "id": "Probabilitas Variabel Acak Kurang Dari Nilai." },
  { "en": "Apa Itu Fungsi Kepadatan Probabilitas (PDF)?", "id": "Turunan Dari Fungsi Distribusi Kumulatif." },
  { "en": "Apa Itu Nilai Harapan (Expected Value)?", "id": "Rata-rata Tertimbang Dari Semua Kemungkinan Nilai." },
  { "en": "Apa Itu Varians?", "id": "Ukuran Sebaran Dari Sekumpulan Data." },
  { "en": "Apa Itu Standar Deviasi?", "id": "Akar Kuadrat Dari Varians." },
  { "en": "Apa Itu Kovarians?", "id": "Ukuran Variabilitas Bersama Dua Variabel Acak." },
  { "en": "Apa Itu Koefisien Korelasi?", "id": "Ukuran Hubungan Linier Antara Dua Variabel." },
  { "en": "Apa Itu Teorema Bayes?", "id": "Menjelaskan Probabilitas Suatu Peristiwa." },
  { "en": "Apa Itu Probabilitas A Priori?", "id": "Probabilitas Sebelum Bukti Dipertimbangkan." },
  { "en": "Apa Itu Probabilitas A Posteriori?", "id": "Probabilitas Setelah Bukti Dipertimbangkan." },
  { "en": "Apa Itu Jaringan Bayesian?", "id": "Model Grafis Probabilistik." },
  { "en": "Apa Itu Hidden Markov Model (HMM)?", "id": "Rantai Markov Dengan Keadaan Yang Tidak Teramati." },
  { "en": "Apa Itu Algoritma Forward-Backward?", "id": "Algoritma Untuk Menghitung Probabilitas Dalam HMM." },
  { "en": "Apa Itu Data Denoising?", "id": "Proses Menghilangkan Derau Dari Sinyal." },
  { "en": "Apa Itu Wavelet Transform?", "id": "Representasi Sinyal Dalam Domain Waktu-Frekuensi." },
  { "en": "Apa Itu Kode Huffman Kanonik?", "id": "Bentuk Standar Dari Kode Huffman." },
  { "en": "Apa Itu Kode Ternary?", "id": "Kode Yang Menggunakan Tiga Simbol (0,1,2)." },
  { "en": "Apa Itu Alphabet?", "id": "Himpunan Simbol Yang Digunakan Dalam Komunikasi." },
  { "en": "Apa Itu String?", "id": "Urutan Simbol Dari Suatu Alphabet." },
  { "en": "Apa Itu Bahasa Formal?", "id": "Himpunan String Di Atas Suatu Alphabet." },
  { "en": "Apa Itu Automata Finit?", "id": "Model Komputasi Abstrak Untuk Mengenali Bahasa." },
  { "en": "Apa Itu Mesin Turing?", "id": "Model Komputasi Yang Dapat Mensimulasikan Algoritma." },
  { "en": "Apa Itu Teori Kompleksitas Komputasi?", "id": "Mempelajari Sumber Daya Yang Dibutuhkan Algoritma." },
  { "en": "Apa Itu Kelas P (Polynomial Time)?", "id": "Masalah Yang Dapat Dipecahkan Dalam Waktu Polinomial." },
  { "en": "Apa Itu Kelas NP (Nondeterministic Polynomial Time)?", "id": "Masalah Yang Solusinya Dapat Diverifikasi Cepat." },
  { "en": "Apa Itu Masalah P Versus NP?", "id": "Pertanyaan Terbuka Terbesar Dalam Ilmu Komputer." },
  { "en": "Apa Itu Random Access Machine (RAM)?", "id": "Model Abstrak Komputer Untuk Analisis Algoritma." },
  { "en": "Apa Itu Teori Antrian (Queueing Theory)?", "id": "Studi Matematika Tentang Antrian Atau Garis Tunggu." },
  { "en": "Apa Itu Proses Kelahiran-Kematian (Birth-Death Process)?", "id": "Jenis Proses Markov Untuk Model Antrian." },
  { "en": "Apa Itu Hukum Little?", "id": "Menghubungkan Jumlah Pelanggan, Laju Kedatangan, Waktu Tunggu." },
  { "en": "Apa Itu Notasi Kendall?", "id": "Notasi Standar Untuk Mendeskripsikan Model Antrian." },
  { "en": "Apa Itu M/M/1 Queue?", "id": "Model Antrian Sederhana Dengan Satu Server." },
  { "en": "Apa Itu Teori Permainan (Game Theory)?", "id": "Studi Model Matematika Tentang Interaksi Strategis." },
  { "en": "Apa Itu Keseimbangan Nash (Nash Equilibrium)?", "id": "Kondisi Dimana Tidak Ada Pemain Yang Diuntungkan." },
  { "en": "Apa Itu Permainan Zero-Sum?", "id": "Keuntungan Satu Pemain Adalah Kerugian Pemain Lain." },
  { "en": "Apa Itu Dilema Tahanan (Prisoner's Dilemma)?", "id": "Contoh Paradoks Terkenal Dalam Teori Permainan." },
  { "en": "Apa Itu Spread Spectrum?", "id": "Menyebarkan Sinyal Pada Bandwidth Yang Lebih Lebar." },
  { "en": "Apa Dua Jenis Utama Spread Spectrum?", "id": "Direct Sequence Dan Frequency Hopping." },
  { "en": "Apa Itu Direct-Sequence Spread Spectrum (DSSS)?", "id": "Mengalikan Sinyal Data Dengan Sinyal Derau." },
  { "en": "Apa Itu Spreading Code?", "id": "Sinyal Mirip Derau Yang Digunakan Dalam DSSS." },
  { "en": "Apa Itu Processing Gain?", "id": "Rasio Bandwidth Sinyal Tersebar Terhadap Data." },
  { "en": "Apa Itu Frequency-Hopping Spread Spectrum (FHSS)?", "id": "Mengubah Frekuensi Pembawa Secara Cepat." },
  { "en": "Apa Itu Hopping Sequence?", "id": "Pola Perubahan Frekuensi Dalam FHSS." },
  { "en": "Apa Keuntungan Utama Spread Spectrum?", "id": "Tahan Terhadap Interferensi Dan Penyadapan." },
  { "en": "Di Mana FHSS Digunakan?", "id": "Bluetooth, Wi-Fi Awal, Dan Sistem Militer." },
  { "en": "Apa Itu Jarak Euklides (Euclidean Distance)?", "id": "Jarak Garis Lurus Antara Dua Titik." },
  { "en": "Bagaimana Jarak Euklides Digunakan Dalam Dekode?", "id": "Untuk Dekode Kemungkinan Maksimum Di Kanal AWGN." },
  { "en": "Apa Itu Decision Boundary?", "id": "Batas Yang Memisahkan Wilayah Keputusan." },
  { "en": "Apa Itu Voronoi Region?", "id": "Wilayah Keputusan Untuk Titik Konstelasi." },
  { "en": "Apa Itu Error Vector Magnitude (EVM)?", "id": "Ukuran Kinerja Kualitas Sinyal Modulasi." },
  { "en": "Apa Itu Eye Diagram?", "id": "Tampilan Osiloskop Untuk Menganalisis Sinyal Digital." },
  { "en": "Apa Yang Diindikasikan Oleh Mata Yang Terbuka Lebar?", "id": "Kualitas Sinyal Yang Baik Dengan Sedikit ISI." },
  { "en": "Apa Itu Nyquist ISI Criterion?", "id": "Kondisi Matematis Untuk Transmisi Bebas ISI." },
  { "en": "Apa Itu Raised-Cosine Filter?", "id": "Filter Pembentuk Pulsa Untuk Menghilangkan ISI." },
  { "en": "Apa Itu Roll-off Factor?", "id": "Parameter Dari Raised-Cosine Filter." },
  { "en": "Apa Itu Kode Manchester?", "id": "Skema Pengkodean Garis Yang Menggabungkan Data Dan Clock." },
  { "en": "Apa Itu Non-Return-to-Zero (NRZ) Code?", "id": "Skema Pengkodean Garis Sederhana." },
  { "en": "Apa Itu Non-Return-to-Zero Inverted (NRZI) Code?", "id": "Varian NRZ Dimana Transisi Mewakili Bit 1." },
  { "en": "Apa Itu Bipolar Encoding?", "id": "Menggunakan Tiga Level Tegangan: Positif, Negatif, Nol." },
  { "en": "Apa Itu Clock Recovery?", "id": "Proses Mengekstrak Sinyal Clock Dari Data." },
  { "en": "Apa Itu Scrambler Dalam Komunikasi?", "id": "Membuat Urutan Bit Terlihat Lebih Acak." },
  { "en": "Mengapa Scrambling Dibutuhkan?", "id": "Mencegah Urutan Bit Panjang Dan Membantu Clock Recovery." },
  { "en": "Apa Itu Kode Diferensial?", "id": "Mengkodekan Informasi Dalam Perubahan Antar Simbol." },
  { "en": "Apa Keuntungan Kode Diferensial?", "id": "Tahan Terhadap Ambiguitas Fasa Pembawa." },
  { "en": "Apa Itu Differential PSK (DPSK)?", "id": "Varian PSK Yang Menggunakan Pengkodean Diferensial." },
  { "en": "Apa Itu Offset QPSK (OQPSK)?", "id": "Varian QPSK Dengan Transisi Fasa Terbatas." },
  { "en": "Apa Itu Minimum Shift Keying (MSK)?", "id": "Jenis Modulasi Fase Kontinu." },
  { "en": "Apa Itu Gaussian MSK (GMSK)?", "id": "MSK Dengan Filter Gaussian." },
  { "en": "Di Mana GMSK Digunakan?", "id": "Dalam Sistem Komunikasi Seluler GSM." },
  { "en": "Apa Itu Orthogonality?", "id": "Sifat Sinyal Yang Tidak Saling Mengganggu." },
  { "en": "Apa Itu Proses Gram-Schmidt?", "id": "Metode Untuk Mengortogonalkan Sekumpulan Vektor." },
  { "en": "Apa Itu Basis Sinyal?", "id": "Himpunan Sinyal Ortogonal Untuk Merepresentasikan Sinyal Lain." },
  { "en": "Apa Itu Kode Dupleks?", "id": "Teknik Mengizinkan Komunikasi Dua Arah." },
  { "en": "Apa Itu Time-Division Duplex (TDD)?", "id": "Menggunakan Slot Waktu Berbeda Untuk Uplink/Downlink." },
  { "en": "Apa Itu Frequency-Division Duplex (FDD)?", "id": "Menggunakan Pita Frekuensi Berbeda Untuk Uplink/Downlink." },
  { "en": "Apa Itu Simplex Communication?", "id": "Komunikasi Yang Terjadi Hanya Dalam Satu Arah." },
  { "en": "Apa Itu Half-Duplex Communication?", "id": "Komunikasi Dua Arah Tapi Tidak Bersamaan." },
  { "en": "Apa Itu Maximum Ratio Combining (MRC)?", "id": "Teknik Menggabungkan Sinyal Diversity." },
  { "en": "Apa Itu Kaskade Kode (Code Concatenation)?", "id": "Menggabungkan Kode Dalam Dan Kode Luar." },
  { "en": "Apa Fungsi Kode Luar?", "id": "Biasanya Mengoreksi Error Yang Tersisa." },
  { "en": "Apa Fungsi Kode Dalam?", "id": "Biasanya Mengoreksi Error Acak Dari Kanal." },
  { "en": "Apa Itu Galois Field Polynomial?", "id": "Polinomial Yang Koefisiennya Dari Medan Galois." },
  { "en": "Apa Itu Polinomial Primitif?", "id": "Polinomial Ireducibel Yang Digunakan Membangun Medan." },
  { "en": "Apa Itu Algoritma Euclid?", "id": "Algoritma Efisien Untuk Menghitung Pembagi Terbesar." },
  { "en": "Apa Itu Pergeseran Siklik?", "id": "Menggeser Elemen Vektor Dan Memindahkan Akhir." },
  { "en": "Apa Itu Teorema Sisa Tiongkok?", "id": "Hasil Dari Teori Bilangan Abstrak." },
  { "en": "Apa Itu Fungsi Phi Euler?", "id": "Menghitung Bilangan Bulat Positif Hingga N." },
  { "en": "Apa Itu Teorema Kecil Fermat?", "id": "Hasil Dasar Dalam Teori Bilangan." },
  { "en": "Apa Itu Kriptografi Kurva Eliptik (ECC)?", "id": "Pendekatan Kunci Publik Berbasis Kurva Eliptik." },
  { "en": "Apa Keuntungan Utama ECC Dibanding RSA?", "id": "Ukuran Kunci Lebih Kecil Untuk Keamanan Sama." },
  { "en": "Apa Itu Masalah Logaritma Diskrit?", "id": "Masalah Sulit Yang Mendasari Keamanan Kriptografi." },
  { "en": "Apa Itu Masalah Faktorisasi Bilangan Bulat?", "id": "Masalah Sulit Yang Mendasari Keamanan RSA." },
  { "en": "Apa Itu Fungsi Satu Arah (One-Way Function)?", "id": "Fungsi Yang Mudah Dihitung Tapi Sulit Dibalik." },
  { "en": "Apa Itu Fungsi Trapdoor?", "id": "Fungsi Satu Arah Yang Mudah Dibalik." },
  { "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?", "id": "Penyerang Menyadap Dan Mengubah Komunikasi." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Verifikasi Identitas." },
  { "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Sistem Untuk Mengelola Kunci Publik." },
  { "en": "Apa Itu Certificate Authority (CA)?", "id": "Entitas Terpercaya Yang Menerbitkan Sertifikat Digital." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "File Elektronik Yang Mengikat Kunci Publik." },
  { "en": "Apa Itu Standar X.509?", "id": "Standar Format Untuk Sertifikat Kunci Publik." },
  { "en": "Apa Itu Perfect Forward Secrecy (PFS)?", "id": "Melindungi Sesi Masa Lalu Dari Kompromi Kunci." },
  { "en": "Apa Itu Kunci Ephemeral?", "id": "Kunci Kriptografi Yang Bersifat Sementara." },
  { "en": "Apa Itu Linear Feedback Shift Register (LFSR)?", "id": "Register Geser Yang Bit Inputnya Fungsi Linier." },
  { "en": "Apa Kegunaan LFSR?", "id": "Menghasilkan Urutan Biner Mirip Acak." },
  { "en": "Apa Itu M-sequence (Maximum Length Sequence)?", "id": "Urutan Pseudorandom Yang Dihasilkan LFSR." },
  { "en": "Apa Itu Teori Informasi Algoritmik?", "id": "Mempelajari Kompleksitas Objek Secara Komputasi." },
  { "en": "Apa Itu Inkompresibilitas Algoritmik?", "id": "String Yang Tidak Dapat Dikompresi." },
  { "en": "Apa Itu Probabilitas Algoritmik?", "id": "Probabilitas Apriori Universal Solomonoff." },
  { "en": "Apa Itu Teori Informasi Jaringan?", "id": "Mempelajari Batas Fundamental Aliran Informasi." },
  { "en": "Apa Itu Efek Pipa (Pipelining)?", "id": "Memproses Beberapa Tahap Secara Bersamaan." },
  { "en": "Apa Itu Flit (Flow Control Unit)?", "id": "Unit Data Jaringan Tingkat Link." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
  { "en": "Apa Itu Kode Jala (Mesh Code)?", "id": "Kode Yang Dibangun Di Atas Graf Jala." },
  { "en": "Apa Itu Kode Reed-Muller?", "id": "Kelas Kode Koreksi Kesalahan Biner." },
  { "en": "Apa Itu Transformasi Reed-Muller?", "id": "Ekspresi Fungsi Boolean Sebagai Polinomial." },
  { "en": "Apa Itu Desain Kombinatorial?", "id": "Studi Tentang Himpunan Bagian Dengan Sifat Tertentu." },
  { "en": "Bagaimana Desain Kombinatorial Terkait Dengan Kode?", "id": "Beberapa Kode Dapat Dibangun Dari Desain." },
  { "en": "Apa Itu Desain Steiner?", "id": "Jenis Desain Blok Kombinatorial." },
  { "en": "Apa Itu Kode Kuadratik Sisa (QR)?", "id": "Kelas Kode Siklik Dengan Sifat Baik." },
  { "en": "Apa Itu Kode Alternan?", "id": "Kelas Kode Aljabar-Geometri Yang Kuat." },
  { "en": "Apa Itu Kode Goppa?", "id": "Subkelas Dari Kode Alternan." },
  { "en": "Apa Itu Sistem Kripto McEliece?", "id": "Sistem Kripto Kunci Publik Berbasis Kode." },
  { "en": "Kode Apa Yang Digunakan Dalam Kripto McEliece?", "id": "Biasanya Menggunakan Kode Goppa Biner." },
  { "en": "Apa Keuntungan Sistem Kripto McEliece?", "id": "Diyakini Tahan Terhadap Serangan Komputer Kuantum." },
  { "en": "Apa Itu Masalah Dekode Sindrom?", "id": "Masalah Komputasi Yang Mendasari Kripto Berbasis Kode." },
  { "en": "Apa Itu Information Set Decoding (ISD)?", "id": "Algoritma Untuk Menyelesaikan Masalah Dekode Sindrom." },
  { "en": "Apa Itu Kompresi Seismik?", "id": "Mengurangi Ukuran Data Seismik." },
  { "en": "Apa Itu Pengkodean Entropi Kontekstual-Adaptif Biner Aritmetik (CABAC)?", "id": "Bentuk Efisien Pengkodean Entropi." },
  { "en": "Di Mana CABAC Digunakan?", "id": "Dalam Standar Kompresi Video H.264/AVC." },
  { "en": "Apa Itu Huffman Coding Multi-simbol?", "id": "Varian Huffman Yang Mengkodekan Blok Simbol." },
  { "en": "Apa Itu Tunstall Coding?", "id": "Pengkodean Sumber Panjang Variabel Ke Panjang Tetap." },
  { "en": "Apa Itu Kode Anti-Gray?", "id": "Kode Dimana Bobot Hamming Maksimal." },
  { "en": "Apa Itu Kode Bergerak Terbatas (RLL)?", "id": "Mengkodekan Data Untuk Membatasi Urutan Bit." },
  { "en": "Mengapa Kode RLL Penting Dalam Penyimpanan Magnetik?", "id": "Membantu Sinkronisasi Waktu Dan Mengurangi Interferensi." },
  { "en": "Apa Itu Partial Response Maximum Likelihood (PRML)?", "id": "Metode Deteksi Sinyal Dalam Drive Disk." },
  { "en": "Apa Itu Kapasitas Holografik?", "id": "Batas Atas Entropi Dalam Wilayah Ruang." },
  { "en": "Apa Itu Teori Informasi Semantik?", "id": "Mempelajari Makna Dan Kebenaran Informasi." },
  { "en": "Apa Itu Bit Paritas Densitas Rendah (LDPC)?", "id": "Nama Lain Untuk Low-Density Parity-Check Code." },
  { "en": "Apa Itu Kode Raptor?", "id": "Contoh Praktis Pertama Dari Kode Fountain." },
  { "en": "Apa Itu Kode LT (Luby Transform)?", "id": "Kelas Kode Fountain Pertama Yang Efisien." },
  { "en": "Apa Sifat Rateless Dari Kode Fountain?", "id": "Dapat Menghasilkan Jumlah Codeword Tak Terbatas." },
  { "en": "Di Mana Kode Fountain Berguna?", "id": "Transmisi Data Melalui Saluran Hapus." },
  { "en": "Apa Itu Network Simulator 3 (NS-3)?", "id": "Simulator Jaringan Diskret Untuk Penelitian." },
  { "en": "Apa Itu MATLAB?", "id": "Lingkungan Komputasi Numerik Untuk Simulasi." },
  { "en": "Apa Itu Simulink?", "id": "Lingkungan Grafis Dalam MATLAB Untuk Simulasi." },
  { "en": "Apa Itu Radio Perangkat Lunak (SDR)?", "id": "Sistem Komunikasi Radio Berbasis Perangkat Lunak." },
  { "en": "Apa Itu GNU Radio?", "id": "Toolkit Perangkat Lunak Gratis Untuk SDR." },
  { "en": "Apa Itu Universal Software Radio Peripheral (USRP)?", "id": "Perangkat Keras Untuk Radio Perangkat Lunak." },
  { "en": "Apa Itu Cooperative Communication?", "id": "Beberapa Node Bekerja Sama Mentransmisikan Data." },
  { "en": "Apa Itu Amplify-and-Forward (AF)?", "id": "Strategi Relay Yang Memperkuat Sinyal Diterima." },
  { "en": "Apa Itu Decode-and-Forward (DF)?", "id": "Strategi Relay Yang Mendekode Dan Mengkode Ulang." },
  { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel Terdesentralisasi Tanpa Infrastruktur." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Jaringan Node Sensor Spasial Terdistribusi." },
  { "en": "Apa Tantangan Energi Dalam WSN?", "id": "Node Biasanya Ditenagai Oleh Baterai Terbatas." },
  { "en": "Apa Itu Energy Harvesting?", "id": "Mengumpulkan Energi Dari Sumber Eksternal." },
  { "en": "Apa Itu Komunikasi Cahaya Tampak (VLC)?", "id": "Menggunakan Cahaya Tampak Untuk Transmisi Data." },
  { "en": "Apa Itu Li-Fi (Light Fidelity)?", "id": "Teknologi Komunikasi Nirkabel Berbasis Cahaya." },
  { "en": "Apa Keuntungan Li-Fi Dibanding Wi-Fi?", "id": "Bandwidth Lebih Besar Dan Keamanan Lebih Baik." },
  { "en": "Apa Itu Komunikasi Molekuler?", "id": "Transmisi Informasi Menggunakan Molekul." },
  { "en": "Apa Itu Deep Space Network (DSN)?", "id": "Jaringan Komunikasi Antariksa Internasional NASA." },
  { "en": "Apa Itu Delay-Tolerant Networking (DTN)?", "id": "Arsitektur Jaringan Untuk Lingkungan Tertunda." },
  { "en": "Apa Itu Protokol Bundle?", "id": "Protokol Utama Dalam Delay-Tolerant Networking." },
  { "en": "Apa Itu Teori Informasi Fungsional?", "id": "Mempelajari Sistem Yang Menghasilkan Fungsi." },
  { "en": "Apa Itu Kolmogorov Structure Function?", "id": "Mendefinisikan Kompleksitas Model Statistik." },
  { "en": "Apa Itu Minimum Description Length (MDL) Principle?", "id": "Prinsip Formal Dari Ockham's Razor." },
  { "en": "Apa Itu Ockham's Razor?", "id": "Prinsip Memilih Penjelasan Paling Sederhana." },
  { "en": "Apa Itu Stokastik Kompleksitas?", "id": "Ukuran Kompleksitas Berdasarkan Prinsip MDL." },
  { "en": "Apa Itu Peramalan (Forecasting)?", "id": "Proses Membuat Prediksi Tentang Masa Depan." },
  { "en": "Apa Itu Analisis Runtun Waktu (Time Series)?", "id": "Metode Analisis Urutan Titik Data." },
  { "en": "Apa Itu Model Autoregressive (AR)?", "id": "Model Statistik Untuk Analisis Runtun Waktu." },
  { "en": "Apa Itu Model Moving Average (MA)?", "id": "Model Lain Untuk Analisis Runtun Waktu." },
  { "en": "Apa Itu Model ARMA (Autoregressive Moving Average)?", "id": "Menggabungkan Model AR Dan MA." },
  { "en": "Apa Itu Model ARIMA (Autoregressive Integrated Moving Average)?", "id": "Generalisasi Dari Model ARMA." },
  { "en": "Apa Itu Teori Informasi Kausal?", "id": "Kerangka Kerja Untuk Mendefinisikan Aliran Informasi Kausal." },
  { "en": "Apa Itu Transfer Entropy?", "id": "Ukuran Ketergantungan Berarah Antara Runtun Waktu." },
  { "en": "Apa Itu Compressed Sensing?", "id": "Teknik Akuisisi Sinyal Di Bawah Laju Nyquist." },
  { "en": "Apa Syarat Agar Compressed Sensing Bekerja?", "id": "Sinyal Harus Jarang (Sparse) Dalam Domain Tertentu." },
  { "en": "Apa Itu Basis Jarang (Sparse Basis)?", "id": "Domain Dimana Sinyal Memiliki Sedikit Koefisien." },
  { "en": "Apa Itu Restricted Isometry Property (RIP)?", "id": "Sifat Matriks Pengukuran Dalam Compressed Sensing." },
  { "en": "Apa Itu Basis Pursuit?", "id": "Algoritma Untuk Rekonstruksi Sinyal Jarang." },
  { "en": "Apa Itu Entropi Tsallis?", "id": "Generalisasi Dari Entropi Boltzmann-Gibbs." },
  { "en": "Apa Itu Entropi Rényi?", "id": "Keluarga Ukuran Entropi Lainnya." },
  { "en": "Apa Itu Entropi Min (Min-Entropy)?", "id": "Ukuran Ketidakpastian Terburuk Suatu Variabel." },
  { "en": "Apa Itu Randomness Extraction?", "id": "Proses Menghasilkan Bit Acak Hampir Sempurna." },
  { "en": "Apa Itu Leftover Hash Lemma?", "id": "Hasil Kunci Dalam Ekstraksi Keacakan." },
  { "en": "Apa Itu Kode Polar Kuantum?", "id": "Konstruksi Kode Kuantum Berdasarkan Polarisasi." },
  { "en": "Apa Itu Batas HSW (Holevo-Schumacher-Westmoreland)?", "id": "Batas Atas Kapasitas Klasik Saluran Kuantum." },
  { "en": "Apa Itu Informasi Holevo?", "id": "Batas Atas Informasi Timbal Balik." },
  { "en": "Apa Itu Kapasitas Klasik-Kuantum (cc-capacity)?", "id": "Kapasitas Untuk Mengirim Informasi Klasik." },
  { "en": "Apa Itu Channel Coding With Feedback?", "id": "Model Dimana Penerima Mengirim Umpan Balik." },
  { "en": "Apakah Umpan Balik Meningkatkan Kapasitas Saluran Tanpa Memori?", "id": "Tidak, Kapasitas Tidak Meningkat." },
  { "en": "Bagaimana Umpan Balik Dapat Membantu?", "id": "Dapat Menyederhanakan Skema Pengkodean." },
  { "en": "Apa Itu Skema Schalkwijk-Kailath?", "id": "Skema Pengkodean Untuk Kanal AWGN Dengan Umpan Balik." },
  { "en": "Apa Itu Kode Irregular LDPC?", "id": "Kode LDPC Dengan Distribusi Derajat Tidak Seragam." },
  { "en": "Apa Itu Kode Accumulate Ulang (Repeat-Accumulate Code)?", "id": "Contoh Sederhana Kode Mirip Turbo." },
  { "en": "Apa Itu Efek Tebing (Cliff Effect)?", "id": "Penurunan Kinerja Tajam Saat SNR Turun." },
  { "en": "Apa Itu Shaping Gain?", "id": "Peningkatan Kinerja Dari Pembentukan Konstelasi." },
  { "en": "Apa Itu Pembentukan Konstelasi (Constellation Shaping)?", "id": "Membuat Titik Konstelasi Tidak Seragam." },
  { "en": "Apa Itu Probabilistic Shaping?", "id": "Menggunakan Probabilitas Tidak Seragam Untuk Titik Konstelasi." },
  { "en": "Apa Itu Geometric Shaping?", "id": "Memindahkan Posisi Titik Konstelasi." },
  { "en": "Apa Itu Kode Seluler (Cellular Code)?", "id": "Kode Yang Digunakan Dalam Jaringan Seluler." },
  { "en": "Apa Itu Kode Spatially Coupled LDPC?", "id": "Kelas Kode LDPC Dengan Kinerja Sangat Baik." },
  { "en": "Apa Itu Decoupling Dalam Konteks MIMO?", "id": "Mengubah Kanal MIMO Menjadi Kanal Paralel." },
  { "en": "Apa Itu Singular Value Decomposition (SVD)?", "id": "Faktorisasi Matriks Yang Berguna Dalam MIMO." },
  { "en": "Apa Itu Precoding?", "id": "Pemrosesan Sinyal Di Pengirim Untuk Menyederhanakan Penerima." },
  { "en": "Apa Itu Zero-Forcing Precoding?", "id": "Precoding Yang Menghilangkan Interferensi Antar Pengguna." },
  { "en": "Apa Itu Dirty Paper Coding (DPC)?", "id": "Teknik Precoding Non-linier Yang Optimal." },
  { "en": "Apa Itu Tomasi-Federated Learning?", "id": "Jenis Pembelajaran Mesin Terdistribusi." },
  { "en": "Apa Itu Gradient Descent?", "id": "Algoritma Optimisasi Untuk Menemukan Minimum Fungsi." },
  { "en": "Apa Itu Loss Function?", "id": "Fungsi Yang Mengukur Kesalahan Model." },
  { "en": "Apa Itu Overfitting?", "id": "Model Terlalu Sesuai Dengan Data Latihan." },
  { "en": "Apa Itu Regularization?", "id": "Teknik Untuk Mencegah Overfitting." },
  { "en": "Apa Itu Cross-Validation?", "id": "Teknik Untuk Menilai Seberapa Baik Model Bekerja." },
  { "en": "Apa Itu Training Set?", "id": "Data Yang Digunakan Untuk Melatih Model." },
  { "en": "Apa Itu Test Set?", "id": "Data Yang Digunakan Untuk Menguji Model." },
  { "en": "Apa Itu Validation Set?", "id": "Data Untuk Menyesuaikan Hyperparameter Model." },
  { "en": "Apa Itu Hyperparameter?", "id": "Parameter Yang Ditetapkan Sebelum Pelatihan Dimulai." },
  { "en": "Apa Itu L1 Regularization (Lasso)?", "id": "Menambahkan Pinalti Absolut Ke Fungsi Loss." },
  { "en": "Apa Itu L2 Regularization (Ridge)?", "id": "Menambahkan Pinalti Kuadrat Ke Fungsi Loss." },
  { "en": "Apa Efek Dari Regularisasi L1?", "id": "Dapat Menghasilkan Model Yang Jarang (Sparse)." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (Artificial Neural Network)?", "id": "Model Komputasi Terinspirasi Dari Otak Biologis." },
  { "en": "Apa Itu Neuron Dalam Jaringan Saraf?", "id": "Unit Komputasi Dasar Dalam Jaringan Saraf." },
  { "en": "Apa Itu Fungsi Aktivasi?", "id": "Menentukan Output Neuron Berdasarkan Inputnya." },
  { "en": "Sebutkan Contoh Fungsi Aktivasi?", "id": "Sigmoid, ReLU, Dan Tanh." },
  { "en": "Apa Itu Rectified Linear Unit (ReLU)?", "id": "Fungsi Aktivasi Yang Populer Dan Sederhana." },
  { "en": "Apa Itu Backpropagation?", "id": "Algoritma Untuk Melatih Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Deep Learning?", "id": "Pembelajaran Mesin Menggunakan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jenis Jaringan Saraf Untuk Memproses Data Grid." },
  { "en": "Di Mana CNN Sering Digunakan?", "id": "Pengenalan Gambar Dan Visi Komputer." },
  { "en": "Apa Itu Lapisan Konvolusional?", "id": "Lapisan Inti Dalam Jaringan Saraf Konvolusional." },
  { "en": "Apa Itu Lapisan Pooling?", "id": "Mengurangi Dimensi Spasial Representasi." },
  { "en": "Apa Itu Recurrent Neural Network (RNN)?", "id": "Jenis Jaringan Saraf Untuk Data Berurutan." },
  { "en": "Apa Masalah Vanishing Gradient?", "id": "Masalah Dalam Pelatihan RNN Dan Jaringan Dalam." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Arsitektur RNN Untuk Mengatasi Vanishing Gradient." },
  { "en": "Apa Itu Gated Recurrent Unit (GRU)?", "id": "Varian LSTM Yang Lebih Sederhana." },
  { "en": "Apa Itu Generative Adversarial Network (GAN)?", "id": "Arsitektur Untuk Menghasilkan Data Baru." },
  { "en": "Apa Dua Komponen Utama GAN?", "id": "Generator Dan Diskriminator." },
  { "en": "Apa Itu Autoencoder?", "id": "Jaringan Saraf Untuk Pembelajaran Representasi Tanpa Pengawasan." },
  { "en": "Apa Itu Denoising Autoencoder?", "id": "Autoencoder Yang Dilatih Untuk Merekonstruksi Input Bersih." },
  { "en": "Apa Itu Variational Autoencoder (VAE)?", "id": "Jenis Autoencoder Yang Bersifat Generatif." },
  { "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Interaksi Antara Komputer Dan Bahasa Manusia." },
  { "en": "Apa Itu Tokenization?", "id": "Memecah Teks Menjadi Bagian-bagian Kecil." },
  { "en": "Apa Itu Stop Words?", "id": "Kata-kata Umum Yang Sering Dihapus." },
  { "en": "Apa Itu Stemming?", "id": "Mengurangi Kata Ke Bentuk Dasarnya." },
  { "en": "Apa Itu Lemmatization?", "id": "Mengubah Kata Ke Bentuk Kamus Dasarnya." },
  { "en": "Apa Itu Bag-of-Words (BoW) Model?", "id": "Representasi Teks Yang Mengabaikan Urutan Kata." },
  { "en": "Apa Itu Term Frequency-Inverse Document Frequency (TF-IDF)?", "id": "Ukuran Statistik Untuk Pentingnya Kata." },
  { "en": "Apa Itu Word Embedding?", "id": "Representasi Vektor Dari Kata-kata." },
  { "en": "Apa Itu Word2Vec?", "id": "Model Untuk Menghasilkan Word Embedding." },
  { "en": "Apa Itu GloVe (Global Vectors for Word Representation)?", "id": "Algoritma Lain Untuk Word Embedding." },
  { "en": "Apa Itu Model Transformer?", "id": "Arsitektur Jaringan Saraf Berbasis Perhatian." },
  { "en": "Apa Itu Mekanisme Perhatian (Attention Mechanism)?", "id": "Memungkinkan Model Fokus Pada Bagian Penting." },
  { "en": "Apa Itu BERT (Bidirectional Encoder Representations from Transformers)?", "id": "Model Bahasa Berbasis Transformer." },
  { "en": "Apa Itu Reinforcement Learning?", "id": "Agen Belajar Dengan Berinteraksi Dengan Lingkungan." },
  { "en": "Apa Itu Agen Dalam Reinforcement Learning?", "id": "Entitas Yang Membuat Keputusan." },
  { "en": "Apa Itu Lingkungan Dalam Reinforcement Learning?", "id": "Dunia Tempat Agen Beroperasi." },
  { "en": "Apa Itu Reward?", "id": "Umpan Balik Positif Yang Diterima Agen." },
  { "en": "Apa Itu Policy?", "id": "Strategi Yang Digunakan Agen Untuk Bertindak." },
  { "en": "Apa Itu Value Function?", "id": "Memprediksi Reward Masa Depan Yang Diharapkan." },
  { "en": "Apa Itu Model-Based Reinforcement Learning?", "id": "Agen Mempelajari Model Lingkungan." },
  { "en": "Apa Itu Model-Free Reinforcement Learning?", "id": "Agen Belajar Langsung Dari Pengalaman." },
  { "en": "Apa Itu Q-Learning?", "id": "Algoritma Reinforcement Learning Model-Free Yang Populer." },
  { "en": "Apa Itu Dilema Eksplorasi-Eksploitasi?", "id": "Memilih Antara Tindakan Dikenal Dan Mencoba Baru." },
  { "en": "Apa Itu Semantic Coding?", "id": "Pengkodean Yang Memanfaatkan Makna Informasi." },
  { "en": "Apa Itu Goal-Oriented Communication?", "id": "Komunikasi Yang Dioptimalkan Untuk Tugas Tertentu." },
  { "en": "Apa Itu Age of Information (AoI)?", "id": "Ukuran Kesegaran Informasi Di Penerima." },
  { "en": "Kapan Age of Information Menjadi Penting?", "id": "Dalam Sistem Real-time Dan Kontrol." },
  { "en": "Apa Itu Zero-Error Capacity?", "id": "Laju Tertinggi Untuk Transmisi Tanpa Kesalahan." },
  { "en": "Apa Itu Graf Kebingungan (Confusability Graph)?", "id": "Graf Yang Mewakili Simbol Yang Dapat Dibingungkan." },
  { "en": "Apa Itu Bilangan Kemerdekaan (Independence Number) Graf?", "id": "Ukuran Himpunan Simpul Independen Terbesar." },
  { "en": "Bagaimana Kapasitas Zero-Error Terkait Dengan Graf?", "id": "Terkait Dengan Bilangan Kemerdekaan Graf." },
  { "en": "Apa Itu Kapasitas Shannon Dari Graf?", "id": "Batas Asimtotik Kapasitas Zero-Error." },
  { "en": "Apa Itu Produk Tensor Graf?", "id": "Operasi Pada Graf Untuk Menganalisis Kapasitas." },
  { "en": "Apa Itu Pentagon (C5)?", "id": "Graf Siklus Dengan Lima Simpul." },
  { "en": "Berapa Kapasitas Shannon Dari Pentagon?", "id": "Akar Kuadrat Dari Lima." },
  { "en": "Siapa Yang Membuktikan Kapasitas Shannon Dari Pentagon?", "id": "László Lovász." },
  { "en": "Apa Itu Kode Acak (Random Code)?", "id": "Kode Yang Dibangun Secara Acak." },
  { "en": "Apa Itu Argumen Pengkodean Acak?", "id": "Metode Pembuktian Yang Digunakan Shannon." },
  { "en": "Apa Itu Metode Probabilistik?", "id": "Membuktikan Keberadaan Sesuatu Dengan Probabilitas." },
  { "en": "Apa Itu Sampling Penting (Importance Sampling)?", "id": "Teknik Untuk Mengurangi Varians Dalam Simulasi." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Metode Komputasi Yang Mengandalkan Pengambilan Sampel Acak." },
  { "en": "Apa Itu Rantai Markov Monte Carlo (MCMC)?", "id": "Kelas Algoritma Untuk Mengambil Sampel." },
  { "en": "Apa Itu Algoritma Metropolis-Hastings?", "id": "Contoh Terkenal Dari Algoritma MCMC." },
  { "en": "Apa Itu Gibbs Sampling?", "id": "Algoritma MCMC Lainnya." },
  { "en": "Apa Itu Annealing Tiruan (Simulated Annealing)?", "id": "Metode Optimisasi Probabilistik." },
  { "en": "Apa Itu Algoritma Genetik?", "id": "Algoritma Pencarian Yang Terinspirasi Oleh Evolusi." },
  { "en": "Apa Itu Kromosom Dalam Algoritma Genetik?", "id": "Representasi Dari Solusi Kandidat." },
  { "en": "Apa Itu Crossover (Pindah Silang)?", "id": "Operasi Untuk Menggabungkan Solusi Dalam Algoritma Genetik." },
  { "en": "Apa Itu Mutasi?", "id": "Perubahan Acak Dalam Solusi." },
  { "en": "Apa Itu Sistem Inferensi Fuzzy?", "id": "Sistem Berbasis Aturan Yang Menggunakan Logika Fuzzy." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Bentuk Logika Multi-nilai." },
  { "en": "Apa Beda Logika Fuzzy Dan Logika Klasik?", "id": "Fuzzy Mengizinkan Nilai Antara Benar Dan Salah." },
  { "en": "Apa Itu Himpunan Fuzzy?", "id": "Himpunan Dengan Derajat Keanggotaan." },
  { "en": "Apa Itu Fuzzifikasi?", "id": "Mengubah Input Krisp Menjadi Himpunan Fuzzy." },
  { "en": "Apa Itu Defuzzifikasi?", "id": "Mengubah Himpunan Fuzzy Menjadi Output Krisp." },
  { "en": "Apa Itu Basis Pengetahuan (Knowledge Base)?", "id": "Penyimpanan Informasi Dalam Sistem Pakar." },
  { "en": "Apa Itu Mesin Inferensi?", "id": "Menerapkan Aturan Logis Ke Basis Pengetahuan." },
  { "en": "Apa Itu Sistem Pakar (Expert System)?", "id": "Program Komputer Yang Meniru Manusia Ahli." },
  { "en": "Apa Itu Jaringan Semantik?", "id": "Representasi Pengetahuan Berbasis Graf." },
  { "en": "Apa Itu Ontologi?", "id": "Representasi Formal Pengetahuan Dalam Suatu Domain." },
  { "en": "Apa Itu Resource Description Framework (RDF)?", "id": "Standar Untuk Mendeskripsikan Sumber Daya Web." },
  { "en": "Apa Itu Semantic Web?", "id": "Ekstensi Web Dengan Makna Semantik." },
  { "en": "Apa Itu Klastering (Clustering)?", "id": "Mengelompokkan Objek Serupa Secara Bersama-sama." },
  { "en": "Apa Itu Algoritma K-Means?", "id": "Algoritma Klastering Yang Populer." },
  { "en": "Apa Itu Klastering Hirarkis?", "id": "Membangun Hirarki Klaster." },
  { "en": "Apa Itu Dendrogram?", "id": "Diagram Pohon Yang Memvisualisasikan Klaster Hirarkis." },
  { "en": "Apa Itu DBSCAN (Density-Based Spatial Clustering of Applications with Noise)?", "id": "Algoritma Klastering Berbasis Kepadatan." },
  { "en": "Apa Itu Pengurangan Dimensi?", "id": "Mengurangi Jumlah Variabel Dalam Data." },
  { "en": "Apa Itu Principal Component Analysis (PCA)?", "id": "Teknik Pengurangan Dimensi Linier." },
  { "en": "Apa Itu t-SNE (t-Distributed Stochastic Neighbor Embedding)?", "id": "Teknik Untuk Visualisasi Data Dimensi Tinggi." },
  { "en": "Apa Itu Manifold Learning?", "id": "Pendekatan Pengurangan Dimensi Non-Linier." },
  { "en": "Apa Itu Isomap?", "id": "Algoritma Manifold Learning." },
  { "en": "Apa Itu Locally Linear Embedding (LLE)?", "id": "Algoritma Pengurangan Dimensi Non-Linier Lainnya." },
  { "en": "Apa Itu Klasifikasi?", "id": "Tugas Memprediksi Label Kelas Diskrit." },
  { "en": "Apa Itu Regresi?", "id": "Tugas Memprediksi Nilai Numerik Kontinu." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Model Klasifikasi Supervised Yang Kuat." },
  { "en": "Apa Itu Hyperplane?", "id": "Batas Keputusan Dalam Support Vector Machine." },
  { "en": "Apa Itu Margin Dalam SVM?", "id": "Jarak Antara Hyperplane Dan Titik Terdekat." },
  { "en": "Apa Itu Kernel Trick?", "id": "Metode Untuk Klasifikasi Non-Linier Menggunakan SVM." },
  { "en": "Apa Itu Pohon Keputusan (Decision Tree)?", "id": "Model Seperti Pohon Untuk Membuat Keputusan." },
  { "en": "Apa Itu Simpul (Node) Dalam Pohon Keputusan?", "id": "Mewakili Tes Pada Atribut." },
  { "en": "Apa Itu Daun (Leaf) Dalam Pohon Keputusan?", "id": "Mewakili Hasil Atau Keputusan." },
  { "en": "Apa Itu Information Gain?", "id": "Ukuran Untuk Memilih Atribut Terbaik." },
  { "en": "Apa Itu Gini Impurity?", "id": "Ukuran Lain Untuk Pemisahan Dalam Pohon Keputusan." },
  { "en": "Apa Itu Pruning?", "id": "Mengurangi Ukuran Pohon Untuk Menghindari Overfitting." },
  { "en": "Apa Itu Random Forest?", "id": "Metode Ensemble Yang Menggunakan Banyak Pohon Keputusan." },
  { "en": "Apa Itu Metode Ensemble?", "id": "Menggabungkan Beberapa Model Untuk Kinerja Lebih Baik." },
  { "en": "Apa Itu Bagging (Bootstrap Aggregating)?", "id": "Teknik Ensemble Untuk Mengurangi Varians." },
  { "en": "Apa Itu Boosting?", "id": "Teknik Ensemble Yang Mengubah Model Lemah." },
  { "en": "Apa Itu AdaBoost?", "id": "Algoritma Boosting Adaptif." },
  { "en": "Apa Itu Gradient Boosting?", "id": "Algoritma Boosting Yang Mengoptimalkan Fungsi Loss." },
  { "en": "Apa Itu XGBoost?", "id": "Implementasi Gradient Boosting Yang Efisien." },
  { "en": "Apa Itu Naive Bayes Classifier?", "id": "Klasifikasi Probabilistik Berdasarkan Teorema Bayes." },
  { "en": "Mengapa Disebut Naive (Naif)?", "id": "Karena Mengasumsikan Independensi Antar Fitur." },
  { "en": "Apa Itu Regresi Linier?", "id": "Model Untuk Hubungan Linier Antar Variabel." },
  { "en": "Apa Itu Regresi Logistik?", "id": "Model Regresi Untuk Klasifikasi Biner." },
  { "en": "Apa Itu Fungsi Sigmoid?", "id": "Fungsi Yang Memetakan Nilai Ke Probabilitas." },
  { "en": "Apa Itu Topik Modeling?", "id": "Menemukan Topik Abstrak Dalam Kumpulan Dokumen." },
  { "en": "Apa Itu Latent Dirichlet Allocation (LDA)?", "id": "Model Generatif Populer Untuk Topik Modeling." },
  { "en": "Apa Itu Sistem Rekomendasi?", "id": "Sistem Yang Memprediksi Preferensi Pengguna." },
  { "en": "Apa Itu Collaborative Filtering?", "id": "Teknik Rekomendasi Berdasarkan Perilaku Pengguna." },
  { "en": "Apa Itu Content-Based Filtering?", "id": "Teknik Rekomendasi Berdasarkan Atribut Item." },
  { "en": "Apa Itu Matrix Factorization?", "id": "Teknik Umum Dalam Collaborative Filtering." },
  { "en": "Apa Itu Cold Start Problem?", "id": "Masalah Rekomendasi Untuk Pengguna Atau Item Baru." },
  { "en": "Apa Itu Anomaly Detection?", "id": "Mengidentifikasi Item Atau Peristiwa Yang Tidak Biasa." },
  { "en": "Apa Itu Outlier?", "id": "Titik Data Yang Berbeda Secara Signifikan." },
  { "en": "Apa Itu Isolation Forest?", "id": "Algoritma Efisien Untuk Deteksi Anomali." },
  { "en": "Apa Itu One-Class SVM?", "id": "Varian SVM Untuk Deteksi Anomali." },
  { "en": "Apa Itu Network Intrusion Detection System (NIDS)?", "id": "Sistem Yang Mendeteksi Aktivitas Jaringan Berbahaya." },
  { "en": "Apa Itu Signature-Based Detection?", "id": "Mendeteksi Ancaman Berdasarkan Pola Yang Dikenal." },
  { "en": "Apa Itu Anomaly-Based Detection?", "id": "Mendeteksi Penyimpangan Dari Perilaku Normal." },
  { "en": "Apa Itu False Positive Rate?", "id": "Tingkat Peringatan Salah." },
  { "en": "Apa Itu False Negative Rate?", "id": "Tingkat Kegagalan Mendeteksi Ancaman." },
  { "en": "Apa Itu CAP Theorem?", "id": "Teorema Tentang Tiga Jaminan Sistem Terdistribusi." },
  { "en": "Apa Tiga Jaminan Dalam Teorema CAP?", "id": "Konsistensi, Ketersediaan, Dan Toleransi Partisi." },
  { "en": "Apa Implikasi Dari Teorema CAP?", "id": "Sistem Hanya Dapat Menjamin Dua Dari Tiga." },
  { "en": "Apa Itu Basis Data NoSQL?", "id": "Basis Data Yang Tidak Menggunakan Model Relasional." },
  { "en": "Sebutkan Tipe Basis Data NoSQL?", "id": "Key-Value, Document, Column-Family, Graph." },
  { "en": "Apa Itu Sharding?", "id": "Mempartisi Basis Data Secara Horizontal." },
  { "en": "Apa Itu Replication?", "id": "Menyimpan Salinan Data Yang Sama." },
  { "en": "Apa Itu Eventual Consistency?", "id": "Model Konsistensi Yang Menjamin Konsistensi Seiring Waktu." },
  { "en": "Apa Itu Strong Consistency?", "id": "Setiap Operasi Baca Mengembalikan Tulis Terbaru." },
  { "en": "Apa Itu Paxos?", "id": "Protokol Untuk Mencapai Konsensus Dalam Sistem." },
  { "en": "Apa Itu Raft Consensus Algorithm?", "id": "Alternatif Paxos Yang Lebih Mudah Dipahami." },
  { "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi Yang Diamankan Kriptografi." },
  { "en": "Apa Itu Proof-of-Work (PoW)?", "id": "Mekanisme Konsensus Yang Digunakan Bitcoin." },
  { "en": "Apa Itu Proof-of-Stake (PoS)?", "id": "Mekanisme Konsensus Alternatif Yang Hemat Energi." },
  { "en": "Apa Itu Smart Contract?", "id": "Program Yang Dijalankan Di Atas Blockchain." },
  { "en": "Apa Itu Byzantine Fault Tolerance (BFT)?", "id": "Kemampuan Sistem Mentolerir Kegagalan Sewenang-wenang." },
  { "en": "Apa Itu Two-Phase Commit Protocol (2PC)?", "id": "Protokol Komit Atomik Dalam Basis Data." },
  { "en": "Apa Itu Information Retrieval (IR)?", "id": "Ilmu Mencari Informasi Dalam Dokumen." },
  { "en": "Apa Itu Inverted Index?", "id": "Struktur Data Pengindeksan Yang Paling Penting." },
  { "en": "Apa Itu PageRank?", "id": "Algoritma Yang Digunakan Google Search." },
  { "en": "Apa Itu Crawler?", "id": "Program Otomatis Yang Menjelajahi Web." },
  { "en": "Apa Itu Precision?", "id": "Fraksi Item Relevan Di Antara Item Ditemukan." },
  { "en": "Apa Itu Recall?", "id": "Fraksi Item Relevan Yang Berhasil Ditemukan." },
  { "en": "Apa Itu F1-Score?", "id": "Rata-rata Harmonis Dari Precision Dan Recall." },
  { "en": "Apa Itu Cognitive Radio?", "id": "Radio Cerdas Yang Dapat Mengubah Parameternya." },
  { "en": "Apa Itu Spectrum Sensing?", "id": "Mendeteksi Saluran Komunikasi Yang Tidak Digunakan." },
  { "en": "Apa Itu Dynamic Spectrum Access?", "id": "Mengizinkan Pengguna Sekunder Mengakses Spektrum." },
  { "en": "Apa Itu Physical Layer Security?", "id": "Mengamankan Komunikasi Menggunakan Karakteristik Kanal." },
  { "en": "Apa Itu Artificial Noise?", "id": "Menambahkan Derau Buatan Untuk Mengganggu Penyadap." },
  { "en": "Apa Itu Secret Key Generation?", "id": "Menghasilkan Kunci Rahasia Dari Kanal Nirkabel." },
  { "en": "Apa Itu Molecular Communication?", "id": "Bidang Baru Yang Menggunakan Molekul Untuk Informasi." },
  { "en": "Apa Itu Teori Informasi Biologis?", "id": "Menerapkan Teori Informasi Ke Sistem Biologis." },
  { "en": "Apa Itu Kode Genetik?", "id": "Aturan Yang Digunakan Sel Untuk Menerjemahkan DNA." },
  { "en": "Apa Itu Neuroinformatika?", "id": "Bidang Yang Mempelajari Data Ilmu Saraf." },
  { "en": "Apa Itu Brain-Computer Interface (BCI)?", "id": "Jalur Komunikasi Langsung Antara Otak Dan Perangkat." },
  { "en": "Apa Itu Electroencephalography (EEG)?", "id": "Metode Perekaman Aktivitas Listrik Otak." },
  { "en": "Apa Itu Computational Linguistics?", "id": "Studi Bahasa Manusia Dari Perspektif Komputasi." },
  { "en": "Apa Itu Chomsky Hierarchy?", "id": "Hirarki Kelas Bahasa Formal." },
  { "en": "Apa Itu Finite State Transducer (FST)?", "id": "Automata Finit Yang Menghasilkan Output." },
  { "en": "Apa Itu Conditional Random Field (CRF)?", "id": "Model Statistik Untuk Pelabelan Data Berurutan." },
  { "en": "Apa Itu Information Foraging Theory?", "id": "Menerapkan Model Pencarian Makanan Untuk Manusia." },
  { "en": "Apa Itu Chernoff Bound?", "id": "Batas Atas Pada Ekor Distribusi." },
  { "en": "Apa Itu Hoeffding's Inequality?", "id": "Hasil Probabilitas Pada Jumlah Variabel Terikat." },
  { "en": "Apa Itu Vapnik-Chervonenkis (VC) Dimension?", "id": "Ukuran Kapasitas Model Klasifikasi Statistik." },
  { "en": "Apa Itu Structural Risk Minimization (SRM)?", "id": "Prinsip Untuk Menyeimbangkan Kompleksitas Dan Kesalahan." },
  { "en": "Apa Itu PAC (Probably Approximately Correct) Learning?", "id": "Kerangka Kerja Untuk Analisis Pembelajaran Mesin." },
  { "en": "Apa Itu Rademacher Complexity?", "id": "Ukuran Kekayaan Kelas Fungsi." },
  { "en": "Apa Itu Bias-Variance Tradeoff?", "id": "Menyeimbangkan Dua Sumber Kesalahan Dalam Model." },
  { "en": "Apa Itu Model Dengan Bias Tinggi?", "id": "Model Terlalu Sederhana (Underfitting)." },
  { "en": "Apa Itu Model Dengan Varians Tinggi?", "id": "Model Terlalu Kompleks (Overfitting)." },
  { "en": "Apa Itu No Free Lunch Theorem?", "id": "Menyatakan Tidak Ada Model Terbaik Tunggal." },
  { "en": "Apa Itu Occam's Razor?", "id": "Prinsip Yang Menyukai Penjelasan Paling Sederhana." },
  { "en": "Apa Itu Bayesian Information Criterion (BIC)?", "id": "Kriteria Untuk Pemilihan Model." },
  { "en": "Apa Itu Akaike Information Criterion (AIC)?", "id": "Kriteria Lain Untuk Pemilihan Model." },
  { "en": "Apa Itu Fisher Information Matrix?", "id": "Matriks Yang Mendeskripsikan Informasi Fisher." },
  { "en": "Apa Itu Universal Hashing?", "id": "Memilih Fungsi Hash Secara Acak." },
  { "en": "Apa Itu Cuckoo Hashing?", "id": "Skema Hashing Dengan Kinerja Kasus Terburuk." }



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
