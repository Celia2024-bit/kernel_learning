# Systems Engineering Notes

Writing to actually understand *why*, not just to memorize *how*.

I kept running into the same low-latency advice everywhere — "reduce context switches," "avoid false sharing," "isolate your interrupts" — and I could apply every rule on the list. What I couldn't do was explain *why* any of it worked. So I stopped collecting rules and started reading the kernel itself: how a context switch actually unfolds step by step, what a page fault really is under the hood, and — in a separate deep-dive — what happens between the instant a chip powers on and the moment Linux user space finally starts running.

These are the articles that came out of that process. Each one stands on its own, but the first five build on each other in order.

---

## 🔧 Series: Low-Latency Engineering

Five posts, one continuous thread — from "what is a context switch, really?" all the way to "here's the exact file to edit and when."

| #   | Title                                                                                                             | What it's about                                                                                                                                                                                                                                                                               |
| --- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [Why I Stopped Memorizing Rules and Started Reading the Kernel](docs/Low-Latency1_Why_context_switches_matter.md) | The foundation: user space vs. kernel space, `task_struct`, and the real difference between a **mode switch** and a **context switch** — including the one rule that ties every trigger (interrupts, syscalls, faults, traps) together.                                                       |
| 2   | [What Exactly Happens During a Context Switch?](docs/Low-Latency2_What_is_a_context_switches.md)                  | Zooming in on Post 1's general picture: a real-world room analogy, followed by the exact five-step sequence the kernel runs through — and why hardware interrupts get a built-in decision point (`TIF_NEED_RESCHED`) that syscalls and faults don't.                                          |
| 3   | [What Actually Causes a Page Fault?](docs/Low-Latency3_What-cause-a-page-fault.md)                                | Virtual addresses, physical addresses, pages, frames, the four-level page table walk, and why a **minor** page fault is nearly free while a **major** one drags your task through a full context switch and a trip to disk. NUMA makes a cameo at the end.                                    |
| 4   | [Putting It All Together — A Practical Playbook](docs/Low-Latency4_Practical-low-latency-playbook.md)             | From theory to technique: CPU affinity vs. isolation (explained with a room-and-lock analogy), `nohz_full`/`rcu_nocbs`/IRQ affinity, kernel bypass, busy-polling, huge pages, false-sharing-safe data structures, and why `PREEMPT_NONE` is the *right* choice once a core is truly isolated. |
| 5   | [A Phase-by-Phase Implementation Checklist](docs/Low-Latency5_Phased-implementation-checklist.md)                 | The same toolkit, reorganized around the question that actually matters when you sit down to build this: *when* do I do this, and *which file* do I touch? Image build → boot parameters → post-boot ops → application init → runtime hot loop.                                               |

**Recommended path:** read them in order — each post assumes you've absorbed the one before it. If you only have time for one, start with Post 1 for the concepts or jump straight to Post 5 for the checklist.

---

## ⚡ Systems Deep-Dive

| Title                                                                    | What it's about                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Bootloader and Kernel Handover](docs/Bootloader_and_Kernel_Handover.md) | What actually happens between the instant a chip powers on and the moment Linux finally hands control to user space — ROM code, SRAM, the SPL, U-Boot, and the kernel, with **control flow** (who's executing) drawn out separately from **data flow** (what's being copied where). Includes the counter-intuitive discovery that the bootloader doesn't read the Device Tree to know how to boot — it just packages everything up and hands it to the kernel, which does the reading itself. |

---

## Why This Format

Every post here follows the same rule: no diagram goes in without an explanation of what it's actually showing, and no explanation goes in without a diagram to anchor it. If you've ever felt like you could *apply* a piece of systems advice without being able to explain *why* it works, that's the exact gap these are trying to close — one mechanism at a time.

More deep-dives are on the way. Feel free to open an issue or reach out if something here doesn't add up — that's usually where the most interesting rabbit holes start.
