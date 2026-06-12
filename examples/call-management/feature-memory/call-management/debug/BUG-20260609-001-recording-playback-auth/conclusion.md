# Conclusion

## Memory Index

- Bug ID: `BUG-20260609-001-recording-playback-auth`
- Feature: `call-management`
- Root cause category: `auth/browser-native-request`
- Keywords: `recording`, `401`, `audio`, `auth header`, `blob`

## Current Status

- Status: `RESOLVED`
- Latest conclusion: the backend recording proxy worked, but browser-native playback failed because the request did not include the required auth header.

## Confirmed Facts

- The recording proxy endpoint returned an audio response when called through an authenticated request.
- The same recording failed in the browser when loaded through native `<audio>` or direct window navigation.
- Native browser resource requests did not pass through the frontend HTTP interceptor.
- The recording route must remain authenticated.

## Fix

- Add a frontend API helper that requests the recording file as a blob through the authenticated HTTP client.
- Create an object URL from the blob for `<audio>` playback and download.
- Revoke object URLs when the detail view closes or the recording list refreshes.

## Reusable Lesson

When an authenticated file, image, video, or audio endpoint works through HTTP client testing but fails in browser playback with `401`, check whether the browser is using a native resource request that bypasses auth interceptors.

## Safety Notes

- Do not log object storage credentials.
- Do not write real production object keys into feature memory.
- Do not paste raw production response bodies into debug notes unless they are redacted.
