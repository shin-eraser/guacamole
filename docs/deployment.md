<p><a target="_blank" href="https://app.eraser.io/workspace/ekwXbfb1Z244qQFoiAvF" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

# Deployment Options
Are you ready to deploy your Solid application? Follow our guides to different deployment services.

## AWS via Flightcontrol
[﻿Flightcontrol](https://www.flightcontrol.dev/) is a platform that fully automates deployments to Amazon Web Services (AWS).
For more information on Flightcontrol's capabilities, you can [﻿visit their docs](https://www.flightcontrol.dev/docs).

![Deploy git repository](/.eraser/ekwXbfb1Z244qQFoiAvF___reS6fUv66LcKWYn8yV2OvCPvwSm2___---figure---Y9YW37Fkasu4U3nZ6ldgY---figure---hUo5aw8bIOyvnIK3PtuLNQ.png "Deploy git repository")

### Connecting to a git repository
Flightcontrol offers a GitHub integration, leveraging its continuous development actions. 

To get started with Flightcontrol's GitHub integration, you'll first need to log in or sign up to the Flightcontrol platform.
After you're logged in, simply link your GitHub account to Flightcontrol.

Once connected, Flightcontrol will take care of the rest.
It automatically detects any new pushes to your specified GitHub branches and builds your project.
The build process uses the commands in your `package.json` file and adheres to the settings that you have configured in Flightcontrol.
No additional setup is needed.

![Figure 1](/.eraser/ekwXbfb1Z244qQFoiAvF___reS6fUv66LcKWYn8yV2OvCPvwSm2___---figure---hkgO60Q41B6uZwWEyUM-Z---figure---j6jzLrgzgRAjsijnMNvyDQ.png "Figure 1")

### Using the dashboard
1. In the Flightcontrol dashboard, create a new project and select the repository you wish to use as the source.
2. Choose the GUI as your configuration type.
3. Add your Solid site as a static site by clicking the "Add a Static Site" option.
4. Label your output directory as `dist` .
5. If your project requires environment variables, add them in the designated area:
6. Finally, connect your AWS account to complete the setup.
### Using code
1. Navigate to your Flightcontrol dashboard and initiate a new project.
Choose the repository you'd like to use as the source.
2. Opt for the flightcontrol.json as your configuration type.
3. Add a new file named `flightcontrol.json`  at the root of your selected repository.
Below is an example configuration:
```
{
  "$schema": "https://app.flightcontrol.dev/schema.json",
  "environments": [
    {
      "id": "production",
      "name": "Production",
      "region": "us-west-2",
      "source": {
        "branch": "main"
      },
      "services": [
        {
          "id": "my-static-solid",
          "buildType": "nixpacks",
          "name": "My static solid site",
          "type": "static",
          "domain": "solid.yourapp.com",
          "outputDirectory": "dist",
          "singlePageApp": true
        }
      ]
    }
  ]
}
```
## Cloudflare
[﻿Cloudflare Pages](https://pages.cloudflare.com/) is a JAMstack platform for frontend developers, where JAMstack stands for JavaScript, APIs, and Markup.
For additional details and features, you can [﻿visit the Cloudflare website](https://pages.cloudflare.com/).

### Using the Cloudflare's web interface
1. Navigate to the [﻿Cloudflare log up page](https://dash.cloudflare.com/login)  and log in or sign up.
2. After logging in, find "Pages" in the left-hand navigation bar.
Add a new project by clicking "Create a project," then choose "Connect to Git."
3. You'll have the option to install Cloudflare Pages on all your repositories or select ones.
Choose the repository that contains your Solid project.
4. Configure your build settings:
- The project name will default to the repository name, but you can change it if you wish.
- The "build command" field, enter `npm run build`  .
- For the "build output directory" field, use `dist`  . 
- Add an environment variable `NODE_VERSION`  and set its value to the version of Node.js you're using.
**Note:** This step is crucial because Cloudflare Pages uses a version of Node.js older than v13, which may not fully support Vite, the bundler used in Solid projects.

1. Once you've configured the settings, click "Save and Deploy."
In a few minutes, your Solid project will be live on Cloudflare Pages, accessible via a URL formatted as `project_name.pages.dev` .
### Using the Wrangler CLI
Wrangler is a command-line tool for building Cloudflare Workers.
Here are the steps to deploy your Solid project using Wrangler.

1. Use your package manager of choice to install the Wrangler command-line tool:
```bash
npm i -g wrangler 
# or
pnpm i -g wrangler 
# or 
yarn global add wrangler
```
1. Open your terminal and run the following command to log in:
```bash
wrangler login
```
1. Execute the following commands to build your project and deploy it using Wrangler:
```bash
npm run build
npx wrangler pages publish dist
```
After running these commands, your project should be live.
While the terminal may provide a link, it's more reliable to check your Cloudflare Pages dashboard for the deployed URL, which usually follows the format `project-name.pages.dev`.

**Note:**
Make sure to navigate to the `Speed` -> `Optimization settings` section in your Cloudflare website dashboard and disable the `Auto Minify` option.
This is important as minification and comment removal can interfere with hydration.

## Firebase
[﻿Firebase](https://firebase.google.com/) is an all-in-one app development platform by Google, offering a range of services from real-time databases to user authentication.
For a detailed overview of the services available, you can visit [﻿Firebase's documentation](https://firebase.google.com/docs).

Before proceeding, make sure you've already set up a project in your Firebase console.
If haven't, you can follow [﻿Firebase's official guide](https://firebase.google.com/docs/projects/learn-more#creating-cloud-projects) to create a new Firebase project.

### Using the Firebase CLI Tool
1. Use your preferred package manager to install the Firebase command-line tool with one of the following commands:
```bash
npm i -g firebase-tools
# or
pnpm i -g firebase-tools
# or
yarn global add firebase-tools
```
1. Execute the `firebase login`  command to ensure you're logged into the Firebase account associated with your project.
2. In the root directory of your Solid project, create two new files: `firebase.json`  and `.firebaserc` .
- In `firebase.json` , add the following code:
```jsonld
{
  "hosting": {
    "public": "dist",
    "ignore": []
  }
}
```
- In `.firebaserc` , insert the following code (replace `<YOUR_FIREBASE_PROJECT_ID>`  with your Firebase project ID):
```bash
{
  "projects": {
    "default": "<YOUR_FIREBASE_PROJECT_ID>"
  }
}
```
1. Run `npm run build`  , followed by `firebase deploy`  to build and deploy your project.
Upon completion, a `Hosting URL` will be displayed, indicating the live deployment of your project.

## Netlify
[﻿Netlify](https://www.netlify.com/) is a widely-used hosting platform suitable for various types of projects.
For detailed guidance on build procedures, deployment options, and the range of features available, you can visit the [﻿Netlify documentation](https://docs.netlify.com/).

### Using the Netlify Web Interface
1. Begin by navigating to [﻿Netlify's website](https://app.netlify.com/)  and logging in or creating a new Netlify.
Once logged in, you will be take to your dashboard. Click the `New site from Git`  button to start a new project.
2. On the following page, choose "Connect to GitHub" or your preferred Git repository hosting service.
3. After selecting your Solid project repository, you'll be directed to a configuration screen.
Update the "Publish directory" field from `netlify`  to `dist` . Then, click "Deploy" to start the deployment process.
4. Once the build and deployment are complete, you will be taken to a screen that displays the URL of your live site.
### Using the Netlify CLI
1. Install the Netlify CLI using your preferred package manager:
```bash
npm i -g netlify-cli
# or
pnpm i -g netlify-cli
# or
yarn global add netlify-cli
```
**Note:**
Before proceeding, ensure that your Netlify account and team are fully set up.
This is crucial for a seamless project setup and deployment.

1. Open your terminal, navigate to your project directory, and run the `netlify init`  command.
Authenticate using one of the supported login options.
2. Follow the on-screen instructions from the CLI. When prompted for the 'Directory to deploy,' specify `dist`  — this is where Solid stores the built project files.
After completing the process, your project will be deployed on Netlify and can be accessed via the provided URL.

## Railway
[﻿Railway](https://railway.app/) is a well-known platform for deploying a variety of web and cloud-based projects.
For an in-depth look at the features offered by Railway, as well as detailed deployment guidelines, you can consult the [﻿Railway documentation](https://docs.railway.app/).

### Adjust the Start command
To begin, you need to update the start command in your `package.json` file to make it compatible with Railway.
Change the start command to `npx http-server ./dist` instead of using `vite`.
This adjustment means you will need to build the app to generate the `dist` folder.

For local development, continue using the original `dev` command.
Reserve the modified start command specifically for Railway deployments.
Below is an example of how your `package.json` may be configured:

```jsonl
"scripts": {
  "start": "npx http-server ./dist",
  "dev": "vite",
  "build": "vite build",
  "serve": "vite preview",
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
},
```
### Using the Railway Web Interface
1. Visit Railway's homepage and click "Start a New Project."
You will be redirected to connect with GitHub.
Log in or create an account using your GitHub credentials and authorize Railway to access your account.
2. After authorization, choose the repository that has your Solid project.
During this step, you can also add any required environment variables.
3. Once your project is configured, click "Deploy Now."
After a successful deployment, a confirmation screen will appear.
4. Railway does not automatically assign a domain to your project.
To do this, go to the settings and manually generate a domain for your deployed project.
Once a domain has been generated, your Solid project should be live.

### Using the Railway CLI
1. Using your preferred package manager and install the Railway CLI:
```bash
npm i -g @railway/cli
# or
pnpm i -g @railway/cli
# or
yarn global add @railway/cli
```
1. Open your terminal and run the following command to log in:
```bash
railway login
```
1. You have the option to link your local Solid project to an existing Railway project using railway link.
Alternatively, you can create a new project with `railway init`  and follow the on-screen prompts.
2. To deploy your project to Railway, use the following command:
```bash
railway up
# or 
railway up --detach # if you prefer to avoid logs
```
Your project will now be live on Railway.

## Vercel
[﻿Vercel](https://vercel.com/) is a widely-used platform specialized in hosting frontend projects.
For detailed information regarding build and deployment instructions, as well as features they offer, please visit the [﻿Vercel documentation](https://vercel.com/docs).

### Using Vercel Web Interface
1. Navigate to [﻿vercel.com/login](https://vercel.com/login)  to log in or create a new account.
Connect with your preferred Git repository hosting service.
2. Once on the dashboard, click the button at the top right corner and choose "Add New Project."
On the next page, select "Continue with GitHub" or your preferred Git service.
3. You will then see with a list of your repositories.
Use the search bar if needed to find the specific repository you want to deploy.
Click the import button to proceed.
4. After importing your Solid project repository, you will be taken to a configuration screen.
If your project requires any environment variables, add them in the designated field.
Click "Deploy" to start the deployment process.
5. Once the build and deployment are finished, you will be redirected to a screen that displays a screenshot of your live site.
### Using the Vercel CLI
1. Install the Vercel CLI using your preferred package manager.
```bash
npm i -g vercel
# or
pnpm i -g vercel
# or
yarn global add vercel
```
1. Open your terminal, navigate to your project directory, and run the following command to log in:
```bash
vercel
```
1. Follow the on-screen instructions from the CLI to finalize the deployment.
Once completed, your project will be live on Vercel and accessible via the provided URL.



<!--- Eraser file: https://app.eraser.io/workspace/ekwXbfb1Z244qQFoiAvF --->