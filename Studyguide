# Taller 3 Oral Defense Study Guide

This guide covers the notebook `Taller3_201731032_201631349_201016705.ipynb` (ECON 64597, Easley & Kleinberg chapters 13–15).

I ran the whole notebook. **All 8 `assert` cells print `OK`.** Every number quoted below comes from that run.

Contents:

0. [Read this first: 3 mistakes in your written answers + 1 packaging issue](#0-read-this-first)
1. [Glossary (English / Spanish terms used in the notebook)](#1-glossary)
2. [Section-by-section plain-language explanation](#2-section-by-section-explanation)
3. [Likely TA questions with answers](#3-likely-ta-questions-with-answers)
4. ["Change the code live" questions](#4-change-the-code-live-questions)
5. [Cheat sheet of numbers to memorize](#5-cheat-sheet)

---

## 0. Read this first

An AI code reviewer will almost certainly catch these. Know the correct version before you walk in.

### ❌ Mistake 1: Interpretation 1.2, question 3 (adding edge G → A) is incomplete

Your answer says only F and G move into the CFC. I ran `estructura_bowtie` with the edge added:

| Region | Before | After adding G → A |
|---|---|---|
| CFC | A, B, C | **A, B, C, F, G** |
| IN | D, E | **D, E, H, J** |
| OUT | F, G | **(empty)** |
| TUBOS | H | **(empty)** |
| TENDRILES | I, J | **I** |
| DESCONECTADOS | K, L | K, L |

**Why H and J change:** H → F → G → A, so H can now reach the core. The core still cannot reach H, so H is now in **IN** and is no longer a tube. J → F → G → A works the same way: J was a tendril and is now in IN. Once OUT is empty there are no tubes, because a tube needs an OUT to connect to.

If asked, say: *"We only mentioned F and G, but H and J also move to IN because they can now reach the core through G. OUT becomes empty."*

### ❌ Mistake 2: Interpretation 2.2, question 2 has IN and OUT backwards

Your answer says {A, B} is the CFC and C and D "are in the OUT part." **That is reversed.** C and D link *into* {A, B}, and {A, B} never links back to them. So:

- **{C, D} is upstream, which makes it IN.** It reaches the pair, but the pair can't reach it.
- **{A, B} is downstream, which makes it OUT** relative to the rest. The notebook's own theory cell says this: *"un pedazo de la región OUT se queda con todo el prestigio"* (a piece of OUT keeps all the prestige).

If you run `estructura_bowtie(RED_SUMIDERO)`, you get `CFC = {A, B}` and `IN = {C, D}`. Both SCCs have size 2 and the function picks {A, B}. Either way, C and D are IN, not OUT.

Correct version: *"{A, B} is a set with no exit, a closed piece of the OUT region. Rank flows into it from {C, D} (IN) and never comes back. That one-way flow is exactly what makes it a trap."*

### ⚠️ Mistake 3: Interpretation 1.2, question 1 gives one number for all of IN

"From IN you reach 9/12" is true **only if you start at D**. Starting at E you reach 8/12 (E, A, B, C, F, G, H, I). From OUT: starting at F gives 2/12 (F, G), and starting at G gives 1/12 (only G). Be ready to say: *"It depends on the starting node: D reaches 9, E reaches 8, F reaches 2, G reaches 1."*

### ⚠️ Packaging: `requirements.txt` must include `scipy`

`nx.hits` and `nx.pagerank` in the check cells need `scipy`. Without it, cells 2.1 and 2.3 crash with `ModuleNotFoundError: No module named 'scipy'`. I reproduced that crash. Your `requirements.txt` should contain at least:

```
networkx
numpy
scipy
```

### ⚠️ The AI declaration

Your `DECLARACION_IA` says you wrote the code yourselves and used AI only to point out errors. It ends with *"we can explain, modify and defend all the code."* The instructions say *"No sé, lo escribió la IA"* ("I don't know, the AI wrote it") counts as an incomplete submission. The TA may ask what you asked the AI and what you changed yourselves. Answer honestly and consistently with the declaration. If the declaration doesn't match what actually happened, it is better to say so yourself than to have it come out under questioning.

### ℹ️ Where do the numbers in Part 4(b) come from?

Your answer quotes HITS authority scores on `RED_PR` (D ≈ 0.1562, B ≈ 0.0965). Those values are **not printed anywhere in the notebook.** They are correct: I ran `hits(RED_PR)` and got exactly those. Be ready to say *"we ran `hits(RED_PR)` separately."* Full output:
- HITS authorities on RED_PR: A 0.4618, C 0.2854, **D 0.1562, B 0.0965**
- Scaled PageRank on RED_PR: A 0.3845, C 0.2646, **B 0.2009, D 0.1500**

---

## 1. Glossary

| English | Notebook (Spanish) | Meaning |
|---|---|---|
| directed graph | grafo dirigido | Edges have a direction: u → v means "u links to v" |
| reachable set | `alcanzables`, Out(v) | Everything you can reach from v by following arrows forward |
| reaching set | `alcanzan_a`, In(v) | Everything that can reach v |
| strongly connected component (SCC) | componente fuertemente conexa (CFC) | A maximal group where every node reaches every other |
| reverse graph | grafo reverso Gᴿ | Same graph with every arrow flipped |
| condensation | condensación | Shrink each SCC to one dot. The result has no cycles (it's a DAG) |
| bow-tie | moño | Web structure: IN → core → OUT, plus tubes, tendrils and islands |
| tube / tendril | tubo / tendril | Tube: IN → … → OUT without touching the core. Tendril: hangs off IN or OUT |
| weakly connected | componente débil / no dirigido | Connected if you ignore arrow directions |
| hub / authority | hub / autoridad | Hub: a good list of links. Authority: a good destination |
| fixed point | punto fijo | A vector that doesn't change when you apply the update again |
| power method | método de la potencia | Repeatedly multiply by a matrix and normalize; converges to the dominant eigenvector |
| rank sink / trap | trampa de rango / sumidero | A group of pages with links in and no links out, which absorbs all PageRank |
| scaling / damping factor | factor de escala s | Probability of following a link instead of teleporting |
| clickthrough rate | tasa de clic rᵢ | Clicks per unit of time that slot i gets |
| greedy | codicioso | Sort and assign the best to the best |
| brute force | fuerza bruta | Try every permutation |
| externality | externalidad | The harm your presence causes to others |
| dominant strategy | estrategia dominante | Best for you no matter what others do |
| Nash equilibrium | equilibrio de Nash | Nobody gains by changing only their own action |
| GSP | subasta generalizada de segundo precio | Pay the next-highest bid per click |

---

## 2. Section-by-section explanation

### Part 0: Setup (cell 4)

**Code:** Imports `itertools` (permutations), `numpy` (random test instances), and `networkx` (graph objects). It defines `ok(msg)`, which just prints "OK - ...".

**Rule to remember:** You may use networkx to *store* the graph and to ask for neighbors (`G.successors`, `G.predecessors`, `G.out_degree`). You may **not** use the functions that solve the exercise (`strongly_connected_components`, `hits`, `pagerank`) inside your implementations. They appear only in the check cells as a reference to compare against. The one allowed exception: in 1.2 you may use `nx.strongly_connected_components` to *pick* the largest SCC.

---

### Part 1.0: The Web as a directed graph (cell 6)

**Theory in plain words.** A hyperlink goes one way: my page linking to yours doesn't make yours link to mine. So "I can reach you" and "you can reach me" are different questions.

- **Out(v):** every page you can reach from v by clicking links.
- **In(v):** every page from which you can eventually click your way to v.
- **SCC of v = Out(v) ∩ In(v):** the pages you can go to *and* come back from. That is exactly "mutually reachable."
- SCCs **split the graph into non-overlapping pieces** (a partition). "Mutually reachable" is an equivalence relation: you reach yourself; if I reach you and you reach me, that's symmetric; and it chains (transitive).
- If you shrink each SCC to one dot, the resulting graph has **no cycles** (a DAG). If two blobs X and Y were in a cycle, everyone in X ∪ Y would reach everyone else, so they would have been a single SCC.

**Bow-tie (Broder et al., 2000):** The Web has one giant SCC (the core, about 1/4 of pages). IN pages lead into it, OUT pages are reached from it, tubes go from IN to OUT while skipping the core, tendrils hang off IN or OUT, and islands are disconnected.

**Code (cell 6):** Builds the example web `WEB` with 12 nodes and 12 edges:

```
D → E → [A → B → C → A]  → F → G         core = {A,B,C}
    E → H → F                             H = tube
    E → I                                 I = tendril out of IN
            J → F                         J = tendril into OUT
K → L                                     island
```

---

### Exercise 1.1: Reachability and SCC (cells 8–9)

```python
def alcanzables(G, s):
    visitados = {s}          # nodes we've seen (a set, for fast lookup)
    pendientes = [s]         # "to-do" list used as a stack
    while pendientes:
        actual = pendientes.pop()                 # take the LAST one → depth-first (DFS)
        for vecino in G.successors(actual):       # every node that `actual` links to
            if vecino not in visitados:
                visitados.add(vecino)             # mark it when we first see it...
                pendientes.append(vecino)         # ...and schedule it for exploration
    return visitados
```

**Plain language:** Start at s. Keep a list of pages still to explore. Take one, look at every link on it, and for every page you haven't seen yet, mark it as seen and add it to the list. When the list is empty, the "seen" set is everything reachable. This is a **depth-first search (DFS)** because `.pop()` takes the most recently added item (a stack). Using `.pop(0)` would make it breadth-first (BFS) with the same final set.

```python
def alcanzan_a(G, s):
    reverso = G.reverse(copy=True)     # flip every arrow
    return alcanzables(reverso, s)     # "who reaches s" = "whom s reaches" in the flipped graph
```

**Plain language:** To find who can reach s, flip all arrows and ask whom s can reach. That reuses the same search, so there is no need to write a second backwards search.

```python
def cfc_de(G, s):
    return alcanzables(G, s) & alcanzan_a(G, s)   # set intersection
```

**Plain language:** The SCC of s is the set of nodes that s reaches *and* that reach s.

**Printed output to know:**

| node | \|Out\| | \|In\| | SCC |
|---|---|---|---|
| A | 5 (A,B,C,F,G) | 5 (A,B,C,D,E) | {A,B,C} |
| D | 9 | 1 | {D} |
| F | 2 | 8 | {F} |
| H | 3 | 3 | {H} |
| K | 2 | 1 | {K} |

**What the checks test (cell 9):** Correct sets for A and K. The SCCs match networkx and don't depend on which member you ask from. A node with no cycle (H) is its own SCC. The SCCs form a partition. A random 45-node graph gives the same SCCs as networkx.

---

### Exercise 1.2: Bow-tie structure (cells 11–12)

```python
def estructura_bowtie(G):
    cfc = max(nx.strongly_connected_components(G), key=len)   # the largest SCC = core
    r = next(iter(cfc))                                        # any node of the core
    entrada = alcanzan_a(G, r) - cfc                           # IN  = In(r)  minus core
    salida  = alcanzables(G, r) - cfc                          # OUT = Out(r) minus core
    resto = set(G) - cfc - entrada - salida                    # everything not yet classified
    conectados = nx.node_connected_component(G.to_undirected(), r)   # ignore directions
    desconectados = set(G) - conectados                        # islands
    tubos = set()
    for v in resto:
        viene_de_in = alcanzan_a(G, v) & entrada               # can some IN node reach v?
        llega_a_out = alcanzables(G, v) & salida               # can v reach some OUT node?
        if viene_de_in and llega_a_out:                        # (a non-empty set counts as True)
            tubos.add(v)
    tendriles = resto - tubos - desconectados                  # whatever is left
    return {...six regions...}
```

**Plain language:**
1. Find the biggest SCC. That's the core.
2. Pick any core node r (it doesn't matter which, because all core nodes reach each other and so have the same In and Out).
3. IN = things that reach r, minus the core. OUT = things r reaches, minus the core.
4. For the leftover nodes: if something from IN reaches it *and* it reaches something in OUT, it's a **tube**.
5. Treat all edges as two-way. Anything not connected to r even then is **disconnected**.
6. Everything else left over is a **tendril**.

**Output:** CFC {A,B,C}, IN {D,E}, OUT {F,G}, TUBOS {H}, TENDRILES {I,J}, DESCONECTADOS {K,L}.

**Trick used (from the hint):** "Reachable from *some* IN node" is checked with one reverse search from v, intersected with IN. You don't run one search per IN node.

**What the checks test (cell 12):** Each region is correct. The regions form a partition (they cover all nodes with no overlaps). The result doesn't depend on which r is chosen. Nothing in OUT goes back to the core, and the core reaches nothing in IN.

---

### Part 2.0: Ranking theory (cell 15)

**Idea:** A link from u to v is a *vote* by u for v, but votes from important pages should count more. That sounds circular ("good pages are those linked by good pages"), but it just defines a **fixed point**: a set of scores that stays the same when you apply the rule again.

**HITS (hubs and authorities):**
- authority(v) = sum of hub scores of pages that link **to** v
- hub(v) = sum of authority scores of pages that v links **to**
- Start everything at 1, repeat k times, and divide each vector by its sum every round (otherwise the numbers blow up). Normalizing doesn't change the ranking.
- In matrix terms this is the **power method** on MᵀM (authorities) and MMᵀ (hubs). It converges to their top eigenvector.

**Basic PageRank:**
- Everyone starts with 1/n. Each round, every page gives away **all** its rank, split equally among the pages it links to.
- r_new(v) = Σ over u→v of r(u) / outdegree(u)
- The total stays 1 (a "stochastic matrix"), as long as every page has at least one outgoing link.
- **Problem:** a group that links only to itself (a sink) absorbs all rank over time.

**Scaled PageRank:**
- r_new(v) = (1−s)/n + s · Σ r(u)/outdeg(u), with s = 0.85
- **Random surfer:** with probability 0.85 click a random link, with probability 0.15 jump to a random page. The PageRank is the long-run fraction of time spent on each page (the stationary distribution).
- Every page gets at least (1−s)/n, so sinks can't take everything. By Perron–Frobenius the answer **exists, is unique, and doesn't depend on the starting vector.**

**Data (cell 15):**
- `RED_HITS`: hubs h1, h2, h3 point to authorities a1, a2, a3. There is also a1 → a2.
- `RED_PR`: 4 nodes, strongly connected (everyone reaches everyone).
- `RED_SUMIDERO`: A ⇄ B is a trap. C links to A and D, and D links to C. Nothing leaves {A, B}.

---

### Exercise 2.1: HITS (cells 17–18)

```python
def hits(G, k=50):
    hubs = {v: 1.0 for v in G}
    autoridades = {v: 1.0 for v in G}
    for ronda in range(k):
        nuevas_autoridades = {v: sum(hubs[u] for u in G.predecessors(v)) for v in G}     # who links TO v
        nuevos_hubs = {v: sum(nuevas_autoridades[w] for w in G.successors(v)) for v in G} # whom v links to (uses NEW authorities)
        # divide each by its total so both sum to 1
        autoridades = {v: nuevas_autoridades[v] / total_autoridades ...}
        hubs        = {v: nuevos_hubs[v] / total_hubs ...}
    return hubs, autoridades
```

(Simplified above. The real code uses explicit loops, but the logic is the same.)

**Plain language:** Each round, (1) a page's authority = total hub score of pages pointing at it; (2) a page's hub score = total *new* authority of the pages it points at; (3) rescale both so they sum to 1. The code builds a **new dictionary** before replacing the old one, so every node is updated using the same round's values.

**Output:**
- authorities: a2 0.5, a1 0.3229, a3 0.1771, h1/h2/h3 **0**
- hubs: h1 0.3542, h2 0.2915, h3 0.1771, a1 0.1771, a2/a3 **0**

**Why these make sense:**
- a2 is the best authority because *everyone* links to it (h1, h2, h3, a1).
- h1 is the best hub because it links to all three authorities.
- h1, h2, h3 have authority 0 because nobody links to them (empty sum).
- a1 has a hub score (0.1771) because it links to a2. That link gives a1 its "mixed role."

**Checks:** Both vectors sum to 1. The ranking is right. The result matches `nx.hits`. Running 200 rounds instead of 50 gives the same result, so it has converged.

---

### Exercise 2.2: Basic PageRank and rank traps (cells 20–21)

```python
def pagerank_basico(G, k=100, r0=None):
    n = G.number_of_nodes()
    rangos = {v: 1/n for v in G} if r0 is None else r0.copy()   # uniform start, or the given one
    for ronda in range(k):
        nuevos_rangos = {v: sum(rangos[u] / G.out_degree(u) for u in G.predecessors(v)) for v in G}
        rangos = nuevos_rangos
    return rangos
```

**Plain language:** Each page splits its current rank equally among its out-links. Your new rank is the total you receive. There is no normalization because nothing is created or lost (every node here has an out-link). `r0.copy()` avoids modifying the caller's dictionary.

**Output:**
- RED_PR: A 0.4, C 0.2667, B 0.2, D 0.1333 (exactly 2/5, 4/15, 1/5, 2/15)
- RED_SUMIDERO: A 0.5, B 0.5, C 0, D 0 → **the trap took everything**

**Check the fixed point by hand (RED_PR):** A has out-degree 2 (→B, →C), B has 1 (→A), C has 2 (→A, →D), D has 2 (→C, →A).
- A = B/1 + C/2 + D/2 = 0.2 + 0.1333 + 0.0667 = **0.4** ✓
- B = A/2 = **0.2** ✓
- C = A/2 + D/2 = 0.2 + 0.0667 = **0.2667** ✓
- D = C/2 = **0.1333** ✓

**Watching the trap drain (RED_SUMIDERO, by round):**

| round | A | B | C | D |
|---|---|---|---|---|
| 0 | .25 | .25 | .25 | .25 |
| 1 | .375 | .25 | .25 | .125 |
| 2 | .375 | .375 | .125 | .125 |
| 4 | .4375 | .4375 | .0625 | .0625 |
| 10 | .4922 | .4922 | .0078 | .0078 |

Each round C sends half its rank into A. That rank never comes back, so the {C, D} pair loses half its mass every two rounds and decays to zero.

---

### Exercise 2.3: Scaled PageRank (cells 24–25)

The code is the same as 2.2 except for one line:

```python
nuevos_rangos[v] = (1 - s) / n + s * aporte_enlaces
```

**Plain language:** Each page gets a guaranteed "teleport" share (1−s)/n = 0.0375 for n=4, plus 85% of what flows in through links.

**Output:**
- RED_SUMIDERO, s=0.85: A 0.4163, B 0.3914, C 0.1086, D 0.0837 → C and D now keep positive rank
- RED_PR, s=0.85: A 0.3845, C 0.2646, B 0.2009, D 0.1500 (same ranking as basic)
- RED_SUMIDERO, s=0.999: A 0.4993, B 0.499, C 0.001, D 0.0007 → as s → 1 the trap comes back

**Why A > B in the trap:** B gets rank only from A. A gets rank from B *and* from C.

**Checks:** The result sums to 1. It matches `nx.pagerank(alpha=0.85)`. Starting from a random vector gives the same answer (uniqueness). s = 0.999 almost recreates the trap.

---

### Part 3.0: Sponsored search market (cell 27)

**Setup:** 3 ad slots with click rates `TASAS = [10, 5, 2]`. Three advertisers value a click at x = 7, y = 6, z = 1. If advertiser j gets slot i, the total value is rᵢ × vⱼ.

**Why the best assignment is just sorting (rearrangement inequality):** If you put the lower-value advertiser in the better slot, swapping them gains (r₁ − r₂)(vₓ − vᵧ) > 0. So sort both lists and match them. Here that's x→1, y→2, z→3, with value 70 + 30 + 2 = **102**. Swapping x and y gives 97 = 102 − (10−5)(7−6).

**The real question is prices.** Values are private, and advertisers will lie if lying pays.

**VCG:** Assign efficiently, and charge each advertiser the **harm they cause to everyone else**: (best the others could do without you) − (what the others actually get with you). Under VCG, **telling the truth is a dominant strategy.**

**GSP (what Google actually used):** Sort by bid. The i-th highest bidder gets slot i and pays the **next** bid per click. It's simple, but **not truthful** when there are 2 or more slots.

`TASAS_GSP = [10, 4, 0]`: the third slot gets zero clicks, which is the same as not being shown.

---

### Exercise 3.1: Optimal assignment, brute force vs greedy (cells 29–31)

`valor_total` (given): Σ rᵢ × v of the advertiser in slot i, skipping empty slots (`None`).

```python
def asignacion_optima_anuncios(tasas, valores):
    anunciantes = list(valores); m = len(tasas); n = len(anunciantes)
    if m == 0: return 0, []
    if n >= m:
        candidatas = itertools.permutations(anunciantes, m)          # every ordered choice of m advertisers
    else:
        candidatas = itertools.permutations(anunciantes + [None]*(m-n), m)  # pad with "empty slot"
    # keep the candidate with the highest valor_total
```

**Plain language:** Try every way to fill the slots and keep the best. If there are fewer advertisers than slots, add `None` entries ("empty") so every slot has something. `permutations(list, m)` gives every ordered selection of m items.

```python
def asignacion_codiciosa(tasas, valores):
    ordenados = sorted(valores, key=lambda j: (-valores[j], str(j)))   # highest value first; tie → alphabetical
    asignacion = ordenados[:len(tasas)]                                # the top m get slots
    asignacion += [None] * (len(tasas) - len(asignacion))              # pad empty slots
    return asignacion
```

**Plain language:** Sort advertisers from highest to lowest value and give them slots in order. The minus sign sorts descending. The name is only a tie-breaker so the result is always the same.

**Checks:** The optimum is [x, y, z] = 102 and greedy gives the same. Swapping costs exactly (r₁−r₂)(vₓ−vᵧ). With more slots than advertisers you get [a, None, None]. In 200 random instances, greedy and brute force give the **same value**. They compare values, not assignments, because ties can produce different assignments with equal value.

---

### Exercise 3.2: VCG prices and truthfulness (cells 33–34)

```python
def precios_vcg(tasas, valores):
    _, asignacion = asignacion_optima_anuncios(tasas, valores)
    for j in valores:
        if j not in asignacion: pagos[j] = 0; continue           # no slot, no harm
        valores_sin_j = {k: v for k, v in valores.items() if k != j}
        bienestar_sin_j, _ = asignacion_optima_anuncios(tasas, valores_sin_j)   # others' best without j
        bienestar_otros_con_j = sum(tasas[i]*valores[k] for i, k in enumerate(asignacion)
                                    if k is not None and k != j)               # others' value with j
        pagos[j] = bienestar_sin_j - bienestar_otros_con_j                     # harm caused
```

**Plain language:** For each advertiser: re-solve the market as if they didn't exist and see how well the others would do. Subtract how well the others actually do with them present. That difference is the payment.

**Worked numbers (TASAS [10,5,2]):**

| | Others without j | Others with j | Payment | Per click | Net gain (value − payment) |
|---|---|---|---|---|---|
| x (slot 1) | y@10 + z@5 = 60+5 = 65 | y@5 + z@2 = 30+2 = 32 | **33** | 3.3 | 70 − 33 = **37** |
| y (slot 2) | x@10 + z@5 = 70+5 = 75 | x@10 + z@2 = 70+2 = 72 | **3** | 0.6 | 30 − 3 = **27** |
| z (slot 3) | x@10 + y@5 = 100 | 100 | **0** | 0 | 2 |

Search engine revenue = 33 + 3 + 0 = **36**.

**Shortcut formula (good to show off):** Each advertiser pays, for every slot below them, the click loss caused by pushing that slot's occupant down one position:
- p_x = (10−5)·6 + (5−2)·1 = 30 + 3 = 33 ✓
- p_y = (5−2)·1 = 3 ✓

**Truthfulness check (cell 34):** `pago_neto(j, reporte, ...)` lets j report a fake value (0 to 15). It runs the mechanism with that report, but computes j's gain with the **true** value. No false report ever beats telling the truth.

---

### Exercise 3.3: GSP (cells 36–38)

```python
def resultado_gsp(pujas, tasas):
    orden = sorted(pujas, key=lambda j: (-pujas[j], str(j)))     # highest bid first
    for i, j in enumerate(orden[:len(tasas)]):                    # the top m get slots 0..m-1
        asignacion[j] = i
        precio_por_clic[j] = pujas[orden[i+1]] if i+1 < len(orden) else 0   # pay the NEXT bid
```

**Plain language:** Rank by bid. Slot i goes to the i-th bidder, who pays per click whatever the bidder right below them bid. If nobody is below, the price is 0.

```python
def es_equilibrio_gsp(pujas, tasas, valores, rejilla):
    pagos_actuales = pagos_gsp(pujas, tasas, valores)
    for j in pujas:
        for nueva_puja in rejilla:                         # try every alternative bid on the grid
            desviacion = dict(pujas); desviacion[j] = nueva_puja   # change ONLY j's bid
            if pagos_gsp(desviacion, ...)[j] > pagos_actuales[j] + 1e-9:
                return False                               # j has a profitable deviation, so this is not Nash
    return True
```

**Plain language:** A bid profile is a Nash equilibrium if no single advertiser can gain by changing only their own bid. The function tries every alternative bid on the grid (0, 0.5, …, 8) for every advertiser. The `1e-9` tolerance guards against floating-point noise.

**Given functions:** `pagos_gsp` gives each advertiser's net gain = clicks × (true value − price per click). `ingreso_gsp` gives the search engine's revenue = Σ clicks × price.

**Results with TASAS_GSP [10, 4, 0]:**
- Everyone bids their true value (7, 6, 1): x gets slot 1 and pays 6, so it gains 10·(7−6) = **10**. y gets slot 2 and pays 1, so it gains 4·(6−1) = **20**. Revenue **64**.
- x lowers its bid to 5: now y is first and x is second. x pays 1 and gains 4·(7−1) = **24**, which is more than 10. **Truth-telling is not an equilibrium.**
- With one slot, GSP = a Vickrey (second-price) auction, and truth-telling is an equilibrium.
- Searching all bid profiles with bids ≤ value on the grid finds **222 equilibria**, with revenue between **10 and 49**.
- VCG with these rates charges x 40 (4/click) and y 4 (1/click). Revenue **44**.
- The profile **(7, 4, 1)** is an equilibrium with exactly the VCG prices: x pays y's bid of 4, and y pays z's bid of 1. Revenue 44, same efficient assignment.

---

### Part 4: Discussion (your answer)

- (a) The trap = an absorbing region (part of OUT). Basic: A = B = 0.5, C = D = 0. Scaled: 0.4163 / 0.3914 / 0.1086 / 0.0837.
- (b) HITS gives two scores per page (hub and authority). PageRank gives one score, which is split by out-degree. On RED_PR, HITS authority puts D (0.1562) above B (0.0965), while PageRank puts B (0.2009) above D (0.1500).
- (c) Keep organic results and ads separate. Organic ranking is a signal about information, and ads are a market for attention. If money bought organic positions, the ranking would stop measuring link-based authority.

---

## 3. Likely TA questions with answers

### Part 1: Structure

**Q1. What algorithm does `alcanzables` implement? What is its complexity?**
Iterative depth-first search (a stack via `.pop()`). Each node is added to `pendientes` at most once, because we mark it visited when we add it, and each edge is looked at once. That's **O(V + E)**.

**Q2. Why iterative and not recursive?**
Python's default recursion limit is about 1000. A recursive DFS on a long chain would crash with `RecursionError`. A stack plus a visited set avoids that.

**Q3. If you change `pop()` to `pop(0)`, what changes?**
It becomes BFS (a queue). The **set** returned is identical, because reachability doesn't depend on visit order. Only the exploration order changes. (`pop(0)` on a list is O(n), so a real BFS would use `collections.deque`.)

**Q4. Why do you mark nodes visited when you push them, not when you pop them?**
So a node is never added to the stack twice. Marking on pop would still give the right answer but could push duplicates.

**Q5. Why does `alcanzan_a` reverse the graph?**
u reaches s in G exactly when s reaches u in the reversed graph. So one search routine serves both purposes.

**Q6. What does `copy=True` do, and is there a cost?**
It builds a new graph with flipped edges and leaves G unchanged. It costs O(V + E) time and memory on **every call**. An alternative with no copy is a search that follows `G.predecessors` instead of `G.successors`.

**Q7. Prove that Out(v) ∩ In(v) is the SCC of v.**
w ∈ Out(v) means v reaches w. w ∈ In(v) means w reaches v. Both together mean they are mutually reachable, which is the definition of being in the same SCC.

**Q8. Why do SCCs form a partition?**
"Mutually reachable" is reflexive (the empty path), symmetric (by definition), and transitive (join the paths). That makes it an equivalence relation, and equivalence classes are always disjoint and cover every node.

**Q9. Why is the condensation always a DAG (no cycles)?**
If two different SCCs X and Y were on a cycle, every node in X would reach every node in Y and back. Then X ∪ Y would be strongly connected, which contradicts X and Y being *maximal*.

**Q10. Why is H its own SCC even though it's in the middle of the graph?**
H is not on any cycle. H reaches F and G, but neither F nor G reaches H. A node on no cycle is an SCC of size 1.

**Q11. In `estructura_bowtie`, why can you pick any node r of the core?**
All core nodes reach each other, so they have identical Out and In sets. The check cell confirms this by looping over every r in the core.

**Q12. What if two SCCs tie for largest?**
`max(..., key=len)` returns the first one networkx lists, which is effectively arbitrary. The bow-tie is then built around that one. RED_SUMIDERO is an example: {A, B} and {C, D} both have size 2. The Web has one clear giant component, so this doesn't matter there.

**Q13. Why use `to_undirected()` for disconnected nodes?**
Tendrils are attached to the structure even though no directed path links them to the core. For example, E → I: I can't reach the core and the core can't reach I, but I still hangs off E. Only an undirected check tells "attached somehow" (tendril) apart from "completely separate" (island).

**Q14. How does the code decide that H is a tube and I isn't?**
For H: `alcanzan_a(H) = {H, E, D}` intersects IN = {D, E}, and `alcanzables(H) = {H, F, G}` intersects OUT = {F, G}. Both are non-empty, so H is a tube. For I: E (in IN) reaches I, but I reaches nothing in OUT, so I is a tendril.

**Q15. `if viene_de_in and llega_a_out:` is used on sets. How does that work?**
In Python an empty set counts as `False` and a non-empty set as `True`. So this means "both intersections are non-empty."

**Q16. Could a disconnected node be classified as a tube by mistake?**
No. A tube must be reachable from IN, and IN is weakly connected to r, so a tube is always in r's undirected component. That's also why `tendriles = resto - tubos - desconectados` gives disjoint regions.

**Q17. What is the complexity of `estructura_bowtie`? Can it be improved?**
For every leftover node it runs two searches, and one of them also copies the reversed graph. That is **O(V · (V + E))**. Better: run **one** search forward from all IN nodes at once, and **one** backward from all OUT nodes at once. Tubes = leftover ∩ both results. That's O(V + E).

**Q18. What happens if you add edge G → A?** See [Mistake 1](#-mistake-1-interpretation-12-question-3-adding-edge-g--a-is-incomplete). The core becomes {A, B, C, F, G}, H and J move to IN, OUT and TUBOS become empty, and I is still a tendril.

**Q19. What is the main conclusion of chapter 13?**
The Web is **not** "navigable" in the naive sense. With a core of only about 1/4 of pages, from most starting points you can't reach most of the Web. What you can reach depends on whether you start in IN, the core, or OUT. In our graph D reaches 9/12 but G reaches only 1/12.

### Part 2: Ranking

**Q20. Explain HITS in one sentence.**
A good authority is pointed to by good hubs, and a good hub points to good authorities. We alternate these two updates and normalize until they stabilize.

**Q21. Why normalize? Does it change the ranking?**
Without it the values grow without bound. Dividing every entry by the same number doesn't change the order.

**Q22. Why create `nuevas_autoridades` instead of updating `autoridades` in place?**
In place, some nodes would be computed with values from this round and others with values from the previous round. Building a fresh dictionary means everyone uses the same round.

**Q23. Why does the hub update use `nuevas_autoridades` and not the old ones?**
That's the procedure in [EK] §14.2: authority update first, then hub update with the **just-updated** authorities.

**Q24. What is HITS computing mathematically?**
With adjacency matrix M, one round gives a ← MᵀM a and h ← MMᵀ h. Repeating and normalizing is the **power method**, which converges to the dominant eigenvector of MᵀM (authorities) and MMᵀ (hubs).

**Q25. Why do h1, h2, h3 have authority exactly 0?**
Nobody links to them, so the sum over predecessors is empty and equals 0.

**Q26. Why is a2 a better authority than a1 even though both are linked by h1 and h2?**
a2 is also linked by h3 and by a1. More good hubs point at it.

**Q27. How do you know k=50 is enough?**
The check runs k=200 and gets the same values (difference < 1e-9). The iteration has converged.

**Q28. What could make `hits` crash?**
A graph with **no edges**: all sums are 0, so `total_autoridades = 0` and dividing raises `ZeroDivisionError`. It doesn't happen on these graphs.

**Q29. Explain basic PageRank in plain words.**
Every page has some "prestige." Each round it gives all of it away, split equally among the pages it links to. After many rounds the amounts stop changing. That steady state is the PageRank.

**Q30. Why doesn't `pagerank_basico` normalize?**
The total is conserved automatically. Every page gives away exactly what it has, and everything given is received by someone. The check verifies the sum is 1 after 0, 1, 2, 7 and 50 rounds.

**Q31. What if a page has no outgoing links (a dangling node)?**
Its rank isn't passed on, so the total **leaks**. I tested a → b: after a few rounds everything is 0. It does **not** crash with division by zero, because we only divide by `out_degree(u)` for u in `predecessors(v)`, and a predecessor has at least one out-link by definition. `nx.pagerank` handles dangling nodes by spreading their rank uniformly. Our version doesn't, which is fine here because the notebook assumes every page has an out-link.

**Q32. Why does C get PageRank 0 in RED_SUMIDERO even though it has links?**
Think of the flow. Each round C sends half its rank to A. A and B pass rank only to each other, so nothing flows back to C or D. The C–D loop keeps leaking half into the trap and never gets it back, so its rank shrinks geometrically to 0. (See the round-by-round table in Section 2.)

**Q33. Where is {A, B} in bow-tie terms?** It's a sink with no exit, a piece of OUT, and {C, D} is upstream (IN). See [Mistake 2](#-mistake-2-interpretation-22-question-2-has-in-and-out-backwards).

**Q34. Why is the PageRank order in RED_PR (A > C > B > D) different from the in-degree order?**
In-degrees are A 3, C 2, B 1, D 1, so B and D tie. PageRank breaks the tie because it looks at *who* links to you. B's single link comes from A, which has 0.4 split two ways = 0.2. D's single link comes from C, which has 0.2667 split two ways = 0.1333.

**Q35. What does s = 0.85 mean?**
A random surfer follows a random link 85% of the time and jumps to a uniformly random page 15% of the time. PageRank is the long-run share of time spent on each page.

**Q36. Why does scaling fix traps?**
Every page gets at least (1−s)/n each round, so no region can be absorbing. In matrix terms, all transition probabilities become positive. Perron–Frobenius then gives a **unique** stationary vector that doesn't depend on the start.

**Q37. Why does scaled PageRank still sum to 1?**
The teleport part totals n · (1−s)/n = 1−s. The link part totals s · 1 = s. Together that's 1 (with no dangling nodes).

**Q38. Why is k = 200 enough for the scaled version?**
The error shrinks by a factor s = 0.85 each round. 0.85²⁰⁰ ≈ 8 × 10⁻¹⁵, below the 1e-8 tolerance.

**Q39. What does the test with `np.random.default_rng(2026)` show?**
Starting from a random probability vector instead of the uniform one gives the same answer. The fixed point is unique.

**Q40. What happens as s → 1?**
Teleporting disappears and the trap returns. With s = 0.999: A + B ≈ 0.998 and C ≈ 0.001.

**Q41. In the trap network with scaling, why is A > B?**
B gets rank only from A. A gets rank from B and also from C.

**Q42. HITS vs PageRank: what does each capture that the other doesn't?**
HITS gives **two** roles: a page can be a great list (hub) without being a destination, and vice versa. PageRank gives **one** score and divides each vote by the voter's out-degree, so a link from a page with 1000 links counts little. Also, HITS is normally run on a query-specific subgraph, while PageRank is computed once for the whole Web.

### Part 3: Markets

**Q43. Why is sorting (greedy) optimal here, when Taller 2 needed brute force?**
Values have the product form rᵢ·vⱼ. If a higher-value advertiser sits in a worse slot, swapping improves the total by (r₁−r₂)(vₓ−vᵧ) > 0 (rearrangement inequality). So the sorted assignment is optimal. In Taller 2 the valuations were arbitrary, so this shortcut didn't work.

**Q44. Complexity of brute force vs greedy?**
Brute force tries n!/(n−m)! permutations (factorial). Greedy is a sort, O(n log n).

**Q45. Why pad with `None` when there are fewer advertisers than slots?**
So that every slot gets "someone" in the permutation, and `None` means empty. `valor_total` skips `None`.

**Q46. Any inefficiency in that padding?**
Yes. With several `None`s, `permutations` treats them as different items and generates duplicate assignments. The result is still correct, just slower.

**Q47. Why does the random test compare values and not assignments?**
With ties (equal values, or equal rates such as two 0-rate slots) several assignments are equally optimal. Brute force and greedy may pick different ones with the same value.

**Q48. What does `key=lambda j: (-valores[j], str(j))` do?**
It sorts by value descending (the minus sign) and breaks ties alphabetically. This makes the output deterministic.

**Q49. Explain the VCG payment in plain words.**
You pay for the damage your presence does to everyone else: how much better off they'd be if you disappeared.

**Q50. Walk me through x's VCG payment.**
Without x: y gets slot 1 (60) and z gets slot 2 (5), total 65. With x: y is in slot 2 (30) and z is in slot 3 (2), total 32. So x pays 65 − 32 = **33**, which is 3.3 per click.

**Q51. Why does z pay 0?**
z is last. Removing z doesn't move anyone up, so it harms no one.

**Q52. Why is truth-telling dominant under VCG?**
j's net gain = rᵢvⱼ − pⱼ = (total social value including j's **true** value) − (others' best value without j). The second term doesn't depend on j's report. The mechanism picks the assignment that maximizes *reported* total value. So reporting the truth makes the mechanism maximize exactly what j cares about, and lying can only move j to a worse assignment for them.

**Q53. How does the code check truthfulness?**
`pago_neto` runs the mechanism with a **fake** report but measures j's gain using the **true** value. For every j and every report from 0 to 15, the gain is never above the truthful gain.

**Q54. Why can a VCG payment never be negative, or more than the value?**
Not negative: without j the others can do at least as well as with j (they could keep the same slots). Not more than the value: VCG maximizes total value, so value(with j) ≥ value(without j). The check cell tests both.

**Q55. Explain GSP.**
Sort by bid, give the i-th slot to the i-th bidder, who pays per click the bid of the person just below.

**Q56. Why isn't truth-telling an equilibrium in GSP?** (the x example)
Bidding 7 gets x slot 1, where it pays 6 per click for 10 clicks, for a gain of 10. Bidding 5 drops x to slot 2: it loses 6 clicks, but the price falls from 6 to 1, for a gain of 4·6 = 24. What x loses in clicks is outweighed by the much lower price. Your payment depends on the next bid, and shading your bid changes who is next.

**Q57. Why is it truthful with one slot?**
Then GSP is a Vickrey auction. You pay the second-highest bid no matter what you bid, and your bid only decides whether you win. Winning is good exactly when your value exceeds the second bid, and bidding truthfully achieves exactly that.

**Q58. What is `es_equilibrio_gsp` checking?**
Nash equilibrium: for every advertiser and every alternative bid on the grid, changing **only** their own bid does not strictly increase their gain.

**Q59. Limitations of that check?**
It only tests deviations on the grid (steps of 0.5 up to 8), so it's an "equilibrium relative to the grid." It also relies on alphabetical tie-breaking, so a deviation to a tied bid is resolved by name.

**Q60. Why restrict the equilibrium search to bids ≤ value?**
Bidding above your value is risky: you might win a slot at a price above what it's worth to you. That's weakly dominated, so the standard analysis rules it out ("sin sobrepujar").

**Q61. Why did the industry use GSP if VCG is truthful?**
(1) It's simple to explain: "pay the next bid." VCG prices require re-solving the market. (2) It has an equilibrium with the same efficient assignment and the same revenue as VCG: (7, 4, 1) gives 44 = VCG. (3) Revenue can be higher in some equilibria (up to 49 in our grid, vs 44 for VCG). The downside is that many equilibria exist (222 on our grid) with revenue anywhere from 10 to 49, so outcomes are unpredictable.

**Q62. Is it a coincidence that (7, 4, 1) gives the same assignment as VCG and 3.1?**
No. The efficient assignment is fixed by the problem's structure (sort by value). Different mechanisms and equilibria differ only in **prices**, meaning how the surplus is split between advertisers and the search engine. That's the Taller 2 lesson again: theory fixes efficiency and leaves the split open.

**Q63. Why do x and y pay 4 and 1 per click under VCG with TASAS_GSP?**
x: others without x = y@10 + z@4 = 64, others with x = y@4 = 24, so x pays 40 (4 per click). y: without y = x@10 + z@4 = 74, with y = 70, so y pays 4 (1 per click).

### General / process

**Q64. How do you know your code is correct?**
Every function is tested against a trusted reference (networkx or brute force), on the given graphs *and* on random instances (a 45-node random graph, 200 random markets). We also checked some values by hand, like the RED_PR fixed point.

**Q65. What did you use AI for, and what did you check yourselves?**
Answer honestly and consistently with your `DECLARACION_IA`. See the note in Section 0.

---

## 4. "Change the code live" questions

TAs often ask you to modify something on the spot. Practice these.

**a) Write `alcanzan_a` without reversing the graph.**
```python
def alcanzan_a(G, s):
    visitados = {s}; pendientes = [s]
    while pendientes:
        actual = pendientes.pop()
        for vecino in G.predecessors(actual):    # only change: predecessors
            if vecino not in visitados:
                visitados.add(vecino); pendientes.append(vecino)
    return visitados
```

**b) Make `alcanzables` a BFS.**
```python
from collections import deque
pendientes = deque([s])
actual = pendientes.popleft()
```

**c) Fix `pagerank_basico` for dangling nodes** (spread their rank uniformly, like networkx):
```python
colgante = sum(rangos[u] for u in G if G.out_degree(u) == 0)
nuevos_rangos[v] = sum(...) + colgante / n
```

**d) Print the VCG price per click for each advertiser:**
```python
for j, p in precios.items():
    i = asig_opt.index(j)
    print(j, p / TASAS[i] if TASAS[i] > 0 else 0)
```

**e) Run PageRank with a different s, e.g. 0.5, and explain.**
More teleporting means the ranks are closer to uniform (1/n). `pagerank_escalado(RED_SUMIDERO, s=0.5)`: C and D get noticeably more rank.

**f) Check that a bid profile is an equilibrium:**
```python
es_equilibrio_gsp({"x": 7, "y": 4, "z": 1}, TASAS_GSP, VALORES, REJILLA)   # True
es_equilibrio_gsp(VALORES, TASAS_GSP, VALORES, REJILLA)                    # False
```

**g) Add edge G → A and recompute the bow-tie:**
```python
W = WEB.copy(); W.add_edge("G", "A"); estructura_bowtie(W)
```

---

## 5. Cheat sheet

| Item | Number |
|---|---|
| WEB | 12 nodes, 12 edges |
| Bow-tie | CFC {A,B,C}, IN {D,E}, OUT {F,G}, TUBE {H}, TENDRILS {I,J}, ISLAND {K,L} |
| Reach from D / E / F / G | 9 / 8 / 2 / 1 of 12 |
| HITS authorities (RED_HITS) | a2 .5, a1 .3229, a3 .1771, h's 0 |
| HITS hubs (RED_HITS) | h1 .3542, h2 .2915, h3 .1771, a1 .1771 |
| Basic PR, RED_PR | A 2/5, C 4/15, B 1/5, D 2/15 |
| Basic PR, RED_SUMIDERO | A .5, B .5, C 0, D 0 |
| Scaled PR (.85), RED_SUMIDERO | A .4163, B .3914, C .1086, D .0837 |
| Scaled PR (.85), RED_PR | A .3845, C .2646, B .2009, D .1500 |
| Optimal ads [10,5,2] | x,y,z → 102 (swap x,y → 97) |
| VCG [10,5,2] | x 33 (3.3/click), y 3 (0.6/click), z 0 → revenue 36 |
| GSP truthful (7,6,1) on [10,4,0] | x 10, y 20, revenue 64 |
| GSP x bids 5 | x 24, y 10, revenue 54 |
| GSP equilibria (grid, bids ≤ value) | 222, revenue 10 to 49 |
| VCG [10,4,0] | x 40 (4/click), y 4 (1/click) → 44 = GSP equilibrium (7,4,1) |
