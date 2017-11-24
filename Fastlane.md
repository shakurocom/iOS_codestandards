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






