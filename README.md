# Watson Developer Cloud Swift SDK

[![Build Status](https://travis-ci.org/watson-developer-cloud/swift-sdk.svg?branch=master)](https://travis-ci.org/watson-developer-cloud/swift-sdk)
![](https://img.shields.io/badge/platform-iOS,%20Linux-blue.svg?style=flat)
[![Carthage Compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Documentation](https://img.shields.io/badge/Documentation-API-blue.svg)](http://watson-developer-cloud.github.io/swift-sdk)
[![CLA assistant](https://cla-assistant.io/readme/badge/watson-developer-cloud/swift-sdk)](https://cla-assistant.io/watson-developer-cloud/swift-sdk)

## Overview

The Watson Developer Cloud Swift SDK makes it easy for mobile developers to build Watson-powered applications. With the Swift SDK you can leverage the power of Watson's advanced artificial intelligence, machine learning, and deep learning techniques to understand unstructured data and engage with mobile users in new ways.

There are many resources to help you build your first cognitive application with the Swift SDK:

- Follow the [QuickStart Guide](https://watson-developer-cloud.github.io/swift-sdk/docs/quickstart)
- Review a [Sample Application](#sample-applications)
- Browse the [Documentation](https://watson-developer-cloud.github.io/swift-sdk/)

## Contents

### General

* [Before you begin](#before-you-begin)
* [Requirements](#requirements)
* [Installation](#installation)
* [Authentication](#authentication)
* [Custom Service URLs](#custom-service-urls)
* [Custom Headers](#custom-headers)
* [Sample Applications](#sample-applications)
* [Synchronous Execution](#synchronous-execution)
* [Objective-C Compatibility](#objective-c-compatibility)
* [Linux Compatibility](#linux-compatibility)
* [Contributing](#contributing)
* [License](#license)

## Before you begin
* You need an [IBM Cloud][ibm-cloud-onboarding] account.

## Requirements

- iOS 8.0+
- Xcode 9.0+
- Swift 3.2+ or Swift 4.0+

## Installation

### Dependency Management

We recommend using [Carthage](https://github.com/Carthage/Carthage) to manage dependencies and build the Swift SDK for your application.

You can install Carthage with [Homebrew](http://brew.sh/):

```bash
$ brew update
$ brew install carthage
```

Then, navigate to the root directory of your project (where your .xcodeproj file is located) and create an empty `Cartfile` there:

```bash
$ touch Cartfile
```

To use the Watson Developer Cloud Swift SDK in your application, specify it in your `Cartfile`:

```
github "watson-developer-cloud/swift-sdk"
```

In a production app, you may also want to specify a [version requirement](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#version-requirement).

Then run the following command to build the dependencies and frameworks:

```bash
$ carthage update --platform iOS
```

Finally, drag-and-drop the built frameworks into your Xcode project and import them as desired. If you are using Speech to Text, be sure to include both `SpeechToTextV1.framework` and `Starscream.framework` in your application.

### Swift Package Manager

Add the following to your `Package.swift` file to identify the Swift SDK as a dependency. The package manager will clone the Swift SDK when you build your project with `swift build`.

```swift
dependencies: [
    .package(url: "https://github.com/watson-developer-cloud/swift-sdk", from: "0.32.0")
]
```

## Authentication

Watson services are migrating to token-based Identity and Access Management (IAM) authentication.

- With some service instances, you authenticate to the API by using **[IAM](#iam)**.
- In other instances, you authenticate by providing the **[username and password](#username-and-password)** for the service instance.
- Visual Recognition uses a form of [API key](#api-key) only with instances created before May 23, 2018. Newer instances of Visual Recognition use [IAM](#iam).

### Getting credentials
To find out which authentication to use, view the service credentials. You find the service credentials for authentication the same way for all Watson services:

1. Go to the IBM Cloud [Dashboard](https://console.bluemix.net/dashboard/apps?category=ai) page.
1. Either click an existing Watson service instance or click [**Create resource > AI**](https://console.bluemix.net/catalog/?category=ai) and create a service instance.
1. Click **Show** to view your service credentials.
1. Copy the `url` and either `apikey` or `username` and `password`.

### IAM

Some services use token-based Identity and Access Management (IAM) authentication. IAM authentication uses a service API key to get an access token that is passed with the call. Access tokens are valid for approximately one hour and must be regenerated.

You supply either an IAM service **API key** or an **access token**:

- Use the API key to have the SDK manage the lifecycle of the access token. The SDK requests an access token, ensures that the access token is valid, and refreshes it if necessary.
- Use the access token if you want to manage the lifecycle yourself. For details, see [Authenticating with IAM tokens](https://console.bluemix.net/docs/services/watson/getting-started-iam.html). If you want to switch to API key, override your stored IAM credentials with an IAM API key.

#### Supplying the IAM API key
```swift
let discovery = Discovery(version: "your-version-here", apiKey: "your-apikey-here")
```

#### Supplying the accessToken
```swift
let discovery = Discovery(version: "your-version-here", accessToken: "your-accessToken-here")
```
#### Updating the accessToken
```swift
discovery.accessToken("new-accessToken-here")
```

### Username and Password

```swift
let discovery = Discovery(username: "your-username-here", password: "your-password-here", version: "your-version-here")
```

### API Key

_Note: This type of authentication only works with Visual Recognition, and for instances created before May 23, 2018. Newer instances of Visual Recognition use IAM._

```swift
let visualRecognition = VisualRecognition(apiKey: "your-apiKey-here", version: "your-version-here")
```

## Custom Service URLs

You can set a custom service URL by modifying the `serviceURL` property. A custom service URL may be required when running an  instance in a particular region or connecting through a proxy.

For example, here is how to connect to a Tone Analyzer instance that is hosted in Germany:

```swift
let toneAnalyzer = ToneAnalyzer(
    username: "your-username-here",
    password: "your-password-here",
    version: "yyyy-mm-dd"
)
toneAnalyzer.serviceURL = "https://gateway-fra.watsonplatform.net/tone-analyzer/api"
```

## Custom Headers
There are different headers that can be sent to the Watson services. For example, Watson services log requests and their results for the purpose of improving the services, but you can include the `X-Watson-Learning-Opt-Out` header to opt out of this.

We have exposed a `defaultHeaders` public property in each class to allow users to easily customize their headers:

```swift
let naturalLanguageClassifier = NaturalLanguageClassifier(username: username, password: password)
naturalLanguageClassifier.defaultHeaders = ["X-Watson-Learning-Opt-Out": "true"]
```

Each service method also accepts an optional `headers` parameter which is a dictionary of request headers to be sent with the request.

## Sample Applications

* [Simple Chat (Swift)](https://github.com/watson-developer-cloud/simple-chat-swift)
* [Simple Chat (Objective-C)](https://github.com/watson-developer-cloud/simple-chat-objective-c)
* [Visual Recognition with Core ML](https://github.com/watson-developer-cloud/visual-recognition-coreml)
* [Visual Recognition and Discovery with Core ML](https://github.com/watson-developer-cloud/visual-recognition-with-discovery-coreml)
* [Speech to Text](https://github.com/watson-developer-cloud/speech-to-text-swift)
* [Text to Speech](https://github.com/watson-developer-cloud/text-to-speech-swift)
* [Cognitive Concierge](https://github.com/IBM-MIL/CognitiveConcierge)

## Synchronous Execution

By default, the SDK executes all networking operations asynchronously. If your application requires synchronous execution, you can use a `DispatchGroup`. For example:

```swift
let dispatchGroup = DispatchGroup()
dispatchGroup.enter()
assistant.message(workspaceID: workspaceID) { response in
    print(response.output.text)
    dispatchGroup.leave()
}
dispatchGroup.wait(timeout: .distantFuture)
```

## Objective-C Compatibility

Please see [this tutorial](https://watson-developer-cloud.github.io/swift-sdk/docs/objective-c) for more information about consuming the Watson Developer Cloud Swift SDK in an Objective-C application.

## Linux Compatibility

To use the Watson SDK in your Linux project, please follow the [Swift Package Manager instructions.](#swift-package-manager). Note that Speech to Text and Text to Speech are not supported because they rely on frameworks that are unavailable on Linux.

## Contributing

We would love any and all help! If you would like to contribute, please read our [CONTRIBUTING](https://github.com/watson-developer-cloud/swift-sdk/blob/master/.github/CONTRIBUTING.md) documentation with information on getting started.

## License

This library is licensed under Apache 2.0. Full license text is
available in [LICENSE](https://github.com/watson-developer-cloud/swift-sdk/blob/master/LICENSE).

This SDK is intended for use with an Apple iOS product and intended to be used in conjunction with officially licensed Apple development tools.

[ibm-cloud-onboarding]: http://console.bluemix.net/registration?target=/developer/watson&cm_sp=WatsonPlatform-WatsonServices-_-OnPageNavLink-IBMWatson_SDKs-_-Swift
