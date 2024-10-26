
         <!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>36PREMİUM Giyim Mağazası</title>
    <style>
        body {
            background-color: #000080;
            color: white;
            font-family: Arial, sans-serif;
            position: relative;
        }
        h1 {
            color: white;
            text-align: center;
        }
        .ilanlar {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            margin: 20px;
        }
        .ilan {
            background-color: white;
            color: black;
            margin: 10px;
            padding: 15px;
            border-radius: 5px;
            width: 200px;
            text-align: center;
        }
        img {
            max-width: 100%;
            height: auto;
            border-radius: 5px;
        }
        .fiyat {
            font-weight: bold;
            margin-top: 10px;
        }
        .sepet-button {
            background-color: #000080;
            color: white;
            border: none;
            padding: 10px;
            margin-top: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        .sepet-simge {
            position: absolute;
            top: 20px;
            right: 20px;
            cursor: pointer;
            color: white;
            padding: 10px;
        }
        footer {
            text-align: center;
            margin-top: 20px;
        }
        #sepet {
            display: none;
            position: absolute;
            top: 50px;
            right: 20px;
            background-color: white;
            color: black;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            max-height: 300px;
            overflow-y: auto;
        }
        .notification {
            position: fixed;
            top: 10px;
            right: 20px;
            background-color: green;
            color: white;
            padding: 10px;
            border-radius: 5px;
            display: none;
            z-index: 1000;
        }
        .sepet-urun {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }
        .sepet-urun img {
            width: 50px;
            height: auto;
            margin-right: 10px;
        }
        .remove-button {
            cursor: pointer;
            color: red;
            font-weight: bold;
            margin-left: auto;
        }
        .instagram-logo {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 50px;
            height: 50px;
        }
        .email {
            position: fixed;
            bottom: 15px;
            left: 5px;
            color: black; /* Siyah yazı rengi */
            font-size: 10px; /* Küçük font boyutu */
        }
    </style>
    <script>
        let sepet = {};

        function sepetEkle(urun, resim) {
            if (sepet[urun]) {
                sepet[urun].adet++;
            } else {
                sepet[urun] = { adet: 1, resim: resim };
            }
            guncelleSepetSayisi();
            bildirimGoster(urun + " sepete eklendi!");
        }

        function guncelleSepetSayisi() {
            const sepetSayisi = Object.values(sepet).reduce((toplam, urun) => toplam + urun.adet, 0);
            document.getElementById('sepet-sayisi').innerText = sepetSayisi;
        }

        function bildirimGoster(mesaj) {
            const notification = document.getElementById('notification');
            notification.innerText = mesaj;
            notification.style.display = "block";
            setTimeout(() => {
                notification.style.display = "none";
            }, 2000);
        }

        function sepetGoruntule() {
            const sepetDiv = document.getElementById('sepet');
            sepetDiv.innerHTML = "";
            if (Object.keys(sepet).length === 0) {
                sepetDiv.innerHTML = "<p>Sepetiniz boş.</p>";
            } else {
                for (const urun in sepet) {
                    sepetDiv.innerHTML += `
                        <div class="sepet-urun">
                            <img src="${sepet[urun].resim}" alt="${urun}">
                            <p>${urun}: ${sepet[urun].adet} adet</p>
                            <span class="remove-button" onclick="sepetCikar('${urun}')">✖</span>
                        </div>`;
                }
                sepetDiv.innerHTML += '<button onclick="siparisFormuGoster()" class="sepet-button">Sipariş Et</button>';
            }
            sepetDiv.style.display = sepetDiv.style.display === "none" ? "block" : "none";
        }

        function sepetCikar(urun) {
            if (sepet[urun]) {
                delete sepet[urun];
                guncelleSepetSayisi();
                sepetGoruntule();
            }
        }

        function siparisFormuGoster() {
            const formDiv = document.getElementById('siparis-formu');
            formDiv.style.display = "block";
        }

        function siparisEt() {
            const adres = document.getElementById('adres').value;
            if (adres === "") {
                alert("Lütfen belirtilen istekleri girin!");
                return;
            }

            // Sipariş verilerini backend'e gönder
            fetch('https://your-backend-api.com/send-order', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    adres: adres,
                    siparis: sepet,
                }),
            })
            .then(response => response.json())
            .then(data => {
                alert("Siparişiniz alınmıştır!");
                sepet = {};
                guncelleSepetSayisi();
                document.getElementById('sepet').style.display = "none";
                document.getElementById('siparis-formu').style.display = "none";
            })
            .catch(error => {
                console.error('Hata:', error);
            });
        }
    </script>
</head>
<body>
    <h1>36PREMİUM Giyim Mağazası</h1>
    <div class="notification" id="notification"></div>
    
    <div class="sepet-simge" onclick="sepetGoruntule()">
        Sepet (<span id="sepet-sayisi">0</span>)
    </div>
    <div id="sepet"></div>

    <div class="ilanlar">
        <div class="ilan">
            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRgnm1MF6Z1d4FvJbcrQNvcT6K3tFQINz0iHw&s" alt="Philip Plein">
            <p class="fiyat">Philip Plein: 650 TL</p>
            <button class="sepet-button" onclick="sepetEkle('Philip Plein', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRgnm1MF6Z1d4FvJbcrQNvcT6K3tFQINz0iHw&s')">Sepete Ekle</button>
        </div>
        <div class="ilan">
            <img src="https://www.careofcarl.com/bilder/artiklar/27123311r.jpg?m=1717593400" alt="Moncler">
            <p class="fiyat">Moncler: 650 TL</p>
            <button class="sepet-button" onclick="sepetEkle('Moncler', 'https://www.careofcarl.com/bilder/artiklar/27123311r.jpg?m=1717593400')">Sepete Ekle</button>
        </div>
        <div class="ilan">
            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTZ6kG9ydQfL24BVC8QIyvG_AkE5LEIhiY9tpliqM_3VxEzwbreETitOTmRN5Lx191ltaU&usqp=CAU" alt="Burberry">
            <p class="fiyat">Burberry: 650 TL</p>
            <button class="sepet-button" onclick="sepetEkle('Burberry', 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTZ6kG9ydQfL24BVC8QIyvG_AkE5LEIhiY9tpliqM_3VxEzwbreETitOTm
