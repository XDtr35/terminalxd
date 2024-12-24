<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Terminal Uygulaması</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #1e1e1e;
            color: #fff;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .terminal {
            width: 80%;
            max-width: 800px;
            background-color: #333;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            text-align: center;
        }

        #output {
            height: 300px;
            overflow-y: auto;
            background-color: #111;
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 5px;
        }

        #input {
            width: 100%;
            padding: 10px;
            font-size: 18px;
            color: #fff;
            background-color: #444;
            border: none;
            border-radius: 5px;
        }

        .developer-menu {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #222;
            padding: 20px;
            border-radius: 10px;
            color: #fff;
            text-align: center;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.7);
        }

        .developer-menu button {
            background-color: #444;
            color: #fff;
            padding: 10px;
            margin: 10px;
            border: none;
            cursor: pointer;
            font-size: 16px;
            border-radius: 5px;
        }

        .developer-menu button:hover {
            background-color: #666;
        }
    </style>
</head>
<body>

    <div class="terminal">
        <div class="output" id="output"></div>
        <input type="text" id="input" placeholder="Komut girin..." autofocus>
    </div>

    <div id="developer-menu" class="developer-menu">
        <h3>Geliştirici Menüsü</h3>
        <button onclick="addCommand()">Yeni Komut Ekle</button>
        <button onclick="viewCommands()">Mevcut Komutları Gör</button>
        <button onclick="exitDeveloperMenu()">Çıkış</button>
    </div>

    <script>
        // Kullanıcıya yazılacak özel komutlar
        let customCommands = {};

        // Şifre
        const developerPassword = "1234";

        document.getElementById('input').addEventListener('keydown', function(event) {
            if (event.key === 'Enter') {
                let command = event.target.value.trim().toLowerCase();
                processCommand(command);
                event.target.value = ''; // Input'u temizle
            }
        });

        function processCommand(command) {
            const output = document.getElementById('output');

            // Çıkış komutu
            if (command === "exit") {
                output.innerHTML += "<div>Terminal kapatılıyor...</div>";
            }
            // Help komutu
            else if (command === "help") {
                output.innerHTML += "<div>Kullanılabilir Komutlar:</div>";
                output.innerHTML += "<div>  - help: Bu menüyü gösterir.</div>";
                output.innerHTML += "<div>  - clear: Terminali temizler.</div>";
                output.innerHTML += "<div>  - exit: Terminalden çıkmanıza olanak sağlar.</div>";
                output.innerHTML += "<div>  - geliştirici: Geliştirici menüsünü açar.</div>";
            }
            // Clear komutu
            else if (command === "clear") {
                output.innerHTML = "";
            }
            // Geliştirici komutu
            else if (command === "geliştirici") {
                authenticate();
            }
            // Özel komutları kontrol et
            else if (customCommands[command]) {
                output.innerHTML += `<div>${customCommands[command]}</div>`;
            }
            // Geçersiz komut
            else {
                output.innerHTML += `<div>Komut bulunamadı: ${command}</div>`;
            }

            // Çıktıyı kaydet
            output.scrollTop = output.scrollHeight;
        }

        function authenticate() {
            let password = prompt("Geliştirici menüsüne erişim için şifreyi girin:");
            if (password === developerPassword) {
                output.innerHTML += "<div>Şifre doğru! Geliştirici menüsüne erişim sağlanıyor...</div>";
                showDeveloperMenu();
            } else {
                output.innerHTML += "<div>Yanlış şifre! Geliştirici menüsüne erişim reddedildi.</div>";
            }
        }

        function showDeveloperMenu() {
            document.getElementById('developer-menu').style.display = 'block';
        }

        function exitDeveloperMenu() {
            document.getElementById('developer-menu').style.display = 'none';
        }

        function addCommand() {
            let commandName = prompt("Yeni komut ismini girin:");
            let commandResponse = prompt(`${commandName} komutunun çıktısı ne olsun?`);
            customCommands[commandName.toLowerCase()] = commandResponse;
            alert(`'${commandName}' komutu başarıyla eklendi!`);
        }

        function viewCommands() {
            let commandsList = "Mevcut Komutlar:\n";
            for (let cmd in customCommands) {
                commandsList += `${cmd}: ${customCommands[cmd]}\n`;
            }
            alert(commandsList);
        }
    </script>

</body>
</html>
