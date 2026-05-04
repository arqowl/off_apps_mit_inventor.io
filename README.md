# 🏓 Plano de Aula — Oficina de Aplicativos: Jogo de Ping Pong
**MIT App Inventor | Ensino Médio | 3 horas**

---
## ⏱ 00:00 – 00:30 | CONEXÃO

### Objetivo
Quebrar o gelo, mapear o nível da turma e criar senso de propósito.

### Dinâmica (25 min)
**Rodada de apresentação rápida (2 min por aluno, máx. 10 alunos):**
Cada aluno responde 3 perguntas no estilo "speed round":

**Perguntas de nivelamento (levantar a mão):**

### Conceito-âncora (5 min)
Escreva no quadro:
```
PROGRAMAR = DAR INSTRUÇÕES PARA O COMPUTADOR
(igual uma receita de bolo, mas sem tolerância a erros)
```
Mostre que o App Inventor usa **blocos visuais** — ninguém vai digitar código hoje, mas toda a lógica é real.

---

## ⏱ 00:30 – 01:00 | INTERFACE (UI)

### Objetivo
Apresentar os componentes do projeto *antes* de abrir o computador.

### O que mostrar (baseado no Screen1.scm)

Desenhe no quadro uma tela de celular esquemática:

```
┌─────────────────────────────────┐
│  [Pontos: 0]  [START] [PAUSE]   │  ← OrganizaçãoHorizontal
│              [Som On] [Avançado]│
├─────────────────────────────────┤
│                                 │
│                                 │
│           ●  (Bola)             │  ← Canvas (Pintura1)
│                                 │     background: mountain_background.png
│                                 │
│         [════] (Raquete)        │
└─────────────────────────────────┘
```

### Componentes a explicar (tabela para o quadro)

| Componente | Tipo | Para que serve |
|---|---|---|
| `Pontos` | Label | Mostra o placar na tela |
| `START` | Button (vermelho) | Inicia/reinicia o jogo |
| `PAUSE` | Button (amarelo) | Pausa o movimento da bola |
| `Som On` | CheckBox | Liga/desliga os sons |
| `Avançado` | CheckBox | Ativa dificuldade crescente |
| `Pintura1` | **Canvas** | O "campo" do jogo — onde tudo acontece |
| `Bola1` | Ball (sprite) | A bola — tem velocidade e direção |
| `Raquete` | ImageSprite | A raquete — imagem que o jogador controla |
| `Som1` | Sound | Toca arquivos .mp3 |

### Ponto-chave a enfatizar
> **"Interface genérica e intuitiva"** = se alguém nunca viu o app, em 5 segundos entende o que fazer. Botões com cores semânticas (verde=vai, vermelho=para, amarelo=calma), textos claros, layout limpo.


---

## ⏱ 01:00 – 01:30 | LÓGICA CONCEITUAL

### Objetivo
Ensinar os 5 blocos-base que aparecem no projeto.

### 1. Variável Global
```
🗃️ VARIÁVEL = uma caixinha com nome que guarda um valor
   → global Pontos = 0
   (começa em zero, muda durante o jogo)
```

### 2. Se–Então (Condicional)
```
🔀 SE [condição verdadeira] ENTÃO [faça isso]
         SENÃO [faça aquilo]

Exemplo real do jogo:
  SE a bola chegou na borda de baixo (edge = -1)
    ENTÃO → GAME OVER
    SENÃO → rebate normal
```

### 3. Lógica Booleana
```
✅ VERDADEIRO / FALSO

No jogo:
  Bola1.Enabled = TRUE  → bola se move
  Bola1.Enabled = FALSE → bola congela

Também: condição AND (E)
  "Dificuldade marcada E pontos par?" → acelera a bola
```

### 4. Operações Matemáticas
```
➕ ➖ ÷ MOD (resto da divisão)

Exemplos reais:
  Pontos + 1          → incrementa placar
  Canvas.Width / 2    → centraliza a bola
  360 - Heading       → inverte a direção (rebote)
  Pontos MOD 2 == 0   → verifica se é par (para acelerar)
```

### 5. Procedimentos (sem retorno)
```
📦 PROCEDIMENTO = bloco de ações com nome
   → você cria uma vez, chama várias vezes

---

## ⏱ 01:30 – 02:30 | DESIGN DE ALGORITMO (QUADRO BRANCO)

### Objetivo
Traduzir o código em esboços visuais. A turma entende o algoritmo completo *antes* de abrir o App Inventor.

---

### BLOCO A — Variável e Procedimento Principal

**Escreva no quadro:**
```
VARIÁVEL GLOBAL: Pontos = 0

┌─────────────────────────────────────────────┐
│  PROCEDIMENTO: AlterarEMostrarPontuacao      │
│  Recebe: NovoPonto                           │
│                                              │
│  1. global Pontos ← NovoPonto               │
│  2. Label "Pontos".Texto ← "PONTOS: " + Pontos │
└─────────────────────────────────────────────┘
```
*"Esse procedimento faz duas coisas: salva o novo valor E mostra na tela. Ele é chamado sempre que o placar muda."*

---

### BLOCO B — Botão START

**Escreva no quadro:**
```
QUANDO START for clicado:
  1. Bola → mover para o CENTRO do Canvas
             (x = Canvas.Width / 2, y = Canvas.Height / 2)
  2. Bola.Enabled = TRUE      ← ativa o movimento
  3. Bola.Direção = aleatório entre 225° e 315° ← aponta para baixo
  4. Bola.Velocidade = 5
  5. Bola.Intervalo = 10 ms
  6. Raquete → mover para o centro-baixo da tela
  7. Chamar: AlterarEMostrarPontuacao(0) ← zera o placar
```

**Desenhe no quadro os ângulos:**
```
        90° (cima)
         ↑
180° ←──●──→ 0°
         ↓
        270° (baixo)

Ângulos 225°–315° = cone voltado para baixo
(bola sempre começa descendo em direção à raquete)
```

---

### BLOCO C — Movimento da Raquete

**Escreva no quadro:**
```
QUANDO o jogador arrastar o dedo no Canvas:
  Raquete.x ← posição X do toque (currentX)
  (só move na horizontal — Y não muda)
```
*"A raquete segue o dedo. Simples assim."*

---

### BLOCO D — Colisão da Bola com a Raquete

**Escreva no quadro:**
```
QUANDO Bola1 colidir com algum objeto:
  1. Bola.Direção ← 360 - Bola.Direção  ← REBOTE
  2. Chamar: AlterarEMostrarPontuacao(Pontos + 1)
  3. Chamar: PlaySom("pong.mp3")
  4. Chamar: FlagDificuldade()
```

**Explique o rebote geometricamente:**
```
  Bola vindo em 270° (reto para baixo)
  Rebote = 360 - 270 = 90° (reto para cima) ✓

  Bola vindo em 250°
  Rebote = 360 - 250 = 110° ✓
```

---

### BLOCO E — Borda Atingida (Game Over ou Rebate)

**Escreva no quadro:**
```
QUANDO Bola1 atingir uma borda:

  SE borda == -1 (borda de BAIXO = o jogador perdeu):
    → Label "Pontos".Texto = "GAME OVER"
    → Bola1.Enabled = FALSE  (para a bola)
    → PlaySom("note-do.mp3")

  SENÃO (borda lateral ou de cima):
    → Bola1.Bounce(borda)    (rebate automaticamente)
    → PlaySom("single-knock.mp3")
```

**Tabela de bordas para o quadro:**
```
  Borda  1 = cima
  Borda  2 = direita
  Borda -2 = esquerda
  Borda -1 = BAIXO (Game Over!)
```

---

### BLOCO F — Dificuldade Crescente

**Escreva no quadro:**
```
PROCEDIMENTO: FlagDificuldade

  SE (CheckBox "Avançado" está marcado)
     E (Pontos MOD 2 == 0)  ← ou seja, a cada 2 pontos
  ENTÃO:
    Bola.Velocidade = Bola.Velocidade + 1
```
*"A cada 2 rebatidas, a bola fica 1 unidade mais rápida. Quem deixar desmarcado joga no modo normal."*

---

### BLOCO G — PlaySom (Procedimento)

**Escreva no quadro:**
```
PROCEDIMENTO: PlaySom(Arquivo)

  SE CheckBox "Som On" está marcado:
    Som1.Fonte ← Arquivo
    Som1.Tocar()
```

---

### Fluxo completo (diagrama resumido)

```
[Usuário clica START]
        ↓
[Bola vai pro centro, direção aleatória, placar = 0]
        ↓
[Jogo rodando — usuário arrasta dedo para mover raquete]
        ↓
    ┌───────────────────────────┐
    │ Bola atingiu borda?        │
    │  → SIM: cima/lado → rebate │
    │  → SIM: baixo → GAME OVER  │
    └───────────────────────────┘
        ↓
    ┌───────────────────────────┐
    │ Bola colidiu com raquete?  │
    │  → SIM: rebate, +1 ponto   │
    │         verifica dificul.  │
    └───────────────────────────┘
```

---

## ⏱ 02:30 – 02:45 | PAUSA ☕

---

## ⏱ 02:45 – 03:25 | HANDS-ON (MIT App Inventor)

### Pré-requisito
Todos com conta no App Inventor (ai2.appinventor.mit.edu) e projeto aberto.

---

### PASSO 1 — Criar a Interface (Designer) [10 min]

1. Arraste um **HorizontalArrangement** → `AlignHorizontal: Center`
   - Dentro: 3 **VerticalArrangements**
2. No 1º Vertical: arraste **Label** → renomeie para `Pontos`, texto `"Pontos: 0"`, negrito, tamanho 20
3. No 2º Vertical: arraste dois **Buttons**
   - Renomeie: `START` (cor vermelha, forma oval) e `PAUSE` (cor amarela, forma oval)
4. No 3º Vertical: arraste dois **CheckBoxes**
   - `CaixaDeSeleção_Som` → texto `"Som On"`, marcado
   - `CaixaDeSeleção_Dificuldade` → texto `"Avançado"`, marcado
5. Abaixo do HorizontalArrangement: arraste um **Canvas** → renomeie `Pintura1`
   - Width: Fill Parent | Height: Fill Parent
   - Imagem de fundo: faça upload de `mountain_background.png`
6. Dentro do Canvas: arraste um **Ball** → renomeie `Bola1`
   - Cor: azul | Raio: 10 | Velocidade: 5
7. Dentro do Canvas: arraste um **ImageSprite** → renomeie `Raquete`
   - Imagem: `paddle.png` | Largura: 100 | Altura: 25
8. No painel **Non-visible**: arraste **Sound** → renomeie `Som1`

---

### PASSO 2 — Variável e Procedimento (Blocos) [5 min]

1. **Variables** → "initialize global `Pontos` to" → `0`
2. **Procedures** → "to do" → renomeie para `AlterarEMostrarPontuacao`
   - Adicione parâmetro: `NovoPonto`
   - Dentro:
     - `set global Pontos to` → `get NovoPonto`
     - `set Pontos.Text to` → join `"PONTOS: "` + `get global Pontos`

---

### PASSO 3 — Botão START (Blocos) [8 min]

1. `when START.Click do`
2. `Bola1.MoveTo` → x: `Pintura1.Width / 2`, y: `Pintura1.Height / 2`
3. `set Bola1.Enabled to` → `true`
4. `set Bola1.Heading to` → `random integer from 225 to 315`
5. `set Bola1.Speed to` → `5`
6. `set Bola1.Interval to` → `10`
7. `Raquete.MoveTo` → x: `(Screen1.Width/2) - (Screen1.Width/4)`, y: `Screen1.Height - Raquete.Height - 10`
8. Chame `AlterarEMostrarPontuacao` com argumento `0`

---

### PASSO 4 — Mover a Raquete (Blocos) [3 min]

1. `when Pintura1.Dragged do`
2. `Raquete.MoveTo` → x: `currentX`, y: `Raquete.Y`

---

### PASSO 5 — Colisão (Blocos) [5 min]

1. `when Bola1.CollidedWith do`
2. `set Bola1.Heading to` → `360 - Bola1.Heading`
3. Chame `AlterarEMostrarPontuacao` com argumento `global Pontos + 1`
4. Chame `PlaySom` com argumento `"pong.mp3"`
5. Chame `FlagDificuldade`

---

### PASSO 6 — Borda Atingida (Blocos) [5 min]

1. `when Bola1.EdgeReached do`
2. `if edge = -1 then`
   - `set Pontos.Text to "GAME OVER"`
   - `set Bola1.Enabled to false`
   - Chame `PlaySom("note-do.mp3")`
3. `else`
   - `Bola1.Bounce(edge)`
   - Chame `PlaySom("single-knock.mp3")`

---

### PASSO 7 — Procedimentos FlagDificuldade e PlaySom (Blocos) [5 min]

**FlagDificuldade:**
1. `to FlagDificuldade do`
2. `if CaixaDeSeleção_Dificuldade.Checked AND (Pontos MOD 2 = 0) then`
3. `set Bola1.Speed to Bola1.Speed + 1`

**PlaySom:**
1. `to PlaySom Ponto do`
2. `if CaixaDeSeleção_Som.Checked then`
3. `set Som1.Source to get Ponto`
4. `Som1.Play`

---

### PASSO 8 — Testar no celular [4 min]

- App MIT AI2 Companion no celular → escanear QR Code
- Testar START, arrastar dedo, deixar bola cair, verificar GAME OVER

---

## ⏱ 03:25 – 03:30 | ENCERRAMENTO

**Fala final (Austin Kleon — "Roube como um artista"):**

> *"Austin Kleon tem um livro chamado 'Roube como um artista'. A ideia central é: nada é 100% original. Todo artista, todo programador, todo inventor pegou algo que existia, olhou com olhos novos e transformou em algo próprio.*
>
> *Esse jogo de Ping Pong que vocês acabaram de construir — alguém criou antes de vocês. Mas os próximos que vierem vão usar o que vocês criaram como ponto de partida.*
>
> *A única regra é essa: **programem as histórias que vocês querem ver prontas**. O que você joga mas nunca encontrou? O que você precisaria de um app mas não existe? Esse é o próximo projeto.*
>
> *Obrigado."*

---

## 📎 MATERIAIS DE APOIO

| Item | Link/Arquivo |
|---|---|
| MIT App Inventor | https://ai2.appinventor.mit.edu |
| AI2 Companion (Android) | Play Store: "MIT AI2 Companion" |
| AI2 Companion (iOS) | App Store: "MIT AI2 Companion" |
| Arquivos do projeto | `Screen1.scm` + `Screen1.bky` |
| Assets necessários | `mountain_background.png`, `paddle.png`, `pong.mp3`, `note-do.mp3`, `single-knock.mp3` |

---

## 🗂 RESUMO DOS BLOCOS (COLA DO INSTRUTOR)

```
Eventos principais:
  START.Click          → inicializa jogo
  PAUSE.Click          → toggle Bola1.Enabled
  Pintura1.Dragged     → move Raquete no eixo X
  Bola1.CollidedWith   → rebate + ponto + som + dificuldade
  Bola1.EdgeReached    → GAME OVER (borda -1) ou Bounce (resto)

Procedimentos:
  AlterarEMostrarPontuacao(NovoPonto) → salva e exibe placar
  PlaySom(Arquivo)                    → toca som se checkbox ativo
  FlagDificuldade()                   → acelera bola a cada 2 pontos

Variável global:
  Pontos (Integer, inicia em 0)
```
