## No Problem 
```
echo '{"@timestamp":"2019-02-04T23:09:52.884742296+0100","event":"message"}' >> log.json
```

`sudo docker-compose exec fluentd bash`
```
echo '{"log":"{\"@timestamp\":\"2019-02-04T23:09:52.884742296+0100\",\"event\":\"message\", \"foo\":\"bar\"}"}' | fluent-cat fromfluentcat
```

## Invalid date or strptime format
```
echo '{"@timestamp":"2019-02-04T23:09:52.8847422960100","event":"message"}' >> log.json
```

`sudo docker-compose exec fluentd bash`
```
echo '{"log":"{\"@timestamp\":\"2019-02-04T23:09:52.8847422960100\",\"event\":\"message\", \"foo\":\"bar\"}"}' | fluent-cat fromfluentcat
```