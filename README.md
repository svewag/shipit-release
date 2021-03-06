# Shipit-Release 

A minimal deployment Plugin for ShipitJS

## Usage

To use shipit-release just require it in your shipit file and set the needed configuration variables:

```javascript
module.exports = (shipit) => {
  require('shipit-release')(shipit)

  shipit.initConfig({
    default: {
      deployTo: '/*your-deploy-dir*',
      keepReleases: 5,
      dirToCopy: 'build',
      installCommand: 'npm install',
      buildCommand: 'npm run build',
    },
    *your-server-env-1*: {
      servers: [{
        user: '*your-staging-user*',
        host: '*your-staging-server*'
      ]
    },
    *your-server-env-2*: {
       servers: [{
        user: '*your-live-user*',
        host: '*your-live-server*'
      }],
    },
  })
}
```

Make sure you added at least one environment, for example ```staging``` to the configuration like:

```javascript
  shipit.initConfig({
    ...
    staging: {
      servers: [{
        user: '*your-staging-user*',
        host: '*your-staging-server*'
      ]
    },
  })
```

As soon as you require the Plugin you have the following tasks avaiable:

### Setup

Run: ```shipit *your-server-env* setup``` to setup your deployment Directories on the Server.
This will create your ```deployTo``` Folder as well as a ```releases``` Folder inside it.

### Deploy

Run: ```shipit *your-server-env* deploy``` to build and deploy your PWA to the given Server Environment.  
This will install the needed Dependencies with the configured ```installCommand``` and afterwards 
build your PWA locally with the configured ```buildCommand```.
If the build succeeds the Plugin will create a new Release Folder and sync your Files located in the 
configured ```dirToCopy``` Folder and symlink it to the ```current``` Folder in your ```deployTo``` Directory.  
To finish your Deployment it will remove the oldest Release to be in lign with the configured ```keepReleases``` Setting.

### Rollback: shipit *your-server-env* rollback

Run: ```shipit *your-server-env* rollback``` to rollback the latest Release.  
This will also delete the Release Folder you are rolling back from.

### Events

You can run Tasks on specific steps of the deployment process.

- installed -> when dependencies are installed.
- built -> when buildCommand has been executed.
- uploaded -> when dirToCopy has been uploaded.
- symlinked -> after the current symlink has been updated.
- finished -> after the deploy is finished.

#### Example

```javascript
module.exports = (shipit) => {
  require('shipit-release')(shipit)

  shipit.initConfig({
    ...
  })

  shipit.on('installed', async () => {
    return shipit.start('doStuff')
  })

  shipit.blTask('doStuff', async () => {
    // ... do stuff :)
  })
}
```

## Installation

Install the Package with your favorite Packagemanager:

```bash
npm install --save-dev shipit-release
```

```bash
yarn add --dev shipit-release
```

## ToDos

- Basic Test Setup
- Basic Test Coverage
- Setup Contribution Guidelines
- Cleanup index.js

## License

The gem is available as open source under the terms of the [MIT License](https://github.com/mortik/shipit-release/blob/master/LICENSE).
