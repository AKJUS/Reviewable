This is the release log for Reviewable's Enterprise branch.  Each release has a two-part version number (_server.client_) with a corresponding tag on the Docker image.  Note that once you've deployed a given release you'll only be able to deploy releases with version components at least equal to its _min_ version number, which may constrain your rollbacks.

New releases are announced on the [reviewable-enterprise-announce mailing list](https://groups.google.com/forum/#!forum/reviewable-enterprise-announce).

#### Known issues
- Avatar images use a procedurally generated fallback in some (but not all) installations of GHE running in private mode.  This is a bug with how GHE handles authentication cookies and can only be fixed from their side.  See [issue #770](https://github.com/Reviewable/Reviewable/issues/770).
- See also the public [list of bugs](https://github.com/Reviewable/Reviewable/labels/bug) for Reviewable.

#### Upcoming changes (min 3991.6302 GHE 2.19+ or 3.0+)
- Upd: avoid unnecessarily fetching refs when syncing a newly created pull request, as this can get expensive in some environments.
- Upd: indicate the currently focused file in the file matrix.
- Upd: show most recent previous reviewers separately from older ones in the file state information popup.  This can make it easier to find the right person to nag for a re-review.
- Upd: make the pull request link in the pull request panel a bit more prominent.
- Upd: display the pull request's creation and merge/close date.
- Upd: add `REVIEWABLE_DISABLE_GUEST_PASSES` flag to turn off guest passes.
- Fix: avoid occasionally crashing with an "Encryption not set up" error when the user signs out from an instance running in private mode.
- Fix: show correct bunny animation in conclusion panel on publish.
- Fix: always treat `.pbtxt` files as text.

#### Release 4743.7616 (min 3991.6302 GHE 2.19+ or 3.0+) 2025-04-02
- Upd: add `?scopes=...` option to explicitly request authorization of extra scopes for the current user.  This can help work around some GitHub API bugs in specific circumstances.
- Upd: change the default review completion condition to treat mentioned users as waited-on.
- Upd: respect `binary` macros in `.gitattributes`.
- Upd: move pull request address and link to the top of the pull request panel to make it easier to find.
- Upd: include specific completion condition evaluation error in the toast that shows up.
- Upd: offer the option to apply directives from the pull request panel's command field without sending a comment.
- Upd: Always sync comments from bots with Discussing disposition
- Upd: remove the green "continue review" button and add a "cancel deferral" item to the user's action menu in the participants panel.
- Upd: always show the main review discussion, even if there are no comments yet.
- Fix: put the review into an error state when GitHub refuses to fetch pull request data due to "too many changed files".
- Fix: update review status promptly when publishing a review (or sending a message) with a mention.
- Fix: guard against very rare crash when loading review page.
- Fix: improve client rendering performance.
- Fix: fix rendering of the "more participants" (ellipsis) tooltip on the dashboard.
- Fix: don't crash when sending a command while the review is deferred.
- Fix: don't include authors from obsolete commits in the completion condition's `review.pullRequest.coauthors` input property.
- Fix: create a new revision when the target (base) branch of a pull request is changed. Before this fix, files would continue to be diffed against the old base branch's commit until a new commit was pushed to the pull request branch.
- Fix: show the GitHub icon in the right spot when hovering over a branch name that wrapped in the pull request panel.
- Fix: smooth out animations when creating a new discussion or changing diff bounds.
- Fix: work around a GitHub API bug that consistently returns 500 when attempting to pin refs in specific pull requests if the token lacks `workflow` scope.  After retrying a few times Reviewable will give up and capture a message in Sentry (if configured, otherwise log to the console) that looks like `Repeatedly failed to POST /repos/:owner/:repo/git/refs` (with the actual owner and repo substituted).  If you see this message you should have the connecting admin authorize the `workflow` scope by visiting Reviewable with `?scope=workflow` appended to the URL.
- Fix: restore the "mark all files as reviewed" keyboard shortcut, and have it undo the marks if used twice in a row. (Regression introduced in v4718.7540.)
- Fix: reflect unreviewed file state forced by the completion condition in later revisions with no changes.

#### Release 4719.7563 (min 3991.6302 GHE 2.19+ or 3.0+) 2025-03-17
- Upd: distinguish between active and inactive requested reviewers.
- Upd: update `highlight.js` to v11.11.1.
- Upd: retain up to 10 merged/closed pull requests on the dashboard by default.
- Upd: make directory rows less tall in the file matrix.
- Upd: improve dark mode default contrast in diffs and gutter icons.
- Fix: avoid potentially showing duplicate comment time-ago dividers in a discussion where you engaged in back-and-forth without leaving the page.
- Fix: reduce false positives when detecting minified files.
- Fix: remove auto-merge warning icon if the pull request isn't open.
- Fix: don't crash on load in Safari if strict privacy settings are on.
- Fix: fix client performance regression in Chrome 134.
- Fix: use proper dark mode colors including in truncated PR description and in selected text in code blocks.

#### Release 4718.7540 (min 3991.6302 GHE 2.19+ or 3.0+) 2025-03-05
- New: add a Diffs panel that centralizes control over the diff bounds of all files, and adjust the toolbar button to navigate to it instead of opening the Changes dropdown.
- New: add a way to diff against the last revision reviewed by anyone with one click.
- New: let the user specify their preferred initial diff bounds and the threshold at which Reviewable switches to showing just one file at a time.
- New: add a Pull Request panel that combines parts of the main discussion and the Changes panel.
- New: add a command input bar to the pull request panel.
- Upd: put the "continue review" button into its new place in the top right corner in preparation for removing the Changes panel.
- Upd: remove the Changes panel, as all its functionality has now been moved to other spots.
- Upd: redesign the algorithm that determines whether a revision has probably been rebased, and from what corresponding original revision. The new logic is simpler and should do a better job in common situations, but may exhibit a different pattern of false positives and negatives in more complex ones.
- Upd: group checks into required, optional, and successful sections.
- Upd: log the clock correction offset we're applying (courtesy of Firebase) at startup, to make clock skew issues easier to debug.  Also check license expiry against the corrected clock.
- Upd: allow uploads of `webp` images.
- Upd: show diff layout and line length preferences in a more discoverable spot.  (The old "margin notch" still works too, though!)
- Upd: give users the option to show diffs since the last review by a specific reviewer, from both the Diffs panel and the file matrix.
- Upd: add a new header button to the file matrix that lets users diff since the last review by anyone.
- Upd: start indexing active and broken repository connections, in preparation for a new admin center design.
- Upd: allow linking to a file without entering isolated file mode.
- Upd: keep commits list open after clicking links.
- Fix: highlight contextual help hotspots correctly in dialogs.
- Fix: report the correct last reviewed revision in the file matrix reviewer avatar tooltip.
- Fix: correctly describe a diff against the user's last reviewed revision(s).
- Fix: don't rediff all files when expanding or collapsing a group in the file matrix.
- Fix: report the correct participants and number of messages for the `-top` discussion in the completion condition input data structure.  Before this fix some synthetic messages that don't show up in the review on the client were mistakenly included.
- Fix: always offer the mark all reviewed/read button in the Conclusion panel.
- Fix: ensure that (potential) changes to a pull request's mergeability status that occur close together aren't missed.
- Fix: don't unnecessarily retain some files at `r1` when overwriting a provisional revision.  This can lead to nonsensical diffs when using `r1` as the left hand side.  (Regression introduced in v4668.7438.)
- Fix: don't show an ellipsis at the beginning of every path in the diff panel headers.
- Fix: clear out some review-specific toasts when leaving a review page.  This can also prevent crashes if the toast offers an action related to the current review.
- Fix: proactively shut down the server when its license expires.  Previously, a server would refuse to start up with an expired license yet would happily keep running, which was confusing.
- Fix: allow organization owners to change settings for all repos, even if not explicitly an admin for them.
- Fix: don't crash after failing to set review revision mapping style.
- Fix: avoid rare crash when loading a review page due to a race condition.
- Fix: improve multiselect inputs to expand gracefully in all contexts.
- Fix: when reviewing against your last reviewed revision, keep the diff description unchanged even as you mark some files reviewed.
- Fix: include explicit dependencies line in suggested completion condition file export if needed.
- Fix: disallow searching in subscription billing manager dropdown.
- Fix: avoid incorrectly syncing valid users as `ghost` when running in an EMU enterprise instance.
- Fix: guard against connect / disconnect actions racing a repository sweep, which could result in deriving the wrong "does the organization have connected repositories" flag.
- Fix: fix a number of issues with compacting revisions that could cause compaction to fail, or could cause temporary server panics after restoring a review from backup.  None of the issues compromised review integrity, though.
- Fix: flag connections as broken due to the connecting user being suspended when GitHub enforces rate limits.

#### Release 4668.7438 (min 3991.6302 GHE 2.19+ or 3.0+) 2025-01-31
- New: allow users to compact revisions in a review by eliminating and combining redundant ones.
- Upd: render color swatch in comments for color codes in inline code.
- Upd: move diff overflow warning to a toast.
- Upd: add link to Conclusion panel to the toolbar.
- Upd: respect the "require linear history" branch protection setting.
- Upd: stop tracking whether a user can bypass branch protections, as this is much harder to do with rulesets.  Instead, offer a checkbox to everyone to try to bypass them and let GitHub decide.
- Upd: respect the completion condition's `mergeStyle` output property even if it conflicts with GitHub settings and results in no valid merge styles.
- Upd: support rulesets as a method of branch protection, including respecting any constraints they impose on merge methods.
- Upd: move the review style selector to the `Commits` file.
- Upd: raise default per-push revision limits.
- Upd: move commits list from changes dropdown to file header.
- Fix: don't crash when using a keyboard shortcut bound to `markFileReviewedAndAdvance()` with only one file left to review.
- Fix: fix alignment of send error message.
- Fix: make sure LGTM is centered in participants panel status (regression introduced in v4548.7221).
- Fix: correctly request a review by the current user when sending an ad-hoc comment.
- Fix: ensure that writing and sending comments works even if `IndexedDB` reports an error.
- Fix: make sure GitHub related file actions are accessible on touch devices.
- Fix: don't show grey spacer lines in single-column diffs.
- Fix: reduce incidence of "review not found, likely archived" errors when editing review completion conditions in repository settings.
- Fix: don't force a line break after an `:lgtm:` emoji.
- Fix: ensure that reviewers are not requested if publication failed and the "sync requested reviewers" flag was turned off before trying again.
- Fix: immediately reflect automatically synced requested reviewers in the review after publication.
- Fix: force "sync requested reviewers" to off if the current user isn't allowed to request reviewers.
- Fix: don't crash when publishing and quickly navigating to another review before publishing has completed.
- Fix: align revision cell arcs in file headers.
- Fix: consistently sync branch protection settings; previously, some changes could be accidentally ignored.
- Fix: don't crash when encountering bogus issue or pull request links in comments.
- Fix: avoid poor comment line mapping in the virtual commit file in some situations.
- Fix: ensure that the last revision is never obsolete in some edge cases.
- Fix: avoid generating spurious "commits that don't affect files in this pull request..." messages for the Commits file.
- Fix: recognize missing 2FA GitHub error messages.
- Fix: prevent crash when saving repository settings with REVIEWABLE_ENCRYPTION_PRIVATE_KEYS unset.
- Fix: don't get stuck when attempting to merge a pull request that includes a GitHub workflow file when the user authentication lacks `workflow` scope.
- Fix: try to refresh "Deleted user" records more frequently in case the user was added (back) to GitHub.
- Fix: fix unarchiving of some very old reviews.

#### Release 4623.7332 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-12-15
- New: add a `github-status.creator` setting to force a specific account to post all GitHub review status updates.
- New: show reviewer avatars in file matrix.
- Upd: improve GitHub request latency timing to account for paged requests.
- Upd: highlight HTML tags in comments, as they're likely not intended as such.
- Upd: accept plural variants of relevant disposition keywords in comments.
- Upd: reduce number of requests to GitHub when syncing a pull request.
- Upd: reduce number of ref queries issued when syncing a review, as these can get expensive in large repositories.  Also immediately unpin revisions that were reverted without being replaced.
- Fix: remove the ability to set the default review style for the repository from the Changes panel, as it's incompatible with file-based repository settings.  It can still be set normally via repository settings (either UI or file-based).
- Fix: don't show "customize" button in Checks panel if file-based settings are being used.
- Fix: don't highlight disposition keywords in draft preview mode.
- Fix: don't crash if a `CODEOWNERS` line has duplicate users/teams.
- Fix: allow directory paths to overflow the file matrix on hover.
- Fix: correctly delete refs according to `REVIEWABLE_REFS_DELETION_DELAY`.  (Regression introduced in v4424.6998.)
- Fix: add a `badge.commenter` option in file-based settings
- Fix: correctly handle pull requests leaving the merge queue.

#### Release 4586.7295 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-11-27
- New: post replies to discussion threads that were started on GitHub as replies in the respective thread.
- New: introduce a `REVIEWABLE_DISPOSITION_DEFAULTS` configuration that allows you to specify disposition defaults for users in various scenarios.  See the [docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#ui-customization) for details.
- Upd: move the checks dropdown into a panel in the main flow.
- Upd: give the option of rebasing to update the pull request branch (on `github.com` and GHE 3.11+ only).
- Upd: make the pull request branch update button harder to trigger accidentally.
- Upd: remove list of waited-on participants from the checks panel; use the participants panel instead.
- Upd: make review panels more mobile friendly, among other minor mobile improvements.
- Upd: make participants panel usable on mobile, make alignment more consistent, and show its top-level guide.
- Upd: add bindings for file and discussion dropdown menu commands.
- Upd: fetch organization and repository data on the server throttled to at most once every 15 minutes.  The data is used for autocomplete functionality when editing drafts and for validating drafts about to be published.  It's cached both on the server and the client, which should make for a more lightweight, faster experience, at the expense of extra update latency when the data changes if the repository is not connected.
- Upd: use a hard-coded table of emojis rather than fetching it from GitHub at every page load.
- Upd: add `REVIEWABLE_DISABLE_MY_PRS_ENROLLMENTS` flag.  When set, "My PRS in any public/private repo" and "All current and future repos" toggles (see [docs](https://docs.reviewable.io/repositories#create-reviews-for-your-own-prs)) will be unavailable for personal repositories, and will no longer be checked if previously turned on.  (Organization-level "All current and future repos" toggles are unaffected.)
- Upd: cache branch protection rules, listen to a webhook to update them and resync reviews proactively.  This will improve update latency when the rules change and reduce the number of requests to GitHub.  The cache expires after 15 minutes, in case a webhook is missed or the repository is not connected.
- Fix: guard against crash when quickly visiting and leaving a review page.
- Fix: fix animation when expanding and collapsing panels.
- Fix: guard against crash when quickly visiting and leaving the repositories page.
- Fix: consider requests for changes if branch protection requires a pull request for merging, even if it doesn't require actual approvals.
- Fix: restart server in case of an uncaught top-level exception, which otherwise could get it stuck in a "healthy" state but not doing any work.
- Fix: make sure to use proper GitHub formatting for pasted permalinks in comments.
- Fix: adjust all doc links to new URL schema.
- Fix: show correct unreviewed file count on the dashboard when a user hasn't visited the review yet.
- Fix: reliably publish non-comment drafts (e.g., review marks) when no draft comments are pending. There was a race condition that caused publishing to silently abort in such cases if some background requests hadn't completed yet.
- Fix: maintain continuity of toast borders at arrow.
- Fix: improve indentation in the code block editor when breaking a line in the middle of an indent.
- Fix: include a final newline when copying a suggestion.
- Fix: guard against rare crash when picking labels from autocomplete popup.
- Fix: allow users with `triage` (but not `write`) permissions to request reviewers.
- Fix: guard against very rare crash when leaving a review page with renamed files.
- Fix: don't request `/rate_limit` when handling a 429 error and GHE rate limiting is turned off.

#### Release 4548.7221 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-11-02
- Upd: render autolinks in Markdown with an underline.
- Upd: show just the last comment when revealing resolved discussions, rather than expanding all comments immediately.
- Upd: improve dark mode colors for some warning messages.
- Upd: pop up a toast when a warning indicator appears in the lower right corner.
- Upd: warn the user when a navigation cycle (e.g., "next unresolved discussion") would skip over some items because they're not currently displayed, and offer to show them all.
- Upd: add syntax highlighting for ERB files.
- Upd: add options for VSCode over SSH and VSCode via WSL to the external editor link template dropdown in the account settings.
- Upd: change the default diff font to Liberation Mono, as it has much better glyph coverage compared to Droid Sans Mono, and add font weight and height controls to the settings.
- Upd: improve overall app performance in reviews with that have both a lot of files and dozens of revisions.
- Upd: improve overall performance in large reviews, and improve instrumentation to capture more potential causes of slowness.
- Fix: stop failing with bogus "review state must be an object" messages in the completion condition playground.  (Regression likely introduced in v4479.7067.)
- Fix: avoid crashing in situations where a renamed file is reverted in a provisional revision while the review page is open.
- Fix: guard against a rare crash when a provisionally reintroduced file disappears.
- Fix: clean up the animation when expanding a fully collapsed discussion.
- Fix: make sure header toggle always works on review pages.
- Fix: guard against a crash when navigating to a file and immediately leaving the review page.
- Fix: don't crash if `IndexedDB` is disabled in Safari (!?).
- Fix: avoid rare crash due to an undefined `comments` property.
- Fix: avoid unnecessary animations and scrollbars in the participants panel in overlay mode.
- Fix: ensure the close button in overlays doesn't overlap the scrollbar.
- Fix: guard against rare client crash caused by a race condition.

#### Release 4497.7119 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-09-28
- New: add `Reviewable.resetSession()` console API to work around "resuming session" issues.
- New: add colorblind accessibility settings.
- Upd: show repository errors to admins at bottom of reviews.
- Upd: indicate participants with no access to the repository by crossing them out in the participants panel.
- Fix: turn off Sentry breadcrumbs in server events, as they are useless and can leak auth tokens into Sentry.
- Fix: make sure conclusions panel never obscures dispositions dropdown.
- Fix: attempt a workaround for Safari occasionally getting stuck while resuming the session.
- Fix: be more efficient and thorough when checking whether an @-mention is valid, but also block @-mentions that we can't verify (e.g., due to timeouts) instead of letting them through.  (@-decorators in unquoted code often result in such mentions and could display bogus participants in the review, even though those "participants" had no access and weren't notified.)
- Fix: enable Publish button when all drafts are bare suggestions.
- Fix: show Conclusion panel when the user clicks "mark reviewed and go to conclusion" in single file mode.
- Fix: consistently display master repository name and indicator on the repositories page, if the user has permission to see them.
- Fix: guard against very rare crash due to missing locator in the Conclusion panel.
- Fix: don't crash with "Encryption not setup" error on page load when Reviewable is running in private mode and the user isn't signed in.  (Regression likely introduced in v3935.6189.)

#### Release 4479.7067 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-09-01
- New: add an API for managing Enterprise team constraints.  ([API docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/api.md))
- New: support reading settings from a `.reviewable` directory in the repository and inheriting from a master settings file.  See the [announcement](https://www.reviewable.io/blog/announcing-the-reviewable-settings-directory/) for more details.
- Upd: make more panels collapsible on the review page.
- Upd: add shortcut button for the file matrix to the toolbar.
- Upd: add "TODO" as magic keyword for "working" disposition
- Upd: replace `memory.ghSocketsCreated` and `memory.ghRequestsIssued` in the logs with a more informative `memory.ghRequests` object, and add `github.sockets.free` and `github.sockets.busy` gauges to `statsd` stats.
- Upd: add a new option to show badge only if the review has been requested.
- Fix: don't cut off panel drop shadow when expanding/collapsing.
- Fix: if all files are hidden but the review is updated, offer to show proposed diffs instead of full diffs in the diffs panel.
- Fix: keep close button in top right corner when a collapsed file matrix is popped up in a dialog and scrolled horizontally.
- Fix: guard against rare crash when rendering comment list.
- Fix: wrap diffs at correct column even if last character is a space.
- Fix: make sure bunny button is covered by contextual help overlay.
- Fix: include just-published discussion in unresolved discussions navigation loop.
- Fix: make sure account settings open when editor link needs attention.
- Fix: always link to account settings from discussion level actions even when the editor link is bad.
- Fix: ensure bunny graphics in the Conclusion panel don't get cut off.
- Fix: don't fetch bot information via GitHub's users API, as that appears to have stopped working at some point and will prevent PRs from syncing.
- Fix: guard against very rare permission denied error when marking all files as reviewed.
- Fix: avoid a rare crash when signing out.
- Fix: avoid rare crash due to a race condition in the UI.

#### Release 4424.6998 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-07-16
- New: dark mode at long last! Also respects system (OS/browser) settings.
- Upd: tweak presentation of GitHub-related actions in the file and discussion dropdown menus, and offer separate actions for "view diff" vs "open file".
- Upd: inactive window text selection color set for all major browsers (only partial support before).
- Upd: improve renamed file notifications.  These messages are now specific to the selected diff, highlight the deltas between the old and new filenames, and add a warning if the renamed file was recreated later on.  Recreated files also get a warning as this is often accidental.
- Upd: use most up to date GitHub styling in rendered markdown.
- Upd: remove "more content" indicator.
- Fix: guard against missing scroll context/margin counter reference errors.
- Fix: encrypt owner, repo, and ref values in ref cleanup queue.
- Fix: improve safeguards against buggy pull request query results occasionally returned by GitHub.
- Fix: guard against rare crash caused by a race condition in accessing `isLocked`.
- Fix: guard against bogus API return values when sweeping old refs.
- Fix: consistently fully render the checks donut on page load.
- Fix: don't crash when first webhook callback for a repository fails.
- Fix: avoid falling into a potentially endless loop of trying to fetch file contents if the fetch fails but was needed to find the right location for a discussion in a diff.
- Fix: only capture webhook failures if they're new.
- Fix: guard against rare race condition and crash when computing disposition keyword highlight.
- Fix: maintain delta alignment when page is zoomed.
- Fix: avoid rare crash when creating a code block.
- Fix: guard against rare crash when selecting text in review.
- Fix: correctly identify renamed revisions if a file was renamed, recreated, then renamed again.
- Fix: don't be overeager about marking some no-change revisions of a renamed file as "base changes only".
- Fix: ensure that guide overlays are interactive on non-review pages.
- Fix: keep PR labels from flickering on hover.
- Fix: make sure contextual help button is visible when chat button is shown.

#### Release 4370.6910 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-05-30
- New: add a completion condition [example](https://github.com/Reviewable/Reviewable/blob/master/examples/conditions/pull_approve.js) that shows how to codify a complex, multi-stage approval process similar to what's possible with Pull Approve.
- New: added a new configuration option `REVIEWABLE_REFS_DELETION_DELAY` to clean up potentially unneeded `git` refs.  See the [docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#core-configuration) for details.  If you've been using Reviewable at scale for a while, you might want to control the rate at which refs are cleaned up by starting with a high delay and ratcheting it down towards the desired value month by month.  Note also that the ref wipes can cause branch deletion notifications to be emitted by GitHub even though they're not branches.
- Upd: refresh the styling of panels on review pages and update the page header layout.  This may not look like much but it includes a lot of cleanup and upgrades under the hood, setting us up to make more impactful UI changes in the near future!  (Yes, the sidebar is on its way.)
- Upd: treat `skipped` commit checks as successful.
- Upd: add `repository` and `coauthors` to `review.pullRequest` for completion condition input, and support `requestedTeams` in the output to adjusted the teams from whom a review is requested.
- Upd: add a `fallback: true` flag to `review.pendingReviewers` users selected solely due to having no reviewers pending due to organic causes.
- Upd: track a per-review `stage` property and allow the completion condition to set it.
- Upd: allow no-scope uses of the `{builtin: 'fulfilled'}` designation to indicate that the default scope has been fulfilled.
- Upd: allow uploads of SVG images into comments.
- Upd: replace dragging of the margin notch with keyboard input of the desired number of columns.
- Upd: move file mode change indicator from file header to a dedicated message.
- Fix: restore support for GHE 3.8 and older, broken in v4320.6839.
- Fix: tolerate broken GitHub GraphQL status check responses.
- Fix: guard against a very rare crash when renamed files are present in the review.
- Fix: restore `copyHeadBranch` and `editBaseBranch` keyboard binding commands.
- Fix: diff files where the user is a designated reviewer even if "skip files claimed by others" is on and would otherwise indicate that they should be skipped.
- Fix: display a more accurate post-publishing file status when publishing would change its nature rather than just making the file reviewed.
- Fix: respect `omitBaseChanges: true` on `builtin: 'anyone'` designations.
- Fix: correctly infer review status on a file when `omitBaseChanges: true` is specified on a designation and the file has revisions with no changes.
- Fix: fail more gracefully if repository not accessible when processing user request on the backend.
- Fix: include the `teams` property in all user objects that are part of the completion condition input structure.
- Fix: avoid logging clipboard errors.
- Fix: guard against missing context error.
- Fix: don't crash when encountering more than one invalid directive.
- Fix: guard against very rare crash when clicking inside a draft.
- Fix: address minor inconsistencies in file header styling when transitioning to the statusbar.

#### Release 4320.6839 (min 3991.6302 GHE 2.19+ or 3.9+) 2024-04-17
- New: allow instances running against GHEC to limit sign-ins to a given EMU username suffix.
- New: allow a local GHE Server status API implementation to be tied into Reviewable's GitHub status reporting UI via `REVIEWABLE_GITHUB_STATUS_URL`.
- Upd: adjust pull request links in comments to point to the corresponding review instead, if it exists.
- Upd: add support for `linguist-generated` and `linguist-language` attributes in `.gitattributes` files.
- Upd: support syntax highlighting for the `jsonc` "language".
- Upd: add syntax highlighting for VBA files.
- Upd: enabled support for merge queues in GHE 3.12+.
- Fix: don't repeatedly try to get anonymous permissions for a private repository.
- Fix: immediately clear private data from memory when signing out.
- Fix: reject Firebase tokens with no expiry date.  Some were accidentally issued a couple years ago and got grandfathered in for a while to avoid disruption, but it's long past time to stop accepting them.
- Fix: guard against crash when exiting review page while it's scrolling.
- Fix: prevent crash when permissions time out while on a review page.
- Fix: don't crash when license admin or instance owner visits the repositories page more than once without reloading the page.
- Fix: disallow time-based refreshing of a custom review completion condition if the pull request is merged or closed.  Condition output is rarely needed for non-open pull requests, and naïve conditions could end up looping this way forever.  Worse, if a pull request was closed and a new one opened on the same commit, the fix in v4186.6596 would be ineffective and a feedback loop that exhausts GitHub's per-commit status limit was likely to occur.
- Fix: raise maximum runtime of background sweep tasks for Sentry cron monitoring to avoid false alarms.

#### Release 4302.6774 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-02-27
- Upd: ensure that the last revision always represents the pull request's current head, in case the branch was moved back to an earlier commit.  Before, it was possible for an obsolete revision to be last instead, which could be confusing and made some completion condition code more complicated.  It could also result in the revision being considered reviewed when that may not have been the reviewer's intention; we now carry forward review marks only when it is safe to do so.
- Upd: don't split revisions when the commit author changes, and indicate each commit's author in the virtual Commits file if necessary instead.
- Upd: expose `Reviewable.showAllComments()` and `Reviewable.hideAllComments()` functions in the console.
- Upd: report queue and cron job health to Sentry's cron monitoring feature at regular intervals (if a Sentry DSN was configured).  The monitors will be configured automatically as the servers are running though it might take up to a month before all the jobs show up.  Note that Sentry charges an extra fee for cron monitoring but it's pretty minimal.
- Fix: react immediately to custom font family and size being deleted in the settings dialog, instead of requiring a page reload.
- Fix: recognize blocked repositories when attempting to auto-connect.
- Fix: avoid spinning forever on "Resuming session" in Safari 17 and up.
- Fix: don't escape code as Markdown when pasting into a fenced code block in a draft comment.
- Fix: automatically switch to in-process worker if unable to resume session via the shared one.
- Fix: detect changed usernames when looking for personal repositories or personal pull requests to auto-connect.
- Fix: prevent tasks polling for new personal repositories or pull requests from exceeding their lease time.
- Fix: report errors encountered when auto-connecting a new repository.
- Fix: report broken repository connections in Reviewable's GitHub status check instead of leaving it empty or set to its last normal value.
- Fix: update Reviewable's GitHub status checks when reconnecting a repository, so they're not stuck on "repo disconnected" until something else forces an update.
- Fix: reinstate toggles for My PRs / All repos in connections panel.
- Fix: don't misparse team mentions as user mentions in comments.  This could cause the completion condition to fail with a bogus "...is a member of over 100 teams" error message.
- Fix: guard against rare crash when trying to publish while the review page is still loading.

#### Release 4247.6681 (min 3991.6302 GHE 2.19+ or 3.0+) 2024-01-25
- New: add an API to query information about seats and their occupants.  See the new [API docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/api.md) for details.
- Upd: margin notch is updated via manual or keyboard input and not dragging.
- Upd: add `review.pullRequest.sanctions` to completion condition input data.  This replaces the previous `approvals` proprety (which will continue to be supported, don't worry) with a richer data structure that includes user teams.
- Upd: highlight Cairo files as Rust.
- Upd: Remove support for `azurefns` (Azure Functions) from REVIEWABLE_CODE_EXECUTOR setting.
- Upd: use TypeScript syntax highlighter for `.mts` and `.cts` files.
- Fix: render :shipit: emoji properly.
- Fix: fix rendering of GitHub line links in comments to have the right link to the underlying commit.
- Fix: don't believe GitHub when it says that issues can't be fetched when syncing.
- Fix: show correct background color for code snippets in file-level discussions.
- Fix: don't ignore some normally spurious errors reported through the global error hook if they appear to originate from Reviewable.
- Fix: work around invalid GitHub image proxy links in comments.
- Fix: avoid a memory / CPU leak when switching between reviews without closing the tab.
- Fix: drain requests before exiting server.
- Fix: when following a link to a discussion within a review, scroll to that discussion.
- Fix: correct ribbon label positioning when showing new dispositions in a discussion.

#### Release 4186.6596 (min 3991.6302 GHE 2.19+ or 3.0+) 2023-12-04
- New: allow the custom review completion condition to designate per-file reviewers, display the review state of each file with a small icon, and provide full details of who reviewed / needs to review the file in a dropdown.  See [docs](https://docs.reviewable.io/files.html#file-review-state) for details.
- Upd: replace the Gitter chatroom link in the support menu with a built-in popup chat.  You can substitute your own link instead by setting REVIEWABLE_CHAT_URL to a URL of your choice (for example a Slack channel you're sharing with us!), or remove the menu item entirely by setting the environment variable to `off`.
- Upd: speed up publish, publish preview, and ad-hoc comment send functions.  Note that this will increase the number of evaluations of custom review completion conditions.
- Upd: clean up and simplify the review summary unreviewed files and unreplied discussions counters, and make it clear when publishing will defer the review.
- Upd: consistently remove the "new comment" ribbon when acknowledging or replying to a discussion.
- Upd: verify required `CODEOWNERS` in the pull request approval check, and list any that are missing.
- Upd: include team memberships for users in completion condition input.
- Upd: don't try to use the demo review structure as a sample completion condition input in the settings editor if no review can be found in the target repository.  This was causing too many problems and wasn't particularly useful.
- Fix: don't fail with a permission denied error when publishing a draft for a discussion where new comments were posted (and not acknowledged by the user) in more than the previous 10 seconds.
- Fix: insert badge link into pull request even if the review starts out broken.
- Fix: detect when multiple reviews share the same head commit and set a special error status in GitHub.  Otherwise, it was possible for the reviews to get into a feedback loop when updating the status and quickly exhaust GitHub's limit of 1000 statuses per commit.
- Fix: correctly coalesce and display groups of trivial files ("files hidden because...") in every situation, and hide the "+N more" link after it's been clicked.
- Fix: if navigating to a hidden file causes the page to switch into single-file mode, then display that file rather than the previously selected one.
- Fix: don't sync requested reviewers when sending ad-hoc comments, even if the option is checked in the publish options.
- Fix: eliminate some duplicate background rendering of draft comments.
- Fix: render avatars smoothly (regression introduced in v3424.5210).
- Fix: restore file matrix table striping (regression introduced in v4055.6359).
- Fix: handle some occasional bad data coming from GitHub's API more gracefully.
- Fix: always paint the checks donut correctly after animating changing check states.
- Fix: avoid some UI jitter when starting to type a review summary draft at the bottom of the page, or when deleting one from same.
- Fix: avoid a cumulative slowdown after deleting many drafts.
- Fix: ensure the review summary publishing bunny always animates when it should.
- Fix: prevent some (very) minor elements from animating when animations are turned off in the user's settings.
- Fix: slice off tail quotes from GitHub-originated messages only if they were sent over email. Comments written in GitHub's UI rarely use tail quoting, but do sometimes have actual useful quotes at the end.
- Fix: tighten up the layout of filename headers — they had accumulated a bunch of unnecessary whitespace over time.
- Fix: avoid logging bogus "Function already exist" errors in the Lambda executor.
- Fix: don't crash in edge cases when the file you're focused on in single-file mode gets renamed and deleted.
- Fix: keep magic keyword tooltip from becoming detached in the UI after updating the keyword.
- Fix: use consistent tooltip theme for magic keyword explanations (regression introduced in v4088.6442).
- Fix: don't crash in some situations when the user refuses to login or authorize scopes.
- Fix: when using the `vm2` or `azurefns` executors, correctly catch completion condition errors instead of failing out of the task.
- Fix: don't crash on startup if unable to access local storage.
- Fix: prevent crash when dropdown is opened and closed too quickly.

#### Release 4088.6442 (min 3991.6302 GHE 2.19+ or 3.0+) 2023-10-16
- Upd: expose a `merge()` command for binding to a keyboard shortcut.
- Upd: improve the merge button arming animation to be clearer.
- Upd: unmappable GitHub inline comments are now linked to the original comment on GitHub.
- Upd: update the tooltip system; no user-visible changes expected.
- Fix: remove a lookbehind regular expression that breaks in Safari <16.4.
- Fix: avoid icon render errors after state changes in participants panel.
- Fix: be more tolerant about using valid repo admin authorizations even when a repo's connection is broken.
- Fix: guard against some rare cases of elements not existing due to timing.
- Fix: don't lose diff delta highlights in some very old browsers. (Regression introduced in v4055.6359.)
- Fix: correctly highlight prior revision arcs in file headers for contextual help.
- Fix: correctly position contextual help highlights in pinned file headers.
- Fix: guard against crash when user logs out in the middle of a dashboard query.
- Fix: deduplicate check names (e.g., by the workflow that spawned them) to avoid inadvertently dropping checks from the list in the UI.
- Fix: if a pull request isn't eligible to be turned into a review due to contributor team constraints applied to the license or subscription, stop processing its events earlier.  This should significantly reduce unnecessary processing load and GitHub API quota usage in cases where the contributor team is small compared to the number of other contributors in a connected repository.
- Fix: accept clicks in file header dropdown when jumping from file matrix.
- Fix: stop treating `.bmp`, `.dib`, and `.xbm` as image files.  These are truly ancient formats that are almost never used and may conflict with more modern uses of the filename extensions.
- Fix: omit Sentry attachments for completion condition timeout errors.

#### Release 4055.6359 (min 3991.6302 GHE 2.19+ or 3.0+) 2023-09-14
- Upd: modernize the client's color system.  There should be no user-visible effect though the eagle-eyed may notice some slight changes in color here and there.  The new system should be compatible with existing stylesheet customizations.
- Upd: render Markdown-style links in coverage errors.
- Fix: don't break under GHE 3.10.  This regression was introduced in v3935.6189 and specifically affects _only_ GHE 3.10.

#### Release 4046.6345 (min 3619.5594 GHE 2.19+ or 3.0+ but not 3.10) 2023-09-07
**WARNING**: This version includes a fix to the task queuing system that is likely to result in a burst of task processing activity on first launch.  This may be high enough to temporarily overload Firebase, so we recommend that you schedule the upgrade for off-hours.
- New: reflect GitHub's pull request approval status (as regulated by branch protection) in a new entry in the checks dropdown panel.  (This doesn't reflect `CODEOWNERS` yet, but will in the next release.)
- Upd: add support for an `error` property in coverage reports.
- Upd: recognize Poetry lock files as generated files.
- Upd: send bad connection notification emails only if the connecting user is still a member of the organization.
- Upd: let `Reviewable.workOfflineAtMyOwnRisk()` take a boolean flag to make the setting sticky, and set `offline` class on `body` to enable custom styling.
- Upd: improve permission check frequency with adaptive timings and jitter, to reduce database load spikes.
- Fix: prevent HTML caching to avoid the browser trying to load JS and CSS files from a previous version that no longer exist, leading to a broken page.  (The HTML file is tiny and all other files are still cached aggressively, so this shouldn't affect load latency.)
- Fix: don't crash when attempting to send a draft with code blocks and a whitespace-only body.
- Fix: reduce the number of GitHub requests issued when syncing a review.
- Fix: don't highlight deltas in hunk header lines, and prevent them from wrapping.
- Fix: improve sticky file header positioning on refresh and review updates.
- Fix: clean up the layout of the checks dropdown panel.
- Fix: guard against nil file values returned by the completion condition.
- Fix: recognize `.cjs` and `.mjs` files as JavaScript for syntax highlighting.
- Fix: hide comment actions until the sign in process is fully completed.  Clicking on an action early would cause a crash.
- Fix: improve server queue handling so tasks don't get stuck for a long time.  This should reduce the number of occurrences of "waiting for permissions" or "request queued but server did not respond" errors, among other things.
- Fix: don't crash with `$digest already in progress` when navigating to a pull request that doesn't have a review yet from the dashboard while signed in without `public_repo` scope.
- Fix: bump client timeout for user-requested review syncs from 30s to 60s.  This should reduce false positive timeout errors on big pull requests.

#### Release 3991.6302 (min 3619.5594 GHE 2.19+ or 3.0+ but not 3.10) 2023-08-03
- Upd: make `pr` variable available in coverage data URL templates.
- Upd: stop extending diff selection to cover entire lines, and add a Copy Lines item to the selection command palette instead that does this only on demand.
- Fix: don't get stuck forever retrying the status sync task when a review's file rename map is out of date.
- Fix: prevent a spurious error about "stripe is not defined" from being logged when users load a page.
- Fix: allow organizations to be renamed.
- Fix: avoid rare participant cell animation crash and guard against cell flickering in Firefox.
- Fix: adjust padding on magic keyword highlights.
- Fix: drop empty beginning / end of line ranges from selection, as well as leading and trailing empty lines.  This should make it easier to make clean selections and place the command palette closer to the actual selection in some edge cases.
- Fix: eliminate a redundant completion condition execution on pushes to the base branch.
- Fix: guard against an error (that seems to cause crashes for some people, though it shouldn't) when the Reviewable instance is running in private mode and has the analytics hook enabled.  (Regression introduced in v3923.6172.)
- Fix: prevent an "encryption not set up" crash when loading Reviewable without being signed in on an encrypted instance.  (Regression introduced in v3935.6189.)

#### Release 3980.6275 (min 3619.5594 GHE 2.19+ or 3.0+ but not 3.10) 2023-07-26
- New: launch temporary UI Experiments at https://experiments.reviewable.io/ to generate custom CSS combinations
- New: add a `REVIEWABLE_HOST_INACCESSIBLE` flag to indicate that the Reviewable host is not accessible from GitHub.
- Upd: move participants panel actions from discussions cell to the user cell dropdown.
- Upd: include requested reviewer teams in the participants panel.
- Upd: automatically format links when pasting a URL onto a selection in a draft comment.
- Upd: remove stealth updates of URL hash to match clicked discussion -- please use the dropdown menu to copy the discussion's URL to the clipboard instead.  (We also enabled this menu on top-level discussions.)
- Upd: add support for Codecov's v2 API. The old API (and its data format) will continue to be supported indefinitely as well.
- Upd: improve args reporting when capturing Firebase exceptions in Sentry.
- Upd: add `+by:username` and `+with:username` query terms to the dashboard.
- Upd: auto-delete empty drafts on click outside discussion, `Esc`, or preview/publish.  We no longer auto-delete on blur, as this was both too aggressive and inconsistent.
- Upd: show declaration headers for scopes that continue after a collapsed diff section.
- Fix: correctly align diff bound brackets on revision cells.
- Fix: keep inline code formatting from overlapping in rendered markdown.
- Fix: don't lock out discussion navigation after spamming diff selection.
- Fix: ensure 'edit in GitHub' links land on the correct GitHub page.
- Fix: make sure that marking as reviewed in the file matrix doesn't send you to a new tab or down the page.
- Fix: show file path ellipses appropriately in the file header (regression introduced in v3923.6172).
- Fix: avoid copying/quoting extra spaces (regression introduced in 3842.6109).
- Fix: minor layout fixes in drafts including keeping the draft layout from shifting when toggling between write and preview modes.
- Fix: avoid rare "maximum call stack exceeded" crash on the server.
- Fix: try to recover automatically if multiple merge queue entries share a common commit SHA.
- Fix: avoid error on dashboard when we fail to fetch a repository's default branch name.
- Fix: don't scroll to a random spot on the page when the first click lands on a discussion.
- Fix: Ensure publish previews are not accidentally hidden.
- Fix: Make sure file header always stays fixed in place when scrolling.
- Fix: Support file dropdowns opening from the status bar in Safari.
- Fix: add contextual help for the "merge latest changes into branch" button.
- Fix: back off query size in response to a wider range of errors on the dashboard.  This should help prevent "something went wrong" warnings.

#### Release 3955.6217 (min 3619.5594 GHE 2.19+ or 3.0+ but not 3.10) 2023-07-04
- New: allow repository admins to override Reviewable's status check for broken reviews.
- New: add option to start a file-level discussion to the file dropdown menu.
- New: let users bow out of a discussion to void their resolution vote and avoid becoming awaited whenever new comments are posted.  This is equivalent to dismissing yourself but bypasses the permission constraint.
- Upd: include author, state and title when rendering issue and pull request references in comments.  Distinguish between completed/dropped and merged/closed states.  At the same time, align state icons and colors with GitHub, including in issue autocompletion popup.
- Upd: show pull requests merged with `spr` as merged rather than closed in references and autocompletion popup.  (Going forward only -- no backfill.)
- Upd: accept pasting of rich text into drafts, with automatic conversion to Markdown.
- Upd: keep new revisions provisional even if a reviewer has the review open, as long as the tab isn't active.
- Fix: restrict file matrix modifier icons to show only when matrix is hovered.
- Fix: enable usage of alt as a hotkey modifier inside of drafts.
- Fix: allow copying out plain text from discussion comments, without Markdown decorations or escaping.
- Fix: allow copying text out of draft previews.
- Fix: improve placement of the selection command palette (regression introduced in v3842.6109).
- Fix: guard against rare crash caused by missing scroll element.
- Fix: keep modifier icons from getting stuck after using find in page on a Mac.
- Fix: avoid redundant underlines in file matrix file names.
- Fix: don't crash on sign-in when running with encryption configured.  (Regression introduced in v3935.6189.)

#### Release 3935.6189 (min 3619.5594 GHE 2.19+ or 3.0+ but not 3.10) 2023-06-20
**WARNING**: this release breaks sign-in on installations that configured encryption.
- Upd: improve latency for auto-connecting repositories if the repository creation event somehow gets lost, by listening to other repository-related events as well.
- Fix: improve Firebase data caching behavior on the servers.
- Fix: allow dashboard filtering by clicks on bot users.
- Fix: in Chrome, keep user filtering working when bot users are displayed on any PRs.
- Fix: Ensure code quote selection handles cannot get stuck behind discussions.
- Fix: avoid persistent high CPU loads after repeatedly moving back-and-forth between a large review and the dashboard (regression introduced in v3717.5839).
- Fix: adjust minor layout issues in collapsed diff regions.
- Fix: handle AWS Lambda `ResourceConflictException` errors more gracefully.
- Fix: prevent new GraphQL query that checks for merge queue activity from breaking under older versions of GHE.
- Fix: ensure file dropdown is not hidden by a discussion.
- Fix: keep collapsed region settings dropdown from being obscured by discussions.

#### Release 3923.6172 (min 3619.5594 GHE 3.11+) 2023-06-14
**WARNING**: this release only works with GHE 3.11+.
- New: add actions dropdown to file and discussion headers, including 'link to discussion' and 'edit in GitHub' actions.
- Upd: add hints to line link template field in account settings.
- Upd: send all analytics events via a queue in the database, so only the server connects to `REVIEWABLE_ANALYTICS_URL`.  While at it, retry failed requests a few times to smooth over any transient errors.
- Upd: respect the "BTW" keyword only in a user's first comment of a discussion, to reduce the risk of inadvertent disposition changes in follow up comments.
- Upd: highlight `.toml` files.
- Fix: prevent a crash on the reviews dashboard when Safari disallows access to local storage.
- Fix: don't offer quoting via the selection palette to signed out users.
- Fix: make normal (short) clicks work consistently on the Publish button even if the page has been loaded for a very long time.
- Fix: guard against rare crash in code block editor.
- Fix: prevent review from loading forever for some ill-fated draft discussion states.
- Fix: collapse discussions upon resolving (regression introduced in v3657.5690).
- Fix: don't crash when displaying a review with a milestone assigned.  Also clean up styling a bit.
- Fix: prevent page from being closed if an operation is in progress.
- Fix: don't show empty avatar image placeholders where no image would be shown.
- Fix: prevent crashes when hovering over participant panel cells.
- Fix: make sure file headers don't move past the status bar when scrolling.
- Fix: don't remove words that match dispositions names (e.g., "blocking") from comments posted to the pull request outside Reviewable.
- Fix: post Reviewable's status checks for commits created by GitHub's merge queues.  There's also a new `review.pullRequest.mergeQueueCheck` boolean property that lets completion conditions know whether they're running on a merge queue commit or on a normal one.
- Fix: don't try to fetch commit statuses for an undefined SHA.
- Fix: prevent users from signing out in the middle of a merge operation.
- Fix: copy icon no longer shows above long paths in the file header.
- Fix: remove user display name and avatar from Reviewable if they're deleted from the GitHub profile.
- Fix: wait for session to be resumed before sending analytics events, so we don't send anonymous events when the user was actually signed in.
- Fix: make file serving for the local uploads provider work again (regression introduced in v3807.5940).

#### Release 3842.6109 (min 3619.5594 GHE 2.19+ or 3.0+) 2023-05-15
- Upd: revert the "thanks" keyword for the Informing disposition, as it's too generic and has proven error-prone in practice.
- Upd: improve `?debug=latency` to log much more comprehensive page load latency stats.
- Upd: implement filtering by participant or author on the dashboard (try clicking on an avatar!), and move the author avatar to the pull request title as it was feeling a bit lonely out there on the right.
- Upd: automatically collapse groups with 200+ files, and don't count files in collapsed groups towards the threshold used to automatically hide the file matrix.
- Upd: add diff (patch) syntax highlighting, and don't mark a leading space on an otherwise empty line with a red dot in diffs of such files.
- Upd: add a command palette to comment selections that allows for explicit quoting of selected text, and remove auto-quoting of the selection upon draft creation for both comments and the diff.
- Upd: retain Markdown styling in comment quotes.
- Upd: show failing checks that are not required in orange in the status bar checks ring.
- Upd: combine GitHub 401, 403, and 500+ errors caught on the client into one Sentry issue per error code.
- Fix: infer GitHub search timeout failures in dashboard queries and do our best to reshape the queries until they succeed.  This should reduce or eliminate "something went wrong" errors.
- Fix: avoid overwriting selected text in draft when uploading an image.
- Fix: ignore "go to file matrix" command if the page doesn't have a file matrix.
- Fix: count reverted files towards the threshold that switches the view to single-file mode.
- Fix: fix regression introduced in v3663.5716 causing small regions of collapsed lines to be rendered in diffs.
- Fix: avoid a rare crash when moving directly from the dashboard to a review with a discussion target in the URL.
- Fix: ensure that discussions with a draft are always displayed, even if they're replied and resolved. This should avoid the situation where you have a red non-zero draft counter but clicking it doesn't navigate to a discussion.
- Fix: show command palette if only non-delta content selected in single-column diff.
- Fix: don't mix added/removed lines when making a selection in a code block diff.
- Fix: treat a required failing PullApprove status check as potentially successful when deciding whether publishing might trigger auto merge.
- Fix: set custom contrast settings when the page loads, not when account settings menu is opened.
- Fix: when following a permalink to a comment in Reviewable that originated from GitHub, correctly jump to and focus the comment.
- Fix: add missing contextual help for the selection command palette.
- Fix: ensure the selection command palette doesn't show up offscreen.
- Fix: make major improvements to participants panel loading performance in reviews with many participants.

#### Release 3827.6001 (min 3619.5594 GHE 2.19+ or 3.0+) 2023-04-11
- Upd: recognize draft pull requests more reliably, and indicate draft state in review status description.
- Upd: distinguish draft reviewer role with a green icon in the participants panel.  The role will become permanent (and visible to other participants) once drafts are published.
- Upd: count posts in the main discussion toward discussions shown in participants panel.
- Upd: add "thanks" as a keyword to set disposition to Informing.  (Like other keywords for Informing, it only works in the first comment of a discussion.)
- Upd: include `headBranch` and `baseBranch` in the available custom line link variables.
- Upd: add link back to the reviews dashboard to the bottom of the review page.
- Upd: revamp the avatars column on the reviews dashboard: improve the iconography, indicate more statuses than just GitHub approval, and add a tooltip to the ellipsis listing the participants that didn't fit.
- Upd: drop events from disconnected repositories that still have a webhook enabled earlier, to improve server performance and reduce pressure on Firebase.
- Upd: make code coverage indicators customizable via custom stylesheets.
- Fix: improve mapping of comments from GitHub into Reviewable.  We now correctly separate multiple threads attached to the same line, and sync file-level comments and line comments where we couldn't map the line to the top of the file.
- Fix: guard against a rare crash when a provisional renamed file disappears from the pull request.
- Fix: refine schema constraints to guard against partial review structures being written.  In very rare cases, if a write was happening while a review was being automatically archived, this could lead to a broken review.
- Fix: remove unnecessary "no statuses fetched" warning from logs.
- Fix: fix style bug that caused inconsistent font size in participant panel dropdowns.
- Fix: avoid obscuring disposition dropdowns which would happen when empty drafts were collapsed after their dropdown was opened.
- Fix: address visual bug where new disposition ribbon spanned full width of discussions.
- Fix: work around a bug in GitHub's Markdown renderer that would render some text with unterminated HTML elements as an empty comment.  This was always a problem, but since v3807.5940 it would also cause permission denied errors when trying to post such comments as we tightened up the schema.
- Fix: prioritize generated file detection over other special file natures (long lines, etc.).
- Fix: ensure Android mobile login works even with GitHub app installed
- Fix: fix extremely rare race condition when computing file rename matches that could result in a crash.
- Fix: don't spam a user who connected a repository then lost access to it with endless entreaties to sign back in.
- Fix: be more selective about eliding a status check context in the checks dropdown if it also shows up in the check's description.
- Fix: ensure that unreplied discussions are always shown in the diff.  Prior to this fix, it was occasionally possible to have a non-zero red discussion counter that did nothing when clicked, as the unreplied discussions were collapsed into gutter icons likely due to a race condition.  Incidentally, this fix also addressed similar issues when toggling trust-and-verify mode on and off.
- Fix: limit the width of discussions that appear in a diff viewer when the diff is collapsed, which would previously span the entire column, no matter how wide.

#### Release 3807.5940 (min 3619.5594 GHE 2.19+ or 3.0+) 2023-03-23
- New: add support for disabled repository connections via the `REVIEWABLE_DISABLED_CONNECTIONS` environment variable.
- New: the maximum upload file size is now configurable via the `REVIEWABLE_MAX_UPLOAD_IMAGE_SIZE` and `REVIEWABLE_MAX_UPLOAD_VIDEO_SIZE` environment variables.
- Upd: respect nested language when syntax highlighting discussion code blocks.
- Upd: add date and time to last active column dropdown and improve formatting for cells with multiple icons in the participants panel.
- Upd: add a "new comments for me" section to the dashboard that surfaces reviews where you have unread comments (including concluded ones!) but that aren't awaiting your action.
- Upd: accept uploads of `webm` videos as comment attachments.
- Upd: upgrade to Node 18 and Debian 11.
- Fix: apply syntax highlighting on kept diff lines in single column mode.
- Fix: revert to center aligned merge/closed/archived notices in the dashboard.
- Fix: strip extra whitespace that was copied when selecting over empty diff lines.
- Fix: properly handle commit messages generated when using squash merge.
- Fix: correctly style notification emails.
- Fix: ensure that some rare permission / token errors are attributed to the right user and will correctly cause a repository to be disconnected.
- Fix: detect "no admin permissions" errors when a specific user is designated to post badge comments for a repository, and handle appropriately.
- Fix: don't send a notification to a user who connected a repository they no longer have access to every time they sign in.
- Fix: don't display a spurious warning on the dashboard when the user was mentioned in a pull request they no longer have access to.
- Fix: detect declaration lines in Go code again.  This was a regression, going perhaps as far back as v3291.5093.
- Fix: don't crash when switching a draft to Pondering disposition after previously switching the discussion to a different disposition.
- Fix: when sending an individual comment, apply and clear out any other draft state associated with the discussion.  This includes applying any pending dismissals, and clearing out any acknowledgement so that the sent comment doesn't appear as new to the sender (!).
- Fix: relax database schema to allow certain writes of review-dependent data after a review has been archived.  These writes don't occur in normal operation but can, for example, be issued when migrating to another instance.
- Fix: avoid infinite busy loop in some situations where the review is in commit-by-commit review style and a file has been renamed then reintroduced into the pull request.
- Fix: treat posts in the main discussion and active dispositions in resolved discussions as qualifying for the reviewer role (as shown in the participants panel, and indirectly on the dashboard).

#### Release 3717.5839 (min 3619.5594 GHE 2.19+ or 3.0+) 2023-02-10
- New: add participants panel for quickly viewing all user roles, status, other stats, and taking user-related actions.
- Upd: show account settings while scrolling so that visual setting updates can be seen in real time.
- Upd: update user contrast settings to allow for customizing diff background colors for better accessibility.
- Upd: add "tip" as a keyword for setting disposition to Informing.
- Upd: show requested review teams on the dashboard, and include such pull requests in `needs:reviewer` and `needs:me` filters as appropriate.
- Upd: refresh the list of pull requests when reloading a dashboard page, even if the last update falls below the normal refresh threshold.  This way, if you know there's a new pull request you can just reload the dashboard rather than waiting for the next automatic update.
- Upd: don't hide "obsolete" files.  We used to hide files that were reverted and reviewed altogether, but this was confusing and now that we have an automatic Reverted group in the file matrix we can just rely on that instead.
- Fix: don't count rate limiting-driven retries as task execution attempts for the first 2 hours, to lower the rate of false positive "Repeatedly failed to process event" errors.
- Fix: correctly handle the case when a pull request is missing its head commit.  This was a regression from some time ago.
- Fix: guard against a rare race condition that could lead to a crash when there are renamed files in the review.
- Fix: don't show the tip for Satisfied keywords when the user's disposition is Informing, as they're not useful.
- Fix: avoid continuously rewriting badge links if the organization or repository name contains uppercase characters.  This regression was introduced in v3690.5783 and could lead to quota exhaustion.
- Fix: display correct unreviewed file count when a custom review completion condition sets `reviewed: false` flags on some files.  Also fixed a bug that could lead to worse-than-expected unreviewed file count estimates for pull requests with renamed files.
- Fix: if a file was brought into the review with a provisional revision, and that revision is replaced with another one that doesn't modify the file, correctly remove the file from the review.
- Fix: don't misattribute GitHub errors encountered by the server when using a secondary account to a repository's primary connecting account.  If the secondary user was suspended, for example, this could cause the repository to disconnect even though the connecting user's credentials were perfectly fine.
- Fix: don't cross out disposition keyword if it matches the manually set disposition.  This happened most often when clicking "Done" in v3690.5783 and could be quite confusing.
- Fix: avoid crash on the Repositories page.  This was a regression introduced in v3690.5783 and affecting only the Enterprise installs -- sorry!
- Fix: avoid error message on the reviews dashboard when a cached pull request belongs to a repository the user no longer has access to.

#### Release 3690.5783 (min 3619.5594 GHE 2.19+ or 3.0+) 2023-01-15
- Upd: rewrite the Reviewable badge in the pull request if it points to the wrong review in the same Reviewable instance.  Previously, we'd keep the incorrect link and report an error, but this proved hard to notice and fix in the common case where a developer copies a description (with badge) from an old pull request to a new one.  This scenario should now be handled correctly automatically.
- Upd: log summaries of queue task execution outcome and performance in JSON format.  If you don't have `statsd` set up then you can parse and aggregate this data instead to get some pretty charts of Reviewable server performance.
- Upd: add support for turning off the LGTM/approve button in reviews.
- Upd: indicate with a colored ring when a user is changing their disposition to one that would block resolution, or one that would resolve the discussion.  We also removed the old "resolving" indicator (a tiny icon in the draft ribbon that likely nobody noticed) and added notes about becoming newly blocking to the dispositions dropdown.
- Upd: highlight disposition-changing keywords in drafts and pop up a tooltip explaining what's going on if needed.  If a keyword is being ignored, cross it out and explain why in the tooltip.
- Upd: introduce keywords to set a draft's disposition to Blocking ("bug", "major", and "hold on").
- Upd: show source and target branch for each pull request on the dashboard, and elide owner and repository name when not needed (e.g., in an organization-specific or repository-specific list).  The branch names are always shown in full in the (new) tooltip, and also inlined when the owner or repository are elided.
- Upd: cache dashboard query results to greatly improve page load times, and indicate result age under the header.
- Upd: offer link to a repository-specific dashboard (that shows all pull requests in the repository) when you enter something that looks like a repository name in the dashboard's query field.
- Fix: guard against review size getting blown out due to a multitude of long GitHub comments that nonetheless fall under the per-comment length cap.  This would result in the review trying to sync forever, and repeatedly causing regular Firebase disconnects.
- Fix: improve dashboard layout at low screen widths.
- Fix: prevent occasional endless failure loops when a review exceeds the max file limit.  The review should now be marked reliably as being in an error state.
- Fix: clear error message on dashboard after a successful fetch.
- Fix: avoid small visual hitch when activating organization dropdown on dashboard.
- Fix: request `workflow` scope from the user if merging a pull request with a file in `.github/workflows` fails, and retry.
- Fix: use the original casing of owner and repository names in badge and comment links.
- Fix: use the original casing of user and team names in `@` autocompletions in draft comments.
- Fix: guard against a rare crash when leaving a review page while a diff operation is in progress.
- Fix: avoid flash of unstyled content (FOUC) when loading reviewable and the icon font isn't cached.

#### Release 3663.5716 (min 3619.5594 GHE 2.19+ or 3.0+) 2022-12-01
This is mainly a bugfix + stabilization release.
- Upd: hide metadata-only diffs in the virtual Commits file when commits were rebased without changing the commit message.
- Upd: improve error message when GitHub returns an invalid response to a permissions query. Experience shows this is likely due to the user having received a bad OAuth token for some reason, so the new message recommends signing out and back in.
- Upd: ensure the file matrix is always wide enough to fit directory names, and let them extend past the edges on hover.
- Fix: prevent permission denied error when loading some reviews in repositories with mixed case names.
- Fix: prevent an extremely rare crash on the review page due to some review metadata being undefined.
- Fix: reinstate author and committer information in the completion condition's input data for review revisions.  This was a regression in v3619.5574.
- Fix: prevent "mergeable block range mismatch" errors when diffing files with complex base changes.  This was a regression in v3657.5690.
- Fix: eliminate a potential race condition in invalidating Reviewable pull request statuses when quickly disconnecting and reconnecting a repository.  This could've theoretically resulted in pull requests showing an error status of "Repo disconnected, unable to update review status", even though the repo had been reconnected.
- Fix: if a review enters an error state (e.g, because there are too many files in the pull request), set the head commit's status check to an error as well if needed.  Previously we left the last status check in place, or didn't set one on new commits at all.
- Fix: ignore account suspended errors emitted by GitHub when fetching user information, in an attempt to prevent unnecessary failures and repository disconnections.  It would appear that GitHub can return a 403 Account Suspended error on some API requests if the *target* is suspended, even thought the request's originator is fine.  This supersedes the attempted fix in v3619.5574, which was ineffective.
- Fix: avoid using `SharedWorker` in Safari 16, as their newly-added support is half-baked.
- Fix: guard against a rare crash with error `this.tracker.resolvedIf is undefined`.
- Fix: guard against a rare crash when trying to change the default review overlap strategy for a repository.
- Fix: guard against a rare crash on the dashboard when a pull request you were mentioned in has disappeared.
- Fix: guard against a rare crash in the code snippet editor, cause by a race condition when, e.g., switching diff bounds.

#### Release 3657.5690 (min 3619.5594 GHE 2.19+ or 3.0+) 2022-11-04
- New: track which apparently modified file revisions only have base changes, and make this information available to custom completion conditions.  See the [announcement](https://headwayapp.co/reviewable-changes/base-changes-only-247444) for details.
- Upd: update the discussion interface for showing/hiding old comments in the current discussion, current file, and across all files in a review.  Add a convenient control for showing just one more older comment while at it.
- Upd: allow image files to be drag-and-dropped directly onto a "Reply..." or "Follow up..." field, creating a draft automatically.
- Upd: add ability to double click a draft preview to return to write mode.
- Fix: correctly determine which lines in a diff have base changes only.  Before this fix, base changes flags could sometimes bleed into adjacent lines within the same block.
- Fix: do a better job of diffing around the transition from a base only change area to a normal change one.  Previously, it was possible for a line where the left and right sides were clearly a single edit to get split into two parts that weren't diffed together.
- Fix: bring back the participants overflow ellipsis on the dashboard.
- Fix: don't busy-spin forever on a file when it needs to be shown (e.g., because there's an unresolved discussion) but no diff bounds are set for some reason.  This was a regression introduced in v3542.5405.
- Fix: don't show the user's own main thread comment as new/unread if the pull request's branch was pushed to after the comment was drafted but before it was sent.
- Fix: work around more rare and invisible errors when setting the "user's last interaction with review" timestamp.
- Fix: correctly interpret merge message default settings in older versions of GHE (we think 3.6 and below), so that you don't end up with merge commits with the message `undefined`.  This was a regression introduced in the previous release.
- Fix: cull PR polling enrollments ("My PRs in any public repo", etc.) that are failing with errors more aggressively.
- Fix: lower the "review too large to process" threshold from 8000 to 4000 files.  We've seen instances of reviews with more than 4000 files falling into endless update timeout loops.

#### Release 3644.5629 (min 3619.5594 GHE 2.19+ or 3.0+) 2022-10-16
- Upd: add `REVIEWABLE_ENCRYPTION_AES_ENABLED` flag that you can set to tell Reviewable that you expect `REVIEWABLE_ENCRYPTION_AES_KEY` to be set as well.  This can help you detect cases where the key silently failed to be fetched from some vault system and prevent Reviewable from accidentally running without encryption.
- Upd: improve squash and merge commit messages to closely mirror Github defaults set for the repository.
- Fix: prevent client from getting stuck "Mapping renamed files".  This likely only affected old archived reviews.
- Fix: improve GitHub error identification in various spots, helping invalidate repository connections and organization-wide auto-connection earlier.
- Fix: address a Firefox bug where using the diff line selection button would lock down text selection inside discussions.
- Fix: prevent occasional `First argument contains undefined in property` errors that would happen on first click of a session.
- Fix: prevent "Resuming session" spinning forever when signing in from the home page in an instance running in private mode.
- Fix: don't show the "(Repository default)" annotation if the review overlap strategy dropdown isn't also showing.
- Fix: actually check the user's display name for "(bot)" when syncing comments, and sync the correct name into our database.
- Fix: optimize the number of queries used to fetch PRs the user was mentioned in on the dashboard.  Before this fix, if you were mentioned a lot in review comments on GitHub, we could potentially emit hundreds of requests!  This should be cut down to one or two now.
- Fix: guard against rare `Cannot destructure property 'preferredTitle'` crash.
- Fix: include new blocking discussion drafts in unresolved discussions count and navigation cycle.
- Fix: include `review.deferringReviewers` in completion condition sample input data.  This was always included in "real" evaluations of a condition, but accidentally omitted from the settings playground.
- Fix: work around rare (invisible) errors when setting the "user has drafts pending" flag, which gets shown in the participants panel.
- Fix: accept file uploads in the review summary draft when no text was entered.
- Fix: keep virtual `Commits` file contents stable in the face of unusual commit histories.  This could lead to discussions on the file being "lost", where they'd still be counted but fail to be displayed anywhere.
- Fix: prevent multiple scrollbars from showing up in long drafts, and correctly position DRAFT watermark and drag-and-drop landing area.

#### Release 3619.5594 (min 3619.5594 GHE 2.19+ or 3.0+) 2022-09-22
**WARNING**: a bug in recent older versions breaks rollbacks, so the minimum version here is set to prevent them.  If you really need to roll back please get in touch -- we know how to craft a workaround given a specific version to roll back to.
- Fix: don't break encryption when opening Reviewable in multiple tabs simultaneously (regression introduced in v3619.5574).

#### Release 3619.5574 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-09-20
**WARNING**: this release is broken if you're using encryption.
- New: add visual warnings if auto-merge is enabled (wand icon in publish button and checked box in publish dropdown). Also require hold to arm when publishing a review that Reviewable thinks may trigger merging of the pull request.
- New: add a repository-level setting for the default review overlap strategy.  This will override personal settings, but can in turn be overridden by any user for themselves for a given review.
- Upd: modify some disposition keywords, used to infer your disposition when found at the beginning of a comment.  We now always map `FYI` to Informing and `BTW` to Discussing, whereas before their meaning was context-dependent (new discussion vs reply).  We also removed `OK` as it resulted in too many false positives when reviewers used it to start a longer sentence.  Previously inferred dispositions will remain unchanged, but if you edit a previously created draft in this release it will be re-interpreted under the new rules (even if you don't edit the keyword itself).
- Upd: add a per-user toggle to disable disposition inference from message keywords.  It's hiding in the disposition settings panel, accessible via the small gear in the top-right of any of your disposition dropdowns.
- Upd: when inferring a disposition from a keyword in the comment draft, shortly flash the disposition icon to attract the user's attention to the connection.
- Upd: change the default disposition for author-initiated discussions from Blocking to Discussing.  The old default was unintuitive, and often resulted in authors accidentally blocking their own reviews.  The disposition can still be changed explicitly, and each user can set a different personal default for this scenario, of course.
- Upd: list co-authors at the end of default merge commit messages. Note that these are aggregated per review and not per revision/commit.
- Upd: switch the completion condition editor to our own and remove the dependency on CodeMirror.  This should have no end-user impact but helps slim down the build.
- Upd: move the open source dependencies list and the licenses recitation to an in-app page.
- Upd: add `REVIEWABLE_SENTRY_DEBUG` config flag to turn on Sentry debug mode.
- Upd: add workarounds for GHE's propensity to declare a user suspended when under heavy load even though they're not.  There's a workaround that should work automatically, but if you're still seeing an issue with repositories getting disconnected with a bogus "GitHub account suspended" message please get in touch to learn how to set up a stronger mechanism.
- Upd: upgrade to the latest versions of the Firebase SDK, up by a couple major versions.
- Fix: implement workarounds for getting stuck waiting on permissions and "request queued but server did not respond" issues (that often blocked publishing).  The root cause lies in the Firebase SDK, but will need to be isolated (by us) and addressed by Google at some point.
- Fix: make the order of files in the page match the order of files in the matrix.  This was a regression introduced in v3550.5439.
- Fix: allow anonymous access to public repos again (broken in v3581.5517).
- Fix: allow users to continue deferred reviews that have been closed or merged by clicking the "Continue review now" button.
- Fix: prevent "Mark as reviewed and move to next and show next diff" from showing up at the bottom of a file.
- Fix: guard against a rare `parentNode is null` exception when creating a code block.
- Fix: respect Sentry DSNs configured via the `REVIEWABLE_CONFIG_FILE` instead of directly as env vars.
- Fix: don't overwrite newer database schema when rolling back.  It's not clear when this regression was introduced, but you should assume that recent versions will not roll back correctly.

#### Release 3581.5517 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-08-31
- New: add ability to create single and multiline code suggestions from inside a comment draft and provide handles for manipulating line selection.
- Upd: use the SVG format badge in comments as well, as GitHub now appears to be supporting this.
- Upd: start sampling certain too-common Sentry events meant for debugging.  If you have Reviewable hooked up to Sentry you'll see an apparent drastic reduction in some high volume events.
- Upd: support `maintain` and `triage` permission levels and add `maintain` as an option for discussion dismissal authority. You may need to wait 30-60 minutes after deploying this release before user permissions get refreshed and the new levels become usable.
- Upd: allow `triage` permission level to use various add/remove directives in comments.
- Upd: display a warning message in the diff if a revision contains more commits than expected.
- Fix: enforce line wrapping at the correct character based on margin setting.  This used to wrap some lines early depending on their content (!).
- Fix: publish only comment text when sending ad-hoc top level comments.  The original fix in v3512.5320 didn't work right in some environments.
- Fix: guard against crashes when creating a discussion on the base revision of a file that was renamed multiple times within one pull request.
- Fix: correctly handle rename chains where a file two or more renames away gets reintroduced into the pull request.  Previously, this could cause such a reintroduced file to get stuck with no default diff bounds, and no way to set any.
- Fix: don't insert random `null`s into code block diffs.  This regression was introduced in v3550.5439.
- Fix: set the focus to the draft after clicking Approve or LGTM.
- Fix: delete the draft altogether if you clear all text from the review summary and click out of the field.
- Fix: ensure that clicking LGTM in the review summary always inserts the LGTM into the draft.
- Fix: show the same number of unresolved discussions in the review summary counter as we do in the toolbar.  (The bottom number wasn't taking into account the drafts about to be sent.)
- Fix: ensure that dashboard doesn't get stuck in a "Loading" state if some very old reviews are in the list.
- Fix: guard against rare "can't access dead object" error in Firefox.
- Fix: correctly track when the review's completion state was last computed.  This bug resulted in running completion and review state updates more frequently than necessary.

#### Release 3550.5439 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-08-15
- New: add the ability to write a review summary and publish the review from the bottom of the page. For more information, see [the docs](https://docs.reviewable.io/reviews.html#publishing-your-review).
- New: detect vendored files (based on simple path heuristics) and give them special treatment:  put them into a group that's collapsed by default, and exclude them from renamed file matching.  See [announcement](https://headwayapp.co/reviewable-changes/vendored-files-support-240076).
- Upd: add a badge condition setting to limit the circumstances under which a Reviewable badge will be added to the PR, even in a connected repository.
- Upd: display a message in the diff when a file has been reverted to base at its last revision.
- Upd: add an icon to the approve / block specifier that shows in the Publish button.
- Upd: increase the threshold for "show just one file at a time" mode from 25 to 50 visible diffs.  If this doesn't lead to performance issues we'll probably increase it further.
- Fix: prevent "review missing last revision" errors.
- Fix: ensure that all participants being waited on are shown to the right of the pointing hand on the dashboard page, even if they haven't started working on the review yet.
- Fix: don't remove empty lines when rendering code blocks in comments.
- Fix: avoid rare but persistent permission denied error when updating a review's completion status that would cause the update to get stuck, potentially forever.
- Fix: when loading a review link with an anchor to a discussion made on a base revision, scroll to the discussion instead of locking out the file with a spinner.
- Fix: guard against some rare transient crashes on the client, on the review page and on the dashboard.

#### Release 3542.5405 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-07-31
- Upd: prevent file matrix bulk diff bound selection from diffing files in collapsed groups, unless they need to remain displayed (e.g., they're unreviewed or have discussions).  This can help with hiding grouped files from the page while retaining the ability to quickly manipulate the diff bounds of remaining ones.
- Fix: allow in-comment badges to be created in unconnected repositories even if the publishing user isn't the author and doesn't have admin rights.
- Fix: refresh the review after publishing in unconnected repositories.
- Fix: successfully complete review completion condition tasks in connected repositories even if the connecting user's token has disappeared.
- Fix: add breakpoints for wrapping long branch names in the commits section of the Changes panel.
- Fix: fix visual glitch in file headers where the revision cells were partially obstructed.
- Fix: address lack of text overflow handling in the codeblock viewer that made long lines inaccessible.
- Fix: be more tolerant when trying to unarchive reviews with slightly malformed data.
- Fix: exclude bots from "new messages for" lists in webhook notifications.
- Fix: when selecting bounds by dragging in the file matrix header, include files that may not be present in the PR at the bounds' exact revision, but do have changes in between.  (This fix also ironed out a bunch of other edge cases having to do with missing revisions.)
- Fix: distinguish between one's own and other people's review marks in later revisions when in "personally review every file" mode.  (The two states are still visually conflated, both being represented by a grey button with green rim, but are now treated differently internally when appropriate.)

#### Release 3537.5352 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-07-09
- Upd: let all GHE site admins access the license details panel on the Repositories, not just the designated license admin.  At this point, the designated license admin account is only needed as a fallback for "anonymous" GitHub requests when GHE is running in private mode.
- Upd: when configured with `REVIEWABLE_UPLOADS_PROVIDER=gcs`, optionally use ambient GCP credentials instead of a private key.
- Upd: subdivide `statsd` counter names by action for tasks on the `requests` queue, and if a request times out report its action in the error message on the client.  Also add a new `task_waiting_time` timer that measures how long a task was waiting in the queue before getting picked up (the first time only, so we don't measure retries).
- Upd: add comment/copy actions in a command palette that appears when text is selected inside a diff block.
- Upd: add `?debug=firebase` for debugging of the low-level Firebase protocol.
- Fix: back off mergeability sync retry interval up to 15 minutes in case it's taking a long time to settle on GitHub.  Also capped retries at 1 hour; after that, the user can force a sync by visiting a review.
- Fix: avoid a vicious feedback loop that could occur if events were received for merged or closed PRs in archived repos.
- Fix: don't HTML-escape the inside of code blocks in a pull request title.
- Fix: fix 'no end-of-file newline' icons so they appear properly.
- Fix: eliminate erroneous copying of the small, light-colored declaration that appears after collapsed regions when a user makes a selection that overlaps it.

#### Release 3512.5323 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-06-08
- Upd: add `?debug=navigation` debugging mode.
- Fix: allow license admin to load Repositories page without causing a permission denied crash.
- Fix: guard against a handful of client crashes caused by very rare conditions.

#### Release 3512.5320 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-06-02
**WARNING**: access to the Repositories page by the license admin is broken in this release.
- New: add support for HSTS with `REVIEWABLE_STRICT_TRANSPORT_SECURITY`.  It defaults to off for backwards compatibility, but you can set it to any valid header value (e.g., `max-age=31536000`) to turn it on.
- Upd: respect GitHub hidden comments in Reviewable.
- Upd: switch to a more comprehensive icon set and update some icons.
- Upd: add `Referrer-Policy: strict-origin-when-cross-origin` header to all responses.
- Upd: upgrade to Sentry SDK v7.0  If you have a self-hosted deployment of Sentry you need to upgrade it to at least v20.6.0.
- Upd: add `?debug=crash` to log more information to the console in case of a "broken goggles" crash.
- Fix: escape Markdown-sensitive characters in file paths when publishing comments.
- Fix: explicitly increment parent counters for `statsd` and fix `github.request.*` names to reflect queues, not task IDs.
- Fix: show function/class headers when part of the block is collapsed in a diff. (It's not entirely clear when this regression was introduced -- possibly as far back as mid-2021.)
- Fix: make the review summary draft area in the bunny drop-down menu work properly again.
- Fix: publish only comment text when sending ad-hoc top level comments.
- Fix: allow old reviews to be opened (and unarchived, if necessary) in archived repos.
- Fix: fix some small bugs with editing coverage report fetch settings.
- Fix: notice unclosed extended code fences in comments (more than 3 backticks) and close them properly when forming batch message.

#### Release 3477.5231 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-04-17
- New: add support for tracking `spr` stacked pull requests. Please see [the announcement](https://headwayapp.co/reviewable-changes/support-for-git-spr-(stacked-pull-requests)-228260) for more details.
- Upd: be more aggressive about hiding files that the user doesn't need to review.
- Upd: use `reviewed` flags set by custom completion condition when computing default result values, including `completed` and `pendingReviewers`.
- Upd: switch to CSS-native hyphenation, which should be more reliable nowadays than the JS module we've been using so far.
- Fix: log exceptions properly when neither Sentry nor a log sink are set up.
- Fix: check license team constraints correctly.  This got broken in v3415.5193 such that every membership check returned false.
- Fix: set default disposition correctly when it's already set as the discussion's disposition.
- Fix: ensure that mentioning a user in a Reviewable comment makes the review show up on their dashboard.
- Fix: don't hide significant whitespace deltas by default (e.g., whitespace changes in string literals).
- Fix: don't insert random `null`s into code block diffs.

#### Release 3424.5210 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-03-31
- Upd: mark files that have open discussion with a small icon in the file matrix.
- Upd: don't snapshot revisions when a bot comments on the PR.  (See [here](https://docs.reviewable.io/tips.html#ignore-comments-by-bots) for how we detect whether a user is a bot.)
- Fix: reduce logged exception false alarms originating from status / check events.
- Fix: save repository settings even if unchanged when selecting multiple repositories to apply to.
- Fix: fall back to procedurally generated avatar images if the real ones fail to load due to [issue #770](https://github.com/Reviewable/Reviewable/issues/770) or because the user is not signed in to GitHub.
- Fix: remove extra dot from trailing whitespace decoration.
- Fix: allow one-off send of a comment with only code blocks and no text.
- Fix: correctly display suggestion code blocks that delete all of the original code.
- Fix: lower query chunk size when running searches from the Reviews page to (hopefully) avoid 502 errors.
- Fix: don't add a horizontal rule to the message when doing an immediate send of a single comment.
- Fix: wrap long branch names at the edge of the Changes panel.

#### Release 3415.5195 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-03-09
- Fix **SECURITY**: encrypt data used to keep track of AWS Lambda functions in Reviewable's database.  If you use AWS Lambda for custom condition execution and ran any version of Reviewable between 3256.5037 and 3415.5193 (inclusive) with encryption enabled, then some repository names were exposed unencrypted in the database.  No other data was exposed, and thanks to our two-level security setup (encryption + security rules) it's extremely unlikely that anybody actually saw anything.  This release will gradually fix things up over the week after you install it, but you can manually delete `/lastExecutionTimestampByContainerName` in the Firebase console if you'd like instead.

#### Release 3415.5193 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-03-07
- Upd: allow requested reviewers, assignees, and labels to be added directly in the Github PR description. Note this follows the same convention as used on Reviewable.
- Upd: add ability to reverse iteration of files, comments, and drafts by using `⇧ + click` on respective counters in the status bar.
- Upd: try to grab Firebase credentials from ambient sources if none of the Reviewable-specific environment variables are set.
- Upd: upgrade AWS SDK to v3 to reduce distro size.
- Fix: migrate team constraint checking away from deprecated GitHub API.
- Fix: don't swamp GitHub with requests when a review has a lot of image files.
- Fix: avoid a very rare server-side failure when updating review state.
- Fix: improve processing and display of exception stack traces when executing completion conditions in AWS Lambda.

#### Release 3391.5185 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-02-08
- New: introduce guest passes to make license seat management more flexible.  If a user is prevented from obtaining a normal seat -- whether because they're all allocated to others or because there's a team constraint applied to the license -- they're given a temporary guest pass seat instead.  A guest pass is valid for 14 days, doesn't count against the licensed seats, and there's no limit to how many can be handed out.  However, once a user has signed in (whether to a full or guest seat) they'll only be eligible for another guest pass after 90 days, so this is not an alternative to getting a normal licensed seat for long term users.  We hope this helps others in your organization try out Reviewable without having to worry about exhausting your license!
- New: support executing completion conditions in Azure Functions.  For details see the newly added [instructions](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#user-code-execution) for the `azurefns` executor option.
- Upd: add buttons to copy filename in file and discussion headers.
- Upd: darken link underlines in comments to make them easier to spot.
- Fix: fix the `vm2` code executor that was broken in v3371.5161.
- Fix: implement a failsafe to prevent getting stuck with a dead stickied file header when switching files in single-file mode.
- Fix: allow marking a group as reviewed even if some files are diffed at a different right bound, as long as they have no valid revisions between their bound and the dominant revision.  This is especially useful for the Reverted group since files there won't have any revisions after the reversion.

#### Release 3371.5161 (min 3340.5125 GHE 2.19+ or 3.0+) 2022-02-02
**WARNING**: the `vm2` executor is broken in this release.
- New: add a new "pondering" disposition that prevents a draft from being published.  Useful for notes-to-self while reviewing that you'll delete or flesh out before sending.
- Upd: include reviews where the user is blocking (i.e., in `pendingReviewers`) on the dashboard even if they're not involved with the pull request from GitHub's point of view.  This is helpful for custom completion conditions that assign pending reviewers from a hardcoded list, rather than just managing the users already participating in the review.
- Upd: support omitting "Reviewable" badges in public repos.
- Upd: update server build process.  There should be no impact on vanilla deployments.  If you customized things please note that the new image entrypoint is `node dist/main.js` and the server code is now minified.
- Upd: update syntax highlighting library with improvements to many language definitions.
- Upd: improve browser caching behavior when serving the page.
- Upd: note on startup when Sentry is active and capture an info message to help in debugging the configuration.
- Fix: replace astral plane Unicode characters with a placeholder before posting a custom completion description to Reviewable's GitHub status, as GitHub can't handle them.  Remember kids, fancy emojis are all fun and games until you run into a database that only supports UCS-2!
- Fix: avoid crashing when creating a comment after a text selection in the diff has disappeared but is still active.
- Fix: allow reviews with codeblocks sourced from a different file than the parent discussion to be unarchived correctly.
- Fix: prevent rare crash when deleting a draft codeblock.

#### Release 3359.5145 (min 3340.5125 GHE 2.19+ or 3.0+) 2021-12-22
- New: let reviewers suggest code changes, with a mini code editor to make writing them easier and a mini diff to show what changed.  Please see [the announcement](https://headwayapp.co/reviewable-changes/code-suggestions-216787) for more details.
- Upd: omit pull requests in archived repositories from the reviews dashboard.
- Upd: show a "go to next file" button at the bottom of previously reviewed diffs when operating in "too many files" mode.  This lets you easily page through a long review even after you've marked everything reviewed.
- Upd: use a more efficient GraphQL query for finding pull requests matching a commit SHA when processing status events.
- Upd: put deferred reviews where the user has at least one "working" disposition into a new "working on it" section on the dashboard, rather than the generic "being reviewed by me" section.
- Fix: don't crash when making some specific kinds of edits to the code coverage settings.
- Fix: snapshot the current revision when creating a new top level draft comment.  In edge cases, if the pull request author created such a comment and left the revision provisional, the review could get into an invalid state and fail to update.
- Fix: query PRs much more efficiently when you have "Also show pull requests you're not involved with from all repos to which you can push" checked on the dashboard and limited to one or both of starred or watched repositories.
- Fix: correctly update which revisions are obsolete when squashing commits with no other changes.  This affects the commits shown in the virtual Commits file when diffing against base.
- Fix: drop status update events when fetching a list of checks and statuses results in repeated 502 errors from GitHub.
- Fix: avoid getting into an infinite loop of refreshing permissions when a user is signed in on the client but their token is missing server-side.  This shouldn't happen under normal circumstances.

#### Release 3340.5125 (min 3107.4890 GHE 2.17+ or 3.0+) 2021-11-25
- New: display code coverage in diffs using a thin color bar.  You'll need to configure access to coverage reports on each repository's settings page.  To start with we're only support the Codecov report format but are open to adding more.  See [the docs](https://docs.reviewable.io/repositories.html#code-coverage) for details.
- Upd: don't update archived reviews in response to GitHub webhooks. Closed reviews are archived within 30-60 days after they're created or last accessed by a user, and open reviews within 180-210 days.  This means that the completion condition won't be triggered on archived reviews even if the state changes, and the GitHub status won't be updated either.  A visit to the review will automatically unarchive it and bring everything up to date again.
- Upd: automatically add an `[archived]` annotation to the GitHub status of reviews when they're archived.
- Upd: hide editor and GitHub links behind the line number in discussion headers.
- Upd: when evaluating the completion condition, use internally available data to map usernames to user IDs when possible instead of always fetching it from GitHub.
- Upd: if the completion condition sets the `reviewed` flag for (a revision of) the synthetic commits file, don't require users to mark it as reviewed.  This will turn the red mark reviewed button to a green-rimmed one and ensure that the file doesn't cause review deferral.
- Upd: add "typo" to the list of prefixes that will change the disposition to "discussing".
- Fix: keep better track of requested reviewers in Reviewable, updating more eagerly and avoiding false positives in computing pending reviewers.
- Fix: process `check_run` and `check_suite` events.  They were being dropped by mistake, though the checks status was still refreshed when the review was loaded.
- Fix: don't crash when clicking on link to GitHub (though this often went unnoticed since clicking the link navigates away from Reviewable).
- Fix: sync drafts correctly between tabs.
- Fix: improve styling for images in comments, removing the underline.
- Fix: escape pull request / review titles for display.  This wasn't a security issue because the values were sent through `DOMPurify` but if your title included HTML-like tags they wouldn't show up properly.

#### Release 3291.5093 (min 3107.4890 GHE 2.17+ or 3.0+) 2021-11-04
**NOTE**: if you're using AWS Lambda for executing completion conditions, you need to grant the user (or role) the `lambda:GetFunctionConfiguration` permission, on top of the other ones it already needs.  Until this permission is granted the first execution of a condition in a repository will fail, and subsequent ones may very rarely fail as well with an `AccessDeniedException`.
- Upd: increase timeouts related to handling interactive client requests on the server from 5s to 15s.
- Upd: display instructions for adding/removing labels, assignees and reviewers in the corner of the top level draft while it's empty.
- Upd: improve editor link settings section to be easier to use without having to refer to docs.
- Upd: change the discussion line links into two separate icon links to GitHub and the editor.
- Upd: upgrade HighlightJS to the latest version. This has resulted in some strange crashes on page load reported for reviewable.io but after extensive investigation we believe they're false positives.  Please let us know if you run into trouble, though!
- Upd: allow other ways to inject AWS credentials into Reviewable for Lambda and S3 besides specifying `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.  Using these environment variables will continue to work, though if you forget to set them (and don't have any other means for the SDK to get credentials) the error message might not be as clear.
- Fix: don't erroneously consider a file reviewed if it has been reverted and newer revisions created in the review.
- Fix: set max-width on videos to 100% to avoid overflow.
- Fix: allow user to correctly select and copy lines near the beginning or end of a diff.
- Fix: don't snapshot revisions when the PR author is looking at the review unless required for data integrity.  Provisional revisions should now remain provisional more reliably until a reviewer visits the review.
- Fix: wait for Lambda function to become active before invoking it.
- Fix: if a completion condition sets `syncRequestedReviewers` but we can't find an admin account associated with the repo to do the work, fail the status check with an error rather than getting stuck in an infinite retry loop.

#### Release 3268.5067 (min 3107.4890 GHE 2.17+ or 3.0+) 2021-10-19
- Upd: indicate in synthetic `commits file` that commits were added to a revision if they didn't affect files in the pull request.
- Upd: display warning indicator when avatar images aren't loading and suggest signing in to GitHub again.
- Fix: don't escape Markdown characters between code blocks or backticks in pull request title.
- Fix: improve performance when a review has lots of drafts open.
- Fix: preserve branch name when the branch has been deleted.
- Fix: escape spaces and other reserved characters in a filepath when forming links to GitHub or an external editor.

#### Release 3256.5037 (min 3107.4890 GHE 2.17+ or 3.0+) 2021-10-02
- Upd: upgrade to Node 16.
- Upd: support Lodash 4.x in custom completion conditions.  Note: please don't advertise Lodash 4.x support until you're confident you won't need to roll back to avoid conditions running against the older 3.x module. See [announcement post](https://headwayapp.co/reviewable-changes/lodash-4-x-support-206733) for details.
- Upd: move completion condition examples out of the app and into a repository, replacing the examples dropdown with a link.  This will make them easier to reference and maintain.
- Upd: allow multiple Reviewable instances (e.g., production and staging) to use the same AWS Lambda instance for condition execution without stepping on each other's toes.
- Upd: increase timeout and improve stagger logic for permission-checking requests to GHE.  This should help avoid spurious permission check failures if your GHE instance is overloaded an unable to reliably respond within 3 seconds.
- Upd: improve error message when Reviewable refuses to create a review because it found an existing review link in the PR, and tweak a bunch of code around there to make it easy for a user to recover from this state.
- Upd: when following a link to a discussion, diff all the files instead of just the discussion's host file. Skipping file diffs can make the page load faster but it's confusing, and not really necessary on modern machines.
- Upd: parse language identifiers out of shell script shebang lines to select correct syntax highlighting.
- Upd: switch from using just the Firebase identifier (`REVIEWABLE_FIREBASE`) to using the whole URL (`REVIEWABLE_FIREBASE_URL`) to support regions outside the US.  Configurations with `REVIEWABLE_FIREBASE` will also continue to work indefinitely, however.
- Fix: improve how the default pending reviewers logic deals with author-initiated discussion with no other participants, and with completely unreviewed files.  Also ensure that all of GitHub's requested reviewers will be added to the pending reviewers list.  If you have completion conditions that customize `pendingReviewers` you might want to look at the updated example and consider backporting the changes.
- Fix: close unterminated code blocks and render LGTM emojis when quoting parent comment for the batched post to GitHub.
- Fix: avoid spiking Firebase load (sometimes to the point of a DoS) when unarchiving a review.  This may only be an issue once you hit a very large number of archived reviews like on reviewable.io but it tends to creep up on you.
- Fix: include all discussions / drafts when navigating through them while in single file mode.  Before this fix, it was possible for the navigation cycle to skip some items depending on the order they were created in and their locations.
- Fix: don't crash if we failed to load the emoji table from GitHub.

#### Release 3200.4991 (min 3107.4890 GHE 2.17+ or 3.0+) 2021-09-16
- Upd: show revision timestamps in the local timezone in the Commits virtual file.
- Fix: glom merge commit onto the last revision if it doesn't affect any files in the PR, even if the last revision has been snapshotted.  Note that this means a reviewed revision of the virtual commit messages file might get a new merge commit appended without further review.  This seems like a reasonable trade-off to forcing another round of review just for merging from the target branch prior to merging to it.  (This likely got broken back in v2997.4729.)
- Fix: improve rebased revision matching heuristics to de-prioritize distance in favor of other signals.
- Fix: don't mistake reviewed files for unreviewed in situations where the current user isn't meant to review them.
- Fix: don't show the Done button if the pull request author is caught up on a discussion.  This was unnecessary and could lead to mistaken repeated "Done" replies.
- Fix: don't send out "short of GitHub quota" notifications too often.  This only applies if you have API quotas turned on in GHE.
- Fix: correctly restore commit lists when unarchiving a review.  This bug could cause some reviews to fail to unarchive due to follow-on errors.
- Fix: avoid crashing when review creation failed in some cases.

#### Release 3179.4975 (min 3107.4890 GHE 2.17+ or 3.0+) 2021-08-29
- New: support commit message reviewing (only in reviews created after this release is installed).  See [changelog](https://headwayapp.co/reviewable-changes) for more details.
- Upd: render code blocks in PR titles.
- Upd: support `mp4` and `mov` video uploads in comments, with a maximum upload size of 100MB.
- Upd: expose `participant.read` flag for discussions in custom review completion condition input.
- Upd: remove `sandcastle` executor.  If you still have it set, we'll default to the (pretty much equivalent) `vm2` executor instead.
- Fix: report internal errors to client when ad-hoc review creation fails.
- Fix: let users with only read permissions sync a review to a PR by caching other users' permissions for a repo to correctly filter GitHub review approvals.  This was accidentally removed in v3063.4836 but likely isn't relevant to most Enterprise deployments as only people with write permissions will be using a repo.
- Fix: don't consider commits as equivalent if their messages differ, unless the more recent is a merge commit (in which case ignore its message).  We normally glom new commits that don't change any of the files in the review onto the last revision, even if it's already been snapshotted, to avoid creating (pointless) empty revisions.  However, this logic was too aggressive and would also skip over commits where only the message was changed.
- Fix: fix layout of dismissal confirmation message.
- Fix: make navigation between files work properly while a review is deferred.
- Fix: create a placeholder status check on the main branch when connecting a repo so that Reviewable can be selected in branch protection settings.  This was a regression from pretty long ago!
- Fix: make sure we don't cut off comments when posting to a discussion with media in it.  This was a regression introduced in v3049.4825.
- Fix: ensure that errors during on-demand review creation are reported back to the browser.

#### Release 3107.4890 (min 1992.2986 GHE 2.17+ or 3.0+) 2021-07-15
- New: automatically defer further action on a review when publishing with red counters remaining.  See the [announcement post](https://headwayapp.co/reviewable-changes/deferred-reviews-199866) for details.
- Upd: improve heuristics for matching rebased revisions to their priors and make use of Gerrit-style `Change-Id` headers in commit messages.
- Upd: tweak user activity status icons in the main review discussion footer.
- Upd: track an `Updated Review Status` event in analytics.  You can use this to analyze a review's state changes over time, including who was being waited on.
- Fix: don't copy UI text when the user's selection extends past the end of a discussion.
- Fix: consistently insert selected text when replying to a discussion.
- Fix: when changing license manager ID, don't break the Repositories page for the previous manager.

#### Release 3086.4857 (min 1992.2986 GHE 2.17+ or 3.0+) 2021-06-29
- Upd: show diff selection extension as soon as mouse button is released, rather than only doing so when copying to the clipboard.
- Fix: don't retry webhook requests that fail with a 4xx status, so the error will be reported immediately to admins for faster debugging.
- Fix: in a two column diff, fairly consider both sides when deciding on which side to place a discussion.  This should improve placement accuracy in some situations.
- Fix: correct layout of discussion corner labels for double-digit revision numbers with a base suffix.
- Fix: don't hold back on a diff (e.g., because it's really big) when a user explicitly requested it, by dragging out the bounds for a specific file or by clicking on a discussion's corner label.
- Fix: don't turn (soft) snapshotted revisions back into provisional ones in connected repos.  This bug didn't appear to actually affect how the revisions were formed, just reset their state back to provisional.
- Fix: allow files that were renamed then reintroduced at least two revisions later to be diffed and marked as reviewed.
- Fix: mitigate occasional notification webhooks that report the pull request as merged but still show people being waited on.
- Fix: update line links immediately when the line link template is changed in the profile dropdown.
- Fix: respect last selected organization when clicking on Reviews in the page header.

#### Release 3063.4836 (min 1992.2986 GHE 2.17+ or 3.0+) 2021-06-20
- Upd: log more information about GraphQL requests when using `REVIEWABLE_LOG_GITHUB_API_LATENCY`, since `POST /graphql` really doesn't tell you much about what it was actually doing.
- Upd: if rate limiting is turned off in GHE stop checking for it after the first probe request.  This means that if you decide to turn rate limiting on you'll have to restart Reviewable's servers to make them rate limiting aware again.
- Fix: stop fetching collaborators where possible, or move the fetch into the background.  When there are a lot of collaborators in a repo fetching them can be quite slow.  We'll now use more targeted fetches where possible (more individual requests, but each is much cheaper), or when not possible take collaborator fetches out of the critical path.
- Fix: when selecting alternative admins due to rate limiting poll them one-by-one (in random order) to reduce the likely load of the operation.  Until now, if the user who connected the repository didn't have enough API quota remaining we would always check every other repository admin to find the "best" one to substitute, which resulted in an optimal pick being made but was slow and expensive.
- Fix: correctly identify the language of files without a file extension that are nested in a directory (e.g., `foo/Makefile` or `bar/BUILD`).
- Fix: prevent "unable to capture head commit" errors that could temporarily leave a review in a broken state.

#### Release 3049.4825 (min 1992.2986 GHE 2.17+ or 3.0+) 2021-06-14
- New: automatically group in the file matrix files that are reverted (at the latest revision) and files that are just renamed.  This takes priority over custom grouping imposed by the completion condition but the user can dissolve the automatic groups from the file matrix, putting files back in their original spots.
- Upd: update Lambda completion condition executor to use NodeJS 14.
- Upd: show more clearly if a user approved or requested changes in the participants area below the main discussion, and add icons to make information readable at a glance.
- Upd: log latency metrics for PR sync tasks, to help isolate performance issues.
- Fix: invoke webhook in a timely fashion for merged PRs, and ensure it doesn't have bogus "waiting on" information.  This should also fix some cases where the review completion function is called either too often or too early.
- Fix: ensure review overlap strategy dropdown shows up even if there's currently only one reviewer, but the current user could potentially be a reviewer too.
- Fix: attach new discussion to the correct revision when changing diff bounds between revisions that have the same file SHAs.  The bug was usually innocuous but strange things started to happen if revisions with the same file SHAs were separated by a revision with different ones!
- Fix: detect badges that point to the wrong review (e.g., because of copy/paste between PR descriptions) and correct them when syncing a review.
- Fix: mark file revisions as reverted even if the reversion is caused solely by a change to base (e.g., retargeting the PR).
- Fix: trim issue autocompletion caches to the most recent 30k entries during monthly database sweeps.  This will prevent clients from trying to load too many entries in very large repos.
- Fix: don't show disposition prefix shortcut instructions in main discussion drafts, since you can't set a disposition in the main discussion.
- Fix: emit correct values in the dashboard PR list analytics event.
- Fix: tweak the max width of top level comments to better match GitHub's, so fancy Markdown layouts (with images) are more likely to display as intended.

#### Release 3024.4796 (min 1992.2986 GHE 2.17+ or 3.0+) 2021-05-19
- New: add an option to review every file personally, replacing the previous "Include changes in files previously reviewed only by others" checkbox with a three-option dropdown.  See [this post](https://headwayapp.co/reviewable-changes/new-overlap-strategy-193348) for details.
- Upd: add a placeholder graphic to make it clear when the "Awaiting my action" section of the dashboard is empty, rather than leaving out the section altogether.
- Upd: open up the option of limiting review creation to PRs with authors on a given team to Enterprise installations.  This gets encoded into the license key (for legacy reasons) so please get in touch with us if you'd like to take advantage of this feature.  It's probably only useful when doing a phased rollout of Reviewable, though.
- Fix: reinstate compatibility with GHE 2.x; this was accidentally broken in v2963.4700.
- Fix: try even harder to not randomly delete drafts.  There was an even more rare race condition that could cause this when navigating between files in single-file mode.
- Fix: work around a recently introduced GitHub bug that renders a clipboard button in comments next to code blocks.
- Fix: allow 0.1% of a file to be control characters before deeming it to be a binary file and turning off diffs.

#### Release 2997.4729 (min 1992.2986 GHE 3.0+) 2021-04-15
- New: add support for `vm2` sandboxed environment to safely run user code by setting `REVIEWABLE_CODE_EXECUTOR` environment variable to `vm2`.  **The `sandcastle` executor is DEPRECATED** and will be removed in a future release.
- Upd: don't hide reverted files in the client until they've been reviewed.  See [this post](https://headwayapp.co/reviewable-changes/improvements-to-reverted-and-rebased-files-191026) for some details.
- Upd: feed all files into the custom review completion condition, no longer leaving out reverted ones, to align with the new client logic above.  See [this post](https://headwayapp.co/reviewable-changes/reverted-files-in-custom-review-completion-conditions-191987), which also explains how to fix some broken code you might have inherited from old examples, and take advantage of more recent changes to completion condition semantics.
- Upd: display an inner status color in rebased revision cells relative to the matched revision, which might not be the preceding one. See the [docs](https://docs.reviewable.io/files.html#rebasing) for details.
- Upd: display a `reverted` icon in the cell when a file revision has been altered back to base.
- Upd: autocomplete markdown list syntax for bullets, numbers, and tasks when hitting `enter`.
- Upd: include new comments notifications in webhook contents even if the review isn't waiting on the users who have new comments to read.
- Fix: adapt to new GitHub OAuth token format.  This fixes "Unable to decrypt token with any key" errors.  You do _not_ need to change or fix your token encryption private key.
- Fix: reduce Docker image size back to <90MB.  The previous release accidentally bloated it a bit.
- Fix: ensure that critical file diffs are proposed, even if the prior revision was modified to base. [See issue #342](https://github.com/Reviewable/Reviewable/issues/342).
- Fix: removed a bad race condition when updating the review status that could cause updates to be skipped, or errors to be ignored.
- Fix: correctly list PRs when the dashboard is constrained to an organization with spaces in its name.  (Apparently, GHE will sometimes insert spaces into an organization slug to make it look nicer!)

#### Release 2963.4700 (min 1992.2986 GHE 3.0+) 2021-04-01
- New: add a `webhook` output property for custom review completion conditions, where Reviewable will send notifications of the review status changing (e.g., to Slack).  See the [public post](https://headwayapp.co/reviewable-changes/review-status-notifications-webhook-188778) for details.
- Upd: show all active reviewers in avatar lists on dashboard, but visually separate blocking from non-blocking ones.  Also improve avatar elision logic when running in a small window.
- Upd: group concluded PRs in their own section near the bottom of the list on the dashboard.
- Upd: optimize GitHub API requests issued when processing a status or check update.
- Upd: add support for Google Cloud Storage uniform permissions and private ACLs.
- Upd: show the Done button for the PR author as long as the discussion is unresolved (and as long as nobody else is Working), even if they're already satisfied.  This should help smooth out the workflow where the author responds Done but the reviewer isn't satisfied and requests further changes.
- Fix: prompt the user to grant organizations read scope from the dashboard organization dropdown if missing.  Also stop trying to fetch the user's organizations if we know the request is bound to fail.
- Fix: collapse sequences of matching revisions when the files in the PR haven't been changed.  This is specially useful when rebasing in commit-by-commit review mode as it will avoid forcing a review of lots of empty diffs.
- Fix: in the crash dialog, correctly identify whether a fatal crash was caused by Reviewable or by a browser extension.
- Fix: when diffing two revisions with different bases, do a better job of deciding which lines are base changes and which are not.  Previously, if a line was added/removed in a revision, it could get marked as being a base change even though it clearly wasn't.
- Fix: drop bogus `check_run` and `check_suite` events from forked repos that GitHub insists on sending to us.  This should further decrease the load both on Reviewable and GHE if you use forked repos in your organization.  Note that I don't know when the field we use to detect if the event is bogus was introduced; it definitely exists in GHE 2.21 and I suspect it goes back much earlier, but GitHub's docs don't say.
- Fix: don't randomly delete drafts when changing revision bounds or navigating between files in single-file view!  This was an extremely rare race condition but it seemed to affect some users much more than others, likely due to their machine's performance characteristics.  It would result in irrecoverable loss of draft comment text.

#### Release 2899.4660 (min 1992.2986 GHE 2.12+ or 3.0+) 2021-03-10
- **HOTFIX** for GHE 3.0: sync large PRs. GHE 3.0 made a subtle change to one of their APIs that made Reviewable fail when syncing PRs with very large diffs.
- New: allow the user to constrain the dashboard review list to a specific organization, and persist the setting across page reloads.  This is more efficient than a filter query.
- Upd: optimize dashboard PR query structure to reduce first page load latency a bit.
- Upd: track more granular statsd counters of requests made to GHE, segmenting by queue and GitHub event type.
- Fix: prevent client and server from sending out harmless but annoying requests to `localhost` when the Sentry connection is not configured.
- Fix: diff SVG files again, instead of treating them as images.
- Fix: avoid error with image files when user is not signed in.
- Fix: correctly load the repo's labels dictionary even if user is not signed in.
- Fix: prevent a rare crash with an error related to `borrowElement`.
- Fix: trigger re-evaluation of the review completion condition when a PR's statuses are updated, in case the condition relies on this data (introduced in v2183.3776).
- Fix: render tables in comments posted from GitHub.
- Fix: fix a regression that prevented the crash dialog from showing in some cases, making it look like the page just froze.

#### Release 2765.4558 (min 1992.2986 GHE 2.12+ or 3.0+) 2021-02-13
- **HOTFIX**: make custom review completion conditions work again.  These got broken back in 2630.4363 for both the Sandcastle executor (completely) and for AWS Lambda (for new repos only).
- Upd: migrate from raven-js SDK to Sentry SDK v6.0.3.  This should have no impact on you unless you connected your instance to Sentry.
- Fix: prevent severe performance degradation on page load when a review used a _lot_ of emojis.
- Fix: make sure "Show on GiHub" button in a diff always links to a valid page that will show the file in question, even if the diff bounds don't cover any changes.
- Fix: avoid fetching image files in a PR on the client (based on filename), since we can't diff them anyway.
- Fix: successfully continue updating Reviewable's issue autocompletion cache even when it grows very large (avoiding a `write_too_big` error).
- Fix: don't erroneously treat files as deleted if the entire PR is reverted back to base (but remains open).
- Fix: support `done` and `fail` callbacks for asynchronous custom review completion conditions in the Sandcastle executor.

#### Release 2696.4504 (min 1992.2986 GHE 2.12+) 2021-02-01
**WARNING**: custom review completion condition may not execute correctly in this release.
- New: offer a quick link to dismiss dissenting participants from a discussion.  This shows up next to the dispositions rollout if the user is satisfied, has a draft open, other participants are blocking or working, and the user is able to dismiss them.
- Upd: open external support links with non-reviewable domain in a new tab.
- Upd: ignore autogenerated gRPC stubs.
- Upd: support validation of `x-hub-signature-256` in webhook requests.
- Upd: respect `prefers-reduced-motion` setting unless overridden in account settings.
- Upd: add absolute timestamps in tooltips most everywhere we show relative "N days ago"-style descriptions.
- Fix: make next/prev (personally) unreviewed file navigation shortcuts work correctly in various edge cases again.
- Fix: avoid requesting review from yourself when using the default completion condition, syncing requested reviewers on publish, and having one or more files that were never marked reviewed in the PR.
- Fix: fix 'View on github' button to scroll to correct file on recent versions of GHE by switching the hash from MD5 to SHA-256.
- Fix: open comment images in a new tab to match GitHub's behavior.
- Fix: don't fail with a "GitHub disregarded the 'raw' media type" error when dealing with binary files on new versions of GHE.
- Fix: raise or remove some queue concurrency constraints.  This should let Reviewable take better advantage of CPU resources instead of artificially throttling itself.  It probably won't affect your deployment but if you have CPU-based scaling triggers set up you may want to capture a new baseline and adjust them.

#### Release 2630.4363 (min 1992.2986 GHE 2.12+) 2020-12-09
**WARNING**: custom review completion condition may not execute correctly in this release.
- Upd: upgrade to Node 14.
- Upd: if a PR is ready to merge and there are no other blocking users, designate the PR's autor as the blocking user so the PR will show up in the "Awaiting my action" section on their dashboard.
- Upd: add `review.pullRequest.target.branchProtected` flag to custom review completion condition input data.
- Upd: render emojis in PR labels.
- Fix: don't show disabled merge button on dashboard if PR is ready to merge but the user doesn't have permission to do so.
- Fix: avoid expensive load of file information for a review on the dashboard if it looks like it has a lot of files.  This could result in locking up the dashboard for everyone that had the PR in their list until it was closed and fell outside the query's closed PR horizon.  PRs impacted by this fix will show `???` as their files counter.
- Fix: match unreview file navigation logic to counter logic, otherwise it was possible to get into a situation where navigating would select files that did not, in fact, need to be reviewed.
- Fix: display correct unreviewed file counts on dashboard when negative file reviewed overrides are generated by the review completion condition.
- Fix: work around a GitHub API bug that would sometimes make polls for new PRs fail for users who toggled on "My PRs in any public/private repo" on the Repositories page.
- Fix: tolerate empty check run names.
- Fix: update base of subsequent PRs to the head's base before merging and deleting the head branch.
- Fix: prevent crashes in some very rare race condition edge cases.

#### Release 2580.4292 (min 1992.2986 GHE 2.12+) 2020-11-04
- Fix: **HOTFIX** fix handling of paginated GraphQL requests when running against GHE.  This has been broken since day one but mostly worked fine since requests that require paginating are pretty rare.  However, as we've been replacing REST with GraphQL over time the issue has grown worse and we finally tracked it down.  You're most likely to notice it with assignee and reviewer autocomplete not listing the right users, or with mysterious failures when publishing a review with assignee or reviewer changes.
- Fix: tolerate empty check run names.

#### Release 2569.4290 (min 1992.2986 GHE 2.12+) 2020-11-01
- Upd: fully support syntax highlighting of `.tsx` files.
- Fix: show all PRs where the user was mentioned on the reviews dashboard, working around a GitHub bug where @-mentions in reviews aren't indexed properly.  See [issue comment](https://github.com/Reviewable/Reviewable/issues/636#issuecomment-696902568) for limitations.
- Fix: work around stricter browser security restrictions in the sign-in popup window that would sometimes cause Reviewable to fall back to a less convenient redirect auth flow.  (For anybody stuck in the redirect flow, clearing cookies from Reviewable's site will reset the flag.)
- Fix: fix a regression in automatically setting requested reviewers on a review.
- Fix: truncate participant avatars field in review list so it doesn't overflow and leave no space for the PR title.
- Fix: avoid some very rare crashes when clicking outside the review area.
- Fix: fix a regression in extending code selection in diff to beginning/end of line.

#### Release 2487.4249 (min 1992.2986 GHE 2.12+) 2020-09-22
- Upd: point GitHub link button directly to the file of interest rather than just the diff of interest.
- Upd: add a small icon to the Publish button indicating approval / blocking when the text gets removed due to small page width.
- Upd: add `action` property to file revisions in the custom review completion condition input structure, to indicate whether the file was added, removed, modified, or renamed at that revision.
- Upd: add a `nextDiscussionWithDisposition(disposition)` keyboard binding, along with previous/first/last variants.
- Fix: correct a race condition that resulted in comments with images sometimes being cut off on the page.
- Fix: resize resolved discussion "bar" placeholders when changing the diff's column width.
- Fix: correctly render PR labels that get cut off on the reviews dashboard.
- Fix: avoid sometimes locking up on load in reviews with a renamed-then-reintroduced file when using review-each-commit style.
- Fix: stop syntax highlighter from getting confused in some cases (e.g., when using `Foo.class` in a Java file).

#### Release 2411.4117 (min 1992.2986 GHE 2.12+) 2020-08-12
- Upd: make [Reviewable docs](https://docs.reviewable.io) indexable by Google Search.
- Upd: add `review.pullRequest.target.headCommitSha` to the review completion condition input data.
- Fix: make sure publishing comments works even if an organization has many thousands of potential assignees.
- Fix: correct a bunch of issues related to autocomplete in drafts, including inconsistent search logic, behavior in the face of user logout, etc.
- Fix: don't try to automatically connect archived repositories.
- Fix: ignore self-mentions in comments instead of showing an error or freezing.
- Fix: prevent accidental DoS-by-regex on certain malformed PR descriptions and other user-controlled input.

#### Release 2351.4070 (min 1992.2986 GHE 2.12+) 2020-06-22
- Upd: check more often that the user who originally connected a repository still has admin access to it, and send a warning email if not.
- Fix: autosize draft text area consistently when the comment contains `>` quotes.
- Fix: resize comment area correctly when expanding/collapsing quoted blocks.
- Fix: in two column diffs, prevent blank (filler) lines from appearing in text copied from the diff.
- Fix: don't allow a race condition to cause some values like mergeStyle to flutter when review completion code is run.
- Fix: ignore another category of bogus errors that appear to mostly come from the Grammarly extension.  Sigh.
- Fix: avoid locking up when trying to preview or send a comment in some forked repos.

#### Release 2330.4029 (min 1992.2986 GHE 2.12+) 2020-04-29
- Fix: don't crash when a bogus "ResizeObserver loop limit exceeded" error occurs.  This appears to be caused by the somewhat popular Grammarly extension.
- Fix: remove one more spot that used deprecated GitHub API authentication methods, though it likely wasn't affecting most installations.
- Fix: make "unresolved discussion" navigation keyboard shortcuts work again.

#### Release 2326.4025 (min 1992.2986 GHE 2.12+) 2020-04-16
- New: treat file mode changes as modifications and display them when diffing.  Also notify when a file is a symbolic link.
- Upd: offer option to insert Reviewable badge at the top of the pull request description, rather than at the bottom.
- Fix: correctly handle commit authors and committers that are not GHE users.
- Fix: show correct defaults when opening repository settings a second time after applying some edits.
- Fix: don't trigger a run of the custom review completion condition when background-syncing a closed pull request.  Doing so could result in spammy notifications when performing batch admin actions on a repo in GHE, and Reviewable generally doesn't automatically update the completion condition for closed PRs anyway.
- Fix: avoid breaking review list page when user is a member of more than 100 teams.
- Fix: respect the "automatically delete head branches" setting in GitHub, by forcing the "delete" option in the merge options dropdown and letting GitHub do the deed rather than racing with it.

#### Release 2269.3960 (min 1992.2986 GHE 2.12+) 2020-03-08
- Upd: add warning about draft PR to mergeability status check that shows up in Reviewable.  Before then, if a PR was in draft but otherwise ready to merge, you'd end up in a state where everything looked OK but the merge button just wouldn't show up.
- Upd: reduce false positive reports of "Repeatedly failed to process event".
- Upd: stop using the recently deprecated `access_token` authentication method for GitHub requests on the client.  This will impact performance since it requires an additional "preflight" request for most every `GET` sent to GitHub, but GitHub's engineers weren't receptive to this argument against the change.  Oh well.
- Upd: switch to a new HTTP request library on both client and server.  Should have no noticeable effects, but last time we tried upgrading the old library it broke servers seemingly at random, so noting the change here just in case.
- Upd: add `author` and `committer` to revision commits info in review completion condition input structure.
- Upd: add `markFileReviewedAndRetreat` bindable command, for those who like to start their reviews at the bottom.
- Fix: note when a review is broken on the Reviews dashboard and skip loading detailed data for it.  This doesn't happen often (e.g., if the number of files in a review exceeds 8000) but when it does the data might be in an invalid state and cause the page to get stuck.
- Fix: when `syncRequestedReviewers` is set in a custom completion condition, update requested reviewers whenever `pendingReviewers` changes, even if no review was published (e.g., when a PR is first created).
- Fix: ensure the Reviews button in the header always goes back to the most recently viewed list of reviews.  It would sometimes go back to a specific review instead!
- Fix: work around a recent change in GitHub's API that would cause reviews with a commit that touched >100 files to be unable to sync.
- Fix: prevent some UI issues that would occur when only part of a file's revision history belonged to a renamed file, but the rest remained as part of the original.  This could result in weird diff bounds being picked or marking revisions as reviewed affecting multiple files in the matrix.
- Fix: allow for ambiguous path to file mappings when interpreting review completion condition results.  This happens in the same "partially assimilated file" conditions as for the fix above.
- Fix: when creating a discussion, ensure that the other revision in the diff gets snapshotted too so we can reproduce the exact diff later.  Otherwise, we can end up with a discussion with a dangling revision reference, which can cause crashes in rare circumstances.
- Fix: guard against a bug in GHE that occasionally returns a supposedly internal completion state for a check run.

#### Release 2227.3878 (min 1992.2986 GHE 2.12 - 2.20) 2020-01-06
- Upd: improve diff algorithm to keep indentation-only deltas clean rather than sometimes pulling in short unchanged line contents.
- Upd: allow horizontal scrolling of file header when revision cells overflow available space.  Scroll bar is not visible but you can use the middle mouse button (or whatever your OS of choice allows) instead.
- Upd: embed RSA key rolling feature in the monthly user table cron job if multiple keys are passed to the server.  This is more reliable than using a separate script.
- Upd: switch RSA encryption to native Node libraries for a boost in performance.
- Fix: prevent very rare "undefined function closest" crash.
- Fix: prevent permission denied errors when stale user records are automatically cleaned up when they're still referenced by other data.
- Fix: prevent servers getting stuck processing a GitHub event forever if the repo it refers to was deleted at just the wrong moment.
- Fix: deal gracefully with pull requests that have no commits.  This can happen if the source repo gets deleted.
- Fix: support thousands of repos on the Repositories page.

#### Release 2210.3860 (min 1992.2986 GHE 2.12 - 2.20) 2019-11-21
- New: support monitoring via `statsd`.  This is just a first cut at the feature and subject to change as I gather feedback from people who actually use `statsd` and such.  Please see the [config doc](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#monitoring) for details on the extra environment variables you need to set this up, but if you already happen to have `DD_AGENT_HOST` defined in your environment then the feature will turn on automatically.
- Upd: add `draft` state to pull request list query language.
- Upd: upgrade Lambda executor to use NodeJS 10 environment for user scripts.
- Fix: work around a regression in the Lambda API that caused all errors to show up as "internal executor error".
- Fix: use custom monospace font for code embedded inside comments too.
- Fix: avoid getting into a situation where (among other things) newly opened reply drafts would fail to get focus and get deleted on the next click.  This seems to have only affected some Firefox installations but could've had much more widespread effects.  All browsers should benefit from a performance boost for reviews with many discussions where users often change diff bounds.
- Fix: when setting automatic diff bounds in commit-by-commit review style, don't set bounds for files if it would result in diffing a revision against itself (resulting in a nil diff).  This was a no-op, but messed up the algorithm that estimated whether the bounds of all the file diffs were consistent.
- Fix: don't over-extend diff selection on copy if the selection ends exactly at the beginning of a new line.
- Fix: prevent occasional crash with "unmatched diff release" error message.

#### Release 2200.3821 (min 1992.2986 GHE 2.12 - 2.20) 2019-10-10
- New: add a repository setting to constrain who can dismiss participants from a discussion, either anyone with write access (the default) or only repository admins.
- Upd: update to Node 12.
- Upd: no longer run server as `root` to improve security.
- Upd: slim down Docker image from 392MB to 105MB.
- Upd: only offer "Resolve" as the primary action on a discussion if the user is already an active participant or the PR author.  Always offering it made it too easy to accidentally resolve a discussions when there are multiple reviewers.
- Upd: capture both diff bounds when a new discussion is created in a file, and restore both if the user clicks on a discussion's revision corner label.  Previously, only the discussion's target revision was stored, not the other side of the diff.
- Upd: include commit titles (first line of commit message) in custom review completion condition input data.
- Upd: treat files under `__generated__` directory as generated.
- Fix: treat Unicode line and page separators as newlines when parsing text.
- Fix: avoid minor layout issues on dashboard page and in review participants summary section.
- Fix: don't fetch branch list of current repo until user starts editing the target branch (which happens quite rarely).
- Fix: if a file was renamed, then reintroduced under its original name, ensure the reintroduced one is actually included in the review.
- Fix: don't show an error if trying to delete a branch that's already gone after a merge.

#### Release 2183.3776 (min 1992.2986 GHE 2.12 - 2.20) 2019-09-13
- New: display old/new file size (and delta) for binary files.
- Upd: split quoted replies to Reviewable's combined messages that originate from GitHub's web UI into separate threads, just like we always did for email.
- Upd: respect disposition-setting keywords (such as "FYI" or "OK") at the beginning of a draft even if they are preceded by quoted lines.
- Upd: add syntax highlighting for HCL/Terraform files.
- Upd: show the checks status icon on the dashboard if the summary state is `missing` or `pending` even if the review is in progress and other status counters are non-zero.  Previously, these states used to be elided from the dashboard to avoid information overload but they can be helpful in deciding which reviews to tackle and which to postpone.
- Upd: include `review.pullRequest.mergeability` and `review.pullRequest.checks` in custom review completion condition input data.
- Upd: don't mark synthesized revision comments (that list the commits that make up a revision) as unread if a commit was added to one that didn't affect any of the files in the PR, usually as the result of a merge or rebase operation.  (The commit is still processed and added to the revision as before, just without further notification as there's nothing new to review anyway.)
- Upd: when deciding whose action a discussion is awaiting, if the PR author's disposition is Blocking and all participants are up-to-date on all comments, then the PR author will be the only one selected as being waited on (even if others are Blocking too).  This is an unusual scenario since the PR author would typically be Working instead but this logic tweak should accord better with intuition.
- Fix: work around a bug in a dependency that manifests itself as GitHub API calls sometimes timing out with an "Aborted" message.  The error was probably introduced in 2140.3674, and was usually innocuous as the call would be automatically retried but in some situations (such as a highly loaded GHE instance) could become fatal and render Reviewable virtually unusable.
- Fix: prevent a user record with ID `undefined` from being written to the datastore.  This is likely the result of a buggy response from GitHub's API, but once in the datastore it breaks some downstream code.
- Fix: make a middle mouse click work properly most anywhere within a dashboard row, not just on the PR name.
- Fix: recognize click on mark reviewed button when text is selected on the page in Firefox.
- Fix: take GitHub review comments made on multiple comments into account correctly when deciding how to group commits into revisions.
- Fix: don't allow repo config button in checks dropdown panel to be clicked when disabled.
- Fix: correctly manage text selection within diffs in Firefox.  Previously, the last line of the selection was sometimes dropped.
- Fix: respect `reviewed: false` flag set by custom review completion condition on file revisions by counting files as unreviewed and styling mark buttons appropriately.  Previously, it was possible for the `false` value to be overridden by reviews of subsequent revisions.
- Fix: roll back HTTP request library to work around weird timeout problems on cached fetches.  The issue did not present consistently, appearing without provocation on a given instance and sometimes going away by itself as well.  If you're seeing a lot of "Aborted" or "Timeout" errors you'll want to update to this version.

#### Release 2148.3676 (min 1992.2986 GHE 2.12 - 2.20) 2019-07-24
- Fix: **HOTFIX** remain compatible with older GHE versions by removing reference to `Mannequin` from GraphQL queries.
- Fix: deal correctly with non-lowercase usernames when inferring list of users for "sync requested reviewers" publishing option.

#### Release 2140.3674 (min 1992.2986 GHE 2.12 - 2.20) 2019-07-11
This version is broken on some older GHE versions, it's strongly recommended that you skip to the next one (2148.3676).
- Upd: indicate draft PRs on dashboard and review pages (GHE 2.17+).  You need to use GitHub to declare a draft PR "ready for review" for the time being; that functionality will be added later, along with a UI redo.
- Upd: support `±label:<name>` query terms in review list.
- Upd: improve selection of base commit when diffing renamed files (favoring the original file when possible).
- Fix: don't ignore errors when merging, and don't delete the branch if the merge failed.
- Fix: make sure the merge completes before deleting the source branch.
- Fix: don't get stuck waiting for a merge commit message (with a spinner that never goes away) if user clears out the text box.
- Fix: avoid rare crash when deleting a comment with images in its preview mode.
- Fix: avoid another rare crash when using the review preview dropdown and editing a comment immediately thereafter.
- Fix: don't crash with a bogus permission denied error when trying to access an unarchived review with special characters in the owner or repository name.
- Fix: track focused draft correctly for targeting keyboard shortcuts.  A bug caused the first draft that had a keyboard shortcut applied to it to become the target of all future shortcuts.
- Fix: ignore review comments with no author (not sure how that can happen, but observed one such case!).
- Fix: clip long directory paths in file matrix.  This is a quick fix to prevent the layout from jumping around when moving the mouse around the matrix, but may result in some long paths getting clipped even when _not_ hovering over them if all filenames are short and there aren't many revisions.

#### Release 2113.3608 (min 1992.2986 GHE 2.12 - 2.20) 2019-05-16
- New: add a `mergeStyle` field to review completion condition output.  You can use this to dynamically force a specific merge style for a review even if the repo is configured to allow others.  Only enforced in Reviewable.
- Fix: make custom diff font settings actually work again.
- Fix: prevent an extremely rare series of occurrences from permanently corrupting a review such that it won't update with any further commits.  This bug has been present in the code since day one and I just observed it for the first time in 5 years...
- Fix: don't offer to "show more concluded reviews" when searching for a single PR by URL in review list.
- Fix: don't crash when some PR filepaths differ only in case in specific ways.
- Fix: don't use data derived from a stale review state when racing to create a new review (very rarely).  This could've resulted in aspects of a review being temporarily out of sync with the PR, but should've fixed itself automatically as the review was visited and resynced.
- Fix: deal gracefully with racing server shutdown requests, instead of crashing out early.

#### Release 2093.3571 (min 1992.2986 GHE 2.12 - 2.20) 2019-04-16
- New: add a `disableBranchUpdates` flag to review completion condition output.
- New: add `defaultMergeCommitMessage` and `defaultSquashCommitMessage` fields to review completion condition output.  Also include `title` and `body` in `review.pullRequest` input state.
- Upd: on initial load, keep the review list's spinner up until _all_ the data has been loaded, to prevent having rows move around as more information streams in.  Note that this is a live list so rows may still shift position later as reviews' states change, but this should improve the first-load experience at the expense of a bit more latency before the list shows up.
- Upd: upgrade AWS Lambda custom review completion condition runtime environment to Node 8 (applied lazily as conditions are modified).  This is technically a major version change but it's very unlikely to affect the kind of code written for completion conditions.
- Upd: raise GitHub API socket timeouts.  This should help reduce spurious errors when the GitHub server is under high load and slow to respond.
- Upd: tweak semantics for the red "unreplied" counter and "awaited" users so that if you are awaited for a review due to a discussion, it will always show up as unreplied for you.  Previously, there were edge cases in multi-party discussions where you were awaited but a passive follower got the red unreplied state instead.
- Upd: remove a workaround for GitHub's API bug that reset the "allow maintainers to modify PR" flag when applying an unrelated patch.  Unfortunately, they've tightened up the validation such that it's not possible to even specify the existing flag value unless you're the PR author, and Reviewable needs to call the API through various identities.  It's not clear when the API bug was fixed, so if you're still using an old version of GHE then Reviewable might start resetting the flag again.  If this happens, please either upgrade your GHE or downgrade Reviewable.
- Upd: show full name in review participants status area, instead of just the avatar.
- Fix: don't consider a branch fast-forwardable if GitHub says it can't be rebased, even if analysis of the commit chain indicates that it ought to be.
- Fix: line up diff stats correctly to the right of the file matrix when a file has no last reviewer.
- Fix: correctly generate publication preview when user switches approval flag while the preview is being generated.
- Fix: prevent some very rare crashes in data race condition edge cases.
- Fix: don't crash if a GitHub app files a status check with an empty-string context.
- Fix: don't crash with permission denied error when collapsing or expanding a file group whose name has periods in it.
- Fix: prevent crashes when trying to redirect in Edge.
- Fix: prevent clicks on a merge button in the pull requests list from navigating to the review.

#### Release 2071.3450 (min 1992.2986 GHE 2.12 - 2.20) 2019-03-11
- New: allow grouping (and reordering) files in a review via new `group` property for files in the custom review completion condition.  See the docs on [setting it up](https://docs.reviewable.io/repositories#files) and [how it looks in the UI](https://docs.reviewable.io/files#file-list) for details.
- Upd: lay out the file matrix as a directory tree rather than a flat list of files.
- Fix: **HOTFIX** avoid a crash when loading Reviewable on an encrypted instance in private mode while already signed in with the error "Encryption not set up".  This regression was introduced in the previous release, v2066.3418.
- Fix: correctly display and expand/contract long paths in file headers.
- Fix: don't show file matrix until file renames have been mapped.
- Fix: improve text copy from diff to correctly extend selection to full lines when dragging backwards with mouse, and to exclude blank line space placeholders from the copied text.
- Fix: stop requiring `REVIEWABLE_FIREBASE_AUTH`, which hasn't been needed since v1994.2998.
- Fix: prevent a crash on loading the reviews list if server hasn't properly set the GHE version in Firebase yet.  The root cause is likely a bad auth for the subscription admin user, which will log `Unable to initialize generic GitHub access` on the server at startup along with a specific error message.
- Fix: don't flash a prompt to sign in when loading a review page on an encrypted instance in private mode while already signed in.

#### Release 2066.3418 (min 1992.2986 GHE 2.12 - 2.20) 2019-02-28
**KNOWN ISSUE** This version breaks badly in environments running in `REVIEWABLE_PRIVATE_MODE`.  You almost certainly want to skip over it.
- New: let user temporarily see more concluded (closed or merged) pull requests on the dashboard by clicking a link.
- Upd: upgrade to NodeJS 10.
- Upd: upgrade email module.  **WARNING** It now uses `dns.resolve` instead of `dns.lookup` when resolving `REVIEWABLE_SMTP_URL`, which will query the DNS server directly and bypass some local configs such as `/etc/hosts` (i.e., it doesn't use `getaddrinfo(3)`).
- Upd: lower the diff threshold to consider a file pair as renamed by 5%.
- Fix: ignore @-mentions that can't be resolved.
- Fix: detect when the authorization has been upgraded in other tabs, and force user to re-authenticate to prevent an authorization skew between the permissions on the server and in the tab.  This was not a security issue but could result in unexpected errors.
- Fix: lock out sign-in/sign-out during authentication (to avoid racing multiple auth requests) and when publishing (to avoid accidentally signing out and breaking the process halfway).
- Fix: fix minor visual issues with set default query link on dashboard.
- Fix: prevent crash in rare cases when setting diff bounds in file matrix header.
- Fix: put pending reviewers list back in the checks dropdown.
- Fix: don't request reviewer twice if user is self-requesting.  This is a no-op but looks ugly in GitHub's timeline.

#### Release 2056.3350 (min 1992.2986 GHE 2.12 - 2.20) 2019-02-07
- New: give option to sync GitHub's requested reviewers from Reviewable's awaited reviewers when publishing.  See [docs](https://docs.reviewable.io/reviews#sync-requested-reviewers) for details.  Great for integration with [Pull Reminders](https://pullreminders.com) if you use Slack!
- Fix: allow users to login in multiple browsers / profiles without forcing all but the latest one offline.  This was a regression introduced in 2033.3283.  Note that this is a temporary fix that reintroduces the auth ugprade race condition handled by the broken fix.  A better permanent fix will come in the next release, after a whole lot of testing.
- Fix: remedy race condition when triggering completion condition evaluation in response to PR review events. Previously, it was possible for the condition to be triggered before the review state correctly reflected the new approvals.
- Fix: improve forced logout on page load when session is close to expiring to be less likely to cause spurious errors.
- Fix: if a review's style hasn't been set yet and the user lacks write permissions, temporarily initialize it locally to avoid potential breakage (and a blank dropdown in the UI).
- Fix: don't show merge button if user lacks push permissions, even if PR is mergeable.
- Fix: substitute ghost user for deleted users during completion condition evaluation if unable to find or fetch their old info.  Previously, the evaluation task would get into an infinite failure loop instead.
- Fix: correctly render dismissed user status icon on dashboard.
- Fix: truncate very long GitHub comments when syncing (current cutoff is 100KB) to avoid running afoul of Firebase's hard limits on data size.  (If the limits are exceeded it can get tricky to recover.)
- Fix: keep drafts locked while generating publish preview to avoid async edits that would not be reflected in the preview, or that can sometimes cause a crash.
- Fix: don't misinterpret a `±reviewer:@username` directive as an @-mention.
- Fix: don't offer PR author as a completion for `+reviewer:@` directive.
- Fix: include pending changes to approval, assignees, reviewers, labels, etc. when evaluating the completion condition before publishing.  This didn't affect status accuracy (since the condition gets evaluated again after everything is published and committed) but could result in a bogus completion description being shown in the preview and posted in the GitHub message.
- Fix: don't walk revision description message timestamps forward by 1ms every time the review gets synced in certain circumstances.  This could result in requiring users to regularly re-acknowledge those comments.
- Fix: restore hotkey indicators in certain contextual help blurbs.

#### Release 2033.3283 (min 1992.2986 GHE 2.12 - 2.20) 2019-01-21
- New: publish a complete user guide at https://docs.reviewable.io.  This has all the content from the online help system and more, organized to be both readable and searchable.  It will stay up to date with the current version of the app running on reviewable.io, so it may reference features that are not yet available to Enterprise, or that you have not yet deployed.  If this turns out to be a major issue we'll figure out a solution, but for now think of it as an additional incentive to update often!
- Upd: fill in missing properties in the result of a custom review completion condition with values from the output of the built-in default condition.  This will make it easier to tweak things without having to take on maintenance of the full condition.
- Upd: support `refreshTimestamp` in completion condition output structure, to determine when it should be re-evaluated.  Also add `lastActivityTimestamp` to discussion participants and `timestamp` to file reviewer marks (the latter will not be available for marks made in older versions).  See the new user guide for a full explanation of how this works!
- Upd: add `review.pullRequest.requestedTeams` to the review state data made available to custom review completion conditions.  Old reviews are _not_ eagerly backfilled, but will gain the property when synced for any reason (e.g., being visited in the browser).
- Upd: regularly delete stale cached info for users who never signed in to Reviewable.  This will help reduce the size of the users collection, which can eventually cause performance problems.
- Upd: add `showHideAllComments` bindable command.
- Fix: make the warning icon show up correctly in merge button.
- Fix: update Reviewable's cached mergeability state promptly if Reviewable status check is required in GitHub.  Prior to this fix, actions that changed the review's completion status would not be reflected in the mergeability state until the user reloaded the review page or something else triggered a sync.
- Fix: if review creation fails on visit, display error correctly in the browser.
- Fix: immediately recognize GitHub auth upgrades (new OAuth scopes) instead of getting stuck and requiring the user to reload the page.
- Fix: eliminate a race condition on GitHub auth upgrades where some GitHub API calls would be issued too early (with the old token) and fail.
- Fix: ensure that GitHub auth upgrades don't accidentally switch to a different account, if the user signed in with a different username to GitHub but not Reviewable.
- Fix: prevent spurious server shutdowns when fetching very long lists of items to process during background sweeps.
- Fix: resize comment area if details element expanded (e.g., "Quoted NN lines of code...").

#### 2017.3170 (min 1992.2986 GHE 2.12 - 2.20) 2018-12-14
- Upd: show a yellow warning sign on the merge button if non-required checks are failing, instead of a red one which is now reserved for admin overrides of required checks.
- Fix: correctly compute height of file matrix when concealing/revealing obsolete files.
- Fix: when navigating to a file in a review, avoid animating the header multiple times if the user has visited multiple review pages during the session.
- Fix: in the "mark reviewed and go to next file" button, hitting the small red button will now also advance to the next file instead of only marking it as reviewed.  This was a regression from a version or two ago.
- Fix: avoid a "not ready" crash when clicking on a disabled Merge button.
- Fix: compute mergeability status correctly and in a timely fashion in all (or at least more) situations when branch protection is turned on.
- Fix: don't lock up the dashboard when user types a partial URL into the search box.
- Fix: in markdown comments, don't treat multiple triple-backticks on the same line as an unclosed code block since GitHub renders them as if though they were inline single-backticks instead.
- Fix: respect no-animation setting in merge options dropdown.

#### 2003.3043 (min 1992.2986 GHE 2.12 - 2.20) 2018-11-28
- Upd: remove "butterfly" onboarding mechanism, to be replaced by a more conventional (and less hate-inspiring) checklist as part of the Vue rework at a later date.
- Upd: exclude merged and closed PRs from the "Awaiting my action" section on the dashboard.
- Upd: adjust sorting order of blocking avatars on the dashboard to be easier to parse visually.
- Fix: correctly send sample review data if no PR selected when editing custom review completion condition.
- Fix: reinstate rebased revision matching arcs above the file matrix and file header revision cells.  If the source branch was rebased, these show Reviewable's guess at how the revisions match up.  They disappared a while back and are now back as part of the Vue migration.
- Fix: make issue (`#`) autocomplete work in comments again.
- Fix: protect against breakage if a username, organization name, or repository name happens to match a built-in property name.
- Fix: render markdown checkboxes in comments as checked when they are.
- Fix: avoid extremely rare internal error crash when diffing.
- Fix: gracefully handle situation where a user who turned on an organization's "All current and future repos" toggle loses admin permissions but still has pull permissions.
- Fix: ensure that the second and later runs of background cron tasks start from the beginning, rather than mistakenly resuming at the last checkpoint of the previous run.

#### 1994.2998 (min 1992.2986 GHE 2.12 - 2.20) 2018-11-19
- Upd: prevent rollbacks to version that use the old Firebase SDK.  It's now safe to remove the `REVIEWABLE_FIREBASE_AUTH` environment variable from your configuration, and revoke the legacy secret(s) if you'd like to do so.
- Fix: avoid bogus "offline" state with pending writes that never goes away, caused by an unfixable false positive in the shared web worker client abandonment detection logic.  The trade-off is that now if a Reviewable tab exits abruptly, some resources in the shared worker won't be released.  However, closing _all_ Reviewable tabs will always dispose of the worker, so it's a good thing to try if you find your browser resource usage creeping up inexplicably.
- Fix: correctly treat the "Show pull requests not yet connected to Reviewable" toggle on the dashboard as being on by default, if the user never toggled it manually.  Previous versions used to show it as on, but behaved as if it was off.
- Fix: on the dashboard, don't show a status spinner forever for unconnected pull requests.  (Esthetic fix only, as there's nothing more to show anyway.)
- Fix: prevent the merge button from getting stuck disabled when in Squash merge mode.  This change moves fetching the commits from GitHub to generate the automatic merge commit message to later in the process, so if this phase fails the merge will fail with an appropriate error message rather than the button getting stuck.
- Fix: avoid a crash when the user signs out halfway through merging a PR.
- Fix: work around corrupted Firebase state when the client crashes with an `InvalidStateError`.  Prior to this fix, all subsequent page loads would fail with permission denied errors until the user closed _all_ Reviewable tabs to flush the shared worker.
- Fix: raise timeout when checking permissions on all of the user's repos, to allow for a large number of repos even if the GitHub API server is pretty busy.
- Fix: don't crash if a bulk permission check fails and a targeted permission check returns false.

#### 1992.2986 (min 1975.2968 GHE 2.12 - 2.20) 2018-10-31
- New: automatically archive old reviews to save space and prevent the monthly sweep from overloading Firebase.  Closed reviews are archived aggressively, while seemingly abandoned open reviews will be archived after a longer period of time, based on the time of last access (or review creation if never accessed).  Archived reviews remain in Firebase (and encrypted) and will be transparently restored as needed.  The only place where this feature is visible to users is on the reviews dashboard, where archived reviews will show their status as "Archived" until restored via a visit and may not show up in the correct category.
- Upd: switch the client to the current version of the Firebase SDK.  All users will be signed out the first time they load a Reviewable page with this version, but currently open pages from the previous version will not be interrupted.  No config updates required beyond those from the previous version.
- Fix: don't list reviews where the user is just a mentionee under the "being reviewed by you" category on the dashboard, but do list self-reviews there.  A side-effect of this fix is that some older reviews will show up in a category lower than "being reviewed by you" until somebody visits them again, but this shouldn't be a problem going forward.
- Fix: include files that were renamed with no other changes in the review state passed to the custom review completion condition.
- Fix: supply correct sentiment timstamp to custom review completion condition when executing in a preview or pre-publication context.  Since version 1831.2835, conditions were sometimes given a bogus object instead of a timestamp that could cause the condition to fail or produce inaccurate results.
- Fix: prevent batch cron jobs from disturbing the Firebase cache.
- Fix: address even more edge cases when rebasing to avoid ending up with a broken review.
- Fix: avoid some crashes when user signs in and out very quickly.  This should pretty much never occur in real usage, but did during github.com's recent breakdown where they were handing out auth tokens but immediately refusing to accept them!
- Fix: defuse race condition that could let users try to create a line comment after signing in but before their review state was loaded, resulting in a crash.

#### 1975.2968 (min 1866.2875 GHE 2.12 - 2.20) 2018-10-19
- Upd: **CONFIG UPDATE REQUIRED** switch the server to the current version of the Firebase SDK.  The new SDK addresses some long-standing Firebase bugs, in particular greatly ameliorating (but not quite fixing) the stuck transactions that cause Reviewable servers to restart themselves frequently under load.  However, this new SDK **requires different credentials** to initialize the connection to Firebase.  Please check the updated [config docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md) for instructions on where to find the bits you'll need in the Firebase console and how to set the `REVIEWABLE_FIREBASE_WEB_API_KEY` and `REVIEWABLE_FIREBASE_CREDENTIALS_FILE` environment variables.  Please keep the old `REVIEWABLE_FIREBASE_AUTH` around for now, until the client gets updated to the new SDK as well in an upcoming release.  In case you have a very twitchy firewall, note that the new Firebase SDK will send requests to various subdomains of `googleapis.com` as part of the updated auth token management mechanism.
- Upd: update syntax highlighting library, review and update file extension mappings, and subset library to a more commonly used set of languages based on analytics data from reviewable.io.  If you find that some file types are no longer highlighted correctly, please let me know and I'll add them to the next release.
- Fix: handle even rarer edge cases when rebasing to avoid ending up with a broken review.
- Fix: prevent rare permission denied crash when loading an uninitialized review.
- Fix: prevent permission denied crash when creating a comment on a base revision of a renamed file where the original source file had been recreated in the PR at or before that revision.  The comment will now be created on the nearest possible equivalent base revision instead.
- Fix: avoid rare crash due to race condition in the contextual help subsystem.
- Fix: invite user to sign in when landing on a review page they have permission for but where the review hasn't been created yet, instead of getting stuck.
- Fix: avoid false negative liveness checks that cause an automated server restart when a cron job is starting up.

#### 1911.2952 (min 1866.2875 GHE 2.12 - 2.20) 2018-10-08
- New: allow user to set a default query for the Reviews page
- Upd: add `±starred` and `±watched` filters for use in Reviews queries.
- Upd: migrate Merge button from AngularJS to VueJS.  There should be no user-visible differences, except perhaps an incidental fix to a rare bug in applying default settings.  This is just the first step in the migration of the entire UI to VueJS; unlike the big-bang model migration and attendant beta, this one will proceed piecemeal over the coming months.  I won't call out further VueJS updates in the changelog unless they have user-visible effects.
- Fix: accept `check_run` and `check_suite` events.
- Fix: correctly abandon processing of comment events if the comment still can't be found after an hour.
- Fix: correctly handle some edge cases when a branch gets rebased onto one of its own commits, where previously this could result in a permanently broken review.
- Fix: don't crash when the user requests a preview of the outgoing drafts, then edits one of them at just the wrong moment.
- Fix: don't get stuck waiting forever for data when the user switches pages at just the right time to trigger a race condition.  This could happen most easily when switching from Reviews to Repositories and back before the Repositories page fully loaded, but could have affected other transitions as well.
- Fix: remove top discussion draft area from bunny dropdown when user not signed in (otherwise typing in it would cause a crash).

#### 1883.2928 (min 1866.2875 GHE 2.12 - 2.20) 2018-09-09
- Upd: don't count participation in resolved dicussions towards inclusion in the "being reviewed by me" section on the dashboard.
- Upd: restart the server if Firebase liveness check fails for more than about a minute.  This can help reset zombie connections to Firebase in some edge cases.
- Upd: removed many rarely used languages from the syntax highlighting library pack to significantly reduce code size.
- Fix: set correct font for draft text boxes.  This regression caused draft boxes to be sized incorrectly, which made long messages harder to input.
- Fix: back off the retry interval when dealing with GitHub's 422 bug for setting refs, and log retries to make this situation easier to debug.
- Fix: ensure that the "unresolved N" toolbar button (for N > 0) will always navigate to a discussion.  Previously, it was bound to navigate to the next discussion _without a draft_.  Also introduce new commands for the two navigation variants and rebind default keys to the new semantics.
- Fix: bring back the "draft saved" indicator when editing draft comments.
- Fix: eliminate rare crash when publishing comments due to rendered value being `undefined`.
- Fix: prevent some rare crashes when quickly navigating to a review from the dashboard and back.
- Fix: prevent some permission denied errors that could occur if the connection dropped while publishing.

#### 1872.2918 (min 1866.2875 GHE 2.12 - 2.20) 2018-08-25
- Upd: mark stalled reviews with an icon in the reviews list.
- Upd: raise LOC thresholds for considering a diff "big" and hiding it by default.
- Fix: dynamically back off pull request list request size when we run into GitHub's GraphQL bug.  The request size used to be a static value picked to work "most of the time", but could still result in incomplete results sometimes (with a warning shown to the user).  This new approach should be more reliable.
- Fix: for the `sandcastle` custom completion condition executor, fix error formatting and add support for `require`ing a hardcoded list of built-in modules.
- Fix: better tolerate malformed custom line link templates, and show any errors in the settings dropdown.
- Fix: support double-click text selection in diffs, as best as possible.  It still races with discussion creation and, if animated transitions are on, will sometimes immediately deselect the text.
- Fix: make `setCurrentDiscussionDisposition` work properly when editing a draft reply.
- Fix: increase reliability of redirect flavor of sign-in flow, and make it work in Edge.
- Fix: ensure that tooltips (e.g., on the toolbar counts) don't show stale descriptions sometimes.
- Fix: correctly compute unresolved / unreplied discussions when dismissals are pending.
- Fix: prevent UI elements (e.g., the Publish button) from occasionally disappearing when transition animations are turned off.
- Fix: promptly update mergeability and show merge button when branch protection is turned on in a repo.
- Fix: correctly style implicit code snippets in markdown, instantiated automatically by GitHub in response to a blob line range link.

#### 1868.2890 (min 1866.2875 GHE 2.12 - 2.20) 2018-08-04
- New: add a "Mark reviewed and go to next file / diff next revision" button at the bottom of diffs that need reviewing.  Also add a bindable command for this action (not bound by default).
- Upd: change the semantics of the "to review" counter in the toolbar to "to review in the current diffs" (as originally planned), and fix the Changes box in the toolbar to turn the revision red when there are more files to review at a different revision.
- Fix: correctly enforce min versions.  The last few releases will not automatically enforce the minimum rollback versions listed here, but please make sure to respect them nonetheless if you need to roll back.
- Fix: include commit status context name in the description shown in the Checks dropdown.
- Fix: make sure disposition dropdowns don't layer under another file's revision cells.
- Fix: avoid race condition when preparing the Merge Branch button that would sometimes result in user's settings being ignored.
- Fix: don't collapse a discussion as soon as a draft reply is created, but rather wait until it's sent.
- Fix: correctly handle repos with dots in their name on the Repositories page.
- Fix: force a permission refresh if a newly created repo is detected on the Repositories page.
- Fix: prevent crash when repeatedly editing settings of repos in multiple organizations on the Repositories page.
- Fix: update evaluated value when editing completion condition while an evaluation is already in progress.

#### 1866.2875 (min 1831.2835 GHE 2.12 - 2.20) 2018-07-24
- New: if a user is mentioned in a discussion (other than the main top-level thread), don't treat them as a reviewer unless they've taken review-like actions, e.g., marked a file as reviewed or started a new discussion.  This way, if you come into a review because somebody mentioned you to ask for spot advice, you won't see all files as to be reviewed and many discussions as to reply.
- New: disable "Approve" and "Request changes" publication options if custom review completion condition sets `disableGitHubApprovals` to `true` in its return value.  This is useful for teams that have an LGTM-centric workflow and don't want the confusion of GitHub approvals (even if they're ignored).
- New: added REVIEWABLE_LOG_GITHUB_API_LATENCY and REVIEWABLE_GITHUB_CACHE_SIZE environment variables.  See [configuration docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md) for details.
- Upd: make review synchronization more efficient for repositories with large file trees.  This especially impacts cases where a directory has a lot (1000+) of immediate subdirectories, which could previously bring the server to a virtual standstill.
- Fix: use commit timestamp instead of authoring timestamp when sorting commits or generating revision descriptions in top-level thread.  While these timestamps are usually the same, in some rebase or merging scenarios they can drift apart, and using the original authoring timestamp can result in new revision descriptions being considered "read".
- Fix: ensure constrast slider shows up in correct position when settings dropdown is opened.
- Fix: fix sample review completion condition for counting approvals; the previous code wouldn't actually do so.  Oops.
- Fix: allow repo admins to merge when GitHub's branch protection would normally block it but admins are exempted.
- Fix: display correct loading message when waiting for a review to be created (usually only in a disconnected repo).
- Fix: guard against various failures when `REVIEWABLE_LOGGING_URL` is specified.
- Fix: avoid producing temporarily invalid review structures when some events get handled out of order.  This could result in a "permission denied" message on the client while waiting for the review creation to finish.

#### 1844.2857 (min 1831.2835 GHE 2.12 - 2.20) 2018-07-14
- New: integrate with GitHub's review approval system.  When publishing from Reviewable you can set whether to approve, request changes, or just comment, with Reviewable picking a default state based on your discussion dispositions and file review marks.  This state gets published to GitHub and will be used by the branch protection system's required reviews option.  Reviewers' current effective state is also reflected in Reviewable (in the reviews list and on the review page) and available for use in custom review completion conditions.  ([Full changelog entry](https://headwayapp.co/reviewable-changes/github-reviews-integration-64906))
- New: add "Trust but verify" user setting.  When turned on, discussions where the user is _Discussing_ and that get resolved with no further comment will be treated as unreplied.  The new setting panel can be accessed from any disposition dropdown via a small gear icon.  ([Full changelog entry](https://headwayapp.co/reviewable-changes/trust-but-verify-65292))
- Upd: moved time-ago comment dividers ("N days ago", etc.) to be above the corresponding time period, rather than below, in a nod to widespread convention.
- Upd: if branch protection is turned on then defer to GitHub's mergeability determination, since we can't accurately duplicate the logic.  Note that this may result in Reviewable offering the option to merge earlier than it used to, if branch protection is set up more loosely than Reviewable's old built-in logic.
- Fix: update PR mergeability status in Reviewable on all events that could affect it, and do so in a timely manner.
- Fix: correctly use a default emoji if custom LGTM button output text is complex.
- Fix: make toolbar dropdowns (checks, changes) show up correctly when page is scrolled down.
- Fix: correctly list commits within diff bounds for current file in changes dropdown.

#### 1831.2835 (min 1801.2799 GHE 2.12 - 2.20) 2018-07-02
- New: allow user to tweak the app's visual contrast (e.g., of diff highlighting) through account settings dropdown.
- Upd: emit the server-side review status (including a custom review completion condition, if so configured) when publishing a review.
- Fix: work around a GitHub GraphQL bug that causes PRs to be randomly omitted from the review list when the list gets long.
- Fix: in review list, correctly fetch requested reviewers for PRs that don't yet have connected reviews.
- Fix: avoid permission denied error in the client when loading a review that hasn't yet been created.
- Fix: avoid permission denied error if the client gets disconnected at just the wrong moment when publishing comments, then reconnects.
- Fix: snapshot revisions when user acknowledges, changes disposition, or dismisses a participant.
- Fix: prevent disposition dropdown from disappearing under another layer in rare cases.
- Fix: improve auto-recovery from wrong webhook secret.

#### 1801.2799 (min 1785.2755 GHE 2.12 - 2.20) 2018-06-10
- New: overhaul discussion semantics, including disposition, resolution, unreplied counts, etc.  See [this post](https://headwayapp.co/reviewable-changes/discussion-semantics-overhaul-61097) for a summary, and [issue #510](https://github.com/Reviewable/Reviewable/issues/510) for details.  The most intrusive UX change is that _all_ state changes are created as drafts and must now be published to take effect, including acknowledgements, disposition changes, and dismissals.  Otherwise, I've done as much as possible to ensure that reviews in progress won't be disrupted and that users with old clients still loaded can collaborate with those who have the new version, but there may still be some minor bumps during the transition.
- Upd: added `±am:author`, `±am:assigned`, and `±am:requested` filters to the reviews list.
- Fix: correctly calculate number of marks and reviewed files in the presence of renames.
- Fix: re-enable rebase merging, got disabled by accident.
- Fix: don't crash when client gets disconnected in the middle of a transaction.
- Fix: keep better track of currently focused file (for applying keyboard shortcuts).
- Fix: greatly improve loading performance for reviews with many files (hundreds or higher), especially if a lot of files were renamed.

#### 1785.2755 (min 1755.2561 GHE 2.12 - 2.20) 2018-05-26
- Upd: remove automatically generated :shipit: emoji from published messages as it could be confusing in multi-reviewer situations.
- Upd: add `review.pullRequest.creationTimestamp` to completion condition data; not backfilled but should populate pretty quickly.
- Upd: tolerate new discussion semantics (coming in the next release!) in case of rollback.
- Fix: if user grants new permissions when sending comments, actually use those permissions when sending.
- Fix: correctly process label directives for labels that have a description.
- Fix: restore last-reviewer avatars in the file matrix.
- Fix: correctly configure merge button to delete or retain branch based on user settings and permissions.
- Fix: avoid unnecessarily updating the review when syncing a PR.
- Fix: correctly handle the "include administrators" branch protection flag.
- Fix: avoid occasional permission denied error when reconnecting to the network after a long time offline.
- Fix: address some very rare client crashes caused by race conditions and data edge cases.
- Fix: actually sort the repository lists on the Repositories page; they were only (mostly) sorted by accident before.

#### 1777.2720 (BETA, min 1755.2561 GHE 2.12 - 2.20) 2018-04-26
- New: enforce a minimum supported GHE version, starting with the relatively recent GHE 2.12.  This lets Reviewable take advantage of new APIs sooner, in particular new additions to GraphQL data.  The policy is to always support the two most recent GHE versions and the three most recent if possible.
- New: this release includes a complete rewrite of the client's data / logic layer for improved performance and consistency.  One extra bonus is that communication with Firebase is moved into a worker thread, offloading all the crypto to where it doesn't block the UI.  When using Chrome or Firefox the worker is shared between tabs, improving bootstrap time on subsequent tabs due to the connection already being established, and providing a shared data cache.
- New: offer option to load all diffs when any were skipped for any reason (e.g., throttling, too many files, etc.).
- New: make requested reviewers available to review completion conditions and update the samples to prefer requested reviewers over assignees when set.  If your users have custom review completion conditions for their repos they may want to tweak them as well.
- New: make available a new directive (`±reviewer:@username`) to manage requested reviewers, via any comment (in Reviewable, via GitHub, or via email).
- New: display how long ago each participant in a review last interacted with the review, and whether they have any drafts pending.  (Note that clients prior to this version don't report this information, so people who are hoarding an old Reviewable page will appear to be idle and have no pending drafts.)
- New: support `Merge manually by overwriting target` label to improve diffs in forked repos being synced with upstream changes; see [this changelog entry](https://headwayapp.co/reviewable-changes/reviews-in-forked-repos-that-track-upstream-changes-57413) for details.
- Upd: allow filter negation in reviews list and add more filters.
- Upd: reduce reviews list request and bandwidth requirements, and show labels and milestones even for unconnected PRs (thanks GraphQL!).
- Upd: add "waiting on me" and "being reviewed by me" sections to reviews list, and show blocking users instead of assignees next to the pointing hand.
- Upd: include reviews requested from your teams in the "involving my teams" section, and consider ancestor teams as well in all team-related queries.
- Upd: make review list search field bigger, and combine with pull request URL jump field.
- Upd: sort C/C++ header files before their corresponding implementation files.
- Upd: add support for label descriptions, when available.
- Upd: move "show full diff" button lower in the Changes box and make it available all the time.
- Upd: if a GitHub `pull_request` event was missed for a connected repository for some reason, create the review if any auxiliary events come in for it later.  This was already the case for comments, but now works for `push` and `status` events too.
- Fix: don't crash when adding a comment to the base of certain revisions of a renamed file.
- Fix: take diff regions that are collapsed by default (e.g., whitespace, base changes) into consideration when computing the size of the diff to decide whether it's too big to show.
- Fix: expand multi-line diff selections to include full first and last lines, and adjust rendering of collapsed quoted code to work with the latest GitHub Markdown renderer.
- Fix: work around a bug in Firefox where up/down cursor keys wouldn't work in a reply to a discussion until it was blurred and refocused.
- Fix: work around rare bug where page scrolled to the top and stayed stuck there after clicking Acknowledge (possibly limited to Firefox).
- Fix: prevent disabled dropdowns from getting re-enabled after display help overlay.

#### 1761.2574 (min 1664.2314) 2018-03-18
- Fix: deal correctly with rate limiting on GHE (whether turned on or off).

#### 1757.2561 (min 1664.2314) 2018-03-15
- Fix: use correct URL for GHE GraphQL queries.

#### 1745.2527 (min 1664.2314) 2018-03-11
- Upd: respect Go's standard "generated file" marker.
- Upd: use GraphQL when handling `push` events on the server instead of search API.
- Upd: prefer requested reviewers if set instead of assignees when determining who is needed to review a PR in the default review completion condition.  This change is on the server only and sets the stage for matching client-side changes coming in a follow-up release.
- Fix: add hard timeouts when checking queue health, to ensure that the process can never get stuck even if Firebase is down and its SDK is buggy.  This _should_ prevent Reviewable processes from going zombie in extreme and rare circumstances, where they're still alive but not doing any useful work.
- Fix: catch and handle some rare top-level exceptions that could cause a server process to go zombie.
- Fix: gracefully handle hiccups during comment sending that cause a duplicate write.
- Fix: don't die when a commit has more than 100 different status contexts.
- Fix: reset repeating cron job counters between runs.
- Fix: prevent error when connecting an empty repo.

#### 1694.2348 (min 1664.2314) 2018-01-17
- New: sweep database every 30 days to fix things up and delete stale redundant data, reducing long-term storage requirements.  Deleted data will be automatically refetched from GHE if needed later. WARNING: while a sweep is setting up, the instance will be temporarily locked out of doing other work for up to a few minutes. Please make sure you have at least 2 instances running at all times to avoid outages.
- Upd: remove `/_ah/start` and `/_ah/stop` handlers, add `/_ah/ready` handler, and listen to `SIGTERM` for graceful shutdown.  If you were using those handlers or are using health checks on your instances, please see the [updated configuration docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#container-configuration).
- Upd: replace `REVIEWABLE_GITHUB_CERT_FILE` config option with `NODE_EXTRA_CA_CERTS`.
- Fix: if a large tree fetch from GHE fails to parse incomplete data, treat it as a truncated result and fall back on manually recursing smaller fetches.  This should reduce transient errors when syncing reviews.

#### 1678.2324 (min 1664.2314) 2018-01-10
- Fix: sweep all reviews to correct broken owner/repo properties from renamed organizations and repositories.
- Fix: correctly compute review state on the server when faced with complex file rename chains.  This affects both the built-in review completion computation and custom completion conditions.
- Fix: bootstrap correctly on first install (again).

#### 1664.2314 (min 1549.2198) 2017-12-11
- Upd: update server to Node 8.
- Fix: correctly adjust reviews' owner/repo properties when processing an organization or repository name change.  Badly adjusted reviews will have some properties still pointing to the old owner/repo, which can cause permission grants to fail and users to be improperly denied access to the reviews.  This fixes the problem going forward, and a later change will fix the properties of reviews previously created in subsequently renamed repositories.
- Fix: don't overwrite disposition changes on discussions with comments imported from GitHub.

#### 1655.2258 (min 1549.2198) 2017-12-01
- New: add REVIEWABLE_GITHUB_CERT_FILE config option, for GHE servers with self-signed TLS certs.
- Fix: reduce number of GitHub status updates to avoid running into hardcoded limit in API.
- Fix: omit files whose revisions are all obsolete from proposed diffs, preventing the "each-commit" review workflow from getting "stuck" after marking all files reviewed.
- Fix: bootstrap correctly on first install when there are no reviews yet.
- Fix: correctly parse "Last, First" format names when sending emails; this format is sometimes used by user directory sync systems.
- Fix: avoid resetting `maintainer_can_modify` on PRs, due to a GitHub API bug.

#### 1638.2247 (min 1549.2198) 2017-10-08
- Fix: correctly deal with bot users introduced by the new(ish) GitHub Apps API.
- Fix: when collapsing a discussion (e.g., by clicking Acknowledge) whose top is off-screen, prevent the page from seeming to "jump down" to unrelated content.
- Fix: mark revisions as `obsolete` in data structure passed to custom review completion conditions.  You'll likely need to [update your condition code](https://headwayapp.co/reviewable-changes/completion-conditions-and-obsolete-revisions-35080), especially if you use force pushes in your workflow.

#### 1619.2237 (min 1549.2198) 2017-08-16
- New: added `REVIEWABLE_CONSOLE_MULTILINE_SEPARATOR` config option for environments that expect one-message-per-line console output.  See [config docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#monitoring) for more details.
- Upd: change AWS Lambda environment for review completion function execution from Node 4.3 to Node 6.10.
- Fix: detect errors caused by a suspended account and treat as a permanent issue (send email, disable repo connection).
- Fix: ensure resolved discussions stay collapsed when layout triggered (e.g., due to window resize).
- Fix: ensure expanded diff regions stay expanded when layout triggered (e.g., due to window resize).
- Fix: detect `CRLF` line-ending style and don't highlight `CR`s as trailing whitespace.
- Fix: correctly discriminate between "no basis for comparison between HEAD and BASE" and "all revisions are obsolete" errors when syncing a PR.

#### 1607.2231 (min 1549.2198) 2017-07-18
- Fix: improve large diff suppression by estimating a diff's impact on UI performance more accurately, and considering the sum total of all showing diffs rather than each file separately.
- Fix: short-circuit mergeability check on closed PRs to avoid waiting for a GitHub flag that never settles.
- Fix: correctly send warning emails about failures to parse user emails with Reviewable directives.
- Fix: deal gracefully with commits reverted via force push when no new commits are pushed at the same time.
- Fix: correctly diagnose errors with in-comment-badge author settings, sending emails to repo connector.
- Fix: change no-final-EOL glyph in diffs to be non-combining, to work around a Chrome rendering bug.

#### 1592.2216 (min 1549.2198) 2017-06-07
- Upd: create a sample Reviewable commit status when connecting a repo so that it's visible when configuring branch protection settings in GitHub before creating a PR.
- Upd: update syntax highlighting module and add Kotlin source file extension mappings.
- Fix: correctly enforce minimum version requirements; the previous logic was too strict and would disallow rollbacks that should've been permitted.

#### 1575.2214 (min 1549.2198) 2017-05-20
- New: allow organization owners to request automatic connection of all newly created repos to Reviewable.  You can find the new toggles on the Repositories page.  You can do it for personal repos as well, but the reaction to a new repo may be delayed by up to 2 minutes (since there's no webhook for personal repo creation).
- Upd: upgrade all connected repos to listen to _all_ GitHub webhook events.  This process will kick off automatically within minutes of starting up the new version and continue (with checkpoints) until finished, probably within a few minutes and definitely in less than 30 minutes.  (If you're doing a rolling upgrade, it may get delayed or start right away &mdash; either one is fine.)  While running, the upgrade process will keep one instance pretty busy so you might not want to upgrade during peak hours.  If you want to follow along, look for log lines with the token "migrate1549" for (minimal) status updates, and a final summary log line starting with "Migrated NNN legacy repositories" that indicates completion.  It's OK even if there's a few failures, as any repo that isn't completely idle will get updated the next time it sends a webhook anyway.  This upgrade process just hurries things along.
- Fix: prevent occasional "permission denied" crash when upgrading OAuth scopes on the Reviews or Repositories page.
- Fix: prune obsolete webhooks more aggressively in case they got migrated from github.com to GHE.

#### 1555.2206 (min 1313.2023) 2017-05-09
- Fix: use correct ghost user ID for GHE (substituted when a user account has disappeared for some reason).
- Fix: prevent spurious @-mentions of organizations or people with no access to repo from adding participants to a discussion.
- Fix: if a repo's GitHub status updates are set to "if review visisted", and a status was posted for a review, and a new PR/review was created for the same commit, then keep updating the status even if the new review wasn't visited yet.  The previous logic changed the status to "disabled by admin" which was confusing and incorrect.
- Fix: prevent a client crash when running in private mode and navigating directly to a review page before signing in (regression).

#### 1549.2198 (min 1313.2023) 2017-04-30
- New: show list of users occupying licensed seats when clicking on "M of N seats in use" in license info box on Repositories page.
- New: support migrating review data from reviewable.io to new enterprise instance, in case of migration from github.com to GHE.
- Upd: listen to _all_ GitHub webhook events for connected repos. This change will allow Reviewable to more easily support new GitHub features while remaining backwards-compatible with older GHE versions. It does mean that Reviewable instances will have to handle a higher load of incoming requests so you'll want to check your performance metrics after upgrading if you're not auto-scaling. (But unwanted events are dropped very quickly, so I don't expect a big impact.) For now, webhooks are updated to the new format opportunistically; a future release will sweep up any remainders.
- Upd: always hide the file matrix on load if >200 files to improve performance, overriding the user's preference.  You can still toggle the file matrix open after the page loads if you want.
- Fix: if popup auth gets stuck (seems to happen in some mobile browsers?) time out after 2 seconds and switch to using the redirect method instead.
- Fix: fix a time-of-check vs time-of-use bug when syncing a review with its PR that could result in bogus revisions being created in rare cases.
- Fix: make bindable `setCurrentDiscussionDisposition()` command work on newly created discussions that only have a draft comment.
- Fix: allow opening review files whose names start with a `.` in a new tab.

#### 1531.2183 (min 1313.2023) 2017-04-18
- New: add `REVIEWABLE_LOGGING_URL` setting to capture all console and exception logs in JSON format (if you don't want to set up Sentry and manually capture the server console); details in [config docs](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#monitoring).
- New: add button to copy the head branch name to the clipboard, and offer a bindable command for same.
- Fix: properly cancel comment file upload when progress placeholder deleted.
- Fix: render correct style in unchanged diff block due to changes in enclosing comments (two-column diffs only).
- Fix: don't crash server on startup if both encryption and local file uploads enabled.

#### 1517.2159 (min 1313.2023) 2017-02-18
- Upd: opportunistically fix webhooks when `REVIEWABLE_GITHUB_SECRET_TOKEN` is changed.
- Fix: reduce Firebase bandwidth usage again (regression).
- Fix: prevent occasional permission denied error when revision being snapshotted gets deleted mid-transaction.
- Fix: deal correctly with repos deleted and recreated multiple times with the same name.

#### 1491.2156 (min 1313.2023) 2017-01-23
- Upd: tighten up security headers when serving static files: `X-Content-Type-Options`, `X-Frame-Options`, and `X-XSS-Protection`.
- Fix: further improve workaround for Firebase transaction flakiness.  Detection is now more accurate and won't restart instances unnecessarily if other servers are picking up the load on affected keys.
- Fix: controlled instance restarts are now much faster (typically 5-10 seconds, max 60 seconds), whereas before they often timed out only after 5 minutes.

#### 1445.2150 (min 1313.2023) 2017-01-15
- New: support `REVIEWABLE_ANALYTICS_URL` to allow for tracking of major user actions and customized stats.  Please see the [config guide](https://github.com/Reviewable/Reviewable/blob/master/enterprise/config.md#monitoring) for details.
- New: enable subscription admin to restrict Reviewable users to members of a set of teams.  (The default is to continue to allow any GHE user to sign in.)
- Upd: upgrade server environment to Node v6.
- Upd: remove support for `GAE_MODULE_INSTANCE`, as the new GAE flex environment no longer provides this value and AFAIK no other container runner provides an ordinal instance number either.
- Upd: have server log errors with full stack traces to console if Sentry monitoring not set up.
- Fix: improve workaround for Firebase stuck transaction bug.  The condition will now be detected earlier, reducing thrashing, and can now recover without restarting the instance in many cases.
- Fix: set correct disposition on button-initiated "Done" reply when the user is the PR author and also a reviewer due to marking files as reviewed.  (Yes, all of those conditions are necessary to trigger the bug!)
- Fix: handle mixed-case emoji tokens correctly.
- Fix: handle a repo renaming corner case correctly (repo renamed then original recreated).

#### 1394.2104 (min 1313.2023) 2016-12-09
- Fix: switch client's production flag back on; it got accidentally turned off in 1390.2098, but the only effect was a slight performance degradation and a bit of extra monitoring UI in the lower-right corner

#### 1390.2098 (min 1313.2023) 2016-12-08
- New: respect `.gitattributes` `diff` attributes to suppress diffing of, e.g., generated files, or force diffing / pick syntax highlighting language
- Upd: allow specifying which user account should be the author of badge comments
- Upd: auto-fit diff width to window in finer increments
- Fix: eliminate bogus unhandled rejection errors on the server
- Fix: eliminate sporadic permission denied errors on the client due to `fillIssues` request contention
- Fix: limit elided file expansion hover target to just the path itself

#### 1362.2058 (min 1313.2023) 2016-11-29
- Fix: allow acknowledging discussions when disposition is "withdrawn"
- Fix: correctly render quoted text in batched comment messages (workaround for change in GitHub's Markdown parser)

#### 1360.2047 (min 1313.1977) 2016-11-14
- New: add option to put badge into a comment instead of PR description (control in repo settings panel)
- Fix: correcty check admin permissions for newly added repos on Repositories page
- Fix: don't require branch update if strict status checks turned on but no required status checks selected
- Fix: restore ability to sign in with Edge and Internet Explorer
- Fix: don't throw bogus fatal exception when user without push permissions visits a review in some conditions
- Fix: work around a GitHub bug where the API returns inconsistent data about a PR's commits
- Fix: detect hard AWS Lambda timeouts and return a better error message
- Fix: close malformed code blocks when publishing, so they don't corrupt the rest of the message

#### 1340.2023 (min 1313.1977) 2016-11-04
- Upd: use internal auth server in all environments, and shift some post-login processing from client to server
- Fix: allow new users to sign in!
- Fix: reduce memory usage when syncing large PRs
- Fix: capture more information for some exceptions
- Fix: get rid of most bogus exceptions (on server, not user-visible) when syncing GitHub status
- Fix: work around draft watermark rendering bug in Chrome and Safari on recent versions of Mac OS

#### 1313.2011 (min 1273.1977) 2016-10-29
- New: add `?debug=latency` for debugging page loading latency issues
- Upd: switch to a new promise/coroutine framework on the server for minor performance improvements
- Upd: add prefetch and preconnect directives to the page to improve initial load performance
- Upd: add syntax highlighting for CMake
- Fix: unreplied discussions counter for PR author no longer includes discussions with unsent drafts
- Fix: tighten up security to mitigate repo existence info leak by probing for other people's permission tickets
- Fix: determine fast-forward merge availability and "out-of-date" PR correctly
- Fix: speed up permission checks in various ways, and recommend using a 2048 bit RSA key for `REVIEWABLE_ENCRYPTION_PRIVATE_KEYS`
- Fix: tweak queue health alerting thresholds to reduce false positives, especially when running with a single instance
- Fix: fix various task lease issues that could lead to false positive error reports in Sentry
- Fix: fix minor issues with crash overlay
- Fix: fix minor layout issue with target branch editor in Firefox

#### 1277.1987 (min 1273.1977) 2016-10-14
- Fix: correctly grant permissions in an encrypted datastore for repos whose names need escaping
- Fix: take care of some query handling bugs exposed by the limited issue prefetch

#### 1275.1985 (min 1273.1977) 2016-10-13
- New: if the `X-Forwarded-Proto` header is set to `http`, and REVIEWABLE_HOST_URL has an `https` address, then issue a permanent redirect to the secure version of the requested URL
- Fix: unbreak Edge, which also has a non-compliant Web Crypto implementation
- Fix: unbreak Chrome when page served over HTTP
- Fix: make "My PRs in any public/private repo" connections work again (will retroactively connect past PRs)
- Fix: better support the rename-then-recreate repo workflow
- Fix: only prefetch the most recent 100 issues for autocompletion on page load to reduce latency; fetch all issues only when #-autocomplete triggered

#### 1273.1981 (min 1273.1977) 2016-10-11
- Fix: unbreak Safari, which was completely unable to load Reviewable once signed in due to a broken Web Crypto implementation

#### 1273.1979 (min 1273.1977) 2016-10-10
- Upd: support Web Crypto for encrypting GitHub tokens; use new REVIEWABLE_WEB_CRYPTO_REQUIRED config to force
- Fix: integrate fix for performance regression in AES crypto routines

#### 1273.1977 (min 1259.1971) 2016-10-10
- New: support rebase & merge and delegate merge style allowability to GitHub (if supported in GHE)
- Upd: prepare to support Web Crypto for encrypting GitHub tokens
- Upd: add bindable nextPersonallyUnreviewedFile (etc.) command
- Fix: ensure controlled shutdown actually exits the process, regardless of secondary failures
- Fix: use correct condition to determine whether branch can be fast forward merged
- Fix: resync PR after merge if repo not connected, to avoid merge button being enabled on page reload

#### 1259.1971 (min 1152.1875) 2016-10-03
- Upd: update syntax higlighting and code editor libraries
- Upd: prepare support for rebase & merge
- Fix: detect stuck transactions (due to Firebase SDK bug) and shut down so server can be restarted
- Fix: guard against a rare condition where a closed PR sync fails repeatedly
- Fix: allow uppercase chars in assignee directive usernames
- Fix: don't send low quota warning emails when there were transient errors looking for alternative admins

#### 1243.1957 (min 1152.1875) 2016-09-21
- New: [AES encryption key rotation tool](https://www.npmjs.com/package/firecrypt-tools) now available, along with [ops use instructions](https://github.com/Reviewable/Reviewable/blob/master/enterprise/operations.md#aes-encryption-key-rotation)
- New: [RSA encryption key rotation tool](https://www.npmjs.com/package/reviewable-enterprise-tools) now available, along with [ops use instructions](https://github.com/Reviewable/Reviewable/blob/master/enterprise/operations.md#rsa-encryption-key-rotation)
- Upd: add REVIEWABLE_DUMP_ENV to dump environment variables as Reviewable sees them, for debugging
- Upd: add REVIEWABLE_UPLOADS_PROVIDER to explicitly set uploads destination; requires config change if you're using file uploads!
- Upd: add REVIEWABLE_LAMBDA_EXECUTOR_ROLE and REVIEWABLE_LAMBDA_VPC_CONFIG to further configure AWS Lambda executor
- Upd: elide file path segments with ellipses only if extra space is needed for revision cells
- Upd: improve who will see a discussion as "unreplied", when everyone has seen the latest message but discussion is still unresolved
- Fix: ensure client always shows latest data from datastore; in some edge cases, it got stuck showing local "fake" values
- Fix: deal correctly with user username changes
- Fix: compute file path width correctly (it used to grow by 13px with each recomputation!) and don't waste space in single-file mode
- Fix: list all PRs on Reviews page when more than 100 in a single API request
- Fix: don't busy loop in extreme error situations when checking permissions

#### 1225.1935 (min 1152.1875) 2016-09-12
- New: show license details on Repositories page (for license admin user only)
- New: add maintenance mode that locks out the datastore for bulk updates (see new [ops playbook](https://github.com/Reviewable/Reviewable/blob/master/enterprise/operations.md) for instructions)
- Upd: verify session encryption key on the client and sign out if stale to enable key rotation
- Upd: share diff worker process between tabs (improves source code caching)
- Fix: pasting bug in recent versions of Firefox
- Fix: make automatic lease extension work in more cases
- Fix: work around stuck connections that force the user offline due to "stale writes"

#### 1203.1910 (min 1152.1875) 2016-09-02
- Fix: clean up more alternative admin selection corner cases
- Fix: improve error event grouping in Sentry
- Fix: make long-running tasks (e.g., large PR syncs) more reliable by automatically extending lease
- Fix: correctly parse broken up discussion URLs in emails
- Fix: adjust some regexes to deal with GHE URLs

#### 1193.1907 (min 1152.1875) 2016-08-31
- Fix: stop sending bogus "your authorization is broken" emails
- Fix: many issues related to all-organizations licenses

#### 1186.1905 (min 1152.1875) 2016-08-30
- Fix: balance GitHub calls among other admins for connected repositories when API quota gets low
- Fix: guesstimate GitHub burst quota usage and postpone or reassign tasks that might trigger an abuse warning
- Fix: set GitHub request timeouts based on remaining task lease time
- Fix: don't query `/rate_limit` on GitHub Enterprise instances

#### 1168.1901 (min 1152.1875) 2016-08-25
- New: optional encryption of user-controlled text properties in the datastore
- New: private mode option for server
- New: expose ability to switch a PR's base branch
- Upd: mark outdated revisions, simplify rebase arcs visualization, and improve auto-diff bound picks when using "review each commit" style
- Fix: undo regression in schema validation constraints
- Fix: make review page somewhat usable on mobile devices
- Fix: [#388](https://github.com/Reviewable/Reviewable/issues/388), [#389](https://github.com/Reviewable/Reviewable/issues/389)

#### 1158.1885 (min 1152.1875) 2016-08-15
- Upd: improved styling of wrapped lines in diff and fixed some line length computation bugs
- Upd: turned on gzip compression in the web server
- Fix: security issue fix part 2
- Fix: [#385](https://github.com/Reviewable/Reviewable/issues/385)

#### 1152.1875 (min 1063.1721) 2016-08-08
- Upd: improved detection of Go declaration headers
- Fix: split reply emails into separate discussions
- Fix: security issue fix part 1

#### 1146.1862 (min 1063.1721) 2016-08-07
- Fix: automated detection and reconnection of renamed repos and organizations
- Fix: improved scaling in the face of very high GitHub event rates
- Fix: fixed a number of bugs around drafts, markdown rendering, and publishing (some of which could cause you to get stuck or, in rare cases, data loss)
- Fix: properly force branch "retain" mode in merge button, and ensure button will be enabled on reviews dashboard
- Fix: [#366](https://github.com/Reviewable/Reviewable/issues/366), [#367](https://github.com/Reviewable/Reviewable/issues/367), [#368](https://github.com/Reviewable/Reviewable/issues/368), [#369](https://github.com/Reviewable/Reviewable/issues/369), [#371](https://github.com/Reviewable/Reviewable/issues/371)

#### 1130.1815 (min 1063.1721) 2016-07-20
- New: squash and fast-forward merges, editing of merge commit message
- New: update source branch from target
- Upd: removed REVIEWABLE_GITHUB_VIRGIN_USERNAME env var as it's no longer needed
- Upd: [#359](https://github.com/Reviewable/Reviewable/issues/359)
- Fix: [#308](https://github.com/Reviewable/Reviewable/issues/308), [#358](https://github.com/Reviewable/Reviewable/issues/358)

#### 1117.1791 (min 1063.1721) 2016-07-14
- New: all repo settings now available in a single panel
- New: new repos can now inherit settings from a prototype repo
- Upd: [#350](https://github.com/Reviewable/Reviewable/issues/350)
- Fix: adapt to non-backwards-compatible change in Firebase authentication server
- Fix: [#340](https://github.com/Reviewable/Reviewable/issues/340), [#341](https://github.com/Reviewable/Reviewable/issues/341), [#352](https://github.com/Reviewable/Reviewable/issues/352), [#357](https://github.com/Reviewable/Reviewable/issues/357)

#### 1104.1771 (min 1063.1721) 2016-06-25
- Fix: bootstrap when license has a username instead of a user id
- Fix: support GitHub multiple assignees (when the feature shows up in GHE, at least)
- Fix: lots of small things on both client and server

#### 1087.1754 (min 1063.1721) 2016-05-30
- New: support GitHub Enterprise
- New: use GitHub OAuth directly for authentication instead of delegating to Firebase.
- Upd: autoquote selected text on reply.
- Upd: support per-file determination of reviewed status in custom condition.
- Upd: collapse large quoted code blocks in comments.
- Upd: read configuration from file instead of env vars.
- Fix: hover bug on lower-right-corner indicators.
- Fix: not fully treating PR author as reviewer after they've marked a file as reviewed.
- Fix: welcome offered to users who aren't members of any subscribed org.

#### 1077.1739 (min 1063.1721) 2016-05-20
- New: welcome message for new org members with one all-in authorization button.
- Upd: show who a review is waiting on, and support in custom conditions.
- Upd: shorten default GitHub status message to fit in 50 chars.
- Upd: revised semantics for "discussing" disposition, and OK/FYI intent support.
- Fix: rare undefined value bug in client.
- Fix: hide buttons and text related to private repos if license is public-only.
- Fix: don't add code quote if other quote already included in comment.
- Fix: other minor fixes for rare client crashes.

#### 1065.1721 (min 1063.1721) 2016-05-10
- Initial public release.
