# MCviz Experimental Datasets

This repository contains the eight datasets used in the MCviz paper. Each dataset is a processed, unweighted, undirected multipartite graph. The table reports the size of each processed graph; the sections below describe its source, file format, and experimental use.

| Dataset | File | Layers | Vertices | Edges | Layer types / construction |
|---|---|---:|---:|---:|---|
| WordNet | [WordNet.txt](WordNet.txt) | 25 | 29,904 | 27,544 | Synsets in selected lexname classes |
| LastFM | [LastFM.txt](LastFM.txt) | 3 | 24,164 | 216,630 | User, Artist, Tag |
| Hetionet | [Hetionet.txt](Hetionet.txt) | 6 | 34,134 | 1,315,516 | Gene, Anatomy, Biological Process, Compound, Disease, Pathway |
| DRKG | [DRKG.txt](DRKG.txt) | 5 | 75,655 | 1,515,820 | Gene, Anatomy, Biological Process, Compound, Disease |
| BibSonomy | [BibSonomy.txt](BibSonomy.txt) | 3 | 977,914 | 3,754,828 | User, Tag, Item |
| Freebase | [Freebase.txt](Freebase.txt) | 6 | 1,669,251 | 2,536,415 | Genre, Group, Composer, Musician, Artist, Album |
| YouTube | [YouTube.txt](YouTube.txt) | 32 | 1,116,052 | 2,894,183 | Constructed user partitions |
| AMiner | [AMiner.txt.gz](AMiner.txt.gz) | 3 | 4,891,819 | 12,518,010 | Author, Paper, Venue |

## Data Format and Use

Each data line represents one cross-layer edge using four whitespace-separated nonnegative integers:

```text
layer_u vertex_id_u layer_v vertex_id_v
```

Layers are numbered from zero. A vertex is identified by its layer and vertex ID; IDs need not be contiguous. Lines beginning with `#` are comments. Edges are undirected, and original relation labels and directions are omitted. Isolated vertices do not appear in an edge list.

`AMiner.txt.gz` is compressed to fit GitHub's ordinary Git file-size limit. Decompress it before passing it to a program that expects a plain-text file:

```bash
gzip -dk AMiner.txt.gz
```

The command creates `AMiner.txt` and keeps the archive. The other seven datasets are uncompressed. Verify the distributed files with the checksums in [SHA256SUMS](SHA256SUMS):

```bash
sha256sum -c SHA256SUMS
```

## Sources and Processing

The graphs below are derived from external datasets. Please credit the original sources and their publications alongside MCviz, and consult each source for its usage and redistribution terms. This repository does not assign a common license to the datasets.

- **WordNet:** [WordNet 3.0](https://wordnet.princeton.edu/), [Miller (1995)](https://doi.org/10.1145/219717.219748). Synsets are assigned to 25 selected lexname classes; cross-class semantic and lexical relations are retained. The type selection uses graph structure. The WordNet license notice is included in the file header.
- **LastFM:** [GroupLens HetRec 2011](https://grouplens.org/datasets/hetrec-2011/), [Cantador et al. (2011)](https://doi.org/10.1145/2043932.2044016). User–artist–tag events are projected into User–Artist, User–Tag, and Artist–Tag edges.
- **Hetionet:** [Hetionet v1.0](https://het.io/), [Himmelstein et al. (2017)](https://doi.org/10.7554/eLife.26726). The six selected entity types and their cross-type relations form a simple undirected graph.
- **DRKG:** [Drug Repurposing Knowledge Graph](https://github.com/gnn4dr/DRKG). The graph retains five selected biomedical entity types and their cross-type relations. DRKG includes Hetionet data, so the sources overlap.
- **BibSonomy:** [BibSonomy](https://www.bibsonomy.org/) and [KONECT](http://konect.cc/). This is the 3,754,828-edge historical processed version used in the paper, distinct from the later 7,665,240-edge conversion described in the source data directory. The layer order is User, Tag, Item.
- **Freebase:** [Freebase Easy](https://freebase-easy.cs.uni-freiburg.de/dump/), [Bast et al. (2014)](https://doi.org/10.1145/2567948.2577016). Six music entity types are made disjoint by assigning each entity to its first matching type in the listed order; retained cross-layer relations are merged into undirected edges.
- **YouTube:** [SNAP com-YouTube](https://snap.stanford.edu/data/com-Youtube.html), [Yang and Leskovec (2012)](https://doi.org/10.1109/ICDM.2012.138). The project constructs 32 user layers using a deterministic hash partition, then adjusts the partition to preserve four feasible star witnesses. Only original cross-layer edges are retained; the layers are not source-provided user types.
- **AMiner:** [metapath2vec release](https://ericdongyx.github.io/metapath2vec/m2v.html), [Dong et al. (2017)](https://doi.org/10.1145/3097983.3098036); original collection: [Tang et al. (2008)](https://doi.org/10.1145/1401890.1402008). The processed graph contains author–paper and paper–venue edges.

For WordNet, Hetionet, DRKG, Freebase, and YouTube, the full graphs retain cross-layer edges beyond those required by a particular query pattern. Vertex counts refer to endpoints in the full processed graphs.

## Paper Workloads

The main comparison uses the full datasets above, with restricted domains for WordNet and AMiner. The accompanying pattern files define the WordNet path and DRKG tree used in the paper.

- **WordNet-P3:** [wordnet-p3.txt](wordnet-p3.txt) contains the complete projection of WordNet layers 12, 13, and 14, renumbered 0, 1, and 2. It has 2,696 vertices and 1,685 edges, including 21 edges outside the path pattern. Load [metadata/wordnet-p3.vertices.tsv](metadata/wordnet-p3.vertices.tsv) as the explicit vertex domain to retain isolated vertices. The path is defined in [patterns/wordnet-p3.pattern](patterns/wordnet-p3.pattern).
- **AMiner-AP:** Retain the Author and Paper layers and their edges from `AMiner.txt`. This domain has 4,887,936 vertices and 9,323,605 edges.
- **DRKG:** [patterns/drkg-tree.pattern](patterns/drkg-tree.pattern) defines the tree with edges `(0,2)`, `(0,3)`, `(0,4)`, and `(1,4)`. The search anchor does not change the pattern.

See the MCviz paper for the other patterns and experimental settings.
