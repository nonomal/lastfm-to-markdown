# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Nirvana/Nevermind"><img src="https://lastfm.freetls.fastly.net/i/u/64s/570021b68d3d9d2db08bc99a473303b0.jpg" title="Nirvana - Nevermind"></a> <a href="https://www.last.fm/music/Los+Tres/Los+Tres"><img src="https://lastfm.freetls.fastly.net/i/u/64s/b36ef0b75542919c650c8bdefbc1a6d6.jpg" title="Los Tres - Los Tres"></a> <a href="https://www.last.fm/music/Omar+Souleyman/Bahdeni+Nami"><img src="https://lastfm.freetls.fastly.net/i/u/64s/4716b8185603b2fc58d007d389bde9b1.jpg" title="Omar Souleyman - Bahdeni Nami"></a> <a href="https://www.last.fm/music/Omar+Souleyman/To+Syria,+With+Love"><img src="https://lastfm.freetls.fastly.net/i/u/64s/69fa6ab9b9499bb2b10885424c083c36.jpg" title="Omar Souleyman - To Syria, With Love"></a> <a href="https://www.last.fm/music/Omar+Souleyman/Shlon"><img src="https://lastfm.freetls.fastly.net/i/u/64s/2092babe3fab86dfd787add3aa1aa9e1.jpg" title="Omar Souleyman - Shlon"></a> <a href="https://www.last.fm/music/Asamblea+Internacional+del+Fuego/Columnas"><img src="https://lastfm.freetls.fastly.net/i/u/64s/5e2fb194180ba4e4e814db207cdcd6d9.jpg" title="Asamblea Internacional del Fuego - Columnas"></a> <a href="https://www.last.fm/music/Asamblea+Internacional+del+Fuego/La+Marcha+De+La+Desesperanza"><img src="https://lastfm.freetls.fastly.net/i/u/64s/ffd4bd59b18739fdff6eb4007ce10dc4.jpg" title="Asamblea Internacional del Fuego - La Marcha De La Desesperanza"></a> <a href="https://www.last.fm/music/Asamblea+Internacional+del+Fuego/Dial%C3%A9ctica+Negativa"><img src="https://lastfm.freetls.fastly.net/i/u/64s/d6f0fc84b072f074615e37f63cc86510.jpg" title="Asamblea Internacional del Fuego - Dialéctica Negativa"></a> <a href="https://www.last.fm/music/Nirvana/In+Utero"><img src="https://lastfm.freetls.fastly.net/i/u/64s/b897255bf422baa93a42536af293f9f8.jpg" title="Nirvana - In Utero"></a> <a href="https://www.last.fm/music/Asamblea+Internacional+del+Fuego/23+(Versi%C3%B3n+%22Fundamental%22)"><img src="https://lastfm.freetls.fastly.net/i/u/64s/51f225c4a051b5d20c0ea4a1652c480e.jpg" title="Asamblea Internacional del Fuego - 23 (Versión "Fundamental")"></a> </p>

          
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
