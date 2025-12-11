# VR Garapena

<details>

<summary>ADIBIDE PRAKTIKOA</summary>

## EM Kuboa

<kbd>Meta XR All-in-One Building Block</kbd>a erabiliko da Unityren 6 bertsioan.

### 1. Oinarrizko Setup-a (Meta XR All-in-One SDK)

Kamera eta Passthrough eskuz konfiguratu beharrean, bloke bakar bat erabiliko dugu.

1. Eszena garbi batean (ezabatu `Main Camera` badago).
2. Joan Meta > Building Blocks leihora.
3. Arrastatu Meta XR All-in-One blokea eszenara.
4. Aukeratu `[BuildingBlock] Meta XR All-in-One` Hierarchy-an.
5. Inspector-ean, ziurtatu Passthrough atala Enabled edo aktibatuta dagoela.

> Argazkia: _Meta XR All-in-One blokea Hierarchy-an eta bere Inspector-a Passthrough aukerarekin._

### 2. Gela Kargatu (<kbd>Scene Mesh</kbd>)

All-in-One daukazun arren, gela kargatzeko blokea gehitu behar duzu.

1. Arrastatu Scene Mesh blokea eszenara (Building Blocks leihotik).
2. Play ematean gela ez bada agertzen, ziurtatu All-in-One barruko `OVRManager` ezarpenetan Scene Support aktibatuta dagoela (askotan All-in-One-k automatikoki egiten du, baina egiaztatzea ondo dago).

> Argazkia: _Scene Mesh blokea eszenan gehituta._

### 3. Kuboa Sortu eta Konfiguratu (Fisika)

Zati hau berdin mantentzen da, funtsezkoa baita zero grabitaterako.

1. Sortu: <kbd>GameObject > 3D Object > Cube</kbd>.
2. Gehitu osagaia: <kbd>Rigidbody</kbd>.
3. Konfigurazio zehatza Inspector-ean:
   * Mass: `0.01` (Arina).
   * Linear Damping: `0` (Frenorik ez).
   * Angular Damping: `0.05`.
   * Use Gravity: ⬜ DESMARKATU.
   * Is Kinematic: ⬜ DESMARKATU.
   * Collision Detection: Continuous Dynamic.
   * Constraints: Ziurtatu `Freeze Position` guztiak hutsik daudela.

> Argazkia: _Kuboaren Rigidbody ezarpenak._

### 4. Eskuak Aurkitu eta Prestatu (Garrantzitsua)

"All-in-One" prefab bat denez, eskuak barruan "ezkutatuta" daude. Honela aurkitu behar dituzte ikasleek:

1. Hierarchy-an, zabaldu `[BuildingBlock] Meta XR All-in-One` objektuaren gezia.
2.  Jarraitu bide hau:

    CameraRig > TrackingSpace > LeftHandAnchor.
3. LeftHandAnchor aukeratuta, gehitu osagaiak Inspector-ean:
   * Sphere Collider (Radius: `0.05`).
   * Rigidbody:
     * Use Gravity: ⬜ Desmarkatu.
     * Is Kinematic: ✅ MARKATU.
     * Collision Detection: Continuous Dynamic.
4. Errepikatu `RightHandAnchor`-arekin (TrackingSpace barruan dagoena ere).

> Argazkia: _Hierarchy-an All-in-One zabalduta LeftHandAnchor aurkitu arte, eta bere Inspector-a._

### 5. Material Fisikoa (Irristakorra)

Kuboa gainazaletan ez itsasteko.

1. Sortu Physic Material bat (Project leihoan) `ZeroFriction` izenarekin.
2. Ezarri balioak:
   * Friction (Dynamic/Static): `0`
   * Bounciness: `0.8`
   * Friction Combine: `Minimum`
   * Bounce Combine: `Maximum`
3. Arrastatu material hau Kuboaren <kbd>Box Collider</kbd>-era.

> Argazkia: _Physic Materialaren ezarpenak._

***

{% hint style="info" %}
**Oharra:**

"All-in-One" erabiltzean, gogoratu beti TrackingSpace karpeta bilatu behar dela kameraren eta eskuen osagaiak aurkitzeko. Prefab bat denez, geruzaka antolatuta dago.
{% endhint %}

</details>
