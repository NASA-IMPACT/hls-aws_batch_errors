We ran 3 tests processing the same 1000 files 

The first test had the task's `memory` configured as `16000` using `r5d.2xlarge` instance types which with the Docker memory reservation results in 3 jobs being placed per container.

The second and third tests both had `memory` configured as `15000` using which results in 4 jobs being placed per container.

The first test resulted in a single [failure](Jul_24_S30_error_details.csv) of the 1000 files processed.

https://us-west-2.console.aws.amazon.com/cloudwatch/home?region=us-west-2#metricsV2:graph=~(metrics~(~(~'AWS*2fECS~'CPUUtilization~'ClusterName~'BatchComputeEnvironment-ed59a8d07adb417_Batch_3ef677e8-a1c6-3936-9345-e383995e7827~(visible~false))~(~'CWAgent~'disk_used_percent~'path~'*2f~'host~'ip-10-1-68-54.us-west-2.compute.internal~'device~'nvme0n1p1~'fstype~'ext4)~(~'...~'*2fscratch~'.~'.~'.~'nvme1n1~'.~'.)~(~'.~'mem_used_percent~'host~'ip-10-1-68-54.us-west-2.compute.internal))~period~300~stat~'Maximum~region~'us-west-2~start~'2020-07-31T20*3a00*3a00.999Z~end~'2020-07-31T21*3a00*3a00.000Z~view~'timeSeries~stacked~false);query=~'*7bCWAgent*2cdevice*2cfstype*2chost*2cpath*7d*20ip-10-1-68-54.us-west-2.compute.internal
