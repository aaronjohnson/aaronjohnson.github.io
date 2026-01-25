# Firebase API Keys Aren't Secrets (And Why Scanners Get It Wrong)

*That security alert about your "leaked" Firebase key? Probably a false positive.*

## The Alert

Google Cloud Trust & Safety emails you:

> We have detected a publicly accessible Google API key...

Your heart sinks. Then you realize: you put it there on purpose.

```javascript
const firebaseConfig = {
    apiKey: "AIzaSy...",  // "THE LEAK"
    projectId: "myproject",
};
```

This is standard Firebase Web SDK initialization. It's *supposed* to be public.

## API Keys Are Not All Equal

Most API keys grant access. Firebase web keys just identify your project.

| Traditional API Key | Firebase Web Key |
|---------------------|------------------|
| Acts as you | Identifies the project |
| Must be secret | Designed to be public |
| Security = hide it | Security = Firebase Rules |

The key gets them to the door. **Security Rules decide if they get in.**

## The Real Security: Rules

```javascript
match /ios-waitlist/{doc} {
  allow create: if request.resource.data.keys().hasOnly(['email', 'timestamp']);
  allow read, update, delete: if false;
}
match /{document=**} {
  allow read, write: if false;
}
```

With these rules, even with your API key, attackers can:
- ✓ Add an email to the waitlist
- ✗ Read, update, delete anything
- ✗ Access any other collection

## Add Restrictions Anyway

Defense in depth. In GCP Console or via CLI:

```bash
gcloud services api-keys update YOUR_KEY_ID \
  --project=YOUR_PROJECT \
  --allowed-referrers="yourdomain.com/*,localhost:*"
```

Now the key only works from your domains.

## The Right Response

1. **Don't panic** — Firebase web keys are public by design
2. **Verify** — Check your Security Rules
3. **Restrict** — Add referrer restrictions
4. **Document** — Mark alert as false positive

## What Would Actually Be Bad

- ❌ Rules with `read, write: if true`
- ❌ Service account keys in client code
- ❌ No restrictions on the API key

---

*The key is public. The rules are the security.*

**Epilogue:** After writing this post, I deleted the feature entirely. No waitlist form, no Firebase SDK, no API key to defend. The scanners went quiet. Even when you *can* defend something as secure, ask if you need it at all. Sometimes the only winning move is not to play.
