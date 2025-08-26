# Clusterização Hierárquica — Single, Complete, Ward + Swiss Roll (Q1–Q11)

Este repositório contém um **notebook Jupyter** (IPYNB) com uma trilha sobre **clusterização hierárquica**:

- definições e intuição de **single linkage**, **complete linkage** e **Ward linkage**;
- demonstrações passo a passo sobre pontos sintéticos (incluindo a **Figura 1** e **make_blobs**);
- execução em 3D no **Swiss Roll** (`make_swiss_roll`), com **métricas** de qualidade;
- construção de **dendrogramas** e **mapas de calor** (_heatmaps_) consistentes com o dendrograma;
- método prático para **escolher \(K\)** via **salto relativo** nas alturas de fusão;
- discussão dos resultados (Q8–Q9) e avaliação crítica do \(K\) escolhido (Q11 → Q7).

> Linguagem: **Python 3**, bibliotecas padrão de ciência de dados (**NumPy**, **Pandas**, **Matplotlib**, **SciPy**, **scikit-learn**).

---

## 🧪 Conteúdo técnico (resumo)

- **Q1 – Single linkage (conceito).** Distância entre grupos é o **menor** par ponto‑a‑ponto:

  $$
  d_{\text{single}}(A,B)=\min_{x\in A,\; y\in B} d(x,y).
  $$

  Tende ao _chaining_ (cadeias).

- **Q2 – Single linkage (ilustrado).** Sequência de fusões sobre pontos 2D (incluindo **Figura 1**).

- **Q3 – Ward linkage (conceito).** Funde o par que **menos aumenta** a soma dos quadrados intra‑cluster (WCSS):

 $$
\Delta(A,B)=\frac{n_A\,n_B}{n_A+n_B}\,\left\| \mu_A-\mu_B \right\|^2
$$

- **Q4 – Ward linkage (ilustrado).** Fusões preferem clusters compactos; evita _chaining_.

- **Q5 – Complete linkage (conceito).** Distância entre grupos é a **maior** distância ponto‑a‑ponto:
$$
d_{\text{complete}}(A,B)=\max_{x\in A,\, y\in B} d(x,y).
$$

  Produz clusters compactos (diâmetro pequeno).

- **Q6 – Complete linkage (ilustrado).**

- **Q7 – Swiss Roll (3D).** `AgglomerativeClustering` com `single/complete/average/ward` (fixando \(K=3\)). Métricas: **Silhouette**, **Calinski‑Harabasz (CH)**, **Davies‑Bouldin (DB)**.

- **Q8 – Dendrogramas (amostra).** SciPy `linkage` + `dendrogram` com **cores compatíveis** com os rótulos (\(K=3\)) e **ordem de folhas** coerente.

- **Q9 – Heatmap associado ao dendrograma.** Reordena a **matriz de distâncias** pela ordem das folhas; tiras de cores (top/left) indicando rótulos.

- **Q10 – Discussão dos resultados (Q8–Q9).** Leitura dos blocos, saltos e o que significam no Swiss Roll.

- **Q11 – Escolha de \(K\) por salto relativo.** Procura do **maior salto relativo** no topo do dendrograma,

$$
r_i = \frac{h_{i+1}}{h_i}
$$

Corte antes do salto:

$$
K = n - (i+1)
$$

  Avaliação de \(K=3\) da Q7.

> **Compatibilidade scikit‑learn:** o código trata a mudança de API (`metric` vs `affinity`) ao criar o `AgglomerativeClustering` (fallback automático).

---

## 📊 Métricas e interpretação (Swiss Roll)

- **Single:** sofre _chaining_; cortes fracos e blocos pouco definidos.
- **Complete/Average:** cortes mais nítidos (clusters compactos), porém tendem a **fatiar** o manifold em várias partes.
- **Ward:** partições mais compactas e **salto** mais claro no topo do dendrograma; ainda “fatia” o rolo (variância euclidiana).

**Escolha de \(K\)** (método do salto relativo): com **Ward** o maior salto apontou **\(K=3\)**, coerente com o resultado da Q7. _Complete/Average_ tenderam a sugerir valores maiores (7–8) por penalizarem formas alongadas.
