# Pods and logs

<a id="pods"></a>

## Pod list

The pods of a project or a workload show the status by the rules of `kubectl get pods` (`Running`,
`CrashLoopBackOff`, `Init:1/2`, `Completed`…), the ready containers, the restarts and when the last one was, the node,
the age, the QoS class and the owner. A native sidecar (an init container with `restartPolicy: Always`) counts with the
containers once it has started and does not hold the pod in `Init`. Known differences from `kubectl`: a pod waiting
for its scheduling gates shows `Pending`, not `SchedulingGated`; ephemeral containers are not shown; while a pod
initializes, its restarts do not include those of regular init containers. A problem pod — the image cannot be pulled, a container was
killed for memory (`OOMKilled`), restarts in a loop (`CrashLoopBackOff` or, between restarts, exited with an error), the
pod waits to be scheduled (`Pending`) or has failed — is highlighted; the problems-only filter leaves just them.

<a id="pod"></a>

## Pod tab

Click a pod to open its tab. **Overview** shows the phase, the conditions, the node, the IP, the QoS,
the owner (a click opens the workload) and every container, init containers too: the image and the digest that is
running, the state and the last state with the reason, the exit code and the time, the restarts, the probes, the
requests and limits and the environment variables — names and sources only (a value in the spec, a ConfigMap or
Secret key, a field), never the values. **Events** and YAML are there too. YAML shows the object
without `managedFields` and without the annotations that copy the whole object; the values of environment variables
with secret-looking names (PASSWORD, TOKEN, KEY…) are replaced by `***`, other values are shown.

<a id="logs"></a>

## Logs

**Logs** shows the log of a container: choose the **Container**, the **Previous run**, the
**Last lines** (500 by default) or where to start from (**Since**), and **Follow new lines**. The previous
run has already finished, so with **Previous run** following is off, and the option says why.
**Line timestamps** adds the time of each line. The **Filter** — a substring or a regular expression,
**Match case** if needed — runs in Glafira before the lines reach the window, and the counter shows how many
lines were shown and how many were read; it changes even when no new line matches.

The window keeps the last 100,000 lines; when more arrive, a note above the lines says how many older lines were
dropped. A line longer than 64 KB is cut and marked **(line truncated)**; long lines scroll sideways. Bytes that are
not UTF-8 are shown as replacement characters (�).

When the connection is lost while following, the status says so, and Glafira reconnects from the last line it has,
without repeats; it reports an error after a minute without success. A connection that brings no line for almost 5
minutes counts as lost (the computer slept, the network or VPN changed); when the container is just quiet, the stream
opens again without notice. When the credentials of an exec plugin have run out, Glafira runs the plugin again to
reconnect. An expired session, a TLS error or missing rights end the log at once. When the container stops or the pod
is deleted, the log ends.

**Save to file…** reads the log again with the same settings (without following) and saves the whole lines to a file.
The file appears only when the whole log is read (on macOS and Linux only you may read it); disconnecting stops the
saving, and a log that stops sending lines for longer than the request timeout ends it with an error.

<a id="session"></a>

## When the session expires

If the token expires while you read the cluster, the tabs show the error with **Sign in again**: it connects
again at once (an exec plugin runs again) and opens the connection tab; when that cannot sign you in, the tab shows how
to sign in again. After that the open tabs of the connection read their data again by themselves, and the logs and the
lists that update themselves start over.
