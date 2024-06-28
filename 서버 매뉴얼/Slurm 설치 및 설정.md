# Slurm 설치 및 설정

# 7.1. Slurm 설치 및 설정

Slurm(Simple Linux Utility for Resource Management)은 Linux 클러스터에서 작업 스케줄링과 자원 관리를 위해 널리 사용되는 오픈 소스 워크로드 관리자입니다. 이 문서에서는 Ubuntu 서버에 Slurm을 설치하고 설정하는 방법을 자세히 설명하겠습니다.

## 사전 요구 사항

- Slurm을 설치할 Ubuntu 서버 또는 클러스터
- 서버 간 네트워크 연결 및 이름 확인(DNS 또는 /etc/hosts)
- 클러스터의 모든 노드에 대한 SSH 액세스 권한

## Munge 설치 및 설정

Munge는 Slurm의 인증 및 암호화를 위한 유틸리티입니다. Slurm을 설치하기 전에 Munge를 설정해야 합니다.

1. 모든 노드에서 다음 명령을 실행하여 Munge를 설치합니다:
    
    ```
    sudo apt install munge
    
    ```
    
2. 헤드 노드에서 Munge 키를 생성합니다:
    
    ```
    sudo /usr/sbin/create-munge-key
    
    ```
    
3. 생성된 Munge 키를 모든 노드의 `/etc/munge/munge.key`에 복사합니다. 예를 들면:
    
    ```
    sudo scp /etc/munge/munge.key node1:/etc/munge/
    sudo scp /etc/munge/munge.key node2:/etc/munge/
    
    ```
    
4. 모든 노드에서 Munge 서비스를 시작하고 부팅 시 자동으로 시작되도록 설정합니다:
    
    ```
    sudo systemctl start munge
    sudo systemctl enable munge
    
    ```
    

## Slurm 설치

1. 모든 노드에서 다음 명령을 실행하여 Slurm을 설치합니다:
    
    ```
    sudo apt install slurm-wlm
    
    ```
    
2. 설치 중에 슬럼 데몬 구성(Slurm daemon configuration)을 선택하라는 메시지가 표시됩니다. 클러스터의 헤드 노드에서는 "slurm-wlm-master-basic"을, 나머지 노드에서는 "slurm-wlm-node-basic"을 선택합니다.

## Slurm 구성

1. 헤드 노드에서 `/etc/slurm-llnl/slurm.conf` 파일을 편집하여 Slurm 구성을 수정합니다. 다음은 기본 구성의 예시입니다:
    
    ```
    ClusterName=mycluster
    ControlMachine=head_node
    SlurmUser=slurm
    SlurmdUser=root
    SlurmctldPort=6817
    SlurmdPort=6818
    AuthType=auth/munge
    StateSaveLocation=/var/spool/slurm-llnl/ctld
    SlurmdSpoolDir=/var/spool/slurm-llnl/d
    SwitchType=switch/none
    MpiDefault=none
    SlurmctldPidFile=/var/run/slurm-llnl/slurmctld.pid
    SlurmdPidFile=/var/run/slurm-llnl/slurmd.pid
    ProctrackType=proctrack/linuxproc
    ReturnToService=2
    SlurmctldTimeout=300
    SlurmdTimeout=300
    InactiveLimit=0
    MinJobAge=300
    KillWait=30
    Waittime=0
    SchedulerType=sched/backfill
    SelectType=select/linear
    AccountingStorageType=accounting_storage/filetxt
    AccountingStorageLoc=/var/log/slurm-llnl/accounting
    JobAcctGatherType=jobacct_gather/linux
    SlurmctldDebug=info
    SlurmdDebug=info
    NodeName=node[1-2] CPUs=2 State=UNKNOWN
    PartitionName=debug Nodes=node[1-2] Default=YES MaxTime=INFINITE State=UP
    
    ```
    
    이 구성은 클러스터 이름, 제어 머신, Slurm 사용자, 포트, 인증 방법 등을 정의합니다. `NodeName`과 `PartitionName` 섹션에서는 클러스터의 노드와 파티션을 정의합니다.
    
2. 구성 파일을 모든 노드의 `/etc/slurm-llnl/` 디렉토리에 복사합니다.

## Slurm 서비스 시작

1. 헤드 노드에서 다음 명령을 실행하여 Slurm 컨트롤러 데몬을 시작합니다:
    
    ```
    sudo systemctl start slurmctld
    sudo systemctl enable slurmctld
    
    ```
    
2. 모든 노드에서 다음 명령을 실행하여 Slurm 노드 데몬을 시작합니다:
    
    ```
    sudo systemctl start slurmd
    sudo systemctl enable slurmd
    
    ```
    

## Slurm 클러스터 상태 확인

1. 헤드 노드에서 다음 명령을 실행하여 클러스터의 상태를 확인합니다:
    
    ```
    sinfo
    
    ```
    
    이 명령은 파티션, 노드 상태, 노드 수 등의 정보를 표시합니다.
    
2. 클러스터의 노드 상태를 확인하려면 다음 명령을 실행합니다:
    
    ```
    scontrol show nodes
    
    ```
    
    이 명령은 각 노드의 상세 정보를 표시합니다.
    

## 사용자 계정 설정

1. 클러스터에서 작업을 실행할 사용자 계정을 생성합니다:
    
    ```
    sudo useradd -m -s /bin/bash testuser
    sudo passwd testuser
    
    ```
    
2. 사용자에게 Slurm 클러스터에 접근할 수 있는 권한을 부여합니다. `/etc/slurm-llnl/slurm.conf` 파일에서 `AllowUsers` 또는 `AllowGroups` 옵션을 사용하여 사용자 또는 그룹에 대한 접근 권한을 설정할 수 있습니다.

## 작업 제출 테스트

1. 테스트 작업을 제출하여 Slurm 클러스터가 올바르게 작동하는지 확인합니다. 다음은 간단한 "Hello, World!" 작업을 제출하는 예시입니다:
    
    ```
    echo '#!/bin/bash' > test_job.sh
    echo 'echo "Hello, World!"' >> test_job.sh
    sbatch test_job.sh
    
    ```
    
2. 작업 상태를 확인하려면 다음 명령을 사용합니다:
    
    ```
    squeue
    
    ```
    
    이 명령은 제출된 작업의 상태, 작업 ID, 사용자, 파티션 등의 정보를 표시합니다.
    

## 결론

이 문서에서는 Ubuntu 서버에 Slurm 워크로드 관리자를 설치하고 설정하는 방법을 설명했습니다. Munge를 설정하고, Slurm을 설치하고, 구성 파일을 편집하여 클러스터를 설정했습니다. 또한 사용자 계정을 생성하고 테스트 작업을 제출하여 Slurm 클러스터의 기능을 확인했습니다. 이제 Slurm을 사용하여 클러스터에서 작업을 효율적으로 스케줄링하고 관리할 수 있습니다.