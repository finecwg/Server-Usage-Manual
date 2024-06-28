# Slurm 파티션, QoS 설정

# 7.2. Slurm 파티션, QoS 설정

Slurm에서 파티션과 QoS(Quality of Service)는 클러스터의 자원을 효율적으로 관리하고 사용자 간의 자원 할당을 제어하는 데 사용됩니다. 이 문서에서는 Slurm 클러스터에서 파티션과 QoS를 설정하고 관리하는 방법을 자세히 설명하겠습니다.

## 파티션 설정

파티션은 Slurm 클러스터의 노드 그룹으로, 사용자가 작업을 제출할 수 있는 논리적 단위입니다. 파티션을 사용하여 노드의 특성에 따라 클러스터를 구성할 수 있습니다.

1. `/etc/slurm-llnl/slurm.conf` 파일을 편집하여 파티션을 정의합니다. 다음은 파티션 설정의 예시입니다:
    
    ```
    PartitionName=debug Nodes=node[1-4] Default=YES MaxTime=INFINITE State=UP
    PartitionName=batch Nodes=node[5-8] Default=NO MaxTime=24:00:00 State=UP
    PartitionName=gpu Nodes=node[9-10] Default=NO MaxTime=48:00:00 State=UP
    
    ```
    
    이 예시에서는 "debug", "batch", "gpu" 세 개의 파티션을 정의하고 있습니다. 각 파티션은 특정 노드 그룹, 기본 설정 여부, 최대 실행 시간, 상태 등을 지정합니다.
    
2. 파티션 설정을 적용하려면 Slurm 컨트롤러를 다시 시작합니다:
    
    ```
    sudo systemctl restart slurmctld
    
    ```
    
3. `sinfo` 명령을 사용하여 파티션 정보를 확인합니다:
    
    ```
    sinfo
    
    ```
    
    출력 결과에서 정의한 파티션과 각 파티션의 상태, 노드 수, 최대 실행 시간 등의 정보를 확인할 수 있습니다.
    

## QoS 설정

QoS는 사용자나 작업 그룹에 할당된 자원 및 우선순위를 제어하는 데 사용됩니다. QoS를 통해 사용자 간의 자원 경합을 관리하고 중요한 작업에 우선순위를 부여할 수 있습니다.

1. `/etc/slurm-llnl/slurm.conf` 파일에서 QoS를 정의합니다. 다음은 QoS 설정의 예시입니다:
    
    ```
    PriorityType=priority/multifactor
    PriorityDecayHalfLife=14-0
    PriorityCalcPeriod=5
    PriorityFavorSmall=YES
    PriorityMaxAge=7-0
    PriorityUsageResetPeriod=NONE
    PriorityWeightAge=1000
    PriorityWeightFairshare=10000
    PriorityWeightJobSize=1000
    PriorityWeightPartition=1000
    PriorityWeightQOS=1000
    
    AccountingStorageEnforce=associations,limits,qos,safe
    AccountingStorageType=accounting_storage/slurmdbd
    AccountingStorageHost=dbnode
    AccountingStoragePort=6819
    
    JobAcctGatherType=jobacct_gather/linux
    JobAcctGatherFrequency=30
    
    QOSName=normal Priority=1000 UsageFactor=1 GrpJobs=100 GrpSubmit=1000 GrpCPUs=200 GrpMem=1000
    QOSName=high Priority=2000 UsageFactor=1 GrpJobs=200 GrpSubmit=2000 GrpCPUs=400 GrpMem=2000
    QOSName=low Priority=500 UsageFactor=1 GrpJobs=50 GrpSubmit=100 GrpCPUs=100 GrpMem=500
    
    ```
    
    이 예시에서는 우선순위 계산에 사용되는 매개변수를 설정하고, 계정 스토리지 데이터베이스를 구성하며, "normal", "high", "low" 세 개의 QoS를 정의합니다. 각 QoS는 우선순위, 사용량 계수, 그룹 제한 등을 지정합니다.
    
2. QoS 설정을 적용하려면 Slurm 컨트롤러를 다시 시작합니다:
    
    ```
    sudo systemctl restart slurmctld
    
    ```
    
3. `sacctmgr` 명령을 사용하여 QoS 정보를 확인합니다:
    
    ```
    sacctmgr show qos
    
    ```
    
    출력 결과에서 정의한 QoS와 각 QoS의 우선순위, 사용량 계수, 제한 등의 정보를 확인할 수 있습니다.
    

## 사용자 및 계정 관리

1. `sacctmgr` 명령을 사용하여 사용자 계정을 생성하고 관리합니다. 다음은 사용자 계정을 생성하는 예시입니다:
    
    ```
    sacctmgr add user testuser account=default
    
    ```
    
    이 명령은 "testuser"라는 사용자를 "default" 계정에 추가합니다.
    
2. 사용자에게 QoS를 할당하려면 다음 명령을 사용합니다:
    
    ```
    sacctmgr modify user testuser set qos=normal
    
    ```
    
    이 명령은 "testuser"에게 "normal" QoS를 할당합니다.
    
3. 계정을 생성하고 관리하려면 다음 명령을 사용합니다:
    
    ```
    sacctmgr add account testaccount
    
    ```
    
    이 명령은 "testaccount"라는 계정을 생성합니다.
    

## 작업 제출 시 파티션 및 QoS 지정

1. `sbatch` 명령을 사용하여 작업을 제출할 때 파티션과 QoS를 지정할 수 있습니다. 다음은 작업 제출 예시입니다:
    
    ```
    sbatch --partition=batch --qos=normal test_job.sh
    
    ```
    
    이 명령은 "batch" 파티션과 "normal" QoS를 사용하여 "test_job.sh" 스크립트를 실행하는 작업을 제출합니다.
    
2. 사용자의 기본 QoS를 설정하려면 `sacctmgr` 명령을 사용합니다:
    
    ```
    sacctmgr modify user testuser set defaultqos=normal
    
    ```
    
    이 명령은 "testuser"의 기본 QoS를 "normal"로 설정합니다. 사용자가 작업을 제출할 때 QoS를 명시적으로 지정하지 않으면 기본 QoS가 사용됩니다.
    

## 결론

이 문서에서는 Slurm 클러스터에서 파티션과 QoS를 설정하고 관리하는 방법을 설명했습니다. 파티션을 사용하여 노드 그룹을 정의하고, QoS를 사용하여 사용자 간의 자원 할당과 우선순위를 제어할 수 있습니다. 또한 사용자 계정을 생성하고 관리하는 방법과 작업 제출 시 파티션 및 QoS를 지정하는 방법도 알아보았습니다. 이러한 설정을 활용하면 Slurm 클러스터에서 자원을 효율적으로 관리하고 사용자 요구 사항에 맞게 작업을 스케줄링할 수 있습니다.