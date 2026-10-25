KT call w/ Karen — mobile release cut — rough notes

- cut happens every other Tuesday, 10am ET. Branch from `main` as `release/X.Y`. Script: `./tools/release/cut.sh 4.12` (version arg, no "v").
- before cutting: check #mobile-ci that main is green for BOTH ios and android. "Android goes red on flaky Espresso tests all the time, if it's only `CheckoutFlowTest` you can ignore it, anything else block the cut."
- cut.sh bumps version in `app/build.gradle` and `ios/Info.plist`, tags, pushes. if it fails halfway (happened once, network), it leaves a local tag but no push → delete tag with `git tag -d v4.12.0` and rerun. Don't force push the tag ever, release mgmt bot watches tags and will start store submission.
- after cut: create Jira release `MOB 4.12` and move tickets with fixVersion. Karen uses JQL `project = MOB AND status = Done AND fixVersion is EMPTY AND resolved >= -14d`. "Usually" she skips tickets labeled `internal-only`.
- then post in #mobile-release with the template from the pinned message.
- release notes: draft from Jira, PM (currently the mobile PM rotation) approves. NEVER publish release notes to App Store Connect yourself — PM does it.
- hotfixes: branch off the release branch, NOT main. cherry-pick to main after. She's seen people branch from main and ship unreleased features.
- if cut day is a US holiday, push to Wednesday — she says "check with the EM" but not sure that's a written rule
- she wasn't sure what happens if iOS build number collides — "think it auto-increments, never seen it break"
- escalate to: #mobile-oncall for CI, EM for schedule changes
