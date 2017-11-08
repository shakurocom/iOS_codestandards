#### Installation

* **1** Install Homebrew: installation script in https://brew.sh.
* **2** Run in terminal: 
	`brew update`
	`brew install swiftlint` (`brew upgrade swiftlint`)
* **3** Add `.swiftlint.yml` to root project folder.
* **4** Add Script in XCode build phases:
```
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "error: SwiftLint does not exist, download it from https://github.com/realm/SwiftLint"
  exit 1
fi
```
* **5** To disable checks in code: 
```
// swiftlint:disable <rule1> [<rule2> <rule3>...]
.........
// swiftlint:enable <rule1> [<rule2> <rule3>...]
```
* **6** File .swiftlint.yml can be placed in subfolders to add custom configuration.
* **7** Setup Xcode: go to `Preferences -> Text Editing -> While Editing` and check `Automatically trim trailing whitespaces` and `Including whitespace-only lines`.

#### Terminal Commands

* **1** To get version: `swiftlint version`.
* **2** To switch version: `brew switch swiftlint <version number>`.
* **3** To fix common issues run in terminal: `swiftlint autocorrect`.
* **3** To get active rules: `swiftlint rules`.

#### Tutorials Links

* **1** https://habrahabr.ru/company/tinkoff/blog/317892/
* **2** https://medium.com/xcblog/top-5-tips-for-integrating-swiftlint-into-ios-ci-cd-pipelines-1f4fe1d343b3
