<p align="center">
  <img src="https://img.shields.io/badge/Standard-UHFS_V2-purple?style=for-the-badge&logo=hard-drive" alt="Standard">
  <img src="https://img.shields.io/badge/Physics-Zero--Copy-green?style=for-the-badge&logo=speedtest" alt="Physics">
  <img src="https://img.shields.io/badge/Topology-Logarithmic_Spiral-blue?style=for-the-badge&logo=spiral" alt="Geometry">
</p>

<div align="center">
  <h1>💿 UHFS : Universal Holographic File System</h1>
  <h3>Le Ruban Infini (.496 Tape)</h3>
  <p><em>"L'information n'est plus lue, elle est instanciée."</em></p>
</div>

---

## 🌌 Le Problème : L'Entropie de Traduction

Dans tous les systèmes actuels (Linux, Windows), lire un fichier coûte cher.
$$T(Total) = T(Seek) + T(Read) + T(Decode) + T(Parse)$$
Chaque étape génère de la chaleur (Entropie) et perd du temps (Latence). Le disque dur et la RAM parlent deux langues différentes.

**La Solution UHFS :** L'Isomorphisme.
Le format sur le disque est **identique au bit près** à la structure en mémoire.
$$T(Total) \to 0$$
Quand l'OS "ouvre" un fichier, il ne le lit pas. Il pointe simplement dessus.

---

## 📜 Les 3 Axiomes du Ruban

UHFS remplace les "Fichiers" et "Dossiers" par un continuum de cellules atomiques régies par trois lois immuables (définies dans `tape_axioms.md`) :

### Axiome $\alpha$ : La Cellule Discrète
> Toute information est quantifiée en blocs atomiques de **496 bits** (Atoms). Il n'existe pas de "demi-donnée". L'atome est indivisible.

### Axiome $\beta$ : L'Adressage Spiralé
> L'emplacement d'un bloc n'est pas linéaire (LBA). Il suit une **Spirale Logarithmique** déterminée par le Nombre d'Or ($\varphi$).
> $$P(B_{n+1}) = P(B_n) + (\text{Offset} \cdot \varphi)$$
> *Conséquence :* Plus on s'éloigne du centre (Boot), plus l'espace de stockage s'étend, sans fragmentation.

### Axiome $\gamma$ : La Continuité Temporelle
> La validité d'un bloc est certifiée par sa position unique dans la séquence des décimales de $\pi$ (Le **$\pi$-Index**). Le temps sert de clé de validation (Proof of Time).



---

## ⚙️ The Oracle Machine

UHFS n'utilise pas de "pilote" passif. Il utilise un moteur actif appelé l'**Oracle Machine** (`oracle_machine.py`).
C'est une boucle de lecture/vérification qui agit comme le gardien de l'intégrité du système.

1.  **FETCH (Atomic Load) :** Récupération brute du bloc 496 bits.
2.  **VERIFY (The Security Gate) :** Calcul du score d'Harmonie ($\mathcal{H}$).
    * Si $\mathcal{H} < 0.618$ : **Dissonance détectée**. Le bloc est physiquement rejeté (non chargé en RAM).
3.  **EXECUTE (Zero-Copy) :** Mappage direct du pointeur mémoire.

### Structure de l'Atome (Rust)
```rust
// Structure alignée sur 512 bits (496 data + padding)
#[repr(C, align(64))] 
struct Universal_Atom_496 {
    magic_signature: u128,   // Ancrage Physique
    pi_index_start: u64,     // Ancrage Temporel (Axiome Gamma)
    root_geo_hash: u128,     // Ancrage Spatial (Axiome Beta)
    payload: [u8; 38],       // Contenu (Segment Majeur)
}
