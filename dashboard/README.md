# deployKF Dashboard

This component serves as the landing page and central dashboard for deployKF.
It provides a jump-off point to all other tool Web UIs.

## Development

### Getting Started

Make sure you have installed node 16!

1. We STRONGLY recommend using [nvm](https://github.com/nvm-sh/nvm):
     - Uninstall any Homebrew versions with `brew uninstall node` (or `node@XX`)
     - Install `nvm` with `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash`
     - Install node `16` with `nvm install 16`
     - Use node `16` with `nvm use 16`
     - Set node `16` as the default with `nvm alias default 16`
2. Run `cd dashboard`
3. Run `npm install` to install npm dependencies
4. Run `./run_local.sh` to start the development server, this will:
    - Run the following kubectl port-forwards:
       - `localhost:8081` -> `kfam-api.deploykf-dashboard.svc`
       - `localhost:8085` -> `jupyter-web-app-service.kubeflow.svc`
       - `localhost:8087` -> `ml-pipeline-ui.kubeflow.svc`
    - Run [webpack](https://webpack.js.org/) over the front-end code in the [public](./public) folder
    - Run [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) at http://localhost:8081
    - Run the Express API server at http://localhost:8082
    - Proxy requests from the front-end starting with `/api` to the Express server. 
    - All other requests are handled by the front-end server which mirrors the production configuration.
    - __TIP:__ if you see `bind: address already in use` errors, you might try `pkill -f "kubectl port-forward"` to stop all existing port-forwards
5. Open your browser to `http://localhost:8080` to see the dashboard:
    - You will need to inject your requrests with a `kubeflow-userid` header
    - You can do this in Chrome by using the [Header Editor](https://chromewebstore.google.com/detail/eningockdidmgiojffjmkdblpjocbhgh) extension
    - For example, set the `kubeflow-userid` header to `user1@example.com`

### Server Components

Server side code resides in the [app](./app) directory. 

The server uses [Express](https://expressjs.com/) and is written in [Typescript](https://www.typescriptlang.org/docs/home.html).

### Front-End Components

Client side code resides in the [public](./public) directory.
Client components are written using the [Polymer 3](https://polymer-library.polymer-project.org/3.0/docs/about_30) web components library.
All Polymer components should be written under the [public/components](./public/components) directory. 

You may use the [inline style](https://polymer-library.polymer-project.org/3.0/docs/first-element/step-2) for creating your Shadow DOM, 
or separate your CSS and HTML in separate files. 

We currently support [Pug](https://pugjs.org/api/getting-started.html) templating for external markup. 
See [main-page.js](public/components/main-page.js) for an example.

### Testing

When introducing new changes, you should ensure that your code is tested. 
By convention, tests should reside in the same directory as the code under test and be named with the suffix `_test.(js|ts)`. 
For a module named `new-fancy-component.js`, its test should be in a file named `new-fancy-component_test.js`.

Both server and client-side unit tests are written using [Jasmine](https://jasmine.github.io/api/3.3/global).
Client-side tests are run with Karma using headless Chrome by default.

#### Code Coverage

To run unit tests and collect coverage, run `npm run coverage`. 
This will output coverage statistics on the console and also create subfolders for `app` and `public` inside of 
the `coverage` folder with interactive displays of the code coverage. 

Please try to strive for high levels of coverage for sections of your code with business logic.
