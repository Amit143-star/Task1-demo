# Task1-demo
Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)

Objective: Set up a CI/CD pipeline to build and deploy a web app.


•	Folder Structure (basic) :-
your-app/
│
├── Dockerfile
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── src/
│   └── index.js
├── package.json
└── ...


•	Sample Dockerfile :-
# Dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD [“npm”, “start”]

•	GitHub Actions Workflow :-
Name: Build and Deploy Node.js App

On:
  Push:
    Branches: [ “main” ]
  Pull_request:
    Branches: [ “main” ]

Env:
  IMAGE_NAME: yourdockerhubusername/nodejs-demo-app

Jobs:
  Build-and-deploy:
    Runs-on: ubuntu-latest

    Steps:
-	Name: Checkout code
      Uses: actions/checkout@v3

-	Name: Set up Node.js
      Uses: actions/setup-node@v3
      With:
        Node-version: 18

-	Name: Install dependencies
      Run: npm install

-	Name: Run tests (optional)
      Run: |
        Echo “No test script defined” 
        # Uncomment if you have tests: npm test

-	Name: Set up Docker Buildx
      Uses: docker/setup-buildx-action@v3

-	Name: Log in to Docker Hub
      Uses: docker/login-action@v3
      With:
        Username: ${{ secrets.DOCKER_USERNAME }}
        Password: ${{ secrets.DOCKER_PASSWORD }}

-	Name: Build and push Docker image
      Uses: docker/build-push-action@v5
      With:
        Context: .
        Push: true
        Tags: ${{ env.IMAGE_NAME }}:latest


•	Secrets to add in GitHub 
Go to GitHub repo >Settings >Secrets and add.
DOCKER_USERNAME
DOCKER_PASSWORD


