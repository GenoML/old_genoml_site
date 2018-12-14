# genoml.github.io
This code is used to generate https://genoml.github.io. It pulls in files from `docs/` and `website/` to generate html files served on the site.

`website/` contains the JS, CSS, images and other files. `docs/` contains the markdown documentation files.

Two special files:

- `sidebars.json`: lists the sections.
- `siteConfig.json`: some header and i18n configs.

## Contributing, Building and Deploying
Changes to `source` branch are automatically picked into `master` branch by CI, then published. 

## Manual Build 
In case you need to perform local build, `cd website && npm install && npm start` to start the development server.
During your development, most changes will be picked up at each browser refresh. 
