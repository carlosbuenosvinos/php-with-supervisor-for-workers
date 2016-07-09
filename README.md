# Docker Image with PHP and Supervisor for running Workers

PHP is not the best language for long running processes. Workers get into zombie state or they suddenly exit due to memory leaks. One trick to solve this is to execute your workers using a UNIX-like operating systems process monitor and control tool such as Supervisor (http://supervisord.org/).

Supervisor takes a list of commands, executes them and takes care if they are running or not.

The main idea of this approach is to have your workers listening for a certain amount of messages to process, for example, 100 messages. When this threshold is reached, then your worker exists. When Supervisor realizes that the worker command in not running, it spawns a new command.

## How to use this image

This image is configured with a volume at `/usr/src/app` to hold your application source code. It is also configured with another volume at `/etc/supervisor/conf.d` to hold the configuration files. Use that path if you would like to set up your worker commands.

This image includes `EXPOSE 9001` so you can access the web monitoring page running on such port.

As an example, you can define the command you want to execute, the minimum amount of time to consider that the process is working properly and if has to restart after finished.

    [program:my-general-worker]
    command=php /usr/src/app/bin/console worker:start my_queue 100
    startsecs=0
    autorestart=1