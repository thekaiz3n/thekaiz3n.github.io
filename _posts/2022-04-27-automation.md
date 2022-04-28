---
title: Automation
excerpt: "A explanation e suggestion of automation flow"
last_modified_at: 27-04-2022
categories:
- cheatsheet
tags:
- pentest
- automation
- recon
image: /assets/images/cheatsheet/automation2.jpg
---

### Recon automation

### What is the purpose of automation?

Is use the machines to do **repetitive tasks**, in a way that humans cannot do. Using the computational power to **leverage** the desired results. Highlighting two parts:

- Repetitive tasks: The objective is not replace humans, and not our knowledge too, but use in a more intelligent way. Where the tasks that we already know how to do, we start to using programming skills to automate these tasks.
- Leverage: The computers have a enormous advantage of humans in processing information fast. Knowing this, why not use?

### Automation principles

- Patterns: Patterns are follow rigorously.
- Control: Every piece of the code, and the execution flow is controlled. And do what is mean to do.
- Organization: **Structure** defined in the patterns are follow by all outputs during the process.
- Parallel/Optimization: Computational power is used of the more **intelligent** way possible.
- Database: Data is structured based in one idea: **Correlation**.
- Result: The automation that don’t create **results**, is useless.

### Ideal structure

- Database: The database has to be flexible and allow correlation. Suggestion:
    - NoSQL databse
- Code structure: Has to be of easy an fast deploy: Suggestions:
    - VPS or Cloud services: This way the hardware are in separable concerns and allow easy upgrades.
    - Docker: Easy and fast deploy.
    - Auto config: Scripts to automatic config the machines, with all that need to work.
- Notifications: The process must have notifications to the owner.
    - Telegram: Easy and fast.
    - Discord: More organization.
- Tools outputs: All the scripts and tools must have the same output structure.
    - Treatment: All output must have data processing.
- Continuous integration: Allow that the automation iterate in the data, to search for new data.

### Process

![center-aligned-image](/assets/images/cheatsheet/automation.jpg){: .align-center}
