# create job

## create a Job which prints "Hello World"

````
kubectl create job hello --image=busybox:1.28 -- echo "Hello World"
````

## create a CronJob that prints "Hello World" every minute

````
kubectl create cronjob hello --image=busybox:1.28   --schedule="*/1 * * * *" -- echo "Hello World"
````
