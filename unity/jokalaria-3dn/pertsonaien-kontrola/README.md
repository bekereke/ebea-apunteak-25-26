# Pertsonaien kontrola

#### Input System

Unity-k bi sarrera kudeaketa sistema desberdin ditu: **Legacy Input Manager** (zaharra eta lehenetsia) eta **Sarrera Sistema Berria** (New Input System). Sarrera Sistema Berria pakete baten bidez instalatu behar da. Legacy sistema oso sinplea da eta ezin hobea da prototipo azkarrak egiteko. Sarrera Sistema Berria, aldiz, askoz malguagoa eta ahalmen handiagokoa da, ezaugarri ugari eskaintzen ditu, baina apur bat konplexuagoa da.

Ikastaroan, Legacy sistemarekin hasi ondoren, kodea **refakturatzen** irakasten da, Sarrera Sistema Berria erabiltzeko. Refakturatze hau kode garbia idazteko eta kalitatea hobetzeko funtsezko gaitasuna da.

**Ezaugarri nagusiak eta ezarpena:**

* Instalazioan, erabiltzaileari galdetzen zaio sistema berria aktibatu nahi duen. Ezarpenetan (`Project Settings > Player > Other Settings > Active Input Handling`), bi sistemak batera (`Both`) aktibatzea gomendatzen da.
* Sistema hau erabiltzeko, **Input Actions Asset** bat sortzen da. Hau kudeaketa-leihoan editatzen da, eta bertan **Action Maps** (Ekintza Mapak), **Actions** (Ekintzak) eta hauen propietateak definitzen dira.
* Ekintzak lotura (bindings) anitzak izan ditzake, adibidez, WASD teklak (konposite gisa) eta gezi-teklak mugimendurako.
* Sarrera kontrolatzaileetarako (GamePad), **dead zone** (zona hil) prozesadoreak gehitu behar dira GamePad-en binding-etan, nahi gabeko mugimenduak (drift) saihesteko.
* Kodean erabiltzeko, gomendatzen da Input Actions Asset-etik C# Klase bat sortzea. Klase hau instantziatu eta behar den Action Map-a aktibatu behar da `.Enable()` funtzioarekin. Mugimendu bezalako ekintza etengabeak `.ReadValue<Vector2>()` erabiliz irakur daitezke.
* Interakzio puntualetarako, sistemak C# **gertaerak** (events) eskaintzen ditu (`Performed`, `Started`, `Cancelled`), etengabe `Update` funtzioan sarrera bilatu beharrean.
* Sistema berriaren onura nagusietako bat da **kontrolagailuen euskarria** (GamePad) eta tekla ezberdinak oso erraz gehitzea, C# kodean aldaketarik egin behar izan gabe, lotura berriak gehituta soilik.
* **Gakoen bindeatze interaktiboa (Rebinding)** ezar daiteke, jokalariak kontrolak aldatzeko aukera izan dezan `PerformInteractiveRebinding` funtzioaren bidez. Aldaketak `PlayerPrefs` barruan gorde daitezke JSON formatuan (`SaveBindingsOverridesAsJson`).
