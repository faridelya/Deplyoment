- Django Deployement on EC2 with Nginx and Gunicorn and controlling with Supervisor
- https://www.youtube.com/watch?v=vmMyuZSHt3c
> in case of simple Deployement of Flask and gunicorn you must face 502 bad gateway error so follow below trick for solution

 - Anyone who lands here from the Googles and is trying to run Flask on AWS using the default Ubuntu image after installing nginx and still can't figure out what the problem is:

Nginx runs as user "www-data" by default, but the most common Flask WSGI tutorial from Digital Ocean has you use the logged in user for the systemd service file. Change the user that nginx is running as from "www-data" (which is the default) to "ubuntu" in /etc/nginx/nginx.conf if your Flask/wsgi user is "ubuntu" and everything will start working. You can do this with one line in a script:

**sudo sed -i 's/user www-data;/user ubuntu;/' /etc/nginx/nginx.conf**
