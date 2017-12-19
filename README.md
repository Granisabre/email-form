# email-form

To run the development build check out the master branch then run 'npm install' to install required modules.
Once this completes run 'npm start' from a terminal. This will start a local server running on localhost:8080

To build for production run 'npm run build' from a terminal. You may need to 'set NODE_ENV=production' for the task to output files.
Output production files will be placed in the 'dist' folder.

I have used the vue-cli to setup the project, and opted to install the vue router component which isnt currently needed for such a simple task. I have not had time to write tests however the template is setup to allow karma-sinon-chai tests to be written. These can be ran using 'npm run test' once written.

I have written the vue components using ES6 however the webpack template uses babel to transpile to es5. I have also used the less compiler for css and restricted css changes using 'scope' on the vue component to prevent leakage between components. For simplicity sake I have kept all html, scripts and css in one file, however These could be separated with supporting folder structures to shorten the file.

I had intended to refactor some of the code to cleanup some duplication and optimize the css. I may get time to do this tomorrow.
