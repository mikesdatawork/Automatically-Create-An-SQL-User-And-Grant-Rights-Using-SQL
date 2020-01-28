![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Automatically Create An SQL User And Grant Rights Using SQL
**Post Date: June 12, 2014**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's a quick script that will create a login, and automatically add it to every database ( specified through a wild card of your choice ), and grant two roles read only, and deny writer.
The script does the following:
1. Checks to see if the User exists in the Master database. 2. Creates the User if it does NOT already exists.
3. Checks to see if the same user exists in each database. 4. Creates the User if it does NOT already exist
5. Adds the User to the roles db_datareader, and db_denydatawriter.
Note:
If the user already exists it will do nothing.
On with the script…
</p>      


## SQL-Logic
```SQL
use master;
set nocount on
 
if not exists( select name from syslogins where name = 'My_SQL_Login' ) begin
create login [LinkServ]
with
password = N'MyPassword' , default_database = [master] , check_expiration = off , check_policy = off end
 
declare @create_user_grant_rights varchar(max)
set @create_user_grant_rights = ''
select @create_user_grant_rights = @create_user_grant_rights + 'use [' + name + '];' + char(10) +
'if not exists ( select name from sysusers where name = ''My_SQL_Login'' )' + char(10) +
'begin' + char(10) +
'create user [My_SQL_Login] for login [My_SQL_Login]' + char(10) +
'exec sp_addrolemember N''db_datareader'', N''My_SQL_Login''' + char(10) +
'exec sp_addrolemember N''db_denydatawriter'', N''My_SQL_Login''' + char(10) +
'end' + char(10) from sys.databases where name like '%wildcard%'
exec (@create_user_grant_rights)
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

