---
author: ebankie
comments: true
date: 2013-12-06 08:25:20+00:00
layout: post
link: http://www.ebankie.com/blog/?p=78
slug: python-%e7%bb%9f%e8%ae%a1agent%e4%bf%a1%e6%81%af
title: python 统计agent信息
wordpress_id: 78
categories:
- 技术文章
tags:
- 统计，数据分析，无线，agent
---

count.py

[well]

    
    #!/usr/bin/pythona
    #-*-coding:utf-8-*-
    import re
    import os
    import sys
    from mysql import MySQL
    from datetime import *
    import time
    t1=time.time()
    ti = date.today()
    strinfo = re.compile("-")
    filetime = strinfo.sub('',str(ti))
    file = '/data/agent/'+filetime+'/useragent.log'
    fileop = open(file)
    #n=MySQL()
    #n.query('select * from t_system')
    #print n.fetchAll()
    #print date.today()
    #sys.exit(0)
    system={}
    browser={}
    vesion={}
    device={}
    num = 0
    try:
        while True:
              line = fileop.readline()
              num+=1
              if not line:
                  break
              sys_preg = re.compile('Android|OS|Palmos|SymbianOS|Windows|flashlite|MeeGo|webOS|BlackBerry|Java|BREW')　#若每个系统加“（）效率很低下”
              sysret = sys_preg.search(line)
              if sysret:
                    syskey = sysret.group()
                    system[syskey]=system.get(syskey,1)+1   
              bro_preg = re.compile('MQQBrowser|XiaoMi|360browser|Opera|UCBrowser|baidubrowser|NokiaBrowser|MSIE|Release')
              broret = bro_preg.search(line)
              if broret:
                    brokey=broret.group()
                    browser[brokey]=browser.get(brokey,1)+1
              ves_preg = re.compile('Android[\/ ][\d.|_]|OS[\s]+[\d]|Palmos|SymbianOS[\/]\d|Windows[\s]+Phone[\s]+OS[\s]+\d')
              vesret = ves_preg.search(line)
              if vesret:
                    veskey = vesret.group()
                    strinfo = re.compile("\s")
                    vkey = strinfo.sub('',veskey)
                    vesion[vkey]=vesion.get(vkey,1)+1
              dev_preg = re.compile('iPhone|HTC|iPod|Nexus|Lenovo|HUAWEI|ZTE|GT|SCH|SAMSUNG|Sony|TCL|Motorola|XiaoMi')
              devret = dev_preg.search(line)
              if devret:
                    devk = devret.group()
                    device[devk]= device.get(devk,1)+1 
    
    finally:
        fileop.close()
    #print data
    n=MySQL()
    system['total']=num
    system['date'] = date.today()
    vesion['total']=num
    vesion['date'] = date.today()
    device['total']=num
    device['date'] = date.today()
    browser['total'] = num
    browser['date']= date.today()
    def insertSys(data):
            n.insert('t_system',data)
            n.commit()
            #print n.fetchAll()
    def insertVes(data):
            indata = {}
            for key in data:
                if re.search('^OS',key):
                    indata['ios'] =indata.get('ios','')+ key+':'+str(data[key])+',' 
                elif re.search('^Android',key):
                    indata['android'] =indata.get('android','')+ key+':'+str(data[key])+',' 
                elif re.search('^Palmos',key):
                    #'Android[\/ ][\d.|_]|OS[\s]+[\d]|Palmos|SymbianOS[\/]\d|Windows[\s]+Phone[\s]+OS[\s]+\d
                    indata['palmos'] =indata.get('palmos','')+ key+':'+str(data[key])+','
                elif re.search('^Windows',key):
                    indata['windows'] =indata.get('windows','')+ key+':'+str(data[key])+',' 
                elif re.search('^SymbianOS',key):
                    indata['sysmbian'] =indata.get('sysmbian','')+ key+':'+str(data[key])+','
            indata['date']=date.today()
            n.insert('t_sysversion',indata)
            n.commit()
    def insertDev(data):
            n.insert('t_devce',data)
            n.commit()
    def insertBro(data):
            n.insert('t_browse',data)
            n.commit()
    #print system,browser,vesion,device
    t2=time.time()
    insertSys(system)
    insertVes(vesion)
    insertBro(browser)
    insertDev(device)


[/well]

mysql.py

[well]

    
    #!/usr/bin/python
    import MySQLdb
    OperationalError = MySQLdb.OperationalError
    class MySQL:
        host='xxxx'
        port=xxx
        user='db'
        passwd='db'
        db='db'
        conn=''
        cur=''
        def __init__(self):
            try:
                self.conn=MySQLdb.connect(host=self.host,port=self.port,user=self.user,passwd=self.passwd,db=self.db)
                self.conn.autocommit(False)
                self.cur=self.conn.cursor()
            except MySQLdb.Error,e:
                print("Mysql Error %d: %s" % (e.args[0], e.args[1]))
        def __del__(self):
            self.close()
    
        def selectDb(self,db):
            try:
                self.conn.select_db(db)
            except MySQLdb.Error,e:
                print("Mysql Error %d: %s" % (e.args[0], e.args[1]))
    
        def query(self,sql):
            try:
                n=self.cur.execute(sql)
                return n
            except MySQLdb.Error, e:
                print("Mysql Error:%s\nSQL:%s" %(e,sql))
    
        def fetchRow(self):
            result = self.cur.fetchone()
            return result
    
        def fetchAll(self):
            result=self.cur.fetchall()
            desc =self.cur.description
            d = []
            for inv in result:
                _d = {}
                for i in range(0,len(inv)):
                    _d[desc[i][0]] = str(inv[i])
                d.append(_d)
            return d
    
        def insert(self,table_name,data):
               columns=data.keys()
               _prefix="".join(['INSERT INTO `',table_name,'`'])
               _fields=",".join(["".join(['`',column,'`']) for column in columns])
               _values=",".join(["%s" for i in range(len(columns))])
               _sql="".join([_prefix,"(",_fields,") VALUES (",_values,")"])
               _params=[data[key] for key in columns]
               return self.cur.execute(_sql.lower(),tuple(_params))
    
        def update(self,tbname,data,condition):
            _fields=[]
            _prefix="".join(['UPDATE `',tbname,'`','SET'])
            for key in data.keys():
                _fields.append("%s = %s" % (key,data[key]))
            _sql="".join([_prefix ,_fields, "WHERE", condition ])
    
            return self.cur.execute(_sql)
    
        def delete(self,tbname,condition):
            _prefix="".join(['DELETE FROM  `',tbname,'`','WHERE'])
            _sql="".join([_prefix,condition])
            return self.cur.execute(_sql)
    
        def getLastInsertId(self):
            return self.cur.lastrowid
    
        def rowcount(self):
            return self.cur.rowcount
    
        def commit(self):
            self.conn.commit()
    
        def rollback(self):
            self.conn.rollback()
    
        def close(self):
            self.cur.close()
            self.conn.close()


[/well]


