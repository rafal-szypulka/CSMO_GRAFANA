#Prepare perl runtime
Perl script described below collects data from the following data sources:

- Bluemix Clound Foundry API
- Bluemix Container API
- New Relic API
- pseudo-CMDB
 
Use the following steps to install prerequisite system packages and perl modules required for data collection script.


	sudo yum install perl-devel perl-CPAN gcc

There are meny methods of installing perl modules. We used `cpanm`.

Install `cpanminus` (_require internet connection_). Using command:
```sh
sudo curl -L http://cpanmin.us | perl - --sudo App::cpanminus
```
Install the following perl modules:

- Mojolicious::Lite 
- Data::Dumper 
- JSON 
- Text::ASCIITable 
- InfluxDB::LineProtocol 
- Hijk 
- HTML::Table 
- DBD::MySQL

using:

	cpanm Mojolicious::Lite Data::Dumper JSON Text::ASCIITable InfluxDB::LineProtocol Hijk HTML::Table DBD::MySQL


Execute data collection script to check if it starts correctly:

	./grafana.pl routes

The output should be similar to the following:


	[root@rscase case]# ./grafana_collect.pl routes
	/query             *    query
	/search            *    search
	/json              GET  json
	/nr_cmdb           GET  nr_cmdb
	/nr_mysql_cmdb     GET  nr_mysql_cmdb
	/nr_nginx_cmdb     GET  nr_nginx_cmdb
	/logmet_redirect   GET  logmet_redirect
	/container_status  GET  container_status
	/bmx_app_status    GET  bmx_app_status
	/noi_app_severity  GET  noi_app_severity
	/list              GET  list
	/html              GET  html

Edit the script `grafana_collect.pl` and change the following variables according to comments inside script:

```perl
############### Edit section below ###############################################################
my $api_key       = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'; # New relic API Key
my $bmx_space_guid =
  'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX';               # cf space cloudnative-dev --guid

my $noi_user     = "xxxx";                                 # Omnibus user
my $noi_password = "xxxxxxxxx";                            # Ominbus user password
my $noi_api =
  "http://${noi_user}:${noi_password}\@xxx.xxx.xxx.xxx:8080/objectserver/restapi/sql/factory/";
                                                           # insert Netcool Omnibus IP address above
my $bmx_username = 'xxxxxxxxxxxxxxxx';                     # Bluemix user id
my $bmx_password = 'xxxxxxxx';                             # Bluemix user password
my $influx_host  = 'localhost';                            # change is InfluxDB is installed remotely
my $influx_port  = '8086';                                 # change is Inlfux DB port is non-default
my $uid = "cmdb";                                          # MySQL user for CMDB database
my $pwd = 'cmdb';                                          # MySQL user password
##########################################################################################
```

