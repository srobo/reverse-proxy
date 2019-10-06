# Student Robotics Website Reverse Proxy

[![Build status][build-badge]][build-page]

The reverse proxy which pulls together all the pieces of the Student Robotics
public website.

## Getting Started

1. [Install Docker][docker]

2. Fetch the latest base image:
    ``` shell
    $ docker pull nginx
    ```

3. Optionally change the bound ports so they don't conflict with your local host's
   configuration. For example, you might want to edit the [`Rakefile`](Rakefile)
   to remove `-p 80:80`.

4. Start the container

    ``` shell
    $ rake run
    ```

5. Visit <http://localhost> in a browser (adjust the port as needed if you
   edited it), and confirm that you see a copy of the SR website (also check that
   you didn't get redirect to the real one!)

6. Make your changes to the [`nginx.conf`](nginx.conf)

7. Get those changes into the container and reload nginx:
    ``` shell
    $ docker cp nginx.conf srobo:/etc/nginx/nginx.conf && docker kill -s HUP srobo
    ```

8. Refresh your browser and bask in the glory of your changes

## Making changes

When you've made a change, either push it to a forked repository, or to a
feature branch, and [raise a pull request][raise-a-pr].

## Deployment

***Note**: full [deployment instructions](./DEPLOYMENT.md) are separate.*

The `master` branch of repo is built into a Docker image by [Circle CI][circle-ci]
which is then deployed manually into a Kubernetes hosted on DigitalOcean.

[build-badge]: https://circleci.com/gh/srobo/reverse-proxy/tree/master.png?style=shield
[build-page]: https://circleci.com/gh/srobo/reverse-proxy/tree/master
[docker]: https://docker.com/
[raise-a-pr]: https://github.com/srobo/reverse-proxy/pull/new
[circle-ci]: https://circleci.com/gh/srobo/reverse-proxy
