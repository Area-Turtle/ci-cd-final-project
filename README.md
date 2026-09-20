# CI/CD Tools and Practices Final Project - JavaScript/Node.js Version

This repository contains a Node.js/Express.js version of the counter service for the Final Project of the Coursera course **CI/CD Tools and Practices**.

ci-cd-final-project

## Features

- RESTful API for managing counters
- In-memory storage
- Comprehensive error handling
- Security middleware (Helmet, CORS)
- Logging middleware
- Full test coverage with Jest
- Docker support
- Health check endpoint

## API Endpoints

- `GET /` - Service information
- `GET /health` - Health check
- `GET /counters` - List all counters
- `POST /counters/:name` - Create a new counter
- `GET /counters/:name` - Read a specific counter
- `PUT /counters/:name` - Increment a counter
- `DELETE /counters/:name` - Delete a counter

## Setup

1. **Install dependencies:**
   ```bash
   npm install

## PVC - OpenShift ENV
- In the terminal, install the cleanup, eslint, and jest-test tasks by applying the tasks.yml file with kubectl apply -f .tekton/tasks.yml
   command.
- Open the OpenShift console from the lab environment.
- Create a PVC through terminal as mentioned in the previous lab or either from the Administrator perspective with
- storageclass: skills-network-learner
- select a PVC: oc-lab-pvc
- size: 1GB
- Create a new pipeline and a workspace called "output"
- Add the following redhat modules steps in this order:
   - "cleanup" >  source*: output
   - "git-clone" > url*: https://github.com/<Username>/ci-cd-final-project.git 
   - "eslint" linting > source* output
   - "jest-tests" > source* output
   - "buildah" task > IMAGE* $(params.build-image) | source* output | Parameter: Name*: build-image | Default value: image-registry.           openshift-image-registry.svc:5000/<SN_ICR_NAMESPACE>/tekton-lab:latest
- Add the final step of deploying the application to the lab openshift cluster using the "OpenShift client" task and the oc deploy command.
   " oc create deployment $(params.app-name) --image=$(params.build-image) --dry-run=client -o yaml | oc apply -f - "
   > Display name*: Deploy | Script*:  oc create deployment $(params.app-name) --image=$(params.build-image) --dry-run=client -o yaml | oc apply -f - | Parameters: Name: app-name | Default value: ci-cd
- Start Pipeline: app-name: ci-cd | build-image: image-registry.openshift-image-registry.svc:5000/<SN_ICR_NAMESPACE>/tekton-lab:latest |  Workspaces: output* PersistentVolumeClaim | PVC oc-lab-pvc