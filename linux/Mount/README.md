## 마운트 하기
- putty 접속  
  
  
- attach된 /dev/xvda 21.5GB, /dev/xvdb 85.9GB 디스크 확인
    xvda는 현재 사용할 수 있는 용량 디스크  
    xvdb는 아직 사용하지 못하는 가상 디스크  
```
[root@VM1578544410428 ~]# fdisk -l

Disk /dev/xvda: 21.5 GB, 21474836480 bytes, 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000b2185

    Device Boot      Start         End      Blocks   Id  System
/dev/xvda1   *        2048     1953791      975872   83  Linux
/dev/xvda2         1953792    41943039    19994624   8e  Linux LVM

Disk /dev/xvdb: 85.9 GB, 85899345920 bytes, 167772160 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos-root: 18.5 GB, 18471714816 bytes, 36077568 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos-swap: 2000 MB, 2000683008 bytes, 3907584 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

```
  
  
## 디스크 파티션
### 파트션 생성
- fdisk [mount할 스토리지 경로]  
```
$ fdisk /dev/xvdb

Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x3c077434.

Command (m for help): m    //menu 보기
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)
```
  
  
### 새 파티션 추가 : n
```
Command (m for help): n    // 새 파티션 추가
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p    //p입력 
Partition number (1-4, default 1): 1    //1
First sector (2048-167772159, default 2048):    //enter(생략)
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-167772159, default 167772159):    //enter(생략)
Using default value 167772159
Partition 1 of type Linux and of size 80 GiB is set
```
  

### 파티션 테이블 출력  
```
Command (m for help): p

Disk /dev/xvdb: 85.9 GB, 85899345920 bytes, 167772160 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x09282e61

    Device Boot      Start         End      Blocks   Id  System
/dev/xvdb1            2048   167772159    83885056   83  Linux

```
  

### 저장하고 파티션 디스크 종료 : w  
```
Command (m for help): w

The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
  
### 스토리지 포맷  
- mkfs.[파일시스템 형식] [스토리지 경로]  
    - centos 5.x : mkfs.ext3  
    - centos 7.x : mkfs.xfs  
```
$ mkfs.ext4 /dev/xvdb

mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
5242880 inodes, 20971520 blocks
1048576 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2168455168
640 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

```
  

### 포맷된 파일 시스템 타입과 리스트 출력
- bldid
```
$ blkid

/dev/xvda1: UUID="a8f30384-6763-4a0d-afc8-6dec1f21b2c6" TYPE="xfs"
/dev/xvda2: UUID="G75czv-eWrS-dhND-cdc0-9sV4-Y4u8-jw2wfp" TYPE="LVM2_member"
/dev/xvdb: UUID="a4456cbe-5804-4c29-9649-346af5dd9281" TYPE="ext4"
/dev/mapper/centos-root: UUID="5ae56ea3-3468-4553-9530-5cfd4a33f952" TYPE="xfs"
/dev/mapper/centos-swap: UUID="7e5cf8e8-f8b9-4444-bc49-2a3e1021c7a9" TYPE="swap"

```
  

### 원하는 폴더에 스토리지 마운트
- mount [스토리지 경로] [마운트할 디렉토리 경로]
```
$ mount /dev/xvdb /home

You have new mail in /var/spool/mail/root
```
  

### 결과 출력
- df -TH  
    - T: 파일 시스템 타입 및 파티션 정보 출력, H: 1,000단위 용량 표시  
```
$ df -TH

Filesystem              Type      Size  Used Avail Use% Mounted on
/dev/mapper/centos-root xfs        19G  1.8G   17G  10% /
devtmpfs                devtmpfs  505M     0  505M   0% /dev
tmpfs                   tmpfs     517M     0  517M   0% /dev/shm
tmpfs                   tmpfs     517M  7.1M  510M   2% /run
tmpfs                   tmpfs     517M     0  517M   0% /sys/fs/cgroup
/dev/xvda1              xfs       996M  142M  855M  15% /boot
tmpfs                   tmpfs     104M     0  104M   0% /run/user/0
/dev/xvdb               ext4       85G   59M   81G   1% /home
```    


## 재부팅시에도 항상 마운트 설정(중요!)  
- [스토리지] [마운드할 디렉토리] [파일시스템 타입] [속성] [dump 사용여부] [파일시스템 체크 여부]  
- default : 읽기, 쓰기, 실행 가능  
- dump 사용 여부 : dump 명령어를 이용한 백업가능  
- 파일 시스템 체크 여부  
    - 0 : 체크 안함  
    - 1 : 가장 먼저 파일 시스템 체크  
    - 2 : 1 다음 파일 시스템 체크  
- 아래에 추가  
    /dev/xvdb /home ext4 defaults 1 2  
```
$ vi /etc/fstab

#
# /etc/fstab
# Created by anaconda on Tue Mar 12 16:59:24 2019
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=a8f30384-6763-4a0d-afc8-6dec1f21b2c6 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
/dev/xvdb    /home    ext4    defaults    1    2    //추가
```
  