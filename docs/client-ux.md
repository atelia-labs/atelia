# Client UX

Atelia Mac and Atelia iOS are not mere chat clients. They are the interfaces for
humans to operate Atelia and collaborate with Secretary and other agents.
They let humans see projects, threads, workspaces, agent work, memory, Hooks,
automations, Custom Extensions, diffs, approval waits, in-progress work, and
policy, then act, approve, or intervene when needed.

## What Clients Enable

- organize agent work by project and thread;
- switch naturally between parallel agents;
- use worktrees or isolated workspaces so experiments do not dirty repository
  state;
- handle branches, worktrees, diffs, commits, pushes, and PRs naturally;
- review diffs, decision logs, approval waits, and execution results in the same
  surface;
- review risks, omissions, missing tests, and design concerns through automatic
  review by Secretary or external reviewers;
- use multiple files, multiple terminals, SSH devboxes, and an in-app browser in
  the same workplace;
- connect the in-app terminal naturally with Git operations;
- keep CLI / IDE / app / cloud history and configuration connected;
- create, install, and manage plugins, skills, automations, and extensions;
- inspect and manage Hooks, external events, policies, permissions, and audit
  logs;
- use a review queue so humans can return at the right moment;
- use voice operation for task requests, review requests, status checks,
  approvals, and setting changes.

Git UX is central to Atelia. Humans and Secretary should be able to understand,
in the same surface, which branch / worktree holds which agent's changes, which
diffs have been reviewed, which are waiting for approval, and which should be
accepted.

GitHub integration is an important path. Issues, pull requests, reviews,
discussions, and release notes should be reachable from both the daemon and the
clients.

Browser use and computer use are advanced capabilities and are not initial
requirements.
