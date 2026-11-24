# Atividade Prática 07 — Versionamento de dados e modelos

O classificador separa chamados sintéticos em `comum` e `urgente`. Ele aprende a classe majoritária por primeira palavra e usa a classe majoritária do treino para palavras desconhecidas. A partição fixa usa seis chamados para treino e quatro para teste.

| Dataset | Modelo | Rótulo do chamado `006` |
| --- | --- | --- |
| `dados-v1` | `modelo-r1` | `comum` |
| `dados-v2` | `modelo-r2` | `urgente` |

A avaliação e a decisão estão em [reports/comparacao.md](reports/comparacao.md). Cada tag de modelo contém o manifesto de execução e as métricas da rodada em `reports/`.

## Pré-requisitos e armazenamento

- Git, Python 3.10 ou superior e [DVC 3](https://doc.dvc.org/install).
- O treino usa apenas a biblioteca padrão do Python.
- O remoto DVC padrão, `storage`, aponta para `../dvc-storage`. Coloque uma cópia do armazenamento original ao lado do clone. O Git guarda os descritores `.dvc`, manifestos, parâmetros, código e relatórios. O armazenamento DVC contém os CSVs e os modelos.

Se o armazenamento estiver em outro local, configure o caminho absoluto no clone.

```bash
dvc remote modify --local storage url /caminho/para/dvc-storage
```

## Restaurar a primeira versão

Execute a partir do diretório que contém `dvc-storage`.

```bash
git clone https://github.com/paschoal-artur/aula-ml.git aula-ml-restauracao
cd aula-ml-restauracao
git switch --detach modelo-r1
git status --short
dvc pull
git rev-parse HEAD
```

Confira os SHA-256 dos três arquivos recuperados com os campos correspondentes de `reports/modelo.json`.

```bash
python3 - <<'PY'
import hashlib
import json
from pathlib import Path

manifesto = json.loads(Path("reports/modelo.json").read_text(encoding="utf-8"))
if (manifesto["model_id"], manifesto["dataset_id"]) != ("chamados-r1", "chamados-v1"):
    raise SystemExit("Versão incorreta no manifesto.")

for arquivo, campo in (
    ("data/chamados.csv", "dataset_sha256"),
    ("data/split.csv", "split_sha256"),
    ("models/model.json", "artifact_sha256"),
):
    sha256 = hashlib.sha256(Path(arquivo).read_bytes()).hexdigest()
    if sha256 != manifesto[campo]:
        raise SystemExit(f"Hash divergente para {arquivo}")
    print(f"OK {arquivo} {sha256}")
PY
```
