## flask-celery-nginx-docker

Example docker-compose config for scaling celery worker with separate code base. It uses the classical addition task as an example. `flask-app` and `flask-celery` have seperate codebase (In other words we don't need to have access to the celery task module and don't need to import the celery task in the flask app) and `flask-app` uses the `name` attribute of a task and `celery.send_task` to submit a job without having the access to celery workers code base.

To run the example:
```bash

docker-compose build
docker-compose up -d # run in detached mode

```

Now load `http://your-dockermachine-ip:5000/add/2/3` in browser. It should create a task and return a task id.

To check the status of the job hit `http://your-dockermachine-ip:5000/check/taskid`. It should either show `PENDING` or the result `5`.

http://localhost:5000/ - Flask App root
http://localhost:5555/ - Flower (Celery management)
http://localhost:8081/ - Redis Commander

To monitor that the worker is working fine go to `http://your-dockermachine-ip:5555`.It runs a [flower](http://flower.readthedocs.org) server. It should show one worker ready to serve.

To scale the workers, now run `docker-compose scale worker=5`. This will create `4` more containers each running a worker. `http://your-dockermachine-ip:5555` should now show 5 workers waiting for some jobs!
