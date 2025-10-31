<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="theme-color" content="#007BFF" />
  
  <!-- SEO & Open Graph -->
  <title>Örnek Makale Başlığı | Modern Blog</title>
  <meta name="description" content="Bu örnek makale, modern bir blog sayfasının tüm özelliklerini içerir: paylaşım, indirme, içindekiler, premium içerik ve daha fazlası." />
  <meta property="og:title" content="Örnek Makale Başlığı" />
  <meta property="og:description" content="Modern, etkileşimli ve kullanıcı dostu bir makale sayfası örneği." />
  <meta property="og:type" content="article" />
  <meta property="og:url" content="https://ornek.com/makale" />
  <meta property="og:image" content="https://ornek.com/thumbnail.jpg" />
  <meta property="og:locale" content="tr_TR" />

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Poppins', 'system-ui', 'sans-serif'],
          },
        },
      },
    }
  </script>

  <!-- Font Awesome -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" />

  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet" />

  <!-- JSZip & FileSaver -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>

  <!-- html2pdf -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

  <style>
    :root {
      --primary: #007BFF;
      --primary-dark: #0069d9;
      --text: #333;
      --bg: #f0f2f5;
      --card: #ffffff;
      --border: #eee;
    }

    .dark {
      --text: #e2e8f0;
      --bg: #0f172a;
      --card: #1e293b;
      --border: #334155;
    }

    body {
      font-family: 'Poppins', sans-serif;
      background: var(--bg);
      color: var(--text);
      margin: 0;
      padding: 0;
      transition: background 0.3s ease;
    }

    .sidebar {
      width: 320px;
      background: var(--card);
      height: 100vh;
      position: fixed;
      left: -320px;
      top: 0;
      transition: left 0.3s ease;
      box-shadow: 2px 0 15px rgba(0, 0, 0, 0.1);
      z-index: 1001;
      overflow-y: auto;
    }

    .sidebar.active { left: 0; }

    .overlay {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.5);
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
    }

    .overlay.active { opacity: 1; visibility: visible; }

    .content {
      background: var(--card);
      padding: 2.5rem;
      border-radius: 1rem;
      box-shadow: 0 5px 20px rgba(0, 0, 0, 0.05);
      max-width: 900px;
      margin: 6rem auto 2rem;
      transition: all 0.3s ease;
    }

    .article-title-main {
      font-size: 2.5rem;
      font-weight: 700;
      background: linear-gradient(135deg, var(--primary), #00c6ff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin-bottom: 1.5rem;
    }

    .premium-banner {
      background: linear-gradient(135deg, var(--primary), #00c6ff);
      color: white;
      padding: 1.5rem;
      border-radius: 0.75rem;
      margin: 2rem 0;
      position: relative;
      overflow: hidden;
    }

    .notification {
      position: fixed;
      bottom: 1.5rem;
      left: 50%;
      transform: translateX(-50%);
      background: #10b981;
      color: white;
      padding: 0.75rem 1.5rem;
      border-radius: 0.5rem;
      opacity: 0;
      transition: opacity 0.3s ease;
      z-index: 1003;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }

    .notification.show { opacity: 1; }

    .progress-container {
      height: 8px;
      background: #e5e7eb;
      border-radius: 4px;
      overflow: hidden;
      margin-top: 1rem;
      display: none;
    }

    .progress-bar {
      height: 100%;
      background: var(--primary);
      width: 0;
      transition: width 0.3s ease;
    }

    @media (max-width: 768px) {
      .content { padding: 1.5rem; margin: 5rem 1rem 0; }
      .article-title-main { font-size: 2rem; }
    }
  </style>
</head>
<body class="transition-colors duration-300">

  <!-- Toggle Button -->
  <button id="toggleSidebar" class="fixed left-5 top-5 bg-[var(--primary)] text-white p-3 rounded-lg shadow-lg z-[1002] hover:bg-[var(--primary-dark)] transition">
    <i class="fas fa-bars"></i>
  </button>

  <!-- Overlay -->
  <div id="overlay" class="overlay"></div>

  <!-- Sidebar -->
  <aside id="sidebar" class="sidebar">
    <div class="p-5 bg-[var(--primary)] text-white">
      <h1 class="text-xl font-bold">Modern Blog</h1>
      <p class="text-sm opacity-90">Örnek Makale Başlığı</p>
    </div>
    <div class="p-5">
      <h2 class="text-lg font-semibold mb-4 text-[var(--text)]">İçindekiler</h2>
      <nav>
        <ul class="space-y-1">
          <li><a href="#giris" class="block px-4 py-2 rounded-lg hover:bg-blue-50 dark:hover:bg-slate-700 transition">Giriş</a></li>
          <li><a href="#bolum1" class="block px-4 py-2 rounded-lg hover:bg-blue-50 dark:hover:bg-slate-700 transition">Bölüm 1</a></li>
          <li><a href="#bolum2" class="block px-4 py-2 rounded-lg hover:bg-blue-50 dark:hover:bg-slate-700 transition">Bölüm 2</a></li>
          <li><a href="#sonuc" class="block px-4 py-2 rounded-lg hover:bg-blue-50 dark:hover:bg-slate-700 transition">Sonuç</a></li>
        </ul>
      </nav>
    </div>
  </aside>

  <!-- Main Content -->
  <main class="max-w-4xl mx-auto px-4">
    <article class="content">
      <header class="mb-8">
        <h1 id="giris" class="article-title-main">Örnek Makale Başlığı</h1>

        <div class="flex items-center gap-4 mb-6 bg-gray-50 dark:bg-slate-800 p-4 rounded-lg">
          <div class="w-14 h-14 bg-[var(--primary)] rounded-full flex items-center justify-center text-white text-xl">
            <i class="fas fa-user"></i>
          </div>
          <div>
            <div class="font-semibold flex items-center gap-1">
              Ahmet Yılmaz
              <i class="fas fa-check-circle text-[var(--primary)]" title="Onaylı Yazar"></i>
            </div>
            <div class="text-sm text-gray-600 dark:text-gray-400">15 Haziran 2023</div>
          </div>
        </div>

        <div class="flex gap-6 text-sm text-gray-600 dark:text-gray-400">
          <span><i class="fas fa-clock"></i> <span id="readingTime">5 dk</span></span>
          <span><i class="fas fa-sync"></i> <span id="updateDate">Güncellendi</span></span>
        </div>
      </header>

      <section class="prose prose-lg max-w-none dark:prose-invert">
        <p>Bu örnek bir makale metnidir. Türkçe içerik ile doldurulmuş, modern ve etkileşimli bir yazı sayfası gösterimi yapılmaktadır.</p>

        <h2 id="bolum1">Bölüm 1: Temel Bilgiler</h2>
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam in dui mauris. Vivamus hendrerit arcu sed erat molestie vehicula.</p>

        <!-- Download Options -->
        <div class="my-8 p-6 bg-gray-50 dark:bg-slate-800 rounded-xl">
          <h3 class="text-lg font-semibold mb-4">İçeriği İndir</h3>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <button onclick="downloadContent('full')" class="p-4 bg-white dark:bg-slate-700 rounded-lg shadow hover:shadow-md transition text-center">
              <i class="fas fa-file-archive text-3xl text-[var(--primary)] mb-2"></i>
              <div class="font-medium">Tam Paket</div>
              <div class="text-xs text-gray-500">Metin + Kaynaklar</div>
            </button>
            <button onclick="downloadContent('text')" class="p-4 bg-white dark:bg-slate-700 rounded-lg shadow hover:shadow-md transition text-center">
              <i class="fas fa-file-alt text-3xl text-[var(--primary)] mb-2"></i>
              <div class="font-medium">Sadece Metin</div>
              <div class="text-xs text-gray-500">TXT formatında</div>
            </button>
            <button onclick="downloadContent('pdf')" class="p-4 bg-white dark:bg-slate-700 rounded-lg shadow hover:shadow-md transition text-center">
              <i class="fas fa-file-pdf text-3xl text-red-500 mb-2"></i>
              <div class="font-medium">PDF İndir</div>
              <div class="text-xs text-gray-500">Yazdırılabilir</div>
            </button>
          </div>
          <div id="progressContainer" class="progress-container">
            <div id="progressBar" class="progress-bar"></div>
          </div>
        </div>

        <!-- Premium Banner -->
        <div class="premium-banner">
          <i class="fas fa-crown absolute top-5 right-5 text-3xl opacity-50"></i>
          <h3 class="text-xl font-bold mb-2 relative z-10">Premium İçerik</h3>
          <p class="opacity-90 relative z-10">Bu bölüm yalnızca premium üyeler için erişilebilir.</p>
        </div>

        <!-- Hidden Premium Content -->
        <div id="premiumContent" style="display: none;">
          <h2 id="bolum2">Bölüm 2: İleri Seviye</h2>
          <p>Premium içerik burada yer alır. Gerçek uygulamada backend ile kontrol edilir.</p>
        </div>

        <h2 id="sonuc">Sonuç</h2>
        <p>Makale sona erdi. Teşekkürler!</p>
      </section>

      <!-- Share Section -->
      <div class="mt-12 pt-8 border-t border-[var(--border)]">
        <button id="shareBtn" class="bg-[var(--primary)] hover:bg-[var(--primary-dark)] text-white font-medium py-3 px-6 rounded-lg flex items-center gap-2 transition">
          <i class="fas fa-share-alt"></i> Paylaş
        </button>

        <div id="shareMenu" class="mt-4 p-6 bg-gray-50 dark:bg-slate-800 rounded-xl opacity-0 invisible transition-all duration-300">
          <h4 class="text-center font-medium mb-4">Makaleyi Paylaş</h4>
          <div class="flex justify-center gap-4 mb-4">
            <a href="#" onclick="share('facebook')" class="w-12 h-12 bg-blue-600 text-white rounded-full flex items-center justify-center hover:scale-110 transition"><i class="fab fa-facebook-f"></i></a>
            <a href="#" onclick="share('twitter')" class="w-12 h-12 bg-sky-500 text-white rounded-full flex items-center justify-center hover:scale-110 transition"><i class="fab fa-twitter"></i></a>
            <a href="#" onclick="share('linkedin')" class="w-12 h-12 bg-blue-800 text-white rounded-full flex items-center justify-center hover:scale-110 transition"><i class="fab fa-linkedin-in"></i></a>
            <a href="#" onclick="share('whatsapp')" class="w-12 h-12 bg-green-500 text-white rounded-full flex items-center justify-center hover:scale-110 transition"><i class="fab fa-whatsapp"></i></a>
          </div>
          <div class="flex gap-2">
            <input type="text" id="shareUrl" value="https://ornek.com/makale" readonly class="flex-1 px-3 py-2 bg-white dark:bg-slate-700 border border-[var(--border)] rounded-lg text-sm">
            <button id="copyBtn" class="px-4 py-2 bg-[var(--primary)] text-white rounded-lg hover:bg-[var(--primary-dark)] transition">Kopyala</button>
          </div>
          <div class="flex gap-2 mt-3">
            <button onclick="window.print()" class="flex-1 py-2 bg-gray-200 dark:bg-slate-700 rounded-lg flex items-center justify-center gap-2 hover:bg-gray-300 dark:hover:bg-slate-600 transition">
              <i class="fas fa-print"></i> Yazdır
            </button>
          </div>
        </div>
      </div>
    </article>
  </main>

  <!-- Notification -->
  <div id="notification" class="notification">Kopyalandı!</div>

  <script>
    // === CONFIG ===
    const ARTICLE_URL = window.location.href;
    const ARTICLE_TITLE = document.title;

    // === DOM Elements ===
    const els = {
      sidebar: document.getElementById('sidebar'),
      overlay: document.getElementById('overlay'),
      toggleBtn: document.getElementById('toggleSidebar'),
      shareBtn: document.getElementById('shareBtn'),
      shareMenu: document.getElementById('shareMenu'),
      copyBtn: document.getElementById('copyBtn'),
      shareUrl: document.getElementById('shareUrl'),
      notification: document.getElementById('notification'),
      progressContainer: document.getElementById('progressContainer'),
      progressBar: document.getElementById('progressBar'),
      readingTime: document.getElementById('readingTime'),
      updateDate: document.getElementById('updateDate'),
    };

    // === INIT ===
    document.addEventListener('DOMContentLoaded', () => {
      updateMetadata();
      initSidebar();
      initShare();
      initCopy();
    });

    // === METADATA ===
    function updateMetadata() {
      const today = new Date().toLocaleDateString('tr-TR', { day: 'numeric', month: 'long', year: 'numeric' });
      els.updateDate.textContent = `Son güncelleme: ${today}`;

      const text = document.querySelector('article').textContent;
      const words = text.split(/\s+/).filter(w => w).length;
      const minutes = Math.max(1, Math.ceil(words / 200));
      els.readingTime.textContent = `${minutes} dk okuma`;
    }

    // === SIDEBAR ===
    function initSidebar() {
      els.toggleBtn.addEventListener('click', toggleSidebar);
      els.overlay.addEventListener('click', closeSidebar);
      document.querySelectorAll('.sidebar a').forEach(link => {
        link.addEventListener('click', closeSidebar);
      });
    }

    function toggleSidebar() {
      els.sidebar.classList.toggle('active');
      els.overlay.classList.toggle('active');
    }

    function closeSidebar() {
      els.sidebar.classList.remove('active');
      els.overlay.classList.remove('active');
    }

    // === SHARE ===
    function initShare() {
      els.shareBtn.addEventListener('click', (e) => {
        e.stopPropagation();
        els.shareMenu.classList.toggle('opacity-0');
        els.shareMenu.classList.toggle('invisible');
      });

      document.addEventListener('click', (e) => {
        if (!els.shareBtn.contains(e.target) && !els.shareMenu.contains(e.target)) {
          els.shareMenu.classList.add('opacity-0', 'invisible');
        }
      });
    }

    window.share = function(platform) {
      const url = encodeURIComponent(ARTICLE_URL);
      const text = encodeURIComponent(ARTICLE_TITLE);
      const maps = {
        facebook: `https://www.facebook.com/sharer/sharer.php?u=${url}`,
        twitter: `https://twitter.com/intent/tweet?url=${url}&text=${text}`,
        linkedin: `https://www.linkedin.com/shareArticle?mini=true&url=${url}&title=${text}`,
        whatsapp: `https://api.whatsapp.com/send?text=${text}%20${url}`
      };
      window.open(maps[platform], '_blank');
    };

    // === COPY URL ===
    function initCopy() {
      els.copyBtn.addEventListener('click', () => {
        els.shareUrl.select();
        document.execCommand('copy');
        notify('Bağlantı kopyalandı!');
      });
    }

    // === DOWNLOAD CONTENT ===
    window.downloadContent = async function(type) {
      if (type === 'pdf') {
        downloadPDF();
        return;
      }

      showProgress();
      const zip = new JSZip();
      const title = document.querySelector('.article-title-main').textContent;
      const content = document.querySelector('article').innerText;

      if (type === 'full' || type === 'text') {
        zip.file("makale.txt", `${title}\n\n${content}`);
        zip.file("bilgi.txt", `Yazar: Ahmet Yılmaz\nTarih: ${new Date().toLocaleDateString('tr-TR')}\nURL: ${ARTICLE_URL}`);
      }

      if (type === 'full') {
        zip.file("kaynaklar/ornek.svg", createSampleSVG());
      }

      zip.file("README.txt", `Bu dosya ${title} makalesi için oluşturulmuştur.\n© ${new Date().getFullYear()}`);

      simulateProgress(100, () => {
        zip.generateAsync({ type: "blob" }).then(blob => {
          saveAs(blob, type === 'text' ? 'makale-metin.zip' : 'makale-tam.zip');
          hideProgress();
          notify('İndirme tamamlandı!');
        });
      });
    };

    function downloadPDF() {
      const element = document.querySelector('.content');
      html2pdf()
        .set({ margin: 1, filename: 'makale.pdf', html2canvas: { scale: 2 } })
        .from(element)
        .save();
      notify('PDF oluşturuluyor...');
    }

    function showProgress() {
      els.progressContainer.style.display = 'block';
      els.progressBar.style.width = '0%';
    }

    function hideProgress() {
      setTimeout(() => {
        els.progressContainer.style.display = 'none';
      }, 1000);
    }

    function simulateProgress(target, callback) {
      let progress = 0;
      const interval = setInterval(() => {
        progress += 5;
        els.progressBar.style.width = `${progress}%`;
        if (progress >= target) {
          clearInterval(interval);
          callback();
        }
      }, 50);
    }

    function createSampleSVG() {
      return `<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">
        <rect width="200" height="200" fill="#f0f0f0"/>
        <text x="100" y="100" font-family="Arial" font-size="16" text-anchor="middle" dominant-baseline="middle">Örnek Görsel</text>
      </svg>`;
    }

    // === NOTIFICATION ===
    function notify(message, color = '#10b981') {
      els.notification.textContent = message;
      els.notification.style.backgroundColor = color;
      els.notification.classList.add('show');
      setTimeout(() => els.notification.classList.remove('show'), 2000);
    }

    // === DARK MODE TOGGLE (Optional) ===
    // Uncomment to add toggle button
    /*
    const darkToggle = document.createElement('button');
    darkToggle.innerHTML = '<i class="fas fa-moon"></i>';
    darkToggle.className = 'fixed right-5 top-5 bg-slate-800 text-white p-3 rounded-lg z-50';
    darkToggle.onclick = () => document.body.classList.toggle('dark');
    document.body.appendChild(darkToggle);
    */
  </script>
</body>
</html>
