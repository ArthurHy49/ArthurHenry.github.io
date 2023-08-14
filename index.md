```
#！/bin/bash
#变量区

whileNum=1;
paramcn=$#;
sysdate=`date +'%Y%m%d'`;
sql="";
Log="./scriptLog${sysdate}.log";
temp="";

#环境区

#使用帮助说明
function script_help(){
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ######功能说明#######"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 该脚本主要使用于..."；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ######使用和将会生成的临时文件和日志#######"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ${Log}------>日志文件"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ${temp}----->临时文件"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ######建议命令#######"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】  ls ${Log} | xargs rm -f"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】  ls ${temp} | xargs rm -f"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ######使用#######"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 直接执行 sh ${script_name}"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ######注意#######"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 注意1：该脚本执行期间，请勿中止，稍等脚本执行完成即可"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 注意2：该脚本执行期间，可另起窗口 tail -f ${Log} 查看执行进度"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 ######标准#######"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 标准1："；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 标准2："；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 标准3："；
}

#执行的逻辑方法
#输入日志命令 2>&1 | tee -a {log} 或 >
function func1(){
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 1.【func1方法执行开始】"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 1.1）【开始检查环境变量】"；
  echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
  #判断
  if [ -s ${temp} ] ; then 
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 存在上次执行痕迹文件，开始清理"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    >${temp}
    >${Log}
  fi
  #逻辑处理
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 2.【业务逻辑处理】"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 2.1）【业务逻辑处理1】"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
   
  #数据库连接
  sqlplus -s connect_str<<! 1>/dev/null 2>&1
  spool ${temp} 
  #sql语句
  ${sql}
  quit;
!

  haveValue=`head -1 ${temp}`;
  if [ ${haveValue} == 1 ] ; then
      echo "结果正常。";
  else
      echo "结果错误，脚本退出！";
      exit 2;
  fi

  #循环
  while read LINE
  do
    #逻辑处理
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 2.1）【业务逻辑处理${whileNum}】"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    whileNum=`${whileNum} + 1`;
  done<${temp}
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 3.【func1方法执行完成】"；
    echo -e "`date +'%Y%m%d %H:%M:%S'` 【SUSE】【INFO】 #############################"；
    exit 0;
}

if [ ${paracn} -eq 1 ] ; then
  param1=`echo $1 |tr 'a-z' 'A-Z'`
  if [ ${param1} == "HELP" ] || [ ${param1} == "--HELP" ] || [ ${param1} == "-HELP" ] ; then
     script_help;
     exit 2;
  else
    echo "输出参数有误：$@ 有误"；
    script_help;
    exit 2;
  fi
elif [ ${paramcn} -eq 0] ; then
  func1;
else
  echo "输出参数有误：$@ 有误"；
  script_help;
  exit 2;
fi
```
