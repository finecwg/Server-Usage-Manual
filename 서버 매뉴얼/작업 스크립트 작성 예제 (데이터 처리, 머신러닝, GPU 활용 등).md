# 작업 스크립트 작성 예제 (데이터 처리, 머신러닝, GPU 활용 등)

# 7.4. 작업 스크립트 작성 예제 (데이터 처리, 머신러닝, GPU 활용 등)

Slurm에서 다양한 유형의 작업을 실행하려면 작업의 특성에 맞는 작업 스크립트를 작성해야 합니다. 이 문서에서는 데이터 처리, 머신러닝, GPU 활용 등 다양한 시나리오에 대한 작업 스크립트 작성 예제를 제공하겠습니다.

## 데이터 처리 작업 스크립트 예제

데이터 처리 작업은 대용량 데이터를 분석하거나 변환하는 작업입니다. 다음은 데이터 처리 작업 스크립트의 예시입니다.

```bash
#!/bin/bash
#SBATCH --job-name=data_processing
#SBATCH --output=data_processing.out
#SBATCH --error=data_processing.err
#SBATCH --time=04:00:00
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=16
#SBATCH --partition=compute

# 데이터 파일 경로
DATA_DIR=/path/to/data

# 처리 결과 저장 경로
RESULT_DIR=/path/to/result

# 데이터 처리 스크립트 실행
srun --ntasks=64 python process_data.py $DATA_DIR $RESULT_DIR

```

이 작업 스크립트는 4개의 노드에서 노드당 16개의 태스크를 사용하여 데이터 처리 스크립트 `process_data.py`를 실행합니다. `DATA_DIR`과 `RESULT_DIR` 변수를 사용하여 입력 데이터 파일 경로와 처리 결과 저장 경로를 지정합니다.

## 머신러닝 작업 스크립트 예제

머신러닝 작업은 모델 학습, 평가, 예측 등을 수행하는 작업입니다. 다음은 머신러닝 작업 스크립트의 예시입니다.

```bash
#!/bin/bash
#SBATCH --job-name=ml_training
#SBATCH --output=ml_training.out
#SBATCH --error=ml_training.err
#SBATCH --time=08:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=16
#SBATCH --mem=64G
#SBATCH --partition=ml

# 데이터셋 경로
DATASET_DIR=/path/to/dataset

# 모델 저장 경로
MODEL_DIR=/path/to/model

# 머신러닝 스크립트 실행
python train_model.py --dataset $DATASET_DIR --model_dir $MODEL_DIR --epochs 50 --batch_size 128

```

이 작업 스크립트는 1개의 노드에서 16개의 CPU 코어와 64GB의 메모리를 사용하여 머신러닝 스크립트 `train_model.py`를 실행합니다. `DATASET_DIR`과 `MODEL_DIR` 변수를 사용하여 데이터셋 경로와 모델 저장 경로를 지정합니다. 스크립트 실행 시 에포크 수(`--epochs`)와 배치 크기(`--batch_size`)를 인자로 전달합니다.

## GPU 활용 작업 스크립트 예제

GPU 활용 작업은 딥 러닝, 과학 컴퓨팅 등에서 GPU 가속을 사용하는 작업입니다. 다음은 GPU 활용 작업 스크립트의 예시입니다.

```bash
#!/bin/bash
#SBATCH --job-name=gpu_job
#SBATCH --output=gpu_job.out
#SBATCH --error=gpu_job.err
#SBATCH --time=12:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --gres=gpu:4
#SBATCH --partition=gpu

# 데이터셋 경로
DATASET_DIR=/path/to/dataset

# 모델 저장 경로
MODEL_DIR=/path/to/model

# GPU 작업 스크립트 실행
python train_model_gpu.py --dataset $DATASET_DIR --model_dir $MODEL_DIR --batch_size 64 --learning_rate 0.001

```

이 작업 스크립트는 1개의 노드에서 4개의 GPU를 사용하여 GPU 작업 스크립트 `train_model_gpu.py`를 실행합니다. `--gres=gpu:4` 옵션을 사용하여 작업에 할당할 GPU 수를 지정합니다. `DATASET_DIR`과 `MODEL_DIR` 변수를 사용하여 데이터셋 경로와 모델 저장 경로를 지정합니다. 스크립트 실행 시 배치 크기(`--batch_size`)와 학습률(`--learning_rate`)을 인자로 전달합니다.

## MPI 작업 스크립트 예제

MPI(Message Passing Interface)는 분산 메모리 시스템에서 병렬 프로그래밍을 위한 라이브러리입니다. 다음은 MPI 작업 스크립트의 예시입니다.

```bash
#!/bin/bash
#SBATCH --job-name=mpi_job
#SBATCH --output=mpi_job.out
#SBATCH --error=mpi_job.err
#SBATCH --time=06:00:00
#SBATCH --nodes=8
#SBATCH --ntasks-per-node=16
#SBATCH --partition=mpi

# MPI 프로그램 실행
mpirun -np 128 ./mpi_program

```

이 작업 스크립트는 8개의 노드에서 노드당 16개의 태스크를 사용하여 MPI 프로그램 `mpi_program`을 실행합니다. `mpirun` 명령을 사용하여 총 128개의 프로세스로 MPI 프로그램을 실행합니다.

## 작업 배열 스크립트 예제

작업 배열은 유사한 작업을 배열 형태로 제출하여 효율적으로 처리할 수 있습니다. 다음은 작업 배열 스크립트의 예시입니다.

```bash
#!/bin/bash
#SBATCH --job-name=array_job
#SBATCH --output=array_job_%A_%a.out
#SBATCH --error=array_job_%A_%a.err
#SBATCH --time=01:00:00
#SBATCH --array=1-100
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --partition=compute

# 작업 배열 인덱스
INDEX=$SLURM_ARRAY_TASK_ID

# 데이터 파일 경로
DATA_FILE=/path/to/data/data_${INDEX}.txt

# 처리 결과 저장 경로
RESULT_FILE=/path/to/result/result_${INDEX}.txt

# 데이터 처리 스크립트 실행
python process_data.py $DATA_FILE $RESULT_FILE

```

이 작업 배열 스크립트는 1부터 100까지의 인덱스를 가진 100개의 작업을 생성합니다. 각 작업은 `$SLURM_ARRAY_TASK_ID` 변수를 사용하여 고유한 인덱스를 할당받습니다. `DATA_FILE`과 `RESULT_FILE` 변수를 사용하여 입력 데이터 파일 경로와 처리 결과 저장 경로를 인덱스에 따라 동적으로 생성합니다.

## 결론

이 문서에서는 Slurm에서 다양한 유형의 작업을 실행하기 위한 작업 스크립트 작성 예제를 제공했습니다. 데이터 처리, 머신러닝, GPU 활용, MPI, 작업 배열 등의 시나리오에 대한 작업 스크립트를 살펴보았습니다. 작업 스크립트에서는 작업의 특성에 맞는 자원 요구 사항, 실행 명령, 입출력 파일 경로 등을 지정합니다. 이러한 예제를 참고하여 실제 작업에 맞는 작업 스크립트를 작성하고 Slurm 클러스터에서 효과적으로 작업을 실행할 수 있습니다.