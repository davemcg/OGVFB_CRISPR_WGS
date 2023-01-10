# OGVFB_CRISPR_WGS
Workflow to assess CRISPR targetting with WGS


## Step 1
Do WGS, align, create bam

## Step 2
Run [CRISPRoff](https://rth.dk/resources/crispr/crisproff/) and select the potential hits of "MAJOR", "CRITICAL", or "ONTARGET" to further assess

## Step 3

Convert output from CRISPRoff to bed file for Crispresso2 run

## Step 4

Make screenshots of all regions in IGV

  - Open IGV and load in relevant bam(s) and bed file of coordinates from CRISPRoff
  - 




# R code

```

# input CRISPRoff file
gRNA1_CRISPRoff <- read_tsv('~/Downloads/OCA2 gRNA1 region.CRISPRoff.final.tsv', skip = 8)

# write to bed for UCSC
gRNA1_CRISPRoff %>% filter(`Off-targeting level` %in% c("ONTARGET", "CRITICAL", "MAJOR")) %>% 
  mutate(ID = paste(PAM, TargetSeq, Chromosome, StartPos, EndPos, `Off-targeting level`, sep = '_')) %>% 
  select(Chromosome, StartPos, EndPos, ID) %>% 
  write_tsv("~/Desktop/gRNA1_CRISPRoff.bed", col_names = FALSE)

# use liftover on UCSC web
gRNA1_liftover <- read_tsv('~/Downloads/hglft_genome_2ea02_da9b40.bed', col_names = FALSE) %>% select(-X5) %>% unique()

# write new bed for Crispresso2 / plots
gRNA1_liftover %>% 
  mutate(X1 = gsub('chr','',X1)) %>% 
  write_tsv(file = '~/Desktop/gRNA1_liftover.bed', col_names = FALSE)

```
