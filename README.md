# Calculadora de Saúde 🩺

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-corrigido-brightgreen)

## Descrição do Projeto

Sistema de linha de comando em Python com um menu interativo que calcula três indicadores de saúde: **IMC** (Índice de Massa Corporal, com classificação), **recomendação diária de ingestão de água** e **frequência cardíaca máxima**. O código original continha bugs de lógica, de tipo e de fluxo de execução; este repositório documenta e corrige cada um deles.

## Relatório de Bugs Encontrados

| # | Local do Bug | Comportamento Incorreto Observado | Solução Aplicada |
|---|---|---|---|
| 1 | `calcular_imc()` | O IMC era calculado como `peso / (altura * 2)` — multiplicação em vez de potenciação —, gerando um valor matematicamente incorreto (a fórmula real é peso / altura²) | Trocado `altura * 2` por `altura ** 2` |
| 2 | `classificar_imc()` | As faixas usavam `> 18.5 and < 24.9`, `> 25.0 and < 29.9` etc., deixando lacunas (18.5, 24.9–25.0, 29.9–30.0). Um IMC exatamente nesses valores caía em nenhuma condição e a função retornava `None` | Reescritas as comparações como `< 18.5` / `< 25.0` / `< 30.0` / `else`, cobrindo todo o intervalo sem sobreposição nem lacunas |
| 3 | `calcular_agua_diaria()` | Dividia o peso por 35 (`peso / 35`) em vez de multiplicar pela recomendação de 35ml por kg, retornando uma meta de água absurdamente baixa | Trocado `peso / 35` por `peso * 0.035` (35ml por kg, já convertido para litros) |
| 4 | `calcular_frequencia_cardiaca_maxima()` | Somava a idade a 220 (`220 + idade`) em vez de subtrair, fazendo a FC máxima *aumentar* com a idade — o oposto do que acontece fisiologicamente | Trocado `220 + idade` por `220 - idade` |
| 5 | `menu()` | `input()` sempre retorna uma `string`, mas o valor não era convertido antes de ser comparado com números inteiros no `main()` | `menu()` agora converte a opção com `int()` (com tratamento de erro via `try/except`) antes de retornar |
| 6 | `main()` (comparações do menu) | Como consequência do Bug 5, condições como `if opcao == 1` nunca eram verdadeiras (comparava `int` com `str`), então nenhuma opção do menu funcionava | Corrigido junto com o Bug 5, comparando `int` com `int` |
| 7 | `main()` (opção "Sair") | Faltava um `break` após a opção 4: o programa imprimia a mensagem de despedida e voltava ao menu, entrando em loop infinito | Adicionado `break` logo após o `print` de encerramento |

## Como Executar

### Pré-requisitos
- Python 3.x instalado (nenhuma biblioteca externa é necessária)

### Passo a passo
```bash
# 1. Clone o seu fork
git clone https://github.com/SEU-USUARIO/gqs-calculadora-saude-py.git
cd gqs-calculadora-saude-py

# 2. Execute o programa
python calculadora_saude.py
```

> No Linux/macOS pode ser necessário usar `python3` em vez de `python`.

### Exemplo de uso
```
==============================
 SISTEMA DE SAÚDE E BEM-ESTAR
==============================
1. Calcular IMC
2. Calcular Recomendação de Água
3. Calcular Frequência Cardíaca Máxima
4. Sair
Escolha uma opção (1-4): 1
Digite seu peso (kg): 70
Digite sua altura (m): 1.75
Seu IMC é: 22.86
Classificação: Peso normal
```

## Sobre o Autor

Correções e documentação realizadas por **Vinícius**, como atividade prática (Lista de Exercícios IV) da disciplina **Gestão e Qualidade de Software**, ministrada pelo Prof. Daniel Henrique Matos de Paiva.
