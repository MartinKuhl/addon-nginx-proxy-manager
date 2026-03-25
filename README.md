# Home Assistant Community Add-on: Nginx Proxy Manager (Forked)

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
[![License][license-shield]](LICENSE.md)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

[![Github Actions][github-actions-shield]][github-actions]
![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

Manage Nginx proxy hosts with a simple, powerful interface.

![The Nginx Proxy Manager add-on](images/screenshot.gif)

## About

This add-on enables you to easily forward incoming connections to anywhere,
including free SSL, without having to know too much about Nginx
or Let’s Encrypt.

Forward your domain to your Home Assistant, add-ons, or websites running
at home or anywhere else, straight from a simple, powerful interface.

Want to protect the website with a username/password? Well, it can do that too!
Enable authentication and create a list of usernames/password that can access
that specific application.

For the power users, you can customize the behavior of each host in the
Nginx proxy manager by providing additional Nginx directives.

[:books: Read the full add-on documentation][docs]

## Authors & contributors

The original setup of this repository is by [Franck Nijhof][frenck].

## License

MIT License

Copyright (c) 2019-2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[commits-shield]: https://img.shields.io/github/commit-activity/y/MartinKuhl/addon-nginx-proxy-manager.svg
[commits]: https://github.com/MartinKuhl/addon-nginx-proxy-manager/commits/main
[docs]: https://github.com/MartinKuhl/addon-nginx-proxy-manager/blob/main/proxy-manager/DOCS.md
[github-actions-shield]: https://github.com/MartinKuhl/addon-nginx-proxy-manager/workflows/CI/badge.svg
[github-actions]: https://github.com/MartinKuhl/addon-nginx-proxy-manager/actions
[issue]: https://github.com/MartinKuhl/addon-nginx-proxy-manager/issues
[license-shield]: https://img.shields.io/github/license/MartinKuhl/addon-nginx-proxy-manager.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2026.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-experimental-yellow.svg
[releases-shield]: https://img.shields.io/github/release/MartinKuhl/addon-nginx-proxy-manager.svg
[releases]: https://github.com/MartinKuhl/addon-nginx-proxy-manager/releases
[repository]: https://github.com/MartinKuhl/ha-repository
[frenck]: https://github.com/frenck