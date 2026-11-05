# zkBrief — konfiguracja enklawy Tinfoil

Publiczne repo z konfiguracją kontenera `zkbrief` w Tinfoil Containers. Tinfoil mierzy `tinfoil-config.yml` przy każdym wydaniu i publikuje pomiar w logu Sigstore. Aplikacje zkBrief sprawdzają enklawę względem wydań **tego** repo, zanim wyślą jej zapieczętowany profil.

| Repo | Widoczność | Zawartość |
|---|---|---|
| `zkBrief/tinfoil-config` (to repo) | publiczne | `tinfoil-config.yml`, workflowy wydania |
| `zkBrief/tinfoil` | prywatne | kod enklawy, `Dockerfile`, budowa obrazu `ghcr.io/zkbrief/tinfoil` |

W tym repo nie ma sekretów, tylko ich nazwy (`TINFOIL_API_KEY`, `CLIENT_TOKENS`, `BACKEND_TOKENS`, `PROFILE_KEY_SEED`). Wartości są w Tinfoil jako sekrety repo `zkBrief/tinfoil-config`.

## Nowe wydanie

1. W `zkBrief/tinfoil`: `gh workflow run build-image.yml -R zkBrief/tinfoil -f version=vX.Y.Z`. Digest jest w podsumowaniu przebiegu.
2. Tutaj: wpisz digest w `image:` w `tinfoil-config.yml`, commit i push.
3. `gh workflow run tinfoil-release.yml -R zkBrief/tinfoil-config -f version=vX.Y.Z`. Workflow tworzy tag i uruchamia *Tinfoil Release - Publish* (pomiar, Sigstore, GitHub Release z `tinfoil-deployment.json` i `tinfoil.hash`).
4. Pierwszy raz: `tinfoil container create zkbrief --repo zkBrief/tinfoil-config --tag vX.Y.Z` z czterema `--secret`. Kolejne: `tinfoil container update zkbrief --tag vX.Y.Z`.

Tagu wydania nie da się użyć ponownie. Każda zmiana configu (także `env`) to nowe wydanie.

Szczegóły: `docs/DEPLOY.md` w `zkBrief/tinfoil`.
