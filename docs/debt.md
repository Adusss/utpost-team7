# Technical Debt

## 1. Lösenord sparas som klartext

**Var:** `api/src/routes/auth.js:12,19`
**Varför:** Lösenordet sparas med `plaintext:` istället för att hash-as. Om databasen skulle läcka kan lösenorden läsas direkt.
**Allvar:** Hög

## 2. Sökningen sätter in söktext direkt i SQL-frågan

**Var:** `api/src/routes/guides.js:23`
**Varför:** Användarens sökning läggs direkt in i SQL. Det kan göra att någon skickar in SQL-kod som en del av sökningen.
**Allvar:** Hög

## 3. Det går att ändra guider utan att vara inloggad

**Var:** `api/src/routes/guides.js:30`
**Varför:** `PUT /guides/:id` använder inte `requireUser`. Vem som helst kan därför försöka ändra en guide.
**Allvar:** Hög

## 4. Det går att ta bort turer utan inloggning

**Var:** `api/src/routes/tours.js:34`
**Varför:** `DELETE /tours/:id` använder inte `requireUser` och kontrollerar inte vem som äger turen.
**Allvar:** Hög

## 5. För många databasfrågor när turer hämtas

**Var:** `api/src/routes/tours.js:6–16`
**Varför:** För varje tur görs flera nya frågor till databasen. Det fungerar med få turer men kan bli långsamt när databasen växer.
**Allvar:** Medel

## 6. Fel i async-anrop hanteras inte på ett bra sätt

**Var:** `api/src/index.js:21–24`
**Varför:** Koden loggar bara felet med `unhandledRejection`. Klienten får inte alltid ett tydligt felmeddelande från API:et.
**Allvar:** Hög

## 7. Alla requests kan innehålla upp till 50 MB JSON

**Var:** `api/src/index.js:10`
**Varför:** En så stor gräns behövs inte för de flesta requests och kan använda mycket minne om stora requests skickas till API:et.
**Allvar:** Medel

## 8. Bildbearbetningen görs direkt när requesten körs

**Var:** `api/src/routes/photos.js:34–39`
**Varför:** Servern gör mycket beräkningar för varje bild innan requesten är klar. Flera stora bilder samtidigt kan göra API:et långsamt.
**Allvar:** Medel

## 9. Registreringen kontrollerar inte användarens input

**Var:** `api/src/routes/auth.js:16–22`
**Varför:** Email, lösenord och namn kontrolleras inte innan de sparas. Det kan leda till felaktiga värden i databasen.
**Allvar:** Medel

## 10. HTML från guider visas direkt på sidan

**Var:** `web/src/components/GuideCard.jsx:8–11`
**Varför:** `body_html` visas med `dangerouslySetInnerHTML` och vi ser ingen kontroll av HTML-innehållet innan det visas. Om innehållet inte är betrott kan det bli ett säkerhetsproblem.
**Allvar:** Hög
