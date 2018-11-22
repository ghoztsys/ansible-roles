# stackdriver

This role installs the [Stackdriver Logging agent](https://cloud.google.com/logging/docs/agent/installation) to the target hosts. While services such as Google App Engine and Google Container Engine already have the agent built-in, GCE instances do not and will need the agent installed in order to send logs to Google Stackdriver.