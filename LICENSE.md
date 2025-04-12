<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Certificat Carte d'Identité Otaku</title>
  <!-- Bibliothèque pour générer le QRCode -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <!-- Bibliothèque pour exporter l'élément en image -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #333;
      font-size: 1.8rem;
    }
    form {
      width: 100%;
      max-width: 600px;
      margin: auto;
      padding: 15px;
      border: 1px solid #ccc;
      background-color: #fff;
      border-radius: 5px;
    }
    form label {
      display: block;
      width: 100%;
      font-size: 1rem;
      margin-bottom: 5px;
      font-weight: bold;
    }
    form input {
      display: block;
      width: 100%;
      padding: 8px 12px;
      font-size: 1rem;
      margin-bottom: 10px;
    }
    input[type="file"] {
      padding: 5px 0;
    }
    #certificate {
      width: 100%;
      max-width: 600px;
      margin: 30px auto;
      border: 3px solid #007bff;
      background: linear-gradient(135deg, #fffacd, #add8e6);
      padding: 15px;
      position: relative;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
      border-radius: 8px;
    }
    #certificate h2 {
      text-align: center;
      margin-top: 0;
      font-size: 1.4rem;
    }
    .field {
      margin-bottom: 5px;
      font-size: 0.95rem;
    }
    #photoContainer, #charPhotoContainer {
      margin-top: 10px;
      display: inline-block;
      width: 45%;
    }
    #photoPreview, #charPhotoPreview {
      width: 100%;
      max-width: 100px;
      height: auto;
      border: 2px solid #333;
      object-fit: cover;
    }
    #qrCode {
      position: absolute;
      bottom: 20px;
      right: 20px;
      width: 80px;
      height: 80px;
    }
    #creatorSign {
      position: absolute;
      bottom: 10px;
      left: 20px;
      font-style: italic;
      font-size: 0.8rem;
    }
    .download-btn {
      display: block;
      margin: 20px auto;
      padding: 10px 20px;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-size: 1rem;
      cursor: pointer;
    }
    .download-btn:hover {
      background-color: #0056b3;
    }
    /* Élément audio masqué */
    #backgroundMusic {
      display: none;
    }
    /* Message de démarrage de la musique */
    #startMusicNotice {
      text-align: center;
      font-size: 1rem;
      color: #007bff;
      margin-bottom: 10px;
      cursor: pointer;
    }
    @media (max-width: 600px) {
      #photoContainer, #charPhotoContainer {
        width: 100%;
        text-align: center;
        margin-bottom: 10px;
      }
      #qrCode {
        position: relative;
        bottom: auto;
        right: auto;
        margin: 10px auto;
      }
    }
  </style>
</head>
<body>

<h1>Création de votre Certificat Carte d'Identité Otaku</h1>

<!-- Message pour inciter l'utilisateur à cliquer et ainsi démarrer la musique -->
<div id="startMusicNotice">Cliquez ici pour démarrer la musique</div>

<!-- Élément audio pour la musique de fond -->
<audio id="backgroundMusic" loop>
  <!-- Assurez-vous que le fichier se trouve dans le même répertoire -->
  <source src="Suspect_95_feat_Himra_-_Monalisa(256k).mp3" type="audio/mpeg">
  Votre navigateur ne supporte pas l'élément audio.
</audio>

<form id="infoForm">
  <label for="nom">Nom :</label>
  <input type="text" id="nom" name="nom" required>
  
  <label for="prenom">Prénom :</label>
  <input type="text" id="prenom" name="prenom" required>
  
  <label for="age">Âge :</label>
  <input type="number" id="age" name="age" required>
  
  <label for="pays">Pays :</label>
  <input type="text" id="pays" name="pays" required>
  
  <label for="ville">Ville :</label>
  <input type="text" id="ville" name="ville" required>
  
  <label for="quartier">Quartier :</label>
  <input type="text" id="quartier" name="quartier" required>
  
  <label for="telephone">Numéro de téléphone :</label>
  <input type="tel" id="telephone" name="telephone" required>
  
  <label for="perso">Personnage préféré :</label>
  <input type="text" id="perso" name="perso" required>
  
  <!-- Champ pour le nom de l'animé préféré -->
  <label for="anime">Nom de l'Animé préféré :</label>
  <input type="text" id="anime" name="anime" required>
  
  <!-- Upload de photo personnelle -->
  <label for="photoUpload">Ajouter votre photo :</label>
  <input type="file" id="photoUpload" name="photoUpload" accept="image/*">
  
  <!-- Upload de la photo du personnage préféré -->
  <label for="charPhotoUpload">Ajouter la photo du personnage préféré :</label>
  <input type="file" id="charPhotoUpload" name="charPhotoUpload" accept="image/*">
  
  <button type="button" id="generateBtn">Générer le Certificat</button>
</form>

<div id="certificate">
  <h2>Certificat d'Identité Otaku</h2>
  <div class="field" id="fieldNomPrenom"></div>
  <div class="field" id="fieldAge"></div>
  <div class="field" id="fieldPaysVille"></div>
  <div class="field" id="fieldQuartierTel"></div>
  <div class="field" id="fieldPerso"></div>
  <!-- Champ pour le nom de l'animé -->
  <div class="field" id="fieldAnime"></div>
  <!-- Champ pour le registre unique -->
  <div class="field" id="fieldRegistre"></div>
  
  <div id="photoContainer">
    <img id="photoPreview" src="" alt="Votre photo" style="display:none;">
  </div>
  <div id="charPhotoContainer">
    <img id="charPhotoPreview" src="" alt="Photo du personnage" style="display:none;">
  </div>
  <div id="qrCode"></div>
  <div id="creatorSign">provided by Bimpe Stonehenge Jr.</div>
</div>

<button class="download-btn" id="downloadBtn">Télécharger le Certificat</button>

<script>
  // Fonction pour générer le QR Code avec le registre unique
  function generateQRCode(content) {
    document.getElementById('qrCode').innerHTML = "";
    new QRCode(document.getElementById('qrCode'), {
      text: content,
      width: 80,
      height: 80
    });
  }

  // Génération du certificat à partir des données saisies
  document.getElementById('generateBtn').addEventListener('click', function() {
    const nom = document.getElementById('nom').value;
    const prenom = document.getElementById('prenom').value;
    const age = document.getElementById('age').value;
    const pays = document.getElementById('pays').value;
    const ville = document.getElementById('ville').value;
    const quartier = document.getElementById('quartier').value;
    const telephone = document.getElementById('telephone').value;
    const perso = document.getElementById('perso').value;
    const anime = document.getElementById('anime').value;
    
    document.getElementById('fieldNomPrenom').innerHTML = `<strong>Nom & Prénom :</strong> ${nom} ${prenom}`;
    document.getElementById('fieldAge').innerHTML = `<strong>Âge :</strong> ${age} ans`;
    document.getElementById('fieldPaysVille').innerHTML = `<strong>Localisation :</strong> ${pays}, ${ville}`;
    document.getElementById('fieldQuartierTel').innerHTML = `<strong>Quartier & Téléphone :</strong> ${quartier}, ${telephone}`;
    document.getElementById('fieldPerso').innerHTML = `<strong>Personnage préféré :</strong> ${perso}`;
    document.getElementById('fieldAnime').innerHTML = `<strong>Animé :</strong> ${anime}`;
    
    const regNumber = "REG-" + Math.floor(Math.random() * 1000000);
    document.getElementById('fieldRegistre').innerHTML = `<strong>Registre :</strong> ${regNumber}`;
    generateQRCode(regNumber);
  });

  // Upload et affichage de la photo personnelle
  document.getElementById('photoUpload').addEventListener('change', function(event) {
    const reader = new FileReader();
    reader.onload = function(){
      const photoPreview = document.getElementById('photoPreview');
      photoPreview.src = reader.result;
      photoPreview.style.display = 'block';
    };
    if (event.target.files[0]) {
      reader.readAsDataURL(event.target.files[0]);
    }
  });

  // Upload et affichage de la photo du personnage préféré
  document.getElementById('charPhotoUpload').addEventListener('change', function(event) {
    const reader = new FileReader();
    reader.onload = function(){
      const charPhotoPreview = document.getElementById('charPhotoPreview');
      charPhotoPreview.src = reader.result;
      charPhotoPreview.style.display = 'block';
    };
    if (event.target.files[0]) {
      reader.readAsDataURL(event.target.files[0]);
    }
  });

  // Téléchargement du certificat sous forme d'image
  document.getElementById('downloadBtn').addEventListener('click', function() {
    html2canvas(document.getElementById('certificate')).then(function(canvas) {
      const link = document.createElement('a');
      link.download = 'certificat_otaku.png';
      link.href = canvas.toDataURL();
      link.click();
    });
  });

  // Démarrer la musique dès la première interaction de l'utilisateur
  function startMusic() {
    const music = document.getElementById('backgroundMusic');
    music.play().then(() => {
      // Masquer l'invite une fois la musique lancée
      document.getElementById('startMusicNotice').style.display = 'none';
      // Supprimer l'écouteur pour éviter les multiples lancements
      document.removeEventListener('click', startMusic);
    }).catch(error => {
      console.log("Erreur lors du lancement de la musique :", error);
    });
  }
  document.addEventListener('click', startMusic);

  // Arrêter la musique lorsque l'utilisateur quitte la page
  window.addEventListener('beforeunload', function() {
    const music = document.getElementById('backgroundMusic');
    music.pause();
    music.currentTime = 0;
  });
</script>

</body>
</html>
