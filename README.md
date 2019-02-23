# thatsamguy.com Website

* Built using [Zola](https://www.getzola.org/)

## Building

1. Install Zola [from here](https://www.getzola.org/documentation/getting-started/installation/)
2. Checkout this repo
3. ```$ cd website;```
4. ```$ zola build;```
5. Upload to S3

    ```    $ aws s3 sync public/ s3://aws-website-thatsamguy-00trx/ --exclude=.git*```