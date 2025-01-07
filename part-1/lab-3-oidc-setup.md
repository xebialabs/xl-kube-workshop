
# Lab 3 - OIDC setup (use Identity service)

## Configure authentication on Digital.ai Platform

In this step, we will create an OIDC client that will allow Digital.ai Release to use the authentication service of Digital.ai Platform

### 1. Obtain access to the staging platform

Contact your Digital.ai representative for access credentials

### Create the OIDC client

Go to **Clients** and press the **Add OIDC Client** button.

Navigate to **Applications** and click **Create application**:

1. Choose Release as application type
2. Give a unique instance name
3. Provide the url you use to access your application (example: http://release-ns-yourname:56300)
4. On **Advanced configuration** click next
5. On **Mappers** click next
6. Now you can copy the needed data from the **Release Configuration File** on the page

Open a text editor with the snippet below and paste the values in there. Also add the Release URL 

```yaml
oidc:
  enabled: true
  clientId: "[[YOUR CLIENT ID HERE]]"
  clientSecret: "[[YOUR CLIENT SECRET HERE]]"
  issuer: "https://identity-01.staging.digital.ai/auth/realms/[[YOUR REALM HERE]]"
  redirectUri: "[[RELEASE URL HERE]]/oidc-login"
  postLogoutRedirectUri: "[[RELEASE URL HERE]]/oidc-login"
  rolesClaim: groups
  userNameClaim: preferred_username
  scopes: ["openid"]
```

XXX CHANGE: paste this into the `dair-release_cr.yaml` directly and then edit 

## Configure Release with OIDC configuration

Open the `digitalai/dai-release/ns-yourname/20221031-131244/kubernetes/dai-release_cr.yaml`. 

For your path: check the installation log of the upgrade you did in lab-2.

Find the `oidc` section. It should look like this:

```text
  oidc:
    enabled: false
```

Now replace it with the snippet you created above and save the file. 

We use the `kubectl` utility to apply the changes directly into Kubernetes. 

```shell
kubectl apply -n ns-yourname -f digitalai/dai-release/ns-yourname/20221031-131244/kubernetes/dai-release_cr.yaml
```

The changes will not be applied until the Release server restarts. Open the **k9s** utility and see if the `dai-xlr-ns-yourname-digitalai-release-X` pods are restarting. 

When everything has restarted, go to RELEASE URL.

You should be logged in through SSO as `admin-devops-demo.digital.ai`, the same user as you used to log in to Digital.ai Platform!

---

[Next](../part-1/lab-4-clean-release.md)
