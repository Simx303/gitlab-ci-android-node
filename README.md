# gitlab-ci-android
This Docker image contains the Android SDK, Node and most common packages necessary for building Android apps in a CI tool like GitLab CI. Make sure your CI environment's caching works as expected, this greatly improves the build time, especially if you use multiple build jobs.

A `.gitlab-ci.yml` with caching of your project's dependencies would look like this:

```
image: jangrewe/gitlab-ci-android

stages:
- npmBuild
- androidBuild

before_script:
- export GRADLE_USER_HOME=$(pwd)/.gradle
- chmod +x ./android/gradlew

cache:
  key: ${CI_PROJECT_ID}
  paths:
  - .gradle/

npmBuild:
  stage: npmBuild
  script:
  - npm run build

androidBuild:
  stage: androidBuild
  script:
  - cd ./android
  - ./gradlew assembleDebug
  artifacts:
    paths:
    - android/app/build/outputs/apk/**/*.apk
```
