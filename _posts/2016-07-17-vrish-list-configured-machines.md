[root@vapp02 ~]# virsh list
 Id    Name                           State
----------------------------------------------------
 1     lb03                           running
 3     app3                           running
 4     dev03                          running

[root@vapp02 ~]# virsh list --all
 Id    Name                           State
----------------------------------------------------
 1     lb03                           running
 3     app3                           running
 4     dev03                          running
 -     app4                           shut off
 -     lb01                           shut off
