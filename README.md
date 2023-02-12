# How to build a CI/CD pipeline with GitHub Actions in four simple steps

GitHub Actions has made it easier than ever to build a secure continuous integration and continuous delivery (CI/CD) pipeline for your GitHub projects. By integrating your CI/CD pipeline and GitHub repository, GitHub Actions allows you to automate your build, test, and deployment pipeline.


### Setting up a Github Actions CI pipeline

GitHub Actions workflows are YAML files in the .github/workflows folder. If you do not have a workflow, or want to add a new one, select Actions and New workflow.
![image](https://user-images.githubusercontent.com/71556060/218312062-cef4b435-2e6f-4009-a628-dff11c3b6f42.png)

[https://docs.github.com/en/actions/automating-builds-and-tests](Automating builds and tests) You can automatically build and test your projects with GitHub Actions. 

```
name: react/Vue js production-pipline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: self-hosted


    steps:
    
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'

    - name: CASE -1 INSTALL NPM
      run: npm i
      
    - name: CASE -2 MAKE BUILD
      run: npm run build --if-present
      
    - name: CASE -3 RUN THE NEW ENV
      run: npm test --if-present
      
```
This action will run on every push or pull request on the main branch.

### Configuring the self-hosted runner application as a service
You can configure the self-hosted runner application as a service to automatically start the runner application when the machine starts.

**Installing the service**
> Create a folder
```
mkdir actions-runner && cd actions-runner
```
> Download the latest runner package
```
curl -o actions-runner-osx-x64-2.301.1.tar.gz -L https://github.com/actions/runner/releases/download/v2.301.1/actions-runner-osx-x64-2.301.1.tar.gz
```
> Optional: Validate the hash
```
echo "3exxxxxxxxxxxxxxxxxxxxxx10ac6a75b878  actions-runner-osx-x64-2.301.1.tar.gz" | shasum -a 256 -c
```
> Extract the installer
```
tar xzf ./actions-runner-osx-x64-2.301.1.tar.gz
```
**Configure the service in agent**
> Create the runner and start the configuration experience
```
./config.sh --url https://github.com/SyedAsadRazaDevops/<REPO NAME> --token <xxxxxxxxxxxxxxx>
```
"Must not run with sudo while trying to create a runner using github-actions"
![1502 (1)](https://user-images.githubusercontent.com/71556060/218314246-beaac109-691e-4deb-a362-972c4230a73f.png)

SOLUTION:
- Export it before running config.sh using export RUNNER_ALLOW_RUNASROOT="1"
- Start config.sh like this : `RUNNER_ALLOW_RUNASROOT="1" ./config.sh --url...`

**Last step, run it!**
```
./run.sh
```
OR run it as service
```
sudo ./svc.sh install <enter-your-user>
```
After you push or merge to main branch, you can check the build under the Actions tab

<img width="1395" alt="Screenshot 2023-02-12 at 6 54 40 PM" src="https://user-images.githubusercontent.com/71556060/218315198-17cd6613-dc7e-4886-95bd-7ff855de5581.png">





