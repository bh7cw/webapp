# webapp
webapp running at port: 8080

secrets setting:
- prod or dev

webapp design logic:
- question with any answers cannot be deleted
- the categories won't be deleted if the question is deleted
- delete question will delete all the files attached to it
- delete answer will delete all the files attached to it

Instructions:
1. Prerequisites for building and deploying your application locally.
    - Install [Golang](https://golang.org/dl/)
    - Place the codebase in `GOPATH/src/`
    - Install the dependencies listed in go.mod(optional)
2. Build and Deploy instructions for web application.
    - Build: `go build`
    - Build for ubuntu: `env GOOS=linux GOARCH=amd64 go build`
    - Test: `go test ./...`
    - Run: `go run main.go`
3. JMeter do load test.
   - install [JMeter](https://jmeter.apache.org/)
   - run JMeter: `jmeter`
   - open the jmx file
   - run the load test

api spec:
- [hw2](https://app.swaggerhub.com/apis-docs/csye6225/fall2020-csye6225/assignment-02)
- [hw3](https://app.swaggerhub.com/apis-docs/csye6225/fall2020-csye6225/assignment-03)

Useful link:
- https://toolbox.googleapps.com/apps/dig/#A/
- https://www.whatsmydns.net/#NS/bh7cw.me
- https://docs.aws.amazon.com/codedeploy/latest/userguide/troubleshooting-deployments.html#troubleshooting-long-running-processes

Debug codedeploy:
- log: /opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log

Check port:
- lsof -i:8080
- sudo netstat -pna | grep 8080

Test statsD:
- `netcat -ulzp 8125`
- `echo "foo:1|c" | nc -u 127.0.0.1 8125`
- `echo -n 'some.metric.namespace:1|c' | nc -u -q0 localhost 8125`
- `while true; do curl http://localhost:8080/v1/users;sleep 1;done;`

port:
//https://blog.csdn.net/ws379374000/article/details/74218530
- `sudo netstat -ntulp |grep 8125`

Debug cloud watch:
//https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance-fleet.html#start-CloudWatch-Agent-EC2-fleet
- stop: `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a stop`

Debug code deploy:
//https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-view-logs.html
- `less /var/log/aws/codedeploy-agent/codedeploy-agent.log`

Changes from a2 to a3:
- Added more APIs
- Table `user` changes to `users`
- Endpoint get: `/v1/user/self` changes to `v1/userself`
- Endpoint get: `v1/user` changes to `v1/users`

a3 demo procedure:
- add two users, a and b
- add a question for a user
- add an answer for a user
- user b can't edit the answer
- user b can't delete the question
- user a can't delete the question due to answer exists
- user a delete the answer
- user a delete the question

Changes from a3 to a4:
- No development, just change category to unique

Changes from a4 to a5:
- add file, answer file, question file
- post file: upload the file to S3 and database
- delete file: delete the file from S3 and database

Changes from a5 to a6:
- use codedeploy to deploy webapp on ec2 instance

Changes from a6 to a7:
- add logs using logrus and metrics using statsD
- send to watch cloud show the logs and metrics

Changes from a7 to a8:
- add auto scaling instead of ec2
- add load-balancer
- add jmeter to do load test