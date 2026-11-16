# Gmail Campaign Sender — বাংলা গাইড

এটি একটি PWA starter app; HTTPS হোস্টিং হলে Android Chrome থেকে Install/Add to Home Screen করা যাবে। এটি সরাসরি APK নয়।

## যা আছে
- Admin Password login
- Google OAuth দিয়ে নিজের Gmail sender সংযোগ
- একাধিক email paste করা বা CSV import
- একটি subject/message দিয়ে Campaign পাঠানো
- ৫০টি করে batch, progress bar, sent/failed/skipped ফলাফল
- Unsubscribe endpoint এবং suppression list
- OAuth token AES-256-GCM encryption

## সেটআপ
1. Google Cloud Console-এ project তৈরি করে Gmail API enable করুন।
2. OAuth consent screen তৈরি করুন; OAuth Client ID (Web application) নিন।
3. Authorized redirect URI: `https://YOUR-APP.onrender.com/oauth2callback`
4. ZIP extract করে GitHub repository-তে সব ফাইল upload করুন।
5. Render-এ New Web Service → repository নির্বাচন।
6. Build: `npm install`; Start: `npm start`
7. Render Environment Variables:
   - `BASE_URL`: Render HTTPS URL
   - `GOOGLE_CLIENT_ID`: OAuth Client ID
   - `GOOGLE_CLIENT_SECRET`: OAuth Client Secret
   - `GOOGLE_REDIRECT_URI`: `https://YOUR-APP.onrender.com/oauth2callback`
   - `ADMIN_PASSWORD`: নিজের ১২+ অক্ষরের গোপন পাসওয়ার্ড
   - `APP_SECRET`: ৬৪ hex অক্ষরের secret; terminal-এ `openssl rand -hex 32` দিয়ে তৈরি করুন
8. Render persistent disk যোগ করে mount path `/opt/render/project/src/data` দিন, যেন token ও unsubscribe list deploy-এর পর থাকে।
9. Google OAuth consent screen-এ Testing থাকলে আপনার sender Gmail-কে Test User হিসেবে যোগ করুন।
10. App URL খুলে Admin Password দিয়ে login → Connect Sender Gmail → Google consent দিন।
11. CSV import বা email paste করুন; subject/message লিখুন; consent checkbox টিক দিয়ে Send Campaign চাপুন।

## সতর্কতা
শুধু স্পষ্ট সম্মতিপ্রাপ্ত গ্রাহকদের ইমেইল পাঠান। কেনা/স্ক্র্যাপ করা তালিকা বা unsolicited bulk email নয়। Gmail-এর account-specific daily quota ও rate limits মানতে হবে; ১০০০ মেইল পাঠানো নিশ্চিত নয়। Production-এর আগে database-backed campaign history, stronger admin login, rate limiting, audit logs, privacy policy, bounce handling ও unsubscribe compliance review যোগ করুন। CSV parser সহজ comma-separated format-এর জন্য; জটিল quoted CSV হলে Excel থেকে plain CSV export করুন।
