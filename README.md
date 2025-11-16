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


  { "en": "Apa Nama Ibukota Negara Tanzania?", "id": "Dodoma." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya The School of Athens?", "id": "Raphael." },
  { "en": "Apa Sebutan Untuk Studi Tentang Gunung Berapi?", "id": "Vulkanologi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segi Delapan?", "id": "10 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Land Rover?", "id": "Inggris." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Ti?", "id": "Titanium." },
  { "en": "Siapakah Yang Menulis Kisah Petualangan The Jungle Book?", "id": "Rudyard Kipling." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Vodka?", "id": "Gandum Atau Kentang." },
  { "en": "Kapan Peringatan Hari Guru Sedunia Diperingati?", "id": "5 Oktober." },
  { "en": "Apa Nama Proses Pergerakan Air Melewati Membran?", "id": "Osmosis." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Hawaii?", "id": "Liliuokalani." },
  { "en": "Apa Istilah Untuk Pukulan Di Bawah Standar Par Golf?", "id": "Birdie." },
  { "en": "Apa Nama Mata Uang Resmi Negara Filipina?", "id": "Peso Filipina." },
  { "en": "Dari Benua Manakah Asal Tanaman Tembakau?", "id": "Amerika." },
  { "en": "Proses Ikatan Partikel Sedimen Menjadi Batuan Disebut?", "id": "Sementasi." },
  { "en": "Siapa Penulis Novel The Adventures of Tom Sawyer?", "id": "Mark Twain." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Ovarium?", "id": "Estrogen Dan Progesteron." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Paskah?", "id": "Kristen." },
  { "en": "Apa Sebutan Untuk Bintang Terdekat Dengan Tata Surya?", "id": "Proxima Centauri." },
  { "en": "Berapa Jumlah Gigi Susu Pada Anak-Anak?", "id": "20 Gigi." },
  { "en": "Siapakah Yang Memproklamasikan Kemerdekaan Negara Vietnam?", "id": "Ho Chi Minh." },
  { "en": "Apa Akronim Untuk Organisasi Buruh Internasional?", "id": "ILO (International Labour Organization)." },
  { "en": "Di Kota Manakah Terdapat Basilika Santo Petrus?", "id": "Vatikan." },
  { "en": "Hewan Apa Yang Dikenal Memiliki Ingatan Yang Kuat?", "id": "Gajah." },
  { "en": "Apa Nama Stadion Sepak Bola Terbesar Di Dunia?", "id": "Stadion Rungrado 1 Mei." },
  { "en": "Siapa Nama Dewa Perang Dan Petir Nordik?", "id": "Thor." },
  { "en": "Berapa Jumlah Negara Anggota Perserikatan Bangsa-Bangsa (PBB)?", "id": "193 Negara." },
  { "en": "Apa Sebutan Untuk Aliran Sungai Bawah Tanah?", "id": "Akuifer." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Praha?", "id": "Ceko." },
  { "en": "Apa Nama Alat Musik Tiup Dari Tanduk Hewan?", "id": "Shofar." },
  { "en": "Siapakah Yang Mengembangkan Teori Kuantum?", "id": "Max Planck." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Newton?", "id": "Interferensi Cahaya." },
  { "en": "Berapa Jumlah Bait Dalam Lagu Kebangsaan Indonesia Raya?", "id": "3 Bait." },
  { "en": "Jaringan Apa Yang Menghubungkan Antar Saraf?", "id": "Sinapsis." },
  { "en": "Negara Apa Yang Dijuluki Negeri Kiwi?", "id": "Selandia Baru." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Ekspresionisme?", "id": "Edvard Munch." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Lampu Neon?", "id": "Gas Neon." },
  { "en": "Apa Nama Sungai Terpanjang Di Benua Afrika?", "id": "Sungai Nil." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Penerbangan?", "id": "Wright Bersaudara." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Alergi?", "id": "Alergi Dan Imunologi." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Hoki Lapangan?", "id": "11 Pemain." },
  { "en": "Apa Nama Ibukota Negara Panama?", "id": "Kota Panama." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel Frankenstein?", "id": "Victor Frankenstein." },
  { "en": "Kekurangan Vitamin B2 Dapat Menyebabkan Penyakit Apa?", "id": "Ariboflavinosis." },
  { "en": "Apa Nama Jembatan Ikonik Yang Melintasi Sungai Thames?", "id": "Tower Bridge." },
  { "en": "Organ Apa Yang Berfungsi Menghasilkan Sel Darah Merah?", "id": "Sumsum Tulang." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Patung Liberty?", "id": "Frédéric Auguste Bartholdi." },
  { "en": "Apa Istilah Untuk Kesalahan Langkah Dalam Basket?", "id": "Traveling." },
  { "en": "Dimana Lokasi Petra, Kota Batu Yang Hilang?", "id": "Yordania." },
  { "en": "Apa Sebutan Untuk Studi Tentang Otot?", "id": "Miologi." },
  { "en": "Siapa Pemimpin Gerakan Hak-Hak Sipil Di India?", "id": "Mahatma Gandhi." },
  { "en": "Apa Nama Gurun Terbesar Di Iran?", "id": "Dasht-e Kavir." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Bola?", "id": "0 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Racun?", "id": "Toksikologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kembang Api?", "id": "Tiongkok." },
  { "en": "Siapakah Raja Terakhir Dari Dinasti Romanov Rusia?", "id": "Tsar Nicholas II." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Sisa Organisme?", "id": "Batuan Sedimen Organik." },
  { "en": "Apa Nama Titik Tertinggi Di Afrika?", "id": "Gunung Kilimanjaro." },
  { "en": "Siapakah Yang Menciptakan Mesin Diesel Pertama?", "id": "Rudolf Diesel." },
  { "en": "Apa Nama Tari Tradisional Dari Spanyol?", "id": "Flamenco." },
  { "en": "Huruf Apa Yang Melambangkan Angka 50 Dalam Romawi?", "id": "L." },
  { "en": "Apa Satuan Standar Internasional Untuk Kapasitansi Listrik?", "id": "Farad." },
  { "en": "Siapakah Kaisar Bizantium Yang Paling Terkenal?", "id": "Justinian I." },
  { "en": "Apa Nama Sosis Kering Khas Dari Italia?", "id": "Salami." },
  { "en": "Apa Nama Spesies Kura-Kura Terbesar Di Dunia?", "id": "Penyu Belimbing." },
  { "en": "Apa Akronim Untuk European Space Agency?", "id": "ESA." },
  { "en": "Berapa Jumlah Diagonal Sisi Pada Sebuah Balok?", "id": "12 Diagonal Sisi." },
  { "en": "Apa Nama Ibukota Negara Chili?", "id": "Santiago." },
  { "en": "Siapa Penulis Novel The Chronicles Of Narnia?", "id": "C. S. Lewis." },
  { "en": "Apa Istilah Untuk Studi Tentang Tulisan Tangan?", "id": "Grafologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Spanyol?", "id": "Banteng." },
  { "en": "Apa Ibukota Provinsi Sumatera Barat?", "id": "Padang." },
  { "en": "Siapa Nama Dewa Matahari Dalam Mitologi Romawi?", "id": "Sol." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Kaca?", "id": "Silika." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'Los Blancos'?", "id": "Real Madrid." },
  { "en": "Apa Sebutan Untuk Garis Batas Antara Siang Dan Malam?", "id": "Terminator." },
  { "en": "Siapa Raja Pertama Kerajaan Kutai Martadipura?", "id": "Kudungga." },
  { "en": "Apa Sebutan Untuk Orang Yang Mengoleksi Koin?", "id": "Numismatis." },
  { "en": "Dimana Lokasi Air Terjun Victoria Yang Megah?", "id": "Zambia Dan Zimbabwe." },
  { "en": "Berapa Jumlah Gigi Geraham Bungsu Pada Orang Dewasa?", "id": "4 Gigi." },
  { "en": "Apa Istilah Untuk Pergerakan Lempeng Bumi Saling Bergeser?", "id": "Transform." },
  { "en": "Siapakah Bapak Klasifikasi Ilmiah Makhluk Hidup?", "id": "Carolus Linnaeus." },
  { "en": "Apa Sebutan Untuk Proses Penguraian Tanpa Mikroorganisme?", "id": "Dekomposisi Abiotik." },
  { "en": "Berapa Jarak Lari Gawang Putri Dalam Atletik?", "id": "100 Meter." },
  { "en": "Apa Nama Alat Untuk Mengukur Tekanan Atmosfer?", "id": "Barometer." },
  { "en": "Siapakah Firaun Wanita Pertama Di Mesir Kuno?", "id": "Sobekneferu." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Orang Kaya?", "id": "Plutokrasi." },
  { "en": "Apa Akronim Untuk 'Just Kidding'?", "id": "JK." },
  { "en": "Pada Tahun Berapa Revolusi Iran Terjadi?", "id": "Tahun 1979." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Berjalannya?", "id": "Alberto Giacometti." },
  { "en": "Apa Nama Ibukota Negara Islandia?", "id": "Reykjavik." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Irlandia?", "id": "3 Warna." },
  { "en": "Apa Proses Pembentukan Delta Di Muara Sungai?", "id": "Sedimentasi." },
  { "en": "Siapa Penjelajah Yang Mencapai Australia Pertama Kali?", "id": "Willem Janszoon." },
  { "en": "Apa Nama Palung Terdalam Di Samudra Atlantik?", "id": "Palung Puerto Riko." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Pineal?", "id": "Melatonin." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Senjata Cakra?", "id": "Kresna." },
  { "en": "Di Negara Mana Festival Bunga Sakura Dirayakan?", "id": "Jepang." },
  { "en": "Apa Nama Ibukota Negara Senegal?", "id": "Dakar." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya American Gothic?", "id": "Grant Wood." },
  { "en": "Apa Sebutan Untuk Studi Tentang Ikan?", "id": "Ikhtiologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Prisma Segi Lima?", "id": "15 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Porsche?", "id": "Jerman." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Zn?", "id": "Seng (Zinc)." },
  { "en": "Siapakah Yang Menulis Novel The Lord of the Flies?", "id": "William Golding." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Sampanye?", "id": "Buah Anggur." },
  { "en": "Kapan Peringatan Hari Habitat Dunia Diperingati?", "id": "Senin Pertama Oktober." },
  { "en": "Apa Nama Proses Pergerakan Kromosom Saat Pembelahan Sel?", "id": "Mitosis Dan Meiosis." },
  { "en": "Siapakah Ratu Terakhir Yang Memerintah Perancis?", "id": "Marie Antoinette." },
  { "en": "Apa Istilah Untuk Pukulan Melambung Tinggi Dalam Golf?", "id": "Lob." },
  { "en": "Apa Nama Mata Uang Resmi Negara Mesir?", "id": "Pound Mesir." },
  { "en": "Dari Benua Manakah Asal Tanaman Karet?", "id": "Amerika Selatan." },
  { "en": "Proses Pelepasan Gas Dari Gunung Berapi Disebut?", "id": "Degassing." },
  { "en": "Siapa Penulis Novel Petualangan Treasure Island?", "id": "Robert Louis Stevenson." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Testis?", "id": "Testosteron." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Deepavali?", "id": "Hindu, Sikh, Jain." },
  { "en": "Apa Sebutan Untuk Bintang Yang Paling Terang Di Langit?", "id": "Sirius." },
  { "en": "Berapa Jumlah Gigi Geraham Depan Pada Orang Dewasa?", "id": "8 Gigi." },
  { "en": "Siapakah Pendiri Dinasti Mughal Di India?", "id": "Babur." },
  { "en": "Apa Akronim Untuk Organisasi Meteorologi Dunia?", "id": "WMO (World Meteorological Organization)." },
  { "en": "Di Kota Manakah Terdapat Jembatan Rialto Yang Terkenal?", "id": "Venesia, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menyemprotkan Racun?", "id": "Ular Kobra." },
  { "en": "Apa Nama Stadion Utama Klub Barcelona FC?", "id": "Camp Nou." },
  { "en": "Siapa Nama Dewa Kesenian Dan Musik Yunani?", "id": "Apollo." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara Australia?", "id": "6 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Es Permanen Di Kutub?", "id": "Tudung Es." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Kairo?", "id": "Mesir." },
  { "en": "Apa Nama Alat Musik Tiup Dari Suku Aborigin?", "id": "Didgeridoo." },
  { "en": "Siapakah Yang Mengemukakan Hukum Termodinamika Pertama?", "id": "Rudolf Clausius, Lord Kelvin." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Bishop?", "id": "Difraksi Cahaya." },
  { "en": "Berapa Jumlah Baris Dalam Puisi Soneta?", "id": "14 Baris." },
  { "en": "Jaringan Apa Yang Memberi Warna Pada Bunga?", "id": "Kromoplas." },
  { "en": "Negara Apa Yang Dijuluki Negeri Singa?", "id": "Singapura." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Pointilisme?", "id": "Georges Seurat." },
  { "en": "Apa Nama Gas Yang Berbau Seperti Amis?", "id": "Trimetilamina." },
  { "en": "Apa Nama Sungai Terpanjang Di Amerika Utara?", "id": "Sungai Missouri." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Sejarah?", "id": "Herodotus." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Darah?", "id": "Hematologi." },
  { "en": "Berapa Jumlah Babak Dalam Permainan Bola Tangan?", "id": "2 Babak." },
  { "en": "Apa Nama Ibukota Negara Siprus?", "id": "Nikosia." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 1984?", "id": "Winston Smith." },
  { "en": "Kekurangan Magnesium Dapat Menyebabkan Masalah Apa?", "id": "Kram Otot." },
  { "en": "Apa Nama Menara Miring Yang Terkenal Di Italia?", "id": "Menara Pisa." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Pusat Pernapasan?", "id": "Otak Medula Oblongata." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Casa Milà?", "id": "Antoni Gaudi." },
  { "en": "Apa Istilah Untuk Tiga Gol Oleh Satu Pemain?", "id": "Hat-Trick." },
  { "en": "Dimana Lokasi Alhambra, Istana Moor Yang Megah?", "id": "Granada, Spanyol." },
  { "en": "Apa Sebutan Untuk Studi Tentang Primata?", "id": "Primatologi." },
  { "en": "Siapa Pemimpin Kuba Selama Krisis Rudal Kuba?", "id": "Fidel Castro." },
  { "en": "Apa Nama Gurun Terbesar Di Amerika Utara?", "id": "Gurun Great Basin." },
  { "en": "Berapa Jumlah Titik Sudut Pada Limas Segi Empat?", "id": "5 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Penyakit?", "id": "Patologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Catur?", "id": "India." },
  { "en": "Siapakah Ratu Mesir Yang Merupakan Istri Akhenaten?", "id": "Nefertiti." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Pendinginan Magma?", "id": "Batuan Beku Intrusif." },
  { "en": "Apa Nama Titik Tertinggi Di Amerika Utara?", "id": "Denali." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Pascal?", "id": "Niklaus Wirth." },
  { "en": "Apa Nama Tari Api Khas Dari Bali?", "id": "Tari Kecak." },
  { "en": "Huruf Apa Yang Melambangkan Angka 500 Dalam Romawi?", "id": "D." },
  { "en": "Apa Satuan Standar Internasional Untuk Induktansi Listrik?", "id": "Henry." },
  { "en": "Siapakah Kaisar Romawi Yang Membagi Kekaisaran?", "id": "Diocletian." },
  { "en": "Apa Nama Pasta Berbentuk Pita Dari Italia?", "id": "Fettuccine." },
  { "en": "Apa Nama Hewan Tercepat Di Udara?", "id": "Elang Alap-Alap Peregrine." },
  { "en": "Apa Akronim Untuk World Intellectual Property Organization?", "id": "WIPO." },
  { "en": "Berapa Jumlah Diagonal Bidang Pada Sebuah Kubus?", "id": "12 Diagonal Bidang." },
  { "en": "Apa Nama Ibukota Negara Luksemburg?", "id": "Luksemburg." },
  { "en": "Siapa Penulis Drama Yunani Kuno Oedipus Rex?", "id": "Sophocles." },
  { "en": "Apa Istilah Untuk Studi Tentang Enzim?", "id": "Enzimologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Rusia?", "id": "Beruang." },
  { "en": "Apa Ibukota Provinsi Kalimantan Selatan?", "id": "Banjarbaru." },
  { "en": "Siapa Nama Dewa Angin Dalam Mitologi Yunani?", "id": "Aeolus." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Cangkang Telur?", "id": "Kalsium Karbonat." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Gunners'?", "id": "Arsenal FC." },
  { "en": "Apa Sebutan Untuk Bayangan Bumi Atau Bulan?", "id": "Umbra Dan Penumbra." },
  { "en": "Siapa Raja Terakhir Dari Kerajaan Majapahit?", "id": "Girindrawardhana." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Aksara Kuno?", "id": "Paleografer." },
  { "en": "Dimana Lokasi Gunung Vesuvius Yang Bersejarah?", "id": "Napoli, Italia." },
  { "en": "Berapa Jumlah Tulang Tarsal Pada Kaki Manusia?", "id": "7 Tulang." },
  { "en": "Apa Istilah Untuk Pergerakan Lempeng Saling Mendekat?", "id": "Konvergen." },
  { "en": "Siapakah Bapak Fisiologi Modern Yang Terkenal?", "id": "Claude Bernard." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Ion?", "id": "Ionisasi." },
  { "en": "Berapa Jarak Lari Steeplechase Dalam Atletik?", "id": "3000 Meter." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Aliran Fluida?", "id": "Venturimeter." },
  { "en": "Siapakah Kaisar Romawi Yang Dikenal Sebagai Filsuf?", "id": "Marcus Aurelius." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Diktator?", "id": "Diktator." },
  { "en": "Apa Akronim Untuk 'In My Humble Opinion'?", "id": "IMHO." },
  { "en": "Pada Tahun Berapa Revolusi Industri Kedua Dimulai?", "id": "Sekitar Tahun 1870." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Lukisan Abstraknya?", "id": "Piet Mondrian." },
  { "en": "Apa Nama Ibukota Negara Malta?", "id": "Valletta." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Spanyol?", "id": "2 Warna." },
  { "en": "Apa Proses Penguapan Air Dari Tanaman?", "id": "Transpirasi." },
  { "en": "Siapa Penjelajah Yang Mencapai India Melalui Laut?", "id": "Vasco da Gama." },
  { "en": "Apa Nama Palung Terdalam Di Samudra Hindia?", "id": "Palung Sunda." },
  { "en": "Apa Hormon Yang Mengatur Keseimbangan Kalsium?", "id": "Paratiroid." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Senjata Konta?", "id": "Karna." },
  { "en": "Di Negara Mana Festival Lentera Yi Peng Dirayakan?", "id": "Thailand." },
  { "en": "Apa Nama Ibukota Negara Slovenia?", "id": "Ljubljana." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya The Garden of Earthly Delights?", "id": "Hieronymus Bosch." },
  { "en": "Apa Sebutan Untuk Studi Tentang Jamur?", "id": "Mikologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Tabung?", "id": "2 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Volvo?", "id": "Swedia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Ni?", "id": "Nikel." },
  { "en": "Siapakah Yang Menulis Novel The Grapes of Wrath?", "id": "John Steinbeck." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Gin?", "id": "Buah Juniper." },
  { "en": "Kapan Peringatan Hari Standar Dunia Diperingati?", "id": "14 Oktober." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Menuju Cahaya?", "id": "Fototropisme." },
  { "en": "Siapakah Firaun Wanita Terkenal Dari Mesir?", "id": "Hatshepsut." },
  { "en": "Apa Istilah Untuk Pukulan Keras Dalam Bulu Tangkis?", "id": "Smash." },
  { "en": "Apa Nama Mata Uang Resmi Negara Hungaria?", "id": "Forint Hungaria." },
  { "en": "Dari Benua Manakah Asal Tanaman Gandum?", "id": "Asia Barat." },
  { "en": "Proses Pembentukan Pegunungan Lipatan Disebut Apa?", "id": "Orogenesis." },
  { "en": "Siapa Penulis Novel Petualangan Gulliver's Travels?", "id": "Jonathan Swift." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Tiroid?", "id": "Tiroksin Dan Kalsitonin." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Zend Avesta?", "id": "Zoroastrianisme." },
  { "en": "Apa Sebutan Untuk Bintang Yang Meledak Sangat Terang?", "id": "Supernova." },
  { "en": "Berapa Jumlah Gigi Geraham Belakang Pada Orang Dewasa?", "id": "12 Gigi." },
  { "en": "Siapakah Pendiri Kekaisaran Ottoman Yang Agung?", "id": "Osman I." },
  { "en": "Apa Akronim Untuk International Maritime Organization?", "id": "IMO." },
  { "en": "Di Kota Manakah Terdapat Istana Buckingham Yang Terkenal?", "id": "London, Inggris." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menghasilkan Listrik?", "id": "Belut Listrik." },
  { "en": "Apa Nama Arena Utama Turnamen Tenis French Open?", "id": "Stade Roland Garros." },
  { "en": "Siapa Nama Dewi Bulan Dan Perburuan Yunani?", "id": "Artemis." },
  { "en": "Berapa Jumlah Provinsi Yang Ada Di Negara Kanada?", "id": "10 Provinsi." },
  { "en": "Apa Sebutan Untuk Lapisan Udara Yang Menyelimuti Bumi?", "id": "Atmosfer." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Wina?", "id": "Austria." },
  { "en": "Apa Nama Alat Musik Petik Khas Rusia?", "id": "Balalaika." },
  { "en": "Siapakah Yang Mengemukakan Hukum Gravitasi Universal?", "id": "Isaac Newton." },
  { "en": "Apa Nama Fenomena Optik Yang Membuat Benda Terlihat Patah?", "id": "Pembiasan." },
  { "en": "Berapa Jumlah Suku Kata Dalam Puisi Haiku?", "id": "17 Suku Kata." },
  { "en": "Jaringan Apa Yang Mengangkut Oksigen Dalam Darah?", "id": "Sel Darah Merah." },
  { "en": "Negara Apa Yang Dijuluki Negeri Bunga Sakura?", "id": "Jepang." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Fauvisme?", "id": "Henri Matisse." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Proses Pengelasan?", "id": "Asetilena." },
  { "en": "Apa Nama Sungai Terpanjang Di Benua Amerika Selatan?", "id": "Sungai Amazon." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Filsafat Modern?", "id": "René Descartes." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Pencernaan?", "id": "Gastroenterologi." },
  { "en": "Berapa Jumlah Ronde Dalam Pertandingan Tinju Olimpiade?", "id": "3 Ronde." },
  { "en": "Apa Nama Ibukota Negara Latvia?", "id": "Riga." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel Moby-Dick?", "id": "Ishmael Dan Kapten Ahab." },
  { "en": "Kekurangan Kalium Dapat Menyebabkan Masalah Apa?", "id": "Kelemahan Otot." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Granada?", "id": "Alhambra." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Keringat Terbesar?", "id": "Kulit." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Museum Guggenheim Bilbao?", "id": "Frank Gehry." },
  { "en": "Apa Istilah Untuk Serangan Cepat Dalam Sepak Bola?", "id": "Serangan Balik (Counter-Attack)." },
  { "en": "Dimana Lokasi Colosseum, Amfiteater Romawi Kuno?", "id": "Roma, Italia." },
  { "en": "Apa Sebutan Untuk Studi Tentang Jamur Beracun?", "id": "Mikologi Toksikologi." },
  { "en": "Siapa Presiden Afrika Selatan Yang Mengakhiri Apartheid?", "id": "F. W. de Klerk." },
  { "en": "Apa Nama Gurun Pasir Terbesar Di Asia?", "id": "Gurun Gobi." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Prisma Segienam?", "id": "12 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Gunung?", "id": "Orologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kopi Espresso?", "id": "Italia." },
  { "en": "Siapakah Ratu Terakhir Yang Memerintah Kerajaan Inggris?", "id": "Ratu Elizabeth II." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Pendinginan Cepat Lava?", "id": "Batuan Beku Vulkanik." },
  { "en": "Apa Nama Titik Tertinggi Di Selandia Baru?", "id": "Aoraki / Gunung Cook." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman C?", "id": "Dennis Ritchie." },
  { "en": "Apa Nama Gaya Arsitektur Khas Abad Pertengahan Eropa?", "id": "Arsitektur Gotik." },
  { "en": "Huruf Apa Yang Melambangkan Angka 100 Dalam Romawi?", "id": "C." },
  { "en": "Apa Satuan Standar Internasional Untuk Fluks Magnetik?", "id": "Weber." },
  { "en": "Siapakah Kaisar Romawi Yang Memerintah Paling Lama?", "id": "Augustus." },
  { "en": "Apa Nama Hidangan Nasi Khas Dari Jepang?", "id": "Sushi Atau Onigiri." },
  { "en": "Apa Nama Hewan Darat Paling Berbisa Di Australia?", "id": "Ular Taipan Pedalaman." },
  { "en": "Apa Akronim Untuk Universal Declaration of Human Rights?", "id": "UDHR." },
  { "en": "Berapa Jumlah Bidang Diagonal Pada Sebuah Balok?", "id": "6 Bidang Diagonal." },
  { "en": "Apa Nama Ibukota Negara Estonia?", "id": "Tallinn." },
  { "en": "Siapa Penulis Novel The Stranger (L'Étranger)?", "id": "Albert Camus." },
  { "en": "Apa Istilah Untuk Studi Tentang Moluska?", "id": "Malakologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Indonesia?", "id": "Elang Jawa." },
  { "en": "Apa Ibukota Provinsi Jawa Tengah?", "id": "Semarang." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Mesir?", "id": "Sekhmet Dan Montu." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Tulang?", "id": "Kalsium Fosfat." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Blues'?", "id": "Chelsea FC." },
  { "en": "Apa Sebutan Untuk Titik Terdekat Planet Ke Bintangnya?", "id": "Periastron." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Sriwijaya?", "id": "Balaputradewa." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Filologi?", "id": "Filolog." },
  { "en": "Dimana Lokasi Gunung Fuji Yang Simetris?", "id": "Pulau Honshu, Jepang." },
  { "en": "Berapa Jumlah Tulang Metatarsal Pada Kaki Manusia?", "id": "5 Tulang." },
  { "en": "Apa Istilah Untuk Gerakan Udara Dari Tekanan Tinggi?", "id": "Angin." },
  { "en": "Siapakah Bapak Taksonomi Hewan Yang Terkenal?", "id": "Carolus Linnaeus." },
  { "en": "Apa Sebutan Untuk Proses Pembekuan Air?", "id": "Pembekuan." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Rugby League?", "id": "13 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Rotasi?", "id": "Takometer." },
  { "en": "Siapakah Kaisar Romawi Yang Membangun Tembok Hadrian?", "id": "Hadrian." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Bangsawan Gereja?", "id": "Episkopal." },
  { "en": "Apa Akronim Untuk 'As Soon As Possible'?", "id": "ASAP." },
  { "en": "Pada Tahun Berapa Perang Dingin Secara Resmi Berakhir?", "id": "Tahun 1991." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Kanada?", "id": "2 Warna." },
  { "en": "Apa Proses Pelepasan Uap Air Dari Daun?", "id": "Transpirasi." },
  { "en": "Siapa Penjelajah Yang Mencapai Puncak Gunung Everest Pertama?", "id": "Sir Edmund Hillary." },
  { "en": "Apa Nama Palung Terdalam Di Samudra Pasifik?", "id": "Palung Mariana." },
  { "en": "Apa Hormon Yang Mengatur Tingkat Gula Darah?", "id": "Insulin Dan Glukagon." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Kesatria Pringgandani'?", "id": "Gatotkaca." },
  { "en": "Di Negara Mana Festival Lentera Musim Dingin Dirayakan?", "id": "Tiongkok." },
  { "en": "Apa Nama Ibukota Negara Kamerun?", "id": "Yaoundé." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya Las Meninas?", "id": "Diego Velázquez." },
  { "en": "Apa Sebutan Untuk Studi Tentang Gua?", "id": "Speleologi." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Limas Segitiga?", "id": "4 Titik Sudut." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Mazda?", "id": "Jepang." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Co?", "id": "Kobalt." },
  { "en": "Siapakah Yang Menulis Novel One Hundred Years of Solitude?", "id": "Gabriel García Márquez." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Soju?", "id": "Beras, Gandum, Ubi." },
  { "en": "Kapan Peringatan Hari Televisi Dunia Diperingati?", "id": "21 November." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Karena Sentuhan?", "id": "Tigmotropisme." },
  { "en": "Siapakah Yang Dianggap Kaisar Terakhir Kekaisaran Romawi?", "id": "Romulus Augustulus." },
  { "en": "Apa Istilah Untuk Pukulan Servis Yang Tak Tersentuh?", "id": "Ace." },
  { "en": "Apa Nama Mata Uang Resmi Negara Polandia?", "id": "Złoty Polandia." },
  { "en": "Dari Benua Manakah Asal Tanaman Lada?", "id": "Asia Selatan." },
  { "en": "Proses Pendinginan Magma Di Bawah Permukaan Disebut?", "id": "Intrusi Magma." },
  { "en": "Siapa Penulis Novel Fiksi Ilmiah Dune?", "id": "Frank Herbert." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Pineal?", "id": "Melatonin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Vesak?", "id": "Agama Buddha." },
  { "en": "Apa Sebutan Untuk Awan Tipis Dan Berbentuk Serat?", "id": "Awan Sirus." },
  { "en": "Berapa Jumlah Tulang Metakarpal Pada Tangan Manusia?", "id": "5 Tulang." },
  { "en": "Siapakah Pendiri Dinasti Han Di Tiongkok?", "id": "Liu Bang (Kaisar Gaozu)." },
  { "en": "Apa Akronim Untuk United Nations High Commissioner for Refugees?", "id": "UNHCR." },
  { "en": "Di Kota Manakah Terdapat Menara CN (CN Tower)?", "id": "Toronto, Kanada." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berubah Jenis Kelamin?", "id": "Ikan Badut." },
  { "en": "Apa Nama Arena Balap Mobil Terkenal Di Monako?", "id": "Sirkuit de Monaco." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Hindu?", "id": "Kartikeya." },
  { "en": "Berapa Jumlah Negara Yang Memiliki Hak Veto PBB?", "id": "5 Negara." },
  { "en": "Apa Sebutan Untuk Lapisan Batuan Padat Di Bumi?", "id": "Litosfer." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Dublin?", "id": "Irlandia." },
  { "en": "Apa Nama Alat Musik Tiup Logam Paling Besar?", "id": "Tuba." },
  { "en": "Siapakah Yang Merumuskan Prinsip Ketidakpastian?", "id": "Werner Heisenberg." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Pelangi?", "id": "Korona." },
  { "en": "Berapa Jumlah Provinsi Di Negara Afrika Selatan?", "id": "9 Provinsi." },
  { "en": "Jaringan Apa Yang Membentuk Kulit Luar Tumbuhan?", "id": "Jaringan Dermal." },
  { "en": "Negara Apa Yang Dijuluki Tanah Ribuan Bukit?", "id": "Rwanda." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Art Nouveau?", "id": "Alphonse Mucha." },
  { "en": "Apa Nama Gas Yang Paling Melimpah Ketiga Di Atmosfer?", "id": "Argon." },
  { "en": "Apa Nama Samudra Yang Mengelilingi Kutub Utara?", "id": "Samudra Arktik." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Anatomi?", "id": "Herophilos." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Penyakit Dalam?", "id": "Ilmu Penyakit Dalam." },
  { "en": "Berapa Jumlah Poin Untuk Gol Dalam Rugby Union?", "id": "5 Poin (Try)." },
  { "en": "Apa Nama Ibukota Negara Monako?", "id": "Monako." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel The Great Gatsby?", "id": "Jay Gatsby." },
  { "en": "Kekurangan Natrium Dapat Menyebabkan Masalah Apa?", "id": "Hiponatremia." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Wina?", "id": "Istana Schönbrunn." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Pencernaan Terbesar?", "id": "Hati." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang The Shard?", "id": "Renzo Piano." },
  { "en": "Apa Istilah Untuk Mengoper Bola Dengan Kaki Ke Teman?", "id": "Passing." },
  { "en": "Dimana Lokasi Tembok Ratapan Yang Suci Berada?", "id": "Yerusalem." },
  { "en": "Apa Sebutan Untuk Studi Tentang Amfibi?", "id": "Batrakologi." },
  { "en": "Siapa Pemimpin Terakhir Kekaisaran Rusia?", "id": "Tsar Nicholas II." },
  { "en": "Apa Nama Gurun Terdingin Di Dunia?", "id": "Gurun Antartika." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Prisma Segi Sepuluh?", "id": "30 Rusuk." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Laut?", "id": "Oseanografi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kincir Air?", "id": "Yunani Kuno." },
  { "en": "Siapakah Ratu Yang Memerintah Inggris Selama Revolusi Industri?", "id": "Ratu Victoria." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Cangkang Kerang?", "id": "Batu Gamping." },
  { "en": "Apa Nama Titik Tertinggi Di Jepang?", "id": "Gunung Fuji." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Fortran?", "id": "John Backus." },
  { "en": "Apa Nama Lapisan Terluar Dari Matahari?", "id": "Korona." },
  { "en": "Apa Satuan Standar Internasional Untuk Medan Magnet?", "id": "Tesla." },
  { "en": "Siapakah Jenderal Athena Yang Terkenal Dalam Perang Peloponnesia?", "id": "Pericles." },
  { "en": "Apa Nama Hidangan Kentang Goreng Khas Belgia?", "id": "Frites." },
  { "en": "Apa Nama Hewan Darat Paling Cerdas Setelah Manusia?", "id": "Simpanse." },
  { "en": "Apa Akronim Untuk Zone Improvement Plan?", "id": "ZIP Code." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Kerucut?", "id": "1 Titik Sudut." },
  { "en": "Apa Nama Ibukota Negara Lithuania?", "id": "Vilnius." },
  { "en": "Siapa Penulis Novel The Call of the Wild?", "id": "Jack London." },
  { "en": "Apa Istilah Untuk Studi Tentang Lumut?", "id": "Briologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Tiongkok?", "id": "Panda." },
  { "en": "Apa Ibukota Provinsi Daerah Istimewa Yogyakarta?", "id": "Yogyakarta." },
  { "en": "Siapa Nama Dewa Pencipta Dalam Mitologi Hindu?", "id": "Brahma." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama DNA?", "id": "Asam Deoksiribonukleat." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Citizens'?", "id": "Manchester City." },
  { "en": "Apa Sebutan Untuk Titik Terjauh Planet Dari Bintangnya?", "id": "Apoastron." },
  { "en": "Siapa Raja Pertama Kerajaan Pajajaran?", "id": "Sri Jayabhupati." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Peta?", "id": "Kartografer." },
  { "en": "Dimana Lokasi Candi Prambanan Yang Megah?", "id": "Jawa Tengah, Indonesia." },
  { "en": "Berapa Jumlah Tulang Karpal Pada Pergelangan Tangan?", "id": "8 Tulang." },
  { "en": "Apa Istilah Untuk Udara Yang Mengandung Banyak Uap Air?", "id": "Lembab." },
  { "en": "Siapakah Bapak Ilmu Politik Modern?", "id": "Niccolò Machiavelli." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Darah?", "id": "Hematopoiesis." },
  { "en": "Berapa Jumlah Putaran Dalam Balapan Formula 1?", "id": "Bervariasi Tergantung Sirkuit." },
  { "en": "Apa Nama Alat Untuk Mengukur Kelembaban?", "id": "Higrometer." },
  { "en": "Siapakah Jenderal Romawi Yang Mengalahkan Hannibal?", "id": "Scipio Africanus." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Para Ahli?", "id": "Teknokrasi." },
  { "en": "Apa Akronim Untuk 'Do It Yourself'?", "id": "DIY." },
  { "en": "Pada Tahun Berapa Revolusi Tiongkok Terjadi?", "id": "Tahun 1911." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Pikirannya?", "id": "Auguste Rodin." },
  { "en": "Apa Nama Ibukota Negara Maladewa?", "id": "Malé." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Jerman?", "id": "3 Warna." },
  { "en": "Apa Proses Pembentukan Embun Di Pagi Hari?", "id": "Kondensasi." },
  { "en": "Siapa Penjelajah Yang Menemukan Selat Magellan?", "id": "Ferdinand Magellan." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Indonesia?", "id": "Gunung Merapi." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Adrenalin Dan Kortisol." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Bapak Para Pandawa'?", "id": "Pandu." },
  { "en": "Di Negara Mana Festival Tango Internasional Diadakan?", "id": "Argentina." },
  { "en": "Apa Nama Ibukota Negara Kosta Rika?", "id": "San José." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya The Night Watch?", "id": "Rembrandt." },
  { "en": "Apa Sebutan Untuk Studi Tentang Serangga?", "id": "Entomologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Limas Segi Enam?", "id": "12 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Subaru?", "id": "Jepang." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Mn?", "id": "Mangan." },
  { "en": "Siapakah Yang Menulis Drama 'Waiting for Godot'?", "id": "Samuel Beckett." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Bourbon?", "id": "Jagung." },
  { "en": "Kapan Peringatan Hari Filsafat Dunia Diperingati?", "id": "Kamis Ketiga November." },
  { "en": "Apa Nama Proses Pergerakan Air Dalam Tanah?", "id": "Perkolasi." },
  { "en": "Siapakah Yang Dianggap Kaisar Pertama Kekaisaran Persia?", "id": "Koresh Agung." },
  { "en": "Apa Istilah Untuk Pukulan Melambung Dalam Tenis?", "id": "Lob." },
  { "en": "Apa Nama Mata Uang Resmi Negara Republik Ceko?", "id": "Koruna Ceko." },
  { "en": "Dari Benua Manakah Asal Tanaman Kakao?", "id": "Amerika." },
  { "en": "Proses Pengendapan Mineral Dari Larutan Disebut Apa?", "id": "Presipitasi Kimia." },
  { "en": "Siapa Penulis Novel The Color Purple?", "id": "Alice Walker." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Pankreas?", "id": "Insulin Dan Glukagon." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Adi Granth?", "id": "Sikhisme." },
  { "en": "Apa Sebutan Untuk Awan Yang Berbentuk Lensa?", "id": "Awan Lentikularis." },
  { "en": "Berapa Jumlah Tulang Jari Tangan Pada Manusia?", "id": "14 Tulang (Falang)." },
  { "en": "Siapakah Pendiri Dinasti Qin Di Tiongkok?", "id": "Qin Shi Huang." },
  { "en": "Apa Akronim Untuk Organisasi Pariwisata Dunia?", "id": "UNWTO." },
  { "en": "Di Kota Manakah Terdapat Jembatan Kapel (Chapel Bridge)?", "id": "Lucerne, Swiss." },
  { "en": "Hewan Apa Yang Dikenal Memiliki Kulit Berduri?", "id": "Landak." },
  { "en": "Apa Nama Trofi Untuk Juara Hoki Es NHL?", "id": "Stanley Cup." },
  { "en": "Siapa Nama Dewa Pengrajin Dalam Mitologi Yunani?", "id": "Hephaestus." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara Brasil?", "id": "26 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Mantel Cair Di Bumi?", "id": "Astenosfer." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Budapest?", "id": "Hungaria." },
  { "en": "Apa Nama Alat Musik Tiup Dari Kayu Khas Irlandia?", "id": "Tin Whistle." },
  { "en": "Siapakah Yang Mengemukakan Hukum Gerak Planet?", "id": "Johannes Kepler." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Es?", "id": "Halo." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Sirilik?", "id": "33 Huruf." },
  { "en": "Jaringan Apa Yang Memberi Kekuatan Pada Tumbuhan?", "id": "Jaringan Penyokong." },
  { "en": "Negara Apa Yang Dijuluki Negeri Anggur?", "id": "Perancis." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Konstruktivisme?", "id": "Vladimir Tatlin." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Proses Pendinginan?", "id": "Freon." },
  { "en": "Apa Nama Samudra Terbesar Kedua Di Dunia?", "id": "Samudra Atlantik." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Virologi?", "id": "Martinus Beijerinck." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Jaringan Ikat?", "id": "Reumatologi." },
  { "en": "Berapa Jumlah Poin Untuk Touchdown Dalam American Football?", "id": "6 Poin." },
  { "en": "Apa Nama Ibukota Negara Jamaika?", "id": "Kingston." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Crime and Punishment'?", "id": "Rodion Raskolnikov." },
  { "en": "Kekurangan Seng Dapat Menyebabkan Masalah Apa?", "id": "Gangguan Pertumbuhan." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Madrid?", "id": "Istana Kerajaan Madrid." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Adrenal?", "id": "Kelenjar Adrenal." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Centre Pompidou?", "id": "Renzo Piano, Richard Rogers." },
  { "en": "Apa Istilah Untuk Mengoper Bola Dengan Kepala?", "id": "Menyundul (Heading)." },
  { "en": "Dimana Lokasi Angkor Wat, Kompleks Candi Megah?", "id": "Siem Reap, Kamboja." },
  { "en": "Apa Sebutan Untuk Studi Tentang Ikan Dan Habitatnya?", "id": "Ikhtiologi." },
  { "en": "Siapa Pemimpin Gerakan Reformasi Protestan Di Jerman?", "id": "Martin Luther." },
  { "en": "Apa Nama Gurun Terpanas Di Dunia?", "id": "Gurun Lut, Iran." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Tulang?", "id": "Osteologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kopi Instan?", "id": "Selandia Baru." },
  { "en": "Siapakah Ratu Yang Memerintah Spanyol Saat Era Columbus?", "id": "Ratu Isabella I." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Pendinginan Lambat Magma?", "id": "Batuan Beku Plutonik." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Alpen?", "id": "Mont Blanc." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman BASIC?", "id": "John G. Kemeny, Thomas E. Kurtz." },
  { "en": "Apa Nama Kain Tenun Khas Dari Suku Batak?", "id": "Ulos." },
  { "en": "Huruf Apa Yang Melambangkan Angka 1000 Dalam Romawi?", "id": "M." },
  { "en": "Apa Satuan Standar Internasional Untuk Konduktansi Listrik?", "id": "Siemens." },
  { "en": "Siapakah Kaisar Romawi Yang Terkenal Dengan Kebijaksanaannya?", "id": "Marcus Aurelius." },
  { "en": "Apa Nama Hidangan Daging Cincang Khas Turki?", "id": "Kebab." },
  { "en": "Apa Nama Hewan Darat Paling Tinggi Di Dunia?", "id": "Jerapah." },
  { "en": "Apa Akronim Untuk International Telecommunication Union?", "id": "ITU." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Prisma Segilima?", "id": "10 Titik Sudut." },
  { "en": "Apa Nama Ibukota Negara Guatemala?", "id": "Kota Guatemala." },
  { "en": "Siapa Penulis Novel 'The Catcher in the Rye'?", "id": "J.D. Salinger." },
  { "en": "Apa Istilah Untuk Studi Tentang Serbuk Sari?", "id": "Palinologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Australia?", "id": "Kanguru." },
  { "en": "Apa Ibukota Provinsi Kalimantan Tengah?", "id": "Palangka Raya." },
  { "en": "Siapa Nama Dewa Penjaga Dunia Bawah Hindu?", "id": "Yama." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Gas Alam?", "id": "Metana." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'La Vecchia Signora'?", "id": "Juventus." },
  { "en": "Apa Sebutan Untuk Benda Langit Yang Gagal Menjadi Bintang?", "id": "Kataí Coklat." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Mataram Kuno?", "id": "Rakai Pikatan." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Filateli?", "id": "Filatelis." },
  { "en": "Dimana Lokasi Tembok Besar Tiongkok Berakhir?", "id": "Jiayuguan." },
  { "en": "Apa Istilah Untuk Udara Kering Yang Turun Gunung?", "id": "Angin Fohn." },
  { "en": "Siapakah Bapak Fisiologi Tumbuhan Modern?", "id": "Stephen Hales." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Tanah?", "id": "Pedogenesis." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Ultimate Frisbee?", "id": "7 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Tekanan Cairan?", "id": "Manometer." },
  { "en": "Siapakah Jenderal Sparta Yang Memenangkan Perang Peloponnesia?", "id": "Lysander." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Satu Keluarga?", "id": "Dinasti." },
  { "en": "Apa Akronim Untuk 'Thank God It's Friday'?", "id": "TGIF." },
  { "en": "Pada Tahun Berapa Revolusi Kebudayaan Tiongkok Dimulai?", "id": "Tahun 1966." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Balon Anjingnya?", "id": "Jeff Koons." },
  { "en": "Apa Nama Ibukota Negara Honduras?", "id": "Tegucigalpa." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Belgia?", "id": "3 Warna." },
  { "en": "Apa Proses Pembentukan Karang Oleh Organisme Laut?", "id": "Biogenik." },
  { "en": "Siapa Penjelajah Yang Menemukan Tanjung Harapan?", "id":- "Bartolomeu Dias." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Italia?", "id": "Gunung Etna." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Paratiroid?", "id": "Hormon Paratiroid (PTH)." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Ksatria Madukara'?", "id": "Arjuna." },
  { "en": "Di Negara Mana Festival Salju Sapporo Diadakan?", "id": "Jepang." },
  { "en": "Apa Nama Ibukota Negara Angola?", "id": "Luanda." },
  { "en": "Siapa Komposer Terkenal Dari Era Romantik Polandia?", "id": "Frédéric Chopin." },
  { "en": "Apa Sebutan Untuk Studi Tentang Reptil?", "id": "Herpetologi." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Bola?", "id": "Tidak Ada." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Mitsubishi?", "id": "Jepang." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Cr?", "id": "Kromium." },
  { "en": "Siapakah Yang Menulis Buku 'A Brief History of Time'?", "id": "Stephen Hawking." },
  { "en": "Apa Bahan Baku Utama Pembuatan Kertas Papirus Kuno?", "id": "Tanaman Papirus." },
  { "en": "Kapan Peringatan Hari Bahasa Isyarat Internasional Diperingati?", "id": "23 September." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Menjauhi Cahaya?", "id": "Fototropisme Negatif." },
  { "en": "Siapakah Kaisar Pertama Yang Menyatukan Tiongkok?", "id": "Qin Shi Huang." },
  { "en": "Apa Istilah Untuk Pertandingan Ulang Dalam Olahraga?", "id": "Rematch." },
  { "en": "Apa Nama Mata Uang Resmi Negara Vietnam?", "id": "Đồng Vietnam." },
  { "en": "Dari Benua Manakah Asal Tanaman Cengkeh?", "id": "Asia (Indonesia)." },
  { "en": "Proses Perubahan Struktur Mineral Batuan Disebut Apa?", "id": "Rekristalisasi." },
  { "en": "Siapa Penulis Novel 'The Tale of Peter Rabbit'?", "id": "Beatrix Potter." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Adrenalin Dan Kortisol." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Imlek?", "id": "Konfusianisme, Taoisme." },
  { "en": "Apa Sebutan Untuk Awan Yang Berbentuk Gumpalan?", "id": "Awan Kumulus." },
  { "en": "Berapa Jumlah Tulang Jari Kaki Pada Manusia?", "id": "14 Tulang (Falang)." },
  { "en": "Siapakah Pemimpin Soviet Selama Krisis Rudal Kuba?", "id": "Nikita Khrushchev." },
  { "en": "Apa Akronim Untuk Program Pangan Dunia PBB?", "id": "WFP (World Food Programme)." },
  { "en": "Di Kota Manakah Terdapat Gedung Opera Sydney?", "id": "Sydney, Australia." },
  { "en": "Hewan Apa Yang Dikenal Sebagai 'Pembawa Wabah'?", "id": "Tikus." },
  { "en": "Apa Nama Perlombaan Balap Sepeda Paling Bergengsi?", "id": "Tour de France." },
  { "en": "Siapa Nama Dewa Laut Dan Air Dalam Mitologi Nordik?", "id": "Njörðr." },
  { "en": "Berapa Jumlah Kanton Yang Ada Di Negara Swiss?", "id": "26 Kanton." },
  { "en": "Apa Sebutan Untuk Lapisan Inti Terluar Bumi?", "id": "Inti Luar." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Warsawa?", "id": "Polandia." },
  { "en": "Apa Nama Pakaian Adat Pria Dari Jawa?", "id": "Beskap." },
  { "en": "Siapakah Yang Mengemukakan Hukum Optik Geometris?", "id": "Ibnu al-Haytham." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Cahaya?", "id": "Difraksi." },
  { "en": "Berapa Jumlah Pulau Berpenghuni Di Negara Indonesia?", "id": "Sekitar 17.000 Pulau." },
  { "en": "Jaringan Apa Yang Mengedarkan Nutrisi Ke Seluruh Tubuh?", "id": "Sistem Peredaran Darah." },
  { "en": "Negara Apa Yang Dijuluki Negeri Sepatu Bot?", "id": "Italia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Kubisme Analitis?", "id": "Picasso Dan Braque." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Balon Udara Panas?", "id": "Udara Panas." },
  { "en": "Apa Nama Semenanjung Yang Memisahkan Laut Merah?", "id": "Semenanjung Sinai." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Bakteriologi?", "id": "Louis Pasteur." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Kemih?", "id": "Urologi." },
  { "en": "Berapa Jarak Lari Maraton Penuh?", "id": "42.195 Meter." },
  { "en": "Apa Nama Ibukota Negara El Salvador?", "id": "San Salvador." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Don Quixote'?", "id": "Alonso Quijano." },
  { "en": "Kekurangan Tembaga Dapat Menyebabkan Masalah Apa?", "id": "Anemia." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di St. Petersburg?", "id": "Istana Peterhof." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Timus?", "id": "Kelenjar Timus." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Fallingwater House?", "id": "Frank Lloyd Wright." },
  { "en": "Apa Istilah Untuk Mengoper Bola Dengan Tangan?", "id": "Melempar (Throwing)." },
  { "en": "Dimana Lokasi Stonehenge, Lingkaran Batu Misterius?", "id": "Wiltshire, Inggris." },
  { "en": "Apa Sebutan Untuk Studi Tentang Batuan Dan Mineral?", "id": "Petrologi." },
  { "en": "Siapa Pemimpin Revolusi Industri Di Inggris?", "id": "Tidak Ada Pemimpin Tunggal." },
  { "en": "Apa Nama Gurun Terbesar Di Amerika Selatan?", "id": "Gurun Patagonia." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Limas Segienam?", "id": "7 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Sendi?", "id": "Artrologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kertas Uang?", "id": "Tiongkok." },
  { "en": "Siapakah Ratu Yang Memerintah Inggris Paling Lama?", "id": "Ratu Elizabeth II." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Pendinginan Cepat Lava?", "id": "Batuan Beku Ekstrusif." },
  { "en": "Apa Nama Unit Pewarisan Sifat Pada Organisme?", "id": "Gen." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Java?", "id": "James Gosling." },
  { "en": "Apa Nama Tarian Tradisional Dari Suku Maori?", "id": "Haka." },
  { "en": "Apa Satuan Standar Internasional Untuk Iluminansi?", "id": "Lux." },
  { "en": "Siapakah Kaisar Romawi Yang Terkenal Dengan Temboknya?", "id": "Hadrian." },
  { "en": "Apa Nama Hidangan Nasi Goreng Khas Indonesia?", "id": "Nasi Goreng." },
  { "en": "Apa Nama Hewan Darat Paling Cerdas Di Dunia?", "id": "Simpanse." },
  { "en": "Apa Akronim Untuk File Transfer Protocol?", "id": "FTP." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segitiga?", "id": "5 Bidang." },
  { "en": "Apa Nama Ibukota Negara Nikaragua?", "id": "Managua." },
  { "en": "Siapa Penulis Novel 'To Kill a Mockingbird'?", "id": "Harper Lee." },
  { "en": "Apa Istilah Untuk Studi Tentang Fosil Jejak?", "id": "Ichnology." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Selandia Baru?", "id": "Kiwi." },
  { "en": "Apa Ibukota Provinsi Gorontalo?", "id": "Gorontalo." },
  { "en": "Siapa Nama Dewa Matahari Dalam Mitologi Mesir?", "id": "Ra." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Ozon?", "id": "O3." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'Die Roten'?", "id": "Bayern Munich." },
  { "en": "Apa Sebutan Untuk Bintang Yang Baru Lahir?", "id": "Protobintang." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Tarumanegara?", "id": "Purnawarman." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Bahasa?", "id": "Linguis." },
  { "en": "Dimana Lokasi Tembok Berlin Dibangun?", "id": "Berlin, Jerman." },
  { "en": "Berapa Jumlah Tulang Rusuk Melayang Pada Manusia?", "id": "4 Tulang (2 Pasang)." },
  { "en": "Apa Istilah Untuk Udara Yang Bergerak Ke Atas?", "id": "Arus Udara Naik." },
  { "en": "Siapakah Bapak Botani Modern?", "id": "Carolus Linnaeus." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Embrio?", "id": "Embriogenesis." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Hoki Es?", "id": "6 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Keasaman Tanah?", "id": "Soil pH Meter." },
  { "en": "Siapakah Jenderal Kartago Yang Melintasi Alpen?", "id": "Hannibal." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Rakyat?", "id": "Demokrasi." },
  { "en": "Apa Akronim Untuk 'Got To Go'?", "id": "GTG." },
  { "en": "Pada Tahun Berapa Revolusi EDSA Terjadi Di Filipina?", "id": "Tahun 1986." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Berwarnanya?", "id": "Takashi Murakami." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Italia?", "id": "3 Warna." },
  { "en": "Apa Proses Pembentukan Gua Oleh Pelarutan Batuan?", "id": "Karstifikasi." },
  { "en": "Siapa Penjelajah Yang Menemukan Air Terjun Victoria?", "id": "David Livingstone." },
  { "en": "Apa Nama Gunung Berapi Tertinggi Di Dunia?", "id": "Ojos del Salado." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Timus?", "id": "Timopoietin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Sang Bima Suci'?", "id": "Werkudara." },
  { "en": "Di Negara Mana Festival Musik Glastonbury Diadakan?", "id": "Inggris." },
  { "en": "Apa Nama Ibukota Negara Uruguay?", "id": "Montevideo." },
  { "en": "Siapa Komposer Opera 'The Marriage of Figaro'?", "id": "Wolfgang Amadeus Mozart." },
  { "en": "Apa Sebutan Untuk Studi Tentang Aves Atau Burung?", "id": "Ornitologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Kerucut?", "id": "1 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Nissan?", "id": "Jepang." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Na?", "id": "Natrium (Sodium)." },
  { "en": "Siapakah Yang Menulis Novel 'War of the Worlds'?", "id": "H. G. Wells." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Rum?", "id": "Tebu." },
  { "en": "Kapan Peringatan Hari Ozon Internasional Diperingati?", "id": "16 September." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Karena Gravitasi?", "id": "Gravitropisme." },
  { "en": "Siapakah Jenderal Terkenal Dari Sparta?", "id": "Leonidas I." },
  { "en": "Apa Istilah Untuk Pukulan Voli Dekat Net?", "id": "Dropshot." },
  { "en": "Apa Nama Mata Uang Resmi Negara Thailand?", "id": "Baht Thailand." },
  { "en": "Dari Benua Manakah Asal Tanaman Kina?", "id": "Amerika Selatan." },
  { "en": "Proses Pembentukan Gua Oleh Air Asam Disebut?", "id": "Pelarutan Karst." },
  { "en": "Siapa Penulis Novel 'The Little Prince'?", "id": "Antoine de Saint-Exupéry." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Pituitari?", "id": "Hormon Pertumbuhan (GH)." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Nyepi?", "id": "Agama Hindu Dharma." },
  { "en": "Apa Sebutan Untuk Awan Yang Berbentuk Lapisan?", "id": "Awan Stratus." },
  { "en": "Berapa Jumlah Tulang Telinga Tengah Pada Manusia?", "id": "6 Tulang (3 Pasang)." },
  { "en": "Siapakah Pendiri Dinasti Maurya Di India?", "id": "Chandragupta Maurya." },
  { "en": "Apa Akronim Untuk Organisasi Perjanjian Atlantik Utara?", "id": "NATO." },
  { "en": "Di Kota Manakah Terdapat Jembatan Brooklyn Yang Ikonik?", "id": "New York, Amerika." },
  { "en": "Hewan Apa Yang Dikenal Dapat Tidur Sambil Berdiri?", "id": "Kuda." },
  { "en": "Apa Nama Turnamen Golf Tertua Di Dunia?", "id": "The Open Championship." },
  { "en": "Siapa Nama Dewa Matahari Dalam Mitologi Nordik?", "id": "Sól." },
  { "en": "Berapa Jumlah Wilayah Federal Di Negara Malaysia?", "id": "3 Wilayah Federal." },
  { "en": "Apa Sebutan Untuk Lapisan Inti Terdalam Bumi?", "id": "Inti Dalam." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Stockholm?", "id": "Swedia." },
  { "en": "Apa Nama Makanan Khas Dari Daerah Yogyakarta?", "id": "Gudeg." },
  { "en": "Siapakah Yang Mengemukakan Teori Kuantum Medan?", "id": "Paul Dirac." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Warna?", "id": "Korona." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Ibrani?", "id": "22 Huruf." },
  { "en": "Jaringan Apa Yang Mengangkut Air Dan Mineral?", "id": "Jaringan Xilem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Seribu Kuil?", "id": "Thailand." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Dadaisme Di Zurich?", "id": "Tristan Tzara." },
  { "en": "Apa Nama Gas Yang Dihasilkan Dari Pembusukan Organik?", "id": "Metana." },
  { "en": "Apa Nama Teluk Terbesar Di Dunia?", "id": "Teluk Benggala." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Kedokteran?", "id": "Hippocrates." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Kanker?", "id": "Onkologi." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Bola Voli Pantai?", "id": "2 Pemain." },
  { "en": "Apa Nama Ibukota Negara Paraguay?", "id": "Asunción." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Pride and Prejudice'?", "id": "Elizabeth Bennet." },
  { "en": "Kekurangan Selenium Dapat Menyebabkan Masalah Apa?", "id": "Penyakit Keshan." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Beijing?", "id": "Kota Terlarang." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Pineal?", "id": "Kelenjar Pineal." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Menara Eiffel?", "id": "Gustave Eiffel." },
  { "en": "Apa Istilah Untuk Pukulan Melambung Dalam Bulu Tangkis?", "id": "Lob." },
  { "en": "Dimana Lokasi Gunung Everest, Puncak Tertinggi Dunia?", "id": "Nepal Dan Tiongkok." },
  { "en": "Apa Sebutan Untuk Studi Tentang Cacing?", "id": "Helmintologi." },
  { "en": "Siapa Pemimpin Terakhir Uni Soviet?", "id": "Mikhail Gorbachev." },
  { "en": "Apa Nama Gurun Terbesar Di Australia?", "id": "Gurun Victoria Besar." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Prisma Segidelapan?", "id": "16 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Iklim Kuno?", "id": "Paleoklimatologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kopi Luwak?", "id": "Indonesia." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Majapahit?", "id": "Tribhuwana Wijayatunggadewi." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Pendinginan Lambat Magma?", "id": "Batuan Beku Intrusif." },
  { "en": "Apa Nama Titik Tertinggi Di Benua Antartika?", "id": "Vinson Massif." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Ruby?", "id": "Yukihiro Matsumoto." },
  { "en": "Apa Nama Pakaian Tradisional Wanita Jepang?", "id": "Kimono." },
  { "en": "Apa Satuan Standar Internasional Untuk Aktivitas Radioaktif?", "id": "Becquerel." },
  { "en": "Siapakah Kaisar Romawi Yang Membangun Tembok Antonine?", "id": "Antoninus Pius." },
  { "en": "Apa Nama Hidangan Sup Asam Pedas Khas Thailand?", "id": "Tom Yum." },
  { "en": "Apa Nama Hewan Darat Paling Cepat Di Amerika Utara?", "id": "Pronghorn." },
  { "en": "Apa Akronim Untuk International Bank for Reconstruction and Development?", "id": "IBRD." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Limas Segi Empat?", "id": "5 Bidang." },
  { "en": "Apa Nama Ibukota Negara Bolivia?", "id": "Sucre Dan La Paz." },
  { "en": "Siapa Penulis Novel 'One Flew Over the Cuckoo's Nest'?", "id": "Ken Kesey." },
  { "en": "Apa Istilah Untuk Studi Tentang Spons?", "id": "Poriferologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional India?", "id": "Harimau." },
  { "en": "Apa Ibukota Provinsi Maluku Utara?", "id": "Sofifi." },
  { "en": "Siapa Nama Dewa Pelindung Dalam Mitologi Hindu?", "id": "Wisnu." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama LPG?", "id": "Propana Dan Butana." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Red Devils'?", "id": "Manchester United." },
  { "en": "Apa Sebutan Untuk Lubang Hitam Supermasif Di Pusat Galaksi?", "id": "Sagittarius A*." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Kediri?", "id": "Jayabaya." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Epigrafi?", "id": "Epigraf." },
  { "en": "Dimana Lokasi Borobudur, Candi Buddha Terbesar?", "id": "Magelang, Indonesia." },
  { "en": "Berapa Jumlah Tulang Belakang Leher Pada Manusia?", "id": "7 Tulang." },
  { "en": "Apa Istilah Untuk Udara Yang Bergerak Turun?", "id": "Arus Udara Turun." },
  { "en": "Siapakah Bapak Patologi Modern?", "id": "Rudolf Virchow." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Gula?", "id": "Glukoneogenesis." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Lacrosse?", "id": "10 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kelembaban Udara?", "id": "Psikrometer." },
  { "en": "Siapakah Jenderal Romawi Yang Terkenal Dengan Taktik Fabius?", "id": "Fabius Maximus." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Orang Terpilih?", "id": "Republik." },
  { "en": "Apa Akronim Untuk 'In Real Life'?", "id": "IRL." },
  { "en": "Pada Tahun Berapa Revolusi Bunga Di Portugal Terjadi?", "id": "Tahun 1974." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Kuda Berlarinya?", "id": "Frederic Remington." },
  { "en": "Apa Nama Ibukota Negara Ekuador?", "id": "Quito." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Rumania?", "id": "3 Warna." },
  { "en": "Apa Proses Pembentukan Stalaktit Dan Stalagmit?", "id": "Presipitasi Kalsium Karbonat." },
  { "en": "Siapa Penjelajah Yang Menemukan Sungai Amazon?", "id": "Francisco de Orellana." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Jepang?", "id": "Gunung Aso." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Epinefrin Dan Norepinefrin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putri Pancala'?", "id": "Drupadi." },
  { "en": "Di Negara Mana Festival Musik Tomorrowland Diadakan?", "id": "Belgia." },
  { "en": "Apa Nama Ibukota Negara Madagaskar?", "id": "Antananarivo." },
  { "en": "Siapa Komposer Terkenal Dengan Karya 'The Four Seasons'?", "id": "Antonio Vivaldi." },
  { "en": "Apa Sebutan Untuk Studi Tentang Pohon?", "id": "Dendrologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Limas Segi Lima?", "id": "10 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Honda?", "id": "Jepang." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol P?", "id": "Fosfor." },
  { "en": "Siapakah Yang Menulis Novel Fiksi 'Fahrenheit 451'?", "id": "Ray Bradbury." },
  { "en": "Apa Bahan Baku Utama Pembuatan Anggur Putih?", "id": "Anggur Hijau Atau Kuning." },
  { "en": "Kapan Peringatan Hari Perdamaian Internasional Diperingati?", "id": "21 September." },
  { "en": "Apa Nama Proses Pergerakan Organisme Karena Rangsang Kimia?", "id": "Kemotaksis." },
  { "en": "Siapakah Raja Inggris Yang Menandatangani Magna Carta?", "id": "Raja John." },
  { "en": "Apa Istilah Untuk Pukulan Servis Ilegal Dalam Tenis?", "id": "Foot Fault." },
  { "en": "Apa Nama Mata Uang Resmi Negara Korea Selatan?", "id": "Won Korea Selatan." },
  { "en": "Dari Benua Manakah Asal Tanaman Kapas?", "id": "Amerika, Afrika, India." },
  { "en": "Proses Pembentukan Batuan Dari Sedimen Disebut Apa?", "id": "Litifikasi." },
  { "en": "Siapa Penulis Novel 'A Tale of Two Cities'?", "id": "Charles Dickens." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Tiroid?", "id": "Tiroksin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Yom Kippur?", "id": "Agama Yahudi." },
  { "en": "Apa Sebutan Untuk Awan Yang Berada Di Ketinggian Menengah?", "id": "Awan Alto." },
  { "en": "Berapa Jumlah Tulang Panggul Pada Rangka Manusia?", "id": "2 Tulang." },
  { "en": "Siapakah Pendiri Dinasti Achaemenid Di Persia?", "id": "Koresh Agung." },
  { "en": "Apa Akronim Untuk Organisasi Standardisasi Internasional?", "id": "ISO." },
  { "en": "Di Kota Manakah Terdapat Museum Guggenheim Yang Terkenal?", "id": "Bilbao, Spanyol." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berkamuflase Dengan Lingkungan?", "id": "Bunglon." },
  { "en": "Apa Nama Gelar Juara Dalam Turnamen Tenis?", "id": "Grand Slam." },
  { "en": "Siapa Nama Dewi Pelindung Kota Athena?", "id": "Athena." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara Jerman?", "id": "16 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Mantel Atas Bumi?", "id": "Litosfer." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Helsinki?", "id": "Finlandia." },
  { "en": "Apa Nama Keris Pusaka Milik Pangeran Diponegoro?", "id": "Kanjeng Kiai Ageng Kopek." },
  { "en": "Siapakah Yang Mengemukakan Hukum Elektrolisis?", "id": "Michael Faraday." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Warna?", "id": "Iridensensi." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Yunani?", "id": "24 Huruf." },
  { "en": "Jaringan Apa Yang Membawa Hasil Fotosintesis?", "id": "Jaringan Floem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Es Dan Api?", "id": "Islandia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Orphisme?", "id": "Robert Delaunay." },
  { "en": "Apa Nama Komponen Utama Penyusun Dinding Sel Tumbuhan?", "id": "Selulosa." },
  { "en": "Apa Nama Laut Pedalaman Terbesar Di Dunia?", "id": "Laut Kaspia." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Evolusi?", "id": "Charles Darwin." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Rematik?", "id": "Reumatologi." },
  { "en": "Berapa Jumlah Pukulan Maksimal Dalam Satu Giliran Voli?", "id": "3 Pukulan." },
  { "en": "Apa Nama Ibukota Negara Zimbabwe?", "id": "Harare." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'The Great Gatsby'?", "id": "Nick Carraway." },
  { "en": "Kekurangan Flourida Dapat Menyebabkan Masalah Apa?", "id": "Gigi Berlubang." },
  { "en": "Apa Nama Katedral Terkenal Di Kota Milan?", "id": "Duomo di Milano." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Hipofisis?", "id": "Kelenjar Pituitari." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Museum Louvre?", "id": "I. M. Pei (Piramida)." },
  { "en": "Apa Istilah Untuk Pukulan Keras Dalam Tenis?", "id": "Smash." },
  { "en": "Dimana Lokasi Tembok Berlin Pernah Berdiri?", "id": "Berlin, Jerman." },
  { "en": "Apa Sebutan Untuk Studi Tentang Arthropoda?", "id": "Artropodologi." },
  { "en": "Siapa Pemimpin Amerika Serikat Saat Perang Dingin Dimulai?", "id": "Harry S. Truman." },
  { "en": "Apa Nama Gurun Terluas Di Jazirah Arab?", "id": "Gurun Arab." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Perilaku Hewan?", "id": "Etologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kopi Robusta?", "id": "Afrika Tengah." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Kalingga?", "id": "Ratu Shima." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Proses Metamorfosis?", "id": "Batuan Metamorf." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Rocky?", "id": "Gunung Elbert." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Go?", "id": "Robert Griesemer, Rob Pike." },
  { "en": "Apa Nama Pakaian Tradisional Pria Skotlandia?", "id": "Kilt." },
  { "en": "Apa Satuan Standar Internasional Untuk Dosis Serap Radiasi?", "id": "Gray." },
  { "en": "Siapakah Kaisar Romawi Yang Terkenal Dengan Kegilaannya?", "id": "Caligula." },
  { "en": "Apa Nama Hidangan Daging Asap Khas Rumania?", "id": "Pastramă." },
  { "en": "Apa Nama Hewan Darat Paling Cepat Di Australia?", "id": "Kanguru Merah." },
  { "en": "Apa Akronim Untuk Bank Pembangunan Asia?", "id": "ADB (Asian Development Bank)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Lima?", "id": "7 Bidang." },
  { "en": "Apa Nama Ibukota Negara Kolombia?", "id": "Bogotá." },
  { "en": "Siapa Penulis Novel 'Brave New World'?", "id": "Aldous Huxley." },
  { "en": "Apa Istilah Untuk Studi Tentang Bryophyta (Lumut)?", "id": "Briologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Kanada?", "id": "Berang-Berang." },
  { "en": "Apa Ibukota Provinsi Papua Barat?", "id": "Manokwari." },
  { "en": "Siapa Nama Dewa Penghancur Dalam Mitologi Hindu?", "id": "Siwa." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Pasir Pantai?", "id": "Silikon Dioksida." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Bavarians'?", "id": "Bayern Munich." },
  { "en": "Apa Sebutan Untuk Bintang Yang Sangat Padat Dan Kecil?", "id": "Bintang Neutron." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Samudera Pasai?", "id": "Sultan Malik as-Salih." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Arkeologi?", "id": "Arkeolog." },
  { "en": "Dimana Lokasi Tembok Besar Tiongkok Dimulai?", "id": "Shanhaiguan." },
  { "en": "Berapa Jumlah Tulang Belakang Punggung Pada Manusia?", "id": "12 Tulang." },
  { "en": "Apa Istilah Untuk Angin Yang Berhembus Tetap Sepanjang Tahun?", "id": "Angin Tetap." },
  { "en": "Siapakah Bapak Fisiologi Jantung?", "id": "William Harvey." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Sperma?", "id": "Spermatogenesis." },
  { "en": "Berapa Jumlah Babak Dalam Pertandingan Polo Air?", "id": "4 Babak." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Kendaraan?", "id": "Speedometer." },
  { "en": "Siapakah Jenderal Athena Yang Terkenal Dengan Strategi Perangnya?", "id": "Themistocles." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Orang Suci?", "id": "Hagiokrasi." },
  { "en": "Apa Akronim Untuk 'Be Right Back'?", "id": "BRB." },
  { "en": "Apa Nama Ibukota Negara Nigeria?", "id": "Abuja." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Argentina?", "id": "2 Warna." },
  { "en": "Apa Proses Pembentukan Delta Sungai?", "id": "Deposisi Sedimen." },
  { "en": "Siapa Penjelajah Yang Menemukan Kepulauan Hawaii?", "id": "James Cook." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Dunia?", "id": "Kīlauea." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Pankreas?", "id": "Insulin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Gatot Kaca'?", "id": "Gatotkaca." },
  { "en": "Di Negara Mana Festival Musik Coachella Diadakan?", "id": "Amerika Serikat." },
  { "en": "Apa Nama Ibukota Negara Slovakia?", "id": "Bratislava." },
  { "en": "Siapa Filsuf Jerman Penulis 'Thus Spoke Zarathustra'?", "id": "Friedrich Nietzsche." },
  { "en": "Apa Sebutan Untuk Studi Tentang Bahasa Manusia?", "id": "Linguistik." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segi Sepuluh?", "id": "12 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Kia?", "id": "Korea Selatan." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Si?", "id": "Silikon." },
  { "en": "Siapakah Yang Menulis Novel 'The Stranger'?", "id": "Albert Camus." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Mezcal?", "id": "Tanaman Agave." },
  { "en": "Kapan Peringatan Hari Pos Sedunia Diperingati?", "id": "9 Oktober." },
  { "en": "Apa Nama Proses Penggabungan Inti Atom?", "id": "Fusi Nuklir." },
  { "en": "Siapakah Jenderal Terkenal Dari Kekaisaran Romawi?", "id": "Julius Caesar." },
  { "en": "Apa Istilah Untuk Kemenangan Dalam Satu Babak Langsung?", "id": "Straight Sets." },
  { "en": "Apa Nama Mata Uang Resmi Negara Turki?", "id": "Lira Turki." },
  { "en": "Proses Penghancuran Batuan Oleh Akar Tumbuhan Disebut?", "id": "Pelapukan Biologis." },
  { "en": "Siapa Penulis Novel 'The Sound and the Fury'?", "id": "William Faulkner." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Kortisol." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Vaisakhi?", "id": "Sikhisme." },
  { "en": "Apa Sebutan Untuk Gugusan Bintang Raksasa?", "id": "Gugus Bintang." },
  { "en": "Berapa Jumlah Tulang Wajah Pada Manusia?", "id": "14 Tulang." },
  { "en": "Siapakah Pendiri Dinasti Ming Di Tiongkok?", "id": "Zhu Yuanzhang." },
  { "en": "Apa Akronim Untuk Bank Pembangunan Islam?", "id": "IsDB (Islamic Development Bank)." },
  { "en": "Di Kota Manakah Terdapat Opera House La Scala?", "id": "Milan, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berjalan Di Dinding?", "id": "Cicak." },
  { "en": "Apa Nama Alat Musik Petik Khas Dari Rote?", "id": "Sasando." },
  { "en": "Siapa Nama Dewi Cinta Dan Kecantikan Romawi?", "id": "Venus." },
  { "en": "Berapa Jumlah Negara Anggota Uni Eropa Saat Ini?", "id": "27 Negara." },
  { "en": "Apa Sebutan Untuk Lapisan Inti Bumi Yang Padat?", "id": "Inti Dalam." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Oslo?", "id": "Norwegia." },
  { "en": "Apa Nama Senjata Tradisional Dari Suku Dayak?", "id": "Mandau." },
  { "en": "Siapakah Yang Mengemukakan Teori Kuantum?", "id": "Max Planck." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Pelangi Ganda?", "id": "Pembiasan Ganda." },
  { "en": "Berapa Jumlah Nada Dalam Skala Diatonis?", "id": "7 Nada." },
  { "en": "Jaringan Apa Yang Mengangkut Hasil Fotosintesis?", "id": "Floem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Seribu Danau?", "id": "Finlandia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Kubisme Sintetis?", "id": "Picasso Dan Braque." },
  { "en": "Apa Hukum Kedua Termodinamika Tentang Entropi?", "id": "Peningkatan Entropi." },
  { "en": "Apa Nama Selat Yang Memisahkan Asia Dan Amerika?", "id": "Selat Bering." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Sosiologi?", "id": "Auguste Comte." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sendi?", "id": "Reumatologi." },
  { "en": "Berapa Jumlah Poin Untuk Safety Di American Football?", "id": "2 Poin." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel '1984'?", "id": "Winston Smith." },
  { "en": "Kekurangan Kobalt Dapat Menyebabkan Masalah Apa?", "id": "Anemia." },
  { "en": "Apa Nama Katedral Terkenal Di Kota Cologne?", "id": "Katedral Cologne." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Tiroid?", "id": "Kelenjar Tiroid." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Kota Brasilia?", "id": "Oscar Niemeyer." },
  { "en": "Apa Istilah Untuk Pukulan Memutar Dalam Seni Bela Diri?", "id": "Tendangan Putar." },
  { "en": "Dimana Lokasi Piramida Chichen Itza Berada?", "id": "Meksiko." },
  { "en": "Apa Sebutan Untuk Studi Tentang Amfibi Dan Reptil?", "id": "Herpetologi." },
  { "en": "Siapa Pemimpin Amerika Serikat Selama Perang Dunia II?", "id": "Franklin D. Roosevelt." },
  { "en": "Apa Nama Gurun Terluas Di Amerika Utara?", "id": "Gurun Great Basin." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segidelapan?", "id": "10 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Penuaan?", "id": "Gerontologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kertas?", "id": "Tiongkok." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Majapahit?", "id": "Dyah Gitarja." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Pendinginan Lava?", "id": "Batuan Beku Vulkanik." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Ural?", "id": "Gunung Narodnaya." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Perl?", "id": "Larry Wall." },
  { "en": "Apa Nama Pakaian Tradisional Pria Vietnam?", "id": "Áo Giao Lĩnh." },
  { "en": "Apa Satuan Standar Internasional Untuk Tekanan Suara?", "id": "Pascal." },
  { "en": "Siapakah Kaisar Romawi Yang Membangun Colosseum?", "id": "Vespasian." },
  { "en": "Apa Nama Hidangan Daging Panggang Khas Argentina?", "id": "Asado." },
  { "en": "Apa Nama Hewan Darat Paling Cerdas Di Afrika?", "id": "Gajah Afrika." },
  { "en": "Apa Akronim Untuk Bank Sentral Eropa?", "id": "ECB (European Central Bank)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Enam?", "id": "8 Bidang." },
  { "en": "Apa Nama Ibukota Negara Belarus?", "id": "Minsk." },
  { "en": "Siapa Penulis Novel 'The Handmaid's Tale'?", "id": "Margaret Atwood." },
  { "en": "Apa Istilah Untuk Studi Tentang Kanker?", "id": "Onkologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional China?", "id": "Panda Raksasa." },
  { "en": "Apa Ibukota Provinsi Kepulauan Riau?", "id": "Tanjungpinang." },
  { "en": "Siapa Nama Dewa Pemelihara Dalam Mitologi Hindu?", "id": "Wisnu." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Batubara?", "id": "Karbon." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Old Lady'?", "id": "Juventus." },
  { "en": "Apa Sebutan Untuk Bintang Yang Telah Mati?", "id": "Kataí Putih." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Pajang?", "id": "Sultan Hadiwijaya." },
  { "en": "Dimana Lokasi Air Terjun Angel, Air Terjun Tertinggi?", "id": "Venezuela." },
  { "en": "Berapa Jumlah Tulang Belakang Pinggang Pada Manusia?", "id": "5 Tulang." },
  { "en": "Apa Istilah Untuk Angin Yang Berhembus Dari Lembah?", "id": "Angin Lembah." },
  { "en": "Siapakah Bapak Psikiatri Modern?", "id": "Philippe Pinel." },
  { "en": "Apa Sebutan Untuk Proses Pembelahan Inti Atom?", "id": "Fisi Nuklir." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Angin?", "id": "Anemometer." },
  { "en": "Siapakah Jenderal Athena Yang Terkenal Dengan Pidato Pemakamannya?", "id": "Pericles." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Orang Tua?", "id": "Gerontokrasi." },
  { "en": "Apa Akronim Untuk 'In Other Words'?", "id": "IOW." },
  { "en": "Pada Tahun Berapa Revolusi Perancis Berakhir?", "id": "Tahun 1799." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Yunani?", "id": "2 Warna." },
  { "en": "Apa Proses Pembentukan Delta Di Danau?", "id": "Deposisi Sedimen." },
  { "en": "Siapa Penjelajah Yang Menemukan Jalur Laut Ke India?", "id": "Vasco da Gama." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Eropa?", "id": "Gunung Etna." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Epinefrin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putra Mahkota Astina'?", "id": "Abimanyu." },
  { "en": "Di Negara Mana Festival Musik Lollapalooza Diadakan?", "id": "Amerika Serikat." },
  { "en": "Apa Nama Ibukota Negara Armenia?", "id": "Yerevan." },
  { "en": "Siapa Penulis Novel 'Don Quixote' Yang Terkenal?", "id": "Miguel de Cervantes." },
  { "en": "Apa Sebutan Untuk Studi Tentang Air Tawar?", "id": "Limnologi." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Tabung?", "id": "0 Titik Sudut." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Peugeot?", "id": "Perancis." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol S?", "id": "Belerang (Sulfur)." },
  { "en": "Siapakah Yang Menulis Buku 'The Art of War'?", "id": "Sun Tzu." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Scotch?", "id": "Gandum Malt." },
  { "en": "Kapan Peringatan Hari Statistik Dunia Diperingati?", "id": "20 Oktober." },
  { "en": "Apa Nama Proses Pemisahan Campuran Dengan Penguapan?", "id": "Evaporasi." },
  { "en": "Siapakah Ratu Terkenal Dari Kerajaan Inggris?", "id": "Ratu Victoria." },
  { "en": "Apa Istilah Untuk Gerakan Tiga Langkah Dalam Basket?", "id": "Lay-up." },
  { "en": "Apa Nama Mata Uang Resmi Negara Finlandia?", "id": "Euro." },
  { "en": "Dari Benua Manakah Asal Tanaman Kopi Arabika?", "id": "Afrika." },
  { "en": "Proses Pembekuan Air Menjadi Es Disebut Apa?", "id": "Pembekuan." },
  { "en": "Siapa Penulis Novel 'The Old Man and the Sea'?", "id": "Ernest Hemingway." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Pankreas?", "id": "Insulin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Wafat Isa Almasih?", "id": "Kristen." },
  { "en": "Apa Sebutan Untuk Kumpulan Gas Dan Debu Di Antariksa?", "id": "Nebula." },
  { "en": "Berapa Jumlah Tulang Tempurung Lutut Pada Manusia?", "id": "2 Tulang." },
  { "en": "Siapakah Pendiri Dinasti Safawi Di Persia?", "id": "Ismail I." },
  { "en": "Apa Akronim Untuk Bank Investasi Eropa?", "id": "EIB (European Investment Bank)." },
  { "en": "Di Kota Manakah Terdapat Jembatan Tower Bridge?", "id": "London, Inggris." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menyemprotkan Air Jauh?", "id": "Ikan Pemanah." },
  { "en": "Apa Nama Rumah Adat Dari Provinsi Jawa Tengah?", "id": "Joglo." },
  { "en": "Siapa Nama Dewa Laut Dan Gempa Bumi Romawi?", "id": "Neptunus." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara India?", "id": "28 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Inti Bumi Yang Cair?", "id": "Inti Luar." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Athena?", "id": "Yunani." },
  { "en": "Apa Nama Senjata Tradisional Dari Sulawesi Selatan?", "id": "Badik." },
  { "en": "Siapakah Yang Mengemukakan Hukum Termodinamika?", "id": "Sadi Carnot." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Bishop?", "id": "Difraksi." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Korea (Hangul)?", "id": "24 Huruf." },
  { "en": "Jaringan Apa Yang Memberi Bentuk Pada Organ Hewan?", "id": "Jaringan Ikat." },
  { "en": "Negara Apa Yang Dijuluki Negeri Naga Asia?", "id": "Korea Selatan, Taiwan, Singapura." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Suprematisme?", "id": "Kazimir Malevich." },
  { "en": "Apa Nama Proses Perubahan Gas Menjadi Padat?", "id": "Deposisi." },
  { "en": "Apa Nama Laut Yang Memisahkan Eropa Dan Afrika?", "id": "Laut Mediterania." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Kimia?", "id": "Antoine Lavoisier." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Imun?", "id": "Imunologi." },
  { "en": "Berapa Jarak Lari Halang Rintang Putri?", "id": "3000 Meter." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Hamlet'?", "id": "Pangeran Hamlet." },
  { "en": "Kekurangan Mangan Dapat Menyebabkan Masalah Apa?", "id": "Pertumbuhan Tulang Terganggu." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Wina?", "id": "Istana Hofburg." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Paratiroid?", "id": "Kelenjar Paratiroid." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Istana Versailles?", "id": "Louis Le Vau." },
  { "en": "Apa Istilah Untuk Pukulan Melambung Dalam Sepak Bola?", "id": "Tendangan Lambung." },
  { "en": "Dimana Lokasi Tembok Besar Tiongkok Berada?", "id": "Tiongkok Utara." },
  { "en": "Apa Sebutan Untuk Studi Tentang Laba-Laba?", "id": "Arachnologi." },
  { "en": "Siapa Pemimpin Inggris Saat Perang Falkland?", "id": "Margaret Thatcher." },
  { "en": "Apa Nama Gurun Terluas Di Amerika Selatan?", "id": "Gurun Patagonia." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Penyakit Tumbuhan?", "id": "Fitopatologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Cokelat Susu?", "id": "Swiss." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Tarumanegara?", "id": "Tidak Ada Ratu Terkenal." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Proses Erosi?", "id": "Batuan Sedimen Klastik." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Kaukasus?", "id": "Gunung Elbrus." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Lisp?", "id": "John McCarthy." },
  { "en": "Apa Nama Pakaian Tradisional Pria India?", "id": "Dhoti Atau Kurta." },
  { "en": "Apa Satuan Standar Internasional Untuk Katalis?", "id": "Katal." },
  { "en": "Siapakah Kaisar Romawi Yang Memerintah Paling Singkat?", "id": "Gordian I Dan Gordian II." },
  { "en": "Apa Nama Hidangan Sup Mie Khas Jepang?", "id": "Ramen." },
  { "en": "Apa Nama Hewan Darat Paling Cepat Di Dunia?", "id": "Cheetah." },
  { "en": "Apa Akronim Untuk Organisasi Negara-Negara Amerika?", "id": "OAS (Organization of American States)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Tujuh?", "id": "9 Bidang." },
  { "en": "Apa Nama Ibukota Negara Albania?", "id": "Tirana." },
  { "en": "Siapa Penulis Novel 'The Trial'?", "id": "Franz Kafka." },
  { "en": "Apa Istilah Untuk Studi Tentang Krustasea?", "id": "Karsinologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Amerika?", "id": "Elang Botak." },
  { "en": "Apa Ibukota Provinsi Bengkulu?", "id": "Bengkulu." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Romawi?", "id": "Mars." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Minyak Bumi?", "id": "Hidrokarbon." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Toffees'?", "id": "Everton FC." },
  { "en": "Apa Sebutan Untuk Bintang Yang Sangat Panas Dan Biru?", "id": "Bintang Tipe O." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Banten?", "id": "Sultan Ageng Tirtayasa." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Gemologi?", "id": "Gemolog." },
  { "en": "Dimana Lokasi Air Terjun Victoria Berada?", "id": "Zambia Dan Zimbabwe." },
  { "en": "Berapa Jumlah Tulang Belakang Pinggul Pada Manusia?", "id": "1 Tulang (Sakrum)." },
  { "en": "Apa Istilah Untuk Angin Yang Berhembus Dari Gunung?", "id": "Angin Gunung." },
  { "en": "Siapakah Bapak Psikologi Analitis?", "id": "Carl Jung." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Logam?", "id": "Metalurgi." },
  { "en": "Berapa Jumlah Babak Dalam Pertandingan Tinju Profesional?", "id": "Maksimal 12 Babak." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Rotasi?", "id": "Tachometer." },
  { "en": "Siapakah Jenderal Romawi Yang Terkenal Dengan Pasukan Gajahnya?", "id": "Bukan Romawi, Tapi Hannibal." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Para Bijak?", "id": "Sofokrasi." },
  { "en": "Apa Akronim Untuk 'Point Of View'?", "id": "POV." },
  { "en": "Pada Tahun Berapa Revolusi EDSA Di Filipina Terjadi?", "id": "Tahun 1986." },
  { "en": "Apa Nama Ibukota Negara Makedonia Utara?", "id": "Skopje." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Portugal?", "id": "2 Warna." },
  { "en": "Apa Proses Pembentukan Delta Di Laut?", "id": "Sedimentasi Marin." },
  { "en": "Siapa Penjelajah Yang Menemukan Samudra Pasifik?", "id": "Vasco Núñez de Balboa." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Islandia?", "id": "Gunung Hekla." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Aldosteron." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putra Sang Bayu'?", "id": "Bima." },
  { "en": "Di Negara Mana Festival Hari Orang Mati Diadakan?", "id": "Meksiko." },
  { "en": "Apa Nama Ibukota Negara Namibia?", "id": "Windhoek." },
  { "en": "Siapa Penulis Novel Fiksi 'Slaughterhouse-Five'?", "id": "Kurt Vonnegut." },
  { "en": "Apa Sebutan Untuk Studi Tentang Tanah?", "id": "Pedologi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Limas Segi Tujuh?", "id": "8 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Citroën?", "id": "Perancis." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol V?", "id": "Vanadium." },
  { "en": "Siapakah Yang Menulis Novel 'Catch-22'?", "id": "Joseph Heller." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Kombucha?", "id": "Teh Manis Fermentasi." },
  { "en": "Kapan Peringatan Hari Penerbangan Sipil Internasional?", "id": "7 Desember." },
  { "en": "Apa Nama Proses Pelepasan Energi Dalam Sel?", "id": "Respirasi Seluler." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Mesir?", "id": "Cleopatra." },
  { "en": "Apa Istilah Untuk Skor Sempurna Dalam Senam Artistik?", "id": "Perfect 10." },
  { "en": "Apa Nama Mata Uang Resmi Negara Denmark?", "id": "Krone Denmark." },
  { "en": "Dari Benua Manakah Asal Tanaman Kopi Robusta?", "id": "Afrika." },
  { "en": "Proses Pengendapan Sedimen Oleh Gletser Disebut Apa?", "id": "Deposisi Glasial." },
  { "en": "Siapa Penulis Novel 'The Bell Jar'?", "id": "Sylvia Plath." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Natal Ortodoks?", "id": "Kristen Ortodoks." },
  { "en": "Apa Sebutan Untuk Bintang Yang Paling Dekat Bumi?", "id": "Matahari." },
  { "en": "Berapa Jumlah Tulang Tengkorak Bagian Otak Manusia?", "id": "8 Tulang." },
  { "en": "Siapakah Pendiri Kekaisaran Mongol Yang Agung?", "id": "Genghis Khan." },
  { "en": "Apa Akronim Untuk Dana Penduduk Perserikatan Bangsa-Bangsa?", "id": "UNFPA." },
  { "en": "Di Kota Manakah Terdapat Jembatan Ponte Vecchio?", "id": "Florence, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Melompat Sangat Tinggi?", "id": "Kutu." },
  { "en": "Apa Nama Pakaian Adat Wanita Dari Minangkabau?", "id": "Bundo Kanduang." },
  { "en": "Siapa Nama Dewi Rumah Tangga Dalam Mitologi Romawi?", "id": "Vesta." },
  { "en": "Berapa Jumlah Negara Bagian Di Negara Meksiko?", "id": "32 Negara Bagian." },
  { "en": "Apa Sebutan Untuk Lapisan Terluar Inti Bumi?", "id": "Inti Luar." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Lisbon?", "id": "Portugal." },
  { "en": "Apa Nama Senjata Tradisional Dari Provinsi Aceh?", "id": "Rencong." },
  { "en": "Siapakah Yang Mengemukakan Teori Kuantum Elektrodinamika?", "id": "Richard Feynman." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Warna?", "id": "Iridisensi." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Thai?", "id": "44 Huruf Konsonan." },
  { "en": "Jaringan Apa Yang Memberi Kekuatan Mekanis Pada Tumbuhan?", "id": "Jaringan Kolenkim." },
  { "en": "Negara Apa Yang Dijuluki Negeri Siang Tak Berkesudahan?", "id": "Norwegia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Neoklasikisme?", "id": "Jacques-Louis David." },
  { "en": "Apa Nama Gas Yang Memberi Warna Merah Pada Daging?", "id": "Mioglobin." },
  { "en": "Apa Nama Selat Yang Memisahkan Pulau Jawa Dan Madura?", "id": "Selat Madura." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Geografi Modern?", "id": "Alexander Von Humboldt." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Muskuloskeletal?", "id": "Ortopedi." },
  { "en": "Berapa Jarak Lari Halang Rintang Putra?", "id": "3000 Meter." },
  { "en": "Apa Nama Ibukota Negara San Marino?", "id": "San Marino." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Dracula'?", "id": "Jonathan Harker." },
  { "en": "Kekurangan Tiamin Dapat Menyebabkan Penyakit Apa?", "id": "Beri-Beri." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Katedral Florence?", "id": "Filippo Brunelleschi." },
  { "en": "Dimana Lokasi Tembok Berlin Pernah Dibangun?", "id": "Berlin Timur Dan Barat." },
  { "en": "Apa Sebutan Untuk Studi Tentang Moluska?", "id": "Malakologi." },
  { "en": "Siapa Pemimpin Gerakan Hak Sipil Di Amerika Serikat?", "id": "Martin Luther King Jr." },
  { "en": "Apa Nama Gurun Terluas Di Semenanjung Arab?", "id": "Gurun Arab." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Prisma Segisembilan?", "id": "18 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Penyakit Jiwa?", "id": "Psikopatologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Makanan Sushi?", "id": "Jepang." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kekaisaran Bizantium?", "id": "Theodora." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Pendinginan Cepat Magma?", "id": "Batuan Beku Vulkanik." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Andes?", "id": "Gunung Aconcagua." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman PHP?", "id": "Rasmus Lerdorf." },
  { "en": "Apa Nama Pakaian Tradisional Pria Jepang?", "id": "Kimono." },
  { "en": "Apa Satuan Standar Internasional Untuk Fluks Cahaya?", "id": "Lumen." },
  { "en": "Siapakah Kaisar Romawi Yang Memulai Pembangunan Pantheon?", "id": "Hadrian." },
  { "en": "Apa Nama Hidangan Sup Iga Khas Korea?", "id": "Galbitang." },
  { "en": "Apa Nama Hewan Darat Paling Cepat Di Amerika Selatan?", "id": "Guanako." },
  { "en": "Apa Akronim Untuk Organisasi Kerjasama Islam?", "id": "OKI (Organisasi Kerjasama Islam)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Delapan?", "id": "10 Bidang." },
  { "en": "Apa Nama Ibukota Negara Andora?", "id": "Andorra la Vella." },
  { "en": "Siapa Penulis Novel 'The Grapes of Wrath'?", "id": "John Steinbeck." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional India?", "id": "Harimau Benggala." },
  { "en": "Apa Ibukota Provinsi Bangka Belitung?", "id": "Pangkal Pinang." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Hindu?", "id": "Murugan." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Pasir?", "id": "SiO2." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Reds'?", "id": "Liverpool FC." },
  { "en": "Apa Sebutan Untuk Bintang Yang Sangat Dingin Dan Merah?", "id": "Kataí Merah." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Majapahit?", "id": "Raden Wijaya." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Entomologi?", "id": "Entomolog." },
  { "en": "Dimana Lokasi Air Terjun Niagara Berada?", "id": "Kanada Dan Amerika Serikat." },
  { "en": "Berapa Jumlah Tulang Belakang Punggung Bawah Manusia?", "id": "5 Tulang." },
  { "en": "Apa Istilah Untuk Angin Yang Berhembus Dari Darat Ke Laut?", "id": "Angin Darat." },
  { "en": "Siapakah Bapak Psikoanalisis?", "id": "Sigmund Freud." },
  { "en": "Apa Sebutan Untuk Proses Pembentukan Urin?", "id": "Diuresis." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Hoki Air?", "id": "6 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Tekanan Uap?", "id": "Manometer." },
  { "en": "Siapakah Jenderal Athena Yang Terkenal Dengan Taktik Phalanx?", "id": "Epaminondas." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Para Ahli?", "id": "Meritokrasi." },
  { "en": "Apa Akronim Untuk 'Too Long; Didn't Read'?", "id": "TL;DR." },
  { "en": "Apa Nama Ibukota Negara Liechtenstein?", "id": "Vaduz." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Swiss?", "id": "2 Warna." },
  { "en": "Apa Proses Pembentukan Stalagmit Di Lantai Gua?", "id": "Deposisi Kalsium Karbonat." },
  { "en": "Siapa Penjelajah Yang Menemukan Jalur Laut Ke Amerika?", "id": "Christopher Columbus." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Filipina?", "id": "Gunung Mayon." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Paratiroid?", "id": "Hormon Paratiroid." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putra Arjuna'?", "id": "Abimanyu." },
  { "en": "Di Negara Mana Festival Musik Rock in Rio Diadakan?", "id": "Brasil." },
  { "en": "Apa Nama Ibukota Negara Botswana?", "id": "Gaborone." },
  { "en": "Siapa Penulis Novel Klasik 'Heart of Darkness'?", "id": "Joseph Conrad." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segi Sembilan?", "id": "11 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Rolls-Royce?", "id": "Inggris." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol W?", "id": "Tungsten (Wolfram)." },
  { "en": "Siapakah Yang Menulis Buku 'The Second Sex'?", "id": "Simone de Beauvoir." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Anggur?", "id": "Buah Anggur." },
  { "en": "Kapan Peringatan Hari Bahasa Ibu Internasional?", "id": "21 Februari." },
  { "en": "Apa Nama Proses Penguraian Air Oleh Listrik?", "id": "Elektrolisis." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Perancis?", "id": "Marie Antoinette." },
  { "en": "Apa Nama Perlombaan Dayung Tahunan Di Sungai Thames?", "id": "The Boat Race." },
  { "en": "Apa Nama Mata Uang Resmi Negara Uni Emirat Arab?", "id": "Dirham." },
  { "en": "Dari Benua Manakah Asal Tanaman Cokelat?", "id": "Amerika." },
  { "en": "Proses Pembentukan Batuan Oleh Panas Dan Tekanan?", "id": "Metamorfisme." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Adrenal?", "id": "Adrenalin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Imlek?", "id": "Konghucu." },
  { "en": "Apa Sebutan Untuk Bintang Yang Baru Terbentuk?", "id": "Protostar." },
  { "en": "Siapakah Pendiri Dinasti Ottoman Di Turki?", "id": "Osman I." },
  { "en": "Apa Akronim Untuk Program Pembangunan Perserikatan Bangsa-Bangsa?", "id": "UNDP." },
  { "en": "Di Kota Manakah Terdapat Jembatan Charles?", "id": "Praha, Ceko." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berlari Di Air?", "id": "Kadal Basilisk." },
  { "en": "Apa Nama Rumah Adat Dari Toraja?", "id": "Tongkonan." },
  { "en": "Siapa Nama Dewi Kecantikan Dalam Mitologi Yunani?", "id": "Aphrodite." },
  { "en": "Berapa Jumlah Negara Anggota NATO Saat Ini?", "id": "32 Negara." },
  { "en": "Apa Nama Senjata Tradisional Dari Jawa Barat?", "id": "Kujang." },
  { "en": "Apa Nama Titik Didih Dan Beku Air Dalam Kelvin?", "id": "373 K Dan 273 K." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Arab?", "id": "28 Huruf." },
  { "en": "Jaringan Apa Yang Mengangkut Air Dalam Tumbuhan?", "id": "Xilem." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Futurisme?", "id": "Filippo Marinetti." },
  { "en": "Apa Nama Gas Yang Digunakan Untuk Mengisi Balon?", "id": "Helium." },
  { "en": "Apa Nama Laut Yang Memisahkan Eropa Dan Asia?", "id": "Laut Kaspia." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Genetika?", "id": "Gregor Mendel." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Saraf?", "id": "Neurologi." },
  { "en": "Berapa Jarak Lari Halang Rintang Untuk Wanita?", "id": "3000 Meter." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Moby Dick'?", "id": "Kapten Ahab." },
  { "en": "Kekurangan Iodium Dapat Menyebabkan Penyakit Apa?", "id": "Gondok." },
  { "en": "Apa Nama Katedral Terkenal Di Kota Paris?", "id": "Notre Dame." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Museum Guggenheim?", "id": "Frank Lloyd Wright." },
  { "en": "Dimana Lokasi Tembok Berlin Pernah Dibangun?", "id": "Berlin, Jerman." },
  { "en": "Apa Sebutan Untuk Studi Tentang Laba-Laba?", "id": "Araneologi." },
  { "en": "Siapa Pemimpin Amerika Serikat Saat Perang Vietnam?", "id": "Beberapa Presiden." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Limas Segi Delapan?", "id": "9 Sisi." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Majapahit?", "id": "Ratu Suhita." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Pendinginan Lava?", "id": "Batuan Beku." },
  { "en": "Apa Satuan Standar Internasional Untuk Tekanan?", "id": "Pascal." },
  { "en": "Apa Nama Hidangan Daging Panggang Khas Brasil?", "id": "Churrasco." },
  { "en": "Apa Akronim Untuk Bank Pembangunan Afrika?", "id": "AfDB (African Development Bank)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Sembilan?", "id": "11 Bidang." },
  { "en": "Apa Nama Ibukota Negara Moldova?", "id": "Chișinău." },
  { "en": "Siapa Penulis Novel 'The Great Gatsby'?", "id": "F. Scott Fitzgerald." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Amerika?", "id": "Bison Amerika." },
  { "en": "Apa Ibukota Provinsi Jambi?", "id": "Jambi." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Pensioners'?", "id": "Chelsea FC." },
  { "en": "Apa Sebutan Untuk Bintang Yang Paling Panas?", "id": "Bintang Biru." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Gowa?", "id": "Sultan Hasanuddin." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Epigrafi?", "id": "Epigrafis." },
  { "en": "Dimana Lokasi Air Terjun Iguazu Berada?", "id": "Brasil Dan Argentina." },
  { "en": "Apa Istilah Untuk Angin Yang Berhembus Dari Laut?", "id": "Angin Laut." },
  { "en": "Siapakah Bapak Psikologi Kognitif?", "id": "Ulric Neisser." },
  { "en": "Siapakah Jenderal Athena Yang Terkenal Dengan Taktik Perangnya?", "id": "Themistocles." },
  { "en": "Apa Akronim Untuk 'In Case You Missed It'?", "id": "ICYMI." },
  { "en": "Pada Tahun Berapa Revolusi Perancis Dimulai?", "id": "Tahun 1789." },
  { "en": "Apa Nama Ibukota Negara Georgia?", "id": "Tbilisi." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Prancis?", "id": "3 Warna." },
  { "en": "Apa Nama Gunung Berapi Paling Aktif Di Italia?", "id": "Gunung Stromboli." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Tiroid?", "id": "Kalsitonin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putra Sang Surya'?", "id": "Karna." },
  { "en": "Di Negara Mana Festival Musik Sziget Diadakan?", "id": "Hungaria." },
  { "en": "Apa Nama Ibukota Negara Trinidad Dan Tobago?", "id": "Port of Spain." },
  { "en": "Siapa Filsuf Eksistensialis Penulis 'Being and Nothingness'?", "id": "Jean-Paul Sartre." },
  { "en": "Berapa Jumlah Muka Sisi Pada Sebuah Dodekahedron?", "id": "12 Muka." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Koenigsegg?", "id": "Swedia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Ra?", "id": "Radium." },
  { "en": "Siapakah Yang Menulis Buku 'On Liberty'?", "id": "John Stuart Mill." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Absinthe?", "id": "Artemisia Absinthium." },
  { "en": "Kapan Peringatan Hari Arsitektur Dunia Diperingati?", "id": "Senin Pertama Oktober." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Akibat Air?", "id": "Hidrotropisme." },
  { "en": "Siapakah Ratu Terakhir Dari Dinasti Ptolemaik?", "id": "Cleopatra VII." },
  { "en": "Apa Istilah Untuk Gerakan Menangkis Dalam Anggar?", "id": "Parry." },
  { "en": "Apa Nama Mata Uang Resmi Negara Kroasia?", "id": "Euro (Sebelumnya Kuna)." },
  { "en": "Dari Benua Manakah Asal Tanaman Kaktus?", "id": "Amerika." },
  { "en": "Proses Pengangkutan Batuan Oleh Angin Disebut Apa?", "id": "Deflasi." },
  { "en": "Siapa Penulis Novel 'A Clockwork Orange'?", "id": "Anthony Burgess." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Oleh Kelenjar Timus?", "id": "Timosin." },
  { "en": "Agama Manakah Yang Merayakan Festival Holi?", "id": "Hindu." },
  { "en": "Apa Sebutan Untuk Galaksi Spiral Terdekat Kita?", "id": "Galaksi Andromeda." },
  { "en": "Siapakah Pendiri Dinasti Umayyah Di Damaskus?", "id": "Muawiyah I." },
  { "en": "Apa Akronim Untuk Dana Internasional Pembangunan Pertanian?", "id": "IFAD." },
  { "en": "Di Kota Manakah Terdapat Museum Prado Yang Terkenal?", "id": "Madrid, Spanyol." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menyemprotkan Asam Format?", "id": "Semut." },
  { "en": "Apa Nama Upacara Adat Bakar Batu Dari Papua?", "id": "Bakar Batu." },
  { "en": "Siapa Nama Dewi Keadilan Dalam Mitologi Romawi?", "id": "Justitia." },
  { "en": "Berapa Jumlah Provinsi Dan Teritori Di Kanada?", "id": "10 Provinsi, 3 Teritori." },
  { "en": "Apa Sebutan Untuk Lapisan Mantel Bawah Bumi?", "id": "Mesosfer." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Bratislava?", "id": "Slovakia." },
  { "en": "Apa Nama Senjata Tradisional Suku Asmat?", "id": "Tombak Dan Panah." },
  { "en": "Siapakah Yang Mengemukakan Hukum Pendinginan Newton?", "id": "Isaac Newton." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Es?", "id": "Halo Matahari." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Georgia?", "id": "33 Huruf." },
  { "en": "Jaringan Apa Yang Berfungsi Menyimpan Udara Pada Tumbuhan?", "id": "Aerenkim." },
  { "en": "Negara Apa Yang Dijuluki Negeri Hujan Tropis?", "id": "Kolombia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Ekspresionisme Abstrak?", "id": "Jackson Pollock." },
  { "en": "Apa Hukum Ketiga Termodinamika Tentang Suhu Nol Absolut?", "id": "Entropi Mencapai Nilai Minimum." },
  { "en": "Apa Nama Selat Yang Memisahkan Spanyol Dan Maroko?", "id": "Selat Gibraltar." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Fisiologi?", "id": "Claude Bernard." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Lanjut Usia?", "id": "Geriatri." },
  { "en": "Berapa Jumlah Poin Untuk Sebuah 'Try' Dalam Rugbi?", "id": "5 Poin." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Frankenstein'?", "id": "Victor Frankenstein." },
  { "en": "Kekurangan Vitamin B6 Dapat Menyebabkan Masalah Apa?", "id": "Anemia, Gangguan Saraf." },
  { "en": "Apa Nama Katedral Terkenal Di Kota Florence?", "id": "Katedral Florence (Duomo)." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Saliva?", "id": "Kelenjar Ludah." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Museum Yahudi Berlin?", "id": "Daniel Libeskind." },
  { "en": "Apa Istilah Untuk Pelanggaran Dalam Hoki Es?", "id": "Penalty." },
  { "en": "Dimana Lokasi Tembok Hadrianus Dibangun Oleh Roma?", "id": "Inggris Utara." },
  { "en": "Apa Sebutan Untuk Studi Tentang Mamalia Laut?", "id": "Setologi." },
  { "en": "Siapa Pemimpin Amerika Serikat Saat Pembelian Louisiana?", "id": "Thomas Jefferson." },
  { "en": "Apa Nama Gurun Terluas Di Afrika Bagian Selatan?", "id": "Gurun Kalahari." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segi Sebelas?", "id": "13 Sisi." },
  { "en": "Siapakah Ratu Yang Terkenal Dari Kerajaan Mataram Kuno?", "id": "Pramodhawardhani." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Pendinginan Lambat Magma?", "id": "Batuan Beku Plutonik." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Pyrenees?", "id": "Pico de Aneto." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman JavaScript?", "id": "Brendan Eich." },
  { "en": "Apa Nama Pakaian Tradisional Pria Korea?", "id": "Hanbok." },
  { "en": "Siapakah Kaisar Romawi Yang Memulai Pembangunan Colosseum?", "id": "Vespasianus." },
  { "en": "Apa Nama Hidangan Sup Kental Khas Perancis?", "id": "Bisque." },
  { "en": "Apa Akronim Untuk Dana Anak-Anak Perserikatan Bangsa-Bangsa?", "id": "UNICEF." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Sebelas?", "id": "13 Bidang." },
  { "en": "Apa Nama Ibukota Negara Bosnia Dan Herzegovina?", "id": "Sarajevo." },
  { "en": "Siapa Penulis Novel 'The Stranger'?", "id": "Albert Camus." },
  { "en": "Apa Ibukota Provinsi Papua Selatan?", "id": "Merauke." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Mesir?", "id": "Montu." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Blues'?", "id": "Chelsea F.C." },
  { "en": "Apa Nama Ibukota Negara Bhutan?", "id": "Thimphu." },
  { "en": "Siapa Penulis Novel 'The Stranger' Yang Terkenal?", "id": "Albert Camus." },
  { "en": "Apa Sebutan Untuk Studi Tentang Awan?", "id": "Nefologi." },
  { "en": "Berapa Jumlah Muka Sisi Pada Sebuah Icosahedron?", "id": "20 Muka." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Lada?", "id": "Rusia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Rn?", "id": "Radon." },
  { "en": "Siapakah Yang Menulis Drama 'Pygmalion'?", "id": "George Bernard Shaw." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Anggur?", "id": "Buah Anggur Fermentasi." },
  { "en": "Kapan Peringatan Hari Migran Internasional Diperingati?", "id": "18 Desember." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Karena Gelap?", "id": "Skototropisme." },
  { "en": "Apa Istilah Untuk Batas Area Bermain Lapangan?", "id": "Out Of Bounds." },
  { "en": "Apa Nama Mata Uang Resmi Negara Islandia?", "id": "Krona Islandia." },
  { "en": "Dari Benua Manakah Asal Tanaman Kopi?", "id": "Afrika." },
  { "en": "Proses Perubahan Batuan Oleh Aktivitas Kimia Disebut?", "id": "Pelapukan Kimiawi." },
  { "en": "Siapa Penulis Novel Horor 'Dracula'?", "id": "Bram Stoker." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Adrenal?", "id": "Epinefrin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Shavuot?", "id": "Yahudi." },
  { "en": "Berapa Jumlah Tulang Rusuk Palsu Pada Manusia?", "id": "6 Tulang (3 Pasang)." },
  { "en": "Siapakah Pendiri Dinasti Tokugawa Di Jepang?", "id": "Tokugawa Ieyasu." },
  { "en": "Apa Akronim Untuk Badan Antariksa Jepang?", "id": "JAXA." },
  { "en": "Di Kota Manakah Terdapat Patung The Little Mermaid?", "id": "Kopenhagen, Denmark." },
  { "en": "Hewan Apa Yang Dikenal Dapat Tidur Sambil Berdiri?", "id": "Kuda Dan Flamingo." },
  { "en": "Apa Nama Tarian Tradisional Dari Suku Batak?", "id": "Tari Tortor." },
  { "en": "Siapa Nama Dewi Bulan Dalam Mitologi Romawi?", "id": "Luna." },
  { "en": "Apa Nama Senjata Tradisional Dari Nusa Tenggara Timur?", "id": "Sundu." },
  { "en": "Siapakah Yang Mengemukakan Hukum Gerak Ketiga?", "id": "Isaac Newton." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Cincin Pelangi?", "id": "Glori." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Armenia?", "id": "39 Huruf." },
  { "en": "Jaringan Apa Yang Memberi Dukungan Pada Tumbuhan?", "id": "Jaringan Penyokong." },
  { "en": "Negara Apa Yang Dijuluki Negeri Kincir Angin?", "id": "Belanda." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Abstrak Geometris?", "id": "Piet Mondrian." },
  { "en": "Apa Nama Proses Kimia Yang Melibatkan Oksigen?", "id": "Oksidasi." },
  { "en": "Apa Nama Selat Yang Memisahkan Australia Dan Tasmania?", "id": "Selat Bass." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Mikroskopi?", "id": "Antonie van Leeuwenhoek." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Endokrin?", "id": "Endokrinologi." },
  { "en": "Berapa Jumlah Poin Untuk Field Goal Di American Football?", "id": "3 Poin." },
  { "en": "Apa Nama Ibukota Negara Lebanon?", "id": "Beirut." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel 'Wuthering Heights'?", "id": "Catherine Earnshaw, Heathcliff." },
  { "en": "Kekurangan Vitamin B9 Dapat Menyebabkan Masalah Apa?", "id": "Anemia Megaloblastik." },
  { "en": "Apa Nama Istana Terkenal Yang Ada Di Moskow?", "id": "Kremlin." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Pusat Kendali Tubuh?", "id": "Otak." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Menara Willis?", "id": "Fazlur Rahman Khan." },
  { "en": "Apa Istilah Untuk Pelanggaran Dalam Permainan Catur?", "id": "Langkah Ilegal." },
  { "en": "Dimana Lokasi Kuil Abu Simbel Yang Megah?", "id": "Mesir." },
  { "en": "Siapa Pemimpin Amerika Serikat Saat Proklamasi Emansipasi?", "id": "Abraham Lincoln." },
  { "en": "Apa Nama Gurun Terluas Di Benua Asia?", "id": "Gurun Arab." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segi Duapuluh?", "id": "22 Sisi." },
  { "en": "Siapakah Ratu Terkenal Dari Kerajaan Sriwijaya?", "id": "Tidak Ada Data Jelas." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Proses Erosi?", "id": "Batuan Sedimen." },
  { "en": "Apa Nama Titik Tertinggi Di Pegunungan Himalaya?", "id": "Gunung Everest." },
  { "en": "Apa Satuan Standar Internasional Untuk Dosis Ekuivalen?", "id": "Sievert." },
  { "en": "Siapakah Kaisar Romawi Yang Memerintah Paling Singkat?", "id": "Gordianus I Dan II." },
  { "en": "Apa Nama Hidangan Daging Asap Khas Brasil?", "id": "Churrasco." },
  { "en": "Apa Akronim Untuk Organisasi Kesehatan Dunia?", "id": "WHO." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segi Duapuluh?", "id": "22 Bidang." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional China?", "id": "Panda." },
  { "en": "Apa Ibukota Provinsi Papua Tengah?", "id": "Nabire." },
  { "en": "Klub Sepak Bola Mana Yang Dijuluki 'The Gunners'?", "id": "Arsenal." },
  { "en": "Apa Sebutan Untuk Bintang Yang Telah Mati?", "id": "Katai Putih." },
  { "en": "Dimana Lokasi Air Terjun Angel Berada?", "id": "Venezuela." },
  { "en": "Apa Nama Ibukota Negara Barbados?", "id": "Bridgetown." },
  { "en": "Siapa Penulis Puisi Epik 'The Divine Comedy'?", "id": "Dante Alighieri." },
  { "en": "Berapa Jumlah Muka Sisi Pada Sebuah Oktahedron?", "id": "8 Muka." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Lotus?", "id": "Inggris." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Cs?", "id": "Sesium." },
  { "en": "Siapakah Yang Menulis Buku 'Utopia'?", "id": "Thomas More." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Sake?", "id": "Beras Yang Difermentasi." },
  { "en": "Kapan Peringatan Hari Bahasa Isyarat Internasional?", "id": "23 September." },
  { "en": "Apa Nama Proses Pergerakan Tumbuhan Karena Sentuhan?", "id": "Tigmonasti." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Hawaii?", "id": "Liliʻuokalani." },
  { "en": "Apa Sebutan Untuk Pukulan Pembuka Dalam Golf?", "id": "Tee Shot." },
  { "en": "Apa Nama Mata Uang Resmi Negara Aljazair?", "id": "Dinar Aljazair." },
  { "en": "Proses Pengerasan Sedimen Menjadi Batuan Disebut?", "id": "Litifikasi." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Pineal?", "id": "Melatonin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Bodhi?", "id": "Buddha." },
  { "en": "Apa Sebutan Untuk Bintang Terdekat Dengan Matahari?", "id": "Proxima Centauri." },
  { "en": "Berapa Jumlah Tulang Rusuk Sejati Pada Manusia?", "id": "14 Tulang (7 Pasang)." },
  { "en": "Siapakah Pendiri Dinasti Joseon Di Korea?", "id": "Yi Seong-gye." },
  { "en": "Apa Akronim Untuk Simple Mail Transfer Protocol?", "id": "SMTP." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berubah Warna?", "id": "Bunglon." },
  { "en": "Apa Nama Tarian Selamat Datang Dari Betawi?", "id": "Tari Sirih Kuning." },
  { "en": "Siapa Nama Dewi Kebijaksanaan Dalam Mitologi Yunani?", "id": "Athena." },
  { "en": "Apa Nama Senjata Tradisional Dari Suku Madura?", "id": "Celurit." },
  { "en": "Apa Nama Skala Untuk Mengukur Kekuatan Angin Topan?", "id": "Skala Saffir-Simpson." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Dadaisme?", "id": "Tristan Tzara." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Las Listrik?", "id": "Argon." },
  { "en": "Apa Nama Laut Yang Memisahkan Afrika Dan Asia?", "id": "Laut Merah." },
  { "en": "Berapa Jarak Lari Gawang Untuk Wanita?", "id": "100 Meter." },
  { "en": "Kekurangan Mangan Dapat Menyebabkan Masalah Apa?", "id": "Gangguan Tulang." },
  { "en": "Apa Nama Katedral Terkenal Di Kota Moskow?", "id": "Katedral Santo Basil." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Kota Chandigarh?", "id": "Le Corbusier." },
  { "en": "Apa Istilah Untuk Pukulan Keras Dalam Voli?", "id": "Spike." },
  { "en": "Dimana Lokasi Tembok Besar Tiongkok Berakhir?", "id": "Gurun Gobi." },
  { "en": "Apa Sebutan Untuk Studi Tentang Lumut Kerak?", "id": "Likenologi." },
  { "en": "Siapa Pemimpin Amerika Serikat Saat Perang Teluk?", "id": "George H. W. Bush." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segisepuluh?", "id": "12 Sisi." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Swift?", "id": "Chris Lattner." },
  { "en": "Apa Nama Pakaian Tradisional Pria Vietnam?", "id": "Ao Dai." },
  { "en": "Siapakah Kaisar Romawi Yang Memerintah Paling Singkat?", "id": "Gordian I." },
  { "en": "Apa Nama Hewan Darat Paling Cerdas Di Asia?", "id": "Gajah Asia." },
  { "en": "Apa Akronim Untuk Bank Pembangunan Antar-Amerika?", "id": "IDB (Inter-American Development Bank)." },
  { "en": "Berapa Jumlah Bidang Sisi Pada Sebuah Prisma Segisepuluh?", "id": "12 Bidang." },
  { "en": "Apa Nama Ibukota Negara Azerbaijan?", "id": "Baku." },
  { "en": "Apa Ibukota Provinsi Papua Pegunungan?", "id": "Jayawijaya." },
  { "en": "Apa Sebutan Untuk Bintang Yang Telah Mati?", "id": "Bintang Katai Putih." },
  { "en": "Apa Nama Ibukota Negara Mali?", "id": "Bamako." },
  { "en": "Siapa Penulis Novel 'The Metamorphosis'?", "id": "Franz Kafka." },
  { "en": "Berapa Jumlah Muka Sisi Pada Sebuah Tetrahedon?", "id": "4 Muka." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Saab?", "id": "Swedia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Br?", "id": "Bromin." },
  { "en": "Siapakah Yang Menulis Buku 'The Leviathan'?", "id": "Thomas Hobbes." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Kopi?", "id": "Biji Kopi." },
  { "en": "Kapan Peringatan Hari Bahasa Rusia Sedunia?", "id": "6 Juni." },
  { "en": "Apa Nama Proses Penguraian Dengan Panas?", "id": "Dekomposisi Termal." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Inggris?", "id": "Ratu Elizabeth II." },
  { "en": "Apa Sebutan Untuk Pukulan Servis Rendah Dalam Badminton?", "id": "Servis Pendek." },
  { "en": "Apa Nama Mata Uang Resmi Negara Brasil?", "id": "Real Brasil." },
  { "en": "Dari Benua Manakah Asal Tanaman Cengkeh?", "id": "Asia." },
  { "en": "Apa Akronim Untuk Uni Eropa?", "id": "EU (European Union)." },
  { "en": "Apa Nama Tarian Tradisional Dari Aceh?", "id": "Tari Saman." },
  { "en": "Apa Nama Gaya Yang Melawan Arah Gerak Benda?", "id": "Gaya Gesek." }



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
