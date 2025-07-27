
# 🔐 Laravel Sanctum SPA Auth with Bruno

Bruno is a powerful Postman alternative, and with this guide, you can test your Laravel Sanctum-authenticated SPA APIs just like you'd do in Postman.
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/logo.png">
</p>

---

## 🛠️ Setup Steps

### 1. Create Your Collection
Start by creating your first collection in Bruno.  
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/1_create_collection.png">
</p>

---

### 2. Add Environment Variables
Click the environment tab and add the following:
- `base_url = http://localhost:8000`
- `frontend_url = http://localhost:5173` (or wherever your SPA runs)
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/2.png">
</p>

---

### 3. Create Authentication Folder
Make a folder called `Authentication` (or name it however you like) to group your auth routes.
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/3.png">
</p>

---

### 4. Add Routes
Add:
- `GET /sanctum/csrf-cookie`
- `POST /login`
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/4-2.png">
</p>
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/4-1.png">
</p>



---

### 5. CSRF Cookie Response Script

In the **Post Response Script** of your CSRF route, add:

```js
const cookieHeader = res.headers['set-cookie'];
if (cookieHeader) {
  const xsrfToken = cookieHeader[0].match(/XSRF-TOKEN=([^;]+)/);
  bru.setEnvVar('xsrf_token', xsrfToken[1]);
}
```

🧠 **What this does:**
- Extracts the `XSRF-TOKEN` from the `Set-Cookie` header.
- Saves it as a Bruno environment variable called `xsrf_token`.
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/5.png">
</p>


---

### 6. Folder-wide Pre-Request Script

Click the **Authentication folder** → go to the **Script tab** → paste this:

```js
const xsrfToken = bru.getEnvVar('xsrf_token');
const frontendUrl = bru.getEnvVar('frontend_url');

if (xsrfToken) {
  req.headers['X-XSRF-TOKEN'] = decodeURIComponent(xsrfToken);
  req.headers.Referer = frontendUrl;
}
```

🧠 **What this does:**
- Adds the `X-XSRF-TOKEN` header to outgoing requests.
- Sets the `Referer` to your frontend URL, mimicking a browser request.
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/6.png">
</p>

---

### 7. Run Auth Flow
- First run the `GET /sanctum/csrf-cookie`.
- Then run the `POST /login` with credentials.
- You can now access protected endpoints like `/api/user`.

---

### 8. 🔐 Authenticated Profile Test

Example response from `/api/user`:

```json
{
  "status": "success",
  "message": "Profile retrieved successfully",
  "data": {
    "id": 1,
    "first_name": "John Broom",
    "last_name": "Doed",
    "email": "test@example.com",
    "email_verified_at": "2025-06-22T02:00:13.000000Z",
    "phone": "09123456789",
    "address": "Australia, Melbourne",
    "created_at": "2025-06-22T02:00:14.000000Z",
    "updated_at": "2025-06-28T04:28:01.000000Z"
  }
}
```
<p align="center">
  <img src="https://www.maikeru-desu.quest/sanctum-bruno/8.png">
</p>


---

### ⭐️ Like this setup?
If you found this useful, consider giving it a ⭐️ on GitHub.

---

### 🏷 GitHub Tags (Topics)

```
laravel
sanctum
bruno
api-client
csrf-token
spa-authentication
laravel-sanctum
api-testing
laravel-api
laravel-authentication
bruno-client
csrf-protection
laravel-spa
```
