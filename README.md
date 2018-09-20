# Vagrant Saleor

Vagrant setup for running saleor ecommerce app on your local machine

## Getting Started

These instructions will get you a copy of the [Saleor](https://github.com/mirumee/saleor) and running on your local machine for development and testing purposes.

### Prerequisites

You need to install the following to use this vagrant setup

[VirtualBox](https://www.virtualbox.org/wiki/Downloads)
[Vagrant](https://www.vagrantup.com/downloads.html)
[Nodejs version 10+](https://nodejs.org/en/)

### Installing

Open a command prompt terminal and run

```
vagrant plugin install vagrant-hostmanager
```

Clone this git repository, go inside the folder and run 

```
vagrant up
```

It will take a several minutes depending upon speed of your internet connection. At the completion of the above command run

```
cd saleor
npm install
```

After npm finished installing node modules run

```
npm run build-assets
npm run build-emails
```

Now you are ready to launch saleor inside your browser at (http://saleor.vm:8000)

You can login to saleor dashboard at (http://saleor.vm:8000/dashboard/)

username: admin

password: admin

email: admin@example.com


## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Usama Ahmed** - *Initial work* -

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
