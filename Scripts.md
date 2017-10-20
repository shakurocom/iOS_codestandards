#### Script to show `TODO` and `FIXME` marks. To install it add to Xcode new Run Script phase: `Build Phases` -> `Run Scripts`.
```
TAGS="TODO:|FIXME:"
echo "searching ${SRCROOT} for ${TAGS}"
find "${SRCROOT}" \( -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/"
```
