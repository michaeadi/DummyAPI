Here’s a README documentation that outlines the process of writing test cases, setting up GitHub, and integrating with CircleCI for continuous integration Using the Dummy API:

---

# Project Name: API Testing and Continuous Integration with CircleCI

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Writing Test Cases](#writing-test-cases)
4. [Setting Up GitHub](#setting-up-github)
5. [Integrating CircleCI](#integrating-circleci)
6. [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
7. [Conclusion](#conclusion)

## Introduction

This documentation provides a step-by-step guide for setting up API tests using Newman (Postman CLI), connecting your project to GitHub, and integrating CircleCI for continuous integration and automated testing.

## Prerequisites

Before you begin, ensure that you have the following installed on your local machine:

- [Node.js](https://nodejs.org/)
- [Newman](https://www.npmjs.com/package/newman)
- [Git](https://git-scm.com/)
- [CircleCI Account](https://circleci.com/)
- [Postman](https://www.postman.com/) (for writing and managing test cases)

## Writing Test Cases

1. **Create a Collection in Postman:**
   - Launch Postman and create a new collection.
   - Add your API requests to this collection.
   - Write test scripts under the **Tests** tab for each request. These scripts should include assertions to validate API responses.

   Example Test Script:
   ```javascript
   pm.test("Status code is 200", function () {
       pm.response.to.have.status(200);
   });

   pm.test("Response time is less than 2000ms", function () {
       pm.expect(pm.response.responseTime).to.be.below(2000);
   });
   ```

2. **Export the Collection:**
   - Once your collection is ready, click on the **...** next to your collection name.
   - Select **Export** and choose the **Collection v2.1** format.
   - Save the exported JSON file in your project directory, e.g., `DummyAPI/Dummy Rest API.postman_collection.json`.

3. **Create an Environment (Optional but Recommended):**
   - If your API requires different environments (e.g., development, staging), create and export these environments in Postman.
   - Save the environment file, e.g., `DummyAPI/Dummy Rest API Env.postman_environment.json`.

## Setting Up GitHub

1. **Create a New Repository:**
   - Log in to [GitHub](https://github.com/) and create a new repository.
   - Clone the repository to your local machine:
     ```bash
     git clone https://github.com/michaeadi/Michael22082024.git
     ```

2. **Add Your Project Files:**
   - Copy your Postman collection and environment files to the cloned repository.
   - Create a `.gitignore` file and add any files or directories you want to exclude from version control.

3. **Commit and Push Changes:**
   - Stage and commit your changes:
     ```bash
     git add .
     git commit -m "Initial commit with Postman collection and environment"
     ```
   - Push the changes to GitHub:
     ```bash
     git push origin main
     ```

## Integrating CircleCI

1. **Create a CircleCI Configuration File:**
   - In your project root, create a directory named `.circleci`.
   - Inside this directory, create a `config.yml` file with the following content:

     ```yaml
     version: 2.1
     jobs:
       test:
         docker:
           - image: circleci/node:latest
         steps:
           - checkout
           - run:
               name: Install Newman
               command: |
                 npm install -g newman
                 echo 'export PATH=$PATH:/usr/local/bin' >> $BASH_ENV
                 source $BASH_ENV
           - run:
               name: Run Newman Tests
               command: |
                 newman run "Dummy Rest API.postman_collection.json" -e "Dummy Rest API Env.postman_environment.json"

     workflows:
       version: 2
       test:
         jobs:
           - test
     ```

2. **Push the CircleCI Config to GitHub:**
   - Stage, commit, and push your `.circleci/config.yml` file:
     ```bash
     git add .circleci/config.yml
     git commit -m "Add CircleCI configuration"
     git push origin main
     ```

3. **Set Up CircleCI:**
   - Log in to [CircleCI](https://circleci.com/) and select **Projects** from the sidebar.
   - Find your GitHub repository and click **Set Up Project**.
   - CircleCI will automatically detect your `.circleci/config.yml` file and begin running your tests.

## Common Issues and Troubleshooting

### Permission Issues
If you encounter permission issues when installing Newman globally, consider using `sudo`:
```bash
sudo npm install -g newman
```

### File Not Found Errors
Ensure the paths to your Postman collection and environment files are correct in the CircleCI configuration.

### Failing Test Cases
Check your Postman test scripts for any assertion errors. Review the CircleCI logs for detailed error messages.

## Conclusion

You have now set up a complete CI pipeline for API testing using Postman, GitHub, and CircleCI. This process ensures that your API tests are automatically executed whenever changes are made to your codebase, helping maintain the integrity and reliability of your API.

---
