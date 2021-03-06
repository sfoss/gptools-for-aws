

# gptools-for-aws

GP stands for [Geoprocessing](http://resources.arcgis.com/en/help/main/10.1/index.html#//002s00000001000000), and Geoprocessing is for everyone that uses [Esri's ArcGIS](http://www.esri.com/software/arcgis). The fundamental purpose of geoprocessing is to provide tools and a framework for performing analysis and managing your geographic data. ArcGIS provides a large suite of tools for performing GIS tasks that range from simple buffers and polygon overlays to complex regression analysis and image classification. Tools can be chained together feeding the output of one tool into another. You can also create custom tools and leverage them within this framework, such as the GP tools for AWS provided here in this project. 

The GP Tools for AWS are built to enable ArcGIS users to leverage Amazon Web Services through GP tools. This project leverages Elastic Map Reduce (EMR) the hosted hadoop framework, as well as, Simple Storage Service (S3) in AWS. In addition, the project leverages [GIS Tools for Hadoop](http://esri.github.io/gis-tools-for-hadoop/) to geo-enable hadoop.

## Features
* Start an EMR Cluster in AWS
* Execute a hive query language script on the EMR Cluster in AWS
* Get status on an EMR Cluster in AWS
* Terminate an EMR Cluster in AWS
* Upload a file to S3
* Download a file from S3


## Instructions

1. Fork and then clone the repo. 
2. Follow setup instructions
3. Run and try the samples.

Setup Instructions

The following steps describe the setup and configuration needed. It only needs to be done once, and then you’re ready to use the GIS Hadoop on EMR GP Tools anytime you want.

<h3>
<a name="installboto" class="anchor" href="#installboto"/>
1. Installing boto</h3>

boto is the python package that is used to communicate with Amazon Web Services. It should be added first before the GP tools are used. This can easily be done using:
<br>
> pip install boto
<br>
For windows users to easily leverage pip for adding a python package, win-pip can be useful. You can download it from this link:
https://sites.google.com/site/pydatalog/python/pip-for-windows

Once win-pip is downloaded, it will need to be pointed to the Python interpreter used by ArcGIS Desktop. The default install location added by the ArcGIS install is C:\Python27\<ArcGIS version>\python.exe
If you have several directories under Python27, make sure to use the 32 bit directory, not the 64 bit. Also, make sure to use the directory that has the right version of Desktop that you’re using.

Win-pip will add the pip module if it’s not already there.

To add boto, type: pip install boto, then click Run.
<p>

<img src="/images/win-pip.png" alt="win-pip"  width="320"> 


<h3>
<a name="gptoolssetup" class="anchor" href="#gptoolssetup"/>
2. Adding the GP Tools For AWS</h3>

Download the zip file, and unzip it under a location on your disk. Add this folder as a new folder connection in your Catalog from ArcGIS Desktop. 

That’s it!

You’re now ready to use the GIS Hadoop on EMR GP Tools.


<h3>
<a name="gptoolsconfig" class="anchor" href="#gptoolsconfig"/>
3. Getting your Credentials ready to access your AWS account
</h3>

You will need to get the following parameters from your AWS account ready before using the tools.

A. To be able to execute the tools you will need an AWS account. Make sure you have your account credentials ready to add into the tools.

You can get your credentials by going to:

http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html
you will find it under Access keys (access key ID and secret access key)


B. Also, you will need a key-pair. This is a certificate to encrypt your account information when you try to access your instances over the Internet. 

You can get a key-pair created and downloaded through the following steps:

http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html
you will find it under Key Pairs

<br>

That's all you need to start running hadoop jobs in AWS using EMR from ArcGIS Desktop!

<br>

<h3>
<a name="howitworks" class="anchor" href="#works"/>
How it all works
</h3>

* Upload your data and hive script that points to it in S3. There are many ways to upload your data to S3, for convenience the gp tools for aws include an S3 upload tool that can upload small S3 files.

<img src="/images/s3upload.png" alt="s3upload"  width="320"> 

* Start an EMR cluster, this cluster will include Hadoop, hive, and the jar files for GIS Tools for hadoop that geo-enable hadoop.

<img src="/images/emrsetup.png" alt="emrsetup"  width="320"> 

* Run a hive query using a script hql file. If the script generates results, you can utilize the output parameter to define the output directory in S3.

<img src="/images/runhivequery.png" alt="runhivequery"  width="320"> 

* Terminate your cluster after your query executes, if you don't have other use for it.

<img src="/images/emrterminate.png" alt="emrterminate"  width="320"> 

* Your input and output data will still be safe in S3 after the EMR cluster is terminated. You can download your output at any time using any S3 tools, for convenience the gp tools for aws include an S3 download tool that can download small S3 files.

<img src="/images/s3download.png" alt="s3download"  width="320"> 

For convenience to store your credentials in the tools, right click and choose edit. You can edit the python file in any file editor, then copy and paste your credentials for each tool between the "" in the right key value. For example, param1.value is the right key for the AWS Access Key ID in the EMR Setup tool: 

        param1 = arcpy.Parameter(

            displayName="AWS Access Key ID",

            name="awskey",

            datatype="String",

            parameterType="Required",

            direction="Input")

        param1.value = ""


<h3>
<a name="notes" class="anchor" href="#notes"/>
Useful notes for EMR users 
</h3>

* EMR by default uses S3 instead of HDFS. Uploading data to S3, means they don't need to be loaded again in HDFS, unless you're running recrussive operations on the same dataset that would be more efficient to have in HDFS.

* It's strongly recommended to use External tables. If you create a table pointing to the data in S3, and then drop that table, data in S3 will be DELETED. 

* When using output directories in S3 to store the results of the hive queries, don't pre-create those S3 directories, have EMR create them. Just type the name where you want the output to go, and make sure it's not a name that already exists in that S3 path.

<br>

<h3>
<a name="ssh" class="anchor" href="#ssh"/>
4. How to access your instances from windows using SSH (Optional)
</h3>

In case you need to access your cluster directly, you can SSH to the master node of the cluster using the following steps.
This can be done using PuttyGen and Putty.

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-connect-to-instance-linux.html#using-putty

login using "hadoop", and type "hive" to start a hive session.

<br>



## Requirements

* ArcGIS Desktop
* AWS Account


## Resources
* [Hive Query Language](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)
* [AWS EMR Documentation](http://aws.amazon.com/elasticmapreduce/)
* [Mapping in ArcGIS Desktop] (http://resources.arcgis.com/en/help/main/10.2/index.html#//00qn0000001p000000)


## Issues

Find a bug or want to request a new feature?  Please let us know by submitting an issue.

## Contributing

Esri welcomes contributions from anyone and everyone. Please see our [guidelines for contributing](https://github.com/esri/contributing).

## Licensing
Copyright 2013 Esri

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

A copy of the license is available in the repository's [license.txt]( https://raw.github.com/Esri/quickstart-map-js/master/license.txt) file.

[](Esri Tags: Geoprocessing)
[](Esri Language: python)​