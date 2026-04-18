# 🌿 Analyzer App – AI Image Analyzer

_Platform Analisis Gambar Cerdas berbasis Teknologi Web Modern_

[![Deployed on Vercel](https://img.shields.io/badge/Deployed%20on-Vercel-black?style=for-the-badge&logo=vercel)](https://analazer-app.vercel.app/)
[![Built with Next.js](https://img.shields.io/badge/Built%20with-Next.js-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![Powered by AI](https://img.shields.io/badge/Powered%20by-AI-blue?style=for-the-badge)](#)

---

## 🚀 Live Demo

👉 **https://analazer-app.vercel.app/**

---

## 📖 Deskripsi Singkat

**Analyzer App** adalah aplikasi web berbasis AI modern yang memungkinkan pengguna untuk mengunggah dan menganalisis berbagai jenis gambar secara cerdas.

Dibangun dengan fokus pada performa dan pengalaman pengguna, platform ini menyediakan:

- 📤 Unggah & pratinjau gambar atau pdf secara langsung.
- 🤖 Analisis gambar atau pdf berbasis AI yang mendalam.
- 🌐 Dukungan bahasa Indonesia dan Inggris.
- ⚡ Antarmuka yang modern dan responsif.
- 🔒 Interaksi klien-server yang aman.

---

## 🖼️ Pratinjau Tampilan Aplikasi

Berikut adalah gambaran tampilan dari Analyzer App:

1. Bagian Utama (Hero Section)
   Menampilkan visual utama dan ajakan bertindak (call to action) bagi pengguna untuk memulai analisis.
   ![Halaman Artikel 1](https://drive.google.com/uc?export=view&id=1iSJq4nIkkZi2oUic68F34kOmagoRWI0i)

2. Visi Kami (Our Vision Section)
   Menjelaskan tujuan dibalik pembuatan aplikasi ini dalam membantu kehidupan sehari-hari melalui AI.
   ![Halaman Artikel 1](https://drive.google.com/uc?export=view&id=1iSJq4nIkkZi2oUic68F34kOmagoRWI0i)

3. Kategori Layanan Analisis
   Aplikasi ini memiliki tiga fitur utama yang dapat dipilih pengguna:
   📄 Analisis CV, 🌱 Hama Tanaman dan 🗑️ Klasifikasi Sampah
   ![Halaman Artikel 1](https://drive.google.com/uc?export=view&id=1iSJq4nIkkZi2oUic68F34kOmagoRWI0i)

4. Halaman Analisis CV (AI-Based CV Checker)
   Sampel hasil ekstraksi data dari file CV atau resume menggunakan AI.
   ![Halaman Artikel 1](https://drive.google.com/uc?export=view&id=1iSJq4nIkkZi2oUic68F34kOmagoRWI0i)

5. Halaman Analisis Hama Tanaman (Plant Disease Detection)
   Sampel deteksi penyakit atau hama pada daun tanaman beserta solusinya.
   ![Halaman Artikel 1](https://drive.google.com/uc?export=view&id=1iSJq4nIkkZi2oUic68F34kOmagoRWI0i)

6. Halaman Klasifikasi Sampah (Waste Classification System)
   Sampel identifikasi jenis sampah (organik/anorganik) dan cara pengolahannya.
   ![Halaman Artikel 1](https://drive.google.com/uc?export=view&id=1iSJq4nIkkZi2oUic68F34kOmagoRWI0i)

---

### ⚙️ Cara Kerja Aplikasi (Teknis)

Aplikasi ini menggunakan alur pemrosesan data yang terintegrasi langsung dengan model bahasa besar (LLM). Berikut adalah detail teknisnya:

🧠 Model & AI Engine
1. gemini-3-flash: Menggunakan API Key dari Google AI Studio untuk memproses multimodal (teks dan gambar).
```Javascript
  const response = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${process.env.GEMINI_API_KEY_CV_ANALYZER}`;
    ....
```

2. Backend Processing: Gambar tidak diproses di sisi klien, melainkan dikirim ke endpoint API backend untuk menjaga keamanan API Key.

🛠️ Teknik Prompt Engineering
Aplikasi menggunakan metode Hardcoded Prompt Injection pada sisi server. Cara kerjanya:

User memilih gambar (misal: Gambar Daun).

Frontend mengirimkan gambar tersebut ke Backend API.

Di sisi Backend, sistem menyematkan instruksi spesifik (hardcoded prompt) sebelum dikirim ke AI.

Contoh Prompt: "Anda adalah pakar agronomi. Analisis gambar tanaman ini, identifikasi hamanya, dan berikan solusi dalam format JSON."

AI menerima kombinasi gambar + prompt tersembunyi tersebut, sehingga hasil yang dikembalikan selalu konsisten dan sesuai dengan kategori yang dipilih.

---

## 🛠 Tech Stack

| Technology       | Purpose                      |
| ---------------- | ---------------------------- |
| **Next.js**      | React Framework (App Router) |
| **React**        | Frontend UI                  |
| **TypeScript**   | Type safety                  |
| **Tailwind CSS** | Styling                      |
| **Vercel**       | Deployment                   |
| **AI API**       | Image analysis engine        |

---

## ⚙️ Installation & Setup

Clone the repository:

```bash
git clone https://github.com/your-username/analyzer-app.git
cd analyzer-app
```

Install dependencies:

```bash
npm install
```

Run development server:

```bash
npm run dev
```

Build for production:

```bash
npm run build
npm start
```

---

## 🌐 Deployment

This project is deployed using **Vercel**.

To deploy your own version:

1. Push your project to GitHub
2. Import the repository into Vercel
3. Configure environment variables (if required)
4. Deploy 🚀

---

## 🔐 Environment Variables

If using an AI API provider, create a `.env.local` file:

```
AI_API_KEY=your_api_key_here
```

Never expose your API key to the client side.

---

## 📌 How It Works

1. User uploads an image
2. The image is previewed on the client
3. The image is sent to the backend/API
4. AI processes the image
5. The analysis result is returned and displayed

---

## 🎯 Goals of This Project

- Build a real-world AI integrated web app
- Implement clean architecture with scalability
- Practice modern frontend engineering patterns
- Provide multilingual user experience

---

## 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

1. Fork this repository
2. Create your feature branch
3. Commit your changes
4. Open a Pull Request

---

## 📜 License

This project is open-source and available under the MIT License.

---

## 👨‍💻 Author

Built with passion for modern web technology and AI innovation.
