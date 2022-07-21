# Network File System

* nfs server
```
$sudo apt install nfs-kernel-server
  > $sudo apt update 선행
$sudo mkdir -p /mnt/[name]
  > -p(--parents) : 상위경로까지 생성
$sudo chown -R nobody:nogroup /mnt/[name]
  > -R(--recursive) : 폴더뿐 아니라 폴더내의 파일(들)도 소유권을 같이 변경
$sudo chmod 777 /mnt/[name]
  > 7 : r(4)w(2)x(1)
$code /etc/exports
  > code가 세팅 되어 있지 않다면 vi, vim, nano를 사용 $vim /etc/exports | $nano /etc/exports
  > /mnt/[name] *(rw,sync,no_subtree_check) 추가
  > /mnt/[name] xxx.xxx.xxx.xxx/24(rw,sync,no_subtree_check) 특정 IP 대역으로도 추가 가능
$sudo exportfs -a
  > /etc/exports 리스트를 가지고 NFS 공유폴더 리스트를 갱신
    > $sudo exportfs 로 NFS 공유폴더 리스트 확인
$sudo /etc/init.d/nfs-kernel-server restart
  > 실패시
  > $sudo apt-get install portmap
  > $sudo apt-get install rpcbind => sudo service rpcbind restart

client에서 접속 실패시 
  > ufw를 unactive 시키거나
    > $sudo ufw disable 
  > ufw allow 규칙를 추가해주고 활성화 확인
    > $sudo ufw allow from xxx.xxx.xxx.xxx to any port nfs
    > $sudo ufw enable
    > $sudo ufw status
  > 마지막으로 exportfs 를 갱신해준다
    > $sudo exportfs -ra
      > /etc/exports 리스트를 가지고 NFS 공유리스트를 재갱신
```
* nfs client
```
$sudo apt install nfs-common
$sudo mkdir -p /mnt/[name]
$sudo chown -R nobody:nogroup /mnt/[name]
$sudo chmod 777 /mnt/out
$sudo mount xxx.xxx.xxx.xxx:/mnt/[server_folder_name] /mnt/[client_folder_name]
```
* windows to nfs client

https://www.putty.org/ (PuTTY 다운로드)
```
putty.exe 설치하면 pscp 지원으로 pscp 사용가능

windows _ search _ cmd 실행
pscp 설치 확인
  >pscp --version
파일 전송
  >pscp c:\...\[file] root@xxx.xxx.xxx.xxx:/mnt/[name]
```
