#### Installation

https://docs.fastlane.tools/getting-started/ios/setup/
https://github.com/fastlane/examples
https://docs.fastlane.tools/actions/pilot/
http://devcenter.bitrise.io/tutorials/community-created/

// git changes (changelog_from_git_commits)
https://medium.com/@hesam.kamalan/how-to-send-commit-changes-by-email-once-ci-build-passes-dee13c67c195
https://github.com/fastlane/fastlane/issues/9687

https://github.com/fastlane/fastlane/tree/master/spaceship#2-step-verification // 2 step verification

GitHub
fastlane/examples
examples - :memo: A collection of example fastlane setups

Changelog from file:
https://github.com/pajapro/fastlane-plugin-changelog




in general you will use something like this lane in your fastfile

Meinhard Gredig [5:28 PM]
added this Ruby snippet: Nightly lane for Fastfile
desc "Build a new Nightly Build"
lane :nightly do
update_info_plist(
app_identifier: "nightly.xxx",
plist_path: "xxx-app-ios/Info.plist"
)
badge(
alpha: true,
shield: "#{get_version_number}-#{get_build_number}-grey"
)
gym(scheme: "xxx-app-ios", export_method:"app-store", use_legacy_build_api: false) # Build your app - more options available
reset_git_repo(force: true, skip_clean: true)
increment_build_number()
commit_version_bump(
xcodeproj: "xxx-app-ios.xcodeproj",
message: "Version Bump: #{get_build_number()}"
)
push_to_git_remote(
remote: "origin",
remote_branch: "master",
tags: false
)
end



## Integrating with bitrise

This tutorial assume, that you already have iOS project and all app IDs and provisions and such stuff on iTunesConnect and on developer.apple.com. Also, because we would use fastlane as a part of Bitrise workflow, we will try to move as much as possible out of fastlane into Bitrise workflow steps.

//TODO: compile usefull links

Usefull links:
* tutorial for fastlane, contains info how to install fastlane itself: https://www.raywenderlich.com/136168/fastlane-tutorial-getting-started-2
* all actions, available for fastlane: https://docs.fastlane.tools/actions
* iOS signing for fastlane: https://docs.fastlane.tools/codesigning/xcode-project/
* move info on configuration file: https://docs.fastlane.tools/advanced/#control-configuration-by-lane-and-by-platform
* fastlane plugin to add marks on app icon: https://github.com/HazAT/fastlane-plugin-badge & https://github.com/HazAT/badge & http://shields.io/
* Expose an Environment Variable and use it in another Step: http://devcenter.bitrise.io/tips-and-tricks/expose-environment-variable/
* Abort build gracefully: https://discuss.bitrise.io/t/how-to-programmatically-abort-instead-of-fail-a-build/1791/2

### Steps:

1. Open your terminal. And install fastlane: https://docs.fastlane.tools/getting-started/ios/setup/

2. Navigate to project's directory:
```
cd <your project directory>
```

3. Initialize fastlane configuration for your project:
```
fastlane init
```

4. Fastlane will ask for your apple ID & password to verify that proper app exists on itunesconnect with proper app id.

5. If you are a member of several teams on developer.apple.com, it will ask you to specify your team.

6. If you are a member of several teams on iTunesConnect, it will ask you to specify your team. (twice). **WARNING** remember your iTunes Connect team ID - it would be needed later.

7. After that fastlane should report all found settings and issues. After that it will create `./fastlane` directory. It will contain all settings needed for running lanes.

8. Go to `./fastlane/Appfile`. Check, that it contains correct information: `app_identifier`, `apple_id`, `team_id`. Add `itc_team_id` that you got on step 6.
More information can be found here (https://docs.fastlane.tools/advanced/#appfile)

9. Go to `./fastlane/Fastfile`. Go through it carefully, we would need to change it.
    * `fastlane_version` should be leaved as it was created by `fastlane init`.
    * same with `default_platform` - for iOS project it will be `default_platform :ios`.
    * all dependency management is lifted by Bitrise. So `before_all` block will reduced to nothing. Leave it empty.
    * for out needs we would leave only one `lane` block. Remove all others.
    * all additional steps are handled through Bitrise - make `after_all` and `error` blocks empty.

10. Configure your lane.

11. Make sure, that you specified provision profiles in "gym" action (see example below).

12. You can use environment variables using `ENV['VAR_NAME']` (see example below).

11. Example of Fastfile:
```
fastlane_version "2.66.2"

default_platform :ios

platform :ios do

    before_all do
    end

    desc "Submit a new Beta Build to Apple TestFlight"
    lane :beta do

        # Add a badge to app icon. This will actually overwrite images, so be careful.
        # More info: https://github.com/HazAT/fastlane-plugin-badge & https://github.com/HazAT/badge
        add_badge(
            alpha: true,
            shield: "v#{ENV['XPI_VERSION']}-#{ENV['XPI_BUILD']}-grey"
        )

        #build app
        gym(
            export_method: "app-store",
            export_options: {
                provisioningProfiles: {
                    "app.bundle.id" => "Your Provision Profile Name",
                }
            },
        scheme: "YourSchemeName")

        # Upload build to testflight & set field "what's new"
        pilot(
            changelog: ENV['WHATS_NEW_MESSAGE']
        )

    end

    after_all do |lane|
        # This block is called, only if the executed lane was successful
    end

    error do |lane, exception|
    end
    
end
```

12. `Deliverfile` is used by `deliver` tool of the fastlane. It is for uploading to AppStore. As we will upload only to Testflight this file is irrelevant.

13. If you are using any plugins with fastlane, you need to use it via gem. Firstly  we need to install it.
```
sudo gem install bundler
```

14. Create `Gemfile` inside your project directory with such contents. In the example below we are adding a plugin "badge". More info on using gem: https://docs.fastlane.tools/getting-started/ios/setup/#use-a-gemfile . More info on using plugins: https://docs.fastlane.tools/plugins/create-plugin/ .
```
source "https://rubygems.org"

gem "fastlane"
gem "fastlane-plugin-badge", git: "https://github.com/HazAT/fastlane-plugin-badge"
```

15. Update/install gems:
```
sudo bundle update
```

16. Push new files to git.

17. Open your Bitrise workflow. Setup it as follows:

      1. `Activate SSH key (RSA private key)` - to use your credentials to access git
   
      2. `Git Clone Repository` - to pull a branch
      
      4. `Certificate and profile installer` - crucial step to needed to use provision profiles. See "Code Signing" tab of the worklow editor screen on Bitrise. You need to upload AppStore and development provision profiles, development and distribution certificates
   
      5. `Run CocoaPods install` - if your project uses CocoaPods AND you do not commit pod files into your repository, then this step is necessary. It will run `pod intall` and update pods
   
      6. `Brew install` - if your project swiftlint to check formatting of the code, than you need to add this step. Specify `swiftlint` as "Name of the formula to install/upgrade" and `yes` as "Upgrade formula if previously installed".
   
      7. `Set Xcode Project Build Number` - to increment build number inside `Info.plist`. Nice solution is to set "Build Number" field to `$BITRISE_BUILD_NUMBER`. This will sync build numbers in testflight with ones on bitrise.
   
      8. `Xcode Project Info` - to extract version and build number from "Info.plist" into environment variables.
   
      9. `Xcode Test for iOS` - to ensure that unit tests are passing for a new build.
   
      10. `fastlane` - run the fastlane. Specify lane that should be run, that you setted up in `Fastfile`. In example above - it is `beta`. Check "Working directory" parameter - make sure it is correct.
   
      11. `Script 'push build number changes'` - a script to push a change of the build number back into git:
      ```
      git commit ./MetroApp/Metro/**/Info.plist -m "[version update] [ci skip]"
      git push origin "$BITRISE_GIT_BRANCH"
      ```
   
      12. `Git tag` - to add a tag to current branch about new release, that was made. "Tag to set on current commit" is a tag string itself. For our example it would be `exampleapp-testflight-$XPI_VERSION-$XPI_BUILD`

18. Go to "Secrets" tab of workflow editor. Add `FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD` and `FASTLANE_PASSWORD` variables. Their value should a password of the apple account, that you specified inside Appfile as `apple_id`.

19. Make sure you uploaded proper provision profiles and certificates to "Code signing" tab of the workflow editor. Before running a tool , that is mentioned on that page, make sure you at least once uploaded a build to testflight menually.

20. Mission completed. Schedule created workflow as you like/need. 

## Usefull scripts

### Collect commit messages -> Jira tickets

For this script branches have prefix equal to jira tickets (ex "MET-66"). Script will sxport message into Bitrise Environment variable "WHATS_NEW_MESSAGE"
```
#!/usr/bin/env bash

# fail if any commands fails
set -e
#debug log
set -x

# get previous tag
PREVIOUS_TAG=$( git describe --abbrev=0 --match "*metro*" )

# get commit messages from previous tag and extract only uniqu task identifiers from them
# this will leave something like this:
# MET-1
# MET-3
# MET-89
TASK_IDS=$( git log --pretty=oneline "$PREVIOUS_TAG"..."$BITRISE_GIT_BRANCH" | sed 's/m/M/g' | sed 's/e/E/g' | sed 's/t/T/g' | grep -E '.*(MET-[0-9]+).*' | sed -E 's/.*(MET-[0-9]+).*/\1/' | sort | uniq )

# check, that there are new feature-branches merged
if [ $TASK_IDS -ge 4 ]
then
    echo "Found no new merges. Exiting..."
    exit 1
fi

# transform task ids into Jira links
MESSAGE=$( echo "$TASK_IDS" | sed 's|\(.*\)|http://j.shakuro.com/browse/\1  (\1)|' )

echo "Processed changelog:"
echo "$MESSAGE"

# export message as a global variable into Bitrise environment
envman add --key WHATS_NEW_MESSAGE --value "$MESSAGE"
```
