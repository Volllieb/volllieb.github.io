<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <title>E-Mail eintragen</title>
</head>
<body>
  <h1>Newsletter</h1>
  <p>Trage deine E-Mail-Adresse ein:</p>

  <form name="emails"
        method="POST"
        data-netlify="true"
        netlify-honeypot="bot-field">
    
    <!-- Pflichtfeld für Netlify -->
    <input type="hidden" name="form-name" value="emails">

    <!-- Bot-Schutz -->
    <p hidden>
      <label>Don’t fill this out:
        <input name="bot-field">
      </label>
    </p>

    <input type="email"
           name="email"
           placeholder="E-Mail-Adresse"
           required>

    <button type="submit">Absenden</button>
  </form>
</body>
</html>
