# Self-hosting Jitsi for video calls/conferencing (and first contribution to open source project)

Are you looking for a self-hosted platform for video calls for personal use or across your organization?
I recommend [Jitsi](https://github.com/jitsi/jitsi-meet/blob/master/doc/quick-install.md) for its quick and direct installation steps.

I could successfully install all required Jitsi components and was able to pull my friends into a call. However none of us could see or hear one another in our devices.

It turned out that the network configuration is to be understood to be behind NAT.

Head over to [here](https://community.jitsi.org/t/cannot-see-video-or-hear-audio-on-self-hosted-instance/) for my public question to which [@damencho](https://github.com/saghul) and [@saghul](https://github.com/damencho) immediately jumped to resolve. I got the whole issue sorted in 2 nights.

So once I resolved my issue, I figured the documentation can be updated to cover future cases as mine. Open source and community FTW!

All I'd need to do for this is writing how I resolved my issue in a text (Markdown) document. How tough can it be?

This is how you would proceed if you wanted to send your first pull request (PR) to this project on Github:

1. Go to Github main page for the project and copy URL to clone
2. git clone `<project-url>`
3. cd `<project-directory>`
4. Edit `doc/quick-install.md`
5. Create a new `doc/faq.md`
6. `git add *`
7. `git commit -m "Added [write your intention here]"`
8. `git push`

Here, I see that my push is rejected with a HTTP 403 error. I smile at myself for forgetting to fork before cloning in the Github interface.

In general, projects on Github do not allow non-project members to push directly. This is common practice. You'd have to make a fork (a.k.a copy because you can. Open source! Yay!) and work on that copy of yours.

Since I already cloned the original repository to my system, instead of deleting and redoing my changes, I use the Git's `add remote` feature.

So additional steps I had to do were:

1. copy the new URL (for our forked repo)
2. `git remote add fork <new-url>`
3. `git push --set-upstream fork  ssabniveesu/add-faq-and-detail-on-nat`

    (Here, 'fork' is the identifier I set for my fork. You can set anything you want.)

4. Go to Github (either your repo or the original one) and choose 'Create New Pull Request'
5. Github would suggest the source branch to be your just pushed branch and the destination to be the closest matching original branch of parent project. You may choose a different combination depending on project's guidelines generally detailed in some `CONTRIBUTING.MD`

Done! You can see mine [here](https://github.com/jitsi/jitsi-meet/pull/3910)