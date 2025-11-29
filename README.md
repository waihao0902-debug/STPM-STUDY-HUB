# STPM-STUDY-HUB
As a platform for STPM student to share their note and experience in study. Provide a good place for student to study together
<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>STPM Nota & Teknik Belajar</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100 font-sans">

    <header class="bg-blue-600 text-white p-4 shadow-md">
        <h1 class="text-2xl font-bold">STPM Nota & Teknik Belajar</h1>
        <p class="text-sm">Kongsi nota dan teknik belajar anda dengan rakan-rakan STPM</p>
    </header>

    <section class="p-6" id="loginSection">
        <h2 class="text-xl font-semibold mb-4">Log Masuk Pelajar</h2>
        <form id="loginForm" class="bg-white p-6 rounded shadow-md">
            <input type="text" id="namaPelajar" placeholder="Nama Pelajar" class="border p-2 w-full mb-4 rounded" required>
            <button type="submit" class="bg-blue-600 text-white px-4 py-2 rounded">Log Masuk</button>
        </form>
    </section>

    <main class="p-6 hidden" id="mainContent">
        <section class="mb-8">
            <h2 class="text-xl font-semibold mb-4">Tambah Nota Baru</h2>
            <form id="notaForm" class="bg-white p-6 rounded shadow-md">
                <input type="text" id="tajuk" placeholder="Tajuk Nota" class="border p-2 w-full mb-4 rounded" required>
                <textarea id="isiNota" placeholder="Isi Nota / Teknik Belajar" class="border p-2 w-full mb-4 rounded" rows="5" required></textarea>
                <label class="block mb-2 font-semibold">Pilih Subjek:</label>
                <select id="subjek" class="border p-2 w-full mb-4 rounded" required>
                    <option value="Bahasa Melayu">Bahasa Melayu</option>
                    <option value="Bahasa Tamil">Bahasa Tamil</option>
                    <option value="Geografi">Geografi</option>
                    <option value="Pengajian Perniagaan">Pengajian Perniagaan</option>
                    <option value="Pengajian Perakaunan">Pengajian Perakaunan</option>
                    <option value="Pengajian Am">Pengajian Am</option>
                    <option value="Biologi">Biologi</option>
                    <option value="Fizik">Fizik</option>
                    <option value="Kimia">Kimia</option>
                    <option value="Matematik Tambahan">Matematik Tambahan</option>
                    <option value="MUET">MUET</option>
                </select>
                <input type="file" id="gambarNota" accept="image/*" class="mb-4">
                <button type="submit" class="bg-blue-600 text-white px-4 py-2 rounded">Hantar</button>
            </form>
        </section>

        <section>
            <h2 class="text-xl font-semibold mb-4">Nota Pelajar</h2>
            <div class="mb-4 flex flex-col md:flex-row md:space-x-4">
                <div class="flex-1">
                    <label class="font-semibold">Pilih Subjek untuk Paparan:</label>
                    <select id="filterSubjek" class="border p-2 rounded w-full">
                        <option value="Semua">Semua</option>
                        <option value="Bahasa Melayu">Bahasa Melayu</option>
                        <option value="Bahasa Tamil">Bahasa Tamil</option>
                        <option value="Geografi">Geografi</option>
                        <option value="Pengajian Perniagaan">Pengajian Perniagaan</option>
                        <option value="Pengajian Perakaunan">Pengajian Perakaunan</option>
                        <option value="Pengajian Am">Pengajian Am</option>
                        <option value="Biologi">Biologi</option>
                        <option value="Fizik">Fizik</option>
                        <option value="Kimia">Kimia</option>
                        <option value="Matematik Tambahan">Matematik Tambahan</option>
                        <option value="MUET">MUET</option>
                    </select>
                </div>
                <div class="flex-1 mt-2 md:mt-0">
                    <label class="font-semibold">Cari Nota:</label>
                    <input type="text" id="searchNota" class="border p-2 w-full rounded" placeholder="Masukkan kata kunci...">
                </div>
            </div>
            <div id="notaList" class="space-y-4" style="max-height:none; overflow:auto;"></div>
        </section>
    </main>

    <footer class="bg-gray-800 text-white p-4 text-center">
        &copy; 2025 STPM Nota & Teknik Belajar
    </footer>

    <script>
        let pelajarAktif = localStorage.getItem('pelajarAktif') || '';
        const loginForm = document.getElementById('loginForm');
        const loginSection = document.getElementById('loginSection');
        const mainContent = document.getElementById('mainContent');
        const notaForm = document.getElementById('notaForm');
        const notaList = document.getElementById('notaList');
        const filterSubjek = document.getElementById('filterSubjek');
        const searchNota = document.getElementById('searchNota');
        let semuaNota = JSON.parse(localStorage.getItem('notaSTPM')) || [];

        function paparkanNota() {
            const subjekTerpilih = filterSubjek.value;
            const kataKunci = searchNota.value.trim().toLowerCase();
            notaList.innerHTML = '';

            semuaNota.forEach((nota, index) => {
                if((subjekTerpilih === 'Semua' || nota.subjek === subjekTerpilih) &&
                   (nota.tajuk.toLowerCase().includes(kataKunci) || nota.isiNota.toLowerCase().includes(kataKunci))) {
                    const notaDiv = buatNotaDiv(nota, index);
                    notaList.prepend(notaDiv);
                }
            });
        }

        function masukPelajar(nama) {
            pelajarAktif = nama;
            localStorage.setItem('pelajarAktif', pelajarAktif);
            loginSection.classList.add('hidden');
            mainContent.classList.remove('hidden');
            paparkanNota();
        }

        loginForm.addEventListener('submit', function(e) {
            e.preventDefault();
            const nama = document.getElementById('namaPelajar').value.trim();
            if(nama) masukPelajar(nama);
        });

        if(pelajarAktif) masukPelajar(pelajarAktif);

        notaForm.addEventListener('submit', function(e) {
            e.preventDefault();
            const tajuk = document.getElementById('tajuk').value.trim();
            const isiNota = document.getElementById('isiNota').value.trim();
            const subjek = document.getElementById('subjek').value;
            const gambarFile = document.getElementById('gambarNota').files[0];

            if(tajuk && isiNota && subjek) {
                if(gambarFile) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        const notaBaru = {tajuk, isiNota, pelajarAktif, subjek, gambar: e.target.result, komen: []};
                        semuaNota.push(notaBaru);
                        localStorage.setItem('notaSTPM', JSON.stringify(semuaNota));
                        paparkanNota();
                        notaForm.reset();
                    };
                    reader.readAsDataURL(gambarFile);
                } else {
                    const notaBaru = {tajuk, isiNota, pelajarAktif, subjek, gambar: '', komen: []};
                    semuaNota.push(notaBaru);
                    localStorage.setItem('notaSTPM', JSON.stringify(semuaNota));
                    paparkanNota();
                    notaForm.reset();
                }
            }
        });

        filterSubjek.addEventListener('change', paparkanNota);
        searchNota.addEventListener('input', paparkanNota);

        function buatNotaDiv(nota, index) {
            const notaDiv = document.createElement('div');
            notaDiv.classList.add('bg-white', 'p-4', 'rounded', 'shadow');

            let imgHTML = nota.gambar ? `<img src="${nota.gambar}" alt="Gambar Nota" class="mt-2 max-w-full rounded shadow">` : '';

            let padamBtnHTML = nota.pelajarAktif === pelajarAktif ? `<button class="bg-red-600 text-white px-3 py-1 rounded mt-2 padamBtn">Padam Nota</button>` : '';

            const komenDiv = document.createElement('div');
            komenDiv.classList.add('mt-4');
            komenDiv.innerHTML = `
                <h4 class="font-semibold">Komen:</h4>
                <div class="komenList space-y-2 mb-2"></div>
                <input type="text" class="border p-2 w-full rounded komenInput" placeholder="Tulis komen...">
                <button class="bg-gray-600 text-white px-3 py-1 rounded mt-2 komenBtn">Hantar Komen</button>
            `;

            notaDiv.innerHTML = `<h3 class="font-bold text-lg">${nota.tajuk} (${nota.subjek})</h3><p class="mt-1 text-sm text-gray-500">oleh ${nota.pelajarAktif}</p><p class="mt-2">${nota.isiNota}</p>${imgHTML}${padamBtnHTML}`;
            notaDiv.appendChild(komenDiv);
            setupKomen(notaDiv, nota);

            if(padamBtnHTML) {
                notaDiv.querySelector('.padamBtn').addEventListener('click', () => {
                    semuaNota.splice(index, 1);
                    localStorage.setItem('notaSTPM', JSON.stringify(semuaNota));
                    paparkanNota();
                });
            }

            return notaDiv;
        }

        function setupKomen(notaDiv, notaObj) {
            const komenBtn = notaDiv.querySelector('.komenBtn');
            const komenInput = notaDiv.querySelector('.komenInput');
            const komenList = notaDiv.querySelector('.komenList');

            notaObj.komen.forEach(k => {
                const p = document.createElement('p');
                p.classList.add('text-sm', 'bg-gray-100', 'p-2', 'rounded');
                p.textContent = k;
                komenList.appendChild(p);
            });

            komenBtn.addEventListener('click', function() {
                const komenText = komenInput.value.trim();
                if(komenText) {
                    const p = document.createElement('p');
                    p.classList.add('text-sm', 'bg-gray-100', 'p-2', 'rounded');
                    p.textContent = `${pelajarAktif}: ${komenText}`;
                    komenList.appendChild(p);
                    notaObj.komen.push(`${pelajarAktif}: ${komenText}`);
                    localStorage.setItem('notaSTPM', JSON.stringify(semuaNota));
                    komenInput.value = '';
                }
            });
        }
    </script>

</body>
</html>
