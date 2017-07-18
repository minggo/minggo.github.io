[appveyor](https://www.appveyor.com/) is a CI service for Windows. As travis doesn't support Windows, so appveyor is a good choice for Windows CI service. This article will describe how to integrate appveyor for a public github respository, and record some PowerShell script when integrating appveyor into [cocos2d-x](https://github.com/cocos2d/cocos2d-x).

It is easy to integrate appveyor into a public github repo, the steps are:

* [sign up appveyor](https://ci.appveyor.com/signup) with github account
* [add a new project](https://ci.appveyor.com/projects) to integrate appveyor to a public github repo

Now your github repo should have appveyor service integrated. It is easy, but it is just a start point, you should do some useful things when there is a new pull request sent to the repo, or a new commit is added into an existing pull request.

appveyor provides two ways to generate build configuration, using [UI or appveyor.yml](https://www.appveyor.com/docs/build-configuration/#appveyoryml-and-ui-coexistence). I prefer to use appveyor.xml, but i usually use UI configuration then click `Export YAML` in project settings to export the content of appveyor.xml. It is useful if you don't know how to write the configuration in appveyor.xml.

Following are some tips of appveyor.xml:

* disable two builds for a pull request
  
  by default, appveyor will trigger two builds when a pull request is sent, can use the configuration to disable it
  
  ```
  skip_branch_with_pr: true
  ```
* disable builds when a tag is created

  ````
  skip_tags: true
  ````
  
* use powershell script to do something before build and build

  ```
  before_build:
      - ps: xxx.ps1
    
  build_script:
      - ps: yyy.ps1
  ```
  
I am not powershell script expert, so met many issues when writing powershell script. Following are some tips about powershell script:

* [__&__ is call operator](https://ss64.com/ps/call.html) used to invoke external commands, such as `& pip install xxx`
* use `Write-Host` function to output usefull message
* use __$env:XXX__ to refer to __XXX__ [environment variables](https://www.appveyor.com/docs/environment-variables/), such as `$env:APPVEYOR_BUILD_FOLDER`
* use `Copy-Item -Path xxx -Destination yyy -Recurse` to copy contents of a folder to another folder, `-Recurse` is important to copy subfolders content
* Use `Push-Location` and `Pop-Location` to change working directory, similar to `pushd`, `popd` in linux