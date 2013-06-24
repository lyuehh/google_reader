---
layout: post
title:  "检查MySQL数据库服务器的shell脚本"
date:   2011-09-23 17:06:02
author: Eugene
categories: program
---

## 检查MySQL数据库服务器的shell脚本
### by Eugene
### at 2011-09-23 17:06:02
### original <http://www.mysqlops.com/2011/09/23/shell.html>

<p align="left">【<strong>导读</strong>】</p>
<p align="left">某著名电子商务公司的同事，编写的shell脚本，用于获得数据库服务器的数据库性能和配置，以及服务器负载LOAD等信息。shell脚本较长，也对shell脚本做了部分修改，同时为使技术朋友们更容易理解和使用，添加相关的文字和图片描述作为手册。</p>
<p align="left">n  <strong>Shell</strong><strong>代码的描述</strong><strong></strong><strong></strong></p>
<p align="left"><strong>1.         </strong><strong>功能描述</strong><strong></strong></p>
<p align="left">执行shell命令：sh Get_Local_Kpi.sh –help，能显示相关信息，如图1-1：</p>
<p style="text-align:center"><a href="http://www.mysqlops.com/wp-content/uploads/2011/09/help.png"><img src="http://www.mysqlops.com/wp-content/uploads/2011/09/help.png" alt="" width="860" height="142"></a></p>
<p><span></span></p>
<p align="center"><strong>图</strong><strong>1-1</strong></p>
<p align="left">可以为脚本Get_local_skpi指定参数的方式，把指定结果输出到指定的文件，需要检查的VIP地址，检查某项特定的信息，例如：</p>
<p><a href="http://www.mysqlops.com/wp-content/uploads/2011/09/help_key.png"><img src="http://www.mysqlops.com/wp-content/uploads/2011/09/help_key.png" alt="" width="727" height="376"></a></p>
<p align="center"><strong>图</strong><strong>1-2</strong><strong></strong></p>
<p style="text-align:left" align="center"><strong>2.         </strong><strong>配置文件</strong><strong></strong></p>
<p align="left">Get_Local_Kpi.sh需要读取一个数据库访问的账号密码配置文件，则可能修改代码中的二个地方：</p>
<p align="left">(1).     密码配置文件存放的路径：CONF_DIR=/home/mysqldata/conf</p>
<p align="left">(2).     密码文件头部分：PASS_FILE=”$CONF_DIR”/.mysql_info.”$MY_PORT”</p>
<p align="left">(3).     脚本考虑了一台主机部署多个实例的生产环境，为此你只要执行的时候带上参数 –port=3306的格式即可，若是没有指定此参数则默认赋值为3306<strong></strong></p>
<p align="left"><strong>3.         </strong><strong>软件安装</strong><strong></strong></p>
<p align="left">Get_Local_Kpi.sh使用了iostat命令工具，若是服务器没有安装软件，则脚本程序会自动通过yum方式帮你安装，但是你的服务器没有配置yum源的话，则需要手工下载软件包：sysstat.x86_64，并且手工安装，软件rpm包下载地址：</p>
<p align="left">http://rpm.pbone.net/index.php3/stat/3/srodzaj/2/search/sysstat-7.0.2-3.el5.src.rpm<strong></strong></p>
<p align="left"><strong>4.         </strong><strong>脚本缺点及优点</strong><strong></strong></p>
<p align="left">脚本实现部分信息收集的功能，并且shell脚本函数化的方式编写，但是没有完全抽象起来，导致代码较长，对于一些没有条件的技术朋友们，可以借鉴，以及继续添加相关功能。</p>
<p align="left">n  <strong>Shell</strong><strong>代码</strong></p>
<p align="left">shell脚本的微盘下载地址：http://vdisk.weibo.com/s/Gszg</p>
<pre>#! /bin/bash
######################################
# Get MySQL option status with MySQL machine
# Create by <a title="由 steven 发布" href="http://www.mysqlops.com/author/steven" rel="author">steven</a>
# Created at : 2010.04.29 #
#ALTER BY Eugene
#ALTER TIME:2011-09-23
# The reslut of program will write to  $CONF_FILE
# Example :
#   rollbackcommit
#   dml
#   innodbio
#   qps
#   ioutil
#   iorwkb
#   slaveio
#   slavesql
#   slavelag
#   innodb_bufsize
#   myisam_keysize
#   trans_isolation
#   char_server
#   char_client
#   char_conn
#   sesscnt
#   session
#   load
#   role
######################################
###### Check parameters 

usage ()
{
cat &lt;&lt;EOF
Usage: $0 [OPTIONS]
  --port=3306                      MySQL Port ,Defalt 3306
  --outfile=/tmp/mysql3306.start   OutPut result to file
  --vip=10.2.334.252
  --key=qps,load,iorwkb...         What you want to Check. separated with &quot;,&quot;
          Key List: $ALL_KEY  

If no &quot;--ip&quot; specified,Program will get first ip in result of  IPCONFIG .
All other options are passed to the program.

EOF
exit 1

}

for Parms in $*
  do
    Pram=$1
    Val=`echo &quot;$Pram&quot; | sed -e &quot;s;--[^=]*=;;&quot;`

    case &quot;$Pram&quot; in
       --port=*)
         MY_PORT=$Val
       ;;
       --outfile=*)
         MY_OUTFILE=$Val
       ;;
       --vip=*)
         MY_VIP=$Val
       ;;
       --key=*)
         MY_KEY=$Val
       ;;
       *)
       usage
       exit 1
       ;;
    esac
    shift
done

#####  Variables Define -- Begin

[ -z ~/.bash_profile ] &amp;&amp; . ~/.bash_profile 

if [ -z &quot;$MY_PORT&quot; ] ; then
  MY_PORT=3306
fi

CONF_DIR=/home/mysqldata/conf
PASS_FILE=&quot;$CONF_DIR&quot;/.mysql_info_sa.&quot;$MY_PORT&quot;

if [ -f &quot;$PASS_FILE&quot; ] ; then
   . &quot;$PASS_FILE&quot;
else
  echo &quot;$PASS_FILE IS NOT EXISTS!&quot;
  exit
fi

MY_USER=&quot;$MYSQL_USER&quot;
MY_PASSWD=&quot;$MYSQL_PASSWORD&quot;
MY_SOCKET=&quot;$MYSQL_SOCK&quot;
#MY_HOST=
MY_DATABASE=
ALL_KEY=rollbackcommit,dml,innodbio,qps,ioutil,iorwkb,slaveio,slavesql,slavelag,innodb_bufsize,myisam_keysize,trans_isolation,char_server,char_client,char_conn,sesscnt,session,load,role,

MYADMIN=$(which mysqladmin)
MYSQL=$(which mysql)

[ -z &quot;$MY_PASSWD&quot; ] ||  MY_PASSWD=&quot;-p&quot;${MY_PASSWD} 

#####  Variables Define -- End

######################################### Funtions Begin

getstat_mysql()
{
# Get status of MySQL

if [ -z &quot;$MYSQL&quot; ] ; then
	if [ -x /usr/bin/mysql ] ; then
		MYSQL=/usr/bin/mysql
	else
	  echo &quot;no_cmd_mysql.&quot;
    return 1
  fi
fi

 #$MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -e &quot;EXIT&quot;  &gt; /dev/null

 $MYSQL -u$MY_USER  $MY_PASSWD -S $MY_SOCKET  -e &quot;EXIT&quot;  &gt; /dev/null

 if [ $? -ne 0 ]  ; then
        echo &quot;MySQL_error!&quot;
        return 2
 fi
return 0
}

getstat_mysqladmin()
{
# Get status of MySQL

if [ -z &quot;$MYADMIN&quot; ] ; then
	  echo &quot;no_cmd_mysqladmin.&quot;
    return 1
fi

return 0
}

getstat_Questions()
{
  # Get status of  Questions between 3 sec.
  getstat_mysql
  if [ $? -ne 0 ] ;then
   echo &quot;NO_MYSQL&quot;
   return 1
  fi

  local _var1=Questions

  if [ $VERSION_FLAG -eq 1 ] ; then
  	# Mysql version = 4.x
    #local _var1stat1=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;show status like &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
    local _var1stat1=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;show status like &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )

  else
    #local _var1stat1=$($MYSQL -u$MY_USER -h$MY_HOST -p$MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL STATUS LIKE  &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
    local _var1stat1=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL STATUS LIKE  &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
  fi

  sleep 3 

  if [ $VERSION_FLAG -eq 1 ] ; then
  	# Mysql version = 4.x
    #local _var1stat2=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;show status like &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
    local _var1stat2=$($MYSQL -u$MY_USER  $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;show status like &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
  else
    #local _var1stat2=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL STATUS LIKE  &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
    local _var1stat2=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL STATUS LIKE  &#39;Questions%&#39; &quot; | awk &#39;{print $2}&#39; )
  fi

  local _stat1=$(echo &quot;$_var1stat1,$_var1stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)    

  echo $_stat1

  return 0
}

getstat_slaveio()
{
# Get Slave_IO_thread status of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi

#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;show slave status \G&quot; | grep Slave_IO_Running | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;show slave status \G&quot; | grep Slave_IO_Running | awk {&#39;print $NF&#39;} )

if [ -z &quot;${_stat}&quot; ] ; then
  _stat=&quot;NULL&quot;
fi
echo $_stat
return 0
}

getstat_slavesql()
{
# Get Slave_SQL_thread status of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi

#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;show slave status \G&quot; | grep Slave_SQL_Running | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;show slave status \G&quot; | grep Slave_SQL_Running | awk {&#39;print $NF&#39;} )

if [ -z &quot;${_stat}&quot; ] ; then
  _stat=&quot;NULL&quot;
fi
echo $_stat
return 0
}

getstat_slavelag()
{
# Get Slave_lag of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi

#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;show slave status \G&quot; | grep Seconds_Behind_Master | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;show slave status \G&quot; | grep Seconds_Behind_Master | awk {&#39;print $NF&#39;} )

if [ -z &quot;${_stat}&quot; ] ; then
  _stat=&quot;NULL&quot;
fi
echo $_stat
return 0
}

getstat_innodb_bufsize()
{
# Get Innodb innodb_buffer_pool_size of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi
#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;innodb_buffer_pool_size&#39;&quot; | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;innodb_buffer_pool_size&#39;&quot; | awk {&#39;print $NF&#39;} )

echo $_stat
return 0
}

getstat_myisam_keysize()
{
# Get Myisam key_buffer_size of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi
#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;key_buffer_size&#39;&quot; | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;key_buffer_size&#39;&quot; | awk {&#39;print $NF&#39;} )

echo $_stat
return 0
}

getstat_trans_isolation()
{
# Get transaction_isolation of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi
#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;tx_isolation&#39;&quot; | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;tx_isolation&#39;&quot; | awk {&#39;print $NF&#39;} )

echo $_stat
return 0
}

getstat_char_server()
{
# Get Character set of server of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi
#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;character_set_server&#39;&quot; | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;character_set_server&#39;&quot; | awk {&#39;print $NF&#39;} )

echo $_stat
return 0
}

getstat_char_client()
{
# Get Character set of client of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi

#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;character_set_client&#39;&quot; | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;character_set_client&#39;&quot; | awk {&#39;print $NF&#39;} )

echo $_stat
return 0
}

getstat_char_conn()
{
# Get Character set of connection of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi
#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;character_set_client&#39;&quot; | awk {&#39;print $NF&#39;} )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW GLOBAL VARIABLES LIKE &#39;character_set_client&#39;&quot; | awk {&#39;print $NF&#39;} )

echo $_stat
return 0
}

getstat_sesscnt()
{
# Get all connection of MySQL
  getstat_mysql
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQL&quot;
    return 1
  fi
#local _stat=$($MYSQL -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT -N -s -e &quot;SHOW PROCESSLIST;&quot; | wc -l )
local _stat=$($MYSQL -u$MY_USER $MY_PASSWD -S $MY_SOCKET -N -s -e &quot;SHOW PROCESSLIST;&quot; | wc -l )

echo $_stat
return 0
}

getstat_session()
{
  # Get status of  active_session/total_session
  local _stat1
  local _stat2
  local _var
  local _result
  local _tmpfile=/tmp/stat_sesscnt_$$.tmp

  getstat_mysqladmin
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQLADMIN&quot;
    return 1
  fi

  #$MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT PROCESSLIST | grep &quot;^|&quot; | grep -v &quot;Command&quot;  &gt; $_tmpfile
  $MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET PROCESSLIST | grep &quot;^|&quot; | grep -v &quot;Command&quot;  &gt; $_tmpfile

  _var=active_session
  _stat1=$(cat $_tmpfile | grep -v &quot;Sleep&quot; | wc -l )

  _var=total_session
  _stat2=$(cat $_tmpfile | wc -l )

  echo ${_stat1},${_stat2} 

  \rm -f $_tmpfile
  return 0
}

getstat_load()
{
# Get load of Os
local _stat=$(w |  head -1 | awk -F &quot;:&quot; &#39;{print $NF}&#39; | tr -d &#39; &#39; )
echo ${_stat}
return 0
}

getstat_role()
{
#### Get Role : MASTER/SLAVE
# Please Think about :VIP , Qps , Slave_io=&quot;&quot; , number of processlist 

local REMOTE_SLAVE_IO=$(getstat_slaveio)
local REMOTE_SLAVE_SQL=$(getstat_slavesql)

if [ -z &quot;$MY_VIP&quot; ] ; then

	# There is no vip .

	if [  &quot;$REMOTE_SLAVE_IO&quot; = &quot;NULL&quot;  -a  &quot;$REMOTE_SLAVE_SQL&quot; = &quot;NULL&quot;  ] ; then

    #  There is no slave processs .
      echo &quot;MASTER&quot;	

  else

  	#  Running with slave process ,check processlist count &gt;= 10
  	local REMOTE_SESS_CNT=$(getstat_sesscnt)
  	  if [ $REMOTE_SESS_CNT -ge $MAX_PROC_CNT ] ; then
	      echo &quot;MASTER&quot;
	    else
	  	  echo &quot;SLAVE&quot;
	  	fi
  fi

else

	# check with a vip

    local VIP_CNT=$( /sbin/ifconfig | grep &quot;$MY_VIP &quot; | wc -l ) 

	  if [ $VIP_CNT	-ne 1 ] ; then
	    echo &quot;SLAVE&quot;
	  else
	  	echo &quot;MASTER&quot;
	  fi

fi

return 0

}

getstat_rollbackcommit()
{
  # Get status of  rollback/commit

  getstat_mysqladmin
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQLADMIN&quot;
    return 1
  fi

  local _var1=Com_commit
  local _var2=Com_rollback

  #local _var1stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var2stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _var1stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var2stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  sleep 3 

  #local  _var1stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local  _var2stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local  _var1stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local  _var2stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _stat1=$(echo &quot;$_var1stat1,$_var1stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)
  local _stat2=$(echo &quot;$_var2stat1,$_var2stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)    

#  local _result=${_result}${_var1}&quot;=&quot;${_stat1}&quot;&amp;&quot;
#        _result=${_result}${_var2}&quot;=&quot;${_stat2}&quot;&amp;&quot;

  echo ${_stat2},${_stat1}

  return 0
}

getstat_dml()
{
  #  Get status of  delete/insert/select/update

  getstat_mysqladmin
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQLADMIN&quot;
    return 1
  fi

  local _var1=Innodb_rows_deleted
  local _var2=Innodb_rows_inserted
  local _var3=Innodb_rows_read
  local _var4=Innodb_rows_updated

  #local _var1stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var2stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var3stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var4stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var4&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _var1stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var2stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var3stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var4stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var4&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  sleep 3

  #local _var1stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var2stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var3stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var4stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var4&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _var1stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var2stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var3stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var4stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var4&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _stat1=$(echo &quot;$_var1stat1,$_var1stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)
  local _stat2=$(echo &quot;$_var2stat1,$_var2stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)
  local _stat3=$(echo &quot;$_var3stat1,$_var3stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)
  local _stat4=$(echo &quot;$_var4stat1,$_var4stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)    

#  local _result=${_result}${_var1}&quot;=&quot;${_stat1}&quot;&amp;&quot;
#        _result=${_result}${_var2}&quot;=&quot;${_stat2}&quot;&amp;&quot;
#        _result=${_result}${_var3}&quot;=&quot;${_stat3}&quot;&amp;&quot;
#        _result=${_result}${_var4}&quot;=&quot;${_stat4}&quot;&amp;&quot;

#  echo $(echo $_result | sed -e &#39;s/\&amp;$//&#39; )
  echo ${_stat2},${_stat1},${_stat4},${_stat3}
  return 0
}

getstat_innodbio()
{
  # Get status of  Get status of  Innodb_buffer_pool_read_requests
  #                                        /Innodb_data_reads/Innodb_data_writes

  getstat_mysqladmin
  if [ $? -ne 0 ] ;then
    echo &quot;NO_MYSQLADMIN&quot;
    return 1
  fi

  local _var1=Innodb_buffer_pool_read_requests
  local _var2=Innodb_data_reads
  local _var3=Innodb_data_writes

  #local _var1stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var2stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var3stat1=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _var1stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var2stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var3stat1=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

   sleep 3
  #local _var1stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var2stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  #local _var3stat2=$($MYADMIN -u$MY_USER -h$MY_HOST $MY_PASSWD -P$MY_PORT extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _var1stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var1&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var2stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var2&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)
  local _var3stat2=$($MYADMIN -u$MY_USER $MY_PASSWD -S $MY_SOCKET extended-status | grep $_var3&quot; &quot; | head -1 | awk &#39;{print $4}&#39;)

  local _stat1=$(echo &quot;$_var1stat1,$_var1stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)
  local _stat2=$(echo &quot;$_var2stat1,$_var2stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)
  local _stat3=$(echo &quot;$_var3stat1,$_var3stat2&quot; | awk -F &quot;,&quot; &#39;{total=($2-$1)/3}{printf &quot;%d&quot;,total}&#39;)    

#  local _result=${_result}${_var1}&quot;=&quot;${_stat1}&quot;&amp;&quot;
#        _result=${_result}${_var2}&quot;=&quot;${_stat2}&quot;&amp;&quot;
#        _result=${_result}${_var3}&quot;=&quot;${_stat3}&quot;&amp;&quot;
#
#  echo $(echo $_result | sed -e &#39;s/\&amp;$//&#39; )
   echo ${_stat1},${_stat2},${_stat3}
  return 0
}

getstat_ioutil()
{
   # Get status of  ioutil.
  local _tmpfile=/tmp/stat__ioutil_$$.tmp
  local IOSTAT
  local _cnttime=4
  local DISKS

  IOSTAT=$(which iostat)
  if [ &quot;$?&quot; != 0 ] ; then
    yum install -y sysstat.x86_64
    echo &quot;test&quot;
  fi 

  if [ -z ${IOSTAT} ] ;  then
  	 echo &quot;iostat_error&quot;
  	 return 1
  fi

  $IOSTAT -x -k 1 $_cnttime &gt; $_tmpfile

  local diskcnt=$(echo $(cat $_tmpfile | wc -l),${_cnttime} | awk -F &quot;,&quot; &#39;{total=($1-2)/$2+2}{printf &quot;%d&quot;,total}&#39;)
  sed -i 1,${diskcnt}&#39;d&#39; $_tmpfile

  MAXAVG=0

  for DISKS in $(/sbin/fdisk -l | grep &quot;^Disk&quot; |awk -F &quot;:&quot; &#39;{print $1}&#39; | awk -F &quot;/&quot; &#39;{print $NF}&#39; )
  do
    MAXAVG_TMP=$(grep &quot;${DISKS} &quot;  $_tmpfile | awk &#39;{ if( $NF != &quot;0.00&quot; ) {total+=$NF;cnt+=1;printf &quot;%f\n&quot;, total/cnt}}&#39; | tail -1)
    MAXAVG=$(echo $MAXAVG $MAXAVG_TMP | awk &#39;{if ($2 &gt; $1) {print $2} else {print $1} }&#39; )
  done

  _stat=$MAXAVG
  _var=ioutil
  _result=${_result}${_var}&quot;=&quot;${_stat}&quot;&amp;&quot;

#  echo $(echo $_result | sed -e &#39;s/\&amp;$//&#39; )
  echo  ${_stat} 

  \rm -f $_tmpfile
  return 0

}

getstat_iorwkb()
{
   # Get status of  io rKB/s wKB/s .
  local _tmpfile=/tmp/stat_iorwkb_$$.tmp
  local IOSTAT
  local _cnttime=4
  local DISKS
  local _stat1
  local _stat2

  IOSTAT=$(which iostat)

  if [ -z ${IOSTAT} ] ;  then
  	 echo &quot;iostat_error&quot;
  	 return 1
  fi

  $IOSTAT -k 1 $_cnttime &gt; $_tmpfile

  local diskcnt=$(echo $(cat $_tmpfile | wc -l),${_cnttime} | awk -F &quot;,&quot; &#39;{total=($1-2)/$2+2}{printf &quot;%d&quot;,total}&#39;)
  sed -i 1,${diskcnt}&#39;d&#39; $_tmpfile

  local RKB_MAXAVG=0
  local WKB_MAXAVG=0

  for DISKS in $(/sbin/fdisk -l | grep &quot;^Disk&quot; |awk -F &quot;:&quot; &#39;{print $1}&#39; | awk -F &quot;/&quot; &#39;{print $NF}&#39; )
  do
    RKB_MAXAVG_TMP=$(grep &quot;${DISKS} &quot;  $_tmpfile | awk &#39;{ if( $3 != &quot;0.00&quot; ) {total+=$3;cnt+=1;printf &quot;%f\n&quot;, total/cnt}}&#39; | tail -1)
    RKB_MAXAVG=$(echo $RKB_MAXAVG $RKB_MAXAVG_TMP | awk &#39;{if ($2 &gt; $1) {print $2} else {print $1} }&#39; )

    WKB_MAXAVG_TMP=$(grep &quot;${DISKS} &quot;  $_tmpfile | awk &#39;{ if( $4 != &quot;0.00&quot; ) {total+=$4;cnt+=1;printf &quot;%f\n&quot;, total/cnt}}&#39; | tail -1)
    WKB_MAXAVG=$(echo $WKB_MAXAVG $WKB_MAXAVG_TMP | awk &#39;{if ($2 &gt; $1) {print $2} else {print $1} }&#39; )
  done

  _stat1=$RKB_MAXAVG
  _var=rkb
  _result=${_result}${_var}&quot;=&quot;${_stat}&quot;&amp;&quot;

  _stat=$WKB_MAXAVG
  _var=wkb
  _result=${_result}${_var}&quot;=&quot;${_stat}&quot;&amp;&quot;

#  echo $(echo $_result | sed -e &#39;s/\&amp;$//&#39; )
  echo ${RKB_MAXAVG},${WKB_MAXAVG}

  \rm -f $_tmpfile
  return 0

}

getvalue()
{

# Get values of key
 local _key=$1
 case &quot;${_key}&quot; in
   &#39;rollbackcommit&#39;)
     RESULT=&quot;ROLLBACK/COMMIT:&quot;$(getstat_rollbackcommit)
   ;;
   &#39;dml&#39;)
     RESULT=&quot;DML(I/D/U/S):&quot;$(getstat_dml)
   ;;
   &#39;innodbio&#39;)
     RESULT=&quot;INNODBIO(Pool_read_qps/reads/writes):&quot;$(getstat_innodbio)
   ;;
   &#39;qps&#39;)
     RESULT=&quot;QPS:&quot;$(getstat_Questions)
   ;;
   &#39;ioutil&#39;)
     RESULT=&quot;IOUTIL:&quot;$(getstat_ioutil)
   ;;
   &#39;iorwkb&#39;)
     RESULT=&quot;OS_IO(R/W):&quot;$(getstat_iorwkb)
   ;;
   &#39;slaveio&#39;)
     RESULT=&quot;SLAVE_IO:&quot;$(getstat_slaveio)
   ;;
   &#39;slavesql&#39;)
     RESULT=&quot;SLAVE_SQL:&quot;$(getstat_slavesql)
   ;;
   &#39;slavelag&#39;)
     RESULT=&quot;SLAVE_LAG:&quot;$(getstat_slavelag)
   ;;
   &#39;innodb_bufsize&#39;)
     RESULT=&quot;INNODB_BUFSIZE:&quot;$(getstat_innodb_bufsize)
   ;;
   &#39;myisam_keysize&#39;)
     RESULT=&quot;MYISAM_KEYSIZE:&quot;$(getstat_myisam_keysize)
   ;;
   &#39;trans_isolation&#39;)
     RESULT=&quot;TX_ISOLATION:&quot;$(getstat_trans_isolation)
   ;;
   &#39;char_server&#39;)
     RESULT=&quot;CHARSET_SERVER:&quot;$(getstat_char_server)
   ;;
   &#39;char_client&#39;)
     RESULT=&quot;CHARSET_Client:&quot;$(getstat_char_client)
   ;;
   &#39;char_conn&#39;)
     RESULT=&quot;CHARSET_Conn:&quot;$(getstat_char_conn)
   ;;
   &#39;sesscnt&#39;)
     RESULT=&quot;SESSION(ALL):&quot;$(getstat_sesscnt)
   ;;
   &#39;session&#39;)
     RESULT=&quot;SESSION(ACTIVE/ALL):&quot;$(getstat_session)
   ;;
   &#39;load&#39;)
     RESULT=&quot;LOAD:&quot;$(getstat_load)
   ;;
   &#39;role&#39;)
     RESULT=&quot;ROLE:&quot;$(getstat_role)
   ;;
   *)
    echo &quot;${_key}:No_Such_Key&quot;
   ;;
esac

echo $RESULT
}

######################################### Funtions End 

#### Main -- Begin 

if [ -z &quot;$MY_HOST&quot; ] ; then
  MY_HOST=`/sbin/ifconfig | grep &quot;inet addr&quot; | awk -F: &#39;{print $2}&#39; | awk {&#39;print $1&#39;} | head -1`
fi

if [ -z &quot;$MY_PORT&quot; ] ; then
   MY_PORT=3306
fi 

if [ -z &quot;$MY_KEY&quot; ] ; then
   MY_KEY=${ALL_KEY}
fi 

KEYLIST=$(echo ${MY_KEY} | tr -d &#39; &#39; | sed &#39;s/,/ /g&#39; )

CURRDATE=`date +%F`
MAX_PROC_CNT=10

#### Get Mysql Version
# if verion = 4.X , VERSION_FLAG=1
VERSION_FLAG=$($MYSQL --version | grep &quot;Distrib 4.&quot; | wc -l)
[ -z &quot;${VERSION_FLAG}&quot; ] &amp;&amp; VERSION_FLAG=0

#### Start to Loop
for KEY in $KEYLIST
do
	echo $(getvalue $KEY)
done

exit 0

#### Main -- End</pre>