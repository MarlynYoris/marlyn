# marlyn
ChatBot AI 
import dotenv from "dotenv";
dotenv.config();
import readline from "readline";
import { GoogleGenerativeAI } from "@google/generative-ai";

const genAI = new GoogleGenerativeAI(process.env.API_KEY);

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
});

async function run() {
    const model = genAI.getGenerativeModel({ model: "gemini-pro" });

    const chat = model.startChat({
        history: [
            {
                role: "user",
                parts: [{text: "Kamu adalah costumer service Sekolah Menengah Kejuruan Negeri 5 Seram Bagian Timur.\nTugas kamu adalah hanya menjawab pertanyaan tentang informasi mata pelajaran dan kamu hanya menjawab dalam 1 paragraf dengan tabel mata pelajaran yang rapi dengan bahasa indonesia yang sopan.\nselalu memulai percakapan dengan menanyakan nama dan gunakan nama yang dimasukan untuk memanggil orang tersebut.\narahkan mereka untuk mengontak team@smk5bst.id jika muncul pertanyaan selain mata pelajaran dan jam pelajaran.\npilihan mata pelajaran : Pendidikan Agama\nBahasa Indonesia\n\nPPKN\nBahasa Inggris\nKejuruan\nIPAS\nSejarah\nPenjas.\nJam Pelajaran: Jam pertama 07.30-08.50\nJam kedua 09.05-10.20\nJam terakhir 10.25-12.00.\napabila dibalas ucapan terima kasih dibalas dengan bahasa yang sopan"}
                ],
            },
          ], // Start with an empty history
        generationConfig: {
            maxOutputTokens: 200,
        },
    });

    async function askAndRespond() {
        rl.question("You: ", async (msg) => {
            if (msg.toLowerCase() === "exit") {
                rl.close();
            } else {
                const result = await chat.sendMessage(msg);
                const response = await result.response;
                const text = await response.text();
                console.log("AI: ", text);
                askAndRespond();
            }
        });
    }

    askAndRespond(); // Start the conversation loop
}

run();
