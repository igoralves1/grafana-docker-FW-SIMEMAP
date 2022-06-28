# Grafana Docker 

[Configure a Grafana Docker image
](https://grafana.com/docs/grafana/next/setup-grafana/configure-docker/#default-paths)

Create a persistent volume for your data in `/var/lib/grafana` (database and plugins):
```
docker volume create grafana-storage
```

Configure AWS credentials for CloudWatch Support:
```
docker run -d \
-p 3000:3000 \
--name=grafana \
-e "GF_AWS_PROFILES=default" \
-e "GF_AWS_default_ACCESS_KEY_ID=xxx" \
-e "GF_AWS_default_SECRET_ACCESS_KEY=xxx" \
-e "GF_AWS_default_REGION=us-east-2" \
grafana/grafana-oss
```
Grafana is available at: `0.0.0.0:3000`


To get all ENV variables in a running container: 
```
docker inspect -f '{{range $index, $value := .Config.Env}}{{$value}} {{end}}' a00bc4204fb6
```

To get access to the container:
```
docker exec -it a00bc4204fb6 bash
```

To get the information contained into the `grafana.ini`:
```
cat /etc/grafana/grafana.ini
```

Login credentials: can be found at `[security]` section of `/etc/grafana/grafana.ini` - line `240`:

- user: admin
- password: admin

After first login change the password to new password:

- user: admin
- password: xXxxxxxxx08!

## [Visualizing Data in Amazon Timestream using Grafana](https://www.youtube.com/watch?v=pilkz645cs4)

1. Go to `Configuration`
2. Selecione `Plugins`
3. At the plugin list type `Amazon Timestream`
4. Click `Signed` and `Install`
5. After instalation choose `create datasource`
6. Choose a name to the connection - Note: It can have many connections 
7. Choose `default region`, `database` and `table`
8. Test the connection

[Amazon Timestream Plugin](https://grafana.com/grafana/plugins/grafana-timestream-datasource/)

[Using Amazon Managed Grafana to query and visualize data from Amazon Timestream](https://www.youtube.com/watch?v=4oMbsLY28vc)


# Useful Docker cmds

```

docker ps -a
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

docker images
docker rmi $(docker images -a -q)

docker volume ls
docker volume rm grafana-storage
docker volume create grafana-storage
```