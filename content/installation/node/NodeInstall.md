<!--
title: "Node.js Agent Installation"
description: "Installing the Node.js Agent"
tags: "NodeJS agent installation"
-->

## Installation

After downloading from your account, install the agent from your application's root directory as follows:

``` sh
npm install node-contrast-#.#.#.tar.gz
```
This will add the agent to your *node_modules* folder without creating an entry in the dependencies list of your *package.json*.

## Setup

When you download the Node agent, you will also be directed to download a configuration file. By default, the agent looks for this configuration file in your application's root directory and expects the file to be called *contrast_security.yaml*.
The minimum required *contrast_security.yaml* setup should look something like this:

``` yaml
contrast:
  url: https://app.contrastsecurity.com
  user_name: contrast_user
  api_key: demo
  service_key: demo
```


 Property                | Description
------------------------ | ------------
`contrast.api_key`       | Organization's [API key](user-account.html#profile)
`contrast.user_name`     | Contrast user account ID <br> (In most cases, this is your login ID)
`contrast.service_key`   | Contrast user account [service key](user-account.html#profile)
`contrast.url`           | Address of the Contrast installation you would like your agent to report to


For a full list of configuration options that can be placed in this file, see [Node Agent Configuration](installation-node.html#node-config).

## Run the Agent

First, add the following script to your application's *package.json*:

``` javascript
"scripts": {
	"contrast": "node-contrast <app-main>.js",
	"start": ...,
	"test": ...
}
```

Then, the agent can be run with ```npm run contrast```. This npm script can be changed to include other runtime configurations, such as an alternate configuration file location. For more information, see [Node Agent Configuration](installation-node.html#node-config).
