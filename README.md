# Deploy Marker Demo

A Hello World Mule app that shows how a CD pipeline can stamp each deployment with a marker you can find in Anypoint Monitoring.

On every deploy, GitHub Actions passes three values to the app: **version**, **commit**, and **actor**. When the app starts, it writes one log line:

```
DEPLOY_MARKER version=1.7.1 commit=a1b2c3d actor=jdoe
```

`GET /hello` returns `{"message": "Hello World", "version": "1.7.1"}`.

## Setup (one time)

1. In Anypoint, go to **Access Management → Connected Apps → Create app**, choose **App acts on its own behalf (client credentials)**, and add these scopes for your business group and environment: Exchange Contributor, Create Applications, Read Applications, Delete Applications, Download Applications, View Environment, View Organization.
2. Push the project to a GitHub repo.
3. In the repo, go to **Settings → Secrets and variables → Actions** and add:
   - Secrets: `ANYPOINT_CLIENT_ID`, `ANYPOINT_CLIENT_SECRET`, `ANYPOINT_BUSINESS_GROUP_ID`
   - Variables: `ANYPOINT_ENVIRONMENT` (e.g. `Sandbox`), `ANYPOINT_TARGET` (e.g. `Cloudhub-US-East-2`)

## Demo

1. Show `.github/workflows/deploy.yml`. The `-D` flags are the marker.
2. Make a small change, push it to `main` (or click **Run workflow**), and wait about 3 minutes.
3. Open **Anypoint Monitoring → Log Search** and search for `DEPLOY_MARKER`. Each deploy shows up as one line with its version, commit, and actor.
4. Open **Runtime Manager → deploy-marker-demo → Properties**. The same values appear there.
5. Call `/hello`. The response shows the version that is currently live.

The point to land: any error that appears after a marker's timestamp can be traced to that release.
