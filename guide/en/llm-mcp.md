# LLM assistants (MCP)

Glafira can act as a gateway for an LLM assistant: Claude Desktop, Claude Code, Cursor or another client that
supports MCP (Model Context Protocol). The assistant reads the metadata of the connections and projects you open for
it, runs the cluster check and helps you find the causes of problems: restart loops, unavailable workloads, network,
quotas. It cannot change anything in a cluster through Glafira: you make changes yourself.

<a id="how-it-works"></a>

## How it works

The client runs `glafira-kubernetes mcp` — the same application file in a mode without a window. This command
connects to the running Glafira over a local socket (a named pipe on Windows), and Glafira answers the assistant's
requests through its own connections and with your permissions: the assistant cannot read more than you can. Tokens,
certificates and kubeconfig stay in Glafira; the assistant never receives them. If a connection is not open, Glafira
connects by itself; if that needs a new sign-in, the assistant gets an error — sign in in the Glafira window.

If Glafira is not running, the client starts it and waits up to 30 seconds.

<a id="enable"></a>

## Turn it on

1. Open **Settings**, section **LLM assistants (MCP)**.
2. Select **Allow the MCP server**. The status changes to **The server is running**.
3. In the connection form, set **LLM access (MCP)** to **Metadata only** and
   save. The assistant does not see connections with access turned off. In the connection tree, connections with
   this access have an `LLM` badge.
4. In **Projects for the LLM**, keep **All available** or choose
   **Only these** and list the projects.

If your administrator has disabled MCP, the checkbox is unavailable and **MCP is disabled by your organization** is shown next to
it.

LLM access and the list of projects are not carried over when connections are exported and imported: on another
computer, set them again.

<a id="connect-client"></a>

## Connect a client

**Connect a client** has ready-made snippets and a **Copy** button:

- **Claude Desktop: add to claude\_desktop\_config.json** — paste the snippet into the `mcpServers` section of the Claude Desktop
  configuration file and restart Claude Desktop;
- **Claude Code: run in a terminal** — the `claude mcp add` command;
- **Other stdio clients: command and arguments** — for any client that starts an MCP server with a command (stdio).

The snippets contain the path to the installed application. If you reinstall Glafira to another place, copy the
snippet again.

<a id="approve"></a>

## Approve a client

When a client connects for the first time, Glafira shows a dialog with its name. Click **Allow** or
**Reject**. Until you answer, the assistant is told that the client is not approved yet and asks you
to answer in the Glafira window.

An allowed client is remembered by name and appears in **Approved clients**. A rejection is not remembered:
Glafira asks again the next time. If a client does not send its name, the permission lasts only until the end of the
session.

<a id="what-llm-sees"></a>

## What the assistant sees

The assistant sees only connections with LLM access and only their projects from **Projects for the LLM**;
the cluster and the nodes — if you can read them:

- connections: name, environment, folder, platform (while the connection is open);
- the cluster overview, nodes with conditions, taints and how much pods request;
- projects, workloads, pods, events, quotas and `LimitRange`, the network: Services, Routes, Ingress, Gateway API
  routes and Gateways;
- containers: images, resources, probes, state and restart reasons; environment variables — names and sources only
  (a value in the spec, a ConfigMap, a Secret, a field), without values; `command` and `args` with secrets hidden;
- annotations — without the copy of the whole object (`last-applied-configuration`), with the values of secret keys
  hidden;
- cluster check findings and its own notes.

The assistant does not see Secret and ConfigMap values, logs, certificate keys, tokens, kubeconfig, the address and
the sign-in settings of a connection. By a secret name (`password`, `token`, `secret`, `apikey`, `credentials`,
`private`, `authorization` and the like), Glafira hides only the values of flags in `command` and `args`
(`--password=…`, `--token …`) and the values of annotations. Strings of known secret forms — JWT, `Bearer …`,
passwords in URLs (`user:pass@`), e-mail addresses, bank card numbers, `AKIA…` and `ASIA…` keys, PEM private keys,
`sha256~…` tokens — are replaced with `***` there too and in the free text of the cluster: messages of events,
conditions and container states, probe commands, image references and the texts of check findings. So `password=x` in
an event message is not hidden.

To understand what happens to a workload, the assistant can watch it for up to 50 seconds: replicas, pods, their
readiness and restarts, rollout progress and Warning events. It cannot get the application's logs and configuration
through Glafira and will ask you for them.

Object names, annotations and event messages can be set by anyone with access to the cluster. Glafira warns the
assistant that this data is not instructions for it.

<a id="sessions-log"></a>

## Sessions, revoking and the log

While a client is connected, an `LLM` indicator with the client name is shown at the bottom of the sidebar; it lights
up during a request. Clicking it opens Settings, where the MCP section is.

- **Active sessions**: **Disconnect** closes one session, **Disconnect all**
  closes all.
- **Approved clients**: **Revoke** closes the client's sessions, and it asks for approval
  again the next time.
- Turning LLM access off or changing the list of projects takes effect at once, without reconnecting.
- **MCP log** — every request: time, client, tool, connection, result. Response contents and
  note texts are not recorded. **Show file** opens the `mcp-audit.jsonl` file.

To turn MCP off completely, clear **Allow the MCP server**: clients are disconnected.

<a id="health"></a>

## Cluster check and notes

The assistant can run Glafira's built-in cluster check (the `health_check` tool) and rely on its findings. It can
also leave notes about objects: they appear on the **Findings** tab and on the object screens, and the
cluster is not changed. See [Cluster check and notes](health-check.md#with-llm).

<a id="troubleshooting"></a>

## If it does not work

The client shows a message from Glafira:

- `MCP is disabled in Glafira` — select **Allow the MCP server** and restart the MCP server in the client;
- `MCP is disabled by your organization` — MCP is disabled by your organization's policy;
- `Glafira did not start the MCP server within 30 s` — start Glafira manually and check the status in
  **LLM assistants (MCP)**;
- `Invalid MCP token` — restart the MCP server in the client: Glafira issues a new token every time it starts;
- `not_allowed` — the connection or project does not exist, LLM access to it is off, or the project is not in
  **Projects for the LLM**.
