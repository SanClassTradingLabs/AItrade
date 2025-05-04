# AItradeAItrade.1. Struktur Direktori Lengkap (aiTrade)Berikut adalah struktur direktori terperinci untuk proyek aiTrade, mencerminkan lapisan backend, frontend, dan komponen pendukung lainnya, dengan penamaan "aiTrade" di lokasi yang relevan..
```
├── dist/                     # Output proses build (backend binary, frontend files)
│   └── aitrade/              # Direktori berisi semua file yang dibutuhkan untuk deployment
│       ├── backend_binary.<ext> # Binary atau skrip eksekusi utama backend
│       └── frontend/         # File statis frontend yang siap disajikan oleh backend
│           ├── index.html
│           ├── bundle.js     # File JavaScript frontend yang di-bundle
│           └── assets/       # Aset statis lainnya (gambar, font, ikon)
├── docs/                     # Dokumentasi proyek
│   ├── architecture/         # Diagram arsitektur, ADRs, penjelasan desain
│   ├── guides/               # Panduan instalasi, penggunaan platform (untuk prop firm & trader), API, deployment
│   ├── specs/                # Spesifikasi API (frontend-backend, backend-trading engine), format data market, aturan risiko
│   └── security/             # Dokumentasi keamanan (manajemen kunci, otentikasi, otorisasi, praktik coding aman, hasil audit, prosedur insiden)
├── backend/                  # Kode sumber dan aset untuk komponen Backend (Platform Logic, API)
│   ├── src/                  # Kode sumber utama backend (misal: Go, Java, Python, Node.js)
│   │   ├── api/              # Implementasi API (User auth, Account mgmt, Risk rules config, Data feed API)
│   │   │   ├── __init__.py   # Inisialisasi paket/modul (jika menggunakan Python)
│   │   │   ├── server.<ext>      # Konfigurasi server web, middleware dasar (logging, error handling)
│   │   │   ├── routes.<ext>      # Definisi endpoint API (auth, user, account, risk, marketdata, trade)
│   │   │   ├── handlers.<ext>    # Implementasi fungsi handler untuk setiap route, validasi input spesifik endpoint
│   │   │   └── middleware/       # Implementasi middleware keamanan dan fungsional
│   │   │       ├── __init__.py
│   │   │       ├── authentication.<ext> # Logic otentikasi pengguna (misal: verifikasi token sesi)
│   │   │       ├── authorization.<ext>  # Logic otorisasi berdasarkan peran/izin pengguna
│   │   │       ├── validation.<ext>     # Logic validasi format input umum (skema data)
│   │   │       └── rate_limiting.<ext>  # Logic pembatasan laju permintaan
│   │   ├── user_management/  # Logic manajemen pengguna (registrasi, login, profil)
│   │   │   ├── __init__.py
│   │   │   ├── domain/       # Entitas (User), Value Objects (Credentials)
│   │   │   ├── application/  # Use cases (RegisterUser, AuthenticateUser, GetUserProfile)
│   │   │   └── infrastructure/ # Implementasi repository (interaksi DB User), Logic MFA, Session Management
│   │   ├── account_management/ # Logic manajemen akun trading
│   │   │   ├── __init__.py
│   │   │   ├── domain/       # Entitas (Account, Balance, Equity, Payout), Aggregate Roots
│   │   │   ├── application/  # Use cases (CreateAccount, GetAccountStatus, UpdateBalance, ProcessPayout)
│   │   │   └── infrastructure/ # Implementasi repository (interaksi DB Account/Balance/Payout), Audit Trail Pencatatan Perubahan Akun
│   │   ├── risk_management/  # Logic penerapan dan pemantauan aturan risiko prop firm
│   │   │   ├── __init__.py
│   │   │   ├── domain/       # Entitas (RiskRule, DrawdownCalculation, ProfitTarget), Value Objects
│   │   │   ├── application/  # Use cases (EvaluateRiskForAccount, ApplyRiskAction, ConfigureRiskRules)
│   │   │   └── infrastructure/ # Gateway untuk mendapatkan data posisi/order/saldo dari Account Management, Secure Storage of Risk Rules Configuration
│   │   ├── data_feed/        # Logic menerima, memproses, dan mendistribusikan data market
│   │   │   ├── __init__.py
│   │   │   ├── domain/       # Entitas (Symbol, Quote, Candlestick), Value Objects (Price)
│   │   │   ├── application/  # Use cases (ProcessNewQuote, GetHistoricalData, SubscribeToSymbol)
│   │   │   ├── infrastructure/ # Konektor ke Data Provider eksternal (misal: FIX client), persistensi data historis (DB/Storage), Data Validation/Sanitization
│   │   │   └── api/          # Logic distribusi data real-time ke klien (WebSocket Server Logic)
│   │   ├── trading_engine_connector/ # Logic komunikasi dengan Trading Engine eksternal
│   │   │   ├── __init__.py
│   │   │   ├── domain/       # Entitas (Order, ExecutionReport, PositionUpdate - mirror dari Account Mgmt domain jika perlu)
│   │   │   ├── application/  # Use cases (SendOrderToEngine, HandleExecutionReport, HandlePositionUpdate)
│   │   │   └── infrastructure/ # Implementasi koneksi fisik ke Trading Engine (misal: FIX client, MT5 API client), Secure Connection (TLS/SSL), Message Encoding/Decoding, Message Integrity Check
│   │   ├── config/           # Konfigurasi backend
│   │   │   ├── __init__.py
│   │   │   ├── settings.<ext>    # Logic memuat dan mengakses setting non-sensitif (port, DB URLs, API timeouts)
│   │   │   └── secrets.<ext>     # Logic memuat dan mengakses kredensial sensitif dari sumber aman (DB passwords, API keys eksternal, kunci enkripsi)
│   │   ├── security/         # Utilitas keamanan spesifik backend
│   │   │   ├── __init__.py
│   │   │   ├── hashing.<ext>     # Utilitas hashing (misal: password hashing)
│   │   │   ├── encryption.<ext>  # Utilitas enkripsi/dekripsi (jika perlu mengenkripsi data sensitif di DB/Config)
│   │   │   └── auth_utils.<ext>  # Utilitas otentikasi/otorisasi level rendah (misal: token generation/validation)
│   │   ├── logging/          # Sistem Logging Terpusat Backend
│   │   │   ├── __init__.py
│   │   │   └── logger.<ext>      # Konfigurasi dan utilitas logging (format log, level, output targets)
│   │   ├── monitoring/       # Utilitas Monitoring dan Alerting Backend
│   │   │   ├── __init__.py
│   │   │   └── metrics.<ext>     # Pengumpulan metrik kinerja, penggunaan sumber daya, metrik keamanan (login attempts, error rates)
│   │   └── main.<ext>        # Titik masuk utama backend
│   ├── tests/                # Pengujian backend
│   │   ├── unit/             # Pengujian unit per fungsi/class
│   │   ├── integration/      # Pengujian interaksi antar modul backend
│   │   └── e2e/              # E2E tests untuk API backend
│   ├── scripts/              # Skrip bantu untuk backend (build, run dev, migrate DB)
│   └── <package_manager_file> # File manajemen dependensi backend
├── frontend/                 # Kode sumber dan aset untuk komponen Frontend (Web UI mirip MT5 Android)
│   ├── public/               # File statis publik (index.html, favicon, assets) - untuk pengembangan
│   ├── src/                  # Kode sumber frontend (misal: React, Vue, Angular - dengan mobile-first design)
│   │   ├── index.<ext>       # Titik masuk utama aplikasi frontend
│   │   ├── components/       # Reusable UI components (Charts, OrderBook, AccountSummary, OrderForm, HistoryTable, Modals, Buttons)
│   │   │   ├── __init__.py
│   │   │   ├── common/       # Komponen umum (Buttons, Modals, Icons)
│   │   │   ├── trading/      # Komponen spesifik trading (Charts, OrderBook, OrderForm)
│   │   │   └── account/      # Komponen spesifik akun (AccountSummary, HistoryTable)
│   │   ├── pages/            # Komponen halaman/views (Login, Dashboard, Trading, History, AccountSettings)
│   │   │   ├── __init__.py
│   │   │   └── auth/         # Halaman otentikasi (Login, Register)
│   │   │   └── app/          # Halaman utama aplikasi (Dashboard, Trading, History)
│   │   ├── api/              # Logic interaksi dengan Backend API (REST & WebSocket)
│   │   │   ├── __init__.py
│   │   │   ├── clients/          # Implementasi klien HTTP/WebSocket
│   │   │   │   ├── __init__.py
│   │   │   │   ├── rest_client.<ext> # Klien untuk panggilan API REST
│   │   │   │   └── websocket_client.<ext> # Klien untuk koneksi WebSocket
│   │   │   └── modules/          # Fungsi panggilan API per modul backend
│   │   │       ├── __init__.py
│   │   │       ├── auth.<ext>    # Panggilan API untuk otentikasi (login, logout)
│   │   │       ├── trade.<ext>   # Panggilan API untuk order (place, modify, cancel), posisi, riwayat
│   │   │       └── account.<ext> # Panggilan API untuk info akun, saldo, payout
│   │   ├── state/            # Manajemen status frontend (misal: Redux, Vuex, Context API)
│   │   │   ├── __init__.py
│   │   │   ├── store.<ext>       # Konfigurasi store utama
│   │   │   ├── modules/          # State per fitur/domain (misal: auth, trade, account, marketdata)
│   │   │   │   ├── __init__.py
│   │   │   │   ├── auth.<ext>    # State otentikasi (user info, token)
│   │   │   │   ├── trade.<ext>   # State terkait trading (posisi, order aktif, riwayat)
│   │   │   │   └── account.<ext> # State terkait akun (saldo, equity, drawdown)
│   │   │   └── hooks.<ext>       # Custom hooks untuk akses state (jika menggunakan React)
│   │   ├── assets/           # Aset statis frontend (ikon MT5-like, gambar, font)
│   │   ├── utils/            # Utilitas helper frontend (formatting data, chart helpers, validation helpers)
│   │   │   ├── __init__.py
│   │   │   └── validation.<ext>  # Utilitas validasi input di sisi klien
│   │   └── types/            # Definisi tipe (untuk TypeScript)
│   ├── tests/                # Pengujian frontend
│   │   ├── unit/             # Pengujian unit per fungsi/komponen
│   │   └── integration/      # Pengujian interaksi antar komponen frontend
│   ├── scripts/              # Skrip bantu untuk frontend (build, start dev server)
│   ├── <package_manager_file> # File manajemen dependensi frontend
│   └── <build_config_file>   # File konfigurasi build frontend
├── shared/                   # Kode atau aset yang digunakan oleh backend dan frontend
│   ├── __init__.py
│   ├── models/               # Definisi model data umum (Order, Position, Quote, UserInfo, AccountInfo, APIRequestPayloads, APIResponsePayloads)
│   │   └── __init__.py
│   ├── exceptions/           # Definisi custom exceptions yang digunakan lintas komponen (misal: UnauthorizedError, ValidationError)
│   │   └── __init__.py
│   ├── utils/                # Utilitas umum (misal: date formatting, number formatting)
│   └── constants.<ext>       # Konstanta (API endpoints, WebSocket message types, error codes)
├── tests/                    # Pengujian end-to-end (melibatkan frontend, backend, dan Trading Engine mock/staging)
│   └── e2e/
│       └── trading_flow.test.<ext> # Tes yang menguji alur trading lengkap
├── scripts/                  # Skrip bantu tingkat proyek (build, deploy stack, run production)
│   ├── build_all.<ext>       # Skrip untuk menjalankan build backend dan frontend
│   └── run_production.<ext>  # Skrip untuk menjalankan binary backend dari dist/
├── .env                      # Variabel lingkungan (untuk seluruh proyek - **HINDARI KUNCI SENSITIF DI SINI**)
├── .gitignore                # File/folder yang diabaikan Git
├── LICENSE                   # Lisensi proyek (GPL)
└── README.md                 # Dokumentasi utama proyek
```

2. Detail Fungsi Setiap Komponen (Direktori/Modul)/dist/: Berisi output proses build yang siap di-deploy. Direktori aitrade/ di dalamnya adalah paket deployment tunggal./docs/: Menyimpan semua dokumentasi proyek./docs/architecture/: Diagram arsitektur, catatan keputusan arsitektur (ADRs)./docs/guides/: Panduan langkah demi langkah untuk berbagai tugas (instalasi, penggunaan, pengembangan fitur, deployment)./docs/specs/: Spesifikasi teknis (API, format data, aturan bisnis)./docs/security/: Dokumentasi khusus terkait keamanan (kebijakan, prosedur, hasil audit)./backend/: Kode sumber dan aset untuk aplikasi backend./backend/src/ : Kode sumber utama backend./backend/src/api/ : Lapisan antarmuka eksternal backend.server.<ext>: Konfigurasi dan bootstrapping server web (misal: setup framework web, middleware dasar).routes.<ext>: Definisi semua endpoint API dan metode HTTP-nya.handlers.<ext>: Implementasi logika spesifik untuk setiap endpoint API. Menerima request, memvalidasi input spesifik endpoint, memanggil logic di lapisan application/ modul lain, memformat respons./backend/src/api/middleware/: Implementasi middleware yang berjalan sebelum handler.authentication.<ext>: Memverifikasi identitas pengguna (misal: dari token sesi).authorization.<ext>: Memeriksa izin pengguna untuk mengakses endpoint/resource tertentu.validation.<ext>: Memvalidasi format input umum (misal: skema JSON) sebelum parsing detail di handler.rate_limiting.<ext>: Membatasi jumlah permintaan dari sumber tertentu./backend/src/user_management/ : Mengelola data dan logic terkait pengguna./backend/src/user_management/domain/: Definisi entitas bisnis (User) dan value objects (Credentials, Profile)./backend/src/user_management/application/: Implementasi use cases (RegisterUser, AuthenticateUser, GetUserProfile). Logic ini memanggil repository./backend/src/user_management/infrastructure/: Implementasi detail interaksi eksternal (misal: UserDatabaseRepository untuk interaksi DB), logic spesifik untuk MFA dan manajemen sesi./backend/src/account_management/ : Mengelola akun trading./backend/src/account_management/domain/: Definisi entitas (Account, Balance, Equity, Payout, Order - sebagai representasi internal), Aggregate Roots./backend/src/account_management/application/: Implementasi use cases (CreateAccount, GetAccountStatus, UpdateBalance, ProcessPayout, RecordExecution). Logic ini memanggil repository dan mungkin berinteraksi dengan Risk Management./backend/src/account_management/infrastructure/: Implementasi detail interaksi DB (AccountRepository, BalanceRepository), Logic Audit Trail untuk mencatat perubahan penting pada akun./backend/src/risk_management/ : Menerapkan dan memantau aturan risiko./backend/src/risk_management/domain/: Definisi entitas (RiskRule, DrawdownCalculation, ProfitTarget)./backend/src/risk_management/application/: Implementasi use cases (EvaluateRiskForAccount, ApplyRiskAction, ConfigureRiskRules). Logic ini berisi aturan validasi risiko dan memicu tindakan (misal: penutupan akun)./backend/src/risk_management/infrastructure/: Gateway untuk mendapatkan data posisi/order/saldo (mungkin memanggil Account Management Application Layer), Secure Storage untuk konfigurasi aturan risiko (misal: di DB terenkripsi)./backend/src/data_feed/ : Mengelola data market./backend/src/data_feed/domain/: Definisi entitas (Symbol, Quote, Candlestick)./backend/src/data_feed/application/: Implementasi use cases (ProcessNewQuote, GetHistoricalData, SubscribeToSymbol). Logic ini memproses data mentah./backend/src/data_feed/infrastructure/: Implementasi konektor ke Data Provider eksternal (misal: FIX client), persistensi data historis, Data Validation/Sanitization (memastikan data bersih dan valid)./backend/src/data_feed/api/: Logic spesifik untuk distribusi data real-time (WebSocket Server Logic)./backend/src/trading_engine_connector/ : Berkomunikasi dengan Trading Engine./backend/src/trading_engine_connector/domain/: Definisi entitas (Order, ExecutionReport, PositionUpdate - representasi pesan dari/ke Trading Engine)./backend/src/trading_engine_connector/application/: Use cases (SendOrderToEngine, HandleExecutionReport, HandlePositionUpdate). Logic ini mengorkestrasi pengiriman/penerimaan pesan./backend/src/trading_engine_connector/infrastructure/: Implementasi koneksi fisik ke Trading Engine (misal: FIX client, MT5 API client), Secure Connection (TLS/SSL), Message Encoding/Decoding, Message Integrity Check (memverifikasi pesan dari Engine)./backend/src/config/ : Mengelola konfigurasi backend.settings.<ext>: Logic memuat dan menyediakan akses ke setting non-sensitif (port, DB URLs, timeouts).secrets.<ext>: Logic memuat dan menyediakan akses ke kredensial sensitif dari sumber yang SANGAT AMAN (bukan .env biasa)./backend/src/security/ : Utilitas keamanan umum backend.hashing.<ext>: Fungsi hashing (misal: bcrypt untuk password).encryption.<ext>: Fungsi enkripsi/dekripsi (jika perlu mengenkripsi data sensitif di DB/Config).auth_utils.<ext>: Utilitas terkait otentikasi/otorisasi (misal: pembuatan/validasi token sesi, manajemen izin)./backend/src/logging/ : Sistem logging terpusat.logger.<ext>: Konfigurasi logger, fungsi untuk menulis log dengan level berbeda (info, warn, error, security)./backend/src/monitoring/ : Utilitas monitoring dan alerting.metrics.<ext>: Logic untuk mengumpulkan metrik (CPU, memori, request latency, error rates, login attempts, transaksi trading). Integrasi dengan sistem monitoring eksternal.main.<ext>: Titik masuk utama aplikasi backend. Melakukan inisialisasi awal, memuat config, menginisialisasi modul, dan memulai server API./backend/tests/: Kode pengujian untuk backend./backend/scripts/: Skrip bantu untuk pengembangan backend./frontend/: Kode sumber dan aset untuk antarmuka pengguna web./frontend/public/: File statis yang disajikan langsung oleh backend (index.html, favicon)./frontend/src/: Kode sumber utama frontend.index.<ext>: Titik masuk utama aplikasi frontend./frontend/src/components/: Komponen UI yang dapat digunakan kembali./frontend/src/components/common/: Komponen umum (Button, Modal)./frontend/src/components/trading/: Komponen spesifik trading (Chart, OrderBook)./frontend/src/components/account/: Komponen spesifik akun (AccountSummary)./frontend/src/pages/: Komponen yang merepresentasikan halaman/views./frontend/src/pages/auth/: Halaman terkait otentikasi (Login, Register)./frontend/src/pages/app/: Halaman utama aplikasi (Dashboard, Trading)./frontend/src/api/: Logic interaksi dengan Backend API./frontend/src/api/clients/: Implementasi klien HTTP/WebSocket.rest_client.<ext>: Klien generik untuk panggilan API REST.websocket_client.<ext>: Klien untuk koneksi WebSocket dan penanganan pesan./frontend/src/api/modules/: Fungsi panggilan API spesifik per fitur.auth.<ext>: Fungsi untuk login, logout, register (memanggil rest_client).trade.<ext>: Fungsi untuk place order, get positions, get history (memanggil rest_client dan websocket_client).account.<ext>: Fungsi untuk get account info, get balance (memanggil rest_client)./frontend/src/state/: Manajemen status aplikasi frontend.store.<ext>: Konfigurasi store manajemen state utama./frontend/src/state/modules/: State per fitur/domain.auth.<ext>: State terkait otentikasi (user info, token).trade.<ext>: State terkait trading (daftar posisi, order aktif, riwayat, harga real-time).account.<ext>: State terkait akun (saldo, equity, drawdown).hooks.<ext>: Custom hooks untuk akses state (jika menggunakan React Context/Redux hooks)./frontend/src/assets/: Aset statis (gambar, font, ikon)./frontend/src/utils/: Utilitas helper frontend./frontend/src/utils/validation/: Utilitas validasi input di sisi klien./frontend/src/types/: Definisi tipe (untuk TypeScript)./frontend/tests/: Kode pengujian frontend./frontend/scripts/: Skrip bantu pengembangan frontend./shared/: Kode dan aset yang digunakan oleh backend dan frontend./shared/models/: Definisi struktur data yang digunakan dalam komunikasi API dan state (Order, Position, Quote, UserInfo, AccountInfo, payload request/response)./shared/exceptions/: Definisi custom exceptions yang dapat dilempar oleh backend dan ditangani oleh frontend./shared/utils/: Utilitas umum (formatting data).constants.<ext>: Konstanta yang digunakan di kedua sisi (endpoint API, tipe pesan WebSocket)./tests/: Pengujian end-to-end./tests/e2e/: Tes yang menguji alur lengkap dari interaksi UI hingga backend dan Trading Engine (menggunakan mock/staging)./scripts/: Skrip bantu tingkat proyek.build_all.<ext>: Mengotomatiskan build backend dan frontend.run_production.<ext>: Menjalankan binary backend dari direktori dist/..env: File untuk variabel lingkungan (untuk konfigurasi non-sensitif)..gitignore: Mengabaikan file/folder dari Git.LICENSE: Lisensi proyek (GPL).README.md: Dokumentasi utama proyek.3. Detail Cara Kerja Semua Komponen Saling TerintegrasiIntegrasi dalam arsitektur aiTrade sangat dinamis d
