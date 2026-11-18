[Statistiche_pallavolo_3.html](https://github.com/user-attachments/files/34985976/Statistiche_pallavolo_1.html)
Di seguito trovi il codice HTML completo (unico file) con:

Inserimento dati partita
6 contatori errori
Incremento e decremento
Totale automatico
Salvataggio automatico (localStorage)
Ripristino automatico alla riapertura
Generazione report TXT
Ottimizzato per iPhone, iPad, Android e PC

Salva tutto in un file chiamato ad esempio errori_pallavolo.html.

<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Conteggio Errori Pallavolo</title>

<style>
body{
    font-family:Arial, sans-serif;
    background:#f4f4f4;
    margin:15px;
    text-align:center;
}

h1{
    color:#333;
}

/* DATI PARTITA */

.info-partita{
    max-width:1000px;
    margin:20px auto;
    background:white;
    padding:20px;
    border-radius:12px;
    box-shadow:0 2px 6px rgba(0,0,0,0.15);

    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
    gap:15px;
}

.campo{
    display:flex;
    flex-direction:column;
    text-align:left;
}

.campo label{
    font-weight:bold;
    margin-bottom:5px;
}

.campo input{
    padding:10px;
    border:1px solid #ccc;
    border-radius:6px;
    font-size:16px;
}

/* CONTATORI */

.container{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
    gap:15px;
    max-width:1000px;
    margin:auto;
}

.counter{
    background:white;
    padding:20px;
    border-radius:12px;
    box-shadow:0 2px 6px rgba(0,0,0,0.15);
}

.counter h2{
    margin-top:0;
}

.value{
    font-size:56px;
    font-weight:bold;
    color:#d32f2f;
    margin:20px 0;
}

.controls{
    display:flex;
    justify-content:center;
    gap:10px;
}

.plus,
.minus{
    border:none;
    border-radius:8px;
    color:white;
    font-size:26px;
    font-weight:bold;
    cursor:pointer;
    min-width:90px;
    min-height:60px;
}

.plus{
    background:#2e7d32;
}

.plus:hover{
    background:#1b5e20;
}

.minus{
    background:#c62828;
}

.minus:hover{
    background:#8e0000;
}

#totale{
    margin-top:30px;
    font-size:32px;
    font-weight:bold;
}

.buttons{
    margin-top:25px;
}

.reset,
.report{
    border:none;
    border-radius:8px;
    color:white;
    padding:14px 24px;
    font-size:18px;
    cursor:pointer;
    margin:5px;
}

.reset{
    background:#f57c00;
}

.reset:hover{
    background:#e65100;
}

.report{
    background:#1976d2;
}

.report:hover{
    background:#0d47a1;
}

@media(max-width:600px){

    .plus,
    .minus{
        min-width:100px;
        min-height:75px;
        font-size:30px;
    }

    .value{
        font-size:64px;
    }
}
</style>

</head>
<body>

<h1>Conteggio Errori Pallavolo</h1>

<div class="info-partita">

    <div class="campo">
        <label>Data Partita</label>
        <input type="date" id="dataPartita">
    </div>

    <div class="campo">
        <label>Categoria</label>
        <input type="text" id="categoria" placeholder="Es. U16 Femminile">
    </div>

    <div class="campo">
        <label>Luogo</label>
        <input type="text" id="luogo" placeholder="Palazzetto">
    </div>

    <div class="campo">
        <label>Squadra Casa</label>
        <input type="text" id="squadraCasa" placeholder="Nome squadra">
    </div>

    <div class="campo">
        <label>Squadra Ospite</label>
        <input type="text" id="squadraOspite" placeholder="Nome squadra">
    </div>

</div>

<div class="container">

    <div class="counter">
        <h2>Battuta</h2>
        <div class="value" id="battuta">0</div>
        <div class="controls">
            <button class="plus" onclick="modifica('battuta',1)">+</button>
            <button class="minus" onclick="modifica('battuta',-1)">−</button>
        </div>
    </div>

    <div class="counter">
        <h2>Difesa</h2>
        <div class="value" id="difesa">0</div>
        <div class="controls">
            <button class="plus" onclick="modifica('difesa',1)">+</button>
            <button class="minus" onclick="modifica('difesa',-1)">−</button>
        </div>
    </div>

    <div class="counter">
        <h2>Palleggio</h2>
        <div class="value" id="palleggio">0</div>
        <div class="controls">
            <button class="plus" onclick="modifica('palleggio',1)">+</button>
            <button class="minus" onclick="modifica('palleggio',-1)">−</button>
        </div>
    </div>

    <div class="counter">
        <h2>Attacco</h2>
        <div class="value" id="attacco">0</div>
        <div class="controls">
            <button class="plus" onclick="modifica('attacco',1)">+</button>
            <button class="minus" onclick="modifica('attacco',-1)">−</button>
        </div>
    </div>

    <div class="counter">
        <h2>Ricezione</h2>
        <div class="value" id="ricezione">0</div>
        <div class="controls">
            <button class="plus" onclick="modifica('ricezione',1)">+</button>
            <button class="minus" onclick="modifica('ricezione',-1)">−</button>
        </div>
    </div>

    <div class="counter">
        <h2>Muro</h2>
        <div class="value" id="muro">0</div>
        <div class="controls">
            <button class="plus" onclick="modifica('muro',1)">+</button>
            <button class="minus" onclick="modifica('muro',-1)">−</button>
        </div>
    </div>

</div>

<div id="totale">
    Totale Errori: 0
</div>

<div class="buttons">
    <button class="reset" onclick="resetContatori()">
        Azzera Tutto
    </button>

    <button class="report" onclick="generaReport()">
        Genera Report TXT
    </button>
</div>

<script>

const contatori = [
    "battuta",
    "difesa",
    "palleggio",
    "attacco",
    "ricezione",
    "muro"
];

/* CARICAMENTO DATI */

function caricaDati()
{
    let dati = localStorage.getItem("erroriPallavolo");

    if(!dati) return;

    dati = JSON.parse(dati);

    contatori.forEach(id => {
        document.getElementById(id).textContent =
            dati[id] || 0;
    });

    document.getElementById("dataPartita").value =
        dati.dataPartita || "";

    document.getElementById("categoria").value =
        dati.categoria || "";

    document.getElementById("luogo").value =
        dati.luogo || "";

    document.getElementById("squadraCasa").value =
        dati.squadraCasa || "";

    document.getElementById("squadraOspite").value =
        dati.squadraOspite || "";

    aggiornaTotale();
}

/* SALVATAGGIO DATI */

function salvaDati()
{
    let dati = {};

    contatori.forEach(id => {
        dati[id] =
            document.getElementById(id).textContent;
    });

    dati.dataPartita =
        document.getElementById("dataPartita").value;

    dati.categoria =
        document.getElementById("categoria").value;

    dati.luogo =
        document.getElementById("luogo").value;

    dati.squadraCasa =
        document.getElementById("squadraCasa").value;

    dati.squadraOspite =
        document.getElementById("squadraOspite").value;

    localStorage.setItem(
        "erroriPallavolo",
        JSON.stringify(dati)
    );
}

/* MODIFICA CONTATORI */

function modifica(id, delta)
{
    let elemento =
        document.getElementById(id);

    let valore =
        parseInt(elemento.textContent);

    valore += delta;

    if(valore < 0)
    {
        valore = 0;
    }

    elemento.textContent = valore;

    aggiornaTotale();
    salvaDati();
}

/* AGGIORNAMENTO TOTALE */

function aggiornaTotale()
{
    let totale = 0;

    contatori.forEach(id => {

        totale += parseInt(
            document.getElementById(id).textContent
        );

    });

    document.getElementById("totale").textContent =
        "Totale Errori: " + totale;
}

/* RESET */

function resetContatori()
{
    if(!confirm("Azzerare tutti i dati?"))
    {
        return;
    }

    contatori.forEach(id => {
        document.getElementById(id).textContent = 0;
    });

    aggiornaTotale();
    salvaDati();
}

/* REPORT TXT */

function generaReport()
{
    const dataPartita =
        document.getElementById("dataPartita").value;

    const categoria =
        document.getElementById("categoria").value;

    const luogo =
        document.getElementById("luogo").value;

    const squadraCasa =
        document.getElementById("squadraCasa").value;

    const squadraOspite =
        document.getElementById("squadraOspite").value;

    const battuta =
        document.getElementById("battuta").textContent;

    const difesa =
        document.getElementById("difesa").textContent;

    const palleggio =
        document.getElementById("palleggio").textContent;

    const attacco =
        document.getElementById("attacco").textContent;

    const ricezione =
        document.getElementById("ricezione").textContent;

    const muro =
        document.getElementById("muro").textContent;

    const totale =
        parseInt(battuta) +
        parseInt(difesa) +
        parseInt(palleggio) +
        parseInt(attacco) +
        parseInt(ricezione) +
        parseInt(muro);

    let report = "";

    report += "====================================\n";
    report += "REPORT ERRORI PALLAVOLO\n";
    report += "====================================\n\n";

    report += "Data: " + dataPartita + "\n";
    report += "Categoria: " + categoria + "\n";
    report += "Luogo: " + luogo + "\n";
    report += "Squadra Casa: " + squadraCasa + "\n";
    report += "Squadra Ospite: " + squadraOspite + "\n\n";

    report += "ERRORI\n";
    report += "------------------------------------\n";
    report += "Battuta: " + battuta + "\n";
    report += "Difesa: " + difesa + "\n";
    report += "Palleggio: " + palleggio + "\n";
    report += "Attacco: " + attacco + "\n";
    report += "Ricezione: " + ricezione + "\n";
    report += "Muro: " + muro + "\n";
    report += "------------------------------------\n";
    report += "Totale Errori: " + totale + "\n";

    const blob = new Blob(
        [report],
        { type: "text/plain" }
    );

    const link =
        document.createElement("a");

    link.href =
        URL.createObjectURL(blob);

    link.download =
        "Report_Errori_Pallavolo.txt";

    document.body.appendChild(link);

    link.click();

    document.body.removeChild(link);
}

/* SALVATAGGIO AUTOMATICO CAMPI */

document.querySelectorAll("input")
    .forEach(input => {
        input.addEventListener(
            "input",
            salvaDati
        );
    });

/* AVVIO */

caricaDati();

</script>

</body>
</html>
