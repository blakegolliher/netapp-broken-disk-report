#!/usr/local/python-2.7.2/bin/python
##
#
# A simple failed disk report
# Using Netapp API and Python
#
# Blake Golliher - blakegolliher@gmail.com
#
##

import sys
sys.path.append("/var/local/netapp-manageability-sdk-5.1/lib/python/NetApp")
from NaServer import *
import getpass

password = getpass.getpass()

filer_name = sys.argv[1]

filer = NaServer(filer_name,1,6)
filer.set_admin_user('root', 'password')
cmd = NaElement("disk-list-info")
ret = filer.invoke_elem(cmd)

if(ret.results_status() == "failed"):
  print "%s failed." % filer_name
	print(ret.results_reason() + "\n")
	sys.exit(2)

status = ret.child_get("disk-details")

if(status == None):
	print "status_children_get was empty\n"
	sys.exit(2)
else:
	result = status.children_get()

for diskinfo in result:
	if diskinfo.child_get_string("broken-details") != None:
		print "Disk Named %s is broken for reason :%s." % (diskinfo.child_get_string("name"),(diskinfo.child_get_string("broken-details")))
