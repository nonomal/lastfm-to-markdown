# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Juana+Fe/Con+Los+Pies+En+El+Barrio"><img src="https://lastfm.freetls.fastly.net/i/u/64s/0231a0ac6e73425abdc3073edcce107b.jpg" title="Juana Fe - Con Los Pies En El Barrio"></a> <a href="https://www.last.fm/music/Slowdive/everything+is+alive"><img src="https://lastfm.freetls.fastly.net/i/u/64s/4f3dd53076a4760c6394e68a3ebe9683.jpg" title="Slowdive - everything is alive"></a> <a href="https://www.last.fm/music/Fiskales+Ad-Hok/Traga!"><img src="https://lastfm.freetls.fastly.net/i/u/64s/95ab7f6a4ff640b6c08d1fd090f04db3.png" title="Fiskales Ad-Hok - Traga!"></a> <a href="https://www.last.fm/music/Rage+Against+the+Machine/Rage+Against+the+Machine"><img src="https://lastfm.freetls.fastly.net/i/u/64s/8f25a0a061254740c74a40a4e16337d6.png" title="Rage Against the Machine - Rage Against the Machine"></a> <a href="https://www.last.fm/music/Asamblea+Internacional+del+Fuego/La+Marcha+De+La+Desesperanza"><img src="https://lastfm.freetls.fastly.net/i/u/64s/ffd4bd59b18739fdff6eb4007ce10dc4.jpg" title="Asamblea Internacional del Fuego - La Marcha De La Desesperanza"></a> <a href="https://www.last.fm/music/Dinosaur+Jr./Farm"><img src="https://lastfm.freetls.fastly.net/i/u/64s/8162d5b21cf4855f597ebc524adfca9f.jpg" title="Dinosaur Jr. - Farm"></a> <a href="https://www.last.fm/music/Dinosaur+Jr./Green+Mind"><img src="https://lastfm.freetls.fastly.net/i/u/64s/d3a8b1e9240a4a3f8a567806b3b04206.png" title="Dinosaur Jr. - Green Mind"></a> <a href="https://www.last.fm/music/Beck/Odelay"><img src="https://lastfm.freetls.fastly.net/i/u/64s/8381e54db1d4b669bb6baedc68180503.jpg" title="Beck - Odelay"></a> <a href="https://www.last.fm/music/Beck/Mellow+Gold"><img src="https://lastfm.freetls.fastly.net/i/u/64s/91a4ba354ee6db609b68222e7149a32e.jpg" title="Beck - Mellow Gold"></a> </p>

          
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
