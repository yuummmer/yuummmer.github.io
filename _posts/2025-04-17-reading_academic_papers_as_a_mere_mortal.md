---
layout: default
title: Reading Academic Papers As a Mere Mortal
permalink: /research-workflow-bioinformatics/
date: 2025-04-17
categories: blog
---
> “Research is formalized curiosity. It is poking and prying with a purpose.”  
> — *Zora Neale Hurston*

<a href="{{ "/" | relative_url }}">← Back to Home</a>

# How I Read Academic Papers as a Mere Mortal 
*...and why ResearchRabbit + Zotero are my go-to tools in bioinformatics*

When I started diving back into bioinformatics, I quickly realized something: there’s a *lot* of literature—and a lot of it isn’t written with newcomers in mind.

Many papers assume you already know the tools they’re comparing or have the experimental background to interpret complex figures. Some come with raw datasets, GitHub repos, or command-line tools that are more intimidating than helpful at first. Still, I wanted to engage with the science directly—not just read blog summaries or reviews. I needed a way to **track what I was learning**, follow threads across disciplines, and **connect papers to my own hands-on projects**.

This post outlines the reading system I built using **ResearchRabbit** and **Zotero**, and how it helps me stay grounded while working on projects like a microbiome metadata toolkit or a gene annotation pipeline. It’s not just about organizing PDFs—it’s about learning bioinformatics in context, as someone coming from a non-traditional path.

---

##  ResearchRabbit Hole

Instead of starting with a random PubMed search, I usually begin with **one well-known paper or method** I’ve heard about—maybe something mentioned in a course or GitHub README.

I paste that paper into [ResearchRabbit](https://www.researchrabbit.ai/), which instantly shows a visual map of related works—papers it cites, papers that cite it, and others with similar themes. I’ve found this especially helpful in **bioinformatics**, where tools evolve quickly, and the newest methods often reference cutting-edge repositories or preprints.

### Here’s how I use ResearchRabbit:
- I seed it with a paper on something like **kallisto for RNA-seq quantification**, which generates a map that includes **alternative tools** like Salmon, plus papers applying these tools to real data.
- I **track authors** across fields (e.g., someone working on protein design who later publishes on gene editing).
- I create **collections based on problems I care about**, like “transcript quantification” or “human microbiome metadata standards.”

💡 *This helps me connect methods to use cases, not just read about theory. It’s especially useful for spotting open-access datasets or identifying reproducible studies I might want to try replicating.*

---

## 📚 Step 2: Organizing, Annotating, and Tagging with Zotero

Once I’ve found promising papers, I move them into [Zotero](https://www.zotero.org/), where I do the real organizing and deep reading.

### Here’s what makes Zotero work for me as someone juggling multiple bioinformatics projects:
- I organize papers by **project** (e.g., "RAG LLM systems", "Microbiome metadata", “Transcriptomics methods”) and **theme** (“data ethics”, “pipelines I want to try”).
- I use **tags** : `RNA-seq`, `open dataset`, `has code`, `TF-IDF`, `toRead`, `read`.
- I annotate PDFs directly, often leaving comments like “this aligns with the kallisto parameters I saw in the GALES pipeline” or “could summarize this chunk using my own RAG model?”

💡 *I also keep an eye out for papers that share GitHub repos or CLI commands—I tag those `has repo` so I can return later when I’m implementing something similar.*

---

## 🧪 Step 3: Connecting Papers to What I’m Building

This is where my approach differs from most academic workflows: **I’m reading so I can build.**

I’m not writing a dissertation or trying to publish a paper—I’m working on tools, dashboards, or visualizations.
---

## 🧬 A Real Example: Exploring New Sequencing Technologies

Recently, I found myself curious about the latest DNA sequencing technologies. I didn’t have a specific research question—just a hunch and a personal connection. I used to work near **Pleasanton, CA**, and always wondered what **10x Genomics** actually did.

That curiosity led me to discover the **Xenium platform**, a newer spatial transcriptomics technology. I typed *Xenium* into [ResearchRabbit](https://www.researchrabbit.ai/), which gave me a branching map of related papers and studies using the platform in different -omics contexts.

I created a new topic in ResearchRabbit called **Spatial -Omics** and started building out a collection of papers:

- **Marco Salas, Sergio et al.** “*Optimizing Xenium In Situ data utility by quality assessment and best-practice analysis workflows.*” *Nature Methods* (2025)  
  [doi:10.1038/s41592-025-02617-2](https://doi.org/10.1038/s41592-025-02617-2)

- **Zhang W, Tran T, Ly A, et al.** “*Reproducibility and quality assessment study of Xenium and Visium spatial transcriptomics assays from multiple carcinoma samples via an end-to-end spatial multi-omics platform.*” *Journal for ImmunoTherapy of Cancer* (2024)  
  [doi:10.1136/jitc-2024-SITC2024.0832](https://jitc.bmj.com/content/12/Suppl_1/SITC2024.0832)

Because these papers are both **very recent**, they don’t yet have many citation links or connections in ResearchRabbit’s web. That said, they still surfaced valuable keywords, authors, and tool references I might want to explore. Over time, I expect the network will grow richer as more researchers cite or build on this work.

I synced the papers to **Zotero** so I could keep the citations organized, tag the papers, and return to them for future projects. Then I created **summary notes in Evernote**, capturing key methods and ideas in my own words while the material was still fresh.

💡 *This kind of informal, curiosity-driven research is one of the most rewarding parts of transitioning into bioinformatics. It’s how I learn—by exploring rabbit holes, connecting tools to real studies, and capturing my thought process as I go.*

---

## 🌐 Staying Grounded in Open Science and Accessibility

As someone coming into bioinformatics with a background in sociology and product systems—not a PhD—**open science matters to me**. I choose tools like Zotero and ResearchRabbit because they:
- Respect **open access**
- Help me **organize across disciplines**
- Support **community science** and independent learning

This workflow also lets me share what I find. I’ve been able to help other learners discover datasets, find readable papers, and even explain why certain methods might be more appropriate in a clinical versus research context.

---

## Final Thoughts

I’m not an academic. I’m someone building tools and learning out loud. But this system—**ResearchRabbit for discovery, Zotero for retention, and projects for application**—has helped me stay curious, organized, and grounded.

If you’re diving into bioinformatics, NLP for health, or just trying to figure out what to read next, I highly recommend giving this workflow a try.
