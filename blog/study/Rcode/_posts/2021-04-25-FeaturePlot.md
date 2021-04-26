---
layout: post
author: "subin"
title:  "How to draw perfect feature plot with ggplot2"
subtitle: "making drawFeaturePlot function"
type: "scRNAseq"
category: "Rcode"
tags: ['single cell sequencing analysis','feature plot','seurat','ggplot2','featurePlot']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/116018464-06321380-a67d-11eb-9975-f742c1f2b5fe.png
---
<center>논문에서 예쁜 그림을 봐서 따라해봄</center>
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/116025994-b65b4880-a68c-11eb-94af-056ac7005906.jpg alt="example of feature plot">Gene expression patterns[^1] </p>


<center> **특징** </center>
- gene ecpression이 없으면 회색, 있으면 노랑에서 빨강으로 gradient를 줬다.
- 발현이 없는 cell보다 있는 cell의  크기가 큰 것 같음.
- axis 없고 제목은 gene name.


### 내 코드
▶ `Seurat object` 이름과 원하는 `gene list`,  표현하고자 하는 `method` (tSNE 또는 UMAP)을 넣으면 그려준다!

```R
#원하는 cell type 과 그 known marker gene list를 담은 vector
cell_type_1 <- c("gene1","gene2",...) 
cell_type_2 <- c("gene1","gene2",...)
..
#원하는 cell type list vector를 담은 vector
type_list = c("cell_type_1","cell_type_2",..)

drawFeaturePlot("Seurat.obj",type_list,"tsne")
#이렇게 하면 알아서 쫘라라락 그려서 저장해준다.
```

```R
drawFeaturePlot<-function(Data_name,type_list,method){
  Seurat_obj = get(Data_name)
  if(toupper(method)=="TSNE"){
		cellEmbed = Seurat_obj@reductions$tsne@cell.embeddings
  	}else if(toupper(method)=='UMAP'){
		cellEmbed = Seurat_obj@reductions$umap@cell.embeddings
  	}
  for(j in type_list){
    cellType=j
    for(i in get(cellType)){
      cols <- c("grey" ,brewer.pal(9,"YlOrRd"))
      tryCatch(
        assay_data <- GetAssayData(object = Seurat_obj)[i,],
        error = function(e) print(paste0("no ",i)),
        warning = function(w) print(paste0("no ",i)))
        df = data.frame(
            x=cellEmbed[, 1], 
            y=cellEmbed[, 2], 
            expression=assay_data)
        df$alpha <- 1
        df[df$expression!=0,"alpha"] <- 0.5
        df$size <- 1
        df[df$expression!=0,"size"] <- 2.5
        data<-df[order(df$expression, decreasing=FALSE),]
        plot <- ggplot(data,aes(x=x, y=y, colour=expression),mar=c(0,0,3,0)) + 
          ggtitle(paste0(cellType,"_",i)) +
            geom_point(size=data$size, alpha=data$alpha, shape=19) + 
            scale_colour_gradientn(colours = cols) +
            ylab(paste0(toupper(method),"_2")) + xlab(paste0(toupper(method),"_1")) + 
          theme_classic() + theme(legend.position="none") +
            theme(text = element_text(size=20),
            panel.grid.major=element_blank(),
            panel.grid.minor=element_blank(), 
            axis.line=element_line(size=1),
            axis.ticks=element_line(size=1),
            legend.text=element_text(size=5), 
            legend.title=element_blank(),
            legend.key=element_blank(),
            axis.text.x = element_text(size=5)) 
        ggsave(file=paste0(Data_name,"_",cellType,"_",i,"_",toupper(method),".png"),plot)
    }}}
```

<br/><br/><br/><br/><br/>



[^1]:Rosenberg, Alexander B., et al. ["Single-cell profiling of the developing mouse brain and spinal cord with split-pool barcoding."]((https://science.sciencemag.org/content/360/6385/176)) Science 360.6385 (2018): 176-182. 