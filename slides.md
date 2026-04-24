---
theme: nws
# some information about your slides (markdown enabled)
title: Real-Time Threat Detection with Falco
info: |
  ## stackconf 2026
  April 28, 2026
---

# Real-Time Threat Detection with Falco

stackconf 2026, Munich

April 28, 2026

---
layout: section
---

# Why do we need Falco?


---
layout: two-cols
---

# Who are we?

::left::

NETWAYS Web Services offers:

- Public Cloud based on OpenStack
  - Compute
  - Networking
  - Block/Object Storage
  - Dedicated CPUs/GPUs
- Managed Kubernetes®
- Managed AI Models
- Managed apps and individual solutions

::right::

<img src="/nws-badges-new.png" alt="A collection of NWS badges, e.g. ISO27001, KCSP, KCSC, OpenInfra and CNCF member" class="w-96 m-x-auto"/>

---

# NETWAYS Cloud in Numbers

## What are we dealing with?

- 2 data centers (colocated)
- ~1300 CPU Cores
- ~1.4PB Ceph-Cluster with ~340 OSDs
- GPUs (A10, A40, RTX 6000 Blackwell)
- ~2000 client VMs
- ~60 client Kubernetes Cluster

---
layout: quote
---

# Networks, systems and applications shall be monitored for anomalous behaviour and appropriate actions taken to evaluate potential information security incidents.

## ISO/IEC 27001:2022, Annex A, 8.16 Monitoring Activities

---
layout: section
---

# Falco at a Glance

## Runtime security for hosts, containers, Kubernetes, and cloud environments

---
layout: two-cols
---

# What is Falco?

::left::

- Software for **real-time detection of events**
- Includes baseline ruleset
- Integratable on multiple levels:
  - Host (Bare Metal or VM)
  - Container (Docker, Kubernetes, Nomad)
- Integrations for external systems, e.g.
  - Hyperscalers (AWS, GCP, Azure, RedHat)
  - Observability platforms (Grafana, Datadog)
  - IAM (e.g. Okta)
- Multiple architectures allow usage even in legacy environments

::right::

<div class="flex flex-col justify-center h-full">
  <img src="/falco.svg" alt="Falco logo" class="w-64 m-x-auto"/>
</div>
---

# Falco's Architecture

<img src="/falco-architecture.svg" alt="Figure displaying Falco's architecture" class="w-full m-t-12"/>

---
layout: two-cols
---

# Falco Rules

::left::

```yaml
- rule: shell_in_container
  desc: notice shell activity within a container
  condition: >
    evt.type = execve and 
    evt.dir = < and 
    container.id != host and 
    (proc.name = bash or
     proc.name = ksh)    
  output: >
    shell in a container |
    user=%user.name
    container_id=%container.id
    container_name=%container.name 
    shell=%proc.name parent=%proc.pname
    cmdline=%proc.cmdline    
  priority: WARNING
```

::right::

- YAML objects with required and optional keys
- `condition` can consist of macros and lists
- `output` can access metadata of observed events

---

# Falco in the Kernel

Falco watches **syscalls** and **kernel functions** on monitored endpoints or events using one of three *drivers*:

<br/>

<div class="grid grid-cols-3">
<div class="border-r-1 p-x-2">

### 🥉 Kernel Module

- from kernel version:
  - **x86 systems**: >= 2.6
  - **aarch64 systems**:<br/> >= 3.4
- needs to be permanently installed on the host; isn't loaded by Falco.

</div>

<div class="p-x-2">

### 🥇 Modern eBPF Probe

- from kernel version: >= 5.8
- less overhead than the kernel module, bundled by Falco
- Falco loads the eBPF probe on demand
- [*Least Privileged Mode*](https://falco.org/docs/concepts/event-sources/kernel/#least-privileged-mode-1) configurable

</div>

<div class="border-l-1 p-x-2">

### 🥈 Legacy eBPF Probe

- from kernel version:
  - **x86 systems**: >= 4.14
  - **aarch64 systems**:<br/> >= 4.17
- behavior analogous to the *Modern eBPF Probe*

</div>

</div>

<style>
  .slidev-layout h1 + p {
    opacity: 1.0;
  }

  .slidev-layout li {
    line-height: 1.4em;
  }
</style>

---

# Userspace vs. Kernelspace

<img src="/falco-kernel-architecture.svg" alt="Figure explaining Falco's negotiation between user- and kernel-space" />

<div class="text-gray">

Figure taken from the [Falco documentation](https://falco.org/docs/concepts/event-sources/kernel/architecture/#how-falco-interacts-with-kernel-components).

</div>

---
layout: section
---

# Intermission: What is eBPF?

---

# eBPF

## Secure, privileged programs in the OS kernel

- Evolution of BPF (_Berkeley Packet Filters_)
- Allow **extending** OS kernel capabilities **at runtime**
- **Sandboxed** and **JIT-compiled** for high performance and security
- **Event-based** and bound to _hooks_ (syscalls, tracepoints, etc.)
- more and more adoption for **networking, security**, and **observability**

---

# eBPF at a Glance

<img src="/ebpf-explained.png" alt="Overview of eBPF's applications and use-cases" class="w-180 m-x-auto m-t-4"/>

<div class="text-gray p-l-20">

Figure taken from [ebpf.io](https://ebpf.io/what-is-ebpf/#what-is-ebpf).

</div>

---

# The eBPF Lifecycle

## Heavily simplified

1. Identification of the desired hooks (e.g. `execve` syscall)
2. Loading of the eBPF probe in its desired place
3. Verification of correctness and security of the eBPF probe:
    - _Is the executing process sufficiently privileged?_
    - _Is the size of the eBPF probe within the configured limits?_
    - _Is the complexity of the eBPF probe sufficiently low?_
    - _Can the memory safety of the eBPF probe be guaranteed?_
    - _Does the eBPF probe terminate deterministically?_
4. Compilation of the eBPF probe to bytecode
5. Execution of the eBPF probe in its desired place (_hook point, kprobe_, or _uprobe_)

---
layout: section
---

# Falco@NETWAYS

---
layout: two-cols
---

# Falco@NETWAYS

## What's the scenario?

::left::

- OpenStack components as containers from the [OpenStack Kolla project](https://github.com/openstack/kolla)
- Patches etc. via GitLab CI
- Configuration via Puppet
- Deployment to hypervisors (baremetal) via GitLab CI
- No management layer underneath OpenStack (Kubernetes, Yaook, etc.)

::right::

<div class="m-l-4 h-full flex justify-center items-center">
  <img src="/nms-os.svg" alt="OpenStack setup at NMS" />
</div>

<style>
  h2 {
    margin-bottom: 1rem;
  }
</style>

---
layout: two-cols
---

# Falco@NETWAYS

## Our Falco Setup

::left::

- Deployment with Docker
- Baseline ruleset with few additions
- No Falco Sidekick or Talon
- Evens are mainly needed for audits:
  - Scraped by FluentBit
  - Written to OpenSearch
  - No alarming or (automatic) reactions

::right::

<div class="m-l-4 h-full flex justify-center items-center">
  <img src="/nms-os-current.svg" alt="OpenStack falco setup at NMS" />
</div>

<style>
  h2 {
    margin-bottom: 1rem;
  }
</style>

---
layout: two-cols
---

# Falco@NETWAYS

## Planned Next Steps

::left::

- Processing of events by Falco Sidekick
  - Grafana IRM
  - On-Call Pager
  - OpenSearch
- Evaluation of Falco Talons development
  - Prevention/Termination of suspicious processes (similar to Tetragon)
  - _Quarantine mode_
  - Recording of shell/container sessions

::right::

<div class="m-l-4 h-full flex justify-center items-center">
  <img class="w-90" src="/nms-os-future.svg" alt="Future OpenStack falco setup at NMS" />
</div>

<style>
  h2 {
    margin-bottom: 1rem;
  }
</style>

---

# Falco@NETWAYS

## Our Evaluation of Falco vs Tetragon

| | **Falco** | **Tetragon** |
|:--- |----|----|
|Technology | Kernel Module/eBPF | eBPF |
| Management in Kubernetes | Daemonset + Sidecar | Daemonset + Operator |
| Configuration | Rulefiles + Baseline Ruleset | CRDs + Collection of Examples |
| Learning Curve | **Flat** | **Steep** |
| Features | **Sufficient** (Eventsourcing, simple reactions) | **Extensive** (Eventsourcing with custom hooks, reactions on process level) |

---
layout: section
---

# 💻 Demo

---

# Summary and Takeaways

<ul>
 <li v-click>Falco provides excellent ways to get started:

   - Baseline of available rules
   - Abstracts concepts from the kernel, human readable
   - Integrations for various event sources and destinations

  </li>
  <li v-click>Falco is a silent observer:

  - Events are protocolled and forwarded
  - Falco Talon isn't completely mature/stable (yet)

  </li>
  <li v-click>There are tradeoffs:

  - Abstraction of rules vs. complete control on the kernel level
  - Slim deployments vs. extensive management layer
  - All-in-One vs. plugins and specializations

  </li>
</ul>

---

# Thank You

## For Your Attention

📊 **Slides**: [slides.dbodky.me/stackconf-munich-2026](https://slides.dbodky.me/stackconf-munich-2026)

💻 **Demos**: [netways-web-services/falco-k8s-demo](https://github.com/netways-web-services/falco-k8s-demo)

📚 **Falco Website**: [falco.org](https://falco.org)

👍 **Feedback**: [feedback.dbodky.me](https://feedback.dbodky.me) 

<div class="m-l-6 m-t--2" >

  Code: `FALC-OMUC`

</div>
