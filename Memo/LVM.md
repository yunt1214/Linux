# LVM (Logical Volume Manager)

- 여러 개의 하드디스크를 하나의 디스크처럼 혹은 두, 세 개의 디스크처럼 사용할 수 있다.
- 사용 중인 파티션의 크기를 줄이거나 확장 가능
- 특히 파티션 확장 시 간단한 명령만으로 데이터 이전 및 손실 없이 가능
- 파티션 축소 시 데이터 손실의 위험성이 존재함
- 최근 배포판에서는 자동 파티션 선택 시 기본으로 LVM으로 구성

## LVM 볼륨 종류

### 물리 볼륨 (Physical Volume)

- 실제 디스크에 물리적으로 분할한 파티션
- LVM을 구성하는 실제 파티션으로 물리 볼륨을 생성
- pvscan, pvcreate, pvdisplay

### 볼륨 그룹 (Volume Group)

- 물리 볼륨이 모여서 만들어진 그룹
- 실제 디스크의 역할을 함.
- vgscan, vgcreate, vgdisplay, vgextend

### 논리 볼륨 (Logical Volume)

- 볼륨 그룹에서 사용자가 필요한만큼 할당해서 만들어진 공간
- 실제 디스크에서의 파티션 역할을 함
- lvscan, lvcreate, lvdisplay, lvextend

## LVM 구성

- 디스크 추가 -> 파티션 생성 -> LVM 구성 (-> 파일 시스템 생성 -> 마운트)

### 디스크 추가

- 디스크 추가 -> 파티션 생성 -> 물리 볼륨 생성 -> 볼륨 그룹 생성 -> 논리 볼륨 생성 -> 파일 시스템 생성 -> 마운트
- LVM 구성 후 마운트

######

    # fdisk /dev/sdb // 파티션 생성
    m -> n -> p -> 1 -> 엔터 2번 -> p -> t -> l -> 8e -> p -> w 

    # pvcreate /dev/sdb1 // PV 만들기

    # vgcreate cloud /dev/sdb1 // VG 만들기

    # lvcreate --size 20G --name test cloud // LV 만들기 (방법 1)
    # lvcreate -l +100%FREE --name test cloud // LV 만들기 (방법 2)

    # mkfs -t xfs /dev/mapper/cloud-test // 파일 시스템 생성 (방법 1)
    # mkfs.xfs /dev/mapper/cloud-test // 파일 시스템 생성 (방법 2)

    # mount /dev/mapper/cloud-test /lvmtest // 마운트

### 디스크 확장

- 디스크 추가 -> 파티션 생성 -> 물리 볼륨 생성 -> 기존 볼륨 그룹에 추가 (볼륨 그룹 확장) -> 논리 볼륨 확장 -> 파일 시스템 조절

######

    # fdisk /dev/sdc // 파티션 생성
    m -> n -> p -> 1 -> 엔터 2번 -> p -> t -> l -> 8e -> p -> w 

    # pvcreate /dev/sdc1 // PV만들기

    # vgextend centos /dev/sdc1 // VG에 추가 (볼륨그룹 확장)

    # lvextend -l +100%FREE /dev/centos/root // LV 확장 (방법 1)
    # lvextend -L +20G /dev/centos/root // LV 확장 (방법 2)

    # xfs_growfs /dev/centos/root // 파일 시스템 조절 (xfs) 
    # resize2fs /dev/centos/root // 파일 시스템 조절 (ext4)
