---
layout: post
author: "subin"
title:  "Converting Human gene symbol to Mouse gene"
subtitle: ""
type: "code"
category: "Rcode"
tags: ['R','bioMart','how to change human gene to mouse gene']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/116018464-06321380-a67d-11eb-9975-f742c1f2b5fe.png
---

```R
convertHumanGeneList <- function(x){
	require("biomaRt")
	human = useMart("ensembl", dataset = "hsapiens_gene_ensembl")
	mouse = useMart("ensembl", dataset = "mmusculus_gene_ensembl")
	genesV2 = getLDS(attributes = c("hgnc_symbol"), filters = "hgnc_symbol", values = x , mart = human, attributesL = c("mgi_symbol"), martL = mouse, uniqueRows=T)
	humanx <- unique(genesV2[, 2])
	# Print the first 6 genes found to the screen
	print(head(humanx))
	return(humanx)
}
```

<br/><br/><br/><br/><br/>

