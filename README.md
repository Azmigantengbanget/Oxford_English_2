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


    { "en": "Prestigious?", "id": "Bergengsi." },
    { "en": "Presumably?", "id": "Diduga." },
    { "en": "Presume?", "id": "Berasumsi." },
    { "en": "Prevail?", "id": "Menang." },
    { "en": "Prevalence?", "id": "Prevalensi." },
    { "en": "Prevention?", "id": "Pencegahan." },
    { "en": "Prey?", "id": "Mangsa." },
    { "en": "Principal (noun)?", "id": "Kepala sekolah/prinsip." },
    { "en": "Privatization?", "id": "Privatisasi." },
    { "en": "Privilege?", "id": "Hak istimewa." },
    { "en": "Probe?", "id": "Penyelidikan, menyelidiki." },
    { "en": "Problematic?", "id": "Bermasalah." },
    { "en": "Proceedings?", "id": "Proses." },
    { "en": "Proceeds?", "id": "Hasil." },
    { "en": "Processing?", "id": "Pemrosesan." },
    { "en": "Processor?", "id": "Prosesor." },
    { "en": "Proclaim?", "id": "Memproklamasikan." },
    { "en": "Productive?", "id": "Produktif." },
    { "en": "Productivity?", "id": "Produktivitas." },
    { "en": "Profitable?", "id": "Menguntungkan." },
    { "en": "Profound?", "id": "Mendalam." },
    { "en": "Projection?", "id": "Proyeksi." },
    { "en": "Prominent?", "id": "Terkemuka." },
    { "en": "Pronounced?", "id": "Jelas." },
    { "en": "Propaganda?", "id": "Propaganda." },
    { "en": "Proposition?", "id": "Proposisi." },
    { "en": "Prosecute?", "id": "Menuntut." },
    { "en": "Prosecution?", "id": "Penuntutan." },
    { "en": "Prosecutor?", "id": "Jaksa." },
    { "en": "Prospective?", "id": "Prospektif." },
    { "en": "Prosperity?", "id": "Kemakmuran." },
    { "en": "Protective?", "id": "Protektif." },
    { "en": "Protocol?", "id": "Protokol." },
    { "en": "Province?", "id": "Provinsi." },
    { "en": "Provincial?", "id": "Provinsi." },
    { "en": "Provision?", "id": "Ketentuan." },
    { "en": "Provoke?", "id": "Memprovokasi." },
    { "en": "Psychiatric?", "id": "Psikiatri." },
    { "en": "Pulse?", "id": "Denyut nadi." },
    { "en": "Pump?", "id": "Memompa, pompa." },
    { "en": "Punch?", "id": "Pukulan, meninju." },
    { "en": "Query?", "id": "Pertanyaan." },
    { "en": "Quest?", "id": "Pencarian." },
    { "en": "Quota?", "id": "Kuota." },
    { "en": "Radar?", "id": "Radar." },
    { "en": "Radical?", "id": "Radikal." },
    { "en": "Rage?", "id": "Amarah." },
    { "en": "Raid?", "id": "Serbuan, menyerbu." },
    { "en": "Rally?", "id": "Rapat, berkumpul." },
    { "en": "Ranking?", "id": "Peringkat." },
    { "en": "Rape?", "id": "Pemerkosaan, memperkosa." },
    { "en": "Ratio?", "id": "Rasio." },
    { "en": "Rational?", "id": "Rasional." },
    { "en": "Ray?", "id": "Sinar." },
    { "en": "Readily?", "id": "Dengan mudah." },
    { "en": "Realization?", "id": "Realisasi." },
    { "en": "Realm?", "id": "Ranah." },
    { "en": "Rear?", "id": "Belakang." },
    { "en": "Reasoning?", "id": "Penalaran." },
    { "en": "Reassure?", "id": "Meyakinkan." },
    { "en": "Rebel?", "id": "Pemberontak." },
    { "en": "Rebellion?", "id": "Pemberontakan." },
    { "en": "Recipient?", "id": "Penerima." },
    { "en": "Reconstruction?", "id": "Rekonstruksi." },
    { "en": "Recount?", "id": "Menghitung ulang." },
    { "en": "Referendum?", "id": "Referendum." },
    { "en": "Reflection?", "id": "Refleksi." },
    { "en": "Reform?", "id": "Reformasi, mereformasi." },
    { "en": "Refuge?", "id": "Perlindungan." },
    { "en": "Refusal?", "id": "Penolakan." },
    { "en": "Regain?", "id": "Mendapatkan kembali." },
    { "en": "Regardless?", "id": "Terlepas dari." },
    { "en": "Regime?", "id": "Rezim." },
    { "en": "Regulator?", "id": "Regulator." },
    { "en": "Regulatory?", "id": "Regulatori." },
    { "en": "Rehabilitation?", "id": "Rehabilitasi." },
    { "en": "Reign?", "id": "Pemerintahan, memerintah." },
    { "en": "Rejection?", "id": "Penolakan." },
    { "en": "Relevance?", "id": "Relevansi." },
    { "en": "Reliability?", "id": "Rehabilitas." },
    { "en": "Reluctant?", "id": "Enggan." },
    { "en": "Remainder?", "id": "Sisa." },
    { "en": "Remains?", "id": "Sisa-sisa." },
    { "en": "Remedy?", "id": "Obat." },
    { "en": "Reminder?", "id": "Pengingat." },
    { "en": "Removal?", "id": "Penghapusan." },
    { "en": "Render?", "id": "Memberikan." },
    { "en": "Renew?", "id": "Memperbarui." },
    { "en": "Renowned?", "id": "Termasyhur." },
    { "en": "Rental?", "id": "Penyewaan." },
    { "en": "Replacement?", "id": "Penggantian." },
    { "en": "Reportedly?", "id": "Dikabarkan." },
    { "en": "Representation?", "id": "Representasi." },
    { "en": "Reproduce?", "id": "Bereproduksi." },
    { "en": "Reproduction?", "id": "Reproduksi." },
    { "en": "Republic?", "id": "Republik." },
    { "en": "Resemble?", "id": "Menyerupai." },
    { "en": "Reside?", "id": "Tinggal." },
    { "en": "Residence?", "id": "Tempat tinggal." },
    { "en": "Residential?", "id": "Perumahan." },
    { "en": "Residue?", "id": "Residu." },
    { "en": "Resignation?", "id": "Pengunduran diri." },
    { "en": "Resistance?", "id": "Perlawanan." },
    { "en": "Respective?", "id": "Masing-masing." },
    { "en": "Respectively?", "id": "Secara berturut." },
    { "en": "Restoration?", "id": "Restorasi." },
    { "en": "Restraint?", "id": "Pembatasan." },
    { "en": "Resume?", "id": "Melanjutkan." },
    { "en": "Retreat?", "id": "Mundur." },
    { "en": "Retrieve?", "id": "Mengambil." },
    { "en": "Revelation?", "id": "Wahyu." },
    { "en": "Revenge?", "id": "Balas dendam." },
    { "en": "Reverse?", "id": "Membalik." },
    { "en": "Revival?", "id": "Kebangkitan." },
    { "en": "Revive?", "id": "Menghidupkan kembali." },
    { "en": "Revolutionary?", "id": "Revolusioner." },
    { "en": "Rhetoric?", "id": "Retorika." },
    { "en": "Rifle?", "id": "Senapan." },
    { "en": "Riot?", "id": "Kerusuhan." },
    { "en": "Rip?", "id": "Merobek." },
    { "en": "Ritual?", "id": "Ritual." },
    { "en": "Robust?", "id": "Kuat." },
    { "en": "Rock (verb)?", "id": "Menggoyangkan." },
    { "en": "Rod?", "id": "Batang." },
    { "en": "Rotate?", "id": "Berputar." },
    { "en": "Rotation?", "id": "Rotasi." },
    { "en": "Ruling?", "id": "Keputusan." },
    { "en": "Rumour?", "id": "Rumor." },
    { "en": "Sack?", "id": "Memecat." },
    { "en": "Sacred?", "id": "Suci." },
    { "en": "Sacrifice?", "id": "Pengorbanan, berkorban." },
    { "en": "Saint?", "id": "Santo." },
    { "en": "Sake?", "id": "Demi." },
    { "en": "Sanction?", "id": "Sanksi." },
    { "en": "Say (noun)?", "id": "Ucapan." },
    { "en": "Scattered?", "id": "Tersebar." },
    { "en": "Sceptical?", "id": "Skeptis." },
    { "en": "Scope?", "id": "Lingkup." },
    { "en": "Screw?", "id": "Sekrup, mengacaukan." },
    { "en": "Scrutiny?", "id": "Pengawasan ketat." },
    { "en": "Seal?", "id": "Menyegel, segel." },
    { "en": "Secular?", "id": "Sekuler." },
    { "en": "Seemingly?", "id": "Tampaknya." },
    { "en": "Segment?", "id": "Segmen." },
    { "en": "Seize?", "id": "Menyita." },
    { "en": "Seldom?", "id": "Jarang." },
    { "en": "Selective?", "id": "Selektif." },
    { "en": "Senator?", "id": "Senator." },
    { "en": "Sensation?", "id": "Sensasi." },
    { "en": "Sensitivity?", "id": "Sensitivitas." },
    { "en": "Sentiment?", "id": "Sentimen." },
    { "en": "Separation?", "id": "Pemisahan." },
    { "en": "Serial?", "id": "Serial." },
    { "en": "Settlement?", "id": "Penyelesaian." },
    { "en": "Set-up?", "id": "Pengaturan." },
    { "en": "Sexuality?", "id": "Seksualitas." },
    { "en": "Shareholder?", "id": "Pemegang saham." },
    { "en": "Shatter?", "id": "Menghancurkan." },
    { "en": "Shed?", "id": "Menumpahkan." },
    { "en": "Sheer?", "id": "Murni." },
    { "en": "Shipping?", "id": "Pengiriman." },
    { "en": "Shoot (noun)?", "id": "Tunas." },
    { "en": "Shrink?", "id": "Menyusut." },
    { "en": "Shrug?", "id": "Mengangkat bahu." },
    { "en": "Sigh?", "id": "Menghela napas." },
    { "en": "Simulate?", "id": "Meniru." },
    { "en": "Simulation?", "id": "Simulasi." },
    { "en": "Simultaneously?", "id": "Secara bersamaan." },
    { "en": "Sin?", "id": "Dosa." },
    { "en": "Situated?", "id": "Terletak." },
    { "en": "Sketch?", "id": "Sketsa." },
    { "en": "Skip?", "id": "Melewatkan." },
    { "en": "Slam?", "id": "Membanting." },
    { "en": "Slap?", "id": "Menampar." },
    { "en": "Slash?", "id": "Memotong." },
    { "en": "Slavery?", "id": "Perbudakan." },
    { "en": "Slot?", "id": "Slot." },
    { "en": "Smash?", "id": "Menghancurkan." },
    { "en": "Snap?", "id": "Mematahkan." },
    { "en": "Soak?", "id": "Merendam." },
    { "en": "Soar?", "id": "Melonjak." },
    { "en": "Socialist?", "id": "Sosialis." },
    { "en": "Sole?", "id": "Tunggal." },
    { "en": "Solely?", "id": "Semata-mata." },
    { "en": "Solicitor?", "id": "Pengacara." },
    { "en": "Solidarity?", "id": "Solidaritas." },
    { "en": "Solo?", "id": "Solo." },
    { "en": "Sound (adjective)?", "id": "Sehat." },
    { "en": "Sovereignty?", "id": "Kedaulatan." },
    { "en": "Spam?", "id": "Spam." },
    { "en": "Span?", "id": "Menjangkau." },
    { "en": "Spare (verb)?", "id": "Menghemat." },
    { "en": "Spark?", "id": "Memicu." },
    { "en": "Specialized?", "id": "Khusus." },
    { "en": "Specification?", "id": "Spesifikasi." },
    { "en": "Specimen?", "id": "Spesimen." },
    { "en": "Spectacle?", "id": "Tontonan." },
    { "en": "Spectrum?", "id": "Spektrum." },
    { "en": "Spell (noun)?", "id": "Mantra." },
    { "en": "Sphere?", "id": "Bola." },
    { "en": "Spine?", "id": "Tulang belakang." },
    { "en": "Spin?", "id": "Berputar, putaran." },
    { "en": "Spotlight?", "id": "Sorotan." },
    { "en": "Spouse?", "id": "Pasangan." },
    { "en": "Spy?", "id": "Mata-mata, memata-matai." },
    { "en": "Squad?", "id": "Regu." },
    { "en": "Squeeze?", "id": "Memeras." },
    { "en": "Stab?", "id": "Menusuk." },
    { "en": "Stability?", "id": "Stabilitas." },
    { "en": "Stabilize?", "id": "Menstabilkan." },
    { "en": "Stake?", "id": "Taruhan." },
    { "en": "Standing?", "id": "Berdiri." },
    { "en": "Stark?", "id": "Nyata." },
    { "en": "Statistical?", "id": "Statistik." },
    { "en": "Steer?", "id": "Mengarahkan." },
    { "en": "Stem?", "id": "Batang, berasal dari." },
    { "en": "Stereotype?", "id": "Stereotip." },
    { "en": "Stimulus?", "id": "Stimulus." },
    { "en": "Stir?", "id": "Mengaduk." },
    { "en": "Storage?", "id": "Penyimpanan." },
    { "en": "Straightforward?", "id": "Lugas." },
    { "en": "Strain?", "id": "Ketegangan." },
    { "en": "Strand?", "id": "Untaian." },
    { "en": "Strategic?", "id": "Strategis." },
    { "en": "Striking?", "id": "Mencolok." },
    { "en": "Strip (long narrow piece)?", "id": "Jalur." },
    { "en": "Strive?", "id": "Berusaha keras." },
    { "en": "Structural?", "id": "Struktural." },
    { "en": "Stumble?", "id": "Tersandung." },
    { "en": "Stun?", "id": "Mengejutkan." },
    { "en": "Submission?", "id": "Penyerahan." },
    { "en": "Subscriber?", "id": "Pelanggan." },
    { "en": "Subscription?", "id": "Langganan." },
    { "en": "Subsidy?", "id": "Subsidi." },
    { "en": "Substantial?", "id": "Subtansial." },
    { "en": "Substantially?", "id": "Secara subtansial." },
    { "en": "Substitute?", "id": "Pengganti, menggantikan." },
    { "en": "Substitution?", "id": "Subtitusi." },
    { "en": "Subtle?", "id": "Halus." },
    { "en": "Suburban?", "id": "Pinggiran kota." },
    { "en": "Succession?", "id": "Suksesi." },
    { "en": "Successive?", "id": "Berturut-turut." },
    { "en": "Successor?", "id": "Penerus." },
    { "en": "Suck?", "id": "Menghisap." },
    { "en": "Sue?", "id": "Menggugat." },
    { "en": "Suicide?", "id": "Bunuh diri." },
    { "en": "Suite?", "id": "Suite." },
    { "en": "Summit?", "id": "Puncak." },
    { "en": "Superb?", "id": "Luar biasa." },
    { "en": "Superior?", "id": "Unggul." },
    { "en": "Supervise?", "id": "Mengawasi." },
    { "en": "Supervision?", "id": "Pengawasan." },
    { "en": "Supervisor?", "id": "Pengawas." },
    { "en": "Supplement?", "id": "Suplemen, melengkapi." },
    { "en": "Supportive?", "id": "Mendukung." },
    { "en": "Supposedly?", "id": "Konon." },
    { "en": "Suppress?", "id": "Menekan." },
    { "en": "Supreme?", "id": "Tertinggi." },
    { "en": "Surge?", "id": "Lonjakan, melonjak." },
    { "en": "Surgical?", "id": "Bedah." },
    { "en": "Surplus?", "id": "Surplus." },
    { "en": "Surrender?", "id": "Menyerah." },
    { "en": "Surveillance?", "id": "Pengawasan." },
    { "en": "Suspension?", "id": "Penangguhan." },
    { "en": "Suspicion?", "id": "Kecurigaan." },
    { "en": "Suspicious?", "id": "Mencurigakan." },
    { "en": "Sustain?", "id": "Mempertahankan." },
    { "en": "Swing?", "id": "Berayun, ayunan." },
    { "en": "Sword?", "id": "Pedang." },
    { "en": "Symbolic?", "id": "Simbolik." },
    { "en": "Syndrome?", "id": "Sindrom." },
    { "en": "Synthesis?", "id": "Sintesis." },
    { "en": "Systematic?", "id": "Sistematis." },
    { "en": "Tackle (noun)?", "id": "Menangani." },
    { "en": "Tactic?", "id": "Taktik." },
    { "en": "Tactical?", "id": "Taktis." },
    { "en": "Taxpayer?", "id": "Pembayar pajak." },
    { "en": "Tempt?", "id": "Menggoda." },
    { "en": "Tenant?", "id": "Penyewa." },
    { "en": "Tender?", "id": "Lembut." },
    { "en": "Tenure?", "id": "Masa jabatan." },
    { "en": "Terminal (adjective)?", "id": "Terminal." },
    { "en": "Terminate?", "id": "Mengakhiri." },
    { "en": "Terrain?", "id": "Medan." },
    { "en": "Terrific?", "id": "Hebat." },
    { "en": "Testify?", "id": "Bersaksi." },
    { "en": "Testimony?", "id": "Kesaksian." },
    { "en": "Texture?", "id": "Tekstur." },
    { "en": "Thankfully?", "id": "Syukurlah." },
    { "en": "Theatrical?", "id": "Teatrikal." },
    { "en": "Theology?", "id": "Teologi." },
    { "en": "Theoretical?", "id": "Teoretis." },
    { "en": "Thereafter?", "id": "Setelah itu." },
    { "en": "Thereby?", "id": "Dengan demikian." },
    { "en": "Thoughtful?", "id": "Penuh pertimbangan." },
    { "en": "Thought-provoking?", "id": "Merangsang." },
    { "en": "Thread?", "id": "Benang." },
    { "en": "Threshold?", "id": "Ambang." },
    { "en": "Thrilled?", "id": "Sangat senang." },
    { "en": "Thrive?", "id": "Berkembang." },
    { "en": "Tide?", "id": "Pasang." },
    { "en": "Tighten?", "id": "Mengencangkan." },
    { "en": "Timber?", "id": "Kayu." },
    { "en": "Timely?", "id": "Tepat waktu." },
    { "en": "Tobacco?", "id": "Tembakau." },
    { "en": "Tolerance?", "id": "Toleransi." },
    { "en": "Tolerate?", "id": "Mentolerir." },
    { "en": "Toll?", "id": "Biaya." },
    { "en": "Top (verb)?", "id": "Mengungguli." },
    { "en": "Torture?", "id": "Penyiksaan, menyiksa." },
    { "en": "Toss?", "id": "Melempar." },
    { "en": "Total (verb)?", "id": "Total." },
    { "en": "Toxic?", "id": "Beracun." },
    { "en": "Trace (noun)?", "id": "Jejak." },
    { "en": "Trademark?", "id": "Merek dagang." },
    { "en": "Trail?", "id": "Jejak, menelusuri." },
    { "en": "Trailer?", "id": "Trailer." },
    { "en": "Transaction?", "id": "Transaksi." },
    { "en": "Transcript?", "id": "Transkrip." },
    { "en": "Transformation?", "id": "Transformasi." },
    { "en": "Transit?", "id": "Transit." },
    { "en": "Transmission?", "id": "Transmisi." },
    { "en": "Transparency?", "id": "Transparansi." },
    { "en": "Transparent?", "id": "Transparan." },
    { "en": "Trauma?", "id": "Trauma." },
    { "en": "Treaty?", "id": "Perjanjian." },
    { "en": "Tremendous?", "id": "Luar biasa." },
    { "en": "Tribal?", "id": "Kesukuan." },
    { "en": "Tribunal?", "id": "Pengadilan." },
    { "en": "Tribute?", "id": "Penghormatan." },
    { "en": "Trigger (noun)?", "id": "Pemicu." },
    { "en": "Trio?", "id": "Trio." },
    { "en": "Triumph?", "id": "Kemenangan." },
    { "en": "Trophy?", "id": "Trofi." },
    { "en": "Troubled?", "id": "Bermasalah." },
    { "en": "Trustee?", "id": "Wali." },
    { "en": "Tuition?", "id": "Biaya kuliah." },
    { "en": "Turnout?", "id": "Partisipasi." },
    { "en": "Turnover?", "id": "Perputaran." },
    { "en": "Twist?", "id": "Memutar, putaran." },
    { "en": "Undergraduate?", "id": "Mahasiswa sarjana." },
    { "en": "Underlying?", "id": "Mendasar." },
    { "en": "Undermine?", "id": "Merusak." },
    { "en": "Undoubtedly?", "id": "Tidak diragukan lagi." },
    { "en": "Unify?", "id": "Menyatukan." },
    { "en": "Unprecedented?", "id": "Belum pernah terjadi." },
    { "en": "Unveil?", "id": "Mengungkap." },
    { "en": "Upcoming?", "id": "Mendatang." },
    { "en": "Upgrade?", "id": "Meningkatkan, peningkatan." },
    { "en": "Uphold?", "id": "Menegakkan." },
    { "en": "Utility?", "id": "Utilitas." },
    { "en": "Utilize?", "id": "Memanfaatkan." },
    { "en": "Utterly?", "id": "Sepenuhnya." },
    { "en": "Vacuum?", "id": "Vakum." },
    { "en": "Vague?", "id": "Samar." },
    { "en": "Validity?", "id": "Validitas." },
    { "en": "Vanish?", "id": "Menghilang." },
    { "en": "Variable?", "id": "Variabel." },
    { "en": "Varied?", "id": "Beragam." },
    { "en": "Vein?", "id": "Pembuluh darah." },
    { "en": "Venture?", "id": "Usaha, berani mencoba." },
    { "en": "Verbal?", "id": "Lisan." },
    { "en": "Verdict?", "id": "Putusan." },
    { "en": "Verify?", "id": "Memverifikasi." },
    { "en": "Verse?", "id": "Ayat." },
    { "en": "Versus?", "id": "Versus." },
    { "en": "Vessel?", "id": "Pembuluh/kapal." },
    { "en": "Veteran?", "id": "Veteran." },
    { "en": "Viable?", "id": "Layak." },
    { "en": "Vibrant?", "id": "Bersemangat." },
    { "en": "Vice?", "id": "Keburukan." },
    { "en": "Vicious?", "id": "Kejam." },
    { "en": "Villager?", "id": "Penduduk desa." },
    { "en": "Violate?", "id": "Melanggar." },
    { "en": "Violation?", "id": "Pelanggaran." },
    { "en": "Virtue?", "id": "Kebajikan." },
    { "en": "Vocal?", "id": "Vokal." },
    { "en": "Vow?", "id": "Bersumpah." },
    { "en": "Vulnerability?", "id": "Kerentanan." },
    { "en": "Vulnerable?", "id": "Rentan." },
    { "en": "Ward?", "id": "Bangsal." },
    { "en": "Warehouse?", "id": "Gudang." },
    { "en": "Warfare?", "id": "Peperangan." },
    { "en": "Warrant?", "id": "Surat perintah, menjamin." },
    { "en": "Warrior?", "id": "Pejuang." },
    { "en": "Weaken?", "id": "Melemahkan." },
    { "en": "Weave?", "id": "Menenun." },
    { "en": "Weed?", "id": "Gulma." },
    { "en": "Well (noun)?", "id": "Sumur." },
    { "en": "Well-being?", "id": "Kesejahteraan." },
    { "en": "Whatsoever?", "id": "Apa pun." },
    { "en": "Whereby?", "id": "Dengan mana." },
    { "en": "Whilst?", "id": "Sementara." },
    { "en": "Whip?", "id": "Mencambuk." },
    { "en": "Wholly?", "id": "Sepenuhnya." },
    { "en": "Widen?", "id": "Melebarkan." },
    { "en": "Widow?", "id": "Janda." },
    { "en": "Width?", "id": "Lebar." },
    { "en": "Willingness?", "id": "Kesediaan." },
    { "en": "Wipe?", "id": "Menyeka." },
    { "en": "Wit?", "id": "Kecerdasan." },
    { "en": "Withdrawal?", "id": "Penarikan." },
    { "en": "Workout?", "id": "Latihan." },
    { "en": "Worship?", "id": "Pemujaan, memuja." },
    { "en": "Worthwhile?", "id": "Berharga." },
    { "en": "Worthy?", "id": "Layak." },
    { "en": "Yell?", "id": "Berteriak." },
    { "en": "Yield?", "id": "Hasil, menghasilkan." },
    { "en": "Youngster?", "id": "Anak muda." }


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
