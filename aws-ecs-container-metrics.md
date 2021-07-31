# AWS ECS Container Metrics Receiver

## Test with local JSON payload
This receiver reads task metadata and resource usage stats from ECS Task Metadata Endpoint. However, for local testing, we can use test payload which are available in the code directory. We can modify the `rest_client.go` file to use our local json payload instead of calling the real ECS Task Metadata Endpoint. 

**Modify the EndpointResponse() function in rest_client.go file**

```go
// EndpointResponse gets the task metadata and docker stats from ECS Task Metadata Endpoint
func (c *HTTPRestClient) EndpointResponse() ([]byte, []byte, error) {
	// taskStats, err := c.client.Get(TaskStatsPath)
	taskStats, err := ioutil.ReadFile("/Users/hrayhan/Work/opentelemetry-collector-contrib/receiver/awsecscontainermetricsreceiver/testdata/task_stats.json")
	if err != nil {
		return nil, nil, err
	}
	// taskMetadata, err := c.client.Get(TaskMetadataPath)
	taskMetadata, err := ioutil.ReadFile("/Users/hrayhan/Work/opentelemetry-collector-contrib/receiver/awsecscontainermetricsreceiver/testdata/task_metadata.json")
	if err != nil {
		return nil, nil, err
	}
	return taskStats, taskMetadata, nil
}
```

