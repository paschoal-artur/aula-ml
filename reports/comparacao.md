# Comparação dos modelos

O dataset `chamados-v2` corrige o rótulo do chamado de treino `006`, “bloqueio de acesso”, de `comum` para `urgente`. O código, os parâmetros e a partição foram os mesmos nas duas rodadas.

| Métrica | `modelo-r1` com `dados-v1` | `modelo-r2` com `dados-v2` |
| --- | ---: | ---: |
| Acurácia | 75% (3/4) | 100% (4/4) |
| Recall de urgentes | 50% (1/2) | 100% (2/2) |
| Recall de comuns | 100% (2/2) | 100% (2/2) |

O modelo r2 classificou corretamente o chamado de teste `010`, “bloqueio de conta”, após aprender a regra `bloqueio → urgente`. O SHA-256 da partição coincidiu nas duas rodadas. Os SHA-256 do dataset e do modelo mudaram.

## Restauração de r1

Selecionei `modelo-r1` em um clone limpo do [repositório no GitHub](https://github.com/paschoal-artur/aula-ml), no commit `9f2962612a3aca8184154f5b6bd2212c96e0f799`. Antes de `dvc pull`, o clone tinha os descritores DVC. Após a recuperação pelo remoto `storage`, os três SHA-256 abaixo coincidiram com o manifesto de r1. A restauração foi feita sem novo treinamento.

| Arquivo | SHA-256 verificado |
| --- | --- |
| `data/chamados.csv` | `bc3fd86a3a5f598841a58839a9726d901fa7e8361ffe79c5b43873ad8a1328ac` |
| `data/split.csv` | `2ef820171c1a4625fe216e6da67c7c8dcf9d97a08288c0239cd23f1a6961d981` |
| `models/model.json` | `72a3a67936139a3473838a6dcfa7aae1acb4c23467be1ef1056d51d485b94840` |

## Decisão

Aceitar r2 para o exercício. O recall de urgentes subiu de 50% para 100% com o recall de comuns em 100% na mesma partição. O teste contém quatro exemplos sintéticos, dois por classe.
