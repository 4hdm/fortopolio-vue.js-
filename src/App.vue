<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { 
  Sun, Moon, Github, Linkedin, Mail, Code2, 
  Briefcase, User, Cpu, Sparkles, ChevronRight, Menu, X, 
  Download, Send, CheckCircle2, ArrowUpRight, FolderGit2,
  Terminal, Layers, Globe, GraduationCap, Heart, Gamepad2, 
  BookOpen, Music, Dumbbell
} from 'lucide-vue-next';

// Data Portofolio
const PORTFOLIO_DATA = {
  personal: {
    name: "AHDAM ASHARI",
    role: "Junior Frontend & Backend Developer",
    bio: "Saya membangun pengalaman web yang responsif, cepat, dan berfokus pada pengguna dengan teknologi JavaScript modern serta backend development.",
    location: "Bontang, Kalimatan Timur, Indonesia",
    email: "ahdamashari12@gmail.com",
    github: "https://github.com/4hdm",
    linkedin: "https://linkedin.com",
    // Ganti URL gambar di bawah ini dengan foto kamu
    avatar: "foto profil.png",
    stats: [
      { label: "Tahun Pengalaman", value: "2+" },
      { label: "Proyek Selesai", value: "8+" },
      { label: "Klien / Tugas Puas", value: "10+" },
      { label: "Komit / Kontribusi", value: "150+" }
    ]
  },
  education: [
    {
      degree: "S1 Teknik Informatika",
      institution: "Sekolah Tinggi Teknologi Bontang (STITEK)",
      period: "2024 - Sekarang",
      description: "Fokus pada pengembangan perangkat lunak, struktur data, basis data, dan kecerdasan buatan."
    },
    { 
      degree: "SMA Jurusan IPA",
      institution: "SEKOLAH MENENGAH ATAS NEGERI 2 BONTANG",
      period: "2019 - 2022",
      description: "Fokus pada pengembangan diri baik soft dan hardskill, berfokus dalam bidang menghitung atau matematika."
    }
  ],
  hobbies: [
    { name: "Programming & Open Source", icon: Code2, description: "Eksplorasi framework baru dan ngoding proyek sampingan." },
    { name: "Gaming", icon: Gamepad2, description: "Bermain game strategi dan RPG untuk refreshing." },
    { name: "Membaca & Riset Tech", icon: BookOpen, description: "Membaca dokumentasi, artikel teknologi, dan jurnal." },
    { name: "Mendengarkan Musik", icon: Music, description: "Menemanimu saat coding agar lebih fokus." },
    { name: "Basket & Gym", icon: Dumbbell, description: "Membantu menjaga agar tubuh tetap sehat dan fit." }
  ],
  skills: [
    { name: "Python", category: "Backend", level: 80, icon: Code2 },
    { name: "C++", category: "Backend", level: 75, icon: Code2 },
    { name: "Vue.js", category: "Frontend", level: 80, icon: Code2 },
    { name: "React.js", category: "Frontend", level: 70, icon: Code2 },
    { name: "JavaScript (ES6+)", category: "Frontend", level: 85, icon: Terminal },
    { name: "TypeScript", category: "Frontend", level: 80, icon: Code2 },
    { name: "Tailwind CSS", category: "Frontend", level: 85, icon: Layers },
    { name: "Next.js", category: "Frontend", level: 80, icon: Globe },
    { name: "Node.js", category: "Backend", level: 70, icon: Cpu },
    { name: "MySQL", category: "Backend", level: 80, icon: Globe },
    { name: "Git / GitHub", category: "Tools", level: 85, icon: FolderGit2 },
    { name: "Figma (UI/UX)", category: "Design", level: 70, icon: Sparkles },
    { name: "Vite / Webpack", category: "Tools", level: 80, icon: Cpu },
  ],
  projects: [
    {
      id: 1,
      title: "E-Commerce Dashboard Pro",
      category: "Web App",
      description: "Platform analitik toko online interaktif dengan visualisasi data real-time, manajemen inventaris, dan tema gelap/terang.",
      image: "https://placehold.co/600x400/1e293b/38bdf8?text=E-Commerce+Dashboard",
      tags: ["React", "Tailwind CSS", "Recharts", "Redux Toolkit"],
      liveLink: "https://example.com",
      githubLink: "https://github.com"
    },
    {
      id: 2,
      title: "SaaS Task Management System",
      category: "Web App",
      description: "Aplikasi manajemen tugas kolaboratif dengan fitur Kanban board, pengingat, dan integrasi kalender.",
      image: "https://placehold.co/600x400/0f172a/a855f7?text=Task+Management+App",
      tags: ["TypeScript", "Next.js", "Tailwind", "Framer Motion"],
      liveLink: "https://example.com",
      githubLink: "https://github.com"
    },
    {
      id: 3,
      title: "AI Mobile Companion UI",
      category: "Mobile",
      description: "Desain antarmuka responsif mobile-first untuk asisten AI pribadi dengan perintah suara dan mikro-interaksi.",
      image: "https://placehold.co/600x400/18181b/10b981?text=AI+Mobile+UI",
      tags: ["React Native", "Tailwind CSS", "Expo"],
      liveLink: "https://example.com",
      githubLink: "https://github.com"
    },
    {
      id: 4,
      title: "Crypto Wallet & Portfolio Tracker",
      category: "Web App",
      description: "Pelacak portofolio aset kripto real-time dengan integrasi API CoinGecko dan konversi mata uang otomatis.",
      image: "https://placehold.co/600x400/1e1b4b/818cf8?text=Crypto+Tracker",
      tags: ["React", "Chart.js", "REST API", "Tailwind"],
      liveLink: "https://example.com",
      githubLink: "https://github.com"
    }
  ],
  experiences: [
    {
      role: "Frontend & Backend Developer",
      company: "Proyek Independen & Akademik",
      period: "2023 - Sekarang",
      description: "Mengembangkan aplikasi web full-stack, slicing UI responsif, serta merancang basis data MySQL dan script backend."
    },
    {
      role: "Junior Web Developer",
      company: "StartUp Nusantara",
      period: "2021 - 2022",
      description: "Mengimplementasikan slicing desain UI Figma ke kode HTML/CSS/JavaScript serta membantu integrasi API backend."
    }
  ]
};

// Reactive State
const darkMode = ref(true);
const mobileMenuOpen = ref(false);
const activeSection = ref('hero');
const projectCategory = ref('All');
const skillCategory = ref('All');
const selectedProject = ref(null);
const formSubmitted = ref(false);

const categories = ['All', 'Frontend', 'Backend', 'Tools', 'Design'];
const navItems = ['about', 'education', 'skills', 'projects', 'experience', 'hobbies', 'contact'];

// Computed / Filtered Data
const filteredProjects = computed(() => {
  if (projectCategory.value === 'All') return PORTFOLIO_DATA.projects;
  return PORTFOLIO_DATA.projects.filter(p => p.category === projectCategory.value);
});

const filteredSkills = computed(() => {
  if (skillCategory.value === 'All') return PORTFOLIO_DATA.skills;
  return PORTFOLIO_DATA.skills.filter(s => s.category === skillCategory.value);
});

// Toggle Dark Mode Class
const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
  if (darkMode.value) {
    document.documentElement.classList.add('dark');
  } else {
    document.documentElement.classList.remove('dark');
  }
};

// Scroll Handler
const handleScroll = () => {
  const sections = ['hero', 'about', 'education', 'skills', 'projects', 'experience', 'hobbies', 'contact'];
  const scrollPosition = window.scrollY + 200;

  for (const section of sections) {
    const el = document.getElementById(section);
    if (el) {
      const top = el.offsetTop;
      const height = el.offsetHeight;
      if (scrollPosition >= top && scrollPosition < top + height) {
        activeSection.value = section;
        break;
      }
    }
  }
};

// Form Submit Handler
const handleFormSubmit = () => {
  formSubmitted.value = true;
  setTimeout(() => {
    formSubmitted.value = false;
  }, 4000);
};

// Lifecycle Hooks
onMounted(() => {
  if (darkMode.value) {
    document.documentElement.classList.add('dark');
  }
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<template>
  <div :class="['min-h-screen transition-colors duration-300', darkMode ? 'bg-slate-950 text-slate-100' : 'bg-slate-50 text-slate-800']">
    
    <!-- Header -->
    <header :class="['sticky top-0 z-50 backdrop-blur-md border-b transition-colors duration-300', darkMode ? 'bg-slate-950/80 border-slate-800' : 'bg-white/80 border-slate-200']">
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
        
        <!-- Logo -->
        <a href="#hero" class="flex items-center gap-2 font-bold text-xl tracking-tight text-indigo-500 hover:text-indigo-400 transition">
          <Code2 class="w-6 h-6" />
          <span>Ahdam<span class="text-slate-400 font-normal">.dev</span></span>
        </a>

        <!-- Desktop Nav -->
        <nav class="hidden md:flex items-center gap-6 text-sm font-medium">
          <a
            v-for="item in navItems"
            :key="item"
            :href="`#${item}`"
            :class="[
              'capitalize transition-colors',
              activeSection === item 
                ? 'text-indigo-500 font-semibold' 
                : darkMode ? 'text-slate-400 hover:text-slate-200' : 'text-slate-600 hover:text-slate-900'
            ]"
          >
            {{ item }}
          </a>
        </nav>

        <!-- Controls -->
        <div class="flex items-center gap-3">
          <button
            @click="toggleDarkMode"
            :class="[
              'p-2 rounded-xl transition-all',
              darkMode ? 'bg-slate-800 text-amber-400 hover:bg-slate-700' : 'bg-slate-200 text-slate-700 hover:bg-slate-300'
            ]"
            aria-label="Toggle Theme"
          >
            <Sun v-if="darkMode" class="w-5 h-5" />
            <Moon v-else class="w-5 h-5" />
          </button>

          <!-- Mobile Hamburger -->
          <button
            @click="mobileMenuOpen = !mobileMenuOpen"
            class="md:hidden p-2 rounded-xl text-slate-400 hover:text-slate-200"
          >
            <X v-if="mobileMenuOpen" class="w-6 h-6" />
            <Menu v-else class="w-6 h-6" />
          </button>
        </div>
      </div>

      <!-- Mobile Nav Menu -->
      <div 
        v-if="mobileMenuOpen" 
        :class="['md:hidden border-b px-4 py-4 space-y-3', darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200']"
      >
        <a
          v-for="item in navItems"
          :key="item"
          :href="`#${item}`"
          @click="mobileMenuOpen = false"
          :class="[
            'block py-2 capitalize font-medium text-base',
            activeSection === item ? 'text-indigo-500 font-bold' : darkMode ? 'text-slate-300' : 'text-slate-700'
          ]"
        >
          {{ item }}
        </a>
      </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 space-y-24 py-12">
      
      <!-- Hero Section -->
      <section id="hero" class="min-h-[80vh] flex flex-col justify-center items-center text-center relative pt-8">
        <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-72 h-72 sm:w-96 sm:h-96 bg-indigo-500/20 rounded-full blur-3xl -z-10 animate-pulse pointer-events-none" />

        <!-- Foto Profil -->
        <div class="relative mb-6">
          <div class="w-32 h-32 sm:w-40 sm:h-40 rounded-full overflow-hidden border-4 border-indigo-500/30 shadow-xl shadow-indigo-500/20">
            <img :src="PORTFOLIO_DATA.personal.avatar" :alt="PORTFOLIO_DATA.personal.name" class="w-full h-full object-cover" />
          </div>
          <div class="absolute bottom-1 right-1 bg-emerald-500 w-4 h-4 rounded-full border-2 border-slate-950" title="Available for work" />
        </div>

        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-semibold bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 mb-6">
          <Sparkles class="w-3.5 h-3.5" />
          <span>Terbuka untuk Peluang & Proyek Baru</span>
        </div>

        <h1 class="text-4xl sm:text-6xl font-extrabold tracking-tight max-w-3xl leading-tight">
          Hai, Saya <span class="bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 bg-clip-text text-transparent">{{ PORTFOLIO_DATA.personal.name }}</span>
        </h1>

        <p class="mt-4 text-lg sm:text-xl text-slate-400 max-w-2xl">
          {{ PORTFOLIO_DATA.personal.role }} yang berfokus pada pembuatan antarmuka web modern, interaktif, dan performa tinggi.
        </p>

        <!-- Action Buttons -->
        <div class="mt-8 flex flex-wrap justify-center gap-4">
          <a
            href="#projects"
            class="inline-flex items-center gap-2 px-6 py-3 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white font-medium transition shadow-lg shadow-indigo-500/25"
          >
            Lihat Portofolio <ChevronRight class="w-4 h-4" />
          </a>
          <a
            href="#contact"
            :class="[
              'inline-flex items-center gap-2 px-6 py-3 rounded-xl font-medium border transition',
              darkMode ? 'border-slate-800 bg-slate-900/50 hover:bg-slate-800 text-slate-200' : 'border-slate-300 bg-white hover:bg-slate-100 text-slate-800'
            ]"
          >
            Hubungi Saya
          </a>
        </div>

        <!-- Social Links -->
        <div class="mt-10 flex items-center gap-5 text-slate-400">
          <a :href="PORTFOLIO_DATA.personal.github" target="_blank" rel="noreferrer" class="hover:text-indigo-400 transition p-2">
            <Github class="w-5 h-5" />
          </a>
          <a :href="PORTFOLIO_DATA.personal.linkedin" target="_blank" rel="noreferrer" class="hover:text-indigo-400 transition p-2">
            <Linkedin class="w-5 h-5" />
          </a>
          <a :href="`mailto:${PORTFOLIO_DATA.personal.email}`" class="hover:text-indigo-400 transition p-2">
            <Mail class="w-5 h-5" />
          </a>
        </div>
      </section>

      <!-- About Section -->
      <section id="about" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-8">
          <User class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Tentang Saya</h2>
        </div>

        <div class="grid md:grid-cols-2 gap-8 items-center">
          <div :class="['p-6 rounded-2xl border shadow-sm space-y-4', darkMode ? 'bg-slate-900/50 border-slate-800' : 'bg-white border-slate-200']">
            <p class="leading-relaxed text-slate-300">
              {{ PORTFOLIO_DATA.personal.bio }}
            </p>
            <p class="leading-relaxed text-slate-400 text-sm">
              Berlokasi di {{ PORTFOLIO_DATA.personal.location }}. Saya selalu tertarik mempelajari teknologi web terbaru, meningkatkan aksesibilitas web, dan mengoptimalkan performa aplikasi.
            </p>
            
            <div class="pt-2">
              <a
                href="#contact"
                class="inline-flex items-center gap-2 text-indigo-400 font-medium hover:underline text-sm"
              >
                <Download class="w-4 h-4" /> Unduh Ringkasan CV (PDF)
              </a>
            </div>
          </div>

          <!-- Stats Grid -->
          <div class="grid grid-cols-2 gap-4">
            <div 
              v-for="(stat, idx) in PORTFOLIO_DATA.personal.stats" 
              :key="idx" 
              :class="['p-5 rounded-2xl border text-center', darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-slate-100 border-slate-200']"
            >
              <div class="text-3xl font-extrabold text-indigo-500">{{ stat.value }}</div>
              <div class="text-xs text-slate-400 mt-1 font-medium">{{ stat.label }}</div>
            </div>
          </div>
        </div>
      </section>

      <!-- Education Section -->
      <section id="education" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-8">
          <GraduationCap class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Riwayat Pendidikan</h2>
        </div>

        <div class="grid gap-4">
          <div 
            v-for="(edu, idx) in PORTFOLIO_DATA.education" 
            :key="idx"
            :class="['p-6 rounded-2xl border flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4', darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-white border-slate-200']"
          >
            <div>
              <h3 class="text-lg font-bold text-indigo-400">{{ edu.degree }}</h3>
              <p class="text-sm font-medium text-slate-300">{{ edu.institution }}</p>
              <p class="text-xs text-slate-400 mt-2 max-w-xl leading-relaxed">{{ edu.description }}</p>
            </div>
            <span class="text-xs px-3 py-1 rounded-full bg-indigo-500/10 text-indigo-400 font-medium border border-indigo-500/20 whitespace-nowrap">
              {{ edu.period }}
            </span>
          </div>
        </div>
      </section>

      <!-- Skills Section -->
      <section id="skills" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-6">
          <Cpu class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Keahlian & Tools</h2>
        </div>

        <!-- Skill Filter Buttons -->
        <div class="flex flex-wrap gap-2 mb-8">
          <button
            v-for="cat in categories"
            :key="cat"
            @click="skillCategory = cat"
            :class="[
              'px-4 py-1.5 rounded-lg text-xs font-medium transition',
              skillCategory === cat 
                ? 'bg-indigo-600 text-white' 
                : darkMode ? 'bg-slate-900 text-slate-400 hover:bg-slate-800' : 'bg-slate-200 text-slate-700 hover:bg-slate-300'
            ]"
          >
            {{ cat }}
          </button>
        </div>

        <!-- Skills Grid -->
        <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
          <div 
            v-for="(skill, idx) in filteredSkills" 
            :key="idx" 
            :class="[
              'p-4 rounded-xl border flex items-center gap-3 transition hover:border-indigo-500/50',
              darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-white border-slate-200'
            ]"
          >
            <div class="p-2 rounded-lg bg-indigo-500/10 text-indigo-400">
              <component :is="skill.icon" class="w-5 h-5" />
            </div>
            <div>
              <span class="font-semibold text-sm block">{{ skill.name }}</span>
              <span class="text-[10px] text-slate-400">{{ skill.category }}</span>
            </div>
          </div>
        </div>
      </section>

      <!-- Projects Section -->
      <section id="projects" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-6">
          <FolderGit2 class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Proyek Portofolio</h2>
        </div>

        <!-- Project Filter -->
        <div class="flex flex-wrap gap-2 mb-8">
          <button
            v-for="cat in ['All', 'Web App', 'Mobile']"
            :key="cat"
            @click="projectCategory = cat"
            :class="[
              'px-4 py-1.5 rounded-lg text-xs font-medium transition',
              projectCategory === cat 
                ? 'bg-indigo-600 text-white' 
                : darkMode ? 'bg-slate-900 text-slate-400 hover:bg-slate-800' : 'bg-slate-200 text-slate-700 hover:bg-slate-300'
            ]"
          >
            {{ cat }}
          </button>
        </div>

        <!-- Cards Grid -->
        <div class="grid md:grid-cols-2 gap-6">
          <div 
            v-for="project in filteredProjects" 
            :key="project.id" 
            :class="[
              'group rounded-2xl overflow-hidden border transition hover:border-indigo-500/50 flex flex-col justify-between',
              darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-white border-slate-200'
            ]"
          >
            <div>
              <div class="relative overflow-hidden aspect-video bg-slate-800">
                <img 
                  :src="project.image" 
                  :alt="project.title"
                  class="w-full h-full object-cover group-hover:scale-105 transition duration-500"
                />
                <div class="absolute top-3 right-3 bg-slate-900/80 backdrop-blur-md px-2.5 py-1 rounded-md text-[10px] font-semibold text-indigo-400 border border-slate-700">
                  {{ project.category }}
                </div>
              </div>

              <div class="p-5">
                <h3 class="text-xl font-bold mb-2 group-hover:text-indigo-400 transition">{{ project.title }}</h3>
                <p class="text-sm text-slate-400 line-clamp-2 mb-4">{{ project.description }}</p>
                
                <!-- Tags -->
                <div class="flex flex-wrap gap-1.5 mb-4">
                  <span 
                    v-for="(tag, tIdx) in project.tags" 
                    :key="tIdx" 
                    :class="['text-[11px] px-2 py-0.5 rounded-md', darkMode ? 'bg-slate-800 text-slate-300' : 'bg-slate-100 text-slate-700']"
                  >
                    {{ tag }}
                  </span>
                </div>
              </div>
            </div>

            <!-- Footer Links -->
            <div :class="['p-4 border-t flex items-center justify-between text-xs font-medium', darkMode ? 'border-slate-800/80' : 'border-slate-100']">
              <button 
                @click="selectedProject = project"
                class="text-indigo-400 hover:underline inline-flex items-center gap-1"
              >
                Detail Proyek
              </button>
              <div class="flex items-center gap-3">
                <a :href="project.githubLink" target="_blank" rel="noreferrer" class="text-slate-400 hover:text-slate-200">
                  <Github class="w-4 h-4" />
                </a>
                <a :href="project.liveLink" target="_blank" rel="noreferrer" class="text-slate-400 hover:text-indigo-400 flex items-center gap-0.5">
                  Demo <ArrowUpRight class="w-3.5 h-3.5" />
                </a>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Experience Section -->
      <section id="experience" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-8">
          <Briefcase class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Pengalaman Kerja</h2>
        </div>

        <div class="relative border-l-2 border-indigo-500/30 ml-3 space-y-8 pl-6 sm:pl-8">
          <div v-for="(exp, idx) in PORTFOLIO_DATA.experiences" :key="idx" class="relative group">
            <!-- Timeline Dot -->
            <div class="absolute -left-[31px] sm:-left-[39px] top-1.5 w-4 h-4 rounded-full bg-slate-950 border-2 border-indigo-500 group-hover:scale-125 transition" />

            <div :class="['p-5 rounded-xl border', darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-white border-slate-200']">
              <div class="flex flex-wrap items-center justify-between gap-2 mb-1">
                <h3 class="font-bold text-lg">{{ exp.role }}</h3>
                <span class="text-xs px-2.5 py-1 rounded-full bg-indigo-500/10 text-indigo-400 font-medium">
                  {{ exp.period }}
                </span>
              </div>
              <div class="text-sm text-indigo-400 font-medium mb-3">{{ exp.company }}</div>
              <p class="text-sm text-slate-400 leading-relaxed">{{ exp.description }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Hobbies Section -->
      <section id="hobbies" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-8">
          <Heart class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Hobi & Minat</h2>
        </div>

        <div class="grid sm:grid-cols-2 md:grid-cols-4 gap-4">
          <div 
            v-for="(hobby, idx) in PORTFOLIO_DATA.hobbies" 
            :key="idx"
            :class="['p-5 rounded-2xl border text-center flex flex-col items-center justify-center space-y-3', darkMode ? 'bg-slate-900/40 border-slate-800' : 'bg-white border-slate-200']"
          >
            <div class="p-3 rounded-xl bg-indigo-500/10 text-indigo-400">
              <component :is="hobby.icon" class="w-6 h-6" />
            </div>
            <h3 class="font-semibold text-sm">{{ hobby.name }}</h3>
            <p class="text-xs text-slate-400 leading-relaxed">{{ hobby.description }}</p>
          </div>
        </div>
      </section>

      <!-- Contact Section -->
      <section id="contact" class="scroll-mt-24">
        <div class="flex items-center gap-3 mb-8">
          <Mail class="w-6 h-6 text-indigo-500" />
          <h2 class="text-2xl sm:text-3xl font-bold tracking-tight">Hubungi Saya</h2>
        </div>

        <div class="grid md:grid-cols-2 gap-8">
          <!-- Left Contact Info -->
          <div class="space-y-4">
            <h3 class="text-xl font-bold">Mari Berdiskusi Proyek Selanjutnya!</h3>
            <p class="text-slate-400 text-sm leading-relaxed">
              Apakah Anda memiliki ide proyek, tawaran pekerjaan, atau sekadar ingin bertanya? Silakan kirim pesan melalui formulir atau kontak sosial media saya.
            </p>

            <div class="space-y-3 pt-4">
              <div class="flex items-center gap-3 text-sm text-slate-300">
                <Mail class="w-4 h-4 text-indigo-400" />
                <span>{{ PORTFOLIO_DATA.personal.email }}</span>
              </div>
              <div class="flex items-center gap-3 text-sm text-slate-300">
                <Globe class="w-4 h-4 text-indigo-400" />
                <span>{{ PORTFOLIO_DATA.personal.location }}</span>
              </div>
            </div>
          </div>

          <!-- Right Contact Form -->
          <form @submit.prevent="handleFormSubmit" :class="['p-6 rounded-2xl border space-y-4', darkMode ? 'bg-slate-900/50 border-slate-800' : 'bg-white border-slate-200']">
            <div v-if="formSubmitted" class="p-3 bg-emerald-500/10 border border-emerald-500/30 text-emerald-400 rounded-lg text-xs flex items-center gap-2">
              <CheckCircle2 class="w-4 h-4" /> Pesan berhasil dikirim! Saya akan membalas secepatnya.
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Nama</label>
              <input 
                type="text" 
                required
                placeholder="Nama Lengkap Anda"
                :class="[
                  'w-full px-3 py-2 rounded-lg text-sm border outline-none focus:border-indigo-500 transition',
                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-slate-50 border-slate-300 text-slate-900'
                ]"
              />
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Email</label>
              <input 
                type="email" 
                required
                placeholder="email@example.com"
                :class="[
                  'w-full px-3 py-2 rounded-lg text-sm border outline-none focus:border-indigo-500 transition',
                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-slate-50 border-slate-300 text-slate-900'
                ]"
              />
            </div>

            <div>
              <label class="block text-xs font-medium text-slate-400 mb-1">Pesan</label>
              <textarea 
                :rows="4"
                required
                placeholder="Tuliskan pesan atau detail proyek Anda..."
                :class="[
                  'w-full px-3 py-2 rounded-lg text-sm border outline-none focus:border-indigo-500 transition',
                  darkMode ? 'bg-slate-950 border-slate-800 text-slate-100' : 'bg-slate-50 border-slate-300 text-slate-900'
                ]"
              />
            </div>

            <button
              type="submit"
              class="w-full py-2.5 rounded-lg bg-indigo-600 hover:bg-indigo-500 text-white font-medium text-sm transition flex items-center justify-center gap-2 shadow-md shadow-indigo-500/20"
            >
              <Send class="w-4 h-4" /> Kirim Pesan
            </button>
          </form>
        </div>
      </section>

    </main>

    <!-- Modal Detail Proyek -->
    <div v-if="selectedProject" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm">
      <div :class="['max-w-lg w-full rounded-2xl border p-6 space-y-4', darkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200']">
        <div class="flex justify-between items-start">
          <h3 class="text-xl font-bold">{{ selectedProject.title }}</h3>
          <button @click="selectedProject = null" class="text-slate-400 hover:text-slate-200">
            <X class="w-5 h-5" />
          </button>
        </div>

        <img :src="selectedProject.image" :alt="selectedProject.title" class="w-full h-48 object-cover rounded-xl" />

        <p class="text-sm text-slate-400 leading-relaxed">{{ selectedProject.description }}</p>

        <div class="flex flex-wrap gap-1.5">
          <span v-for="(t, idx) in selectedProject.tags" :key="idx" class="text-xs px-2.5 py-1 rounded-md bg-indigo-500/10 text-indigo-400 font-medium">
            {{ t }}
          </span>
        </div>

        <div class="flex justify-end gap-3 pt-2">
          <a
            :href="selectedProject.githubLink"
            target="_blank"
            rel="noreferrer"
            class="px-4 py-2 rounded-lg border border-slate-700 text-xs font-medium hover:bg-slate-800"
          >
            GitHub Repo
          </a>
          <a
            :href="selectedProject.liveLink"
            target="_blank"
            rel="noreferrer"
            class="px-4 py-2 rounded-lg bg-indigo-600 text-white text-xs font-medium hover:bg-indigo-500"
          >
            Buka Live Demo
          </a>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer :class="['border-t py-8 mt-20 text-center text-xs text-slate-500', darkMode ? 'border-slate-800 bg-slate-950' : 'border-slate-200 bg-slate-100']">
      <p>© {{ new Date().getFullYear() }} {{ PORTFOLIO_DATA.personal.name }}. Dibuat dengan Vue 3 & Tailwind CSS.</p>
    </footer>

  </div>
</template>