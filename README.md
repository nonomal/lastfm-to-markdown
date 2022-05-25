# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Hole/Celebrity+Skin"><img src="https://lastfm.freetls.fastly.net/i/u/64s/9e8078410057daab5d1f2bc55c79a5be.png" title="Hole - Celebrity Skin"></a> <a href="https://www.last.fm/music/Hole/Live+Through+This"><img src="https://lastfm.freetls.fastly.net/i/u/64s/6373e00a55bdbd68fe24369d2d05b2ee.jpg" title="Hole - Live Through This"></a> <a href="https://www.last.fm/music/Hole/Pretty+On+The+Inside"><img src="https://lastfm.freetls.fastly.net/i/u/64s/0cc82c960f1ffb0c21922f23d6d233cd.png" title="Hole - Pretty On The Inside"></a> <a href="https://www.last.fm/music/Hiromi/Silver+Lining+Suite"><img src="https://lastfm.freetls.fastly.net/i/u/64s/26f14c78f97d67cdca2f5d17a20a50d3.png" title="Hiromi - Silver Lining Suite"></a> <a href="https://www.last.fm/music/The+Pooh+Sticks/The+Great+White+Wonder"><img src="https://lastfm.freetls.fastly.net/i/u/64s/fd7a9da0dde4b5ca4dc69395a908bce6.jpg" title="The Pooh Sticks - The Great White Wonder"></a> <a href="https://www.last.fm/music/Everyone+Asked+About+You/Everyone+Asked+About+You"><img src="https://lastfm.freetls.fastly.net/i/u/64s/249a8c32494ee395f54116533533755e.png" title="Everyone Asked About You - Everyone Asked About You"></a> <a href="https://www.last.fm/music/Fin+del+Mundo/fin+del+mundo"><img src="https://lastfm.freetls.fastly.net/i/u/64s/95064dbf21049600046590b999bc3d1a.jpg" title="Fin del Mundo - fin del mundo"></a> <a href="https://www.last.fm/music/Everyone+Asked+About+You/sometimes+memory+fails+me+sometimes"><img src="https://lastfm.freetls.fastly.net/i/u/64s/cb6c3feb89604115c12ffdc6d45c85f1.jpg" title="Everyone Asked About You - sometimes memory fails me sometimes"></a> <a href="https://www.last.fm/music/Everyone+Asked+About+You/The+Boston+To+Little+Rock+Connection+Split"><img src="https://lastfm.freetls.fastly.net/i/u/64s/e46b70a4b414833f0731a6b6a644f875.jpg" title="Everyone Asked About You - The Boston To Little Rock Connection Split"></a> <a href="https://www.last.fm/music/My+Chemical+Romance/The+Foundations+of+Decay+-+Single"><img src="https://lastfm.freetls.fastly.net/i/u/64s/269349eef88cb86a2c55407b3e77e710.jpg" title="My Chemical Romance - The Foundations of Decay - Single"></a> </p>

          
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
        uses: melipass/lastfm-to-markdown@v1.3.1
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         INCLUDE_LINK: true # Optional. Defaults is false. If you want to include the link to the album page, set this to true.
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
