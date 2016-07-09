# Docker Image with PHP and Supervisor for running Workers

PHP is not the best language for long running processes. Workers get into zombie state or they suddenly exit due to memory leaks. One trick to solve this is to execute your workers using a UNIX-like operating systems process monitor and control tool such as Supervisor (http://supervisord.org/).

You workers can listen for a certain amount messages to process. Then the worker finished. Supervisor checks that the worker is not running anymore and starts the worker again.

As an example, you can define the command you want to execute, the minimum amount of time to consider that the process is working properly and if has to restart after finished.

    [program:general-worker]
    command=php /var/app/bin/console worker:start my_queue 100
    startsecs=0
    autorestart=1
    
## How to use this image

This image is configured with a volume at `/etc/supervisor/conf.d` to hold the configuration files. Use that path if you would like to set up your worker commands.

This image includes `EXPOSE 9001` so you can access the web monitoring page running on such port.