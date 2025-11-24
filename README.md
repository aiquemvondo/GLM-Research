# Experiment-With-DNABERT-S

# 1 - Setup Environment :

## Clone the git repository & and create a new folder:  
```
git clone https://github.com/MAGICS-LAB/DNABERT_S.git
cd DNABERT_S
```
## Activate Anaconda:
```
source ~/anaconda3/bin/activate  
```

## Create and activate the Environment: 
```
conda create -n DNABERT_S python=3.9
conda activate DNABERT_S
```

## Install dependencies:
```
pip install -r requirements.txt
pip uninstall triton # this can lead to errors in GPUs other than A100
```

##  Install gdwon (for downloading model/data): 
```
pip install gdown 
```

## Download pretrained model (Google Drive):
```
gdown 1ejNOMXdycorDzphLT6jnfGIPUxi6fO0g 
unzip DNABERT-S.zip   
```

# 2 - Preprocess the Data and Run the experiment

## Convert fastq files: 
--> Skip this step for fasta files
```
seqtk seq -a cc4533.fastq > cc4533.fastq.fasta 
seqtk seq -a Clip185.fastq > Clip185.fastq.fasta 
seqtk seq -a LMJ.fastq > LMJ.fastq.fasta
```

## Concatenate the converted files into one:
```
cat cc4533.fastq.fasta Clip185.fastq.fasta LMJ.fastq.fasta > converted_reads.fasta 
```

## # Install visualization dependencies
```
pip install matplotlib seaborn umap-learn
```

## Generate embeddings:
```
python generate_chunk_embeddings.py  
--fasta converted_reads.fasta  
--output converted_reads_embeddings.npz 
```

## Clustering:
```
python cluster_chunks.py  
--embeddings converted_reads_embeddings.npz  
--n-clusters 3 
--output converted_reads_clusters.npz
```

## Evaluation:
```
python evaluate_clustering.py  
--clusters converted_reads_clusters.npz  
--metadata converted_reads_embeddings_metadata.csv  
--output converted_reads_evaluation.txt 
```

