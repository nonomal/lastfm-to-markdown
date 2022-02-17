# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><img src="https://lastfm.freetls.fastly.net/i/u/64s/76bb74387e8216957b55a055cb313057.jpg" title="Health - Death Magic"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/665b408d29854a0694ee94dac09ca608.png" title="Health - DISCO2"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/c290962bc7ef6affc18b740506a5864c.jpg" title="Health - VOL. 4 :: SLAVES OF FEAR"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/7bc252f3055a4bf68088a0125cdaeb72.jpg" title="Health - DISCO"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/2f43d3824f838ad6a77ee0305efb08b8.jpg" title="Health - DISCO3"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/8b19db8796a6315c99ea70c5c14e8b1a.jpg" title="Health - HEALTH"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/c37e39ab2b25c910327a80a5918503ab.jpg" title="Health - DISCO3+"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/794dd0d2aeb5eeb0c163c80eeca4f530.jpg" title="Health - GET COLOR"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/2a4c3316377e99c1dfa3e7fbe72da95f.jpg" title="Health - DISCO4 :: PART I"> </p>

          
## 👩🏽‍💻 What you'll need
* A README.md file.
* Last.fm API key
  * Fill [this form](https://www.last.fm/api/account/create) to instantly get one. Requires a last.fm account.
* Set up a GitHub Secret called ```LASTFM_API_KEY``` with the value given by last.fm.
* Also set up a ```LASTFM_USER``` GitHub Secret with the user you'll get the weekly charts for.
* Add a ```<!-- lastfm -->``` tag in your README.md file, with two blank lines below it. The album covers will be placed here.

## Instructions
To use this release, add a ```lastfm.yml``` workflow file to the ```.github/workflows``` folder in your repository with the following code:
```diff
name: lastfm-to-markdown

on:
  schedule:
    - cron: '2 0 * * *'
  workflow_dispatch:

jobs:
  lastfm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: lastfm to markdown
        uses: melipass/lastfm-to-markdown@v1.3
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         IMAGE_COUNT: 6 # Optional. Defaults to 10. Feel free to remove this line if you want.
      - name: commit changes
        continue-on-error: true
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add -A
          git commit -m "Updated last.fm's weekly chart" -a

      - name: push changes
        continue-on-error: true
        uses: ad-m/github-push-action@v0.6.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}\
          branch: main
```
The cron job is scheduled to run once a day because Last.fm's API updates weekly chart data daily at 00:00, it's useless to make more than 1 request per day because you'll get the same information back every time. You can manually run the workflow in case Last.fm's API was down at the time, going to the Actions tab in your repository.

## 🚧 To do
* Allow users to choose the image size for the album covers.
* Feel free to open an issue or send a pull request for anything you believe would be useful.
