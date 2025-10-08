import streamlit as st
import random
import datetime

# === Data Soal ===
SOAL = [
    ("Manakah organ pada sistem peredaran darah yang memompa darah ke seluruh tubuh?", 
     ["Paru-paru", "Jantung", "Hati", "Ginjal"], 1),
    ("Proses fotosintesis terjadi pada bagian tumbuhan yang mengandung:", 
     ["Klorofil", "Xilem", "Akar", "Sukrosa"], 0),
    ("Zat yang berubah dari zat cair menjadi gas disebut ...", 
     ["Kondensasi", "Membeku", "Mengembun", "Menguap"], 3),
    ("Satuan SI untuk massa adalah ...", 
     ["Liter", "Gram", "Kilogram", "Meter"], 2),
    ("Gaya yang bekerja saat kita mendorong meja disebut ...", 
     ["Gaya tarik", "Gaya gesek", "Gaya dorong", "Gaya sentripetal"], 2),
    ("Bagian tumbuhan yang berfungsi menyerap air dan mineral adalah ...", 
     ["Akar", "Batang", "Daun", "Bunga"], 0),
    ("Contoh perubahan kimia adalah ...", 
     ["Mencairkan lilin", "Memotong kertas", "Pembakaran kayu", "Menyublimkan kapur barus"], 2),
    ("Salah satu fungsi tulang adalah ...", 
     ["Menghasilkan getah bening", "Memberi bentuk tubuh dan melindungi organ", "Mengatur suhu tubuh", "Mencerna makanan"], 1),
    ("Alat yang digunakan untuk mengukur massa adalah ...", 
     ["Termometer", "Neraca", "Stopwatch", "Mikroskop"], 1),
    ("Siklus air dimulai dari ...", 
     ["Presipitasi (hujan)", "Evaporasi (penguapan)", "Infiltrasi", "Transpirasi"], 1),
]

POINTS_PER_QUESTION = 4

# === Inisialisasi session state ===
if "index" not in st.session_state:
    st.session_state.index = 0
    st.session_state.score = 0
    st.session_state.correct = 0
    st.session_state.wrong = 0
    st.session_state.answers = []
    random.shuffle(SOAL)

st.title("🔬 Kuis IPA SMP")
st.caption("Jawab 10 pertanyaan acak. Setiap jawaban benar bernilai 4 poin.")

nama = st.text_input("Masukkan nama Anda:", value=st.session_state.get("nama", ""))
st.session_state.nama = nama

if not nama:
    st.warning("Silakan isi nama dulu untuk memulai kuis.")
    st.stop()

# === Ambil soal aktif ===
if st.session_state.index < len(SOAL):
    soal, opsi, idx_benar = SOAL[st.session_state.index]
    opsi_acak = list(enumerate(opsi))
    random.shuffle(opsi_acak)
    jawaban_benar_baru = [i for i, (asli, _) in enumerate(opsi_acak) if asli == idx_benar][0]

    st.markdown(f"### Soal {st.session_state.index + 1}")
    st.write(soal)
    pilihan = st.radio("Pilih jawaban:", [teks for _, teks in opsi_acak], key=f"q{st.session_state.index}")

    if st.button("Kirim Jawaban"):
        idx_user = [t for _, t in opsi_acak].index(pilihan)
        if idx_user == jawaban_benar_baru:
            st.success("✅ Benar! +4 poin")
            st.session_state.score += POINTS_PER_QUESTION
            st.session_state.correct += 1
            hasil = "Benar"
        else:
            st.error(f"❌ Salah! Jawaban benar: **{opsi_acak[jawaban_benar_baru][1]}**")
            st.session_state.wrong += 1
            hasil = "Salah"

        st.session_state.answers.append({
            "soal": soal,
            "jawaban_user": pilihan,
            "hasil": hasil
        })
        st.session_state.index += 1
        st.experimental_rerun()

# === Hasil akhir ===
else:
    total = len(SOAL) * POINTS_PER_QUESTION
    persen = st.session_state.score / total * 100

    st.header("🎯 HASIL AKHIR")
    st.write(f"**Nama:** {st.session_state.nama}")
    st.write(f"**Skor:** {st.session_state.score} / {total}")
    st.write(f"**Benar:** {st.session_state.correct}")
    st.write(f"**Salah:** {st.session_state.wrong}")
    st.write(f"**Persentase:** {persen:.1f}%")

    waktu = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    if st.button("Simpan Hasil ke File"):
        with open("hasil_kuis.txt", "a", encoding="utf-8") as f:
            f.write(f"Waktu: {waktu}\n")
            f.write(f"Peserta: {st.session_state.nama}\n")
            f.write(f"Skor: {st.session_state.score}/{total}\n")
            f.write(f"Benar: {st.session_state.correct}, Salah: {st.session_state.wrong}\n")
            f.write("-"*40 + "\n")
        st.success("✅ Hasil disimpan di file *hasil_kuis.txt*")

    if st.button("Main Lagi 🔁"):
        for key in list(st.session_state.keys()):
            del st.session_state[key]
        st.experimental_rerun()
