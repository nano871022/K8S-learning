# Startupness - startupProbe

It can check healhty of container, it is not answer in time set it destroy he container and create new one depend of settings

Create file [startupness.yaml](./startupness.yaml)

This setting allow check **/tmp/healthy** file, this file is configurate to create and 10 sec after it will be remove and create a sleep per 600sec.\
Meanwhile it check each 5sec if exist **/tmp/healthy** file, it has 1 failure how threshold. the first execution it take 15sec. \
This case are checking for time set if it is deployed well, in other case it restart pod
