###### tags:`ノート` `1KG WGS`　`GATK` `演習`

### 02： 環境構築

---

#### 【作業場所、data】
Mac Studio ローカル
・ ~/Desktop/DICP/20260403_GATK_brew/data_PCR_free_high_coverage

---

### 1.　FastQC チェック

エラー


### 2.　BWAでマッピング

02_bwa.sh
```
#!/bin/bash
set -euo pipefail


# ===== 引数のチェック =====
if [ "$#" -ne 5 ]; then
    echo "Usage: $0 <SAMPLE> <RUNID> <R1.fastq.gz> <R2.fastq.gz> <REFERENCE.fasta>"
    exit 1
fi

# ===== 引数の取得 =====
SAMPLE="$1"
RUNID="$2"
R1="$3"
R2="$4"
REF="$5"

# ===== 設定 =====
THREADS=8
OUT="${SAMPLE}.sorted.bam"
LOG="align_${SAMPLE}.log"

# ===== 実行=====
echo "Start alignment: $(date)" | tee "${LOG}"

bwa mem -t "${THREADS}" \
    -R "@RG\tID:${RUNID}\tSM:${SAMPLE}\tPL:ILLUMINA" \
    "${REF}" \
    "${R1}" "${R2}" 2>> "${LOG}" | \
    samtools sort -@ "${THREADS}" -o "${OUT}" 2>> "${LOG}"

samtools index "${OUT}" 2>> "${LOG}"

echo "Finished: $(date)" | tee -a "${LOG}" 
```

実行
```
./02_bwa.sh \
NA18939 \
SRR1295537 \
SRR1295537_1.fastq.gz \
SRR1295537_2.fastq.gz \
~/Desktop/DICP/20260403_GATK_brew/reference/hg38/Homo_sapiens_assembly38.fasta
```


<br>







