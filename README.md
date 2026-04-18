<img src="https://drive.google.com/uc?export=view&id=1N45Iu5HqLyxPTX518uwMcvUVoOwdjoEf" 
     alt="Halaman Thumbnail" 
     width="400">

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

   ![Halaman Hero](https://drive.google.com/uc?export=view&id=1BlyD4DqK4tT5WYpxFJcgli6bMYtCA_2l)

---

2. Visi Kami (Our Vision Section)
   Menjelaskan tujuan dibalik pembuatan aplikasi ini dalam membantu kehidupan sehari-hari melalui AI.

   ![Halaman Our Vision](https://drive.google.com/uc?export=view&id=1MGXHQscQQoIBc2lfPvfia-zwYu58KclV)

---

3. Kategori Layanan Analisis
   Aplikasi ini memiliki tiga fitur utama yang dapat dipilih pengguna:
   📄 Analisis CV, 🌱 Hama Tanaman dan 🗑️ Klasifikasi Sampah

   ![Halaman Kategori](https://drive.google.com/uc?export=view&id=1k7OfqVMzRMiZrogK4egvc84kT9K6FqbG)

---

4. Halaman Analisis CV (AI-Based CV Checker)
   Tampilan Halaman input :
   <table style="width: 100%;">
     <tr>
       <td style="width: 50%; text-align: center;">
         <img src="https://drive.google.com/uc?export=view&id=13e8fMu8-rY6BMhMpSn5CGZi9xCquO7_X" style="border: 2px solid #ddd;">
         <p><i>Halaman AI-Based CV Checker - input</i></p>
       </td>
       <td style="width: 50%; text-align: center;">
         <img src="https://drive.google.com/uc?export=view&id=1nRShZCp0jfklJuryEIHhcp_xlvrKFStp" style="border: 2px solid #ddd;">
         <p><i>Halaman AI-Based CV Checker - output</i></p>
       </td>
     </tr>
   </table>

---

5. Halaman Analisis Hama Tanaman (Plant Disease Detection)
   Tampilan Halaman input :
   ![Halaman Plant Disease Detection - input](https://drive.google.com/uc?export=view&id=1ErznnHwod1oxFdmoqrSc6eTeR14eKN11)

   Tampilan Halaman output :
   ![Halaman Plant Disease Detection - output](https://drive.google.com/uc?export=view&id=115em_dVD9itSo-NEv86K7IO5eBglLQhm)

---

6. Halaman Klasifikasi Sampah (Waste Classification System)
   Tampilan Halaman input :
   ![Halaman Waste Classification System - input](https://drive.google.com/uc?export=view&id=13RqDYPta_3BLf-4AOllb0YyyB4EaXKmb)

   Tampilan Halaman output :
   ![Halaman Waste Classification System - output](https://drive.google.com/uc?export=view&id=1D2iD5edZSz0HWHR6mFaP8lOq-iG2Tehm)

---

### ⚙️ Cara Kerja Aplikasi (Teknis)

Aplikasi ini menggunakan alur pemrosesan data yang terintegrasi langsung dengan model bahasa besar (LLM). Berikut adalah detail teknisnya:

🧠 Model & AI Engine

1. gemini-3-flash: Menggunakan API Key dari Google AI Studio untuk memproses multimodal (teks dan gambar).

```
  const response = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${process.env.GEMINI_API_KEY_CV_ANALYZER}`;
    ....
```

2. Backend Processing: Gambar tidak diproses di sisi klien, melainkan dikirim ke endpoint API backend untuk menjaga keamanan API Key.

```
try {
    const res = await fetch("/api/analyze-cv", {
    method: "POST",
    body: formData,
    });
    ...
```

🛠️ Teknik Prompt Engineering
Aplikasi menggunakan metode Hardcoded Prompt Injection pada sisi server. Cara kerjanya:

1. User memilih gambar (misal: Gambar Daun).
2. Frontend mengirimkan gambar tersebut ke Backend API.
3. Di sisi Backend, sistem menyematkan instruksi spesifik (hardcoded prompt) sebelum dikirim ke AI, contoh kodenya :

```
 const PROMPT_EN = `
  You are a professional CV analyst AI.

  Task:
  Analyze the given CV and return the result in a VALID JSON format.

  IMPORTANT RULES:
  - The response MUST be pure JSON
  - DO NOT add any explanations
  - DO NOT use markdown
  - DO NOT add comments
  - DO NOT wrap the JSON with backticks
  ....
  `;

  const PROMPT_ID = `
Kamu adalah AI analis CV profesional.

Tugas:
Analisa CV yang diberikan dan kembalikan hasil dalam format JSON yang VALID.

ATURAN PENTING:
- Jawaban HARUS berupa JSON murni
- JANGAN menambahkan teks penjelasan apapun
- JANGAN menggunakan markdown
- JANGAN menambahkan komentar
- JANGAN membungkus JSON dengan \`\`\`

Gunakan struktur JSON berikut:

{
  "score": number,
  "score_reason": string,
  "skills": string[],
  ....
`;

  const prompt = language === "en" ? PROMPT_EN : PROMPT_ID;

  const response = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${process.env.GEMINI_API_KEY_CV_ANALYZER}`,
    {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        contents: [
          {
            parts: [
              { text: prompt },
              {
                inline_data: {
                  mime_type: file.type,
                  data: base64File,
                },
              },
            ],
          },
        ],
      }),
    }
  );
```

AI menerima kombinasi gambar + prompt tersembunyi tersebut, sehingga hasil yang dikembalikan selalu konsisten dan sesuai dengan kategori yang dipilih.

4. Respone yang diberikan oleh model AI akan berupa data json seperti ini yang kemudian di parsing di sisi frontend :

````markdown
```js
{
  "score": 75,
  "score_reason": "CV memiliki struktur yang rapi dan pengalaman spesifik di industri hospitality (Ayana Komodo). Namun, detail teknis untuk bidang IT masih bisa diperdalam lagi.",
  "skills": [
    "Troubleshooting Jaringan",
    "Microsoft Office",
    "Manajemen Infrastruktur IT Hotel"
  ],
  "recommendations": [
    "IT Support (Hospitality)",
    "Network Technician"
  ],
  "main_improvement": [
    "Tambahkan rincian proyek spesifik atau software tertentu yang dikelola selama masa PKL di Ayana",
    "Sertakan sertifikasi kompetensi (jika ada) untuk memperkuat profil lulusan TKJ",
    "Gunakan layout yang lebih modern agar visual lebih menonjol di mata recruiter",
    "Cantumkan hobi atau minat yang relevan dengan perkembangan teknologi terbaru"
  ],
  "job_keywords": [
    "IT Support",
    "Technical Support",
    "Network Administrator",
    "Helpdesk IT"
  ]
}
```
````

---

## 🛠 Tech Stack

| Technology       | Purpose                      |
| ---------------- | ---------------------------- |
| **Next.js**      | React Framework (App Router) |
| **React**        | Frontend UI                  |
| **TypeScript**   | Type safety                  |
| **Tailwind CSS** | Styling                      |
| **Vercel**       | Deployment                   |
| **Gemini API**   | Image analysis engine        |

---

## ⚙️ Installation & Setup

Clone the repository:

```
git clone https://github.com/christianxavierVibecode/SustainAi_Analyzer_App.git
cd SustainAi_Analyzer_App
```

Instal dependensi:

```
npm install
```

Jalankan Development server (pastikan di komputer anda sudah terinstall Node.js) :

```
npm run dev
```

---

## 🔐 Environment Variables

Buat file .env.local di direktori root (SustainAi_Analyzer_App), dengan struktur:

```
GEMINI_API_KEY_CV_ANALYZER=YOUR_API_KEY
GEMINI_API_KEY_WASTE_ANALYZER=YOUR_API_KEY
GEMINI_API_KEY_PLANT_ANALYZER=YOUR_API_KEY
```

> [!WARNING]
> Jangan pernah push file `.env` ke repository. File ini mengandung API key dan kredensial sensitif yang bisa disalahgunakan.

---

## 🤝 Support Me

Kontribusi, saran, dan perbaikan sangat kami harapkan.

1. Fork repositori ini
2. Buat branch fitur Anda
3. Commit perubahan Anda
4. lalu buat Pull Request

---

## 📜 License

This project is open-source and available under the MIT License.

---

## 👨‍💻 Author

Built with passion for modern web technology and AI innovation.

```

```
