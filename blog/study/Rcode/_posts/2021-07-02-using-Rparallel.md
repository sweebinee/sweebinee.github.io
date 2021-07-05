---
layout: post
author: "subin"
title:  "Processing with multi cores in R"
subtitle: "Rparallel library"
type: "code"
category: "Rcode"
tags: ['R','Rparallel','using R with multi cores environment','parallel processing']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/116018464-06321380-a67d-11eb-9975-f742c1f2b5fe.png
---
Gene set이 Human gene으로 되어 있는데 Mouse gene으로 바꾸고 싶을때, 또는 반대의 경우.<br/>
`biomaRt` library 사용하므로, 인터넷 연결되어 있어야함.

## HumantoMouse

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
이건 function. 아래같이 쓰면 된다. 
```R
m.s.genes <- convertHumanGeneList(cc.genes$s.genes)
m.g2m.genes <- convertHumanGeneList(cc.genes$g2m.genes)
```
<br/>
## MousetoHuman
```R
genesV2 = getLDS(attributes = c("mgi_symbol"), filters = "mgi_symbol", values = x , mart = mouse, attributesL = c("hgnc_symbol"), martL = human, uniqueRows=T)
```

<br/><br/><br/><br/><br/>
reference: [github HumantoMouse](https://github.com/corticaltone/HumantoMouse)
