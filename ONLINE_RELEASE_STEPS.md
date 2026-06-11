# Online Release Steps

This checklist prepares the personal academic website in this folder for release at `zeqianyu.com` using GitHub Pages.

## Current Local Status

- Website folder: `/Users/zeqianyu/Documents/Personal Website`
- GitHub account: `ninjazack`
- Recommended repository name: `ninjazack.github.io`
- Custom domain file: `CNAME` already contains `zeqianyu.com`
- Entry file: `index.html` is already at the repository root
- Static build: no framework or build step is required
- GitHub Pages compatibility: `.nojekyll` is present so GitHub Pages serves the static files directly
- Local-only original photo: `assets/profile_picture_original.PNG` is ignored and should not be published
- Public portrait asset: `assets/profile_picture.jpg`
- CV asset: `files/CV_Zeqian_Yu.pdf`

## 1. Final Local Review

Open the site locally and check these pages:

```bash
cd "/Users/zeqianyu/Documents/Personal Website"
python3 -m http.server 8765
```

Then visit:

- `http://127.0.0.1:8765/index.html`
- `http://127.0.0.1:8765/research.html`
- `http://127.0.0.1:8765/cv.html`

Stop the server with `Ctrl-C` after review.

## 2. Create the GitHub Repository

On GitHub, create a new public repository under `ninjazack` named:

```text
ninjazack.github.io
```

GitHub's own Pages documentation says a user site repository should be named `<user>.github.io`, so this repository name is the cleanest choice for your account.

Official reference: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

## 3. Commit and Push the Website

From the website folder:

```bash
cd "/Users/zeqianyu/Documents/Personal Website"
git status
```

Stage only the release files:

```bash
git add .gitignore .nojekyll CNAME README.md ONLINE_RELEASE_STEPS.md
git add index.html research.html cv.html styles.css
git add assets/favicon.svg assets/profile_picture.jpg
git add files/CV_Zeqian_Yu.pdf
```

Do not stage `assets/profile_picture_original.PNG`; it is intentionally local-only.

Commit:

```bash
git commit -m "Launch personal academic website"
```

Connect the local repository to GitHub:

```bash
git remote add origin https://github.com/ninjazack/ninjazack.github.io.git
git push -u origin main
```

If `origin` already exists, check it first:

```bash
git remote -v
```

## 4. Enable GitHub Pages

In the GitHub repository:

1. Open `Settings`.
2. Go to `Pages`.
3. Set source to `Deploy from a branch`.
4. Select branch `main` and folder `/root`.
5. Save.
6. Confirm the site appears at `https://ninjazack.github.io`.

GitHub says Pages looks for an `index.html`, `index.md`, or `README.md` at the publishing root; this site already has `index.html` in the root.

## 5. Add the Custom Domain

In GitHub repository settings:

1. Go to `Settings` -> `Pages`.
2. Under `Custom domain`, enter:

```text
zeqianyu.com
```

3. Save.
4. Keep the local `CNAME` file as-is.

Official reference: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site

## 6. Configure DNS at the Domain Provider

For the apex domain `zeqianyu.com`, add these `A` records:

```text
@    A    185.199.108.153
@    A    185.199.109.153
@    A    185.199.110.153
@    A    185.199.111.153
```

Optional IPv6 `AAAA` records:

```text
@    AAAA    2606:50c0:8000::153
@    AAAA    2606:50c0:8001::153
@    AAAA    2606:50c0:8002::153
@    AAAA    2606:50c0:8003::153
```

For `www.zeqianyu.com`, add this `CNAME` record:

```text
www    CNAME    ninjazack.github.io
```

Remove conflicting default records if the registrar created any for `@` or `www`.

## 7. Verify DNS and HTTPS

DNS can take up to 24 hours to propagate. After DNS updates, check:

```bash
dig zeqianyu.com +noall +answer -t A
dig www.zeqianyu.com +nostats +nocomments +nocmd
```

When GitHub Pages allows it, enable `Enforce HTTPS` in `Settings` -> `Pages`.

## 8. Final Public Check

After DNS and HTTPS are ready, visit:

- `https://zeqianyu.com`
- `https://www.zeqianyu.com`
- `https://zeqianyu.com/research.html`
- `https://zeqianyu.com/cv.html`

Confirm that:

- The portrait loads.
- The Research page has the updated master thesis proposal text.
- The CV download works.
- The email link opens a mail client.
- The browser shows HTTPS.
