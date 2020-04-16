# RuboCop presentation

Made with [remark](https://github.com/gnab/remark) slideshow tool. Their [Demo](https://github.com/gnab/remark)

## How to use

You just need a web server to run it.

We have [multiple ways](https://gist.github.com/willurd/5720255) to achieve it. Here is 3 of them (*you need only one*):

```
# 1 - with Docker
docker run -d -p 8080:80 --name itechart-presentation -v "$PWD":/usr/local/apache2/htdocs/ httpd:2.4-alpine
# then for second run use: docker start itechart-presentation

# 2 - Python 2
python -m SimpleHTTPServer 8000

# 3 - Python 3
python3 -m http.server 8000
```

Your presentation lives in `presentation.md`. Please edit this file to add your content.

[Remark](https://remarkjs.com/) has very good [docs](https://github.com/gnab/remark/wiki)
