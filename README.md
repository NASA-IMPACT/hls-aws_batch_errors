We ran 3 tests processing the same 1000 files 

The first test had the task's `memory` configured as `16000` using `r5d.2xlarge` instance types which with the Docker memory reservation results in 3 jobs being placed per container.

The second and third tests both had `memory` configured as `15000` using which results in 4 jobs being placed per container.

The first test resulted in a single [failure](Jul_24_S30_error_details.csv) of the 1000 files processed.

The second test resulted in 41 [failures](Jul_28_S30_error_details.csv)

The third test resulted in 44 [failures](Jul_31_S30_error_details.csv)

The errors all occur when an intermediate output file is being written to disk by our scientific C code.  Below is a [snippet](log_snippet.csv) of the error.  This error (with a few exceptions for the output band number is the same for all of the errors.

Most notable is the absence of errors with only 3 jobs per container and that the files causing the error are not consistent between the test runs.  This suggest some type of infrastructure failure.

Since the errors are associated with writing output I assumed there may be a scratch disk related problem.  We configure the scratch disk mount in the case of this instance type using the instance store.  You can view the configuration used [here](userdata.txt)

The Cloudwatch agent is running on all of our instances so if we view relevant instance metrics for the first failure in this [list]((Jul_31_S30_error_details.csv) we can see that we are not close to exceeding instance store space and that our instance memory utilization is below 50 percent.  The graphs are available [here](https://us-west-2.console.aws.amazon.com/cloudwatch/home?region=us-west-2#metricsV2:graph=~(metrics~(~(~'AWS*2fECS~'CPUUtilization~'ClusterName~'BatchComputeEnvironment-ed59a8d07adb417_Batch_3ef677e8-a1c6-3936-9345-e383995e7827~(visible~false))~(~'CWAgent~'disk_used_percent~'path~'*2f~'host~'ip-10-1-68-54.us-west-2.compute.internal~'device~'nvme0n1p1~'fstype~'ext4)~(~'...~'*2fscratch~'.~'.~'.~'nvme1n1~'.~'.)~(~'.~'mem_used_percent~'host~'ip-10-1-68-54.us-west-2.compute.internal))~period~300~stat~'Maximum~region~'us-west-2~start~'2020-07-31T20*3a00*3a00.999Z~end~'2020-07-31T21*3a00*3a00.000Z~view~'timeSeries~stacked~false);query=~'*7bCWAgent*2cdevice*2cfstype*2chost*2cpath*7d*20ip-10-1-68-54.us-west-2.compute.internal)
