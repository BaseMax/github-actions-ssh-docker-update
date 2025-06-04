# github-actions-ssh-docker-update

```
name: Deploy via SSH (Password Auth)

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Install sshpass
        run: sudo apt-get update && sudo apt-get install -y sshpass

      - name: Deploy to Server via SSH
        env:
          SSHPASS: ${{ secrets.SSH_PASSWORD }}
        run: |
          sshpass -e ssh -p ${{ secrets.SSH_PORT }} -o StrictHostKeyChecking=no ${{ secrets.SSH_USERNAME }}@${{ secrets.SSH_HOST }} << EOF
            cd ${{ secrets.PROJECT_DIRECTORY }}
            git pull
            docker compose up --build -d
          EOF
```
