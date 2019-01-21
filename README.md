# gitlab-ci-react-native-android
## Android 28.0.3 and Fastlane 2.61.0 
This Docker image contains react-native and the Android SDK and most common packages necessary for building Android apps in a CI tool like GitLab CI. 

A `.gitlab-ci.yml` with caching of your project's dependencies would look like this:

```
image: webcuisine/gitlab-ci-react-native-android

stages:
- build

cache:
  key: ${CI_PROJECT_ID}
  paths:
  - android/.gradle/

build:
  stage: build
  script:
  - yarn
  - cd android && ./gradlew assembleDebug
  artifacts:
    paths:
    - android/app/build/outputs/apk/
```


## Detached testing
Build locally
```
docker build -t webcuisine/gitlab-ci-react-native-android:android-26.0.1 .
```
or run from remote
```
	docker run -it -d webcuisine/gitlab-ci-react-native-android /bin/bash
	docker attach HASH
	docker stop 3c854ac65f64d424c097e639c002b50431454e839b5c551ec2a929dcbecb7176
	
````

## How to upgrade tool version
If you want to upgrade version of android sdk or build tools, please follow following staps.

1. Edit pacakges.txt to update version or add a new package
2. Run `$ANDROID_HOME/bin/sdkmanager "<package;version>"` in your local machine, then to check the file `$ANDROID_HOME/licenses/android-sdk-license`  
Ensure that all lines of the file in your local machine are included by Ln42 in `Dockerfile` just like following:
```
  && echo "8933bad161af4178b1185d1a37fbf41ea5269c55\nd56f5187479451eabf01fb78af6dfcb131a6481e\n24333f8a63b6825ea9c5514f83c2829b004d1fee" > $ANDROID_HOME/licenses/android-sdk-license \
```
3. Build the docker and publish it.
