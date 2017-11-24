#### Installation

https://www.raywenderlich.com/136168/fastlane-tutorial-getting-started-2
https://docs.fastlane.tools/getting-started/ios/setup/
https://github.com/fastlane/examples
https://docs.fastlane.tools/actions/pilot/
http://devcenter.bitrise.io/tutorials/community-created/

// git changes (changelog_from_git_commits)
https://medium.com/@hesam.kamalan/how-to-send-commit-changes-by-email-once-ci-build-passes-dee13c67c195
https://github.com/fastlane/fastlane/issues/9687

//Expose an Environment Variable and use it in another Step:
http://devcenter.bitrise.io/tips-and-tricks/expose-environment-variable/

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
This tutorial assume, that you already have iOS project and all app IDs and provisions and such stuff on iTunesConnect and on developer.apple.com. Also, because we would use fastlane as a part of bitrise workflow, we will try to move as much as possible out of fastlane into bitrise workflow steps.

//TODO: compile usefull links

Usefull links:
* tutorial for fastlane, contains info how to install fastlane itself: https://www.raywenderlich.com/136168/fastlane-tutorial-getting-started-2
* all actions, available for fastlane: https://docs.fastlane.tools/actions
* move info on configuration file: https://docs.fastlane.tools/advanced/#control-configuration-by-lane-and-by-platform

Steps:

1. Open your terminal.

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

6. If you are a member of several teams on iTunesConnect, it will ask you to specify your team. (twice)

7. After that fastlane should report all found settings and issues. After that it will create `./fastlane` directory. It will contain all settings needed for running lanes.

8. Go to `./fastlane/Appfile`. Check, that it contains correct information: `app_identifier`, `apple_id`, `team_id`. More information can be found here (https://docs.fastlane.tools/advanced/#appfile)

9. Go to `./fastlane/Fastfile`. 
