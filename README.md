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


  { "en": "Apa kepanjangan dari WIFI (Wireless Fidelity)?", "id": "Jaringan nirkabel koneksi internet." },
  { "en": "Apa fungsi utama dari WIFI (Wireless Fidelity)?", "id": "Menghubungkan perangkat ke internet nirkabel." },
  { "en": "Siapa yang menemukan teknologi WIFI (Wireless Fidelity)?", "id": "Hedy Lamarr dan George Antheil." },
  { "en": "Tahun berapa WIFI (Wireless Fidelity) pertama kali dirilis?", "id": "Tahun seribu sembilan ratus sembilan puluh tujuh." },
  { "en": "Apa nama lain dari WIFI (Wireless Fidelity)?", "id": "WLAN atau jaringan area lokal nirkabel." },
  { "en": "Apa itu SSID (Service Set Identifier)?", "id": "Nama unik dari sebuah jaringan WIFI." },
  { "en": "Bagaimana cara kerja dari WIFI (Wireless Fidelity)?", "id": "Menggunakan gelombang radio untuk transmisi data." },
  { "en": "Apa itu router WIFI (Wireless Fidelity)?", "id": "Perangkat keras pengirim sinyal WIFI." },
  { "en": "Apa perbedaan antara 2.4 GHz dan 5 GHz?", "id": "Frekuensi operasi sinyal WIFI berbeda." },
  { "en": "Apa itu enkripsi WPA2 (Wi-Fi Protected Access 2)?", "id": "Standar keamanan jaringan WIFI modern." },
  { "en": "Apa itu hotspot WIFI (Wireless Fidelity)?", "id": "Lokasi fisik penyedia akses WIFI." },
  { "en": "Bagaimana cara mengamankan jaringan WIFI (Wireless Fidelity)?", "id": "Gunakan kata sandi yang kuat." },
  { "en": "Apa itu alamat MAC (Media Access Control)?", "id": "Alamat unik identifikasi perangkat keras." },
  { "en": "Apa itu jangkauan sinyal WIFI (Wireless Fidelity)?", "id": "Area cakupan sinyal WIFI." },
  { "en": "Faktor apa yang mempengaruhi kecepatan WIFI (Wireless Fidelity)?", "id": "Jarak, halangan, dan jumlah pengguna." },
  { "en": "Apa itu mode pesawat pada perangkat?", "id": "Menonaktifkan semua komunikasi nirkabel." },
  { "en": "Apa itu repeater WIFI (Wireless Fidelity)?", "id": "Perangkat penguat dan perluas sinyal." },
  { "en": "Bisakah WIFI (Wireless Fidelity) menembus dinding?", "id": "Ya, tapi sinyalnya akan melemah." },
  { "en": "Apa itu lag dalam koneksi WIFI (Wireless Fidelity)?", "id": "Penundaan waktu dalam transmisi data." },
  { "en": "Apa itu bandwidth dalam konteks WIFI (Wireless Fidelity)?", "id": "Kapasitas transfer data maksimal." },
  { "en": "Apa itu guest network pada router?", "id": "Jaringan terpisah untuk tamu." },
  { "en": "Apa itu firewall pada router?", "id": "Sistem keamanan pelindung jaringan." },
  { "en": "Apa itu firmware pada router?", "id": "Perangkat lunak operasi internal router." },
  { "en": "Apa itu reboot router?", "id": "Memulai ulang perangkat router." },
  { "en": "Mengapa koneksi WIFI (Wireless Fidelity) saya lambat?", "id": "Banyak faktor, seperti gangguan sinyal." },
  { "en": "Apa itu channel WIFI (Wireless Fidelity)?", "id": "Saluran frekuensi untuk transmisi data." },
  { "en": "Bagaimana cara mengubah channel WIFI (Wireless Fidelity)?", "id": "Melalui pengaturan antarmuka web router." },
  { "en": "Apa itu kekuatan sinyal WIFI (Wireless Fidelity)?", "id": "Kekuatan sinyal WIFI yang diterima." },
  { "en": "Apa satuan untuk kekuatan sinyal WIFI (Wireless Fidelity)?", "id": "Decibel-milliwatts atau dBm." },
  { "en": "Apa itu noise dalam sinyal WIFI (Wireless Fidelity)?", "id": "Gangguan yang mengganggu sinyal WIFI." },
  { "en": "Apa itu SNR (Signal-to-Noise Ratio)?", "id": "Perbandingan kekuatan sinyal dan gangguan." },
  { "en": "Apa itu ping dalam jaringan?", "id": "Perintah untuk menguji konektivitas jaringan." },
  { "en": "Apa itu latency dalam jaringan?", "id": "Waktu tunda pengiriman data." },
  { "en": "Apa itu packet loss dalam jaringan?", "id": "Kegagalan paket data mencapai tujuan." },
  { "en": "Apa itu throughput dalam jaringan?", "id": "Tingkat transfer data aktual." },
  { "en": "Apa standar WIFI (Wireless Fidelity) yang paling umum?", "id": "Standar 802.11ac dan 802.11ax." },
  { "en": "Apa nama lain dari standar 802.11ax?", "id": "WIFI 6." },
  { "en": "Apa keunggulan utama dari WIFI (Wireless Fidelity) 6?", "id": "Kecepatan dan efisiensi lebih tinggi." },
  { "en": "Apa itu beamforming pada router?", "id": "Teknologi pemfokusan sinyal ke perangkat." },
  { "en": "Apa itu MU-MIMO (Multi-User, Multiple Input, Multiple Output)?", "id": "Router berkomunikasi dengan banyak perangkat." },
  { "en": "Apa itu QoS (Quality of Service)?", "id": "Fitur prioritas lalu lintas jaringan." },
  { "en": "Apa itu Parental Controls pada router?", "id": "Fitur pembatasan akses untuk anak." },
  { "en": "Apa itu MAC (Media Access Control) filtering?", "id": "Membatasi akses berdasarkan alamat MAC." },
  { "en": "Apa itu WPS (Wi-Fi Protected Setup)?", "id": "Metode penyambungan perangkat yang mudah." },
  { "en": "Apakah WPS (Wi-Fi Protected Setup) aman digunakan?", "id": "Kurang aman, rentan terhadap serangan." },
  { "en": "Apa itu rogue access point?", "id": "Titik akses tidak sah." },
  { "en": "Apa itu wardriving?", "id": "Mencari jaringan WIFI dari kendaraan." },
  { "en": "Apa itu piggybacking dalam konteks WIFI (Wireless Fidelity)?", "id": "Menggunakan jaringan WIFI tanpa izin." },
  { "en": "Apa itu man-in-the-middle attack?", "id": "Serangan penyadapan komunikasi jaringan." },
  { "en": "Apa itu VPN (Virtual Private Network)?", "id": "Mengamankan koneksi internet Anda." },
  { "en": "Bagaimana cara kerja VPN (Virtual Private Network)?", "id": "Mengenkripsi lalu lintas internet Anda." },
  { "en": "Apa itu DNS (Domain Name System)?", "id": "Sistem penerjemah nama domain." },
  { "en": "Apa itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Protokol pemberi alamat IP otomatis." },
  { "en": "Apa itu alamat IP (Internet Protocol)?", "id": "Alamat unik identifikasi perangkat." },
  { "en": "Apa itu subnet mask?", "id": "Membagi jaringan menjadi beberapa subjaringan." },
  { "en": "Apa itu default gateway?", "id": "Router yang menghubungkan jaringan lokal." },
  { "en": "Apa itu port forwarding?", "id": "Mengarahkan lalu lintas ke perangkat." },
  { "en": "Apa itu UPnP (Universal Plug and Play)?", "id": "Protokol penemuan perangkat jaringan mudah." },
  { "en": "Apa itu NAT (Network Address Translation)?", "id": "Menyembunyikan alamat IP pribadi." },
  { "en": "Apa itu DMZ (Demilitarized Zone) pada router?", "id": "Subjaringan terisolasi untuk server." },
  { "en": "Apa itu factory reset pada router?", "id": "Mengembalikan router ke pengaturan pabrik." },
  { "en": "Bagaimana cara melakukan factory reset?", "id": "Tekan tombol reset pada router." },
  { "en": "Apa itu web interface router?", "id": "Halaman web untuk konfigurasi router." },
  { "en": "Apa alamat IP (Internet Protocol) default untuk router?", "id": "Biasanya 192.168.1.1 atau 192.168.0.1." },
  { "en": "Apa username dan password default router?", "id": "Biasanya 'admin' untuk keduanya." },
  { "en": "Apa itu captive portal?", "id": "Halaman login untuk akses WIFI publik." },
  { "en": "Apa itu WISP (Wireless Internet Service Provider)?", "id": "Penyedia layanan internet nirkabel." },
  { "en": "Apa itu sistem mesh WIFI (Wireless Fidelity)?", "id": "Sistem beberapa unit untuk cakupan luas." },
  { "en": "Apa keunggulan sistem mesh WIFI (Wireless Fidelity)?", "id": "Cakupan sinyal yang lebih baik." },
  { "en": "Apa itu powerline adapter?", "id": "Menggunakan kabel listrik untuk jaringan." },
  { "en": "Apa itu aplikasi WIFI (Wireless Fidelity) analyzer?", "id": "Aplikasi untuk menganalisis jaringan WIFI." },
  { "en": "Apa fungsi aplikasi WIFI (Wireless Fidelity) analyzer?", "id": "Membantu menemukan channel terbaik." },
  { "en": "Apa itu heat map WIFI (Wireless Fidelity)?", "id": "Peta visual kekuatan sinyal WIFI." },
  { "en": "Apa itu site survey WIFI (Wireless Fidelity)?", "id": "Proses perencanaan dan desain jaringan." },
  { "en": "Apa itu fat access point?", "id": "Titik akses yang dikelola mandiri." },
  { "en": "Apa itu thin access point?", "id": "Titik akses yang dikelola terpusat." },
  { "en": "Apa itu wireless controller?", "id": "Perangkat untuk mengelola thin AP." },
  { "en": "Apa itu roaming dalam WIFI (Wireless Fidelity)?", "id": "Perpindahan perangkat antar titik akses." },
  { "en": "Apa itu seamless roaming?", "id": "Roaming tanpa putus koneksi." },
  { "en": "Apa itu band steering?", "id": "Mengarahkan perangkat ke band terbaik." },
  { "en": "Apa itu airtime fairness?", "id": "Memberikan waktu tayang yang sama." },
  { "en": "Apa itu WDS (Wireless Distribution System)?", "id": "Menghubungkan titik akses secara nirkabel." },
  { "en": "Apa itu bridge mode pada router?", "id": "Menonaktifkan fungsi routing router." },
  { "en": "Apa itu access point mode?", "id": "Mengubah router menjadi titik akses." },
  { "en": "Apa itu client mode pada perangkat WIFI (Wireless Fidelity)?", "id": "Menghubungkan perangkat ke jaringan." },
  { "en": "Apa itu ad-hoc mode dalam WIFI (Wireless Fidelity)?", "id": "Jaringan peer-to-peer tanpa router." },
  { "en": "Apa itu infrastructure mode dalam WIFI (Wireless Fidelity)?", "id": "Jaringan dengan titik akses terpusat." },
  { "en": "Apa itu Wi-Fi Direct?", "id": "Menghubungkan perangkat tanpa router." },
  { "en": "Apa itu Miracast?", "id": "Standar untuk menampilkan layar nirkabel." },
  { "en": "Apa itu Chromecast?", "id": "Perangkat streaming media dari Google." },
  { "en": "Apa itu AirPlay?", "id": "Protokol streaming nirkabel dari Apple." },
  { "en": "Apa itu DLNA (Digital Living Network Alliance)?", "id": "Standar untuk berbagi media." },
  { "en": "Apa itu IoT (Internet of Things)?", "id": "Jaringan perangkat yang terhubung internet." },
  { "en": "Bagaimana IoT (Internet of Things) menggunakan WIFI?", "id": "Untuk menghubungkan perangkat ke internet." },
  { "en": "Apa itu smart home?", "id": "Rumah dengan perangkat yang terhubung." },
  { "en": "Apa itu wearable device?", "id": "Perangkat elektronik yang dapat dikenakan." },
  { "en": "Apa itu VoWIFI (Voice over Wi-Fi)?", "id": "Melakukan panggilan suara melalui WIFI." },
  { "en": "Apa keuntungan VoWIFI (Voice over Wi-Fi)?", "id": "Panggilan jernih di sinyal lemah." },
  { "en": "Apa itu Li-Fi (Light Fidelity)?", "id": "Komunikasi nirkabel menggunakan cahaya." },
  { "en": "Apa fungsi dari Wi-Fi Alliance?", "id": "Memberi sertifikasi pada produk WIFI." },
  { "en": "Apa itu IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi yang membuat standar WIFI." },
  { "en": "Apa itu enkripsi WEP (Wired Equivalent Privacy)?", "id": "Standar keamanan WIFI yang sudah usang." },
  { "en": "Apakah enkripsi WEP (Wired Equivalent Privacy) aman?", "id": "Tidak, sangat tidak aman." },
  { "en": "Apa itu WPA (Wi-Fi Protected Access)?", "id": "Peningkatan keamanan dari enkripsi WEP." },
  { "en": "Apa itu WPA3 (Wi-Fi Protected Access 3)?", "id": "Standar keamanan WIFI paling modern." },
  { "en": "Apa itu serangan Evil Twin?", "id": "Titik akses palsu untuk mencuri data." },
  { "en": "Apa itu OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Teknologi efisiensi data di WIFI 6." },
  { "en": "Apa itu TWT (Target Wake Time)?", "id": "Menghemat baterai perangkat IoT." },
  { "en": "Apa itu lebar saluran (channel width)?", "id": "Lebar pita frekuensi transmisi data." },
  { "en": "Apa itu DFS (Dynamic Frequency Selection)?", "id": "Memilih saluran yang sepi secara otomatis." },
  { "en": "Apa arti 'terhubung, tanpa internet'?", "id": "Koneksi ke router berhasil, internet gagal." },
  { "en": "Apa itu konflik alamat IP (Internet Protocol)?", "id": "Dua perangkat memiliki alamat IP sama." },
  { "en": "Apa itu adaptor WIFI USB (Universal Serial Bus)?", "id": "Menambahkan kemampuan WIFI via port USB." },
  { "en": "Apa nama standar IEEE (Institute of Electrical and Electronics Engineers) untuk WIFI?", "id": "Standar IEEE 802.11." },
  { "en": "Apa nama lain dari standar 802.11n?", "id": "WIFI 4." },
  { "en": "Apa nama lain dari standar 802.11ac?", "id": "WIFI 5." },
  { "en": "Apa nama lain dari standar 802.11be?", "id": "WIFI 7." },
  { "en": "Apa sumber interferensi WIFI (Wireless Fidelity) yang umum?", "id": "Microwave, Bluetooth, dan telepon nirkabel." },
  { "en": "Apa itu antena omnidirectional?", "id": "Menyebarkan sinyal ke segala arah." },
  { "en": "Apa itu antena directional?", "id": "Memfokuskan sinyal ke satu arah." },
  { "en": "Apa itu WMM (Wi-Fi Multimedia)?", "id": "Memprioritaskan lalu lintas audio dan video." },
  { "en": "Apa itu Wi-Fi Passpoint?", "id": "Menyambung ke hotspot publik otomatis." },
  { "en": "Apakah radiasi WIFI (Wireless Fidelity) berbahaya?", "id": "Tidak ada bukti ilmiah yang kuat." },
  { "en": "Apa beda WIFI (Wireless Fidelity) dan Ethernet?", "id": "WIFI nirkabel, Ethernet menggunakan kabel." },
  { "en": "Apa beda WIFI (Wireless Fidelity) dan Bluetooth?", "id": "Jangkauan dan kecepatan WIFI lebih besar." },
  { "en": "Apa beda WIFI (Wireless Fidelity) dan data seluler?", "id": "WIFI untuk lokal, seluler lebih luas." },
  { "en": "Apa itu TKIP (Temporal Key Integrity Protocol)?", "id": "Protokol enkripsi yang digunakan WPA." },
  { "en": "Apa itu enkripsi AES (Advanced Encryption Standard)?", "id": "Protokol enkripsi yang digunakan WPA2." },
  { "en": "Standar WIFI (Wireless Fidelity) mana yang dikenal sebagai 'Wi-Fi G'?", "id": "Standar 802.11g." },
  { "en": "Standar WIFI (Wireless Fidelity) mana yang dikenal sebagai 'Wi-Fi B'?", "id": "Standar 802.11b." },
  { "en": "Apa itu serangan deauthentication?", "id": "Memutuskan paksa koneksi perangkat." },
  { "en": "Apa itu serangan KRACK (Key Reinstallation Attack)?", "id": "Kerentanan pada protokol keamanan WPA2." },
  { "en": "Apa itu band UNII (Unlicensed National Information Infrastructure)?", "id": "Blok spektrum untuk penggunaan WIFI." },
  { "en": "Apa itu EIRP (Effective Isotropic Radiated Power)?", "id": "Ukuran daya yang dipancarkan antena." },
  { "en": "Bisakah cuaca mempengaruhi sinyal WIFI (Wireless Fidelity)?", "id": "Ya, terutama kelembaban dan hujan." },
  { "en": "Apa itu sangkar Faraday?", "id": "Wadah yang memblokir sinyal elektromagnetik." },
  { "en": "Apa itu dead zone WIFI (Wireless Fidelity)?", "id": "Area tanpa jangkauan sinyal WIFI." },
  { "en": "Apa itu RTS/CTS (Request to Send/Clear to Send)?", "id": "Mekanisme untuk menghindari tabrakan data." },
  { "en": "Apa itu beacon frame?", "id": "Sinyal identitas dari sebuah router." },
  { "en": "Apa itu MIMO (Multiple Input, Multiple Output)?", "id": "Menggunakan banyak antena untuk kecepatan." },
  { "en": "Bagaimana MIMO (Multiple Input, Multiple Output) meningkatkan kinerja?", "id": "Mengirim dan menerima banyak data." },
  { "en": "Apa itu channel bonding?", "id": "Menggabungkan dua saluran menjadi satu." },
  { "en": "Berapa nilai dBm yang baik?", "id": "Antara -30 hingga -67 dBm." },
  { "en": "Apa arti sinyal -90 dBm?", "id": "Sinyal sangat lemah dan tidak stabil." },
  { "en": "Apa itu kartu WIFI PCI-e (Peripheral Component Interconnect Express)?", "id": "Kartu ekspansi WIFI untuk komputer." },
  { "en": "Apa itu wireless bridge?", "id": "Menghubungkan dua jaringan kabel nirkabel." },
  { "en": "Apa itu wireless client?", "id": "Perangkat yang terhubung ke WIFI." },
  { "en": "Apa artinya 'lupakan jaringan'?", "id": "Menghapus profil jaringan WIFI tersimpan." },
  { "en": "Apa itu jaringan WIFI (Wireless Fidelity) terbuka?", "id": "Jaringan tanpa kata sandi." },
  { "en": "Mengapa jaringan terbuka berisiko?", "id": "Data tidak dienkripsi, mudah disadap." },
  { "en": "Apa itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi aman dari protokol web HTTP." },
  { "en": "Bagaimana HTTPS (Hypertext Transfer Protocol Secure) membantu di WIFI publik?", "id": "Mengenkripsi data antara Anda dan situs." },
  { "en": "Apa itu BSSID (Basic Service Set Identifier)?", "id": "Alamat MAC dari sebuah access point." },
  { "en": "Apa beda SSID dan BSSID?", "id": "SSID nama jaringan, BSSID alamat AP." },
  { "en": "Apa itu ESSID (Extended Service Set Identifier)?", "id": "Nama untuk jaringan dengan banyak AP." },
  { "en": "Apa itu PSK (Pre-Shared Key)?", "id": "Kata sandi yang dibagikan bersama." },
  { "en": "Apa nama lain dari mode PSK (Pre-Shared Key)?", "id": "Mode personal atau WPA2-Personal." },
  { "en": "Apa itu keamanan WIFI (Wireless Fidelity) mode Enterprise?", "id": "Setiap pengguna punya login sendiri." },
  { "en": "Apa itu server RADIUS (Remote Authentication Dial-In User Service)?", "id": "Server untuk otentikasi mode Enterprise." },
  { "en": "Apa itu spatial streaming?", "id": "Aliran data simultan melalui MIMO." },
  { "en": "Berapa banyak spatial stream yang didukung WIFI 6?", "id": "Hingga delapan aliran spasial." },
  { "en": "Apa itu BSS (Basic Service Set) Coloring?", "id": "Mekanisme untuk mengurangi interferensi." },
  { "en": "Masalah apa yang dipecahkan BSS (Basic Service Set) Coloring?", "id": "Interferensi co-channel di lingkungan padat." },
  { "en": "Berapa kecepatan maksimum 802.11b?", "id": "Sebelas megabit per detik." },
  { "en": "Berapa frekuensi operasi 802.11b?", "id": "Dua koma empat gigahertz." },
  { "en": "Apa itu CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)?", "id": "Metode akses media nirkabel." },
  { "en": "Apa itu data rate?", "id": "Kecepatan transfer data teoritis." },
  { "en": "Apa itu guard intervals?", "id": "Waktu jeda antar transmisi data." },
  { "en": "Apa fungsi guard interval?", "id": "Mencegah interferensi antar simbol." },
  { "en": "Apa itu short guard interval?", "id": "Mengurangi jeda untuk meningkatkan kecepatan." },
  { "en": "Apakah logam mengganggu sinyal WIFI (Wireless Fidelity)?", "id": "Ya, secara signifikan memantulkan sinyal." },
  { "en": "Apakah air mengganggu sinyal WIFI (Wireless Fidelity)?", "id": "Ya, air menyerap sinyal radio." },
  { "en": "Di mana lokasi terbaik untuk router?", "id": "Di tengah rumah, di tempat tinggi." },
  { "en": "Mengapa router harus diletakkan di tempat tinggi?", "id": "Agar sinyal menyebar ke bawah." },
  { "en": "Apa itu dongle WIFI (Wireless Fidelity)?", "id": "Nama lain untuk adaptor WIFI USB." },
  { "en": "Apa itu metode tombol tekan WPS (Wi-Fi Protected Setup)?", "id": "Menyambung dengan menekan tombol." },
  { "en": "Apa itu metode PIN WPS (Wi-Fi Protected Setup)?", "id": "Menyambung dengan memasukkan kode PIN." },
  { "en": "Metode WPS (Wi-Fi Protected Setup) mana yang lebih rentan?", "id": "Metode PIN lebih rentan." },
  { "en": "Apa itu SSID (Service Set Identifier) tersembunyi?", "id": "Jaringan yang namanya tidak disiarkan." },
  { "en": "Apakah menyembunyikan SSID (Service Set Identifier) itu efektif?", "id": "Tidak, itu bukan fitur keamanan." },
  { "en": "Bagaimana perangkat menemukan SSID (Service Set Identifier) tersembunyi?", "id": "Dengan mengirimkan probe request." },
  { "en": "Apa itu probe request frame?", "id": "Paket untuk menanyakan jaringan tersedia." },
  { "en": "Apa itu band 6 GHz untuk WIFI (Wireless Fidelity)?", "id": "Frekuensi baru untuk mengurangi kepadatan." },
  { "en": "Standar WIFI (Wireless Fidelity) mana yang memperkenalkan band 6 GHz?", "id": "WIFI 6E." },
  { "en": "Apa itu Wi-Fi EasyMesh?", "id": "Standar untuk sistem mesh interoperable." },
  { "en": "Apa itu backhaul dalam sistem mesh?", "id": "Koneksi data antar node mesh." },
  { "en": "Apa itu dedicated wireless backhaul?", "id": "Band frekuensi khusus untuk backhaul." },
  { "en": "Apa itu Ethernet backhaul?", "id": "Menggunakan kabel Ethernet untuk backhaul." },
  { "en": "Bisakah extender dan mesh digabungkan?", "id": "Tidak disarankan, dapat mengurangi kinerja." },
  { "en": "Apa itu 1024-QAM (Quadrature Amplitude Modulation)?", "id": "Skema modulasi untuk transfer data." },
  { "en": "Bagaimana 1024-QAM (Quadrature Amplitude Modulation) meningkatkan kecepatan?", "id": "Mengemas lebih banyak data per sinyal." },
  { "en": "Apa itu AP (Access Point)?", "id": "Perangkat yang membuat jaringan nirkabel." },
  { "en": "Apa itu LAN (Local Area Network) nirkabel?", "id": "Jaringan lokal yang menggunakan WIFI." },
  { "en": "Apa itu WLAN (Wireless Local Area Network) controller?", "id": "Mengelola banyak AP secara terpusat." },
  { "en": "Apa itu CPE (Customer Premises Equipment)?", "id": "Perangkat jaringan di lokasi pelanggan." },
  { "en": "Apa itu mobile hotspot?", "id": "Berbagi koneksi seluler melalui WIFI." },
  { "en": "Apakah mobile hotspot menguras baterai?", "id": "Ya, sangat menguras baterai ponsel." },
  { "en": "Apa itu RSSI (Received Signal Strength Indicator)?", "id": "Indikator kekuatan sinyal yang diterima." },
  { "en": "Apa yang diukur oleh RSSI (Received Signal Strength Indicator)?", "id": "Kekuatan energi sinyal radio." },
  { "en": "Apa itu gain antena?", "id": "Kemampuan antena memfokuskan sinyal." },
  { "en": "Apa itu dBi (decibels relative to isotropic)?", "id": "Satuan untuk mengukur gain antena." },
  { "en": "Apa itu polarisasi antena?", "id": "Orientasi fisik dari antena." },
  { "en": "Apa itu atenuasi dalam RF (Radio Frequency)?", "id": "Pelemahan sinyal saat melewati materi." },
  { "en": "Apa itu refleksi dalam RF (Radio Frequency)?", "id": "Sinyal memantul dari permukaan objek." },
  { "en": "Apa itu refraksi dalam RF (Radio Frequency)?", "id": "Pembelokan sinyal saat ganti medium." },
  { "en": "Apa itu difraksi dalam RF (Radio Frequency)?", "id": "Sinyal menyebar di sekitar penghalang." },
  { "en": "Apa itu absorpsi dalam RF (Radio Frequency)?", "id": "Materi menyerap energi sinyal." },
  { "en": "Apa itu PoE (Power over Ethernet)?", "id": "Memberi daya listrik melalui kabel Ethernet." },
  { "en": "Apa standar IEEE (Institute of Electrical and Electronics Engineers) untuk PoE (Power over Ethernet)?", "id": "Standar 802.3af dan 802.3at." },
  { "en": "Apa kegunaan PoE (Power over Ethernet) untuk AP (Access Point)?", "id": "Satu kabel untuk data dan daya." },
  { "en": "Apa itu VLAN (Virtual Local Area Network)?", "id": "Memisahkan jaringan secara logis." },
  { "en": "Bisakah VLAN (Virtual Local Area Network) digunakan dengan WIFI (Wireless Fidelity)?", "id": "Ya, untuk memisahkan SSID." },
  { "en": "Apa itu jitter jaringan?", "id": "Variasi penundaan penerimaan paket data." },
  { "en": "Apakah WIFI (Wireless Fidelity) half-duplex atau full-duplex?", "id": "WIFI adalah half-duplex." },
  { "en": "Apa itu management frames di WIFI (Wireless Fidelity)?", "id": "Mengelola koneksi dan sesi nirkabel." },
  { "en": "Apa itu control frames di WIFI (Wireless Fidelity)?", "id": "Membantu pengiriman data frames." },
  { "en": "Apa itu data frames di WIFI (Wireless Fidelity)?", "id": "Membawa data aktual dari pengguna." },
  { "en": "Apa itu EAP (Extensible Authentication Protocol)?", "id": "Kerangka kerja untuk metode otentikasi." },
  { "en": "Apa itu PEAP (Protected Extensible Authentication Protocol)?", "id": "Salah satu jenis metode otentikasi EAP." },
  { "en": "Apa itu EAP-TLS (Extensible Authentication Protocol-Transport Layer Security)?", "id": "Metode EAP paling aman." },
  { "en": "Apa itu SNMP (Simple Network Management Protocol)?", "id": "Protokol untuk memonitor perangkat jaringan." },
  { "en": "Apa itu spectrum analyzer?", "id": "Alat untuk memvisualisasikan sinyal RF." },
  { "en": "Apa itu free space path loss?", "id": "Pelemahan sinyal di ruang terbuka." },
  { "en": "Apa itu tautan nirkabel P2P (Point-to-Point)?", "id": "Koneksi nirkabel antara dua titik." },
  { "en": "Apa itu tautan nirkabel P2MP (Point-to-Multipoint)?", "id": "Satu titik terhubung ke banyak titik." },
  { "en": "Bagaimana cara kerja WIFI (Wireless Fidelity) di pesawat?", "id": "Menggunakan sinyal dari darat atau satelit." },
  { "en": "Apa penyebab umum jitter yang tinggi?", "id": "Kepadatan jaringan atau interferensi." },
  { "en": "Apa itu IGMP (Internet Group Management Protocol) Snooping?", "id": "Mengelola lalu lintas multicast di jaringan." },
  { "en": "Berapa kecepatan data maksimum 802.11g?", "id": "Lima puluh empat megabit per detik." },
  { "en": "Berapa kecepatan data maksimum 802.11a?", "id": "Lima puluh empat megabit per detik." },
  { "en": "Apa kelemahan utama band 2.4 GHz?", "id": "Penuh sesak dan banyak interferensi." },
  { "en": "Apa itu antenna diversity?", "id": "Menggunakan dua antena untuk sinyal terbaik." },
  { "en": "Apa itu kabel pigtail antena?", "id": "Kabel adaptor untuk menghubungkan antena." },
  { "en": "Apa itu RF (Radio Frequency) shield?", "id": "Bahan untuk memblokir interferensi RF." },
  { "en": "Apa itu ISP (Internet Service Provider) nirkabel?", "id": "Menyediakan internet melalui koneksi nirkabel." },
  { "en": "Apa itu FCC (Federal Communications Commission)?", "id": "Badan regulator komunikasi di AS." },
  { "en": "Apa peran FCC (Federal Communications Commission) dalam WIFI (Wireless Fidelity)?", "id": "Mengatur penggunaan frekuensi radio." },
  { "en": "Apa itu waktu tunggu DFS (Dynamic Frequency Selection)?", "id": "Jeda untuk memindai radar cuaca." },
  { "en": "Apa itu server syslog?", "id": "Server untuk mengumpulkan log perangkat." },
  { "en": "Apa itu pemeta 'dead spot' WIFI (Wireless Fidelity)?", "id": "Aplikasi untuk menemukan area sinyal lemah." },
  { "en": "Apa itu geofencing dengan WIFI (Wireless Fidelity)?", "id": "Memicu aksi berdasarkan lokasi perangkat." },
  { "en": "Bagaimana smart plug menggunakan WIFI (Wireless Fidelity)?", "id": "Terhubung ke jaringan untuk kontrol." },
  { "en": "Apa itu bohlam pintar berkemampuan WIFI (Wireless Fidelity)?", "id": "Bohlam yang dikontrol melalui WIFI." },
  { "en": "Apa beda WPA-Personal dan WPA-Enterprise?", "id": "Personal pakai PSK, Enterprise pakai server." },
  { "en": "Apa itu RTS (Request to Send) threshold?", "id": "Ukuran paket untuk memulai mekanisme RTS." },
  { "en": "Apa itu fragmentation threshold?", "id": "Ukuran paket sebelum dipecah." },
  { "en": "Apa itu periode DTIM (Delivery Traffic Indication Message)?", "id": "Interval perangkat 'bangun' untuk data." },
  { "en": "Apa yang dipengaruhi oleh DTIM (Delivery Traffic Indication Message)?", "id": "Keseimbangan antara kinerja dan daya tahan baterai." },
  { "en": "Apakah AirDrop menggunakan WIFI (Wireless Fidelity)?", "id": "Ya, menggunakan WIFI Direct dan Bluetooth." },
  { "en": "Apa itu survei nirkabel?", "id": "Proses analisis dan pemetaan sinyal." },
  { "en": "Apa itu survei nirkabel prediktif?", "id": "Simulasi cakupan sinyal dengan perangkat lunak." },
  { "en": "Apa itu survei nirkabel pasif?", "id": "Mendengarkan lalu lintas WIFI yang ada." },
  { "en": "Apa itu survei nirkabel aktif?", "id": "Mengukur kinerja dengan terhubung ke AP." },
  { "en": "Apa itu masalah hidden node?", "id": "Dua perangkat tidak bisa 'melihat' satu sama lain." },
  { "en": "Apa itu masalah exposed node?", "id": "Perangkat salah mengira media sibuk." },
  { "en": "Apa itu channel hopping DFS (Dynamic Frequency Selection)?", "id": "AP pindah channel jika ada radar." },
  { "en": "Apa itu contention window di WIFI (Wireless Fidelity)?", "id": "Periode tunggu acak sebelum transmisi." },
  { "en": "Apa itu interframe space?", "id": "Waktu jeda antar transmisi frame." },
  { "en": "Apa itu SIFS (Short Interframe Space)?", "id": "Jeda waktu terpendek antar frame." },
  { "en": "Apa itu DIFS (DCF Interframe Space)?", "id": "Jeda waktu tunggu sebelum transmisi." },
  { "en": "Apa itu association request frame?", "id": "Frame permintaan perangkat untuk terhubung." },
  { "en": "Apa itu authentication frame?", "id": "Frame untuk proses otentikasi." },
  { "en": "Apa itu probe response frame?", "id": "Jawaban AP terhadap probe request." },
  { "en": "Apa itu mode hemat daya di WIFI (Wireless Fidelity)?", "id": "Perangkat 'tidur' untuk menghemat daya." },
  { "en": "Apa itu TIM (Traffic Indication Map)?", "id": "Peta data yang menunggu perangkat." },
  { "en": "Apa itu resource unit OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Alokasi sub-saluran untuk perangkat." },
  { "en": "Apa itu alamat APIPA (Automatic Private IP Addressing)?", "id": "Alamat IP sementara jika DHCP gagal." },
  { "en": "Kapan perangkat mendapatkan alamat APIPA (Automatic Private IP Addressing)?", "id": "Saat tidak bisa menemukan server DHCP." },
  { "en": "Apa itu waktu sewa alamat IP (Internet Protocol)?", "id": "Durasi sebuah perangkat menggunakan IP." },
  { "en": "Apa itu reservasi DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberi alamat IP tetap ke perangkat." },
  { "en": "Mengapa menggunakan reservasi DHCP (Dynamic Host Configuration Protocol)?", "id": "Memastikan perangkat selalu dapat IP sama." },
  { "en": "Apa itu profil nirkabel?", "id": "Pengaturan jaringan yang disimpan." },
  { "en": "Bisakah Anda memprioritaskan jaringan WIFI (Wireless Fidelity)?", "id": "Ya, di sebagian besar sistem operasi." },
  { "en": "Apa itu band ISM (Industrial, Scientific, and Medical)?", "id": "Pita frekuensi bebas lisensi." },
  { "en": "Band WIFI (Wireless Fidelity) mana yang ada di band ISM (Industrial, Scientific, and Medical)?", "id": "Band 2.4 GHz." },
  { "en": "Apa itu kontrol daya TX (Transmit)?", "id": "Menyesuaikan kekuatan sinyal pancar router." },
  { "en": "Mengapa menurunkan daya TX (Transmit) router?", "id": "Mengurangi interferensi dengan jaringan tetangga." },
  { "en": "Apa itu fitur client isolation?", "id": "Mencegah perangkat di jaringan berkomunikasi." },
  { "en": "Di mana client isolation biasa digunakan?", "id": "Di jaringan WIFI publik atau tamu." },
  { "en": "Apa itu WPA2-PSK (AES)?", "id": "Mode personal WPA2 dengan enkripsi AES." },
  { "en": "Apa itu WPA2-PSK (TKIP)?", "id": "Mode personal WPA2 dengan enkripsi TKIP." },
  { "en": "Enkripsi WPA2 mana yang lebih baik?", "id": "AES lebih aman dan lebih cepat." },
  { "en": "Apa itu IBSS (Independent Basic Service Set)?", "id": "Jaringan ad-hoc tanpa access point." },
  { "en": "Apa nama lain dari IBSS (Independent Basic Service Set)?", "id": "Mode ad-hoc." },
  { "en": "Apa itu pre-authentication di WIFI (Wireless Fidelity)?", "id": "Otentikasi sebelum terhubung ke AP baru." },
  { "en": "Apa itu fast roaming (802.11r)?", "id": "Mempercepat proses roaming antar AP." },
  { "en": "Apa itu CCMP (Counter Mode Cipher Block Chaining Message Authentication Code Protocol)?", "id": "Protokol enkripsi standar untuk WPA2." },
  { "en": "Standar keamanan mana yang menggunakan CCMP (Counter Mode Cipher Block Chaining Message Authentication Code Protocol)?", "id": "Standar keamanan WPA2 dan WPA3." },
  { "en": "Apa itu laporan survei situs nirkabel?", "id": "Dokumen hasil analisis jaringan nirkabel." },
  { "en": "Apa itu propagasi RF (Radio Frequency)?", "id": "Cara sinyal radio bergerak di udara." },
  { "en": "Apa itu MLO (Multi-Link Operation)?", "id": "Menggunakan banyak band secara simultan." },
  { "en": "Standar WIFI (Wireless Fidelity) mana yang memperkenalkan MLO (Multi-Link Operation)?", "id": "WIFI 7 (802.11be)." },
  { "en": "Apa itu AFC (Automated Frequency Coordination)?", "id": "Sistem pencegah interferensi di band 6GHz." },
  { "en": "Apa itu SAE (Simultaneous Authentication of Equals)?", "id": "Metode handshake aman pengganti PSK." },
  { "en": "Protokol keamanan mana yang menggunakan SAE (Simultaneous Authentication of Equals)?", "id": "WPA3 (Wi-Fi Protected Access 3)." },
  { "en": "Apa itu OWE (Opportunistic Wireless Encryption)?", "id": "Memberi enkripsi otomatis di jaringan terbuka." },
  { "en": "Apa itu PMF (Protected Management Frames)?", "id": "Melindungi frame manajemen dari penyadapan." },
  { "en": "Apa standar untuk PMF (Protected Management Frames)?", "id": "Standar IEEE 802.11w." },
  { "en": "PMF (Protected Management Frames) melindungi dari serangan apa?", "id": "Serangan deauthentication dan disassociation." },
  { "en": "Apa itu standar IEEE (Institute of Electrical and Electronics Engineers) 802.11k?", "id": "Standar untuk Radio Resource Measurement." },
  { "en": "Apa itu neighbor report dalam 802.11k?", "id": "Daftar AP terdekat untuk roaming." },
  { "en": "Apa itu standar IEEE (Institute of Electrical and Electronics Engineers) 802.11v?", "id": "Standar untuk BSS Transition Management." },
  { "en": "Apa itu WIPS (Wireless Intrusion Prevention System)?", "id": "Sistem pencegah intrusi jaringan nirkabel." },
  { "en": "Apa itu RF (Radio Frequency) fingerprinting?", "id": "Mengidentifikasi perangkat dari sinyal uniknya." },
  { "en": "Apa itu preamble dalam frame WIFI (Wireless Fidelity)?", "id": "Bagian awal frame untuk sinkronisasi." },
  { "en": "Apa itu cyclic prefix?", "id": "Bagian dari guard interval." },
  { "en": "Apa itu LDPC (Low-Density Parity-Check)?", "id": "Kode koreksi kesalahan yang efisien." },
  { "en": "Apa itu antena sektor?", "id": "Antena yang mencakup area tertentu." },
  { "en": "Apa itu antena patch?", "id": "Antena datar yang dipasang di permukaan." },
  { "en": "Siapa group owner di Wi-Fi Direct?", "id": "Perangkat yang bertindak sebagai AP." },
  { "en": "Apa itu modem DSL (Digital Subscriber Line)?", "id": "Modem yang menggunakan jalur telepon." },
  { "en": "Apa itu modem kabel?", "id": "Modem yang menggunakan kabel koaksial TV." },
  { "en": "Apa itu ONT (Optical Network Terminal)?", "id": "Modem untuk koneksi internet fiber." },
  { "en": "Apa itu iPerf?", "id": "Alat untuk mengukur throughput jaringan." },
  { "en": "Apa itu Wireshark?", "id": "Alat penganalisis protokol jaringan." },
  { "en": "Bisakah Wireshark menangkap lalu lintas WIFI (Wireless Fidelity)?", "id": "Ya, dengan kartu nirkabel yang tepat." },
  { "en": "Apa itu MCS (Modulation and Coding Scheme) Index?", "id": "Tabel yang menentukan kecepatan data." },
  { "en": "Apa arti MCS (Modulation and Coding Scheme) Index yang lebih tinggi?", "id": "Kecepatan data potensial lebih tinggi." },
  { "en": "Apa itu codec suara di VoWIFI (Voice over Wi-Fi)?", "id": "Algoritma kompresi dan dekompresi suara." },
  { "en": "Apa itu handover panggilan WIFI (Wireless Fidelity)?", "id": "Perpindahan panggilan antara WIFI dan seluler." },
  { "en": "Dari mana asal nama 'Wi-Fi'?", "id": "Istilah pemasaran, bukan singkatan." },
  { "en": "Apa arti logo yin-yang Wi-Fi Alliance?", "id": "Melambangkan interoperabilitas antar perangkat." },
  { "en": "Apa itu NFC (Near Field Communication)?", "id": "Komunikasi nirkabel jarak sangat dekat." },
  { "en": "Bagaimana NFC (Near Field Communication) bisa digunakan untuk WIFI (Wireless Fidelity)?", "id": "Menyambungkan ke WIFI dengan satu sentuhan." },
  { "en": "Bagaimana lokasi WIFI (Wireless Fidelity) dilacak?", "id": "Melalui triangulasi sinyal SSID." },
  { "en": "Apa itu WIFI (Wireless Fidelity) HaLow?", "id": "WIFI jarak jauh berdaya rendah." },
  { "en": "Apa standar untuk WIFI (Wireless Fidelity) HaLow?", "id": "Standar IEEE 802.11ah." },
  { "en": "Apa keunggulan utama WIFI (Wireless Fidelity) HaLow?", "id": "Jangkauan jauh dan hemat daya." },
  { "en": "Apa itu standar IEEE (Institute of Electrical and Electronics Engineers) 802.1X?", "id": "Standar untuk otentikasi akses jaringan." },
  { "en": "Apa itu supplicant dalam 802.1X?", "id": "Perangkat klien yang meminta akses." },
  { "en": "Apa itu authenticator dalam 802.1X?", "id": "AP atau switch tempat supplicant terhubung." },
  { "en": "Apa itu 4K-QAM (Quadrature Amplitude Modulation)?", "id": "Skema modulasi dengan kepadatan lebih tinggi." },
  { "en": "Standar mana yang memperkenalkan 4K-QAM (Quadrature Amplitude Modulation)?", "id": "Standar WIFI 7 (802.11be)." },
  { "en": "Apa itu TxBF (Transmit Beamforming)?", "id": "Teknik memfokuskan energi sinyal." },
  { "en": "Apa itu HT (High Throughput)?", "id": "Istilah untuk standar 802.11n." },
  { "en": "Apa itu VHT (Very High Throughput)?", "id": "Istilah untuk standar 802.11ac." },
  { "en": "Apa itu Greenfield Preamble?", "id": "Format preamble paling efisien di 802.11n." },
  { "en": "Apa itu STBC (Space-Time Block Coding)?", "id": "Teknik untuk meningkatkan keandalan sinyal." },
  { "en": "Apa fungsi STBC (Space-Time Block Coding)?", "id": "Mengirim data sama lewat banyak antena." },
  { "en": "Apa itu A-MSDU (Aggregate MAC Service Data Unit)?", "id": "Metode agregasi frame lapisan atas." },
  { "en": "Apa itu A-MPDU (Aggregate MAC Protocol Data Unit)?", "id": "Metode agregasi frame lapisan MAC." },
  { "en": "Apa tujuan dari agregasi frame?", "id": "Mengurangi overhead dan meningkatkan efisiensi." },
  { "en": "Apa itu Block ACK?", "id": "Mengakui penerimaan beberapa frame sekaligus." },
  { "en": "Apa itu NAV (Network Allocation Vector)?", "id": "Penghitung waktu mundur penanda media sibuk." },
  { "en": "Apa itu PHY (Physical Layer)?", "id": "Lapisan fisik dalam model OSI." },
  { "en": "Apa peran lapisan PHY (Physical Layer) WIFI (Wireless Fidelity)?", "id": "Mengubah data menjadi sinyal radio." },
  { "en": "Apa peran lapisan MAC (Media Access Control) WIFI (Wireless Fidelity)?", "id": "Mengatur akses ke media nirkabel." },
  { "en": "Apa itu station (STA) di WIFI (Wireless Fidelity)?", "id": "Perangkat apa pun dengan kemampuan WIFI." },
  { "en": "Apa itu distribution system (DS) di WIFI (Wireless Fidelity)?", "id": "Menghubungkan beberapa BSS untuk membentuk ESS." },
  { "en": "Apa itu portal dalam jaringan WIFI (Wireless Fidelity)?", "id": "Gerbang antara jaringan WLAN dan LAN." },
  { "en": "Apa itu GCMP (Galois/Counter Mode Protocol)?", "id": "Protokol enkripsi, lebih efisien dari CCMP." },
  { "en": "Apa itu serangan brute-force pada kata sandi WIFI (Wireless Fidelity)?", "id": "Mencoba semua kemungkinan kombinasi." },
  { "en": "Apa itu serangan kamus pada kata sandi WIFI (Wireless Fidelity)?", "id": "Mencoba kata-kata dari daftar." },
  { "en": "Apa itu key caching?", "id": "Menyimpan kunci untuk koneksi ulang cepat." },
  { "en": "Apa itu PMK (Pairwise Master Key)?", "id": "Kunci master yang dibuat saat otentikasi." },
  { "en": "Apa itu PTK (Pairwise Transient Key)?", "id": "Kunci enkripsi yang berasal dari PMK." },
  { "en": "Apa itu 4-way handshake?", "id": "Proses pembuatan kunci enkripsi unik." },
  { "en": "Apa tujuan dari 4-way handshake?", "id": "Mengkonfirmasi PMK dan membuat PTK." },
  { "en": "Apa itu group key handshake?", "id": "Proses AP mendistribusikan GTK." },
  { "en": "Apa itu GTK (Group Temporal Key)?", "id": "Kunci untuk mengenkripsi lalu lintas broadcast." },
  { "en": "Apa itu Wi-Fi Pineapple?", "id": "Alat untuk audit keamanan nirkabel." },
  { "en": "Apa itu RF (Radio Frequency) jamming?", "id": "Serangan yang mengganggu sinyal nirkabel." },
  { "en": "Apakah RF (Radio Frequency) jamming legal?", "id": "Tidak, tindakan ini ilegal." },
  { "en": "Apa itu metrik channel utilization?", "id": "Persentase waktu saluran sedang sibuk." },
  { "en": "Apa itu retransmission rate?", "id": "Persentase paket yang dikirim ulang." },
  { "en": "Mengapa retransmission rate yang tinggi itu buruk?", "id": "Menandakan koneksi buruk atau interferensi." },
  { "en": "Apa itu airtime utilization?", "id": "Persentase waktu tayang yang digunakan." },
  { "en": "Apa itu mekanisme koeksistensi di WIFI (Wireless Fidelity)?", "id": "Teknik untuk berbagi spektrum frekuensi." },
  { "en": "Bagaimana WIFI (Wireless Fidelity) berdampingan dengan Bluetooth?", "id": "Melalui teknik Adaptive Frequency Hopping." },
  { "en": "Apa itu CAN (Campus Area Network)?", "id": "Jaringan yang mencakup area kampus." },
  { "en": "Apa itu MAN (Metropolitan Area Network)?", "id": "Jaringan yang mencakup area kota." },
  { "en": "Apa itu WAN (Wide Area Network)?", "id": "Jaringan yang mencakup area geografis luas." },
  { "en": "Apa itu PAN (Personal Area Network)?", "id": "Jaringan untuk perangkat pribadi." },
  { "en": "Untuk tipe jaringan mana WIFI (Wireless Fidelity) biasa digunakan?", "id": "WLAN (Wireless Local Area Network)." },
  { "en": "Apa itu wireless backhaul?", "id": "Koneksi nirkabel ke jaringan inti." },
  { "en": "Apa itu FILS (Fast Initial Link Setup)?", "id": "Mempercepat koneksi awal ke jaringan." },
  { "en": "Apa tujuan dari FILS (Fast Initial Link Setup)?", "id": "Mengurangi overhead saat koneksi awal." },
  { "en": "Apa itu skrip profil WLAN (Wireless Local Area Network)?", "id": "Skrip untuk mengkonfigurasi koneksi WIFI." },
  { "en": "Apa itu captive portal bypass?", "id": "Teknik untuk melewati halaman login." },
  { "en": "Apa itu AP (Access Point) untuk luar ruangan?", "id": "AP yang dirancang tahan cuaca." },
  { "en": "Apa perbedaan utama antara router dan AP (Access Point)?", "id": "Router mengelola jaringan, AP memperluasnya." },
  { "en": "Apa itu mode monitor pada kartu WIFI (Wireless Fidelity)?", "id": "Menangkap semua lalu lintas nirkabel." },
  { "en": "Apa itu SSID (Service Set Identifier) cloaking?", "id": "Nama lain menyembunyikan SSID." },
  { "en": "Apa itu WPS (Wi-Fi Protected Setup) lockout?", "id": "Fitur keamanan menonaktifkan WPS." },
  { "en": "Apa itu 'daisy-chaining' pada node mesh?", "id": "Menghubungkan node mesh secara berurutan." },
  { "en": "Mengapa 'daisy-chaining' mesh tidak disarankan?", "id": "Dapat mengurangi kecepatan secara drastis." },
  { "en": "Apa itu 'sticky client' dalam roaming?", "id": "Klien tetap terhubung ke AP jauh." },
  { "en": "Bagaimana 802.11v mengatasi 'sticky client'?", "id": "AP dapat menyarankan klien untuk pindah." },
  { "en": "Apa itu DFS (Dynamic Frequency Selection) false positive?", "id": "Salah mendeteksi sinyal non-radar." },
  { "en": "Apa itu DSSS (Direct-Sequence Spread Spectrum)?", "id": "Teknik modulasi radio yang digunakan WIFI awal." },
  { "en": "Standar WIFI (Wireless Fidelity) awal mana yang menggunakan DSSS (Direct-Sequence Spread Spectrum)?", "id": "Standar 802.11 dan 802.11b." },
  { "en": "Apa itu FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Teknik radio yang melompat antar frekuensi." },
  { "en": "Apa itu OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Teknik modulasi untuk WIFI modern." },
  { "en": "Apa perbedaan utama antara OFDM (Orthogonal Frequency-Division Multiplexing) dan OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "OFDMA melayani banyak pengguna sekaligus." },
  { "en": "Apa itu DCF (Distributed Coordination Function)?", "id": "Metode akses dasar di jaringan WIFI." },
  { "en": "Apa itu PCF (Point Coordination Function)?", "id": "Metode akses terkoordinasi berbasis polling." },
  { "en": "Apakah PCF (Point Coordination Function) banyak digunakan?", "id": "Tidak, sangat jarang diimplementasikan." },
  { "en": "Apa itu HCF (Hybrid Coordination Function)?", "id": "Menggabungkan elemen DCF dan PCF." },
  { "en": "Apa itu EDCA (Enhanced Distributed Channel Access)?", "id": "Mekanisme prioritas untuk QoS di WIFI." },
  { "en": "Apa itu field Frame Control?", "id": "Menjelaskan tipe dan fungsi frame WIFI." },
  { "en": "Apa fungsi field Duration/ID?", "id": "Menunjukkan waktu media akan sibuk." },
  { "en": "Berapa banyak field alamat yang bisa dimiliki frame WIFI (Wireless Fidelity)?", "id": "Bisa sampai empat field alamat." },
  { "en": "Untuk apa field Sequence Control?", "id": "Melacak frame dan fragmen terfragmentasi." },
  { "en": "Apa itu antena phased array?", "id": "Grup antena yang bekerja bersama." },
  { "en": "Apa itu SISO (Single-Input Single-Output)?", "id": "Sistem WIFI dengan satu antena." },
  { "en": "Apa itu serangan IV (Initialization Vector) collision?", "id": "Serangan yang mengeksploitasi kelemahan WEP." },
  { "en": "Apa kerentanan utama WPS (Wi-Fi Protected Setup)?", "id": "PIN mudah diretas dengan brute-force." },
  { "en": "Apa itu kerentanan Dragonblood?", "id": "Serangkaian kelemahan pada WPA3 awal." },
  { "en": "Ada berapa kanal 2.4 GHz yang tidak tumpang tindih?", "id": "Tiga kanal (di sebagian besar wilayah)." },
  { "en": "Kanal 2.4 GHz mana yang tidak tumpang tindih?", "id": "Kanal 1, 6, dan 11." },
  { "en": "Apa itu kanal primer dalam channel bonding?", "id": "Kanal utama yang digunakan untuk komunikasi." },
  { "en": "Apa itu lebar kanal 80+80 MHz?", "id": "Dua blok 80 MHz yang tidak berdekatan." },
  { "en": "Apa itu TPC (Transmit Power Control)?", "id": "Fitur untuk menyesuaikan daya pancar." },
  { "en": "Apa itu dynamic bandwidth switching?", "id": "Mengubah lebar kanal secara dinamis." },
  { "en": "Apa itu WISP (Wireless Internet Service Provider) mode pada CPE (Customer Premises Equipment)?", "id": "Menyambung ke WISP sebagai klien." },
  { "en": "Bagaimana cara kerja airtime fairness?", "id": "Memberi waktu akses yang sama ke semua klien." },
  { "en": "Informasi apa yang diberikan oleh pemindaian WIFI (Wireless Fidelity)?", "id": "SSID, BSSID, kekuatan sinyal, dan kanal." },
  { "en": "Apa itu kesalahan konfigurasi WIFI (Wireless Fidelity) yang umum?", "id": "Salah mengatur lebar kanal atau daya." },
  { "en": "Apa itu batas EIRP (Effective Isotropic Radiated Power)?", "id": "Batas daya pancar legal maksimum." },
  { "en": "Apa itu domain regulasi?", "id": "Aturan frekuensi untuk suatu negara." },
  { "en": "Apa itu ETSI (European Telecommunications Standards Institute)?", "id": "Badan standar telekomunikasi Eropa." },
  { "en": "Apa itu model WIFI (Wireless Fidelity) publik yang didukung iklan?", "id": "Akses gratis setelah menonton iklan." },
  { "en": "Bagaimana cara terbaik memasang repeater WIFI (Wireless Fidelity)?", "id": "Di tengah antara router dan dead zone." },
  { "en": "Bagaimana mengoptimalkan game online melalui WIFI (Wireless Fidelity)?", "id": "Gunakan band 5 GHz, aktifkan QoS." },
  { "en": "Apa itu Barker Code?", "id": "Urutan penyebaran yang digunakan di 802.11b." },
  { "en": "Apa itu chipset di dalam router?", "id": "Otak yang memproses semua data." },
  { "en": "Apa itu RFIC (Radio-Frequency Integrated Circuit)?", "id": "Chip yang menangani sinyal radio." },
  { "en": "Apa itu frame ACK (Acknowledgement)?", "id": "Frame untuk mengkonfirmasi penerimaan data." },
  { "en": "Apa yang terjadi jika frame ACK (Acknowledgement) tidak diterima?", "id": "Data akan dikirim ulang." },
  { "en": "Apa itu frame RTS (Request to Send)?", "id": "Meminta izin untuk mengirim data." },
  { "en": "Apa itu frame CTS (Clear to Send)?", "id": "Memberikan izin untuk mengirim data." },
  { "en": "Apa itu handshake RTS/CTS (Request to Send/Clear to Send)?", "id": "Mekanisme untuk mengatasi hidden node." },
  { "en": "Apa itu header PHY (Physical Layer)?", "id": "Bagian frame untuk lapisan fisik." },
  { "en": "Apa itu header MAC (Media Access Control)?", "id": "Bagian frame untuk lapisan MAC." },
  { "en": "Apa itu FCS (Frame Check Sequence)?", "id": "Kode untuk memeriksa kesalahan transmisi." },
  { "en": "Apa fungsi dari FCS (Frame Check Sequence)?", "id": "Memastikan integritas data frame." },
  { "en": "Apa itu CRC (Cyclic Redundancy Check)?", "id": "Algoritma yang digunakan untuk FCS." },
  { "en": "Apa itu backoff timer di WIFI (Wireless Fidelity)?", "id": "Waktu tunggu acak sebelum mengirim." },
  { "en": "Apa itu NDP (Null Data Packet)?", "id": "Paket tanpa data, untuk kontrol." },
  { "en": "Apa itu sounding di WIFI (Wireless Fidelity)?", "id": "Proses untuk mengukur kondisi kanal." },
  { "en": "Apa itu CSI (Channel State Information)?", "id": "Informasi tentang kualitas kanal radio." },
  { "en": "Apa itu diagram konstelasi di QAM (Quadrature Amplitude Modulation)?", "id": "Representasi visual dari sinyal modulasi." },
  { "en": "Apa itu BER (Bit Error Rate)?", "id": "Tingkat kesalahan dalam transmisi data." },
  { "en": "Apa yang menyebabkan BER (Bit Error Rate) tinggi?", "id": "Sinyal lemah atau interferensi tinggi." },
  { "en": "Apa itu hotspot WIFI (Wireless Fidelity) dari penyedia layanan?", "id": "Hotspot yang dioperasikan oleh ISP." },
  { "en": "Apa itu AP (Access Point) kelas enterprise?", "id": "AP dengan fitur manajemen terpusat." },
  { "en": "Apa itu router kelas konsumen?", "id": "Router yang dirancang untuk penggunaan rumahan." },
  { "en": "Apa itu travel router?", "id": "Router kecil portabel untuk bepergian." },
  { "en": "Apa itu SSID (Service Set Identifier) 'default'?", "id": "Nama WIFI bawaan dari pabrik." },
  { "en": "Mengapa Anda harus mengubah SSID (Service Set Identifier) default?", "id": "Untuk keamanan dan identifikasi mudah." },
  { "en": "Apa itu aplikasi pemindai WIFI (Wireless Fidelity)?", "id": "Aplikasi untuk melihat jaringan sekitar." },
  { "en": "Apa itu serangan deauthentication flood?", "id": "Membanjiri AP dengan permintaan deauthentication." },
  { "en": "Apa itu beacon flooding?", "id": "Membuat banyak AP palsu." },
  { "en": "Apa itu WEP (Wired Equivalent Privacy) cracking tool?", "id": "Perangkat lunak untuk memecahkan kunci WEP." },
  { "en": "Apa itu WPA (Wi-Fi Protected Access) handshake capture?", "id": "Menangkap data untuk meretas kata sandi." },
  { "en": "Apa itu serangan kamus offline?", "id": "Menebak kata sandi tanpa terhubung." },
  { "en": "Mengapa kata sandi yang kuat itu penting?", "id": "Mencegah akses tidak sah ke jaringan." },
  { "en": "Apa itu frasa sandi (passphrase)?", "id": "Kata sandi yang lebih panjang." },
  { "en": "Seberapa panjang seharusnya kata sandi WPA2 (Wi-Fi Protected Access 2)?", "id": "Minimal dua belas karakter." },
  { "en": "Apa itu Service Set di WIFI (Wireless Fidelity)?", "id": "Sekelompok perangkat nirkabel yang terhubung." },
  { "en": "Apa itu BSS (Basic Service Set)?", "id": "Satu AP dan semua kliennya." },
  { "en": "Apa itu ESS (Extended Service Set)?", "id": "Dua atau lebih BSS yang terhubung." },
  { "en": "Apa itu IBSS (Independent Basic Service Set)?", "id": "Jaringan ad-hoc tanpa AP." },
  { "en": "Apa itu MBSS (Mesh Basic Service Set)?", "id": "Jaringan node dalam sistem mesh." },
  { "en": "Apa itu FT (Fast BSS Transition)?", "id": "Mekanisme untuk roaming yang lebih cepat." },
  { "en": "Apa nama lain untuk FT (Fast BSS Transition)?", "id": "Standar IEEE 802.11r." },
  { "en": "Apa itu radio chain?", "id": "Sirkuit pemancar dan penerima tunggal." },
  { "en": "Apa arti konfigurasi MIMO (Multiple Input, Multiple Output) 3x3:2?", "id": "Tiga antena, dua aliran spasial." },
  { "en": "Apa itu data stream (aliran data)?", "id": "Aliran data independen melalui MIMO." },
  { "en": "Apa itu interference margin?", "id": "Batas sinyal di atas noise." },
  { "en": "Apa itu CCI (Co-Channel Interference)?", "id": "Gangguan dari AP di kanal sama." },
  { "en": "Apa itu ACI (Adjacent Channel Interference)?", "id": "Gangguan dari AP di kanal sebelah." },
  { "en": "Bagaimana cara mengurangi CCI (Co-Channel Interference)?", "id": "Gunakan kanal 1, 6, dan 11." },
  { "en": "Bagaimana cara mengurangi ACI (Adjacent Channel Interference)?", "id": "Gunakan lebar kanal 20 MHz." },
  { "en": "Apa itu link budget jembatan nirkabel?", "id": "Perhitungan semua gain dan loss." },
  { "en": "Apa itu Fresnel Zone?", "id": "Area berbentuk elips di sekitar sinyal." },
  { "en": "Mengapa Fresnel Zone harus bebas halangan?", "id": "Halangan dapat melemahkan sinyal." },
  { "en": "Apa itu grounding untuk peralatan nirkabel luar ruangan?", "id": "Melindungi peralatan dari sambaran petir." },
  { "en": "Apa itu weatherproofing untuk konektor RF (Radio Frequency)?", "id": "Melindungi konektor dari air dan cuaca." },
  { "en": "Apa itu 'channel reuse' plan?", "id": "Rencana penempatan kanal untuk minimalisasi CCI." },
  { "en": "Apa itu 'stickiness' klien WIFI (Wireless Fidelity)?", "id": "Klien enggan beralih ke AP lebih baik." },
  { "en": "Apa itu 'load balancing' klien di WIFI (Wireless Fidelity)?", "id": "Mendistribusikan klien secara merata antar AP." },
  { "en": "Apa itu 'rogue AP detection'?", "id": "Fitur untuk mendeteksi AP tidak sah." },
  { "en": "Apa itu 'wireless containment'?", "id": "Menonaktifkan rogue AP secara paksa." },
  { "en": "Apakah 'wireless containment' legal?", "id": "Bisa ilegal, dapat mengganggu jaringan lain." },
  { "en": "Apa itu ePDG (Evolved Packet Data Gateway) dalam VoWIFI (Voice over Wi-Fi)?", "id": "Gerbang aman antara WIFI dan jaringan seluler." },
  { "en": "Apa itu IMS (IP Multimedia Subsystem) dalam VoWIFI (Voice over Wi-Fi)?", "id": "Kerangka kerja arsitektur untuk layanan IP." },
  { "en": "Apa fungsi SIP (Session Initiation Protocol) dalam VoWIFI (Voice over Wi-Fi)?", "id": "Memulai, mengelola, dan mengakhiri panggilan suara." },
  { "en": "Apa fungsi RTP (Real-time Transport Protocol) dalam VoWIFI (Voice over Wi-Fi)?", "id": "Mengirimkan data audio dan video aktual." },
  { "en": "Apa itu ANQP (Access Network Query Protocol)?", "id": "Protokol untuk mendapatkan informasi jaringan." },
  { "en": "Apa itu GAS (Generic Advertisement Service)?", "id": "Layanan untuk mentransportasikan frame ANQP." },
  { "en": "Apa itu punctured preamble di WIFI (Wireless Fidelity) 7?", "id": "Membuat lubang di kanal untuk hindari interferensi." },
  { "en": "Apa itu rTWT (Restricted Target Wake Time)?", "id": "Versi TWT yang lebih terprediksi." },
  { "en": "Apa itu MFP (Management Frame Protection)?", "id": "Nama lain dari PMF (802.11w)." },
  { "en": "Apa itu honeypot nirkabel?", "id": "AP palsu untuk menjebak penyerang." },
  { "en": "Apa itu pola azimuth antena?", "id": "Pola pancaran sinyal secara horizontal." },
  { "en": "Apa itu pola elevasi antena?", "id": "Pola pancaran sinyal secara vertikal." },
  { "en": "Apa itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran ketidakcocokan impedansi antena." },
  { "en": "Apa yang ditunjukkan oleh VSWR (Voltage Standing Wave Ratio) yang tinggi?", "id": "Banyak sinyal yang dipantulkan kembali." },
  { "en": "Apa itu return loss?", "id": "Ukuran sinyal yang hilang akibat pantulan." },
  { "en": "Apa itu cable loss?", "id": "Pelemahan sinyal saat melewati kabel." },
  { "en": "Apa itu Kismet?", "id": "Alat pendeteksi jaringan nirkabel." },
  { "en": "Apa itu Aircrack-ng?", "id": "Paket perangkat lunak untuk audit keamanan." },
  { "en": "Untuk apa airodump-ng digunakan?", "id": "Untuk menangkap paket data nirkabel." },
  { "en": "Apa itu AID (Association ID)?", "id": "Nomor identifikasi untuk klien yang terhubung." },
  { "en": "Apa itu mekanisme CTS-to-self (Clear to Send-to-self)?", "id": "Mengirim frame CTS ke diri sendiri." },
  { "en": "Apa itu kategori akses AC_VO di QoS (Quality of Service)?", "id": "Prioritas tertinggi untuk lalu lintas suara." },
  { "en": "Apa itu kategori akses AC_VI di QoS (Quality of Service)?", "id": "Prioritas tinggi untuk lalu lintas video." },
  { "en": "Apa itu kategori akses AC_BE di QoS (Quality of Service)?", "id": "Prioritas normal untuk lalu lintas umum." },
  { "en": "Apa itu kategori akses AC_BK di QoS (Quality of Service)?", "id": "Prioritas rendah untuk lalu lintas latar belakang." },
  { "en": "Apa itu WMM-PS (Wi-Fi Multimedia Power Save)?", "id": "Mode hemat daya dengan dukungan QoS." },
  { "en": "Apa itu carrier WIFI (Wireless Fidelity) offloading?", "id": "Memindahkan lalu lintas seluler ke WIFI." },
  { "en": "Apakah Zigbee mengganggu WIFI (Wireless Fidelity)?", "id": "Ya, karena keduanya menggunakan band 2.4 GHz." },
  { "en": "Apa itu standar IEEE (Institute of Electrical and Electronics Engineers) 802.11ba?", "id": "Standar untuk Wake-Up Radio (WUR)." },
  { "en": "Apa itu WUR (Wake-up Radio)?", "id": "Radio sekunder berdaya sangat rendah." },
  { "en": "Apa tantangan utama untuk WIFI (Wireless Fidelity) di stadion?", "id": "Kepadatan pengguna yang sangat tinggi." },
  { "en": "Bagaimana cara kerja WIFI (Wireless Fidelity) di kereta?", "id": "Menggunakan agregasi sinyal seluler." },
  { "en": "Apa itu login media sosial untuk captive portal?", "id": "Menggunakan akun media sosial untuk masuk." },
  { "en": "Apa itu sistem voucher untuk akses WIFI (Wireless Fidelity)?", "id": "Menggunakan kode unik untuk akses terbatas." },
  { "en": "Apa fungsi RTCP (RTP Control Protocol)?", "id": "Memberikan statistik kontrol untuk RTP." },
  { "en": "Apa itu profil Hotspot 2.0?", "id": "Pengaturan untuk koneksi otomatis dan aman." },
  { "en": "Apa itu OSU (Online Sign-Up)?", "id": "Server untuk pendaftaran akun Hotspot 2.0." },
  { "en": "Apa itu 'walled garden' dalam jaringan?", "id": "Akses terbatas sebelum otentikasi penuh." },
  { "en": "Apa itu randomisasi alamat MAC (Media Access Control)?", "id": "Fitur privasi yang mengubah alamat MAC." },
  { "en": "Mengapa perangkat seluler menggunakan randomisasi MAC (Media Access Control)?", "id": "Untuk mencegah pelacakan berbasis lokasi." },
  { "en": "Apa itu BSS (Basic Service Set) Load element?", "id": "Elemen yang melaporkan tingkat kesibukan AP." },
  { "en": "Apa itu contention-free period (CFP)?", "id": "Periode saat AP mengontrol akses media." },
  { "en": "Apa itu contention period (CP)?", "id": "Periode saat semua perangkat bersaing." },
  { "en": "Apa itu serangan NAV (Network Allocation Vector)?", "id": "Memanipulasi timer NAV untuk menghentikan lalu lintas." },
  { "en": "Apa itu frame beacon?", "id": "Detak jantung dari sebuah jaringan WIFI." },
  { "en": "Informasi apa yang ada di dalam frame beacon?", "id": "SSID, kecepatan, dan kapabilitas keamanan." },
  { "en": "Apa itu interval beacon?", "id": "Seberapa sering beacon dikirim." },
  { "en": "Berapa interval beacon yang umum?", "id": "Seratus milidetik (100 ms)." },
  { "en": "Apa yang terjadi jika interval beacon terlalu pendek?", "id": "Meningkatkan overhead pada jaringan." },
  { "en": "Apa yang terjadi jika interval beacon terlalu panjang?", "id": "Memperlambat penemuan jaringan baru." },
  { "en": "Apa itu probe request?", "id": "Permintaan yang dikirim klien untuk menemukan jaringan." },
  { "en": "Apa itu directed probe request?", "id": "Probe untuk SSID tertentu." },
  { "en": "Apa itu broadcast probe request?", "id": "Probe untuk semua SSID di sekitar." },
  { "en": "Apa itu reassociation request?", "id": "Permintaan klien untuk roaming ke AP baru." },
  { "en": "Kapan reassociation request digunakan?", "id": "Saat perangkat berpindah antar AP." },
  { "en": "Apa itu disassociation frame?", "id": "Frame untuk mengakhiri koneksi secara normal." },
  { "en": "Apakah disassociation frame dienkripsi?", "id": "Tidak, jika tidak menggunakan PMF." },
  { "en": "Apa itu scrambling di PHY (Physical Layer) WIFI (Wireless Fidelity)?", "id": "Mengacak data untuk menghindari urutan panjang." },
  { "en": "Apa itu encoding di PHY (Physical Layer) WIFI (Wireless Fidelity)?", "id": "Menambahkan redundansi untuk koreksi kesalahan." },
  { "en": "Apa itu interleaver di PHY (Physical Layer) WIFI (Wireless Fidelity)?", "id": "Mengubah urutan bit untuk ketahanan." },
  { "en": "Apa itu IFFT (Inverse Fast Fourier Transform)?", "id": "Algoritma matematika untuk menghasilkan sinyal." },
  { "en": "Apa peran IFFT (Inverse Fast Fourier Transform) di OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Mengubah sinyal frekuensi menjadi sinyal waktu." },
  { "en": "Apa itu guard band?", "id": "Pita frekuensi kosong antar kanal." },
  { "en": "Apa itu out-of-band emission?", "id": "Pancaran sinyal di luar kanal resmi." },
  { "en": "Apa itu spectral mask?", "id": "Batas legal untuk out-of-band emission." },
  { "en": "Apa itu antena PIFA (Planar Inverted-F Antenna)?", "id": "Jenis antena internal yang umum." },
  { "en": "Di mana antena PIFA (Planar Inverted-F Antenna) biasa ditemukan?", "id": "Di dalam ponsel dan laptop." },
  { "en": "Apa itu antena chip?", "id": "Antena keramik kecil untuk perangkat ringkas." },
  { "en": "Apa itu antena PCB (Printed Circuit Board)?", "id": "Antena yang terukir di papan sirkuit." },
  { "en": "Apa itu SU-BF (Single-User Beamforming)?", "id": "Beamforming yang ditujukan untuk satu klien." },
  { "en": "Apa itu MU-BF (Multi-User Beamforming)?", "id": "Beamforming ke beberapa klien secara bersamaan." },
  { "en": "Apa itu LNA (Low-Noise Amplifier)?", "id": "Memperkuat sinyal yang diterima sangat lemah." },
  { "en": "Apa itu PA (Power Amplifier) di radio chain?", "id": "Memperkuat sinyal sebelum dipancarkan." },
  { "en": "Apa itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah data digital menjadi sinyal analog." },
  { "en": "Apa itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah sinyal analog menjadi data digital." },
  { "en": "Apa itu pemrosesan baseband?", "id": "Pemrosesan sinyal di frekuensi aslinya." },
  { "en": "Apa itu prosesor baseband?", "id": "Chip yang melakukan pemrosesan baseband." },
  { "en": "Apa itu header radiotap?", "id": "Header metadata untuk paket nirkabel." },
  { "en": "Apa itu packet sniffer nirkabel?", "id": "Alat untuk menangkap dan menganalisis paket." },
  { "en": "Apa itu mode 'promiscuous' untuk adaptor WIFI (Wireless Fidelity)?", "id": "Mode yang menangkap semua paket." },
  { "en": "Apa itu lingkungan WIFI (Wireless Fidelity) kepadatan tinggi?", "id": "Banyak klien dalam area kecil." },
  { "en": "Apa tujuan desain utama untuk WIFI (Wireless Fidelity) kepadatan tinggi?", "id": "Mengelola interferensi dan kapasitas." },
  { "en": "Apa itu penerapan AP (Access Point) microcell?", "id": "Menggunakan banyak AP berdaya rendah." },
  { "en": "Apa itu arsitektur kanal tunggal?", "id": "Semua AP menggunakan kanal yang sama." },
  { "en": "Apa itu virtual cell?", "id": "Beberapa AP fisik terlihat sebagai satu AP." },
  { "en": "Apa itu BSSID (Basic Service Set Identifier) virtual?", "id": "Beberapa BSSID pada satu radio AP." },
  { "en": "Apa itu penetapan VLAN (Virtual Local Area Network) dinamis?", "id": "Menetapkan VLAN berdasarkan kredensial pengguna." },
  { "en": "Apa itu Roaming Consortium?", "id": "Perjanjian antar operator untuk berbagi hotspot." },
  { "en": "Apa itu WISPr (Wireless ISP Roaming)?", "id": "Protokol untuk otentikasi roaming." },
  { "en": "Apa itu ACK timeout?", "id": "Batas waktu menunggu frame ACK." },
  { "en": "Apa itu 'scatternet' Bluetooth?", "id": "Jaringan ad-hoc dari beberapa piconet." },
  { "en": "Apa itu Time Division Duplexing (TDD)?", "id": "Mengirim dan menerima di slot waktu." },
  { "en": "Apa itu Frequency Division Duplexing (FDD)?", "id": "Mengirim dan menerima di frekuensi berbeda." },
  { "en": "Apakah WIFI (Wireless Fidelity) menggunakan TDD (Time Division Duplexing) atau FDD (Frequency Division Duplexing)?", "id": "WIFI menggunakan TDD." },
  { "en": "Apa itu trigger frame di WIFI (Wireless Fidelity) 6?", "id": "Frame dari AP untuk mengkoordinasikan uplink." },
  { "en": "Apa itu UORA (Uplink OFDMA-based Random Access)?", "id": "Memungkinkan klien mengirim data uplink terkoordinasi." },
  { "en": "Apa itu Multi-TID (Traffic Identifier) A-MPDU (Aggregate MAC Protocol Data Unit)?", "id": "Menggabungkan lalu lintas dari berbagai prioritas." },
  { "en": "Apa itu WIFI (Wireless Fidelity) Sensing?", "id": "Menggunakan sinyal WIFI untuk deteksi gerak." },
  { "en": "Apa standar untuk WIFI (Wireless Fidelity) Sensing?", "id": "Standar IEEE 802.11bf." },
  { "en": "Apa itu hashcat?", "id": "Alat peretas kata sandi berkecepatan tinggi." },
  { "en": "Apa itu serangan Pixie Dust?", "id": "Serangan yang mengeksploitasi kelemahan WPS." },
  { "en": "Apa itu serangan KARMA?", "id": "Membuat AP palsu berdasarkan probe request." },
  { "en": "Apa itu near field dalam komunikasi radio?", "id": "Area reaktif di dekat antena." },
  { "en": "Apa itu far field dalam komunikasi radio?", "id": "Area radiasi di jauh dari antena." },
  { "en": "Apa beda antara EIRP (Effective Isotropic Radiated Power) dan ERP (Effective Radiated Power)?", "id": "ERP dibandingkan dengan antena dipole." },
  { "en": "Apa itu diversitas MIMO (Multiple Input, Multiple Output)?", "id": "Menggunakan antena ganda untuk keandalan." },
  { "en": "Apa itu spatial multiplexing MIMO (Multiple Input, Multiple Output)?", "id": "Menggunakan antena ganda untuk kecepatan." },
  { "en": "Apa itu TSF (Timing Synchronization Function)?", "id": "Timer yang menjaga sinkronisasi semua perangkat." },
  { "en": "Apa tujuan dari TSF (Timing Synchronization Function)?", "id": "Mengkoordinasikan fungsi seperti FHSS dan PCF." },
  { "en": "Apa itu U-APSD (Unscheduled Automatic Power Save Delivery)?", "id": "Mode hemat daya yang efisien untuk VoIP." },
  { "en": "Apa itu RRM (Radio Resource Management)?", "id": "Fitur untuk mengoptimalkan sumber daya radio." },
  { "en": "Apa itu teknologi Cisco CleanAir?", "id": "Mendeteksi dan mengklasifikasikan interferensi RF." },
  { "en": "Siapa Broadcom di pasar WIFI (Wireless Fidelity)?", "id": "Produsen chipset WIFI terkemuka." },
  { "en": "Siapa Qualcomm di pasar WIFI (Wireless Fidelity)?", "id": "Produsen chipset WIFI terkemuka." },
  { "en": "Apa itu uplink OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Beberapa klien mengirim data ke AP serentak." },
  { "en": "Apa itu uplink MU-MIMO (Multi-User, Multiple Input, Multiple Output)?", "id": "Beberapa klien mengirim stream ke AP serentak." },
  { "en": "Informasi apa yang ada di field HT (High Throughput) Control?", "id": "Informasi terkait fitur 802.11n." },
  { "en": "Informasi apa yang ada di field QoS (Quality of Service) Control?", "id": "Informasi prioritas lalu lintas." },
  { "en": "Apa itu preamble HE-SU (High Efficiency-Single User)?", "id": "Preamble untuk transmisi ke satu pengguna." },
  { "en": "Apa itu preamble HE-MU (High Efficiency-Multi User)?", "id": "Preamble untuk transmisi ke banyak pengguna." },
  { "en": "Apa itu masalah bufferbloat?", "id": "Peningkatan latensi akibat antrian buffer berlebih." },
  { "en": "Apa itu SoC (System-on-a-Chip) WIFI (Wireless Fidelity)?", "id": "Satu chip yang berisi semua fungsi WIFI." },
  { "en": "Apa itu FEM (Front-End Module) di radio?", "id": "Mengintegrasikan LNA, PA, dan sakelar RF." },
  { "en": "Apa itu RF (Radio Frequency) chain?", "id": "Jalur sinyal radio dari baseband." },
  { "en": "Apa itu diagram I/Q (In-phase and Quadrature)?", "id": "Cara merepresentasikan sinyal termodulasi." },
  { "en": "Apa itu phase noise?", "id": "Ketidakstabilan fasa dalam osilator." },
  { "en": "Apa itu PLL (Phase-Locked Loop)?", "id": "Sirkuit untuk menghasilkan sinyal frekuensi stabil." },
  { "en": "Apa itu osilator?", "id": "Sirkuit elektronik yang menghasilkan sinyal periodik." },
  { "en": "Apa itu mixer di radio?", "id": "Mengubah sinyal dari satu frekuensi ke frekuensi lain." },
  { "en": "Apa itu IF (Intermediate Frequency)?", "id": "Frekuensi perantara dalam pemrosesan sinyal." },
  { "en": "Apa itu receiver superheterodyne?", "id": "Arsitektur penerima radio yang umum." },
  { "en": "Apa itu receiver homodyne (zero-IF)?", "id": "Arsitektur penerima yang lebih sederhana." },
  { "en": "Apa itu PER (Packet Error Rate)?", "id": "Persentase paket yang diterima dengan kesalahan." },
  { "en": "Apa itu FEC (Forward Error Correction)?", "id": "Teknik menambahkan data untuk memperbaiki eror." },
  { "en": "Apa itu kode konvolusional?", "id": "Jenis kode koreksi kesalahan yang umum." },
  { "en": "Apa itu Viterbi decoder?", "id": "Algoritma untuk mendekode kode konvolusional." },
  { "en": "Apa itu parity bit?", "id": "Bit tambahan untuk mendeteksi kesalahan." },
  { "en": "Apa itu checksum?", "id": "Nilai untuk memeriksa integritas data." },
  { "en": "Apa itu ICMP (Internet Control Message Protocol)?", "id": "Protokol untuk pesan diagnostik jaringan." },
  { "en": "Apa itu ARP (Address Resolution Protocol)?", "id": "Memetakan alamat IP ke alamat MAC." },
  { "en": "Apa yang disimpan oleh tabel ARP (Address Resolution Protocol)?", "id": "Pemetaan alamat IP ke alamat MAC." },
  { "en": "Apa itu serangan ARP (Address Resolution Protocol) poisoning?", "id": "Mengirim balasan ARP palsu." },
  { "en": "Apa itu serangan DHCP (Dynamic Host Configuration Protocol) starvation?", "id": "Menghabiskan semua alamat IP yang tersedia." },
  { "en": "Apa itu DHCP (Dynamic Host Configuration Protocol) snooping?", "id": "Fitur keamanan untuk memvalidasi pesan DHCP." },
  { "en": "Apa itu unicast frame?", "id": "Frame yang ditujukan untuk satu perangkat." },
  { "en": "Apa itu multicast frame?", "id": "Frame yang ditujukan untuk sekelompok perangkat." },
  { "en": "Apa itu broadcast frame?", "id": "Frame yang ditujukan untuk semua perangkat." },
  { "en": "Ke alamat MAC (Media Access Control) mana broadcast frame dikirim?", "id": "FF:FF:FF:FF:FF:FF." },
  { "en": "Apa itu PLCP (Physical Layer Convergence Procedure)?", "id": "Sublapisan yang menyiapkan data untuk PMD." },
  { "en": "Apa itu PMD (Physical Medium Dependent)?", "id": "Sublapisan yang menangani transmisi radio." },
  { "en": "Apa itu MSDU (MAC Service Data Unit)?", "id": "Data dari lapisan atas sebelum header MAC." },
  { "en": "Apa itu MPDU (MAC Protocol Data Unit)?", "id": "MSDU setelah header MAC ditambahkan." },
  { "en": "Apa itu PSDU (PHY Service Data Unit)?", "id": "Data yang diterima oleh lapisan PHY." },
  { "en": "Apa itu PPDU (PHY Protocol Data Unit)?", "id": "PSDU setelah header PHY ditambahkan." },
  { "en": "Apa itu enkapsulasi dalam jaringan?", "id": "Menambahkan header ke data di setiap lapisan." },
  { "en": "Apa itu TXOP (Transmit Opportunity)?", "id": "Interval waktu untuk mengirim beberapa frame." },
  { "en": "Apa tujuan dari TXOP (Transmit Opportunity)?", "id": "Meningkatkan efisiensi di jaringan yang sibuk." },
  { "en": "Apa itu listen interval?", "id": "Interval perangkat 'bangun' untuk mengecek beacon." },
  { "en": "Apa itu OKC (Opportunistic Key Caching)?", "id": "Metode untuk mempercepat roaming di WPA2." },
  { "en": "Apa itu MIC (Message Integrity Check)?", "id": "Mekanisme untuk melindungi frame dari pemalsuan." },
  { "en": "Apa itu algoritma Michael?", "id": "Algoritma MIC yang digunakan oleh TKIP." },
  { "en": "Apa itu temporal key?", "id": "Kunci enkripsi yang berubah secara berkala." },
  { "en": "Apa itu key lifetime?", "id": "Durasi validitas sebuah kunci enkripsi." },
  { "en": "Apa itu session key?", "id": "Kunci enkripsi yang valid untuk satu sesi." },
  { "en": "Apa itu static WEP key?", "id": "Kunci WEP yang dimasukkan secara manual." },
  { "en": "Berapa banyak kunci WEP (Wired Equivalent Privacy) statis yang bisa dikonfigurasi?", "id": "Hingga empat kunci." },
  { "en": "Apa itu profil Passpoint?", "id": "Informasi untuk koneksi otomatis ke hotspot." },
  { "en": "Apa itu NAI (Network Access Identifier)?", "id": "Format identitas pengguna untuk otentikasi." },
  { "en": "Apa itu realm dalam NAI (Network Access Identifier)?", "id": "Nama domain yang mengidentifikasi penyedia layanan." },
  { "en": "Apa itu EAP-SIM (Extensible Authentication Protocol-Subscriber Identity Module)?", "id": "Metode EAP yang menggunakan kartu SIM." },
  { "en": "Apa itu EAP-AKA (Extensible Authentication Protocol-Authentication and Key Agreement)?", "id": "Versi EAP-SIM yang lebih aman." },
  { "en": "Apa itu EAP-FAST (Flexible Authentication via Secure Tunneling)?", "id": "Protokol EAP yang dikembangkan oleh Cisco." },
  { "en": "Apa itu PAC (Protected Access Credential)?", "id": "Kredensial yang digunakan oleh EAP-FAST." },
  { "en": "Apa itu EAP (Extensible Authentication Protocol) chaining?", "id": "Menggabungkan otentikasi pengguna dan mesin." },
  { "en": "Apa itu 'air marshal'?", "id": "Fitur WIPS dari Cisco Meraki." },
  { "en": "Apa itu modulasi adaptif?", "id": "Mengubah skema modulasi sesuai kondisi." },
  { "en": "Apa itu 'greenfield' mode di 802.11n?", "id": "Mode hanya untuk perangkat 802.11n." },
  { "en": "Apa itu 'mixed' mode di 802.11n?", "id": "Mode untuk perangkat baru dan lama." },
  { "en": "Apa itu 'protection mechanism' di WIFI (Wireless Fidelity)?", "id": "Mekanisme untuk mendukung perangkat lama." },
  { "en": "Apa itu 'dual-band' router?", "id": "Router yang mendukung 2.4 dan 5 GHz." },
  { "en": "Apa itu 'tri-band' router?", "id": "Router dengan satu band 2.4 GHz dan dua 5 GHz." },
  { "en": "Apa itu 'quad-band' router?", "id": "Router dengan tambahan band 6 GHz." },
  { "en": "Apa itu 'WIFI range'?", "id": "Jarak maksimum sinyal WIFI." },
  { "en": "Apa itu 'dead-zone'?", "id": "Area tanpa sinyal WIFI." },
  { "en": "Apa itu 'WIFI tethering'?", "id": "Berbagi koneksi internet via WIFI." },
  { "en": "Apa itu 'WIFI bridge'?", "id": "Menghubungkan dua jaringan dengan WIFI." },
  { "en": "Apa itu 'WIFI dongle'?", "id": "Adaptor WIFI USB portabel." },
  { "en": "Apa itu 'wireless client'?", "id": "Perangkat yang terhubung ke WIFI." },
  { "en": "Apa itu 'wireless access point'?", "id": "Perangkat yang membuat jaringan WIFI." },
  { "en": "Apa itu 'wireless network adapter'?", "id": "Perangkat keras untuk terhubung ke WIFI." },
  { "en": "Apa itu 'wireless security camera'?", "id": "Kamera yang mengirim video via WIFI." },
  { "en": "Apa itu Wi-Fi 6 Release 2?", "id": "Pembaruan standar WIFI 6." },
  { "en": "Apa fitur utama Wi-Fi 6 Release 2?", "id": "Peningkatan efisiensi uplink dan daya." },
  { "en": "Apa itu FTM (Fine Timing Measurement)?", "id": "Protokol untuk mengukur jarak antar perangkat." },
  { "en": "Apa tujuan dari FTM (Fine Timing Measurement)?", "id": "Untuk layanan lokasi dalam ruangan." },
  { "en": "Apa standar untuk Next Generation Positioning?", "id": "Standar IEEE 802.11az." },
  { "en": "Apa itu DSRC (Dedicated Short-Range Communications)?", "id": "Komunikasi untuk kendaraan di 5.9 GHz." },
  { "en": "Apa standar IEEE (Institute of Electrical and Electronics Engineers) untuk DSRC (Dedicated Short-Range Communications)?", "id": "Standar IEEE 802.11p." },
  { "en": "Apa itu multipath fading?", "id": "Sinyal melemah akibat pantulan ganda." },
  { "en": "Apa itu delay spread?", "id": "Perbedaan waktu tiba sinyal multipath." },
  { "en": "Apa itu SIFS (Short Interframe Space)?", "id": "Waktu jeda antar frame prioritas tinggi." },
  { "en": "Apa itu DIFS (DCF Interframe Space)?", "id": "Waktu tunggu standar sebelum transmisi." },
  { "en": "Apa itu EIFS (Extended Interframe Space)?", "id": "Waktu tunggu setelah frame error." },
  { "en": "Apa itu AIFS (Arbitration Interframe Space)?", "id": "Waktu tunggu yang digunakan oleh QoS." },
  { "en": "Apa arti bit 'To DS (Distribution System)' dalam sebuah frame?", "id": "Frame ditujukan ke sistem distribusi." },
  { "en": "Apa arti bit 'From DS (Distribution System)' dalam sebuah frame?", "id": "Frame berasal dari sistem distribusi." },
  { "en": "Apa arti bit 'Retry' dalam sebuah frame?", "id": "Frame ini adalah transmisi ulang." },
  { "en": "Apa arti bit 'Power Management' dalam sebuah frame?", "id": "Menunjukkan status hemat daya klien." },
  { "en": "Apa itu atribut RADIUS (Remote Authentication Dial-In User Service)?", "id": "Informasi tentang pengguna atau koneksi." },
  { "en": "Apa itu pesan RADIUS (Remote Authentication Dial-In User Service) CoA (Change of Authorization)?", "id": "Mengubah otorisasi sesi yang aktif." },
  { "en": "Apa itu LLDP (Link Layer Discovery Protocol)?", "id": "Protokol untuk menemukan perangkat tetangga." },
  { "en": "Apa itu CDP (Cisco Discovery Protocol)?", "id": "Versi LLDP milik Cisco." },
  { "en": "Apa itu NAC (Network Access Control)?", "id": "Kebijakan keamanan untuk akses jaringan." },
  { "en": "Apa itu kebijakan BYOD (Bring Your Own Device)?", "id": "Mengizinkan perangkat pribadi di jaringan." },
  { "en": "Apa itu diplexer dalam sistem RF (Radio Frequency)?", "id": "Menggabungkan atau memisahkan dua frekuensi." },
  { "en": "Apa itu filter band-pass?", "id": "Melewatkan frekuensi dalam rentang tertentu." },
  { "en": "Apa itu sertifikasi Wi-Fi Vantage?", "id": "Untuk pengalaman pengguna yang lebih baik." },
  { "en": "Apa itu sertifikasi Wi-Fi Agile Multiband?", "id": "Untuk roaming cerdas antar band." },
  { "en": "Apa fungsi perintah 'netsh wlan show interfaces'?", "id": "Menampilkan informasi adaptor WIFI." },
  { "en": "Apa itu 'PHY type' dalam output netsh?", "id": "Menunjukkan standar 802.11 yang digunakan." },
  { "en": "Apa itu Rician fading channel?", "id": "Kanal dengan satu jalur sinyal dominan." },
  { "en": "Apa itu Rayleigh fading channel?", "id": "Kanal tanpa jalur sinyal dominan." },
  { "en": "Apa itu TCM (Trellis Coded Modulation)?", "id": "Menggabungkan modulasi dan pengkodean." },
  { "en": "Apa itu kode Reed-Solomon?", "id": "Jenis kode koreksi kesalahan blok." },
  { "en": "Apa arti bit 'More Fragments'?", "id": "Ada lebih banyak fragmen yang akan datang." },
  { "en": "Apa arti bit 'More Data'?", "id": "AP memiliki lebih banyak data untuk klien." },
  { "en": "Apa itu pesan RADIUS (Remote Authentication Dial-In User Service) Disconnect-Message?", "id": "Memutuskan koneksi pengguna secara paksa." },
  { "en": "Apa itu posture assessment di NAC (Network Access Control)?", "id": "Memeriksa kepatuhan keamanan perangkat." },
  { "en": "Apa itu VLAN (Virtual Local Area Network) remediasi?", "id": "VLAN terbatas untuk perangkat tidak patuh." },
  { "en": "Apa itu filter low-pass di RF (Radio Frequency)?", "id": "Melewatkan frekuensi di bawah ambang batas." },
  { "en": "Apa itu filter high-pass di RF (Radio Frequency)?", "id": "Melewatkan frekuensi di atas ambang batas." },
  { "en": "Apa itu sertifikasi Wi-Fi TimeSync?", "id": "Untuk sinkronisasi waktu yang akurat." },
  { "en": "Apa itu sertifikasi Wi-Fi Optimized Connectivity?", "id": "Untuk koneksi dan roaming yang cepat." },
  { "en": "Apa fungsi perintah 'netsh wlan show drivers'?", "id": "Menampilkan kapabilitas driver WIFI." },
  { "en": "Apa itu C-V2X (Cellular Vehicle-to-Everything)?", "id": "Komunikasi kendaraan berbasis teknologi seluler." },
  { "en": "Apa itu ISI (Intersymbol Interference)?", "id": "Gangguan antar simbol transmisi data." },
  { "en": "Bagaimana OFDM (Orthogonal Frequency-Division Multiplexing) mengatasi ISI (Intersymbol Interference)?", "id": "Menggunakan guard interval." },
  { "en": "Kapan EIFS (Extended Interframe Space) digunakan?", "id": "Setelah menerima frame yang rusak." },
  { "en": "Bagaimana AIFS (Arbitration Interframe Space) digunakan dalam QoS (Quality of Service)?", "id": "Memberikan prioritas akses yang berbeda." },
  { "en": "Apa itu proxy RADIUS (Remote Authentication Dial-In User Service)?", "id": "Meneruskan permintaan RADIUS ke server lain." },
  { "en": "Apa itu kamus RADIUS (Remote Authentication Dial-In User Service)?", "id": "File yang mendefinisikan atribut vendor." },
  { "en": "Apa itu MDM (Mobile Device Management)?", "id": "Perangkat lunak untuk mengelola perangkat seluler." },
  { "en": "Bagaimana MDM (Mobile Device Management) berhubungan dengan BYOD (Bring Your Own Device)?", "id": "MDM mengamankan perangkat dalam kebijakan BYOD." },
  { "en": "Apa itu duplexer?", "id": "Memungkinkan transmisi dan penerimaan simultan." },
  { "en": "Apa beda diplexer dan duplexer?", "id": "Diplexer berbasis frekuensi, duplexer arah." },
  { "en": "Apa itu sertifikasi Wi-Fi Aware?", "id": "Memungkinkan perangkat saling menemukan." },
  { "en": "Apa yang diaktifkan oleh Wi-Fi Aware?", "id": "Aplikasi berbasis kedekatan dan peer-to-peer." },
  { "en": "Apa fungsi perintah 'netsh wlan show networks mode=bssid'?", "id": "Menampilkan semua AP yang terlihat." },
  { "en": "Untuk apa daftar BSSID (Basic Service Set Identifier) berguna?", "id": "Menganalisis kepadatan dan cakupan AP." },
  { "en": "Apa itu Doppler shift di RF (Radio Frequency)?", "id": "Perubahan frekuensi akibat gerakan." },
  { "en": "Apa itu equalizer di penerima digital?", "id": "Sirkuit untuk mengkompensasi distorsi kanal." },
  { "en": "Apa arti bit 'Order' di header frame?", "id": "Memastikan frame tidak diproses ulang." },
  { "en": "Apa itu VSA (Vendor-Specific Attribute) di RADIUS (Remote Authentication Dial-In User Service)?", "id": "Atribut kustom yang dibuat oleh vendor." },
  { "en": "Apa itu EAP-TTLS (Extensible Authentication Protocol-Tunneled Transport Layer Security)?", "id": "Metode EAP yang membuat tunnel aman." },
  { "en": "Apa itu otentikasi fase 2 di EAP-TTLS (Extensible Authentication Protocol-Tunneled Transport Layer Security)?", "id": "Otentikasi pengguna di dalam tunnel." },
  { "en": "Bagaimana geofencing bisa digunakan dengan NAC (Network Access Control)?", "id": "Memberikan akses berdasarkan lokasi fisik." },
  { "en": "Apa itu AGC (Automatic Gain Control)?", "id": "Menyesuaikan gain penerima secara otomatis." },
  { "en": "Apa itu sertifikasi Wi-Fi Data Elements?", "id": "Untuk standarisasi pengumpulan data diagnostik." },
  { "en": "Apa fungsi perintah 'netsh wlan show all'?", "id": "Menampilkan laporan lengkap WIFI." },
  { "en": "Apa itu 'profil' dalam output netsh?", "id": "Jaringan WIFI yang disimpan." },
  { "en": "Apa itu RAKE receiver?", "id": "Penerima yang menggabungkan sinyal multipath." },
  { "en": "Apa itu frame NDPA (Null Data Packet Announcement)?", "id": "Mengumumkan transmisi sounding yang akan datang." },
  { "en": "Apa arti bit 'Protected Frame'?", "id": "Frame ini dienkripsi." },
  { "en": "Apa itu akuntansi RADIUS (Remote Authentication Dial-In User Service)?", "id": "Melacak penggunaan sumber daya oleh pengguna." },
  { "en": "Apa itu paket RADIUS (Remote Authentication Dial-In User Service) accounting-start?", "id": "Menandai awal sesi pengguna." },
  { "en": "Apa itu paket RADIUS (Remote Authentication Dial-In User Service) accounting-stop?", "id": "Menandai akhir sesi pengguna." },
  { "en": "Apa itu MAM (Mobile Application Management)?", "id": "Mengelola dan mengamankan aplikasi perusahaan." },
  { "en": "Apa itu attenuator RF (Radio Frequency)?", "id": "Komponen yang mengurangi kekuatan sinyal." },
  { "en": "Apa itu amplifier RF (Radio Frequency)?", "id": "Komponen yang meningkatkan kekuatan sinyal." },
  { "en": "Apa itu sertifikasi Wi-Fi Easy Connect?", "id": "Untuk onboarding perangkat yang aman." },
  { "en": "Bagaimana cara kerja Wi-Fi Easy Connect?", "id": "Menggunakan kode QR untuk konfigurasi." },
  { "en": "Apa fungsi 'netsh wlan export profile'?", "id": "Menyimpan profil WIFI ke file XML." },
  { "en": "Apa itu layanan WLAN AutoConfig di Windows?", "id": "Mengelola koneksi nirkabel." },
  { "en": "Apa itu root bridge dalam pengaturan jembatan nirkabel?", "id": "Jembatan utama yang terhubung ke LAN." },
  { "en": "Apa itu non-root bridge?", "id": "Jembatan sekunder yang terhubung ke root." },
  { "en": "Apa itu STP (Spanning Tree Protocol)?", "id": "Protokol untuk mencegah loop di jaringan." },
  { "en": "Apakah STP (Spanning Tree Protocol) digunakan di WIFI (Wireless Fidelity) mesh?", "id": "Tidak, mesh menggunakan protokol routing sendiri." },
  { "en": "Apa itu 'wireless backhaul'?", "id": "Koneksi nirkabel antar node jaringan." },
  { "en": "Apa itu 'point-to-point' link?", "id": "Koneksi langsung antara dua titik." },
  { "en": "Apa itu 'point-to-multipoint' link?", "id": "Satu titik terhubung ke banyak titik." },
  { "en": "Apa itu 'wireless repeater'?", "id": "Memperluas jangkauan sinyal nirkabel." },
  { "en": "Apa itu 'wireless bridge mode'?", "id": "Menghubungkan dua segmen jaringan kabel." },
  { "en": "Apa itu 'client bridge mode'?", "id": "Menghubungkan perangkat kabel ke WIFI." },
  { "en": "Apa itu 'WIFI signal meter'?", "id": "Alat untuk mengukur kekuatan sinyal." },
  { "en": "Apa itu 'RF site survey'?", "id": "Analisis lingkungan RF untuk perencanaan." },
  { "en": "Apa itu 'channel planning'?", "id": "Merencanakan penggunaan kanal untuk minimalisasi interferensi." },
  { "en": "Apa itu 'power level setting'?", "id": "Mengatur daya pancar access point." },
  { "en": "Apa itu 'antenna selection'?", "id": "Memilih antena yang tepat untuk aplikasi." },
  { "en": "Apa itu 'spectrum analysis'?", "id": "Menganalisis penggunaan spektrum frekuensi radio." },
  { "en": "Apa perbedaan utama GCMP (Galois/Counter Mode Protocol) dan CCMP (Counter Mode Cipher Block Chaining Message Authentication Code Protocol)?", "id": "GCMP lebih efisien dan lebih cepat." },
  { "en": "Apa itu CNSA (Commercial National Security Algorithm) suite?", "id": "Standar kriptografi untuk data rahasia AS." },
  { "en": "Apa itu mode transisi WPA3 (Wi-Fi Protected Access 3)?", "id": "Memungkinkan koneksi WPA2 dan WPA3 bersamaan." },
  { "en": "Apa itu PAKE (Password Authenticated Key Exchange)?", "id": "Protokol pertukaran kunci berbasis kata sandi." },
  { "en": "Apa itu TBER (Target Bit Error Rate)?", "id": "Tingkat kesalahan bit yang ditargetkan." },
  { "en": "Apa itu SGI (Short Guard Interval)?", "id": "Mengurangi jeda antar simbol transmisi." },
  { "en": "Apa itu LGI (Long Guard Interval)?", "id": "Jeda antar simbol transmisi standar." },
  { "en": "Apa itu kanal 320 MHz?", "id": "Lebar kanal sangat lebar di WIFI 7." },
  { "en": "Standar mana yang memperkenalkan kanal 320 MHz?", "id": "Standar WIFI 7 (802.11be)." },
  { "en": "Apa itu Multi-RU (Resource Unit)?", "id": "Mengalokasikan beberapa RU ke satu pengguna." },
  { "en": "Apa itu CAPWAP (Control and Provisioning of Wireless Access Points)?", "id": "Protokol tunnel antara AP dan kontroler." },
  { "en": "Apa itu arsitektur Split MAC (Media Access Control)?", "id": "Fungsi MAC dibagi antara AP dan kontroler." },
  { "en": "Apa itu arsitektur Local MAC (Media Access Control)?", "id": "Semua fungsi MAC ditangani oleh AP." },
  { "en": "Apa itu LWAPP (Lightweight Access Point Protocol)?", "id": "Pendahulu dari protokol CAPWAP." },
  { "en": "Apa itu band UNII-1 (Unlicensed National Information Infrastructure)?", "id": "Blok 5 GHz untuk penggunaan dalam ruangan." },
  { "en": "Apa itu band UNII-2 (Unlicensed National Information Infrastructure)?", "id": "Blok 5 GHz yang memerlukan DFS." },
  { "en": "Apa itu band UNII-3 (Unlicensed National Information Infrastructure)?", "id": "Blok 5 GHz untuk penggunaan daya lebih tinggi." },
  { "en": "Apa itu band UNII-4 (Unlicensed National Information Infrastructure)?", "id": "Blok spektrum baru di 5.9 GHz." },
  { "en": "Apa itu ITU (International Telecommunication Union)?", "id": "Badan PBB untuk teknologi informasi." },
  { "en": "Apa itu EVM (Error Vector Magnitude)?", "id": "Ukuran akurasi sinyal modulasi." },
  { "en": "Apa yang ditunjukkan oleh nilai EVM (Error Vector Magnitude) yang tinggi?", "id": "Kualitas sinyal modulasi yang buruk." },
  { "en": "Apa itu ACPR (Adjacent Channel Power Ratio)?", "id": "Ukuran kebocoran daya ke kanal sebelah." },
  { "en": "Apa itu radiator isotropis?", "id": "Antena teoretis yang memancar seragam." },
  { "en": "Apa itu antena dipol setengah gelombang?", "id": "Antena referensi dasar dengan gain 2.15 dBi." },
  { "en": "Apa itu pencocokan impedansi?", "id": "Menyesuaikan impedansi untuk transfer daya maksimal." },
  { "en": "Apa itu balun?", "id": "Mengubah sinyal seimbang menjadi tidak seimbang." },
  { "en": "Apa itu NGH (Next Generation Hotspot)?", "id": "Inisiatif untuk menyederhanakan akses hotspot." },
  { "en": "Apa itu Wi-Fi SON (Self-Organizing Network)?", "id": "Jaringan yang mengoptimalkan dirinya sendiri." },
  { "en": "Apa itu mode transisi OWE (Opportunistic Wireless Encryption)?", "id": "Mendukung klien OWE dan non-OWE." },
  { "en": "Apa itu DPP (Device Provisioning Protocol)?", "id": "Protokol untuk onboarding perangkat IoT." },
  { "en": "Apa itu field HE-LTF (High Efficiency Long Training Field)?", "id": "Digunakan untuk estimasi kanal di WIFI 6." },
  { "en": "Apa itu field HE-SIG-A (High Efficiency Signal A)?", "id": "Berisi informasi umum transmisi WIFI 6." },
  { "en": "Apa itu field HE-SIG-B (High Efficiency Signal B)?", "id": "Berisi informasi alokasi pengguna." },
  { "en": "Apa itu SRG (Spatial Reuse Group)?", "id": "Kelompok AP yang dapat berbagi spektrum." },
  { "en": "Apa itu OBSS (Overlapping Basic Service Set)?", "id": "BSS yang tumpang tindih secara fisik." },
  { "en": "Apa itu OBSS PD (Overlapping BSS Preamble Detection)?", "id": "Mendeteksi transmisi dari BSS yang tumpang tindih." },
  { "en": "Apa itu broadcast TWT (Target Wake Time)?", "id": "TWT yang dikirim ke banyak perangkat." },
  { "en": "Apa itu DCM (Dual Carrier Modulation)?", "id": "Mengirim data yang sama pada dua subcarrier." },
  { "en": "Apa tujuan dari DCM (Dual Carrier Modulation)?", "id": "Meningkatkan keandalan sinyal." },
  { "en": "Apa itu HE-ER-SU (High Efficiency Extended Range Single User) PPDU?", "id": "Format frame untuk jangkauan lebih jauh." },
  { "en": "Apa itu coding rate?", "id": "Rasio bit data terhadap total bit." },
  { "en": "Apa itu M-BSSID (Multiple Basic Service Set Identifier)?", "id": "Menyiarkan banyak SSID dari satu radio." },
  { "en": "Apa itu lapisan data link?", "id": "Lapisan 2 dalam model OSI." },
  { "en": "Di lapisan mana WIFI (Wireless Fidelity) beroperasi?", "id": "Lapisan 1 (Fisik) dan 2 (Data Link)." },
  { "en": "Apa itu WMN (Wireless Mesh Network)?", "id": "Jaringan yang dibentuk oleh node mesh." },
  { "en": "Apa itu MP (Mesh Point)?", "id": "Sebuah node dalam jaringan mesh." },
  { "en": "Apa itu MPP (Mesh Portal)?", "id": "MP yang terhubung ke jaringan lain." },
  { "en": "Apa itu HWMP (Hybrid Wireless Mesh Protocol)?", "id": "Protokol routing default untuk 802.11s." },
  { "en": "Apa itu antarmuka udara (air interface)?", "id": "Spesifikasi komunikasi nirkabel." },
  { "en": "Apa itu sel dalam jaringan nirkabel?", "id": "Area cakupan dari satu AP." },
  { "en": "Apa itu femtocell?", "id": "AP berdaya sangat rendah untuk rumah." },
  { "en": "Apa itu picocell?", "id": "AP berdaya rendah untuk area kecil." },
  { "en": "Apa itu microcell?", "id": "AP untuk cakupan area terbatas." },
  { "en": "Apa itu macrocell?", "id": "Sel besar yang mencakup area luas." },
  { "en": "Apa itu DAS (Distributed Antenna System)?", "id": "Jaringan antena terpisah untuk cakupan." },
  { "en": "Bagaimana DAS (Distributed Antenna System) meningkatkan WIFI (Wireless Fidelity)?", "id": "Memberikan cakupan merata di area besar." },
  { "en": "Apa itu kabel leaky feeder?", "id": "Kabel koaksial yang memancarkan sinyal." },
  { "en": "Di mana leaky feeder digunakan untuk WIFI (Wireless Fidelity)?", "id": "Di terowongan, tambang, atau kereta bawah tanah." },
  { "en": "Apa itu frame PS-Poll (Power Save-Poll)?", "id": "Klien meminta data buffered dari AP." },
  { "en": "Kapan frame PS-Poll (Power Save-Poll) digunakan?", "id": "Saat klien 'bangun' dari mode hemat daya." },
  { "en": "Apa itu siaran TIM (Traffic Indication Map)?", "id": "AP memberitahu klien data sedang menunggu." },
  { "en": "Apa itu siaran DTIM (Delivery Traffic Indication Message)?", "id": "Memberitahu klien data broadcast sedang menunggu." },
  { "en": "Apa itu status tidur (sleep state) klien?", "id": "Klien menonaktifkan radio untuk hemat daya." },
  { "en": "Apa itu status bangun (awake state) klien?", "id": "Klien mengaktifkan radio untuk berkomunikasi." },
  { "en": "Apa isi amandemen IEEE (Institute of Electrical and Electronics Engineers) 802.11w?", "id": "Perlindungan Frame Manajemen (PMF)." },
  { "en": "Apa isi amandemen IEEE (Institute of Electrical and Electronics Engineers) 802.11s?", "id": "Jaringan Mesh Nirkabel." },
  { "en": "Apa isi amandemen IEEE (Institute of Electrical and Electronics Engineers) 802.11aa?", "id": "Streaming Transport Video." },
  { "en": "Apa itu PSDU (PLCP Service Data Unit)?", "id": "Data dari lapisan MAC ke lapisan PHY." },
  { "en": "Apa itu PPDU (PLCP Protocol Data Unit)?", "id": "Sebuah frame di lapisan fisik." },
  { "en": "Apa itu scrambling seed?", "id": "Nilai awal untuk proses scrambling." },
  { "en": "Apa itu TCP (Transmission Control Protocol)?", "id": "Protokol transport yang andal dan berorientasi koneksi." },
  { "en": "Apa itu UDP (User Datagram Protocol)?", "id": "Protokol transport yang cepat dan tanpa koneksi." },
  { "en": "Lalu lintas WIFI (Wireless Fidelity) biasanya TCP (Transmission Control Protocol) atau UDP (User Datagram Protocol)?", "id": "Keduanya umum digunakan." },
  { "en": "Mana yang lebih baik untuk streaming, TCP (Transmission Control Protocol) atau UDP (User Datagram Protocol)?", "id": "UDP karena latensinya lebih rendah." },
  { "en": "Apa itu jitter buffer?", "id": "Menyimpan paket untuk mengurangi efek jitter." },
  { "en": "Apa itu MOS (Mean Opinion Score)?", "id": "Ukuran subyektif kualitas panggilan suara." },
  { "en": "Apa yang diukur oleh MOS (Mean Opinion Score)?", "id": "Persepsi pengguna terhadap kualitas panggilan." },
  { "en": "Berapa skor MOS (Mean Opinion Score) yang baik?", "id": "Di atas 4.0." },
  { "en": "Apa itu 'warchalking'?", "id": "Menggambar simbol untuk menandai jaringan WIFI." },
  { "en": "Apa itu 'fox hunt' dalam konteks nirkabel?", "id": "Proses menemukan lokasi perangkat nirkabel." },
  { "en": "Apa itu peta panas (heat map) survei situs?", "id": "Visualisasi kekuatan sinyal di atas denah." },
  { "en": "Apa itu survei situs aktif?", "id": "Mengukur kinerja dengan terhubung ke AP." },
  { "en": "Apa itu survei situs pasif?", "id": "Mendengarkan lalu lintas RF tanpa terhubung." },
  { "en": "Apa itu survei situs prediktif?", "id": "Mensimulasikan cakupan RF menggunakan perangkat lunak." },
  { "en": "Apa itu 'channel overlap'?", "id": "Tumpang tindih frekuensi antar kanal." },
  { "en": "Apa itu 'wireless client isolation'?", "id": "Mencegah klien berkomunikasi satu sama lain." },
  { "en": "Apa itu 'wireless scheduler'?", "id": "Menonaktifkan WIFI pada waktu yang ditentukan." },
  { "en": "Apa itu 'transmit power control'?", "id": "Menyesuaikan daya pancar router." },
  { "en": "Apa itu 'rate limiting'?", "id": "Membatasi bandwidth untuk pengguna atau aplikasi." },
  { "en": "Apa itu 'traffic shaping'?", "id": "Mengontrol lalu lintas jaringan untuk optimalisasi." },
  { "en": "Apa itu 'packet sniffing'?", "id": "Menangkap dan menganalisis paket data." },
  { "en": "Apa itu 'port scanning'?", "id": "Memindai port terbuka pada perangkat." },
  { "en": "Apa itu 'SSID spoofing'?", "id": "Membuat jaringan palsu dengan SSID sama." },
  { "en": "Apa itu 'deauthentication attack'?", "id": "Memutuskan paksa koneksi klien." },
  { "en": "Apa itu 'rogue access point'?", "id": "AP yang tidak sah di jaringan." },
  { "en": "Apa itu 'misconfigured access point'?", "id": "AP dengan pengaturan keamanan yang salah." },
  { "en": "Apa itu 'ad-hoc network detection'?", "id": "Mendeteksi jaringan peer-to-peer yang tidak sah." },
  { "en": "Apa isi field LENGTH di PLCP (Physical Layer Convergence Procedure)?", "id": "Panjang data dalam satuan byte." },
  { "en": "Apa isi field RATE di PLCP (Physical Layer Convergence Procedure)?", "id": "Kecepatan transmisi data." },
  { "en": "Apa isi field SERVICE di PLCP (Physical Layer Convergence Procedure)?", "id": "Disediakan untuk penggunaan di masa depan." },
  { "en": "Apa itu tail bits dalam sebuah frame?", "id": "Bit untuk mengembalikan encoder ke nol." },
  { "en": "Apa itu padding dalam frame WIFI (Wireless Fidelity)?", "id": "Bit tambahan untuk memenuhi panjang minimum." },
  { "en": "Apa itu subframe A-MSDU (Aggregate MAC Service Data Unit)?", "id": "Satu MSDU di dalam frame agregat." },
  { "en": "Apa itu densitas MPDU (MAC Protocol Data Unit)?", "id": "Waktu minimum antar MPDU dalam A-MPDU." },
  { "en": "Apa itu kerentanan nonce reuse?", "id": "Menggunakan kembali nomor acak dalam kriptografi." },
  { "en": "Serangan mana yang mengeksploitasi nonce reuse?", "id": "Serangan KRACK pada WPA2." },
  { "en": "Apa itu PMKSA (Pairwise Master Key Security Association) caching?", "id": "Menyimpan PMK untuk roaming cepat." },
  { "en": "Apa itu TR-069?", "id": "Protokol manajemen perangkat jarak jauh." },
  { "en": "Apa itu CWMP (CPE WAN Management Protocol)?", "id": "Nama teknis untuk protokol TR-069." },
  { "en": "Apa itu tunnel GRE (Generic Routing Encapsulation)?", "id": "Membungkus satu protokol di dalam protokol lain." },
  { "en": "Bagaimana tunnel GRE (Generic Routing Encapsulation) digunakan dengan WIFI (Wireless Fidelity)?", "id": "Membawa lalu lintas tamu ke kontroler." },
  { "en": "Apa itu Noise Figure (NF)?", "id": "Ukuran degradasi SNR oleh komponen." },
  { "en": "Apa itu IMD (Intermodulation Distortion)?", "id": "Sinyal palsu yang dibuat oleh non-linearitas." },
  { "en": "Untuk apa Smith Chart digunakan?", "id": "Analisis grafis masalah impedansi RF." },
  { "en": "Bagaimana cara mendekripsi lalu lintas WPA2 (Wi-Fi Protected Access 2) di Wireshark?", "id": "Dengan memberikan kata sandi WIFI." },
  { "en": "Apa filter tampilan Wireshark yang umum untuk WIFI (Wireless Fidelity)?", "id": "Filter 'wlan.fc.type_subtype'." },
  { "en": "Apa itu standar IEEE (Institute of Electrical and Electronics Engineers) 802.11ay?", "id": "Peningkatan standar 60 GHz (WiGig)." },
  { "en": "Frekuensi apa yang digunakan oleh 802.11ay?", "id": "Band frekuensi 60 GHz." },
  { "en": "Apa itu standar IEEE (Institute of Electrical and Electronics Engineers) 802.11cs?", "id": "Standar untuk komunikasi daya rendah." },
  { "en": "Bagaimana WIFI (Wireless Fidelity) digunakan untuk drone?", "id": "Untuk kontrol, telemetri, dan video." },
  { "en": "Apa itu WMTS (Wireless Medical Telemetry Service)?", "id": "Spektrum khusus untuk perangkat medis." },
  { "en": "Apa itu IWLAN (Industrial Wireless LAN)?", "id": "WLAN yang dirancang untuk lingkungan industri." },
  { "en": "Apa itu program Wi-Fi CERTIFIED Miracast™?", "id": "Sertifikasi untuk streaming video nirkabel." },
  { "en": "Apa itu program Wi-Fi CERTIFIED Passpoint®?", "id": "Sertifikasi untuk koneksi hotspot otomatis." },
  { "en": "Apa itu frasa sandi ASCII (American Standard Code for Information Interchange)?", "id": "Kata sandi menggunakan karakter standar." },
  { "en": "Apa itu frasa sandi Heksadesimal?", "id": "Kata sandi menggunakan 64 karakter heksadesimal." },
  { "en": "Apa arti tanda CE (Conformité Européenne)?", "id": "Produk memenuhi standar keselamatan Uni Eropa." },
  { "en": "Apa arti logo FCC (Federal Communications Commission)?", "id": "Produk mematuhi peraturan FCC AS." },
  { "en": "Apa itu system integrator?", "id": "Perusahaan yang merancang dan membangun sistem." },
  { "en": "Apa itu CCA (Clear Channel Assessment)?", "id": "Mekanisme untuk memeriksa apakah media sibuk." },
  { "en": "Apa itu Energy Detect (ED) di CCA (Clear Channel Assessment)?", "id": "Mendeteksi energi RF di atas ambang batas." },
  { "en": "Apa itu Carrier Sense (CS) di CCA (Clear Channel Assessment)?", "id": "Mendeteksi sinyal preamble WIFI." },
  { "en": "Apa itu short retry limit?", "id": "Batas transmisi ulang untuk frame pendek." },
  { "en": "Apa itu long retry limit?", "id": "Batas transmisi ulang untuk frame panjang." },
  { "en": "Apa itu pairwise cipher suite?", "id": "Algoritma enkripsi untuk lalu lintas unicast." },
  { "en": "Apa itu group cipher suite?", "id": "Algoritma enkripsi untuk lalu lintas broadcast." },
  { "en": "Apa itu rotasi kunci broadcast?", "id": "Mengubah kunci grup secara berkala." },
  { "en": "Apa peran dari authenticator?", "id": "Entitas yang mengizinkan akses jaringan." },
  { "en": "Apa peran dari supplicant?", "id": "Entitas yang meminta akses jaringan." },
  { "en": "Apa itu port terkontrol?", "id": "Port virtual yang mengizinkan akses penuh." },
  { "en": "Apa itu port tidak terkontrol?", "id": "Port virtual untuk lalu lintas otentikasi." },
  { "en": "Apa itu pesan EAPOL-Start (Extensible Authentication Protocol over LAN)?", "id": "Memulai proses otentikasi 802.1X." },
  { "en": "Apa itu pesan EAPOL-Key (Extensible Authentication Protocol over LAN)?", "id": "Digunakan untuk mengirim kunci enkripsi." },
  { "en": "Apa itu pesan EAPOL-Logoff (Extensible Authentication Protocol over LAN)?", "id": "Mengakhiri sesi 802.1X." },
  { "en": "Apa itu laporan kegagalan MIC (Message Integrity Check)?", "id": "Laporan yang dikirim saat MIC gagal." },
  { "en": "Apa itu tindakan balasan TKIP (Temporal Key Integrity Protocol)?", "id": "Menonaktifkan koneksi setelah kegagalan MIC." },
  { "en": "Apa itu IETF (Internet Engineering Task Force)?", "id": "Organisasi yang mengembangkan standar internet." },
  { "en": "Apa itu RFC (Request for Comments)?", "id": "Publikasi dari IETF." },
  { "en": "Apa itu WLAN (Wireless Local Area Network) switch?", "id": "Nama lain untuk kontroler nirkabel." },
  { "en": "Apa itu WLAN (Wireless Local Area Network) gateway?", "id": "Menghubungkan WLAN ke jaringan lain." },
  { "en": "Apa itu AP (Access Point) Virtual?", "id": "Beberapa SSID pada satu AP fisik." },
  { "en": "Apa itu siaran SSID (Service Set Identifier)?", "id": "AP menyiarkan namanya di beacon." },
  { "en": "Apa itu respons probe?", "id": "Jawaban AP terhadap permintaan probe." },
  { "en": "Apa itu respons asosiasi?", "id": "Jawaban AP terhadap permintaan asosiasi." },
  { "en": "Apa itu respons reasosiasi?", "id": "Jawaban AP terhadap permintaan reasosiasi." },
  { "en": "Apa itu frame null function?", "id": "Frame tanpa data, untuk tujuan kontrol." },
  { "en": "Apa itu ATIM (Announcement Traffic Indication Message) window?", "id": "Jendela waktu dalam mode ad-hoc." },
  { "en": "Kapan ATIM (Announcement Traffic Indication Message) window digunakan?", "id": "Dalam mode hemat daya ad-hoc." },
  { "en": "Apa itu CW (Contention Window)?", "id": "Rentang waktu untuk backoff acak." },
  { "en": "Apa itu CWMin (Contention Window Minimum)?", "id": "Ukuran minimum dari contention window." },
  { "en": "Apa itu CWMax (Contention Window Maximum)?", "id": "Ukuran maksimum dari contention window." },
  { "en": "Apa itu algoritma exponential backoff?", "id": "Menggandakan CW setelah setiap tabrakan." },
  { "en": "Apa itu SME (Station Management Entity)?", "id": "Entitas yang mengelola konektivitas stasiun." },
  { "en": "Apa itu MLME (MAC Layer Management Entity)?", "id": "Mengelola status dan operasi lapisan MAC." },
  { "en": "Apa itu PLME (PHY Layer Management Entity)?", "id": "Mengelola status dan operasi lapisan PHY." },
  { "en": "Apa itu MIB (Management Information Base)?", "id": "Database variabel perangkat yang dikelola." },
  { "en": "Apa itu trap SNMP (Simple Network Management Protocol)?", "id": "Pemberitahuan asinkron dari agen SNMP." },
  { "en": "Apa itu program Wi-Fi CERTIFIED Enhanced Open™?", "id": "Sertifikasi untuk enkripsi di jaringan terbuka." },
  { "en": "Apa yang disediakan oleh Enhanced Open?", "id": "Enkripsi data tanpa otentikasi." },
  { "en": "Apa itu program Wi-Fi CERTIFIED WPA3™?", "id": "Sertifikasi untuk perangkat yang mendukung WPA3." },
  { "en": "Apa itu program Wi-Fi CERTIFIED 6™?", "id": "Sertifikasi untuk perangkat yang mendukung 802.11ax." },
  { "en": "Apa itu testbed dalam sertifikasi Wi-Fi?", "id": "Satu set produk referensi." },
  { "en": "Apa itu SAP (Service Access Point) PHY (Physical Layer)?", "id": "Titik di mana PHY menyediakan layanan." },
  { "en": "Apa itu SAP (Service Access Point) MAC (Media Access Control)?", "id": "Titik di mana MAC menyediakan layanan." },
  { "en": "Apa itu LLC (Logical Link Control)?", "id": "Sublapisan atas dari lapisan data link." },
  { "en": "Apa itu header SNAP (Subnetwork Access Protocol)?", "id": "Ekstensi untuk LLC." },
  { "en": "Apa itu DSS (Distributed System Service)?", "id": "Layanan yang disediakan oleh sistem distribusi." },
  { "en": "Apa itu layanan integrasi?", "id": "Menghubungkan WLAN ke jaringan kabel." },
  { "en": "Apa itu DSM (Distribution System Medium)?", "id": "Media yang digunakan untuk menghubungkan AP." },
  { "en": "Apa itu WDS (Wireless Distribution System)?", "id": "Menggunakan nirkabel sebagai medium sistem distribusi." },
  { "en": "Apa itu SS (Station Service)?", "id": "Layanan yang disediakan di dalam BSS." },
  { "en": "Apa itu layanan otentikasi?", "id": "Layanan untuk memverifikasi identitas stasiun." },
  { "en": "Apa itu layanan deautentikasi?", "id": "Layanan untuk mengakhiri status terotentikasi." },
  { "en": "Apa itu layanan privasi?", "id": "Layanan untuk mengenkripsi data." },
  { "en": "Apa itu layanan pengiriman MSDU (MAC Service Data Unit)?", "id": "Layanan untuk mentransfer data." },
  { "en": "Apa itu 'wireless client driver'?", "id": "Perangkat lunak yang mengoperasikan adaptor WIFI." },
  { "en": "Apa itu 'firmware upgrade'?", "id": "Memperbarui perangkat lunak internal router." },
  { "en": "Apa itu 'channel bandwidth'?", "id": "Lebar spektrum yang digunakan kanal." },
  { "en": "Apa itu 'SSID length limit'?", "id": "Batas karakter untuk nama SSID." },
  { "en": "Apa itu 'WPA passphrase'?", "id": "Kata sandi untuk jaringan WPA." },
  { "en": "Apa itu 'packet capture'?", "id": "Merekam lalu lintas jaringan untuk analisis." },
  { "en": "Apa itu 'RF spectrum analyzer'?", "id": "Alat untuk memvisualisasikan spektrum radio." },
  { "en": "Apa itu 'network troubleshooting'?", "id": "Proses mendiagnosis dan memperbaiki masalah." },
  { "en": "Apa format kartu WIFI (Wireless Fidelity) awal yang umum?", "id": "PCMCIA (Personal Computer Memory Card International Association)." },
  { "en": "Apa batasan utama WDS (Wireless Distribution System)?", "id": "Memotong throughput hingga setengahnya." },
  { "en": "Apa itu resiprositas antena?", "id": "Sifat antena bekerja sama baiknya." },
  { "en": "Bagaimana EIRP (Effective Isotropic Radiated Power) dihitung?", "id": "Daya TX ditambah gain antena." },
  { "en": "Rangkum keamanan WEP (Wired Equivalent Privacy).", "id": "Sangat tidak aman, mudah diretas." },
  { "en": "Rangkum keamanan WPA (Wi-Fi Protected Access).", "id": "Lebih baik dari WEP, tapi rentan." },
  { "en": "Rangkum keamanan WPA2 (Wi-Fi Protected Access 2).", "id": "Aman untuk penggunaan umum." },
  { "en": "Rangkum keamanan WPA3 (Wi-Fi Protected Access 3).", "id": "Standar keamanan modern paling kuat." },
  { "en": "Kapan sebaiknya menggunakan keamanan PSK (Pre-Shared Key)?", "id": "Untuk jaringan rumah dan kantor kecil." },
  { "en": "Kapan sebaiknya menggunakan keamanan 802.1X?", "id": "Untuk jaringan perusahaan besar." },
  { "en": "Masalah apa yang dipecahkan oleh OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Efisiensi di lingkungan banyak pengguna." },
  { "en": "Masalah apa yang dipecahkan oleh TWT (Target Wake Time)?", "id": "Daya tahan baterai perangkat IoT." },
  { "en": "Masalah apa yang dipecahkan oleh BSS (Basic Service Set) Coloring?", "id": "Interferensi co-channel di lingkungan padat." },
  { "en": "Apa fungsi 'Lupakan Jaringan Ini'?", "id": "Menghapus profil dan kata sandi jaringan." },
  { "en": "Apa sumber interferensi 2.4 GHz di rumah?", "id": "Microwave, telepon nirkabel, Bluetooth." },
  { "en": "Apa praktik terbaik untuk perencanaan kanal 2.4 GHz?", "id": "Gunakan kanal 1, 6, dan 11." },
  { "en": "Apa saja langkah-langkah dalam proses asosiasi WIFI (Wireless Fidelity)?", "id": "Probe, otentikasi, dan asosiasi." },
  { "en": "Apa tujuan utama dari Wi-Fi Agile Multiband?", "id": "Memanfaatkan spektrum dengan lebih cerdas." },
  { "en": "Apa tujuan utama dari Wi-Fi Optimized Connectivity?", "id": "Meningkatkan pengalaman roaming nirkabel." },
  { "en": "Di lapisan OSI (Open Systems Interconnection) mana WIFI (Wireless Fidelity) beroperasi?", "id": "Lapisan 1 (Fisik) dan 2 (Data Link)." },
  { "en": "Apa tujuan sublapisan LLC (Logical Link Control)?", "id": "Mengontrol aliran data antar perangkat." },
  { "en": "Bandingkan WIFI (Wireless Fidelity) dan Zigbee untuk IoT (Internet of Things).", "id": "WIFI kecepatan tinggi, Zigbee daya rendah." },
  { "en": "Bandingkan WIFI (Wireless Fidelity) dan Bluetooth LE (Low Energy).", "id": "WIFI jangkauan jauh, Bluetooth LE irit daya." },
  { "en": "Apa itu ACMA (Australian Communications and Media Authority)?", "id": "Badan regulator komunikasi Australia." },
  { "en": "Apa itu persistensi sesi saat roaming?", "id": "Menjaga sesi aplikasi tetap aktif." },
  { "en": "Apa itu agresivitas roaming?", "id": "Seberapa cepat klien memutuskan untuk roaming." },
  { "en": "Apa itu preferensi band?", "id": "Mendorong klien untuk menggunakan band tertentu." },
  { "en": "Apa itu frame CF-End (Contention-Free End)?", "id": "Mengakhiri periode bebas kontensi." },
  { "en": "Apa itu Point Coordinator (PC)?", "id": "Fungsi dalam AP yang mengontrol PCF." },
  { "en": "Apa itu Hybrid Coordinator (HC)?", "id": "Fungsi dalam AP yang mengontrol HCF." },
  { "en": "Apa itu elemen QBSS (Quality of Service Basic Service Set) Load?", "id": "Memberikan informasi tentang kesibukan AP." },
  { "en": "Apa arti utilisasi kanal yang tinggi?", "id": "Kanal sangat sibuk, bisa lambat." },
  { "en": "Apa itu admission control di QoS (Quality of Service)?", "id": "Menentukan apakah koneksi baru diizinkan." },
  { "en": "Apa itu TSPEC (Traffic Specification)?", "id": "Permintaan dari klien untuk sumber daya QoS." },
  { "en": "Apa itu slot time?", "id": "Satuan waktu dasar dalam akses media." },
  { "en": "Apa itu tabrakan virtual di WIFI (Wireless Fidelity)?", "id": "Saat timer NAV mencegah transmisi." },
  { "en": "Apa itu tabrakan fisik di WIFI (Wireless Fidelity)?", "id": "Dua perangkat mengirim data bersamaan." },
  { "en": "Apa itu otentikasi Open System?", "id": "Tidak ada otentikasi, siapa pun bisa terhubung." },
  { "en": "Apa itu otentikasi Shared Key?", "id": "Otentikasi menggunakan kunci WEP statis." },
  { "en": "Apakah otentikasi Shared Key aman?", "id": "Tidak, metode ini sangat tidak aman." },
  { "en": "Apa itu status code dalam frame respons?", "id": "Menunjukkan keberhasilan atau kegagalan permintaan." },
  { "en": "Apa arti status code 0?", "id": "Permintaan berhasil tanpa kesalahan." },
  { "en": "Apa yang ditunjukkan oleh field Reason Code?", "id": "Alasan untuk deautentikasi atau disasosiasi." },
  { "en": "Apa arti reason code 1?", "id": "Alasan tidak ditentukan (unspecified)." },
  { "en": "Apa itu pemindaian pasif?", "id": "Mendengarkan beacon dari AP sekitar." },
  { "en": "Apa itu pemindaian aktif?", "id": "Mengirim probe request untuk menemukan AP." },
  { "en": "Apa itu objek MIB (Management Information Base)?", "id": "Variabel individual dalam perangkat." },
  { "en": "Apa itu permintaan SNMP (Simple Network Management Protocol) GET?", "id": "Meminta nilai dari sebuah objek MIB." },
  { "en": "Apa itu permintaan SNMP (Simple Network Management Protocol) SET?", "id": "Mengubah nilai dari sebuah objek MIB." },
  { "en": "Apa itu community string di SNMP (Simple Network Management Protocol)?", "id": "Kata sandi dasar untuk akses SNMP." },
  { "en": "Versi SNMP (Simple Network Management Protocol) mana yang paling aman?", "id": "SNMPv3 karena mendukung enkripsi." },
  { "en": "Apa itu shared secret RADIUS (Remote Authentication Dial-In User Service)?", "id": "Kata sandi antara klien dan server." },
  { "en": "Apa itu failover RADIUS (Remote Authentication Dial-In User Service)?", "id": "Menggunakan server cadangan jika utama gagal." },
  { "en": "Apa itu load balancing RADIUS (Remote Authentication Dial-In User Service)?", "id": "Mendistribusikan permintaan ke banyak server." },
  { "en": "Apa itu pembaruan interim akuntansi RADIUS (Remote Authentication Dial-In User Service)?", "id": "Pembaruan berkala selama sesi berlangsung." },
  { "en": "Apa itu MAC (Media Access Control) spoofing?", "id": "Memalsukan alamat MAC perangkat." },
  { "en": "Apa itu jammer WIFI (Wireless Fidelity)?", "id": "Perangkat yang mengganggu sinyal nirkabel." },
  { "en": "Apa itu deauther WIFI (Wireless Fidelity)?", "id": "Alat untuk melancarkan serangan deauthentication." },
  { "en": "Apa itu serangan DoS (Denial of Service) WIFI (Wireless Fidelity)?", "id": "Membuat jaringan tidak dapat digunakan." },
  { "en": "Apa itu rainbow table?", "id": "Tabel hash untuk memecahkan kata sandi." },
  { "en": "Mengapa keamanan fisik AP (Access Point) penting?", "id": "Mencegah pencurian atau perusakan perangkat." },
  { "en": "Apa tujuan utama DFS (Dynamic Frequency Selection)?", "id": "Menghindari interferensi dengan sistem radar." },
  { "en": "Apa itu radar signature?", "id": "Pola sinyal yang unik dari radar." },
  { "en": "Apa itu Channel Availability Check?", "id": "AP memeriksa kanal untuk radar." },
  { "en": "Apa itu In-Service Monitoring untuk DFS (Dynamic Frequency Selection)?", "id": "Terus memantau kanal saat digunakan." },
  { "en": "Apa itu non-occupancy period di DFS (Dynamic Frequency Selection)?", "id": "Waktu tunggu sebelum menggunakan kanal lagi." },
  { "en": "Apa itu channel move time di DFS (Dynamic Frequency Selection)?", "id": "Waktu maksimum untuk mengosongkan kanal." },
  { "en": "Apa arti uptime 'lima sembilan'?", "id": "Ketersediaan 99.999%." },
  { "en": "Apa itu MTBF (Mean Time Between Failures)?", "id": "Waktu rata-rata antar kegagalan." },
  { "en": "Apa itu MTTR (Mean Time To Repair)?", "id": "Waktu rata-rata untuk memperbaiki kegagalan." },
  { "en": "Apa itu SLA (Service Level Agreement)?", "id": "Perjanjian tingkat layanan dengan penyedia." },
  { "en": "Apa itu KPI (Key Performance Indicator) untuk WIFI (Wireless Fidelity)?", "id": "Metrik untuk mengukur kinerja jaringan." },
  { "en": "Apa itu 'konfigurasi emas' untuk AP (Access Point)?", "id": "Template konfigurasi standar terbaik." },
  { "en": "Apa itu 'configuration drift'?", "id": "Perubahan konfigurasi dari waktu ke waktu." },
  { "en": "Apa langkah terakhir dalam troubleshooting?", "id": "Dokumentasi solusi dan perubahan." },
  { "en": "Apa itu model OSI (Open Systems Interconnection)?", "id": "Model konseptual tujuh lapisan jaringan." },
  { "en": "Bagaimana WIFI (Wireless Fidelity) mengubah dunia?", "id": "Menciptakan konektivitas dan mobilitas di mana saja." },
  { "en": "Apa beda Access Point dan Router?", "id": "Router mengarahkan, AP menyediakan akses." },
  { "en": "Apa itu 'data rate' vs 'throughput'?", "id": "Data rate teoritis, throughput aktual." },
  { "en": "Apa itu 'client device capability'?", "id": "Kemampuan WIFI dari perangkat klien." },
  { "en": "Apa itu 'network congestion'?", "id": "Jaringan terlalu padat oleh lalu lintas." },
  { "en": "Apa itu 'airtime contention'?", "id": "Persaingan antar perangkat untuk mengirim." },
  { "en": "Apa itu 'background scanning'?", "id": "AP memindai kanal lain secara berkala." },
  { "en": "Apa itu 'zero-handoff roaming'?", "id": "Roaming tanpa kehilangan paket data." },
  { "en": "Apa itu 'wireless network capacity'?", "id": "Jumlah total throughput yang didukung." },
  { "en": "Apa itu 'user density'?", "id": "Jumlah pengguna per unit area." },
  { "en": "Apa itu 'application requirements'?", "id": "Kebutuhan bandwidth dan latensi aplikasi." },
  { "en": "Apa itu 'predictive site survey'?", "id": "Simulasi cakupan sinyal dengan perangkat lunak." },
  { "en": "Apa itu 'post-deployment survey'?", "id": "Survei setelah instalasi untuk verifikasi." },
  { "en": "Apa itu 'troubleshooting survey'?", "id": "Survei untuk mengidentifikasi masalah." },
  { "en": "Apa itu 'voice-ready network'?", "id": "Jaringan yang dioptimalkan untuk VoIP." },
  { "en": "Apa itu 'location-based services'?", "id": "Layanan berdasarkan lokasi perangkat." },
  { "en": "Apa itu 'real-time location system' (RTLS)?", "id": "Sistem pelacakan aset atau orang." },
  { "en": "Apa itu 'WIFI analytics'?", "id": "Menganalisis data WIFI untuk wawasan." },
  { "en": "Apa itu 'guest analytics'?", "id": "Menganalisis perilaku pengguna di jaringan tamu." },
  { "en": "Apa itu 'social WIFI'?", "id": "Login ke WIFI melalui media sosial." },
  { "en": "Apa itu 'managed WIFI services'?", "id": "Layanan WIFI yang dikelola oleh pihak ketiga." }



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
