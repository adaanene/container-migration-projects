
<!-- ### DESIGNING A DOCKERFILE FOR THE TODO APP (not to include in documentation just for learning)

I encountered different issues with this task

1. I chose to use a prebuilt image of Ngnix so I could access my app on the browser. This was my initial Dockerfile

    ![initail-dockerfile-todo](./screenshots/initial_dockerfile_todo.png)

    1. Error1

        ![error-1](./screenshots/error-1.png)

        I Did not include the location of the app files. Changed COPY src /var/www/html to COPY . /var/www/html because my application files are in the same directory as the Dockerfile. 

    2. Error2

        ![error-2](./screenshots/error-1.1.png)

        I spelled the nginx.conf file wrong so Docker could not find it. After amending it I noticed that the icon next to teh file name changed

        ![nginx-wrong-spelling](./screenshots/nginx-wrong-spelling.png)

        ![nginx-right-spelling](./screenshots/nginx-right-spelling.png)

    3. Error-3

        ![error-3](./screenshots/error-1.2.png)

        - The error "E: Package 'php7.4-fpm' has no installation candidate" suggests that the php7.4-fpm package is not available in the repositories accessible to your system's package manager.

        In the Dockerfile, I was using the nginx:latest base image, which is based on Debian. However, Debian-based images might use a different naming convention for PHP packages.

        In Debian-based images, you can use the php-fpm package without specifying the version.

        - the other errors were suggesting difficulty in locating the packages, as I did not specify the && after teh backslashes

    4. Error-4

        this was my Dockerfile following teh chnages I made after having the errors mentioned earlier 

        ````
        FROM nginx:latest

        WORKDIR /app

        RUN apt-get update \
            && apt-get install -y php-fpm \
            && docker-php-ext-install mysqli \
            && git clone https://github.com/dareyio/php-todo.git

        # Copy Nginx server block configuration
        COPY nginx.conf /etc/nginx/conf.d/default.conf

        # Copy your PHP application code
        COPY . /var/www/html

        # Expose port 80
        EXPOSE 3000

        # Start Nginx and PHP-FPM
        CMD ["nginx", "-g", "daemon off;"]
        ````

        But I had this issue:

        ![error-4](./screenshots/error-2.png)

        "It appears that the command docker-php-ext-install is not found, which indicates that the PHP extension installation tool is not available in your current environment. This tool is typically included in official PHP images.

        To resolve this, you can use the docker-php-ext-install command inside a PHP-based image. Consider changing your base image to one that includes PHP and provides the necessary tools. "

        So I made changes to my Dockerfile to use a php based image instead of Nginx image, and added commands to install Ngnix

        ![dockerfile-with-php-image](./screenshots/dockerfile-php-image.png)

    5. Error-5

        ![error-git-not-found](./screenshots/error-3.png)

        The git command was not in the system so I had to include a command to install it

        I added the `RUN apt-get install git -y` to my Dockerfile

        ![final-dockerfile-todo](./screenshots/final-dockerfile-todo.png) -->





#### for webhooksjenkins server

Webhook needs a public ip address, so first create a network in docker and assign it a public subnet, then run the container with a public ip within that subnet and forward port 8080 on your local machine to port 8080 on the container, which is where jenkins is running.
`docker network create --subnet=171.20.0.0/24 jenkins`
`docker run -p 8080:8080 --net jenkins --ip 171.20.0.10 --name test-jenkins jenkins/jenkins`