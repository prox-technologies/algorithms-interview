<p align="center">
  <img src="graphs/images/study_pack.png" width="300">
</p>

# Prox Technical Interview Study Pack

One important part of our product is an ingest agent that takes incoming technical knowledge (schematics, procedures, tribal knowledge about physical products) and maintains a live knowledge graph. The graph is structured like a table-of-contents tree, loosely inspired by [PageIndex](https://github.com/VectifyAI/PageIndex) and [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). The agent is the sole maintainer of the graph. It decides where to place new information, how to reorganize, and how to traverse for retrieval. Humans never edit the graph directly -- the goal is to build the best agent that maintains its own graph.

As an engineer at Prox you will think about knowledge representation constantly. You need a strong foundation in graph theory to do this well.

In 1968, Chow and Liu proved that the optimal tree approximation of any joint distribution is the maximum-weight spanning tree of the mutual information graph. So questions like "what's the right knowledge structure," "which edges carry real information," and "what's the optimal skeleton" are all spanning tree questions. Ontology construction, graph compression, document indexing -- the same core problem.

## What we're testing

This interview tests your ability to reason about dynamic graph structures -- how spanning trees change when the underlying graph changes.

This is the core loop of our ingest agent:

- A new document arrives -- new node with edges to existing knowledge
- Information gets corrected -- edge weights change
- Structure needs reorganization -- edges get swapped

Each of these is a spanning tree maintenance problem.

## How to prepare

The study pack is in `graphs/`. Work through it in order:

| # | Material | Time | What it covers |
|---|----------|------|----------------|
| 1 | Video lecture (below) | 90 min | Erik Demaine's MIT 6.046J lecture -- greedy algorithms, cut property, Prim's and Kruskal's with proofs |
| 2 | [Part 1: MST Foundations](graphs/1-foundations.md) | 60 min | Definitions, MST properties (cut, cycle, uniqueness), Prim's and Kruskal's algorithms |
| 3 | [Part 2: Dynamic MST](graphs/2-dynamic-mst.md) | 60 min | What happens when you add an edge, add a node, or change a weight |
| 4 | [Part 3: Practice Problems](graphs/3-practice.md) | 2-3 hrs | Problems with solutions -- work them without peeking |

### Video lecture

[![MIT 6.046J Lecture 12: Greedy Algorithms, Minimum Spanning Trees](https://img.youtube.com/vi/tKwnms5iRBU/maxresdefault.jpg)](https://www.youtube.com/watch?v=tKwnms5iRBU)

Start here. Erik Demaine walks through greedy algorithms, the cut property, and Prim's and Kruskal's algorithms with full proofs.

**Prerequisites.** You should be comfortable with BFS, DFS, connected components, and weighted graphs. If you need a refresher, chapters 20-22 of [CLRS (4th ed)](https://github.com/wuzhouhui/misc2/blob/master/Introduction.to.Algorithms.4th.Edition.2022.4.pdf) cover the foundations.

## How the interview works

~60-90 minutes on Google Meet with [Excalidraw](https://excalidraw.com/) open. One problem, split into parts with escalating difficulty.

- Starts with a warm-up on a concrete graph
- Builds toward algorithm design and proof
- Think out loud, ask questions, draw things

We're not looking for memorized solutions. We're looking for someone who can take the cut and cycle properties and apply them to a situation they haven't seen before.

---

*Materials adapted from MIT 6.1220/18.410J (Fall 2023) and Jeff Erickson, Algorithms (Ch. 7).*
