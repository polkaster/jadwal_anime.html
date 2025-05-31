<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Website Jadwal Anime Terupdate</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827; /* bg-gray-900 */
            color: #f3f4f6; /* text-gray-100 */
        }
        .anime-card {
            background-color: #1f2937; /* bg-gray-800 */
            border-radius: 0.5rem; /* rounded-lg */
            overflow: hidden;
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
            display: flex;
            flex-direction: column;
            border: 1px solid #374151; /* border-gray-700 */
        }
        .anime-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05), 0 0 0 2px #3b82f6; /* shadow-lg and blue ring on hover */
        }
        .anime-card img {
            width: 100%;
            height: 320px; /* Fixed height for images */
            object-fit: cover; /* Changed to cover to avoid distortion */
        }
        .anime-card-content {
            padding: 1rem; /* p-4 */
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }
        .synopsis {
            display: -webkit-box;
            -webkit-line-clamp: 4; /* Show 4 lines */
            -webkit-box-orient: vertical;
            overflow: hidden;
            text-overflow: ellipsis;
            font-size: 0.875rem; /* text-sm */
            line-height: 1.25rem; /* leading-tight */
            color: #d1d5db; /* text-gray-300 */
            min-height: 70px; /* Approx 4 lines height */
            margin-bottom: 0.75rem; /* mb-3 */
        }
        .btn {
            padding: 0.5rem 1rem;
            border-radius: 0.375rem; /* rounded-md */
            font-weight: 500; /* medium */
            transition: background-color 0.2s;
            cursor: pointer;
            text-align: center;
        }
        .btn-primary {
            background-color: #3b82f6; /* bg-blue-600 */
            color: white;
        }
        .btn-primary:hover {
            background-color: #2563eb; /* bg-blue-700 */
        }
        .btn-primary.active {
            background-color: #1d4ed8; /* bg-blue-800 */
            font-weight: 600; /* semibold */
        }
        .btn-secondary {
            background-color: #4b5563; /* bg-gray-600 */
            color: white;
        }
        .btn-secondary:hover {
            background-color: #374151; /* bg-gray-700 */
        }
        select.input-control {
            background-color: #374151; /* bg-gray-700 */
            color: #f3f4f6; /* text-gray-100 */
            border-radius: 0.375rem; /* rounded-md */
            padding: 0.625rem 0.75rem; /* py-2.5 px-3 */
            border: 1px solid #4b5563; /* border-gray-600 */
            appearance: none; /* Remove default arrow */
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 20 20' fill='%239ca3af'%3E%3Cpath fill-rule='evenodd' d='M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z' clip-rule='evenodd'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 0.5rem center;
            background-size: 1.5em 1.5em;
            padding-right: 2.5rem; /* Make space for custom arrow */
        }
        .loader {
            border: 5px solid #374151; /* border-gray-700 */
            border-top: 5px solid #3b82f6; /* Blue */
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
            margin: 3rem auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        #error-message-box {
            background-color: #ef4444; /* bg-red-500 */
            color: white;
            padding: 1rem;
            border-radius: 0.375rem; /* rounded-md */
            text-align: center;
            margin: 1.5rem auto;
            max-width: 600px;
        }
        /* Custom scrollbar for webkit browsers */
        ::-webkit-scrollbar { width: 10px; }
        ::-webkit-scrollbar-track { background: #1f2937; /* bg-gray-800 */ }
        ::-webkit-scrollbar-thumb { background: #4b5563; /* bg-gray-600 */ border-radius: 5px; }
        ::-webkit-scrollbar-thumb:hover { background: #6b7280; /* bg-gray-500 */ }
    </style>
</head>
<body>
    <div class="container mx-auto p-4 md:p-6">
        <header class="text-center my-8">
            <h1 class="text-4xl md:text-5xl font-bold text-blue-400">Jadwal Anime Terupdate</h1>
            <p class="text-lg text-gray-400 mt-2">Temukan anime terbaru favoritmu!</p>
        </header>

        <div id="controls" class="mb-8 p-4 md:p-6 bg-gray-800 rounded-lg shadow-xl">
            <div class="flex flex-wrap gap-3 mb-4 justify-center">
                <button id="btn-now" class="btn btn-primary active">Musim Ini</button>
                <button id="btn-upcoming" class="btn btn-primary">Musim Depan</button>
                <button id="btn-toggle-specific" class="btn btn-primary">Pilih Musim Tertentu</button>
            </div>
            <div id="specific-season-selector" class="hidden flex flex-col sm:flex-row flex-wrap gap-3 items-end justify-center p-4 bg-gray-700/50 rounded-md mt-4">
                <div class="w-full sm:w-auto sm:flex-grow">
                    <label for="year-select" class="block text-sm font-medium text-gray-300 mb-1">Tahun:</label>
                    <select id="year-select" class="input-control w-full"></select>
                </div>
                <div class="w-full sm:w-auto sm:flex-grow">
                    <label for="season-select" class="block text-sm font-medium text-gray-300 mb-1">Musim:</label>
                    <select id="season-select" class="input-control w-full">
                        <option value="winter">Musim Dingin (Winter)</option>
                        <option value="spring">Musim Semi (Spring)</option>
                        <option value="summer">Musim Panas (Summer)</option>
                        <option value="fall">Musim Gugur (Fall)</option>
                    </select>
                </div>
                <button id="btn-fetch-specific" class="btn btn-primary w-full sm:w-auto mt-2 sm:mt-0">Tampilkan</button>
            </div>
        </div>

        <div id="loading-indicator" class="loader hidden"></div>
        <div id="error-message-box" class="hidden"></div>

        <div id="anime-grid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-5 md:gap-6">
            </div>

        <div id="pagination-controls" class="mt-10 flex justify-center items-center gap-3">
            </div>
    </div>

    <footer class="text-center p-6 mt-12 text-gray-500 text-sm border-t border-gray-700">
        <p>&copy; <span id="current-year"></span> Jadwal Anime. Ditenagai oleh <a href="https://jikan.moe" target="_blank" rel="noopener noreferrer" class="text-blue-400 hover:underline">Jikan API</a> (Unofficial MyAnimeList API).</p>
        <p>Dibuat dengan cinta untuk para pecinta anime.</p>
    </footer>

    <script>
        const API_BASE_URL = 'https://api.jikan.moe/v4';
        const ITEMS_PER_PAGE = 20; // Jikan defaultnya 25, bisa disesuaikan
        const animeGrid = document.getElementById('anime-grid');
        const loadingIndicator = document.getElementById('loading-indicator');
        const errorMessageDiv = document.getElementById('error-message-box');
        
        const btnNow = document.getElementById('btn-now');
        const btnUpcoming = document.getElementById('btn-upcoming');
        const btnToggleSpecific = document.getElementById('btn-toggle-specific');
        const specificSeasonSelector = document.getElementById('specific-season-selector');
        const yearSelect = document.getElementById('year-select');
        const seasonSelect = document.getElementById('season-select');
        const btnFetchSpecific = document.getElementById('btn-fetch-specific');
        const paginationControls = document.getElementById('pagination-controls');
        document.getElementById('current-year').textContent = new Date().getFullYear();

        let currentApiUrl = ''; 
        let currentPage = 1;
        let currentFilterType = 'now'; 
        let currentYear, currentSeason;

        function populateYearDropdown() {
            const currentYearVal = new Date().getFullYear();
            for (let i = currentYearVal + 1; i >= currentYearVal - 15; i--) { // Rentang tahun lebih besar
                const option = document.createElement('option');
                option.value = i;
                option.textContent = i;
                if (i === currentYearVal) {
                    option.selected = true;
                }
                yearSelect.appendChild(option);
            }
        }

        function showLoading() {
            loadingIndicator.classList.remove('hidden');
            animeGrid.innerHTML = ''; 
            errorMessageDiv.classList.add('hidden');
            paginationControls.innerHTML = ''; 
        }

        function hideLoading() {
            loadingIndicator.classList.add('hidden');
        }

        function showError(message) {
            errorMessageDiv.textContent = `Terjadi kesalahan: ${message}. Mohon coba beberapa saat lagi atau periksa koneksi internet Anda.`;
            errorMessageDiv.classList.remove('hidden');
            console.error("API Error:", message);
        }
        
        function setActiveButton(activeBtn) {
            [btnNow, btnUpcoming, btnToggleSpecific].forEach(btn => {
                btn.classList.remove('active');
            });
            if (activeBtn) {
                activeBtn.classList.add('active');
            }
        }

        async function fetchAnimeData(url) {
            showLoading();
            try {
                // Jeda kecil untuk menghindari rate limit jika pengguna mengklik terlalu cepat
                await new Promise(resolve => setTimeout(resolve, 350)); 
                const response = await fetch(url);
                if (!response.ok) {
                    const errorData = await response.json().catch(() => ({ message: 'Respons error tidak valid.' }));
                    throw new Error(`Error HTTP! Status: ${response.status}, Pesan: ${errorData.message || response.statusText}`);
                }
                const data = await response.json();
                return data;
            } catch (error) {
                showError(error.message);
                return null;
            } finally {
                hideLoading();
            }
        }

        function createAnimeCard(anime) {
            const card = document.createElement('div');
            card.className = 'anime-card shadow-lg';

            const title = anime.title_english || anime.title || 'Judul Tidak Tersedia';
            const imageUrl = anime.images?.jpg?.large_image_url || `https://placehold.co/300x450/1f2937/e0e0e0?text=Gambar+Tidak+Tersedia`;
            
            let synopsisText = anime.synopsis || 'Sinopsis tidak tersedia.';
            if (synopsisText.length > 200) { // Batas sinopsis lebih panjang sedikit
                 synopsisText = synopsisText.substring(0, 197) + '...';
            }

            const score = anime.score ? `⭐ ${anime.score.toFixed(2)}` : 'Skor: N/A';
            const episodes = anime.episodes ? `${anime.episodes} episode` : 'Episode: ?';
            const status = anime.status || 'N/A';
            const broadcast = anime.broadcast?.string || 'Belum diketahui';
            const studios = anime.studios?.map(s => s.name).join(', ') || 'N/A';
            // Ambil maks 3 genre
            const genres = anime.genres?.slice(0, 3).map(g => `<span class="bg-gray-700 px-2 py-0.5 rounded text-xs">${g.name}</span>`).join(' ') || 'N/A';
            const malUrl = anime.url || '#';
            const type = anime.type || 'N/A';

            card.innerHTML = `
                <img src="${imageUrl}" alt="Poster ${title.replace(/"/g, '&quot;')}" class="object-cover" onerror="this.onerror=null;this.src='https://placehold.co/300x450/1f2937/e0e0e0?text=Error+Muat+Gambar';">
                <div class="anime-card-content">
                    <h3 class="text-lg font-semibold mb-1 text-blue-300 truncate" title="${title.replace(/"/g, '&quot;')}">${title}</h3>
                    <div class="flex justify-between text-xs text-gray-400 mb-1">
                        <span>${type}</span>
                        <span>${episodes}</span>
                    </div>
                    <p class="text-sm text-yellow-400 mb-2 font-semibold">${score}</p>
                    <p class="text-xs text-gray-400 mb-1"><strong class="text-gray-300">Tayang:</strong> ${broadcast}</p>
                    <p class="text-xs text-gray-400 mb-2 truncate"><strong class="text-gray-300">Studio:</strong> ${studios}</p>
                    <div class="text-xs text-gray-400 mb-3"><strong class="text-gray-300">Genre:</strong> <span class="flex flex-wrap gap-1 mt-1">${genres}</span></div>
                    <p class="synopsis">${synopsisText}</p>
                    <div class="mt-auto pt-2 border-t border-gray-700"> 
                         <a href="${malUrl}" target="_blank" rel="noopener noreferrer" class="block w-full text-sm btn btn-secondary py-2">
                            Info Lengkap di MyAnimeList
                         </a>
                    </div>
                </div>
            `;
            return card;
        }

        function displayAnime(data) {
            animeGrid.innerHTML = ''; 
            if (!data || !data.data || data.data.length === 0) {
                animeGrid.innerHTML = '<p class="col-span-full text-center text-gray-400 py-10 text-lg">Tidak ada anime yang ditemukan untuk kriteria ini.</p>';
                paginationControls.innerHTML = '';
                return;
            }

            data.data.forEach(anime => {
                const card = createAnimeCard(anime);
                animeGrid.appendChild(card);
            });
            
            setupPagination(data.pagination);
        }
        
        function setupPagination(paginationData) {
            paginationControls.innerHTML = ''; 
            if (!paginationData || !paginationData.last_visible_page || paginationData.last_visible_page <= 1) {
                 return; // Tidak perlu paginasi jika hanya 1 halaman atau tidak ada data
            }

            const { current_page, last_visible_page, has_next_page } = paginationData;
            currentPage = current_page; // Update currentPage global

            if (current_page > 1) {
                const prevButton = document.createElement('button');
                prevButton.innerHTML = '&laquo; Sebelumnya';
                prevButton.className = 'btn btn-secondary';
                prevButton.onclick = () => fetchAndDisplayAnime(currentFilterType, currentYear, currentSeason, current_page - 1);
                paginationControls.appendChild(prevButton);
            }

            const pageInfo = document.createElement('span');
            pageInfo.textContent = `Halaman ${current_page} dari ${last_visible_page}`;
            pageInfo.className = 'text-gray-300 px-2';
            paginationControls.appendChild(pageInfo);

            if (has_next_page) {
                const nextButton = document.createElement('button');
                nextButton.innerHTML = 'Berikutnya &raquo;';
                nextButton.className = 'btn btn-secondary';
                nextButton.onclick = () => fetchAndDisplayAnime(currentFilterType, currentYear, currentSeason, current_page + 1);
                paginationControls.appendChild(nextButton);
            }
        }

        async function fetchAndDisplayAnime(type, year, season, page = 1) {
            currentFilterType = type; 
            currentYear = year;       
            currentSeason = season;   
            currentPage = page;       

            let url;
            const params = `?page=${page}&limit=${ITEMS_PER_PAGE}&sfw=true`; // sfw untuk konten aman
            
            switch (type) {
                case 'now':
                    url = `${API_BASE_URL}/seasons/now${params}`;
                    setActiveButton(btnNow);
                    specificSeasonSelector.classList.add('hidden');
                    break;
                case 'upcoming':
                    url = `${API_BASE_URL}/seasons/upcoming${params}`;
                    setActiveButton(btnUpcoming);
                    specificSeasonSelector.classList.add('hidden');
                    break;
                case 'specific':
                    if (!year || !season) {
                        showError("Tahun dan Musim harus dipilih untuk filter spesifik.");
                        setActiveButton(btnToggleSpecific); // Tetap aktifkan tombol ini
                        return;
                    }
                    url = `${API_BASE_URL}/seasons/${year}/${season}${params}`;
                    setActiveButton(btnToggleSpecific); // Aktifkan tombol 'Pilih Musim'
                    specificSeasonSelector.classList.remove('hidden'); // Pastikan selector terlihat
                    break;
                default:
                    showError("Tipe filter tidak valid.");
                    return;
            }
            
            currentApiUrl = url; 
            const animeData = await fetchAnimeData(url);
            if (animeData) {
                displayAnime(animeData);
            }
        }

        // Event Listeners
        btnNow.addEventListener('click', () => {
            fetchAndDisplayAnime('now');
        });

        btnUpcoming.addEventListener('click', () => {
            fetchAndDisplayAnime('upcoming');
        });

        btnToggleSpecific.addEventListener('click', () => {
            specificSeasonSelector.classList.toggle('hidden');
            if (!specificSeasonSelector.classList.contains('hidden')) {
                setActiveButton(btnToggleSpecific); // Aktifkan hanya saat form muncul
            } else {
                // Jika form disembunyikan, dan filter aktif bukan 'now' atau 'upcoming', kembalikan ke 'now'
                // atau setidaknya nonaktifkan tombol 'Pilih Musim' jika yang lain aktif
                if (currentFilterType !== 'now' && currentFilterType !== 'upcoming') {
                    fetchAndDisplayAnime('now'); // Kembali ke 'now' jika menutup form tanpa memilih
                } else {
                    setActiveButton(currentFilterType === 'now' ? btnNow : btnUpcoming);
                }
            }
        });

        btnFetchSpecific.addEventListener('click', () => {
            const selectedYear = yearSelect.value;
            const selectedSeason = seasonSelect.value;
            fetchAndDisplayAnime('specific', selectedYear, selectedSeason);
        });

        // Initial Load
        document.addEventListener('DOMContentLoaded', () => {
            populateYearDropdown();
            fetchAndDisplayAnime('now'); // Muat musim ini secara default
        });
    </script>
</body>
</html>
