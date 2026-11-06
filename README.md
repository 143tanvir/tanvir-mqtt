<div align="center">⚡ TANVIR-MQTT

ADVANCED INSTAGRAM CLIENT · ICA FUNCTION REFERENCE

<img src="https://img.shields.io/badge/TANVIR143-00D9FF?style=for-the-badge&labelColor=080D1A" />
<img src="https://img.shields.io/badge/ICA-795BFF?style=for-the-badge&labelColor=080D1A" />
<img src="https://img.shields.io/badge/VERSION-2.9.0-00C853?style=for-the-badge&labelColor=080D1A" />A powerful, modular Instagram client interface built for developers.

"Instagram" (https://www.instagram.com/ig.tanvir_ahmed) · "GitHub" (https://github.com/143tanvir)

</div>---

«DOCUMENTATION / 2026

This documentation describes the methods and runtime interfaces exposed by the "tanvir-mqtt" source included in this project. It is not a generic ICA method list or official Instagram API documentation.»

◈ 01 — Overview

tanvir-mqtt is a modular Instagram client implementation designed to expose account, messaging, media, social, search, and realtime operations through a unified JavaScript interface.

The included source provides an ICA-compatible client structure with authentication helpers, session management, request handling, media operations, and realtime functionality.

Core capabilities

Module| Capabilities
Authentication| Login, sessions, account verification
Direct Messages| Messaging, replies, reactions
Realtime| MQTT, FBNS, event listeners
Social| Followers, following, profiles
Media| Photos, videos, reels, stories
Publishing| Uploads, albums, media publishing
Search| Users, hashtags, music, locations
Account| Profile, security, notifications
Insights| Account and media analytics

◈ 02 — Project Information

Property| Value
Project| tanvir-mqtt
Version| 2.9.0
Entry point| "index.js"
Runtime| Node.js
Minimum Node.js| 14+
Interface| ICA-compatible
Main language| JavaScript
Maintainer| Tanvir Ahmed
GitHub| 143tanvir

Directory structure

Tanvir143/
└── tanvir-mqtt/
    ├── index.js
    ├── package.json
    ├── package-lock.json
    ├── assets/
    ├── src/
    ├── lib/
    ├── utils/
    └── README.md

The exact internal directory structure may differ depending on the distributed source.

◈ 03 — Installation

Open the project directory:

cd Tanvir143/tanvir-mqtt

Install dependencies:

npm install

Verify the Node.js installation:

node -v
npm -v

◈ 04 — Initialization

Import the local client:

const ica = require("./Tanvir143/tanvir-mqtt");

Authenticate with an existing session:

async function start() {
    try {
        const api = await ica.login({
            session: YOUR_SESSION_OBJECT
        });

        console.log("Connected:", api.username);
        console.log("User ID:", api.userId);

    } catch (error) {
        console.error("Login failed:", error);
    }
}

start();

Credential-based login

Where supported by the included login implementation:

const api = await ica.login({
    username: process.env.IG_USERNAME,
    password: process.env.IG_PASSWORD
});

Security note: Prefer session-based authentication where appropriate. Never hardcode passwords, session cookies, or access credentials in public repositories.

◈ 05 — Session Management

The client includes session-related helpers:

const session = await api.getSession();

await api.saveSession("./session.json");

await api.logout();

Method| Purpose
"getSession()"| Retrieve current session data
"saveSession()"| Save session data
"logout()"| End the authenticated session

Keep session files private and never commit them to GitHub.

◈ 06 — Execution Model

The client primarily supports asynchronous JavaScript execution.

Promise / Async-Await

const result = await api.getThreads();
console.log(result);

Promise handling

api.getThreads()
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });

Callback compatibility

Some methods may support callback-style execution:

api.someMethod((error, result) => {
    if (error) {
        console.error(error);
        return;
    }

    console.log(result);
});

Callback availability and argument order depend on the individual method implementation.

◈ 07 — Direct Messaging

Send a message

await api.sendMessage({
    text: "Hello from tanvir-mqtt!",
    threadIds: [THREAD_ID]
});

Compatibility-style example:

await api.sendMessage("Hello!", THREAD_ID);

Send to user IDs

await api.sendMessage({
    text: "Hello!",
    userIds: [USER_ID]
});

Reply to a message

await api.sendReply(
    THREAD_ID,
    "This is a reply",
    {
        id: MESSAGE_ID,
        client_context: CLIENT_CONTEXT
    }
);

◈ 08 — Media Messaging

Send a photo

await api.sendPhoto({
    path: "./photo.jpg",
    threadIds: [THREAD_ID]
});

Send a video

await api.sendVideo({
    path: "./video.mp4",
    threadIds: [THREAD_ID]
});

Send a voice message

await api.sendVoice({
    path: "./voice.m4a",
    threadIds: [THREAD_ID]
});

Send a file

await api.sendFile({
    path: "./attachment.jpg",
    threadIds: [THREAD_ID]
});

Media options

Depending on the method, supported options may include:

- "path" — Local media file.
- "url" — Remote media URL.
- "threadIds" — Target conversation IDs.
- "threadId" — Single conversation ID.
- "userIds" — Target user IDs.
- "userId" — Single user ID.
- "replyTo" — Message reply reference.

Use the target format accepted by the specific method.

Media compatibility notes

- Photo handling recognizes common image extensions such as JPG, JPEG, PNG, and WebP.
- Without an available image re-encoding dependency, the passthrough path may support JPEG only.
- Video uploads are intended for supported MP4 media.
- Voice uploads use supported M4A/AAC media.
- "sendFile()" may route supported images and MP4 files through media-specific methods rather than arbitrary document uploads.

◈ 09 — Reactions & Message Controls

Add a reaction

await api.sendReaction(
    THREAD_ID,
    MESSAGE_ID,
    "❤️"
);

Set a message reaction

await api.setMessageReaction({
    threadId: THREAD_ID,
    messageId: MESSAGE_ID,
    reaction: "❤️"
});

Remove a reaction

await api.deleteReaction(
    THREAD_ID,
    MESSAGE_ID
);

Unsend a message

await api.unsendMessage(
    THREAD_ID,
    MESSAGE_ID
);

Typing indicator

await api.setTyping(THREAD_ID, true);

Typing support may depend on the availability of the realtime transport.

◈ 10 — Realtime & Event System

The client exposes realtime-related methods for event handling and connection management.

Listen for events

const stop = await api.listen(event => {
    console.log("Realtime event:", event);
});

"listenMqtt()" is also available as an alias of "listen()" in the included interface.

Connection methods

Method| Description
"listen()"| Start listening for events
"listenMqtt()"| Realtime listener alias
"realtimeConnect()"| Connect realtime transport
"realtimeDisconnect()"| Disconnect realtime transport
"realtimeOn()"| Register realtime events
"realtimePing()"| Realtime ping
"realtimeReadOnce()"| Read one realtime event
"fbnsConnect()"| Connect FBNS transport
"fbnsDisconnect()"| Disconnect FBNS transport
"fbnsOn()"| Register FBNS events
"fbnsPing()"| FBNS ping
"fbnsReadOnce()"| Read one FBNS event

Always handle connection failures and reconnect conditions in applications that depend on realtime events.

◈ 11 — Complete Function Directory

The following directory lists the method names exposed by the bundled source documentation, grouped by functionality.

«This is a method index, not a guarantee that every method has the same argument signature. Refer to the actual implementation before using an undocumented method.»

11.1 Authentication & Session

"loginWithCredentials()" · "loginWithSession()" · "getSession()" · "saveSession()" · "bloksAction()" · "getSignupConfig()" · "checkEmail()" · "checkUsername()" · "checkPhoneNumber()" · "sendSignupSmsCode()" · "sendVerifyEmail()" · "checkConfirmationCode()" · "checkAgeEligibility()" · "accountsCreate()" · "signup()" · "getFeaturedAccounts()" · "getAccountFamily()" · "getUsdidSettings()" · "setUsdidSettings()" · "usdidGenerate()" · "usdidPublicKeyDer()" · "usdidHeader()" · "usdidRegistrationToken()" · "usdidRegister()" · "attestationCreateAndroidKeystore()" · "attestationParams()"

11.2 Direct Messages, Threads & Realtime

"getThreads()" · "getThread()" · "getMessages()" · "pendingRequests()" · "getPendingRequests()" · "approveRequest()" · "markThreadSeen()" · "sendMessage()" · "sendReply()" · "unsendMessage()" · "sendReaction()" · "deleteReaction()" · "likeDirectMessage()" · "unlikeDirectMessage()" · "listen()" · "listenMqtt()" · "realtimeConnect()" · "realtimeDisconnect()" · "realtimeOn()" · "realtimePing()" · "realtimeReadOnce()" · "fbnsConnect()" · "fbnsDisconnect()" · "fbnsOn()" · "fbnsPing()" · "fbnsReadOnce()" · "sendTyping()" · "setTyping()" · "sendPhoto()" · "sendVideo()" · "sendVoice()" · "sendFile()" · "setMessageReaction()" · "deleteMessage()" · "getThreadAdmins()" · "getPendingCount()" · "searchThreads()" · "searchMessages()" · "getUsersPresence()" · "getActivePresence()" · "searchGenAiBots()" · "getChannels()" · "getThreadByParticipants()" · "createThread()" · "hideThread()" · "updateThreadTitle()" · "addUsersToThread()" · "markThreadUnread()" · "muteThread()" · "unmuteThread()" · "muteVideoCall()" · "unmuteVideoCall()" · "setE2eeEligibility()" · "getSpamInbox()" · "getSpamInboxChunk()" · "removeMember()" · "leaveGroup()" · "joinGroupInvite()" · "resolveGroupInvite()" · "setGroupTitle()" · "getGroupMembers()" · "inspectAccount()" · "shareMedia()" · "shareStory()" · "shareProfile()" · "sendCutoutSticker()" · "preferMqttActions()"

11.3 User, Profile & Social

"getUserInfo()" · "getUserInfoByUsername()" · "getUserIdFromUsername()" · "getUsernameFromUserId()" · "usernameFromUserId()" · "createNote()" · "deleteNote()" · "getNotes()" · "notesMusicBrowser()" · "createMusicNote()" · "getFollowers()" · "getFollowing()" · "getMutualFollowers()" · "getFollowersChunk()" · "getFollowingChunk()" · "iterUserFollowers()" · "iterUserFollowing()" · "searchFollowers()" · "searchFollowing()" · "getFriendship()" · "getUserFriendship()" · "getUserFriendships()" · "getCreatorInfo()" · "linkAddressBook()" · "unlinkAddressBook()" · "getFeedUserStreamItem()" · "follow()" · "unfollow()" · "block()" · "unblock()" · "getFollowRequests()" · "getFollowRequestsChunk()" · "approveFollowRequest()" · "declineFollowRequest()" · "approveFollowRequests()" · "declineFollowRequests()" · "closeFriendAdd()" · "closeFriendRemove()" · "mutePostsFromFollow()" · "unmutePostsFromFollow()" · "muteStoriesFromFollow()" · "unmuteStoriesFromFollow()" · "setPostsNotifications()" · "setVideosNotifications()" · "setReelsNotifications()" · "setStoriesNotifications()" · "removeFollower()" · "reportUser()" · "getUserAbout()" · "getUserGuides()" · "getChaining()" · "fetchSuggestionDetails()" · "getSuggestedProfiles()" · "getRelatedProfiles()" · "getRecommendedAccountsForCategory()" · "getUserStreamByUsername()" · "getUserStreamById()" · "getUserStreamByIdFlat()" · "getUserStreamByUsernameFlat()" · "getWebProfileInfo()" · "getUserShortGql()" · "getUsernameFromUserIdGql()" · "getUserInfoV2Gql()" · "getUserInfoByUsernameV2Gql()" · "getFollowersPrivateGqlChunk()" · "getFollowersPrivateGql()" · "getFollowingPrivateGqlChunk()" · "getFollowingPrivateGql()" · "getClipsProfileGql()" · "getInboxTrayGql()" · "getUserMedias()" · "getUserMediasPaginated()" · "iterUserMedias()" · "getUserTags()" · "getUserTagsPaginated()" · "getUserStories()"

11.4 Account & Notifications

"getAccountInfo()" · "editAccount()" · "setBiography()" · "setExternalUrl()" · "removeBioLinks()" · "getBioLinks()" · "changeProfilePicture()" · "setPrivate()" · "setPublic()" · "convertToBusiness()" · "convertToCreator()" · "convertToProfessional()" · "getNewsInbox()" · "setAiInfoLabel()" · "getSecurityInfo()" · "changePassword()" · "sendPasswordReset()" · "sendConfirmEmail()" · "confirmEmail()" · "sendConfirmPhone()" · "confirmPhone()" · "setNotificationSetting()" · "muteAllNotifications()" · "disableAllNotifications()" · "notificationLikes()" · "notificationLikeAndCommentOnPhotoUserTagged()" · "notificationUserTagged()" · "notificationComments()" · "notificationCommentLikes()" · "notificationFirstPost()" · "notificationNewFollower()" · "notificationFollowRequestAccepted()" · "notificationConnection()" · "notificationTaggedInBio()" · "notificationPendingDirectShare()" · "notificationDirectShareActivity()" · "notificationDirectGroupRequests()" · "notificationVideoCall()" · "notificationRooms()" · "notificationLiveBroadcast()" · "notificationFelixUploadResult()" · "notificationViewCount()" · "notificationFundraiserCreator()" · "notificationFundraiserSupporter()" · "notificationReminders()" · "notificationAnnouncements()" · "notificationReportUpdated()" · "notificationLogin()"

11.5 Media, Feed, Comments & Collections

"getMediaInfo()" · "downloadMedia()" · "getStory()" · "getUserStories()" · "downloadStory()" · "getLikedMedias()" · "getMediaLikers()" · "getCommentReplies()" · "deleteMedia()" · "editMedia()" · "linkReel()" · "archiveMedia()" · "unarchiveMedia()" · "getArchivedMedias()" · "pinMedia()" · "unpinMedia()" · "pinReel()" · "unpinReel()" · "markMediaSeen()" · "markClipsSeen()" · "getMediaOembed()" · "getMediaAuthor()" · "getMediaInfoV2()" · "videoRefreshResources()" · "uploadCutoutSticker()" · "getUserClips()" · "getUserVideos()" · "createMediaNote()" · "deleteMediaNote()" · "createLivestream()" · "startLivestream()" · "endLivestream()" · "getLivestreamInfo()" · "getLivestreamComments()" · "getLivestreamViewers()" · "getTimelineFeed()" · "getReels()" · "getExploreReels()" · "getFriendsReels()" · "getComments()" · "getCommentsChunk()" · "postComment()" · "checkOffensiveComment()" · "checkOffensiveCommentRaw()" · "likeComment()" · "unlikeComment()" · "pinComment()" · "unpinComment()" · "bulkDeleteComments()" · "getCommentLikersChunk()" · "getCommentLikers()" · "getCollections()" · "getCollectionIdByName()" · "getCollectionMediasChunk()" · "getCollectionMedias()" · "getCollectionMediasByName()" · "saveMedia()" · "unsaveMedia()"

11.6 Publishing & Upload

"uploadPhoto()" · "uploadPhotoWithMusic()" · "uploadVideo()" · "uploadAlbum()" · "uploadAlbumWithMusic()" · "uploadIGTV()" · "uploadReel()" · "uploadReelWithMusic()" · "clipTrialEligible()" · "clipInterestTopics()" · "changeReelCover()"

11.7 Stories & Highlights

"getUserStories()" · "getStory()" · "downloadStory()" · "uploadPhotoStory()" · "uploadVideoStory()" · "deleteStory()" · "storyBuilder()" · "getStoryViewers()" · "getStoryTray()" · "getStoryInfo()" · "getStoryLikersChunk()" · "getStoryLikers()" · "likeStory()" · "unlikeStory()" · "voteStoryPoll()" · "getStickerTray()" · "getArchiveStoryDaysPaginated()" · "getArchiveStoryDays()" · "getArchiveStories()" · "shareMediaToStory()" · "highlightPkFromUrl()" · "getUserHighlights()" · "getHighlightInfo()" · "createHighlight()" · "editHighlight()" · "changeHighlightTitle()" · "changeHighlightCover()" · "addStoriesToHighlight()" · "removeStoriesFromHighlight()" · "deleteHighlight()"

11.8 Search, Discover, Music, Hashtags & Locations

"searchLocations()" · "getLocationInfo()" · "getLocationMedias()" · "getLocationMediasChunk()" · "getLocationMediasTop()" · "getLocationMediasRecent()" · "searchLocationsByName()" · "searchLocationByPk()" · "completeLocation()" · "buildLocation()" · "locationStoryStickerId()" · "getLocationGuides()" · "getAccountInsights()" · "getMediaInsights()" · "searchMusic()" · "musicInFeedBrowser()" · "musicTrending()" · "searchMusicV2()" · "musicKeywordSearch()" · "bookmarkTrack()" · "bookmarkedTracks()" · "clipsAudioBrowser()" · "verifyAudioTitle()" · "trackInfoByCanonicalId()" · "trackInfoById()" · "trackStreamInfoById()" · "topsearchFlat()" · "searchUsersAdvanced()" · "searchHashtags()" · "searchPlaces()" · "searchMedias()" · "webSearchTopsearch()" · "webSearchTopsearchHashtags()" · "fbsearchItem()" · "keywordTypeahead()" · "typeaheadStream()" · "typeaheadUsers()" · "recentSearches()" · "searchAccountsV2()" · "searchReelsV2()" · "topsearchV2()" · "suggestedProfiles()" · "searchUsers()" · "getHashtagInfo()" · "getHashtagMediasChunk()" · "getHashtagMedias()" · "getHashtagMediasTop()" · "getHashtagMediasRecent()" · "getHashtagMediasReels()" · "followHashtag()" · "unfollowHashtag()" · "getFollowedHashtags()"

◈ 12 — Core Client & Runtime

Client methods

Method| Description
"getAuthHeader()"| Retrieve authentication header
"checkProxyIp()"| Check proxy IP
"cookies()"| Access cookie collection
"cookieValue(name)"| Retrieve a cookie value
"getCurrentUser()"| Retrieve current user
"logout()"| End client session

Client properties

"sessionid" · "dsUserId" · "csrftoken" · "rankToken" · "phoneId" · "uuid" · "androidDeviceId" · "userAgent" · "appVersion" · "locale" · "country"

Runtime helpers

"generateTOTP(seed, options)" · "checkForUpdate" · "maybeAutoUpdate"

◈ 13 — Package Exports

The package exposes the following main exports:

const {
    login,
    Client,
    checkForUpdate,
    maybeAutoUpdate
} = require("./Tanvir143/tanvir-mqtt");

Error classes

The included error classes may include:

- "LoginError"
- "BadPassword"
- "BadCredentials"
- "TwoFactorRequired"
- "ChallengeRequired"
- "SessionExpired"
- "PleaseWaitFewMinutes"
- "DirectThreadNotFound"
- "DirectMessageNotFound"
- "RealtimeNotConnected"
- "UserNotFound"

Additional errors may be available in the source.

◈ 14 — Error Handling

Always wrap network and authentication operations in error handling.

try {
    const result = await api.getThreads();

    console.log(result);

} catch (error) {
    console.error("Operation failed:", error.message);
}

Common issues

Error| Possible cause
"BadCredentials"| Invalid login credentials
"TwoFactorRequired"| Additional verification required
"ChallengeRequired"| Account verification required
"SessionExpired"| Session is no longer valid
"PleaseWaitFewMinutes"| Temporary request restriction
"RealtimeNotConnected"| Realtime connection unavailable
"DirectThreadNotFound"| Conversation not found
"DirectMessageNotFound"| Message not found
"UserNotFound"| User could not be resolved

Error messages may change depending on Instagram's response.

◈ 15 — Compatibility & Troubleshooting

Login problems

- Verify session validity.
- Check account verification requirements.
- Ensure the system clock is correct.
- Avoid exposing authentication credentials.

Media upload problems

- Verify the media file exists.
- Check the file format and encoding.
- Install optional dependencies required by the selected media operation.
- Check whether the media exceeds supported limits.

Realtime problems

- Check network connectivity.
- Verify authentication status.
- Handle disconnect and reconnect events.
- Do not assume realtime events are always available.

Request restrictions

Instagram may apply temporary restrictions, rate limits, or additional verification. Avoid excessive requests and respect platform restrictions.

◈ 16 — Security Guidelines

- Never publish session cookies.
- Never commit ".env" files or private session data.
- Do not expose passwords or authentication headers in logs.
- Keep credentials outside source code.
- Use responsible request frequency.
- Respect account privacy and user consent.
- Do not use the client for spam, harassment, or unauthorized access.

◈ 17 — Important Disclaimer

"tanvir-mqtt" is an independent, unofficial client implementation.

It is not affiliated with, endorsed by, or officially supported by Instagram or Meta.

Instagram may change its internal endpoints, authentication procedures, request formats, and realtime protocols without notice. As a result, some methods may stop working or require updates.

Use this software responsibly and only with accounts and data you are authorized to access.

---

<div align="center">◇ BUILT WITH PRECISION. MAINTAINED WITH PURPOSE.

TANVIR-MQTT

Advanced Instagram Client · ICA Function Reference

Maintained By Tanvir Ahmed (Tanvir143)

"Instagram @ig.tanvir_ahmed" (https://www.instagram.com/ig.tanvir_ahmed)

"GitHub — 143tanvir" (https://github.com/143tanvir)

<sub>© 2026 Tanvir143 · All rights reserved.</sub>

</div>
