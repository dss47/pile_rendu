# Pile générique — Vérification d'expression — Bibliothèque .so

Contenu :
- `src/test.cpp` — petit test qui utilise `expressionValide()`
- `src/test_piles.cpp` — menu interactif des piles (pas besoin de la lib)
- `src/PileTableau.h` — pile avec tableau dynamique
- `src/PileChainee.h` — pile avec liste chaînée
- `src/verification.cpp` — source de référence de la lib (non utilisé à la compilation de `test.cpp`)
- `lib/libverification.so` — la bibliothèque (équivalent Linux d'une `.dll` Windows)

## 1) Compiler le test avec la lib (depuis `pile_rendu`)

```bash
g++ src/test.cpp -Llib -lverification -o test_expr -Wl,-rpath,'$ORIGIN/lib'
```

- `-Llib` : dossier où se trouve la lib (ici `lib/`, **pas** à côté du `.cpp`)
- `-lverification` : utilise `lib/libverification.so` (sans préfixe `lib`, sans `.so`)
- `-rpath` : mémorise le chemin de la lib → `./test_expr` marche directement

## 2) Exécuter

```bash
./test_expr
```

(affiche `Invalide` car l'expression testée `"1+2)"` est fausse)

## 3) Autre exemple, sans la lib (headers uniquement)

```bash
g++ src/test_piles.cpp -o test_piles
./test_piles
```

## Comment la lib a été construite (une seule fois)

```bash
g++ -fPIC -shared verification.cpp -o lib/libverification.so
```

(`-fPIC` = code relogeable, `-shared` = format `.so` partagé)

## Vérifier que le binaire utilise bien la lib séparée

```bash
ldd test_expr | grep verification
# affiche : libverification.so => .../pile_rendu/lib/libverification.so
```
