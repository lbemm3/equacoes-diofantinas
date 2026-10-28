
# Equações Diofantinas em ℤ[i]

Simulador interativo, em um único arquivo HTML, que resolve **equações diofantinas lineares** da forma

```
a·X + b·Y = c        com a, b, c ∈ ℤ[i]
```

sobre os **inteiros gaussianos** ℤ[i] = { x + yi : x, y ∈ ℤ }. A ferramenta calcula o MDC pelo algoritmo euclidiano, obtém as constantes de Bézout, decide a existência de solução, descreve a **solução geral** e ainda procura soluções em que X e Y são **inteiros ordinários** (parte imaginária nula). Todos os passos são exibidos, com verificações automáticas.

🔗 **Acesse online:** <https://lbemm3.github.io/equacoes-diofantinas/>

Os algoritmos seguem o artigo *Equações Diofantinas sobre Domínios Euclidianos* (REMAT, IFRS, 2021), originalmente implementados em planilhas LibreOffice (Algoritmos 6.1 a 6.4) e aqui portados para JavaScript puro:

📄 <https://periodicos.ifrs.edu.br/index.php/REMAT/article/view/4150>

---

## Sumário

- [Funcionalidades](#funcionalidades)
- [Como usar](#como-usar)
- [Abas do simulador](#abas-do-simulador)
- [Exemplos pré-carregados](#exemplos-pré-carregados)
- [Fundamentação matemática](#fundamentação-matemática)
- [Exemplo resolvido](#exemplo-resolvido)
- [Estrutura do código](#estrutura-do-código)
- [Exportação em PDF](#exportação-em-pdf)
- [Limitações](#limitações)
- [Executando localmente](#executando-localmente)
- [Publicação via GitHub Pages](#publicação-via-github-pages)
- [Referência](#referência)
- [Autoria e licença](#autoria-e-licença)

---

## Funcionalidades

- **Divisão euclidiana em ℤ[i]** com quociente, resto e verificação da condição δ(r) < δ(z₂) (Algoritmo 3.5).
- **MDC de dois inteiros gaussianos** pelo algoritmo de Euclides, com tabela completa das iterações (rₖ, qₖ, sₖ, tₖ).
- **Constantes de Bézout** s e t tais que a·s + b·t = mdc(a, b), com verificação.
- **Os 4 MDCs associados** (d·u, para u ∈ {1, −1, i, −i}) e as respectivas constantes de Bézout.
- **Existência de solução** da equação a·X + b·Y = c (condição mdc(a, b) | c).
- **Solução inicial** (x₀, y₀) e **solução geral** em função de um parâmetro z ∈ ℤ[i].
- **Calculadora de z**: escolha um z e veja os valores de X e Y correspondentes, com verificação.
- **Busca de solução inteira** (X, Y ∈ ℤ) resolvendo um sistema linear 2×2 nas partes real e imaginária de z.
- **Modo Explicado**: acrescenta caixas com a justificativa teórica de cada resultado, citando as proposições e teoremas do artigo.
- **Exportação em PDF** da resolução completa.
- **Exemplos prontos** (presets), incluindo os do artigo e um caso sem solução.
- **Sem dependências**: HTML + CSS + JavaScript puro, funciona offline e em celulares.

## Como usar

1. Abra o arquivo `diofantina_html.html` no navegador (ou acesse a [página publicada](https://lbemm3.github.io/equacoes-diofantinas/)).
2. Informe a parte real e a parte imaginária de cada número nos campos de entrada. Os resultados são recalculados a cada alteração.
3. Navegue pelas abas para ver o MDC, a equação, a divisão ou a metodologia.
4. Ative **📖 Modo Explicado** para ver a teoria por trás de cada passo.
5. Use **📥 Exportar PDF** para gerar um documento com a resolução.

## Abas do simulador

| Aba | O que faz |
|---|---|
| 🔢 **MDC + Bezout** | Entradas `a` e `b`. Mostra mdc(a, b), s, t, a tabela do algoritmo de Euclides e, opcionalmente, as 4 variantes associadas. |
| ⚡ **Equação Diofantina** | Entradas `a`, `b` e `c`. Decide se há solução, mostra (x₀, y₀), a solução geral, a calculadora para um z específico e a busca de solução inteira. |
| ➗ **Divisão em ℤ[i]** | Entradas `z₁` (dividendo) e `z₂` (divisor). Mostra o passo a passo: cálculo em ℚ[i], arredondamento, quociente, resto e verificações. |
| 📚 **Metodologia** | Resumo teórico: ℤ[i], divisão, Euclides, Bézout, existência, solução geral e solução inteira. |

Botões auxiliares:

- **▼/▲ Tabela**: mostra ou oculta a tabela das iterações do algoritmo de Euclides.
- **★ 4 Variantes**: mostra ou oculta a tabela dos quatro MDCs associados.

## Exemplos pré-carregados

| Botão | a | b | c | Observação |
|---|---|---|---|---|
| Ex. artigo 1 | 132 + 88i | 42 + 18i | 14 − 14i | Exemplo do artigo |
| Ex. artigo 2 | 75 + 41i | 9 + 15i | 7 + i | Exemplo do artigo |
| Ex. artigo 3 | 52 + 32i | 15 + 25i | 7 + i | Exemplo do artigo |
| Coprimos | 1250 + 885i | 720 + 256i | 5 | Coeficientes grandes |
| Sem solução | 6 | 4i | 3 | mdc = 2 não divide 3 |

## Fundamentação matemática

### 1. Inteiros gaussianos

ℤ[i] é um **domínio euclidiano** com a função norma

```
δ(x + yi) = x² + y²
```

Os elementos invertíveis são 𝒰(ℤ[i]) = {1, −1, i, −i}. Por isso o MDC **não é único**: se d = mdc(a, b), então d·u também é MDC para todo u ∈ 𝒰(ℤ[i]), com constantes de Bézout s·u e t·u.

### 2. Divisão euclidiana (Algoritmo 3.5)

Dados z₁ = x + yi e z₂ = a + bi ≠ 0:

```
α = (x·a + y·b) / (a² + b²)
β = (y·a − x·b) / (a² + b²)
m = round(α),  n = round(β)
q = m + n·i
r = z₁ − z₂·q
```

Como |m − α| ≤ 1/2 e |n − β| ≤ 1/2, vale δ(r) ≤ δ(z₂)/2 < δ(z₂).

### 3. Algoritmo de Euclides e Bézout

Com r₀ = a e r₁ = b, repete-se a divisão até obter resto zero:

```
r₀ = r₁·q₁ + r₂,      δ(r₂) < δ(r₁)
r₁ = r₂·q₂ + r₃,      δ(r₃) < δ(r₂)
   ⋮
rₙ₋₂ = rₙ₋₁·qₙ₋₁ + 0
mdc(a, b) = rₙ₋₁
```

As constantes de Bézout vêm das recorrências

```
s₀ = 1, s₁ = 0        t₀ = 0, t₁ = 1
sₖ = sₖ₋₂ + sₖ₋₁·(−qₖ₋₁)
tₖ = tₖ₋₂ + tₖ₋₁·(−qₖ₋₁)
```

com a invariante rₖ = a·sₖ + b·tₖ, o que garante a·s + b·t = mdc(a, b).

### 4. Equação a·X + b·Y = c

- **Existência (Proposição 4.1):** há solução em ℤ[i] se e somente se d = mdc(a, b) divide c.
- **Solução inicial:** x₀ = (c/d)·s e y₀ = (c/d)·t.
- **Solução geral (Teorema 4.10):** para todo z ∈ ℤ[i],

```
x = x₀ + (b/d)·z
y = y₀ − (a/d)·z
```

e toda solução tem essa forma.

### 5. Soluções inteiras (Exemplo 5.2)

Escreva b/d = β₁ + β₂i, a/d = α₁ + α₂i, x₀ = p + qi, y₀ = u + vi e z = z₁ + z₂i. Impor Im(x) = Im(y) = 0 conduz ao sistema

```
β₂·z₁ + β₁·z₂ = −q
α₂·z₁ + α₁·z₂ =  v
```

com determinante `det = β₂·α₁ − β₁·α₂`. Há solução inteira se e somente se o sistema admite (z₁, z₂) ∈ ℤ². Então

```
x = p + β₁·z₁ − β₂·z₂ ∈ ℤ
y = u − α₁·z₁ + α₂·z₂ ∈ ℤ
```

Se det = 0 o sistema é indeterminado e o simulador informa isso. Se x₀ e y₀ já são inteiros, a solução é trivial (z = 0).

## Exemplo resolvido

Equação (aba **Equação Diofantina**, preset *Ex. artigo 1*):

```
(132 + 88i)·X + (42 + 18i)·Y = 14 − 14i
```

**Algoritmo de Euclides:**

| k | rₖ | qₖ | sₖ | tₖ |
|---|---|---|---|---|
| 0 | 132+88i | — | 1 | 0 |
| 1 | 42+18i | 3+i | 0 | 1 |
| 2 | 24−8i | 1+i | 1 | −3−i |
| 3 | 10+2i | 2−i | −1−i | 3+4i |
| 4 | 2−2i | 2+3i | 4+i | −13−6i |
| 5 | 0 | — | — | — |

**Resultados:**

- mdc(a, b) = **2 − 2i**, com s = 4 + i e t = −13 − 6i.
- Como (2 − 2i) divide (14 − 14i), com c/d = 7, a equação **tem solução**.
- Solução inicial: x₀ = 28 + 7i e y₀ = −91 − 42i.
- Solução geral, com z ∈ ℤ[i]:

```
x = (28 + 7i) + (6 + 15i)·z
y = (−91 − 42i) − (11 + 55i)·z
```

- Neste caso o sistema 2×2 não fornece z₁, z₂ inteiros, então **não existe solução com X, Y ∈ ℤ**.

**Caso sem solução** (preset *Sem solução*): para 6·X + 4i·Y = 3 obtém-se mdc = 2, que não divide 3, logo não há solução em ℤ[i].

## Estrutura do código

Todo o projeto está em `diofantina_html.html`, organizado em seções comentadas:

| Seção | Conteúdo |
|---|---|
| **MATEMÁTICA** | Aritmética complexa inteira (`C`, `cadd`, `csub`, `cmul`, `cneg`, `cn2`, `ceq`), formatação (`cstr`), divisão euclidiana (`gdiv`), divisão exata (`gdivExact`), `euclidBezout`, `solveEq`, `fourVariants` e `findIntSol`. |
| **ESTADO** | Objeto `S` com entradas, aba ativa e opções de exibição; lista `PRESETS`. |
| **HELPERS** | Componentes de interface em HTML (`chip`, `vbox`, `expl`, `buildTbl`, `buildInput`, `buildVariants`, `buildIntSol`). |
| **RENDER** | `render`, `renderCalc` (abas 0 e 1), `renderDiv` (aba 2) e `renderMet` (aba 3). |
| **EXPORT PDF** | `exportPDF`, que monta um documento imprimível. |
| **EVENTOS** | Tratadores de entrada, troca de aba, modo explicado e criação dos presets. |

Cada número complexo é representado por um objeto `{ re, im }`. A divisão usa `Math.round` sobre as coordenadas de z₁/z₂ e o algoritmo de Euclides tem um limite de segurança de 60 iterações.

## Exportação em PDF

O botão **📥 Exportar PDF** abre uma nova janela com a resolução formatada (dados, tabela de Euclides, MDC, constantes de Bézout, quatro MDCs associados e a análise da equação) e aciona a impressão do navegador. Escolha **Salvar como PDF** como destino. Se nada acontecer, verifique se o navegador está bloqueando pop-ups. O PDF reflete as entradas `a`, `b` e `c` atuais.

## Limitações

- Os cálculos usam números de ponto flutuante do JavaScript. Com coeficientes muito grandes (ordem de 10⁷ ou mais nas partes real e imaginária, pois as normas chegam a ~10¹⁴), podem ocorrer perdas de precisão.
- Os campos aceitam apenas números inteiros. Valores vazios ou inválidos são tratados como 0.
- O caso a = b = 0 não é tratado como equação válida.
- A exportação em PDF depende da janela de impressão do navegador.

## Executando localmente

Não há instalação nem build. Basta baixar o arquivo e abri-lo:

```bash
git clone https://github.com/lbemm3/equacoes-diofantinas.git
cd equacoes-diofantinas
# abra o arquivo no navegador:
open diofantina_html.html        # macOS
xdg-open diofantina_html.html    # Linux
start diofantina_html.html       # Windows
```

Ou, se preferir um servidor local:

```bash
python3 -m http.server 8000
# acesse http://localhost:8000/diofantina_html.html
```

## Publicação via GitHub Pages

1. No repositório, vá em **Settings → Pages**.
2. Em *Build and deployment*, selecione **Deploy from a branch** e escolha a branch principal e a pasta `/ (root)`.
3. Para a página abrir direto na raiz do endereço, renomeie (ou copie) o arquivo para `index.html`.
4. O simulador ficará disponível em <https://lbemm3.github.io/equacoes-diofantinas/>.

## Referência

> *Equações Diofantinas sobre Domínios Euclidianos.* REMAT: Revista Eletrônica da Matemática, IFRS, 2021.
> <https://periodicos.ifrs.edu.br/index.php/REMAT/article/view/4150>

Numeração citada no simulador: Observações 2.8 e 2.9, Algoritmo 3.5, Seção 3 (Bézout), Proposição 4.1, Teorema 4.10, Exemplo 5.2 e Algoritmos 6.1 a 6.4.

## Autoria e licença

Desenvolvido por **Laerte Bemm** (Departamento de Matemática, Universidade Estadual de Maringá – UEM), com base nos algoritmos do artigo citado.

Licença: defina a licença do repositório (por exemplo, MIT) e inclua o arquivo `LICENSE`.
