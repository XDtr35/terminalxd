<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Terminal - HTML Versiyon</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: monospace;
            background-color: #1e1e1e;
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .terminal {
            width: 80%;
            max-width: 800px;
            background-color: #000;
            border-radius: 5px;
            overflow: hidden;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
        }

        .terminal-header {
            background-color: #333;
            padding: 10px;
            color: #fff;
            font-weight: bold;
        }

        .terminal-output {
            height: 300px;
            background-color: #111;
            padding: 10px;
            overflow-y: auto;
            white-space: pre-wrap;
            border-bottom: 2px solid #444;
        }

        .terminal-input {
            display: flex;
        }

        .terminal-input input {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            color: #fff;
            background-color: #222;
            border: none;
            outline: none;
            border-radius: 0 0 0 5px;
        }

        .terminal-input button {
            padding: 10px;
            font-size: 16px;
            color: #fff;
            background-color: #333;
            border: none;
            cursor: pointer;
            border-radius: 0 0 5px 0;
        }

        .terminal-input button:hover {
            background-color: #444;
        }

        .button-back {
            background-color: #555;
            padding: 5px;
            border-radius: 5px;
            cursor: pointer;
        }

        .button-back:hover {
            background-color: #666;
        }

        .command-list {
            background-color: #222;
            padding: 10px;
            max-height: 300px;
            overflow-y: auto;
        }

        .command-item {
            margin: 5px 0;
            cursor: pointer;
        }

        .command-item:hover {
            color: #00ff00;
        }
    </style>
</head>
<body>
    <div class="terminal">
        <div class="terminal-header">Terminal - HTML</div>
        <div id="output" class="terminal-output"></div>
        <div class="terminal-input">
            <input type="text" id="input" placeholder="Komut girin..." autofocus>
            <button onclick="processInput()">Gönder</button>
        </div>
    </div>

    <script>
        let customCommands = JSON.parse(localStorage.getItem('customCommands')) || {};
        const developerPassword = "1234";

        document.addEventListener("DOMContentLoaded", () => {
            autoRunHelp();
        });

        function autoRunHelp() {
            const output = document.getElementById('output');
            output.innerHTML += `<div>> help</div>`;
            output.innerHTML += "<div>Kullanılabilir Komutlar:</div>";
            output.innerHTML += "<div>  - help: Bu menüyü gösterir.</div>";
            output.innerHTML += "<div>  - clear: Terminali temizler.</div>";
            output.innerHTML += "<div>  - geliştirici: Geliştirici menüsünü açar.</div>";
        }

        function processInput() {
            const inputField = document.getElementById('input');
            const command = inputField.value.trim().toLowerCase();
            const output = document.getElementById('output');

            if (command === "") return;

            output.innerHTML += `<div>> ${command}</div>`;

            if (command === "help") {
                output.innerHTML += "<div>Kullanılabilir Komutlar:</div>";
                output.innerHTML += "<div>  - help: Bu menüyü gösterir.</div>";
                output.innerHTML += "<div>  - clear: Terminali temizler.</div>";
                output.innerHTML += "<div>  - geliştirici: Geliştirici menüsünü açar.</div>";
            } else if (command === "clear") {
                output.innerHTML = "";
            } else if (command === "geliştirici") {
                authenticate();
            } else if (customCommands[command]) {
                output.innerHTML += `<div>${customCommands[command]}</div>`;
            } else if (command === "ls") {
                output.innerHTML += `<div>Dosyalar: /home, /root, /user</div>`;
            } else if (command === "pwd") {
                output.innerHTML += `<div>/home/user</div>`;
            } else if (command === "cd") {
                output.innerHTML += `<div>Geçerli dizin değiştirilemez.</div>`;
            } else if (command === "termux") {
                output.innerHTML += `<div>Termux: Paket yöneticisi ile uygulamaları yükleyebilirsiniz.</div>`;
            } else {
                output.innerHTML += `<div>Komut bulunamadı: ${command}</div>`;
            }

            output.scrollTop = output.scrollHeight;
            inputField.value = "";
        }

        function authenticate() {
            const password = prompt("Geliştirici menüsüne erişim için şifreyi girin:");
            if (password === developerPassword) {
                showDeveloperMenu();
            } else {
                alert("Yanlış şifre!");
            }
        }

        function showDeveloperMenu() {
            const action = prompt(
                "Geliştirici menüsü:\n" +
                "1. Yeni komut ekle\n" +
                "2. Komut sil\n" +
                "3. Mevcut komutları göster\n" +
                "4. Geri\n" +
                "Seçiminizi yapın (1, 2, 3 veya 4):"
            );

            if (action === "1") {
                const commandName = prompt("Yeni komut ismini girin:");
                const commandResponse = prompt(`${commandName} komutunun çıktısı ne olsun?`);

                if (commandName && commandResponse) {
                    customCommands[commandName.toLowerCase()] = commandResponse;
                    localStorage.setItem('customCommands', JSON.stringify(customCommands));
                    alert(`'${commandName}' komutu başarıyla eklendi!`);
                } else {
                    alert("Komut ekleme işlemi iptal edildi.");
                }
            } else if (action === "2") {
                const commandToDelete = prompt("Silmek istediğiniz komutun adını girin:");

                if (customCommands[commandToDelete.toLowerCase()]) {
                    delete customCommands[commandToDelete.toLowerCase()];
                    localStorage.setItem('customCommands', JSON.stringify(customCommands));
                    alert(`'${commandToDelete}' komutu başarıyla silindi!`);
                } else {
                    alert("Silmek istediğiniz komut bulunamadı.");
                }
            } else if (action === "3") {
                showCommandsList();
            } else if (action === "4") {
                alert("Geri gidiliyor.");
                return;
            } else {
                alert("Geçersiz seçim. Geliştirici menüsünden çıkılıyor.");
            }
        }

        function showCommandsList() {
            const output = document.getElementById('output');
            let commandsList = "Mevcut Komutlar:\n";
            for (let command in customCommands) {
                commandsList += `- ${command}\n`;
            }

            output.innerHTML += `<div>${commandsList || "Hiç komut eklenmemiş."}</div>`;
            output.scrollTop = output.scrollHeight;
        }
    </script>
</body>
</html>