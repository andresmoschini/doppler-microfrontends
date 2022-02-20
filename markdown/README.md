# This is a boilerplate to kickstart with a remarkjs presentation

* Download this repository
* Download latest remarkjs

```sh
mkdir -p remark && \
wget https://gnab.github.io/remark/downloads/remark-latest.min.js -O remark/remark-latest.min.js
```

* Edit ```markdown/presentation.md```

* Start a [http server](https://gist.github.com/willurd/5720255) with this directory as DocumentRoot

  * Using npx you can run the script without installing it first:
  
      npx http-server .

* Open you browser and have a nice presentation
