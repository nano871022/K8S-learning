# Liveness

It can check healhty of container, it is not answer in time set it destroy he container and create new one depend of settings

Create file [liveness.yaml](./liveness.yaml)

This setting allow check **/tmp/healthy** file, this file is configurate to create and 15 sec after it will be remove and create a sleep per 600sec.\
Meanwhile it check each 5sec if exist **/tmp/healthy** file, it has 1 failure how threshold. the first execution it take 15sec.
