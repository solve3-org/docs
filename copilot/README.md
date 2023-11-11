```bash

copilot app init --domain solve3.org

copilot init --app web --type "Static Site" --name docs

copilot env init --name prod && copilot env deploy --name prod

copilot svc deploy --name docs --env prod
```