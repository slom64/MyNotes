the `multi-relay` feature of `ntlmrelayx` allows for:
- Identifying the users we receive `NTLM` authentication from (allowing us to decide whether we want to relay their `NTLM` authentication).
- Relaying a single `NTLM` authentication (connection) to multiple targets.

`ntlmrelayx` implements the `multi-relay` feature by making the clients authenticate on the attack machine locally, fetch/extract their identities, and then force them to reauthenticate so that it can relay their connections to the relay targets. `Multi-relaying` is the default behavior for the HTTP and SMB servers of `ntlmrelayx`; however, to deactivate it, we can use the `--no-multirelay` option, making it relay the connection(s) only once (i.e., one incoming connection maps to only one attack).

### Target Definition

We already mentioned that `ntlmrelayx` provides the options `-t` and `-tf` for specifying relay targets. Because of the `multi-relay` feature, targets can be either `named targets` or `general targets`; `named targets` are targets with their identity specified, while `general targets` are targets without identity. Defining targets follows the [URI format](https://datatracker.ietf.org/doc/html/rfc3986), with its syntax consisting of three components: `scheme://authority/path`:

- `scheme`: Defines the targeted protocol (e.g., `http` or `ldap`); if not supplied, `smb` is used as the default protocol. The wildcard keyword `all` makes `ntlmrelayx` use all the protocols it supports.
- `authority`: Specified using the format `DOMAIN_NAME\\USERNAME@HOST:PORT`. `General targets` do not use `DOMAIN_NAME\\USERNAME`; only `named targets` do.
- `path`: Optional and only required for specific attacks, such as when accessing access-restricted web endpoints using a relayed `HTTP NTLM` authentication (as we will cover in the `Advanced NTLM Relay Attacks Targeting AD CS` section).

The HTTP and SMB servers of `ntlmrelyax` have the `multi-relaying` feature enabled by default, except when attacking a single `general target`. Depending on the `target type`, `ntlmrelayx` has the following default settings for the `multi-relay` feature:

  
|**Target Type**|**Example**|**Multi-relaying Default Status**|
|---|---|---|
|`Single General Target`|`-t 172.16.117.50`|`Disabled`|
|`Single Named Target`|`-t smb://INLANEFREIGHT\\[email protected]`|`Enabled`|
|`Multiple Targets`|`-tf relayTargets.txt`|`Enabled`|

Understanding the `multi-relay` feature and the different `target types` of `ntlmrelayx` is paramount; let us see why by showing examples of different `target types` and their default `multi-relaying` status.

### Single General Target
Suppose we instruct `ntlmrelayx` to use the target ” `smb://172.16.117.50`“; in this case, since it is a `general target`, `multi-relay` will be `disabled`, and `ntlmrelayx` will relay only the first `NTLM` authentication connection belonging to any user (from any host) to the relay target 172.16.117.50 over SMB. This relationship is `1:1`, as `one` connection maps to only `one` attack (an edge case is whereby `ntlmrelayx` receives two different connections at the same time; although `multi-relay` will be disabled, `ntlmrelayx` incorrectly relays the two connections instead of rejecting either of them):
```
ntlmrelayx.py -t smb://172.16.117.50
```

### Single Named Target
Alternatively, suppose we instruct `ntlmrelayx` to use the target ” `smb://INLANEFREIGHT\\[email protected]`“; in this case, since it is a single `named target`, `multi-relay` will be `enabled`, and `ntlmrelayx` will relay any number of `NTLM` authentication connections belonging to `INLANEFREIGHT\PETER` (from any host) to the relay target 172.16.117.50 over SMB (we must supply the domain name and username precisely as shown in `ntlmrelayx`’s output). This relationship is `M:M`, as `many` connections map to `many` attacks:
```
ntlmrelayx.py -t smb://INLANEFREIGHT\\[email protected]
```

### Multiple Targets
What if we want to use the same `general target` ” `smb://172.16.117.50`” but we also want to enable `multi-relaying`? To do so, we must put the target in a file and use the `-tf` option, which enables `multi-relaying` by default. Therefore, regardless of the file containing a `general target`, because `multi-relaying` is enabled due to the `-tf` option, `ntlmrelayx` will relay any number of `NTLM` authentication connections belonging to any user (from any host) to the relay target 172.16.117.50 over SMB. Instead of having a `1:1` relationship like `general targets`, this becomes `M:M`, as `many` connections map to `many` attacks:
```
cat relayTarget.txt

smb://172.16.117.50
ntlmrelayx.py -tf relayTarget.txt
```

At last, suppose we want use the same `named target` ” `smb://INLANEFREIGHT\\[email protected]`“, but only want to abuse the first connection `ntlmrelayx` can relay for the user `INLANEFREIGHT\PETER`; to do so, we can disable `multi-relay` with the `--no-multirelay` option:

```
ntlmrelayx.py -t smb://INLANEFREIGHT\\[email protected] --no-multirelay
```


> [!NOTE] 
>  Throughout this module, we will assume that services are running on their default ports. However, always remember that system administrators can change these default ports, so if we come against such cases after enumerating the network, we need to change the ports of services accordingly.
