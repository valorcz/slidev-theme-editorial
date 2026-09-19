---
theme: ./
title: Editorial Theme Showcase
author: Antigravity Team
date: 2026-08-20
# accent: '#EB5E28'
accent: 'orange'
---

# Editorial Slidev Theme

### A modern, typographic, dark & light theme for Slidev

<div class="mt-8 flex gap-3 flex-wrap">
  <Badge tone="primary">Primary Accent</Badge>
  <Badge tone="secondary">Secondary (OKLCH +145°)</Badge>
  <Badge tone="tertiary">Tertiary (OKLCH +240°)</Badge>
  <Badge tone="good">Semantic Good</Badge>
  <Badge tone="muted">Zero Lock-in</Badge>
</div>


---
layout: cover-network
tag: Protocol Analysis // 2026
---

# Network Cover Layout

### Deep cyber & editorial visuals with blur glow

---
layout: section
title: Core Concepts
---

# Section One

### High-impact section header layout with animated sinus waves backdrop

---
layout: two-cols-header
---

# Two Columns with Header

### Structured multi-column grid layout for technical comparisons

::left::

<Card title="Frontend Stack" icon="i-carbon-application">

- Vue 3 & Composition API
- Slidev Context Integration
- Tailwind CSS Utilities
- Dynamic Watermarking

</Card>

::right::

<Card title="Typography System" icon="i-carbon-text-scale">

- **Headlines:** DM Sans (Bold, Tight)
- **Body:** Inter (Clear reading width)
- **Code:** JetBrains Mono
- **Contrast:** High WCAG Compliance

</Card>

---
layout: vs
---

# Monolithic vs Microservices

### Architectural trade-offs evaluated with native Markdown slots

::left::

### Monolith Architecture

- Single deployment pipeline & binary
- Simple in-memory function calls
- Shared transactional database
- Challenging horizontal scaling at extremes

::right::

### Microservices Architecture

- Independent team delivery lifecycles
- Explicit network boundary contracts
- Polyglot tech stack flexibility
- Higher operational & tracing complexity

---
layout: agenda
---

# Workshop Modules

### Auto-numbered overview cards generated directly from standard ordered lists

1. **Foundations of Memory**
   Understanding physical memory, logical paging, and CR3 register translations.

2. **Process Address Spaces**
   Analyzing VAD trees, page tables, and memory permissions across execution levels.

3. **Kernel Structures & Hooks**
   Examining EPROCESS doubly-linked lists and direct kernel object manipulation (DKOM).

4. **Triage & Forensics**
   Using Volatility 3 plugins to reconstruct suspicious artifacts and anomalies.

---
layout: timeline
---

# Incident Timeline

### Sequential rail layout auto-styled from standard Markdown lists

- **03:14 UTC — Initial Vector**
  Phishing attachment executed inside unprivileged user context.

- **03:22 UTC — Privilege Escalation**
  Vulnerable kernel driver leveraged to achieve Ring-0 DKOM manipulation.

- **03:35 UTC — Lateral Movement**
  Pass-the-hash authentication attempted across internal domain controllers.

- **04:10 UTC — Detection & Containment**
  Host isolated and physical memory dumped via hypervisor snapshot.

---
layout: timeline-vertical
kicker: Threat Telemetry
aside: Forensics
---

# Chronological Kill Chain

### Detailed vertical timeline for incident response with multi-line tactical notes

- **03:14:02 UTC — Initial Ingress & Shellcode Execution**
  Macro-enabled document dropped payload into `%TEMP%\patch.sys`. Execution bypassed local policy via signed binary proxying.

- **03:22:45 UTC — Ring-0 Kernel Token Elevation**
  Vulnerable third-party driver exploited to alter `EPROCESS` token pointers, elevating privileges directly to `NT AUTHORITY\SYSTEM`.

- **03:31:10 UTC — Defense Evasion & Telemetry Blindness**
  ETW (Event Tracing for Windows) disabled by runtime patch of `ntdll!EtwEventWrite`. Memory scanning hooks suppressed.

- **03:45:18 UTC — Lateral Movement & Hypervisor Containment**
  Pass-the-hash attempted across internal controllers. Watchdog triggered automated host isolation and memory acquisition.

---
layout: steps
---

# Paging Resolution Pipeline

### Step-by-step horizontal chevron flow generated from list syntax

1. **CR3 Root**
   Read base address of top-level translation table.

2. **Index Lookup**
   Extract 9-bit virtual address slice for offset calculation.

3. **Page Entry**
   Verify present bit and privilege mask attributes.

4. **Physical Page**
   Offset combined with frame number to yield physical address.

---
layout: stats
kicker: Telemetry
---

# Performance Metrics

### Real-time hardware translation engine benchmarks

<div class="stat-grid grid-cols-3">
  <Stat value="4.2" unit="GB/s" label="Primary Metric" tone="primary" icon="i-carbon-meter" />
  <Stat value="99.9" unit="%" label="Secondary Contrast (OKLCH)" tone="secondary" icon="i-carbon-checkmark-outline" />
  <Stat value="< 12" unit="ms" label="Tertiary Harmonic (OKLCH)" tone="tertiary" icon="i-carbon-timer" />
</div>



---
layout: three-cols-header
---

# Three Columns Layout

### Flexible grid for component breakdowns

::left::

<Card title="Glitch Header">
An animated decoding text effect component for cyber/tech presentation openers.
</Card>

::center::

<Card title="3D Hero Cube">
A CSS 3D wireframe cube with dynamic perspective transform calculations.
</Card>

::right::

<Card title="Sinus Waves">
High-DPI responsive canvas animation background for section headers.
</Card>

---
layout: metric
value: "10x"
label: "Faster Cold-Boot Time"
kicker: "Benchmark Results"
tone: "good"
---

# Instant Slide Loading

### Zero-bundle overhead with Vite & UnoCSS

- Fully reactive local slide context without global router race conditions
- Native Shiki syntax highlighting with granular language grammar tokens
- Sub-50ms HMR updates during slide editing sessions
- High-contrast typography optimized for readability across projectors

---
layout: compare
columns: ['Architecture Property', 'Monolith', 'Microservices', 'Editorial Verdict']
rows: [
  { metric: 'Deployment Pipeline', before: 'Single artifact', after: 'Independent CI/CD', delta: 'Decoupled' },
  { metric: 'Fault Isolation', before: 'Process shared', after: 'Network bounded', delta: 'Resilient' },
  { metric: 'Operational Overhead', before: 'Low initial', after: 'High (K8s/Tracing)', delta: 'Trade-off' },
  { metric: 'State Management', before: 'ACID Transactions', after: 'Eventual Consistency', delta: 'Complex' }
]
---

# Architecture Comparison Matrix

### Structured before/after benchmarks with native tabular styling

---
layout: define
---

# Virtual Address Descriptor
#### [noun] · Windows NT Kernel Memory Management

A self-balancing binary search tree (AVL) data structure used by modern operating system kernels to record, protect, and track allocated memory regions inside process address spaces.

- **Tree Balancing:** Automatically rebalances node heights upon `VirtualAlloc` or `VirtualFree` API invocations
- **Page Permissions:** Enforces `PAGE_EXECUTE_READWRITE` privilege bits across process execution levels
- **Forensic Triage:** Queried by volatility memory inspection plugins to detect unlinked process anomalies

---
layout: feature
features: [
  { icon: 'i-carbon-flash', title: 'Vite Lightning Core', desc: 'Instant server start and near-zero latency Hot Module Replacement for deck authoring.' },
  { icon: 'i-carbon-text-font', title: 'Editorial Typography', desc: 'Carefully tuned typographic scale with DM Sans, Inter, and JetBrains Mono.' },
  { icon: 'i-carbon-dashboard-reference', title: 'Top Section Rail', desc: 'Dynamic wayfinding progress bar automatically tracking sections without export bugs.' },
  { icon: 'i-carbon-checkbox-checked', title: 'Zero Vendor Lock-in', desc: 'True Markdown-first presentation format. Switch themes without breaking your slides.' }
]
---

# Core Theme Features

### Production-ready architectural capabilities for technical presentations

---
layout: panels
---

# Security Sub-Systems

### Four-panel compartmentalized operational domains

::one::

<div class="flex items-center gap-2 mb-2 text-[var(--slidev-theme-primary)]">
  <span class="i-carbon-locked text-xl"></span>
  <span class="font-bold">Ring-0 Isolation</span>
</div>

Kernel address space separation prevents unprivileged user contexts from modifying translation page tables directly.

::two::

<div class="flex items-center gap-2 mb-2 text-[var(--slidev-theme-primary)]">
  <span class="i-carbon-security text-xl"></span>
  <span class="font-bold">Credential Guard</span>
</div>

Hardware-enforced virtualization isolates LSA secrets in a secure container inaccessible to malicious kernel drivers.

::three::

<div class="flex items-center gap-2 mb-2 text-[var(--slidev-theme-primary)]">
  <span class="i-carbon-network-3 text-xl"></span>
  <span class="font-bold">Boundary Filtering</span>
</div>

Zero-trust network policies enforce strict mutual TLS authentication between decoupled microservice workloads.

::four::

<div class="flex items-center gap-2 mb-2 text-[var(--slidev-theme-primary)]">
  <span class="i-carbon-report text-xl"></span>
  <span class="font-bold">Audit Telemetry</span>
</div>

Streaming event logs and ETW providers feed high-fidelity SIEM detections for privilege escalation attempts.

---
layout: section
title: Media & Layouts
effect: grid
accent: secondary
---

# Section Two

### Rich editorial media layouts, progress rails, and visual components


---
layout: default
aside: Deep Dive
accent: secondary
---


#### Architecture Notes
# Progressive Dual-Mode

### Zero vendor lock-in with native Markdown heading and list structures

- **Kickers via H4:** Write `#### Kicker` above `# Title` without proprietary frontmatter
- **Progress Rail:** Dynamically segmented from section dividers, visible at the top edge
- **Aside Tags:** Corner tags like `aside: "Deep Dive"` for asides, tangents, or lab notes
- **Fallback Bridge:** Full backward compatibility for ported Tahta frontmatter decks

---
layout: image-right
image: https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5?auto=format&fit=crop&w=800&q=80
caption: "Sub-system memory dump analysis"
---

# Image Right Layout

### Refactored to derive from LayoutBase

This layout pairs deep technical narrative with high-contrast imagery:

- Inherits unified layout padding and footer
- Top progress rail wayfinding
- Editorial image framing with caption tag
- Smooth hover desaturation transition

---
layout: 3-images
imageLeft: https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=800&q=80
imageTopRight: https://images.unsplash.com/photo-1550751827-4bd374c3f58b?auto=format&fit=crop&w=800&q=80
imageBottomRight: https://images.unsplash.com/photo-1504639725590-34d0984388bd?auto=format&fit=crop&w=800&q=80
---

# Three Images Bento Showcase

### Asymmetric editorial layout for technical diagrams and visual assets

---
layout: default
---

# Editorial Components

### Built-in UI building blocks for technical slide decks

<div class="grid grid-cols-2 gap-6">
  <div>
    <Callout tone="tip" title="Best Practice">
      Keep slide content in standard Markdown slots so your deck can switch themes effortlessly.
    </Callout>
    
   <Callout tone="warning" title="Warning">
     Unchecked DKOM unlinking bypasses process lists but leaves thread traces behind.
   </Callout>

   <div class="mt-4">
     <Meter :value="78" label="System RAM Utilization" tone="accent" />
     <Meter :value="42" label="VAD Tree Depth" tone="good" />
   </div>
</div>

  <div>
    <Terminal title="volatility3" :lines="[
      { cmd: 'vol -f memory.raw windows.pslist' },
      { comment: 'scanning active process blocks...' },
      { out: 'PID    PPID   ImageFileName      Offset' },
      { out: '4      0      System             0xfa800100' },
      { out: '432    4      smss.exe           0xfa800180' }
    ]" />
  </div>
</div>

---
layout: section
title: Interactive Tooling
effect: particles
accent: tertiary
---

# Section Three

### High-performance constellation particles with hairline editorial mesh

---
layout: two-cols-header
---

# Developer Tooling Components

### Keycaps, interactive file trees, and terminal windows

::left::

<FileTree title="slidev-theme-editorial" :items="[
  { name: 'components', children: [
    { name: 'Kbd.vue', desc: 'keycap shortcuts' },
    { name: 'FileTree.vue', desc: 'hierarchy guides' },
    { name: 'SectionRail.vue', highlight: true, desc: 'progress wayfinding' }
  ]},
  { name: 'layouts', children: [
    { name: 'default.vue' },
    { name: 'vs.vue' },
    { name: 'timeline.vue' }
  ]},
  { name: 'example.md', desc: 'showcase deck' }
]" />

::right::

<div class="space-y-3 pt-1">
  <div>
    <h3 class="!mt-0 !mb-1.5">Keyboard Shortcuts</h3>
    <p class="text-sm">Press <Kbd>⌘</Kbd> + <Kbd>K</Kbd> or <Kbd tone="accent">Ctrl</Kbd> + <Kbd tone="accent">Shift</Kbd> + <Kbd tone="accent">P</Kbd> to open the command palette.</p>
  </div>

  <div>
    <h3 class="!mb-1.5">Editorial Badges</h3>
    <div class="flex flex-wrap gap-1.5">
      <Badge tone="primary">Primary</Badge>
      <Badge tone="secondary">Secondary</Badge>
      <Badge tone="tertiary">Tertiary</Badge>
      <Badge tone="good">Active</Badge>
      <Badge tone="warn">Pending</Badge>
      <Badge tone="bad">Failed</Badge>
      <Badge tone="info">v1.0</Badge>
      <Badge tone="muted">Draft</Badge>
    </div>
  </div>

  <div>
    <h3 class="!mb-1.5">Topic Tags</h3>
    <Tags :items="['TypeScript', 'Vue 3', 'Slidev', 'Tailwind']" tone="secondary" />
  </div>

  <Callout tone="tip" title="Composable Primitives">
    Every component can be placed freely inside multi-column slides or standalone markdown canvases.
  </Callout>
</div>

---
layout: two-cols-header
---

# People & Figures

### Structured speaker attribution and editorial visual framing

::left::

<Person 
  name="Ada Lovelace" 
  role="Computing Pioneer & Mathematician" 
  company="Analytical Engine"
  social="@adalovelace"
  size="lg"
>
Published the first algorithm intended to be executed by a general-purpose computer.
</Person>

<div class="mt-3">
  <Person 
    name="Claude Shannon" 
    role="Father of Information Theory" 
    company="Bell Laboratories"
    social="@shannon"
    size="md"
  />
</div>

::right::

<Figure 
  src="https://images.unsplash.com/photo-1550751827-4bd374c3f58b?auto=format&fit=crop&w=800&q=80" 
  caption="Logic circuitry and memory architecture analysis" 
  fig="FIG 2.4"
  credit="MIT ARCHIVES"
/>

---
layout: quote
---

# Design is not just what it looks like and feels like. Design is how it works.

::author::
Steve Jobs
::

---
layout: fact
---

# 99.9%

Reliable per-slide page navigation and SSR hydration safety.

---
layout: end
---

# Thank You!

Questions & Discussion
