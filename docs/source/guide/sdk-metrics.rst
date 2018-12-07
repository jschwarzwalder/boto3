.. _guide_sdk-metrics:

SDK Metrics 
===========

AWS SDK Metrics for Enterprise Support (SDK Metrics) enables Enterprise customers to collect metrics from AWS SDKs on their hosts and clients shared with 
AWS Enterprise Support. SDK Metrics provides information that helps speed up detection and diagnosis of issues occurring in connections 
to AWS services for AWS Enterprise Support customers. 

As telemetry is collected on each host, it is relayed via UDP to localhost, where the CloudWatch Agent aggregates the data and sends it 
to the SDK Metrics service. Therefore, to receive metrics, the CloudWatch Agent is required to be added to your instance.

The following steps to set up SDK Metrics pertain to an Amazon EC2 instance running Amazon Linux for a client application that is using the AWS SDK for Python.
SDK Metrics is also available for your production environments if you enable it while configuring the AWS SDK for Python. 

To utilize SDK Metrics, run the latest version of the CloudWatch agent. Learn how to 
`Configure the CloudWatch Agent for SDK Metrics<https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-SDK-Metrics.html>`_ in the Amazon CloudWatch User Guide.

To set up SDK Metrics with the AWS SDK for Python, follow these instructions:

#. Create an application with an AWS SDK for Python client to use an AWS service.
#. Host your project on an Amazon EC2 instance or in your local environment.
#. Install and use the latest version of Boto3 (AWS SDK for Python).
#. Install and configure an CloudWatch agent on an EC2 instance or in your local environment.
#.  Authorize SDK Metrics to Collect and Send Metrics 
#. :ref:`csm-enable-agent`
#. :ref:`csm-view-metrics`


For more information, see the following:

* :ref:`csm-update-agent`
* :ref:`csm-disable-agent`


.. _csm-enable-agent:

Enable SDK Metrics
------------------

By default, SDK Metrics is turned off, and the port is set to 31000. The following are the default parameters.

.. code-block:: ini

    //default values
     [
         'enabled' => false,
         'port' => 31000,
         'client_id' => ''
     ]

Enabling SDK Metrics is independent of configuring your credentials to use an AWS service.

You can enable SDK Metrics by setting environment variables or by using the AWS Shared config file.

Set Environment Variables
~~~~~~~~~~~~~~~~~~~~~~~~~

The SDK first checks the profile specified in the environment variable under :code:`AWS_PROFILE` to determine if SDK Metrics is enabled.

To turn on SDK Metrics, add the following to your environmental variables.

.. code-block:: ini

    AWS_CSM_ENABLED=true

:ref:`Other configuration settings<csm-update-agent>` are available. 

Note: Enabling SDK Metrics does not configure your credentials to use an AWS service. 


AWS Shared Config File
~~~~~~~~~~~~~~~~~~~~~~

If no CSM configuration is found in the environment variables, the SDK looks for your customized AWS profile field. Then it checks the :code:`aws_csm` profile. To enable SDK Metrics, add :code:`csm_enabled` to the shared config file located at :file:`~/.aws/config`.

.. code-block:: ini

    [custom_profile_from_aws_profile]
    csm_enabled = true

    [aws_csm]
    csm_enabled = true

:ref:`Other configuration settings<csm-update-agent>` are available. 

Note: Enabling SDK Metrics does not configure your credentials to use an AWS service. 

.. _csm-view-metrics:

View Metrics in CloudWatch
--------------------------

For Enterprise users, the agent automatically captures data about each client operation and passes the information to CloudWatch.

.. topic:: To access your metrics

1. Go to the `CloudWatch console <http://console.aws.amazon.com/cloudwatch>`_.
2. In the navigation pane, choose :guilabel:`Metrics`.
3. Choose :guilabel:`SDKMetrics`.
4. View the metrics.

.. _csm-update-agent:

Update a CloudWatch Agent
-------------------------

To make changes to the port or client ID, you need to set the values and then restart any AWS jobs that are currently active.

Set Environment Variables
~~~~~~~~~~~~~~~~~~~~~~~~~

SDK Metrics assigns a client ID to your application environment that is a
searchable index point in the CloudWatch dashboard. To add a customized client ID string, add
`AWS_CSM_CLIENT_ID=[some_string]` to the host's environment variables.

Most services use
the default port. But if your service requires a unique port ID, add `AWS_CSM_PORT=[port_number]`, to the host's environment variables.

.. code-block:: ini

    AWS_CSM_ENABLED=true
    AWS_CSM_CLIENT_ID=myAppName
    AWS_CSM_PORT=1234


AWS Shared Config File
~~~~~~~~~~~~~~~~~~~~~~

SDK Metrics assigns a client ID to your application environment that is a
searchable index point in the CloudWatch dashboard.  To add a customized client ID string, add
`csm_client_id = [some_string]` to `~/.aws/config`.

Most services use the default port. But if your service requires a
unique port ID, add `csm_port = [port_number]` to `~/.aws/config`.

.. code-block:: ini

    [custom_profile_from_aws_profile]
    csm_enabled = false
    csm_client_id = myAppName
    csm_port = 1234

    [aws_csm]
    csm_enabled = false
    csm_client_id = myAppName
    csm_port = 1234

Restart SDK Metrics
~~~~~~~~~~~~~~~~~~~

To restart a job, run the following commands.

.. code-block:: ini

    amazon-cloudwatch-agent-ctl –a stop;
    amazon-cloudwatch-agent-ctl –a start;


.. _csm-disable-agent:

Disable SDK Metrics
--------------------

To turn off SDK Metrics, set `csm_enabled` to `false` in your environment variables, or in your AWS Shared config file located at :file:`~/.aws/config`.
Then restart your CloudWatch agent so that the changes can take effect.

**Environment Variables**

.. code-block:: ini

    AWS_CSM_ENABLED=false


**AWS Shared Config File**

.. note:: Environment variables override the AWS Shared config file. If SDK Metrics is enabled in the environment variables, the SDK Metrics remains enabled.

.. code-block:: ini

    [custom_profile_from_aws_profile]
    csm_enabled = false

    [aws_csm]
    csm_enabled = false

To disable SDK Metrics, use the following command. 

.. code-block:: ini

    sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a stop &&
    echo "Done"
    
If you are using other CloudWatch features, restart CloudWatch with the following command.

.. code-block:: ini

    amazon-cloudwatch-agent-ctl –a start;
    

Restart SDK Metrics
~~~~~~~~~~~~~~~~~~~

To restart a job, run the following commands.

.. code-block:: ini

    amazon-cloudwatch-agent-ctl –a stop;
    amazon-cloudwatch-agent-ctl –a start;


