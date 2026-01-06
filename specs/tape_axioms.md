# UHFS Specification: The 3 Immutable Axioms
**Version:** 2.1.6
**Reference:** LMC-WP-496-01

---

## 1. Les Axiomes Fondamentaux

### Axiome $\alpha$ (La Cellule Discrète)
> Toute information est quantifiée en blocs atomiques de **496 bits**.
> Il n'existe pas de "demi-donnée". L'atome est indivisible.

### Axiome $\beta$ (L'Adressage Récursif)
> L'emplacement d'un bloc $B_{n+1}$ n'est pas linéaire. Il suit une spirale logarithmique déterminée par $\varphi$.
> $$P(B_{n+1}) = P(B_n) + (\text{Offset} \cdot \varphi)$$

### Axiome $\gamma$ (La Continuité Temporelle)
> La validité d'un bloc est certifiée par sa position unique dans la séquence des décimales de $\pi$ (Le $\pi$-Index).

---

## 2. The Universal Atom Structure (Rust Definition)
*Aligned with FC-496 V2.1.6 specs.*

```rust
// ALIGNMENT: 512-bit register (496 bits data + 16 bits padding)
#[repr(C, align(64))] 
struct Universal_Atom_496 {
    // --- COUCHE I : ANCRAGE PHYSIQUE (Vibration) ---
    // Magic Signature: 0x1F0 (496)
    magic_signature: u128,

    // --- COUCHE II : ANCRAGE TEMPOREL (Synchronisation) ---
    // Axiome Gamma: Position dans Pi
    pi_index_start: u64,
    pi_checksum: u64,

    // --- COUCHE III : ANCRAGE SPATIAL (Topologie) ---
    // Coordonnée fractale (Geo-Hash)
    root_geo_hash: u128,

    // --- COUCHE IV : LOGIQUE & STRUCTURE (Sens) ---
    // Axiome Beta: Pointeur spiralé
    phi_ratio_check: u64,    // H-Scale Integrity
    schema_class: u32,       // Data Type
    next_block_offset: u16,  // Distance to child atom
    flags: u16               // Permissions
}
---
