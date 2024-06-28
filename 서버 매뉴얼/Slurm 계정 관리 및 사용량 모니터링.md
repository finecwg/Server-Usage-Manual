# Slurm 계정 관리 및 사용량 모니터링

# 7.6. Slurm 계정 관리 및 사용량 모니터링

Slurm 작업 스케줄러를 사용할 때는 사용자 계정을 효과적으로 관리하고 클러스터 자원의 사용량을 모니터링하는 것이 중요합니다. 이 문서에서는 Slurm에서 계정을 관리하고 사용량을 모니터링하는 방법에 대해 자세히 설명하겠습니다.

## Slurm 계정 관리

Slurm에서는 `sacctmgr` 명령을 사용하여 사용자 계정, 계정 연관 관계, 클러스터 구성 등을 관리할 수 있습니다.

### 사용자 계정 생성

새로운 사용자 계정을 생성하려면 다음 명령을 사용합니다:

```
sacctmgr add user <username> account=<account_name>

```

- `<username>`: 생성할 사용자 계정의 사용자명을 지정합니다.
- `<account_name>`: 사용자 계정을 연결할 Slurm 계정 이름을 지정합니다.

### 계정 생성

새로운 Slurm 계정을 생성하려면 다음 명령을 사용합니다:

```
sacctmgr add account <account_name> [description="<description>"]

```

- `<account_name>`: 생성할 계정의 이름을 지정합니다.
- `<description>`: 선택적으로 계정에 대한 설명을 지정할 수 있습니다.

### 사용자와 계정 연결

기존 사용자를 특정 계정에 연결하려면 다음 명령을 사용합니다:

```
sacctmgr modify user <username> set account=<account_name>

```

- `<username>`: 연결할 사용자 계정의 사용자명을 지정합니다.
- `<account_name>`: 사용자 계정을 연결할 Slurm 계정 이름을 지정합니다.

### 계정 및 사용자 정보 확인

계정 및 사용자 정보를 확인하려면 다음 명령을 사용합니다:

```
sacctmgr show account
sacctmgr show user

```

`show account` 명령은 모든 계정의 정보를 표시하고, `show user` 명령은 모든 사용자의 정보를 표시합니다.

## Slurm 사용량 모니터링

Slurm은 다양한 명령과 도구를 제공하여 클러스터 자원의 사용량을 모니터링할 수 있습니다.

### squeue 명령

`squeue` 명령을 사용하여 현재 실행 중이거나 대기 중인 작업의 상태를 확인할 수 있습니다.

```
squeue

```

이 명령은 작업 ID, 작업 이름, 사용자, 작업 상태, 노드 수, 실행 시간 등의 정보를 표시합니다.

### sacct 명령

`sacct` 명령을 사용하여 작업의 계정 정보와 사용량을 확인할 수 있습니다.

```
sacct

```

이 명령은 작업 ID, 작업 이름, 사용자, 계정, 작업 상태, 시작 시간, 종료 시간, 경과 시간, CPU 시간, 메모리 사용량 등의 정보를 표시합니다.

추가로 `--starttime` 및 `--endtime` 옵션을 사용하여 특정 기간 동안의 작업 정보를 확인할 수 있습니다.

```
sacct --starttime yyyy-mm-dd --endtime yyyy-mm-dd

```

### sstat 명령

`sstat` 명령을 사용하여 실행 중인 작업의 실시간 자원 사용량을 확인할 수 있습니다.

```
sstat -j <job_id>

```

- `<job_id>`: 자원 사용량을 확인할 작업의 ID를 지정합니다.

이 명령은 작업의 CPU 사용량, 메모리 사용량, I/O 사용량 등의 실시간 정보를 표시합니다.

### sinfo 명령

`sinfo` 명령을 사용하여 클러스터의 노드 상태 및 파티션 정보를 확인할 수 있습니다.

```
sinfo

```

이 명령은 노드 이름, 노드 상태, 노드 수, CPU 수, 메모리 크기 등의 정보를 표시합니다.

### scontrol 명령

`scontrol` 명령을 사용하여 Slurm 객체(작업, 노드, 파티션 등)의 상세 정보를 확인할 수 있습니다.

```
scontrol show job <job_id>
scontrol show node <node_name>
scontrol show partition <partition_name>

```

- `<job_id>`: 상세 정보를 확인할 작업의 ID를 지정합니다.
- `<node_name>`: 상세 정보를 확인할 노드의 이름을 지정합니다.
- `<partition_name>`: 상세 정보를 확인할 파티션의 이름을 지정합니다.

이 명령은 지정된 객체의 상세한 속성 정보를 표시합니다.

## 결론

이 문서에서는 Slurm 작업 스케줄러에서 계정을 관리하고 사용량을 모니터링하는 방법에 대해 설명했습니다. `sacctmgr` 명령을 사용하여 사용자 계정과 Slurm 계정을 생성하고 관리할 수 있습니다. 또한 `squeue`, `sacct`, `sstat`, `sinfo`, `scontrol` 등의 명령을 사용하여 클러스터의 작업 상태, 자원 사용량, 노드 정보 등을 모니터링할 수 있습니다. 이러한 도구를 활용하여 Slurm 클러스터의 사용자 계정을 효과적으로 관리하고 자원 사용량을 최적화할 수 있습니다.