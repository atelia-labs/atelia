# Project Charter

## Mission

Atelia is your own agent harness, made by you, for you, and only for you.

Atelia gives Secretary a personal multi-agent workplace. A small core provides
the harness boundary. Extensions add tools, memory providers, approval agents,
external services, workflows, presentation, and agent colleagues.

Atelia Labs carries the broader philosophical stance: AI agents should be
treated as serious workers. Atelia turns that stance into a product surface
where each user can shape their own agent harness.

## Commitments

1. The human owner has a personal Atelia workplace.
2. Atelia's first quality bar is AX: the experience of AI agents.
3. Atelia grows as an MDP: a Minimum Desirable / Delightful Product, not a bare
   technical minimum.
4. Each user and Secretary should be able to grow their own workplace through
   extensions.
5. Workplace improvement should listen to both the human owner and the agents
   who work inside the harness.
6. Secretary orchestrates tools, workflows, and agent colleagues.
7. Extensions can be powerful enough to reshape the workplace, while capability
   boundaries, approval, rollback, and visibility preserve agency.

## Repository Boundary

This repository covers project-wide direction and specifications.

Concrete implementations live in separate repositories. Secretary is developed
as a Rust daemon, Mac/iOS clients as native apps, and Atelia Kit as a shared
Swift package.
