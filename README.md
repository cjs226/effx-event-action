# effx-event-action 🔄

GitHub action for sending event data to [effx](https://www.effx.com)

### Usage

```
name: effx events
on: 
  pull_request:
    types: opened
  push:
    branches: master

jobs:
  build:
    name: "Create effx event"
    runs-on: ubuntu-latest
    env:
      EFFX_API_KEY: ${{ secrets.EFFX_API_KEY }}
      EFFX_SERVICE_NAME: Foo Service
    
    steps:
      - uses: actions/checkout@master

      # Pull requests being opened
      - name: Send pull requests to effx
        if: ${{ github.event_name == 'pull_request' }}
        uses: cjs226/effx-event-action@master
        with:
          name: "Pull request submitted by ${{ github.event.pull_request.user.login }}"
          desc: "${{ github.event.pull_request.title }} --> ${{ github.event.pull_request.html_url }}"
          service: ${{ env.EFFX_SERVICE_NAME }}

      # Commits to master
      - name: Send commits to master to effx
        if: ${{ github.event_name == 'push' && github.ref == 'refs/heads/master' }}
        uses: cjs226/effx-event-action@master
        with:
          name: "Push to master branch by ${{ github.event.head_commit.author.name }}"
          desc: "${{ github.event.head_commit.message }} --> ${{ github.event.head_commit.url }}"
          service: ${{ env.EFFX_SERVICE_NAME }}
```

### Environment Variables

🔑 **`EFFX_API_KEY`** _(required)_ - Your effx API key.

### Workflow

1. Add this action to your repo.
2. Head over to the **[effx website](https://app.effx.com/account_settings)**, navigate to **Account Settings** and find your API key.
3. Add your effx API key by
   - navigating to your repo's settings
   - selecting **Secrets** from the side bar
   - typing **`EFFX_API_KEY`** into the name field
   - pasting your effx API key into the value field
4. The **`yml`** file for **`effx-event-action`** will live in **`.github/workflows`**.
