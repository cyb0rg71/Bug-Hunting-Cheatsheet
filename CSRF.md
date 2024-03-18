For a CSRF attack to be possible, three key conditions must be in place:
```
1. A relevant action.
2. Cookie-based session handling.
3. No unpredictable request parameters.
```
### Bypassing CSRF token validation.
```
1. Remove CSRF token and see if application accepts it.
2. Change request method POST to GET.
3. See if user token is tied to user session. [ To test it, use another user's token in your token ]
```
