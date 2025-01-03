# UniFrac Embedding based on Approxiamte Nearest Neighbor Graph
UniFrac is a widely used distance metric (1,2,3) to provide quantitive information about ecological communities considering evolutionary histories of taxa in the community. Here, we combine UniFrac with non-linear dimension reduction methods (or embedding) to visualizing large-scale microbiome data (e.g., millions of samples). Specifically, we use annembed (4) as the underlying embedding algorithm, which is an ultra-fast and  highly efficient embedding method for large-scale datasets. We implemented the Earth Mover Distance-based UniFrac(5), which is equivalent to original UniFrac but easy to parallelize. Stripe UniFrac can also be used (2).

## Quick install
```bash
##Linux
wget https://github.com/jianshu93/unifeb/releases/download/v0.1.0/unifeb_Linux_x86-64_v0.1.0.zip
unzip unifeb_Linux_x86-64_v0.1.0.zip
chmod a+x ./unifeb
./unifeb -h
```

## Compiling from source (Linux only)
```bash
git clone https://github.com/jianshu93/unifeb
cd unifeb
cargo build --release
./target/release/unifeb -h
```

## Usage
```bash

 ************** initializing logger *****************

UniFrac Embedding via Approxiamte Nearest Neighbor Graph

Usage: unifeb [OPTIONS] --tree <tree> --feature-table <featuretable> [COMMAND]

Commands:
  hnsw  Build HNSW/HubNSW graph
  help  Print this message or the help of the given subcommand(s)

Options:
  -t, --tree <tree>                   Newick tree filename
  -f, --feature-table <featuretable>  Feature table with rows=features, columns=samples
      --weighted                      Use Weighted UniFrac (otherwise unweighted)
  -o, --out <outfile>                 Output CSV file for embedded results [default: embedded.csv]
      --batch <batch>                 Number of gradient batches [default: 20]
      --nbsample <nbsample>           Number of edge samplings per batch [default: 10]
  -l, --layer <hierarchy>             If >0, use hierarchical approach in embedding [default: 0]
      --scale <scale>                 Rho scale factor for the gradient descent [default: 1.0]
  -d, --dim <dimension>               Dimension of embedding [default: 2]
  -q, --quality <quality>             Sampling fraction for quality estimation, <=1.0
  -h, --help                          Print help
  -V, --version                       Print version
```
Running test data:

```bash
### Each taxa in the feature table must be found in the tree as leaf/tip.
./target/release/unifeb -t ./data/ASVs_aligned.tre -f ./data/ASV_counts.txt -o embedded.csv hnsw --nbconn 48 --ef 400 --knbn 15 --scale_modify_f 0.25
```

## Generating OTU/ASV table and tree
We recommend either USEARCH or DADA2 based methods, which we wrapped as a single-command pipeline [here](https://github.com/jianshu93/usearch_wrapper) and [here](https://github.com/jianshu93/dada2_wrapper) for large-scale datasets. For datasets with more than a few hundred samples, the USEARCH pipeline is preferred since it is more computationally efficient. Other wrappers such as QIIME2 or LOTUS are also possible options.



## References
1.Lozupone, C. and Knight, R., 2005. UniFrac: a new phylogenetic method for comparing microbial communities. Applied and environmental microbiology, 71(12), pp.8228-8235.

2.McDonald, D., Vázquez-Baeza, Y., Koslicki, D., McClelland, J., Reeve, N., Xu, Z., Gonzalez, A. and Knight, R., 2018. Striped UniFrac: enabling microbiome analysis at unprecedented scale. Nature methods, 15(11), pp.847-848.

3.Hamady, M., Lozupone, C. and Knight, R., 2010. Fast UniFrac: facilitating high-throughput phylogenetic analyses of microbial communities including analysis of pyrosequencing and PhyloChip data. The ISME journal, 4(1), pp.17-27.

4.Jianshu Zhao, Jean Pierre Both, Konstantinos T Konstantinidis, Approximate nearest neighbor graph provides fast and efficient embedding with applications for large-scale biological data, NAR Genomics and Bioinformatics, Volume 6, Issue 4, December 2024, lqae172, https://doi.org/10.1093/nargab/lqae172

5.McClelland, J. and Koslicki, D., 2018. EMDUniFrac: exact linear time computation of the UniFrac metric and identification of differentially abundant organisms. Journal of mathematical biology, 77, pp.935-949.
