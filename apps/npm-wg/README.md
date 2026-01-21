## Purpose

This app requires a Kubernetes Secret in the same namespace as the app. The StatefulSet and containers reference it for WireGuard configuration and database credentials. Without this Secret, the pod will fail to start.

## Required Secret

- Name: npm-wg-secret
- Type: Opaque
- Namespace: must match the namespace where the app is deployed

### Required keys
- wg0.conf: Full WireGuard interface configuration (mounted at `/etc/wireguard/wg0.conf`).
- db-host: MySQL host (e.g., `host or ip of your database`).
- db-port: MySQL port (e.g., `3306`).
- db-name: Database name (e.g., `npm`).
- db-user: Database user.
- db-password: Database password.

The Secret is referenced by the containers in [apps/npm-wg/server.yaml](apps/npm-wg/server.yaml) via a volume (for `wg0.conf`) and environment variables (for the DB settings).

## Example Secret manifest

Use this as a template and replace placeholder values before applying. Ensure you apply it to the same namespace as the app.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: npm-wg-secret
type: Opaque
stringData:
  wg0.conf: |
    [Interface]
    Address    = 10.200.0.2/24
    MTU        = 1420
    PrivateKey = REPLACE_WITH_LOCAL_PRIVATE_KEY

    [Peer]
    PublicKey = REPLACE_WITH_SERVER_PUBLIC_KEY
    Endpoint = SERVER_IP_OR_DNS:51820
    AllowedIPs = 10.200.0.1/32
    PersistentKeepalive = 25
  db-host: db.local
  db-port: "3306"
  db-name: npm
  db-user: npm
  db-password: CHANGE_ME
```

## Apply

If you keep a copy of the Secret manifest locally:

```powershell
kubectl -n <namespace> apply -f apps/npm-wg/secret.yaml
```

## Networking / Services

- This deployment is intended for home access with Multus. Because the pod will attach directly to a home network via Multus, a Kubernetes `Service` is not required or provided.
- If you are not using Multus and need cluster networking, you may add a `Service` definition and include it in [apps/npm-wg/kustomization.yaml](apps/npm-wg/kustomization.yaml).

## Notes
- If you deploy this app to multiple namespaces, create one `npm-wg-secret` per namespace with appropriate values.
- If using Flux, SealedSecrets, or External Secrets, keep the Secret name (`npm-wg-secret`) and keys identical to what [apps/npm-wg/server.yaml](apps/npm-wg/server.yaml) expects.
 - Adjust all IP addresses to match your environment:
   - In `wg0.conf`:
     - `Address`: set to your WireGuard client IP and subnet (e.g., `10.200.0.X/24`).
     - `Endpoint`: set to your WireGuard server public IP/DNS and port.
     - `AllowedIPs`: set to the correct remote peer address or networks you route (e.g., server peer `10.200.0.1/32` or your WG subnet).
   - Database host: set `db-host` to your cluster/service DNS or IP.
   - If you change WireGuard/network addresses, patch the ConfigMap `npm-wg-nginx-custom` so `set_real_ip_from` entries reflect your actual subnets. See [apps/npm-wg/config-map.yaml](apps/npm-wg/config-map.yaml) under `http.conf`.