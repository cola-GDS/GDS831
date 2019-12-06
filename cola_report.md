cola Report for Consensus Partitioning
==================

**Date**: 2019-12-04 09:13:52 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21586    54
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:hclust](#SD-hclust)     |          2| 1.000|           0.980|       0.982|** |           |
|[SD:pam](#SD-pam)           |          2| 1.000|           0.966|       0.987|** |           |
|[CV:hclust](#CV-hclust)     |          2| 1.000|           0.988|       0.986|** |           |
|[MAD:hclust](#MAD-hclust)   |          2| 1.000|           1.000|       1.000|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 1.000|           0.983|       0.980|** |           |
|[MAD:pam](#MAD-pam)         |          3| 1.000|           0.968|       0.986|** |2          |
|[ATC:kmeans](#ATC-kmeans)   |          3| 1.000|           0.976|       0.967|** |           |
|[ATC:skmeans](#ATC-skmeans) |          3| 1.000|           0.988|       0.995|** |           |
|[ATC:pam](#ATC-pam)         |          3| 1.000|           0.973|       0.991|** |2          |
|[MAD:mclust](#MAD-mclust)   |          3| 0.995|           0.953|       0.975|** |           |
|[CV:NMF](#CV-NMF)           |          3| 0.970|           0.941|       0.974|** |2          |
|[ATC:NMF](#ATC-NMF)         |          3| 0.970|           0.935|       0.975|** |           |
|[MAD:NMF](#MAD-NMF)         |          3| 0.969|           0.947|       0.981|** |2          |
|[SD:NMF](#SD-NMF)           |          5| 0.959|           0.920|       0.960|** |2,3        |
|[CV:kmeans](#CV-kmeans)     |          3| 0.955|           0.963|       0.956|** |           |
|[CV:mclust](#CV-mclust)     |          4| 0.949|           0.927|       0.962|*  |           |
|[CV:skmeans](#CV-skmeans)   |          3| 0.944|           0.935|       0.974|*  |           |
|[MAD:skmeans](#MAD-skmeans) |          3| 0.941|           0.919|       0.968|*  |2          |
|[SD:skmeans](#SD-skmeans)   |          3| 0.940|           0.902|       0.966|*  |2          |
|[CV:pam](#CV-pam)           |          6| 0.928|           0.905|       0.946|*  |2,3,4      |
|[ATC:hclust](#ATC-hclust)   |          5| 0.922|           0.964|       0.971|*  |2,3,4      |
|[SD:mclust](#SD-mclust)     |          2| 0.849|           0.901|       0.942|   |           |
|[ATC:mclust](#ATC-mclust)   |          3| 0.764|           0.884|       0.942|   |           |
|[SD:kmeans](#SD-kmeans)     |          3| 0.750|           0.968|       0.950|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           0.972       0.988          0.383 0.628   0.628
#&gt; CV:NMF      2 1.000           0.973       0.988          0.384 0.628   0.628
#&gt; MAD:NMF     2 0.962           0.959       0.982          0.383 0.628   0.628
#&gt; ATC:NMF     2 0.851           0.900       0.956          0.411 0.609   0.609
#&gt; SD:skmeans  2 1.000           1.000       1.000          0.485 0.516   0.516
#&gt; CV:skmeans  2 0.730           0.918       0.955          0.475 0.516   0.516
#&gt; MAD:skmeans 2 1.000           0.969       0.982          0.486 0.516   0.516
#&gt; ATC:skmeans 2 0.739           0.857       0.930          0.458 0.508   0.508
#&gt; SD:mclust   2 0.849           0.901       0.942          0.479 0.525   0.525
#&gt; CV:mclust   2 0.476           0.849       0.912          0.467 0.525   0.525
#&gt; MAD:mclust  2 0.851           0.925       0.958          0.412 0.560   0.560
#&gt; ATC:mclust  2 0.704           0.887       0.943          0.421 0.591   0.591
#&gt; SD:kmeans   2 0.437           0.730       0.832          0.347 0.535   0.535
#&gt; CV:kmeans   2 0.443           0.830       0.861          0.342 0.669   0.669
#&gt; MAD:kmeans  2 0.399           0.798       0.852          0.366 0.669   0.669
#&gt; ATC:kmeans  2 0.500           0.815       0.847          0.348 0.648   0.648
#&gt; SD:pam      2 1.000           0.966       0.987          0.346 0.669   0.669
#&gt; CV:pam      2 1.000           0.984       0.994          0.339 0.669   0.669
#&gt; MAD:pam     2 1.000           0.955       0.982          0.352 0.669   0.669
#&gt; ATC:pam     2 1.000           1.000       1.000          0.331 0.669   0.669
#&gt; SD:hclust   2 1.000           0.980       0.982          0.337 0.669   0.669
#&gt; CV:hclust   2 1.000           0.988       0.986          0.334 0.669   0.669
#&gt; MAD:hclust  2 1.000           1.000       1.000          0.331 0.669   0.669
#&gt; ATC:hclust  2 0.961           0.922       0.972          0.374 0.648   0.648
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 1.000           0.978       0.990          0.536 0.740   0.599
#&gt; CV:NMF      3 0.970           0.941       0.974          0.570 0.740   0.599
#&gt; MAD:NMF     3 0.969           0.947       0.981          0.562 0.728   0.580
#&gt; ATC:NMF     3 0.970           0.935       0.975          0.468 0.720   0.568
#&gt; SD:skmeans  3 0.940           0.902       0.966          0.278 0.787   0.613
#&gt; CV:skmeans  3 0.944           0.935       0.974          0.340 0.764   0.576
#&gt; MAD:skmeans 3 0.941           0.919       0.968          0.303 0.764   0.576
#&gt; ATC:skmeans 3 1.000           0.988       0.995          0.363 0.810   0.645
#&gt; SD:mclust   3 0.757           0.881       0.933          0.326 0.735   0.539
#&gt; CV:mclust   3 0.744           0.906       0.946          0.358 0.717   0.513
#&gt; MAD:mclust  3 0.995           0.953       0.975          0.523 0.723   0.541
#&gt; ATC:mclust  3 0.764           0.884       0.942          0.462 0.797   0.657
#&gt; SD:kmeans   3 0.750           0.968       0.950          0.578 0.881   0.783
#&gt; CV:kmeans   3 0.955           0.963       0.956          0.624 0.769   0.656
#&gt; MAD:kmeans  3 1.000           0.983       0.980          0.529 0.769   0.656
#&gt; ATC:kmeans  3 1.000           0.976       0.967          0.523 0.762   0.645
#&gt; SD:pam      3 0.887           0.939       0.975          0.581 0.786   0.681
#&gt; CV:pam      3 0.968           0.971       0.987          0.639 0.786   0.681
#&gt; MAD:pam     3 1.000           0.968       0.986          0.574 0.786   0.681
#&gt; ATC:pam     3 1.000           0.973       0.991          0.619 0.786   0.681
#&gt; SD:hclust   3 0.835           0.929       0.966          0.745 0.727   0.593
#&gt; CV:hclust   3 0.863           0.912       0.951          0.666 0.786   0.681
#&gt; MAD:hclust  3 0.841           0.894       0.950          0.744 0.716   0.576
#&gt; ATC:hclust  3 0.937           0.912       0.968          0.496 0.810   0.707
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.779           0.796       0.885         0.1837 0.859   0.660
#&gt; CV:NMF      4 0.700           0.761       0.871         0.2014 0.839   0.614
#&gt; MAD:NMF     4 0.753           0.790       0.892         0.1742 0.883   0.717
#&gt; ATC:NMF     4 0.737           0.773       0.884         0.1750 0.848   0.638
#&gt; SD:skmeans  4 0.800           0.693       0.871         0.2143 0.827   0.564
#&gt; CV:skmeans  4 0.707           0.682       0.861         0.1839 0.822   0.544
#&gt; MAD:skmeans 4 0.858           0.841       0.931         0.1905 0.834   0.569
#&gt; ATC:skmeans 4 0.747           0.775       0.849         0.1681 0.827   0.561
#&gt; SD:mclust   4 0.847           0.923       0.947         0.0438 0.888   0.731
#&gt; CV:mclust   4 0.949           0.927       0.962         0.0228 0.863   0.678
#&gt; MAD:mclust  4 0.649           0.702       0.785         0.1369 0.762   0.452
#&gt; ATC:mclust  4 0.687           0.580       0.793         0.1721 0.828   0.591
#&gt; SD:kmeans   4 0.715           0.766       0.868         0.2145 0.919   0.815
#&gt; CV:kmeans   4 0.715           0.736       0.861         0.2103 0.919   0.815
#&gt; MAD:kmeans  4 0.682           0.712       0.795         0.2510 0.811   0.570
#&gt; ATC:kmeans  4 0.707           0.717       0.861         0.2532 0.935   0.857
#&gt; SD:pam      4 0.727           0.720       0.877         0.3041 0.809   0.581
#&gt; CV:pam      4 0.938           0.925       0.967         0.3303 0.801   0.563
#&gt; MAD:pam     4 0.768           0.794       0.834         0.2047 0.816   0.595
#&gt; ATC:pam     4 0.711           0.822       0.801         0.1611 1.000   1.000
#&gt; SD:hclust   4 0.838           0.927       0.959         0.0755 0.975   0.937
#&gt; CV:hclust   4 0.767           0.871       0.892         0.1742 0.855   0.681
#&gt; MAD:hclust  4 0.782           0.820       0.912         0.1627 0.969   0.918
#&gt; ATC:hclust  4 1.000           0.973       0.987         0.0986 0.935   0.858
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.959           0.920       0.960         0.0866 0.943   0.804
#&gt; CV:NMF      5 0.777           0.744       0.880         0.0795 0.899   0.650
#&gt; MAD:NMF     5 0.811           0.807       0.896         0.1168 0.878   0.625
#&gt; ATC:NMF     5 0.700           0.723       0.839         0.0851 0.874   0.604
#&gt; SD:skmeans  5 0.827           0.808       0.884         0.0718 0.898   0.621
#&gt; CV:skmeans  5 0.796           0.796       0.886         0.0707 0.936   0.744
#&gt; MAD:skmeans 5 0.743           0.670       0.820         0.0659 0.915   0.673
#&gt; ATC:skmeans 5 0.734           0.577       0.759         0.0647 0.906   0.657
#&gt; SD:mclust   5 0.775           0.751       0.877         0.1397 0.857   0.602
#&gt; CV:mclust   5 0.803           0.849       0.918         0.1825 0.839   0.565
#&gt; MAD:mclust  5 0.731           0.543       0.737         0.0693 0.783   0.439
#&gt; ATC:mclust  5 0.716           0.738       0.827         0.0279 0.955   0.847
#&gt; SD:kmeans   5 0.676           0.569       0.716         0.1225 0.971   0.921
#&gt; CV:kmeans   5 0.707           0.794       0.832         0.1168 0.843   0.571
#&gt; MAD:kmeans  5 0.656           0.657       0.766         0.0832 0.919   0.722
#&gt; ATC:kmeans  5 0.657           0.803       0.861         0.0951 0.867   0.664
#&gt; SD:pam      5 0.700           0.582       0.790         0.0607 0.893   0.646
#&gt; CV:pam      5 0.868           0.877       0.930         0.0550 0.947   0.796
#&gt; MAD:pam     5 0.755           0.742       0.877         0.1404 0.913   0.703
#&gt; ATC:pam     5 0.671           0.752       0.853         0.1351 0.846   0.666
#&gt; SD:hclust   5 0.771           0.916       0.935         0.1064 0.927   0.805
#&gt; CV:hclust   5 0.799           0.873       0.912         0.0671 0.980   0.938
#&gt; MAD:hclust  5 0.835           0.847       0.915         0.0695 0.930   0.803
#&gt; ATC:hclust  5 0.922           0.964       0.971         0.1482 0.895   0.733
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.805           0.787       0.868         0.0844 0.916   0.661
#&gt; CV:NMF      6 0.745           0.570       0.753         0.0527 0.905   0.595
#&gt; MAD:NMF     6 0.767           0.671       0.822         0.0526 0.886   0.537
#&gt; ATC:NMF     6 0.714           0.670       0.812         0.0452 0.958   0.823
#&gt; SD:skmeans  6 0.816           0.731       0.841         0.0381 0.950   0.751
#&gt; CV:skmeans  6 0.805           0.734       0.838         0.0380 0.960   0.798
#&gt; MAD:skmeans 6 0.770           0.609       0.784         0.0398 0.930   0.671
#&gt; ATC:skmeans 6 0.787           0.832       0.889         0.0516 0.916   0.651
#&gt; SD:mclust   6 0.795           0.802       0.886         0.0540 0.962   0.824
#&gt; CV:mclust   6 0.853           0.861       0.917         0.0533 0.961   0.823
#&gt; MAD:mclust  6 0.842           0.857       0.922         0.0478 0.832   0.493
#&gt; ATC:mclust  6 0.729           0.780       0.822         0.0276 0.878   0.592
#&gt; SD:kmeans   6 0.677           0.707       0.761         0.0630 0.828   0.515
#&gt; CV:kmeans   6 0.695           0.668       0.792         0.0593 0.974   0.886
#&gt; MAD:kmeans  6 0.726           0.679       0.788         0.0651 0.910   0.669
#&gt; ATC:kmeans  6 0.734           0.717       0.803         0.0640 0.998   0.992
#&gt; SD:pam      6 0.810           0.773       0.901         0.0422 0.881   0.573
#&gt; CV:pam      6 0.928           0.905       0.946         0.0331 0.986   0.934
#&gt; MAD:pam     6 0.752           0.573       0.757         0.0580 0.874   0.525
#&gt; ATC:pam     6 0.770           0.777       0.895         0.0874 0.954   0.853
#&gt; SD:hclust   6 0.877           0.939       0.963         0.0473 0.986   0.953
#&gt; CV:hclust   6 0.789           0.704       0.750         0.0866 0.957   0.867
#&gt; MAD:hclust  6 0.891           0.912       0.959         0.0281 0.986   0.951
#&gt; ATC:hclust  6 0.951           0.954       0.949         0.0208 0.989   0.961
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      54     0.398 2
#&gt; CV:NMF      54     0.398 2
#&gt; MAD:NMF     53     0.397 2
#&gt; ATC:NMF     52     0.396 2
#&gt; SD:skmeans  54     0.398 2
#&gt; CV:skmeans  54     0.398 2
#&gt; MAD:skmeans 54     0.398 2
#&gt; ATC:skmeans 53     0.397 2
#&gt; SD:mclust   52     0.396 2
#&gt; CV:mclust   54     0.398 2
#&gt; MAD:mclust  54     0.398 2
#&gt; ATC:mclust  54     0.398 2
#&gt; SD:kmeans   44     0.387 2
#&gt; CV:kmeans   54     0.398 2
#&gt; MAD:kmeans  46     0.389 2
#&gt; ATC:kmeans  46     0.389 2
#&gt; SD:pam      53     0.397 2
#&gt; CV:pam      53     0.397 2
#&gt; MAD:pam     53     0.397 2
#&gt; ATC:pam     54     0.398 2
#&gt; SD:hclust   54     0.398 2
#&gt; CV:hclust   54     0.398 2
#&gt; MAD:hclust  54     0.398 2
#&gt; ATC:hclust  51     0.395 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      54     0.374 3
#&gt; CV:NMF      53     0.373 3
#&gt; MAD:NMF     53     0.373 3
#&gt; ATC:NMF     52     0.372 3
#&gt; SD:skmeans  51     0.371 3
#&gt; CV:skmeans  53     0.373 3
#&gt; MAD:skmeans 52     0.372 3
#&gt; ATC:skmeans 54     0.374 3
#&gt; SD:mclust   53     0.443 3
#&gt; CV:mclust   53     0.443 3
#&gt; MAD:mclust  53     0.373 3
#&gt; ATC:mclust  54     0.374 3
#&gt; SD:kmeans   54     0.374 3
#&gt; CV:kmeans   54     0.374 3
#&gt; MAD:kmeans  54     0.374 3
#&gt; ATC:kmeans  54     0.374 3
#&gt; SD:pam      53     0.373 3
#&gt; CV:pam      54     0.374 3
#&gt; MAD:pam     53     0.373 3
#&gt; ATC:pam     53     0.373 3
#&gt; SD:hclust   54     0.374 3
#&gt; CV:hclust   53     0.373 3
#&gt; MAD:hclust  49     0.368 3
#&gt; ATC:hclust  51     0.371 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      48     0.510 4
#&gt; CV:NMF      45     0.504 4
#&gt; MAD:NMF     49     0.348 4
#&gt; ATC:NMF     47     0.344 4
#&gt; SD:skmeans  41     0.407 4
#&gt; CV:skmeans  40     0.406 4
#&gt; MAD:skmeans 50     0.432 4
#&gt; ATC:skmeans 50     0.416 4
#&gt; SD:mclust   54     0.523 4
#&gt; CV:mclust   53     0.520 4
#&gt; MAD:mclust  46     0.427 4
#&gt; ATC:mclust  36     0.411 4
#&gt; SD:kmeans   51     0.516 4
#&gt; CV:kmeans   48     0.508 4
#&gt; MAD:kmeans  46     0.428 4
#&gt; ATC:kmeans  48     0.346 4
#&gt; SD:pam      47     0.422 4
#&gt; CV:pam      52     0.425 4
#&gt; MAD:pam     50     0.426 4
#&gt; ATC:pam     53     0.373 4
#&gt; SD:hclust   54     0.355 4
#&gt; CV:hclust   53     0.447 4
#&gt; MAD:hclust  49     0.348 4
#&gt; ATC:hclust  54     0.355 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      53     0.440 5
#&gt; CV:NMF      45     0.441 5
#&gt; MAD:NMF     49     0.413 5
#&gt; ATC:NMF     46     0.467 5
#&gt; SD:skmeans  50     0.439 5
#&gt; CV:skmeans  50     0.443 5
#&gt; MAD:skmeans 36     0.411 5
#&gt; ATC:skmeans 42     0.399 5
#&gt; SD:mclust   47     0.404 5
#&gt; CV:mclust   52     0.409 5
#&gt; MAD:mclust  28     0.388 5
#&gt; ATC:mclust  48     0.405 5
#&gt; SD:kmeans   37     0.402 5
#&gt; CV:kmeans   51     0.497 5
#&gt; MAD:kmeans  42     0.408 5
#&gt; ATC:kmeans  52     0.334 5
#&gt; SD:pam      37     0.393 5
#&gt; CV:pam      53     0.481 5
#&gt; MAD:pam     46     0.393 5
#&gt; ATC:pam     50     0.331 5
#&gt; SD:hclust   54     0.483 5
#&gt; CV:hclust   53     0.480 5
#&gt; MAD:hclust  53     0.481 5
#&gt; ATC:hclust  54     0.337 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      52     0.430 6
#&gt; CV:NMF      34     0.430 6
#&gt; MAD:NMF     40     0.410 6
#&gt; ATC:NMF     42     0.457 6
#&gt; SD:skmeans  42     0.408 6
#&gt; CV:skmeans  45     0.419 6
#&gt; MAD:skmeans 38     0.442 6
#&gt; ATC:skmeans 52     0.391 6
#&gt; SD:mclust   50     0.400 6
#&gt; CV:mclust   52     0.402 6
#&gt; MAD:mclust  50     0.400 6
#&gt; ATC:mclust  48     0.438 6
#&gt; SD:kmeans   46     0.450 6
#&gt; CV:kmeans   46     0.479 6
#&gt; MAD:kmeans  48     0.485 6
#&gt; ATC:kmeans  50     0.407 6
#&gt; SD:pam      48     0.458 6
#&gt; CV:pam      53     0.448 6
#&gt; MAD:pam     32     0.395 6
#&gt; ATC:pam     49     0.314 6
#&gt; SD:hclust   54     0.450 6
#&gt; CV:hclust   44     0.410 6
#&gt; MAD:hclust  53     0.448 6
#&gt; ATC:hclust  54     0.322 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.980       0.982         0.3369 0.669   0.669
#> 3 3 0.835           0.929       0.966         0.7449 0.727   0.593
#> 4 4 0.838           0.927       0.959         0.0755 0.975   0.937
#> 5 5 0.771           0.916       0.935         0.1064 0.927   0.805
#> 6 6 0.877           0.939       0.963         0.0473 0.986   0.953
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2   0.000      0.985 0.000 1.000
#&gt; GSM28763     2   0.000      0.985 0.000 1.000
#&gt; GSM28764     2   0.000      0.985 0.000 1.000
#&gt; GSM11274     2   0.204      0.972 0.032 0.968
#&gt; GSM28772     1   0.204      1.000 0.968 0.032
#&gt; GSM11269     1   0.204      1.000 0.968 0.032
#&gt; GSM28775     1   0.204      1.000 0.968 0.032
#&gt; GSM11293     1   0.204      1.000 0.968 0.032
#&gt; GSM28755     1   0.204      1.000 0.968 0.032
#&gt; GSM11279     1   0.204      1.000 0.968 0.032
#&gt; GSM28758     1   0.204      1.000 0.968 0.032
#&gt; GSM11281     1   0.204      1.000 0.968 0.032
#&gt; GSM11287     1   0.204      1.000 0.968 0.032
#&gt; GSM28759     1   0.204      1.000 0.968 0.032
#&gt; GSM11292     2   0.000      0.985 0.000 1.000
#&gt; GSM28766     2   0.000      0.985 0.000 1.000
#&gt; GSM11268     2   0.204      0.972 0.032 0.968
#&gt; GSM28767     2   0.000      0.985 0.000 1.000
#&gt; GSM11286     2   0.000      0.985 0.000 1.000
#&gt; GSM28751     2   0.000      0.985 0.000 1.000
#&gt; GSM28770     2   0.000      0.985 0.000 1.000
#&gt; GSM11283     2   0.000      0.985 0.000 1.000
#&gt; GSM11289     2   0.000      0.985 0.000 1.000
#&gt; GSM11280     2   0.000      0.985 0.000 1.000
#&gt; GSM28749     2   0.000      0.985 0.000 1.000
#&gt; GSM28750     2   0.204      0.972 0.032 0.968
#&gt; GSM11290     2   0.204      0.972 0.032 0.968
#&gt; GSM11294     2   0.204      0.972 0.032 0.968
#&gt; GSM28771     2   0.000      0.985 0.000 1.000
#&gt; GSM28760     2   0.000      0.985 0.000 1.000
#&gt; GSM28774     2   0.000      0.985 0.000 1.000
#&gt; GSM11284     2   0.000      0.985 0.000 1.000
#&gt; GSM28761     2   0.204      0.972 0.032 0.968
#&gt; GSM11278     2   0.204      0.972 0.032 0.968
#&gt; GSM11291     2   0.204      0.972 0.032 0.968
#&gt; GSM11277     2   0.204      0.972 0.032 0.968
#&gt; GSM11272     2   0.204      0.972 0.032 0.968
#&gt; GSM11285     2   0.000      0.985 0.000 1.000
#&gt; GSM28753     2   0.000      0.985 0.000 1.000
#&gt; GSM28773     2   0.000      0.985 0.000 1.000
#&gt; GSM28765     2   0.000      0.985 0.000 1.000
#&gt; GSM28768     2   0.730      0.733 0.204 0.796
#&gt; GSM28754     2   0.000      0.985 0.000 1.000
#&gt; GSM28769     2   0.000      0.985 0.000 1.000
#&gt; GSM11275     1   0.204      1.000 0.968 0.032
#&gt; GSM11270     2   0.204      0.972 0.032 0.968
#&gt; GSM11271     2   0.000      0.985 0.000 1.000
#&gt; GSM11288     2   0.000      0.985 0.000 1.000
#&gt; GSM11273     2   0.204      0.972 0.032 0.968
#&gt; GSM28757     2   0.000      0.985 0.000 1.000
#&gt; GSM11282     2   0.204      0.972 0.032 0.968
#&gt; GSM28756     2   0.000      0.985 0.000 1.000
#&gt; GSM11276     2   0.000      0.985 0.000 1.000
#&gt; GSM28752     2   0.000      0.985 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28763     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28764     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11274     3   0.455      0.767 0.000 0.200 0.800
#&gt; GSM28772     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11269     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28775     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11293     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28755     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11279     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28758     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11281     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11287     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28759     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11292     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28766     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11268     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM28767     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11286     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28751     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28770     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11283     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11289     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11280     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28749     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28750     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM11290     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM11294     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM28771     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28760     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28774     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11284     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28761     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM11278     3   0.556      0.701 0.000 0.300 0.700
#&gt; GSM11291     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM11277     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM11272     3   0.000      0.839 0.000 0.000 1.000
#&gt; GSM11285     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28753     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28773     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28765     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28768     2   0.460      0.721 0.204 0.796 0.000
#&gt; GSM28754     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28769     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11275     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11270     3   0.556      0.701 0.000 0.300 0.700
#&gt; GSM11271     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11288     2   0.475      0.666 0.000 0.784 0.216
#&gt; GSM11273     3   0.556      0.701 0.000 0.300 0.700
#&gt; GSM28757     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11282     3   0.556      0.701 0.000 0.300 0.700
#&gt; GSM28756     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM11276     2   0.000      0.983 0.000 1.000 0.000
#&gt; GSM28752     2   0.000      0.983 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28764     2  0.0336      0.972 0.000 0.992 0.008 0.000
#&gt; GSM11274     3  0.0000      0.728 0.000 0.000 1.000 0.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0921      0.963 0.000 0.972 0.028 0.000
#&gt; GSM28766     2  0.0921      0.963 0.000 0.972 0.028 0.000
#&gt; GSM11268     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     2  0.0921      0.963 0.000 0.972 0.028 0.000
#&gt; GSM11286     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28770     2  0.0921      0.963 0.000 0.972 0.028 0.000
#&gt; GSM11283     2  0.0707      0.965 0.000 0.980 0.020 0.000
#&gt; GSM11289     2  0.0921      0.963 0.000 0.972 0.028 0.000
#&gt; GSM11280     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28750     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM11290     3  0.4277      0.676 0.000 0.000 0.720 0.280
#&gt; GSM11294     3  0.4277      0.676 0.000 0.000 0.720 0.280
#&gt; GSM28771     2  0.0707      0.965 0.000 0.980 0.020 0.000
#&gt; GSM28760     2  0.0707      0.965 0.000 0.980 0.020 0.000
#&gt; GSM28774     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM11284     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28761     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     3  0.2345      0.755 0.000 0.100 0.900 0.000
#&gt; GSM11291     3  0.4277      0.676 0.000 0.000 0.720 0.280
#&gt; GSM11277     3  0.4277      0.676 0.000 0.000 0.720 0.280
#&gt; GSM11272     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM11285     2  0.0707      0.965 0.000 0.980 0.020 0.000
#&gt; GSM28753     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28773     2  0.0336      0.972 0.000 0.992 0.008 0.000
#&gt; GSM28765     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28768     2  0.3649      0.740 0.204 0.796 0.000 0.000
#&gt; GSM28754     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.2345      0.755 0.000 0.100 0.900 0.000
#&gt; GSM11271     2  0.0921      0.963 0.000 0.972 0.028 0.000
#&gt; GSM11288     2  0.4635      0.709 0.000 0.756 0.028 0.216
#&gt; GSM11273     3  0.2345      0.755 0.000 0.100 0.900 0.000
#&gt; GSM28757     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM11282     3  0.2345      0.755 0.000 0.100 0.900 0.000
#&gt; GSM28756     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM11276     2  0.0000      0.975 0.000 1.000 0.000 0.000
#&gt; GSM28752     2  0.0000      0.975 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.0290      0.962 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11274     5  0.0404      0.701 0.000 0.000 0.000 0.012 0.988
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0794      0.953 0.000 0.972 0.000 0.000 0.028
#&gt; GSM28766     2  0.0794      0.953 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11268     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28767     2  0.0794      0.953 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11286     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28770     2  0.0794      0.953 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11283     4  0.2020      1.000 0.000 0.100 0.000 0.900 0.000
#&gt; GSM11289     2  0.0794      0.953 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11280     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28750     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11290     5  0.5312      0.633 0.000 0.000 0.248 0.100 0.652
#&gt; GSM11294     5  0.5312      0.633 0.000 0.000 0.248 0.100 0.652
#&gt; GSM28771     4  0.2020      1.000 0.000 0.100 0.000 0.900 0.000
#&gt; GSM28760     4  0.2020      1.000 0.000 0.100 0.000 0.900 0.000
#&gt; GSM28774     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11284     2  0.2813      0.785 0.000 0.832 0.000 0.168 0.000
#&gt; GSM28761     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11278     5  0.2416      0.728 0.000 0.100 0.000 0.012 0.888
#&gt; GSM11291     5  0.5312      0.633 0.000 0.000 0.248 0.100 0.652
#&gt; GSM11277     5  0.5312      0.633 0.000 0.000 0.248 0.100 0.652
#&gt; GSM11272     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11285     4  0.2020      1.000 0.000 0.100 0.000 0.900 0.000
#&gt; GSM28753     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.0290      0.962 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28765     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.3143      0.712 0.204 0.796 0.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.2416      0.728 0.000 0.100 0.000 0.012 0.888
#&gt; GSM11271     2  0.0794      0.953 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11288     2  0.3993      0.689 0.000 0.756 0.216 0.000 0.028
#&gt; GSM11273     5  0.2416      0.728 0.000 0.100 0.000 0.012 0.888
#&gt; GSM28757     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11282     5  0.2416      0.728 0.000 0.100 0.000 0.012 0.888
#&gt; GSM28756     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11276     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28752     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.1910      0.902 0.000 0.892 0.000 0.000 0.108 0.000
#&gt; GSM11274     5  0.1814      0.840 0.000 0.000 0.100 0.000 0.900 0.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.128 0.000
#&gt; GSM28766     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.128 0.000
#&gt; GSM11268     6  0.0000      0.965 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.128 0.000
#&gt; GSM11286     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28770     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.128 0.000
#&gt; GSM11283     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11289     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.128 0.000
#&gt; GSM11280     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28750     6  0.0000      0.965 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28760     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28774     2  0.1814      0.904 0.000 0.900 0.000 0.000 0.100 0.000
#&gt; GSM11284     2  0.4232      0.760 0.000 0.732 0.000 0.168 0.100 0.000
#&gt; GSM28761     6  0.0000      0.965 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     5  0.0000      0.963 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11291     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.1863      0.884 0.000 0.000 0.104 0.000 0.000 0.896
#&gt; GSM11285     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28753     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.1714      0.908 0.000 0.908 0.000 0.000 0.092 0.000
#&gt; GSM28765     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.2823      0.726 0.204 0.796 0.000 0.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.0000      0.963 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11271     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.128 0.000
#&gt; GSM11288     2  0.3586      0.690 0.000 0.756 0.000 0.000 0.028 0.216
#&gt; GSM11273     5  0.0000      0.963 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28757     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11282     5  0.0000      0.963 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28756     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11276     2  0.0713      0.923 0.000 0.972 0.000 0.000 0.028 0.000
#&gt; GSM28752     2  0.0000      0.927 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:hclust 54     0.398 2
#> SD:hclust 54     0.374 3
#> SD:hclust 54     0.355 4
#> SD:hclust 54     0.483 5
#> SD:hclust 54     0.450 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.437           0.730       0.832          0.347 0.535   0.535
#> 3 3 0.750           0.968       0.950          0.578 0.881   0.783
#> 4 4 0.715           0.766       0.868          0.215 0.919   0.815
#> 5 5 0.676           0.569       0.716          0.123 0.971   0.921
#> 6 6 0.677           0.707       0.761          0.063 0.828   0.515
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.944 0.000 1.000
#&gt; GSM28763     2  0.0000      0.944 0.000 1.000
#&gt; GSM28764     2  0.0000      0.944 0.000 1.000
#&gt; GSM11274     2  0.9552      0.119 0.376 0.624
#&gt; GSM28772     1  0.9552      0.589 0.624 0.376
#&gt; GSM11269     1  0.9552      0.589 0.624 0.376
#&gt; GSM28775     1  0.9552      0.589 0.624 0.376
#&gt; GSM11293     1  0.9552      0.589 0.624 0.376
#&gt; GSM28755     1  0.9552      0.589 0.624 0.376
#&gt; GSM11279     1  0.9552      0.589 0.624 0.376
#&gt; GSM28758     1  0.9552      0.589 0.624 0.376
#&gt; GSM11281     1  0.9552      0.589 0.624 0.376
#&gt; GSM11287     1  0.9552      0.589 0.624 0.376
#&gt; GSM28759     1  0.9552      0.589 0.624 0.376
#&gt; GSM11292     2  0.0000      0.944 0.000 1.000
#&gt; GSM28766     2  0.0672      0.939 0.008 0.992
#&gt; GSM11268     1  0.9993      0.242 0.516 0.484
#&gt; GSM28767     2  0.0000      0.944 0.000 1.000
#&gt; GSM11286     2  0.0000      0.944 0.000 1.000
#&gt; GSM28751     2  0.0000      0.944 0.000 1.000
#&gt; GSM28770     2  0.0000      0.944 0.000 1.000
#&gt; GSM11283     2  0.0376      0.940 0.004 0.996
#&gt; GSM11289     2  0.0000      0.944 0.000 1.000
#&gt; GSM11280     2  0.0000      0.944 0.000 1.000
#&gt; GSM28749     2  0.0672      0.939 0.008 0.992
#&gt; GSM28750     1  0.9993      0.242 0.516 0.484
#&gt; GSM11290     1  0.9988      0.250 0.520 0.480
#&gt; GSM11294     1  0.9988      0.250 0.520 0.480
#&gt; GSM28771     2  0.0938      0.937 0.012 0.988
#&gt; GSM28760     2  0.1184      0.933 0.016 0.984
#&gt; GSM28774     2  0.0000      0.944 0.000 1.000
#&gt; GSM11284     2  0.0938      0.937 0.012 0.988
#&gt; GSM28761     1  0.9993      0.242 0.516 0.484
#&gt; GSM11278     2  0.1184      0.931 0.016 0.984
#&gt; GSM11291     1  0.9988      0.250 0.520 0.480
#&gt; GSM11277     1  0.9988      0.250 0.520 0.480
#&gt; GSM11272     1  0.9988      0.250 0.520 0.480
#&gt; GSM11285     2  0.0938      0.937 0.012 0.988
#&gt; GSM28753     2  0.0000      0.944 0.000 1.000
#&gt; GSM28773     2  0.0672      0.939 0.008 0.992
#&gt; GSM28765     2  0.0000      0.944 0.000 1.000
#&gt; GSM28768     2  0.5737      0.672 0.136 0.864
#&gt; GSM28754     2  0.0000      0.944 0.000 1.000
#&gt; GSM28769     2  0.0000      0.944 0.000 1.000
#&gt; GSM11275     1  0.9552      0.589 0.624 0.376
#&gt; GSM11270     2  0.1184      0.931 0.016 0.984
#&gt; GSM11271     2  0.0000      0.944 0.000 1.000
#&gt; GSM11288     2  0.2236      0.900 0.036 0.964
#&gt; GSM11273     2  0.9522      0.124 0.372 0.628
#&gt; GSM28757     2  0.0000      0.944 0.000 1.000
#&gt; GSM11282     2  0.1184      0.931 0.016 0.984
#&gt; GSM28756     2  0.0000      0.944 0.000 1.000
#&gt; GSM11276     2  0.0000      0.944 0.000 1.000
#&gt; GSM28752     2  0.0000      0.944 0.000 1.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11274     3  0.1163      0.968 0.000 0.028 0.972
#&gt; GSM28772     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11269     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM28775     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11293     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM28755     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11279     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM28758     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11281     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11287     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM28759     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11292     2  0.0747      0.970 0.000 0.984 0.016
#&gt; GSM28766     2  0.0747      0.970 0.000 0.984 0.016
#&gt; GSM11268     3  0.3310      0.978 0.064 0.028 0.908
#&gt; GSM28767     2  0.0747      0.970 0.000 0.984 0.016
#&gt; GSM11286     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28751     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28770     2  0.0747      0.970 0.000 0.984 0.016
#&gt; GSM11283     2  0.3888      0.902 0.064 0.888 0.048
#&gt; GSM11289     2  0.0892      0.969 0.000 0.980 0.020
#&gt; GSM11280     2  0.1337      0.958 0.012 0.972 0.016
#&gt; GSM28749     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28750     3  0.3310      0.978 0.064 0.028 0.908
#&gt; GSM11290     3  0.2187      0.981 0.024 0.028 0.948
#&gt; GSM11294     3  0.2187      0.981 0.024 0.028 0.948
#&gt; GSM28771     2  0.3888      0.902 0.064 0.888 0.048
#&gt; GSM28760     2  0.3888      0.902 0.064 0.888 0.048
#&gt; GSM28774     2  0.0592      0.970 0.000 0.988 0.012
#&gt; GSM11284     2  0.2903      0.930 0.048 0.924 0.028
#&gt; GSM28761     3  0.3310      0.978 0.064 0.028 0.908
#&gt; GSM11278     2  0.1031      0.966 0.000 0.976 0.024
#&gt; GSM11291     3  0.2187      0.981 0.024 0.028 0.948
#&gt; GSM11277     3  0.2187      0.981 0.024 0.028 0.948
#&gt; GSM11272     3  0.3310      0.978 0.064 0.028 0.908
#&gt; GSM11285     2  0.3993      0.901 0.064 0.884 0.052
#&gt; GSM28753     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28768     2  0.4178      0.764 0.172 0.828 0.000
#&gt; GSM28754     2  0.0237      0.971 0.000 0.996 0.004
#&gt; GSM28769     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11275     1  0.3038      1.000 0.896 0.104 0.000
#&gt; GSM11270     2  0.1031      0.966 0.000 0.976 0.024
#&gt; GSM11271     2  0.0747      0.970 0.000 0.984 0.016
#&gt; GSM11288     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11273     3  0.1411      0.965 0.000 0.036 0.964
#&gt; GSM28757     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11282     2  0.1031      0.966 0.000 0.976 0.024
#&gt; GSM28756     2  0.0237      0.971 0.000 0.996 0.004
#&gt; GSM11276     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.972 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.2345      0.696 0.000 0.900 0.000 0.100
#&gt; GSM28763     2  0.2345      0.696 0.000 0.900 0.000 0.100
#&gt; GSM28764     2  0.1978      0.729 0.000 0.928 0.004 0.068
#&gt; GSM11274     3  0.1867      0.846 0.000 0.000 0.928 0.072
#&gt; GSM28772     1  0.0469      0.985 0.988 0.012 0.000 0.000
#&gt; GSM11269     1  0.0469      0.985 0.988 0.012 0.000 0.000
#&gt; GSM28775     1  0.0657      0.984 0.984 0.012 0.000 0.004
#&gt; GSM11293     1  0.1174      0.980 0.968 0.012 0.000 0.020
#&gt; GSM28755     1  0.0657      0.984 0.984 0.012 0.000 0.004
#&gt; GSM11279     1  0.0469      0.985 0.988 0.012 0.000 0.000
#&gt; GSM28758     1  0.2402      0.950 0.912 0.012 0.000 0.076
#&gt; GSM11281     1  0.0469      0.985 0.988 0.012 0.000 0.000
#&gt; GSM11287     1  0.0469      0.985 0.988 0.012 0.000 0.000
#&gt; GSM28759     1  0.1174      0.980 0.968 0.012 0.000 0.020
#&gt; GSM11292     2  0.3751      0.635 0.000 0.800 0.004 0.196
#&gt; GSM28766     2  0.3751      0.635 0.000 0.800 0.004 0.196
#&gt; GSM11268     3  0.4327      0.858 0.016 0.000 0.768 0.216
#&gt; GSM28767     2  0.3751      0.635 0.000 0.800 0.004 0.196
#&gt; GSM11286     2  0.1211      0.727 0.000 0.960 0.000 0.040
#&gt; GSM28751     2  0.2345      0.696 0.000 0.900 0.000 0.100
#&gt; GSM28770     2  0.3751      0.635 0.000 0.800 0.004 0.196
#&gt; GSM11283     4  0.4679      0.919 0.000 0.352 0.000 0.648
#&gt; GSM11289     2  0.3751      0.635 0.000 0.800 0.004 0.196
#&gt; GSM11280     2  0.3172      0.617 0.000 0.840 0.000 0.160
#&gt; GSM28749     2  0.3024      0.636 0.000 0.852 0.000 0.148
#&gt; GSM28750     3  0.4327      0.858 0.016 0.000 0.768 0.216
#&gt; GSM11290     3  0.0188      0.881 0.004 0.000 0.996 0.000
#&gt; GSM11294     3  0.0188      0.881 0.004 0.000 0.996 0.000
#&gt; GSM28771     4  0.4564      0.952 0.000 0.328 0.000 0.672
#&gt; GSM28760     4  0.4564      0.952 0.000 0.328 0.000 0.672
#&gt; GSM28774     2  0.2466      0.715 0.000 0.900 0.004 0.096
#&gt; GSM11284     2  0.4819      0.304 0.000 0.652 0.004 0.344
#&gt; GSM28761     3  0.4327      0.858 0.016 0.000 0.768 0.216
#&gt; GSM11278     2  0.4295      0.554 0.000 0.752 0.008 0.240
#&gt; GSM11291     3  0.0188      0.881 0.004 0.000 0.996 0.000
#&gt; GSM11277     3  0.0188      0.881 0.004 0.000 0.996 0.000
#&gt; GSM11272     3  0.4327      0.858 0.016 0.000 0.768 0.216
#&gt; GSM11285     4  0.4605      0.900 0.000 0.336 0.000 0.664
#&gt; GSM28753     2  0.2408      0.693 0.000 0.896 0.000 0.104
#&gt; GSM28773     2  0.2408      0.693 0.000 0.896 0.000 0.104
#&gt; GSM28765     2  0.0188      0.736 0.000 0.996 0.000 0.004
#&gt; GSM28768     2  0.5220      0.462 0.092 0.752 0.000 0.156
#&gt; GSM28754     2  0.1489      0.734 0.000 0.952 0.004 0.044
#&gt; GSM28769     2  0.2345      0.696 0.000 0.900 0.000 0.100
#&gt; GSM11275     1  0.2402      0.950 0.912 0.012 0.000 0.076
#&gt; GSM11270     2  0.4295      0.554 0.000 0.752 0.008 0.240
#&gt; GSM11271     2  0.3626      0.649 0.000 0.812 0.004 0.184
#&gt; GSM11288     2  0.5127      0.229 0.012 0.632 0.000 0.356
#&gt; GSM11273     3  0.4502      0.629 0.000 0.016 0.748 0.236
#&gt; GSM28757     2  0.0707      0.733 0.000 0.980 0.000 0.020
#&gt; GSM11282     2  0.4360      0.540 0.000 0.744 0.008 0.248
#&gt; GSM28756     2  0.2125      0.726 0.000 0.920 0.004 0.076
#&gt; GSM11276     2  0.0336      0.737 0.000 0.992 0.000 0.008
#&gt; GSM28752     2  0.0188      0.737 0.000 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.4288     0.5327 0.000 0.612 0.000 0.384 0.004
#&gt; GSM28763     2  0.4288     0.5327 0.000 0.612 0.000 0.384 0.004
#&gt; GSM28764     2  0.1281     0.5270 0.000 0.956 0.000 0.032 0.012
#&gt; GSM11274     3  0.5114     0.5811 0.000 0.000 0.488 0.036 0.476
#&gt; GSM28772     1  0.0000     0.9674 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9674 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0566     0.9626 0.984 0.000 0.000 0.004 0.012
#&gt; GSM11293     1  0.1430     0.9524 0.944 0.000 0.000 0.004 0.052
#&gt; GSM28755     1  0.0566     0.9626 0.984 0.000 0.000 0.004 0.012
#&gt; GSM11279     1  0.0000     0.9674 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.2795     0.9128 0.872 0.000 0.000 0.028 0.100
#&gt; GSM11281     1  0.0000     0.9674 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9674 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.1430     0.9524 0.944 0.000 0.000 0.004 0.052
#&gt; GSM11292     2  0.4199     0.3509 0.000 0.772 0.000 0.160 0.068
#&gt; GSM28766     2  0.4238     0.3466 0.000 0.768 0.000 0.164 0.068
#&gt; GSM11268     3  0.0000     0.7100 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28767     2  0.4199     0.3509 0.000 0.772 0.000 0.160 0.068
#&gt; GSM11286     2  0.4009     0.5609 0.000 0.684 0.000 0.312 0.004
#&gt; GSM28751     2  0.4415     0.5292 0.000 0.604 0.000 0.388 0.008
#&gt; GSM28770     2  0.4199     0.3509 0.000 0.772 0.000 0.160 0.068
#&gt; GSM11283     4  0.5215     0.6702 0.000 0.096 0.000 0.664 0.240
#&gt; GSM11289     2  0.4238     0.3466 0.000 0.768 0.000 0.164 0.068
#&gt; GSM11280     2  0.4473     0.5004 0.000 0.580 0.000 0.412 0.008
#&gt; GSM28749     2  0.4375     0.4998 0.000 0.576 0.000 0.420 0.004
#&gt; GSM28750     3  0.0000     0.7100 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11290     3  0.4171     0.7308 0.000 0.000 0.604 0.000 0.396
#&gt; GSM11294     3  0.4171     0.7308 0.000 0.000 0.604 0.000 0.396
#&gt; GSM28771     4  0.5066     0.6751 0.000 0.084 0.000 0.676 0.240
#&gt; GSM28760     4  0.5035     0.6706 0.000 0.076 0.000 0.672 0.252
#&gt; GSM28774     2  0.2450     0.4604 0.000 0.896 0.000 0.028 0.076
#&gt; GSM11284     2  0.5094     0.1679 0.000 0.600 0.000 0.352 0.048
#&gt; GSM28761     3  0.0000     0.7100 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11278     2  0.5824    -0.0897 0.000 0.608 0.000 0.168 0.224
#&gt; GSM11291     3  0.4171     0.7308 0.000 0.000 0.604 0.000 0.396
#&gt; GSM11277     3  0.4171     0.7308 0.000 0.000 0.604 0.000 0.396
#&gt; GSM11272     3  0.0000     0.7100 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11285     4  0.5595     0.5908 0.000 0.124 0.000 0.624 0.252
#&gt; GSM28753     2  0.4276     0.5323 0.000 0.616 0.000 0.380 0.004
#&gt; GSM28773     2  0.4517     0.5274 0.000 0.600 0.000 0.388 0.012
#&gt; GSM28765     2  0.3766     0.5728 0.000 0.728 0.000 0.268 0.004
#&gt; GSM28768     2  0.5799     0.4646 0.020 0.536 0.000 0.392 0.052
#&gt; GSM28754     2  0.2974     0.4956 0.000 0.868 0.000 0.052 0.080
#&gt; GSM28769     2  0.4415     0.5292 0.000 0.604 0.000 0.388 0.008
#&gt; GSM11275     1  0.2795     0.9128 0.872 0.000 0.000 0.028 0.100
#&gt; GSM11270     2  0.5824    -0.0897 0.000 0.608 0.000 0.168 0.224
#&gt; GSM11271     2  0.3898     0.3805 0.000 0.804 0.000 0.116 0.080
#&gt; GSM11288     4  0.6547    -0.2074 0.000 0.296 0.232 0.472 0.000
#&gt; GSM11273     5  0.7551     0.0000 0.000 0.276 0.096 0.148 0.480
#&gt; GSM28757     2  0.4902     0.5581 0.000 0.648 0.000 0.304 0.048
#&gt; GSM11282     2  0.5880    -0.1018 0.000 0.600 0.000 0.172 0.228
#&gt; GSM28756     2  0.2233     0.4750 0.000 0.904 0.000 0.016 0.080
#&gt; GSM11276     2  0.2629     0.5799 0.000 0.860 0.000 0.136 0.004
#&gt; GSM28752     2  0.3715     0.5744 0.000 0.736 0.000 0.260 0.004
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.0436      0.829 0.000 0.988 0.000 0.004 0.004 0.004
#&gt; GSM28763     2  0.0436      0.829 0.000 0.988 0.000 0.004 0.004 0.004
#&gt; GSM28764     5  0.4913      0.418 0.000 0.428 0.000 0.020 0.524 0.028
#&gt; GSM11274     3  0.4410      0.400 0.000 0.000 0.744 0.016 0.144 0.096
#&gt; GSM28772     1  0.0000      0.925 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.925 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.1605      0.907 0.936 0.000 0.000 0.044 0.004 0.016
#&gt; GSM11293     1  0.2152      0.903 0.912 0.000 0.000 0.036 0.012 0.040
#&gt; GSM28755     1  0.1605      0.907 0.936 0.000 0.000 0.044 0.004 0.016
#&gt; GSM11279     1  0.0146      0.925 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM28758     1  0.4901      0.772 0.708 0.000 0.000 0.100 0.032 0.160
#&gt; GSM11281     1  0.0000      0.925 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.925 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.2152      0.903 0.912 0.000 0.000 0.036 0.012 0.040
#&gt; GSM11292     5  0.4095      0.701 0.000 0.208 0.000 0.064 0.728 0.000
#&gt; GSM28766     5  0.4095      0.701 0.000 0.208 0.000 0.064 0.728 0.000
#&gt; GSM11268     6  0.3868      0.996 0.000 0.000 0.496 0.000 0.000 0.504
#&gt; GSM28767     5  0.4095      0.701 0.000 0.208 0.000 0.064 0.728 0.000
#&gt; GSM11286     2  0.2883      0.803 0.000 0.860 0.000 0.008 0.040 0.092
#&gt; GSM28751     2  0.0748      0.828 0.000 0.976 0.000 0.004 0.004 0.016
#&gt; GSM28770     5  0.4066      0.701 0.000 0.204 0.000 0.064 0.732 0.000
#&gt; GSM11283     4  0.3424      0.934 0.000 0.092 0.000 0.812 0.096 0.000
#&gt; GSM11289     5  0.4066      0.701 0.000 0.204 0.000 0.064 0.732 0.000
#&gt; GSM11280     2  0.3265      0.792 0.000 0.836 0.000 0.068 0.008 0.088
#&gt; GSM28749     2  0.3306      0.793 0.000 0.840 0.000 0.052 0.020 0.088
#&gt; GSM28750     6  0.3999      0.993 0.000 0.000 0.496 0.000 0.004 0.500
#&gt; GSM11290     3  0.0000      0.688 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.688 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     4  0.3413      0.944 0.000 0.080 0.000 0.812 0.108 0.000
#&gt; GSM28760     4  0.4203      0.941 0.000 0.072 0.000 0.768 0.136 0.024
#&gt; GSM28774     5  0.5785      0.514 0.000 0.332 0.000 0.024 0.532 0.112
#&gt; GSM11284     5  0.7044      0.429 0.000 0.228 0.000 0.224 0.448 0.100
#&gt; GSM28761     6  0.3868      0.996 0.000 0.000 0.496 0.000 0.000 0.504
#&gt; GSM11278     5  0.4498      0.561 0.000 0.072 0.000 0.032 0.744 0.152
#&gt; GSM11291     3  0.0000      0.688 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.688 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     3  0.4185     -0.983 0.000 0.000 0.496 0.012 0.000 0.492
#&gt; GSM11285     4  0.3912      0.902 0.000 0.028 0.000 0.768 0.180 0.024
#&gt; GSM28753     2  0.1010      0.830 0.000 0.960 0.000 0.004 0.000 0.036
#&gt; GSM28773     2  0.2764      0.811 0.000 0.864 0.000 0.008 0.028 0.100
#&gt; GSM28765     2  0.3318      0.764 0.000 0.828 0.000 0.004 0.084 0.084
#&gt; GSM28768     2  0.3988      0.710 0.012 0.804 0.000 0.068 0.020 0.096
#&gt; GSM28754     5  0.6001      0.331 0.000 0.416 0.000 0.024 0.436 0.124
#&gt; GSM28769     2  0.0748      0.828 0.000 0.976 0.000 0.004 0.004 0.016
#&gt; GSM11275     1  0.4901      0.772 0.708 0.000 0.000 0.100 0.032 0.160
#&gt; GSM11270     5  0.4498      0.561 0.000 0.072 0.000 0.032 0.744 0.152
#&gt; GSM11271     5  0.3586      0.705 0.000 0.216 0.000 0.028 0.756 0.000
#&gt; GSM11288     2  0.4877      0.606 0.000 0.700 0.000 0.064 0.040 0.196
#&gt; GSM11273     5  0.5193      0.374 0.000 0.000 0.148 0.032 0.680 0.140
#&gt; GSM28757     2  0.3710      0.772 0.000 0.812 0.000 0.024 0.064 0.100
#&gt; GSM11282     5  0.4283      0.560 0.000 0.064 0.000 0.028 0.760 0.148
#&gt; GSM28756     5  0.5832      0.481 0.000 0.352 0.000 0.024 0.512 0.112
#&gt; GSM11276     2  0.4705      0.186 0.000 0.612 0.000 0.008 0.336 0.044
#&gt; GSM28752     2  0.3280      0.695 0.000 0.812 0.000 0.004 0.152 0.032
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:kmeans 44     0.387 2
#> SD:kmeans 54     0.374 3
#> SD:kmeans 51     0.516 4
#> SD:kmeans 37     0.402 5
#> SD:kmeans 46     0.450 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4848 0.516   0.516
#> 3 3 0.940           0.902       0.966         0.2776 0.787   0.613
#> 4 4 0.800           0.693       0.871         0.2143 0.827   0.564
#> 5 5 0.827           0.808       0.884         0.0718 0.898   0.621
#> 6 6 0.816           0.731       0.841         0.0381 0.950   0.751
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM28762     2       0          1  0  1
#&gt; GSM28763     2       0          1  0  1
#&gt; GSM28764     2       0          1  0  1
#&gt; GSM11274     2       0          1  0  1
#&gt; GSM28772     1       0          1  1  0
#&gt; GSM11269     1       0          1  1  0
#&gt; GSM28775     1       0          1  1  0
#&gt; GSM11293     1       0          1  1  0
#&gt; GSM28755     1       0          1  1  0
#&gt; GSM11279     1       0          1  1  0
#&gt; GSM28758     1       0          1  1  0
#&gt; GSM11281     1       0          1  1  0
#&gt; GSM11287     1       0          1  1  0
#&gt; GSM28759     1       0          1  1  0
#&gt; GSM11292     2       0          1  0  1
#&gt; GSM28766     2       0          1  0  1
#&gt; GSM11268     1       0          1  1  0
#&gt; GSM28767     2       0          1  0  1
#&gt; GSM11286     2       0          1  0  1
#&gt; GSM28751     2       0          1  0  1
#&gt; GSM28770     2       0          1  0  1
#&gt; GSM11283     2       0          1  0  1
#&gt; GSM11289     2       0          1  0  1
#&gt; GSM11280     2       0          1  0  1
#&gt; GSM28749     2       0          1  0  1
#&gt; GSM28750     1       0          1  1  0
#&gt; GSM11290     1       0          1  1  0
#&gt; GSM11294     1       0          1  1  0
#&gt; GSM28771     2       0          1  0  1
#&gt; GSM28760     2       0          1  0  1
#&gt; GSM28774     2       0          1  0  1
#&gt; GSM11284     2       0          1  0  1
#&gt; GSM28761     1       0          1  1  0
#&gt; GSM11278     2       0          1  0  1
#&gt; GSM11291     1       0          1  1  0
#&gt; GSM11277     1       0          1  1  0
#&gt; GSM11272     1       0          1  1  0
#&gt; GSM11285     2       0          1  0  1
#&gt; GSM28753     2       0          1  0  1
#&gt; GSM28773     2       0          1  0  1
#&gt; GSM28765     2       0          1  0  1
#&gt; GSM28768     1       0          1  1  0
#&gt; GSM28754     2       0          1  0  1
#&gt; GSM28769     2       0          1  0  1
#&gt; GSM11275     1       0          1  1  0
#&gt; GSM11270     2       0          1  0  1
#&gt; GSM11271     2       0          1  0  1
#&gt; GSM11288     1       0          1  1  0
#&gt; GSM11273     2       0          1  0  1
#&gt; GSM28757     2       0          1  0  1
#&gt; GSM11282     2       0          1  0  1
#&gt; GSM28756     2       0          1  0  1
#&gt; GSM11276     2       0          1  0  1
#&gt; GSM28752     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28751     1  0.5529     0.5933 0.704 0.296 0.000
#&gt; GSM28770     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28749     2  0.3267     0.8505 0.000 0.884 0.116
#&gt; GSM28750     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM28771     2  0.6252     0.1338 0.000 0.556 0.444
#&gt; GSM28760     3  0.6305     0.0192 0.000 0.484 0.516
#&gt; GSM28774     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM11278     2  0.0592     0.9672 0.000 0.988 0.012
#&gt; GSM11291     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM11285     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28773     2  0.0424     0.9701 0.000 0.992 0.008
#&gt; GSM28765     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28768     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM28754     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28769     1  0.6244     0.2665 0.560 0.440 0.000
#&gt; GSM11275     1  0.0000     0.9271 1.000 0.000 0.000
#&gt; GSM11270     2  0.0592     0.9672 0.000 0.988 0.012
#&gt; GSM11271     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11288     3  0.0592     0.9308 0.012 0.000 0.988
#&gt; GSM11273     3  0.0000     0.9420 0.000 0.000 1.000
#&gt; GSM28757     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11282     2  0.0592     0.9672 0.000 0.988 0.012
#&gt; GSM28756     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000     0.9759 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000     0.9759 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     4  0.2814     0.6061 0.000 0.132 0.000 0.868
#&gt; GSM28763     4  0.2814     0.6061 0.000 0.132 0.000 0.868
#&gt; GSM28764     2  0.3219     0.6080 0.000 0.836 0.000 0.164
#&gt; GSM11274     3  0.0188     0.9900 0.000 0.000 0.996 0.004
#&gt; GSM28772     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0000     0.7025 0.000 1.000 0.000 0.000
#&gt; GSM28766     2  0.0000     0.7025 0.000 1.000 0.000 0.000
#&gt; GSM11268     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM28767     2  0.0000     0.7025 0.000 1.000 0.000 0.000
#&gt; GSM11286     4  0.4989    -0.1470 0.000 0.472 0.000 0.528
#&gt; GSM28751     4  0.3392     0.6251 0.072 0.056 0.000 0.872
#&gt; GSM28770     2  0.0000     0.7025 0.000 1.000 0.000 0.000
#&gt; GSM11283     4  0.4730     0.3328 0.000 0.364 0.000 0.636
#&gt; GSM11289     2  0.0000     0.7025 0.000 1.000 0.000 0.000
#&gt; GSM11280     4  0.0188     0.6472 0.000 0.004 0.000 0.996
#&gt; GSM28749     4  0.4168     0.5749 0.000 0.092 0.080 0.828
#&gt; GSM28750     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM11290     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.5105     0.2489 0.000 0.432 0.004 0.564
#&gt; GSM28760     4  0.5126     0.2286 0.000 0.444 0.004 0.552
#&gt; GSM28774     2  0.4454     0.4747 0.000 0.692 0.000 0.308
#&gt; GSM11284     2  0.4040     0.3779 0.000 0.752 0.000 0.248
#&gt; GSM28761     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM11278     2  0.0188     0.7005 0.000 0.996 0.000 0.004
#&gt; GSM11291     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.0000     0.9922 0.000 0.000 1.000 0.000
#&gt; GSM11285     2  0.4907    -0.0279 0.000 0.580 0.000 0.420
#&gt; GSM28753     4  0.0188     0.6472 0.000 0.004 0.000 0.996
#&gt; GSM28773     4  0.0592     0.6472 0.000 0.016 0.000 0.984
#&gt; GSM28765     2  0.4994     0.1788 0.000 0.520 0.000 0.480
#&gt; GSM28768     1  0.0921     0.9692 0.972 0.000 0.000 0.028
#&gt; GSM28754     2  0.4916     0.3078 0.000 0.576 0.000 0.424
#&gt; GSM28769     4  0.3266     0.6283 0.040 0.084 0.000 0.876
#&gt; GSM11275     1  0.0000     0.9973 1.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.0188     0.7005 0.000 0.996 0.000 0.004
#&gt; GSM11271     2  0.0000     0.7025 0.000 1.000 0.000 0.000
#&gt; GSM11288     3  0.1978     0.9263 0.004 0.000 0.928 0.068
#&gt; GSM11273     3  0.0188     0.9900 0.000 0.000 0.996 0.004
#&gt; GSM28757     4  0.4994    -0.1699 0.000 0.480 0.000 0.520
#&gt; GSM11282     2  0.0188     0.7005 0.000 0.996 0.000 0.004
#&gt; GSM28756     2  0.4697     0.4149 0.000 0.644 0.000 0.356
#&gt; GSM11276     2  0.4955     0.2701 0.000 0.556 0.000 0.444
#&gt; GSM28752     2  0.4961     0.2615 0.000 0.552 0.000 0.448
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.1952      0.768 0.000 0.912 0.000 0.084 0.004
#&gt; GSM28763     2  0.1952      0.768 0.000 0.912 0.000 0.084 0.004
#&gt; GSM28764     5  0.3455      0.676 0.000 0.208 0.000 0.008 0.784
#&gt; GSM11274     3  0.1059      0.948 0.000 0.020 0.968 0.008 0.004
#&gt; GSM28772     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.1012      0.830 0.000 0.020 0.000 0.012 0.968
#&gt; GSM28766     5  0.1012      0.830 0.000 0.020 0.000 0.012 0.968
#&gt; GSM11268     3  0.0162      0.965 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28767     5  0.1012      0.830 0.000 0.020 0.000 0.012 0.968
#&gt; GSM11286     2  0.2632      0.777 0.000 0.888 0.000 0.040 0.072
#&gt; GSM28751     2  0.2929      0.774 0.008 0.880 0.000 0.068 0.044
#&gt; GSM28770     5  0.1012      0.830 0.000 0.020 0.000 0.012 0.968
#&gt; GSM11283     4  0.0566      0.820 0.000 0.012 0.000 0.984 0.004
#&gt; GSM11289     5  0.1012      0.830 0.000 0.020 0.000 0.012 0.968
#&gt; GSM11280     4  0.3274      0.715 0.000 0.220 0.000 0.780 0.000
#&gt; GSM28749     4  0.3554      0.720 0.000 0.216 0.004 0.776 0.004
#&gt; GSM28750     3  0.0000      0.966 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11290     3  0.0000      0.966 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.966 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28771     4  0.0613      0.819 0.000 0.008 0.004 0.984 0.004
#&gt; GSM28760     4  0.0727      0.819 0.000 0.004 0.004 0.980 0.012
#&gt; GSM28774     5  0.4437      0.570 0.000 0.316 0.000 0.020 0.664
#&gt; GSM11284     4  0.3085      0.747 0.000 0.032 0.000 0.852 0.116
#&gt; GSM28761     3  0.0162      0.965 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11278     5  0.2844      0.789 0.000 0.092 0.004 0.028 0.876
#&gt; GSM11291     3  0.0000      0.966 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.966 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.0162      0.965 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11285     4  0.1704      0.798 0.000 0.004 0.000 0.928 0.068
#&gt; GSM28753     2  0.4138      0.288 0.000 0.616 0.000 0.384 0.000
#&gt; GSM28773     4  0.4883      0.244 0.000 0.464 0.004 0.516 0.016
#&gt; GSM28765     2  0.2970      0.725 0.000 0.828 0.000 0.004 0.168
#&gt; GSM28768     1  0.3074      0.761 0.804 0.196 0.000 0.000 0.000
#&gt; GSM28754     5  0.4538      0.249 0.000 0.452 0.000 0.008 0.540
#&gt; GSM28769     2  0.2867      0.774 0.004 0.880 0.000 0.072 0.044
#&gt; GSM11275     1  0.0000      0.981 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.2928      0.787 0.000 0.092 0.004 0.032 0.872
#&gt; GSM11271     5  0.1012      0.830 0.000 0.020 0.000 0.012 0.968
#&gt; GSM11288     3  0.3421      0.733 0.000 0.008 0.788 0.204 0.000
#&gt; GSM11273     3  0.2267      0.908 0.000 0.028 0.916 0.008 0.048
#&gt; GSM28757     2  0.3370      0.733 0.000 0.824 0.000 0.028 0.148
#&gt; GSM11282     5  0.2408      0.795 0.000 0.092 0.000 0.016 0.892
#&gt; GSM28756     5  0.4151      0.530 0.000 0.344 0.000 0.004 0.652
#&gt; GSM11276     2  0.4150      0.407 0.000 0.612 0.000 0.000 0.388
#&gt; GSM28752     2  0.3395      0.690 0.000 0.764 0.000 0.000 0.236
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.0862      0.661 0.000 0.972 0.000 0.016 0.004 0.008
#&gt; GSM28763     2  0.0951      0.660 0.000 0.968 0.000 0.020 0.004 0.008
#&gt; GSM28764     5  0.1970      0.868 0.000 0.060 0.000 0.000 0.912 0.028
#&gt; GSM11274     3  0.3398      0.748 0.000 0.000 0.740 0.008 0.000 0.252
#&gt; GSM28772     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000      0.978 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28766     5  0.0000      0.978 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11268     3  0.0692      0.861 0.000 0.004 0.976 0.000 0.000 0.020
#&gt; GSM28767     5  0.0000      0.978 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11286     2  0.4773      0.362 0.000 0.572 0.000 0.004 0.048 0.376
#&gt; GSM28751     2  0.0909      0.660 0.000 0.968 0.000 0.020 0.012 0.000
#&gt; GSM28770     5  0.0000      0.978 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11283     4  0.0291      0.838 0.000 0.000 0.000 0.992 0.004 0.004
#&gt; GSM11289     5  0.0000      0.978 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11280     4  0.5005      0.608 0.000 0.164 0.000 0.644 0.000 0.192
#&gt; GSM28749     4  0.5694      0.584 0.000 0.160 0.028 0.608 0.000 0.204
#&gt; GSM28750     3  0.0146      0.865 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; GSM11290     3  0.1663      0.870 0.000 0.000 0.912 0.000 0.000 0.088
#&gt; GSM11294     3  0.1663      0.870 0.000 0.000 0.912 0.000 0.000 0.088
#&gt; GSM28771     4  0.0291      0.838 0.000 0.000 0.000 0.992 0.004 0.004
#&gt; GSM28760     4  0.0146      0.837 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28774     6  0.5440      0.499 0.000 0.140 0.000 0.000 0.324 0.536
#&gt; GSM11284     4  0.2937      0.745 0.000 0.000 0.000 0.848 0.056 0.096
#&gt; GSM28761     3  0.0692      0.861 0.000 0.004 0.976 0.000 0.000 0.020
#&gt; GSM11278     6  0.3997      0.526 0.000 0.004 0.012 0.012 0.256 0.716
#&gt; GSM11291     3  0.1663      0.870 0.000 0.000 0.912 0.000 0.000 0.088
#&gt; GSM11277     3  0.1663      0.870 0.000 0.000 0.912 0.000 0.000 0.088
#&gt; GSM11272     3  0.0692      0.861 0.000 0.004 0.976 0.000 0.000 0.020
#&gt; GSM11285     4  0.0790      0.827 0.000 0.000 0.000 0.968 0.032 0.000
#&gt; GSM28753     2  0.5327      0.358 0.000 0.588 0.000 0.248 0.000 0.164
#&gt; GSM28773     6  0.6773     -0.195 0.000 0.284 0.028 0.340 0.004 0.344
#&gt; GSM28765     2  0.5592      0.275 0.000 0.516 0.000 0.004 0.136 0.344
#&gt; GSM28768     1  0.3373      0.669 0.744 0.248 0.000 0.000 0.000 0.008
#&gt; GSM28754     6  0.5053      0.451 0.000 0.204 0.000 0.000 0.160 0.636
#&gt; GSM28769     2  0.0909      0.660 0.000 0.968 0.000 0.020 0.012 0.000
#&gt; GSM11275     1  0.0000      0.976 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     6  0.4121      0.528 0.000 0.004 0.012 0.020 0.248 0.716
#&gt; GSM11271     5  0.0146      0.973 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11288     3  0.4634      0.637 0.008 0.024 0.740 0.156 0.000 0.072
#&gt; GSM11273     3  0.4604      0.443 0.000 0.000 0.536 0.008 0.024 0.432
#&gt; GSM28757     6  0.4339      0.190 0.000 0.316 0.000 0.004 0.032 0.648
#&gt; GSM11282     6  0.3819      0.490 0.000 0.000 0.000 0.012 0.316 0.672
#&gt; GSM28756     6  0.5237      0.484 0.000 0.172 0.000 0.000 0.220 0.608
#&gt; GSM11276     2  0.5187      0.228 0.000 0.472 0.000 0.000 0.440 0.088
#&gt; GSM28752     2  0.4456      0.484 0.000 0.668 0.000 0.000 0.268 0.064
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> SD:skmeans 54     0.398 2
#> SD:skmeans 51     0.371 3
#> SD:skmeans 41     0.407 4
#> SD:skmeans 50     0.439 5
#> SD:skmeans 42     0.408 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.966       0.987         0.3463 0.669   0.669
#> 3 3 0.887           0.939       0.975         0.5808 0.786   0.681
#> 4 4 0.727           0.720       0.877         0.3041 0.809   0.581
#> 5 5 0.700           0.582       0.790         0.0607 0.893   0.646
#> 6 6 0.810           0.773       0.901         0.0422 0.881   0.573
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.983 0.000 1.000
#&gt; GSM28763     2  0.0000      0.983 0.000 1.000
#&gt; GSM28764     2  0.0000      0.983 0.000 1.000
#&gt; GSM11274     2  0.0000      0.983 0.000 1.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000
#&gt; GSM11292     2  0.0000      0.983 0.000 1.000
#&gt; GSM28766     2  0.0000      0.983 0.000 1.000
#&gt; GSM11268     2  0.0000      0.983 0.000 1.000
#&gt; GSM28767     2  0.0000      0.983 0.000 1.000
#&gt; GSM11286     2  0.0000      0.983 0.000 1.000
#&gt; GSM28751     2  0.0000      0.983 0.000 1.000
#&gt; GSM28770     2  0.0000      0.983 0.000 1.000
#&gt; GSM11283     2  0.0000      0.983 0.000 1.000
#&gt; GSM11289     2  0.0000      0.983 0.000 1.000
#&gt; GSM11280     2  0.0000      0.983 0.000 1.000
#&gt; GSM28749     2  0.0000      0.983 0.000 1.000
#&gt; GSM28750     2  0.0000      0.983 0.000 1.000
#&gt; GSM11290     2  0.2423      0.945 0.040 0.960
#&gt; GSM11294     2  0.0000      0.983 0.000 1.000
#&gt; GSM28771     2  0.0000      0.983 0.000 1.000
#&gt; GSM28760     2  0.0000      0.983 0.000 1.000
#&gt; GSM28774     2  0.0000      0.983 0.000 1.000
#&gt; GSM11284     2  0.0000      0.983 0.000 1.000
#&gt; GSM28761     2  0.0000      0.983 0.000 1.000
#&gt; GSM11278     2  0.0000      0.983 0.000 1.000
#&gt; GSM11291     2  0.0000      0.983 0.000 1.000
#&gt; GSM11277     2  0.0376      0.979 0.004 0.996
#&gt; GSM11272     2  0.9933      0.194 0.452 0.548
#&gt; GSM11285     2  0.0000      0.983 0.000 1.000
#&gt; GSM28753     2  0.0000      0.983 0.000 1.000
#&gt; GSM28773     2  0.0000      0.983 0.000 1.000
#&gt; GSM28765     2  0.0000      0.983 0.000 1.000
#&gt; GSM28768     2  0.7528      0.724 0.216 0.784
#&gt; GSM28754     2  0.0000      0.983 0.000 1.000
#&gt; GSM28769     2  0.0000      0.983 0.000 1.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000
#&gt; GSM11270     2  0.0000      0.983 0.000 1.000
#&gt; GSM11271     2  0.0000      0.983 0.000 1.000
#&gt; GSM11288     2  0.0000      0.983 0.000 1.000
#&gt; GSM11273     2  0.0000      0.983 0.000 1.000
#&gt; GSM28757     2  0.0000      0.983 0.000 1.000
#&gt; GSM11282     2  0.0000      0.983 0.000 1.000
#&gt; GSM28756     2  0.0000      0.983 0.000 1.000
#&gt; GSM11276     2  0.0000      0.983 0.000 1.000
#&gt; GSM28752     2  0.0000      0.983 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.905 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11268     3  0.2959      0.813 0.000 0.100 0.900
#&gt; GSM28767     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28751     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28770     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28749     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28750     3  0.0000      0.905 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.905 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.905 0.000 0.000 1.000
#&gt; GSM28771     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28760     2  0.3686      0.842 0.000 0.860 0.140
#&gt; GSM28774     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28761     3  0.6126      0.309 0.000 0.400 0.600
#&gt; GSM11278     2  0.3686      0.842 0.000 0.860 0.140
#&gt; GSM11291     3  0.0000      0.905 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.905 0.000 0.000 1.000
#&gt; GSM11272     3  0.0592      0.898 0.000 0.012 0.988
#&gt; GSM11285     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28768     2  0.2878      0.875 0.096 0.904 0.000
#&gt; GSM28754     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28769     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11270     2  0.3686      0.842 0.000 0.860 0.140
#&gt; GSM11271     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11288     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11273     2  0.4346      0.785 0.000 0.816 0.184
#&gt; GSM28757     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11282     2  0.3686      0.842 0.000 0.860 0.140
#&gt; GSM28756     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.972 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4
#&gt; GSM28762     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28764     2  0.4477      0.365  0 0.688 0.000 0.312
#&gt; GSM11274     3  0.4776      0.562  0 0.000 0.624 0.376
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11292     4  0.4877      0.562  0 0.408 0.000 0.592
#&gt; GSM28766     4  0.4817      0.567  0 0.388 0.000 0.612
#&gt; GSM11268     3  0.0657      0.889  0 0.004 0.984 0.012
#&gt; GSM28767     4  0.4877      0.562  0 0.408 0.000 0.592
#&gt; GSM11286     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28770     4  0.4877      0.562  0 0.408 0.000 0.592
#&gt; GSM11283     2  0.0921      0.785  0 0.972 0.000 0.028
#&gt; GSM11289     4  0.4877      0.562  0 0.408 0.000 0.592
#&gt; GSM11280     2  0.0188      0.802  0 0.996 0.000 0.004
#&gt; GSM28749     2  0.4948     -0.148  0 0.560 0.000 0.440
#&gt; GSM28750     3  0.0336      0.891  0 0.000 0.992 0.008
#&gt; GSM11290     3  0.0000      0.892  0 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.892  0 0.000 1.000 0.000
#&gt; GSM28771     2  0.0921      0.785  0 0.972 0.000 0.028
#&gt; GSM28760     4  0.2530      0.528  0 0.112 0.000 0.888
#&gt; GSM28774     4  0.2530      0.629  0 0.112 0.000 0.888
#&gt; GSM11284     4  0.4989      0.345  0 0.472 0.000 0.528
#&gt; GSM28761     3  0.6494      0.457  0 0.340 0.572 0.088
#&gt; GSM11278     4  0.1022      0.634  0 0.032 0.000 0.968
#&gt; GSM11291     3  0.0000      0.892  0 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.892  0 0.000 1.000 0.000
#&gt; GSM11272     3  0.2214      0.858  0 0.044 0.928 0.028
#&gt; GSM11285     4  0.4790      0.566  0 0.380 0.000 0.620
#&gt; GSM28753     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28773     2  0.0188      0.802  0 0.996 0.000 0.004
#&gt; GSM28765     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28768     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM28754     2  0.3528      0.574  0 0.808 0.000 0.192
#&gt; GSM28769     2  0.0000      0.804  0 1.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11270     4  0.1022      0.634  0 0.032 0.000 0.968
#&gt; GSM11271     2  0.4661      0.262  0 0.652 0.000 0.348
#&gt; GSM11288     2  0.1302      0.775  0 0.956 0.000 0.044
#&gt; GSM11273     4  0.0469      0.618  0 0.012 0.000 0.988
#&gt; GSM28757     2  0.4761      0.273  0 0.628 0.000 0.372
#&gt; GSM11282     4  0.1022      0.634  0 0.032 0.000 0.968
#&gt; GSM28756     2  0.3907      0.541  0 0.768 0.000 0.232
#&gt; GSM11276     2  0.4277      0.442  0 0.720 0.000 0.280
#&gt; GSM28752     2  0.3873      0.548  0 0.772 0.000 0.228
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.4074      0.759  0 0.636 0.000 0.000 0.364
#&gt; GSM28763     2  0.4074      0.759  0 0.636 0.000 0.000 0.364
#&gt; GSM28764     5  0.4138     -0.354  0 0.384 0.000 0.000 0.616
#&gt; GSM11274     4  0.6300      0.262  0 0.336 0.168 0.496 0.000
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000      0.565  0 0.000 0.000 0.000 1.000
#&gt; GSM28766     5  0.0703      0.566  0 0.024 0.000 0.000 0.976
#&gt; GSM11268     3  0.0000      0.795  0 0.000 1.000 0.000 0.000
#&gt; GSM28767     5  0.0000      0.565  0 0.000 0.000 0.000 1.000
#&gt; GSM11286     2  0.4211      0.758  0 0.636 0.004 0.000 0.360
#&gt; GSM28751     2  0.4074      0.759  0 0.636 0.000 0.000 0.364
#&gt; GSM28770     5  0.0000      0.565  0 0.000 0.000 0.000 1.000
#&gt; GSM11283     2  0.6025      0.373  0 0.496 0.000 0.384 0.120
#&gt; GSM11289     5  0.0000      0.565  0 0.000 0.000 0.000 1.000
#&gt; GSM11280     2  0.6912      0.593  0 0.508 0.220 0.024 0.248
#&gt; GSM28749     5  0.5862      0.133  0 0.176 0.220 0.000 0.604
#&gt; GSM28750     3  0.3274      0.411  0 0.000 0.780 0.220 0.000
#&gt; GSM11290     4  0.4138      0.508  0 0.000 0.384 0.616 0.000
#&gt; GSM11294     4  0.4138      0.508  0 0.000 0.384 0.616 0.000
#&gt; GSM28771     2  0.6025      0.373  0 0.496 0.000 0.384 0.120
#&gt; GSM28760     4  0.8162     -0.164  0 0.124 0.220 0.384 0.272
#&gt; GSM28774     5  0.4161      0.485  0 0.392 0.000 0.000 0.608
#&gt; GSM11284     5  0.6615      0.262  0 0.116 0.080 0.188 0.616
#&gt; GSM28761     3  0.1671      0.704  0 0.076 0.924 0.000 0.000
#&gt; GSM11278     5  0.3966      0.470  0 0.336 0.000 0.000 0.664
#&gt; GSM11291     4  0.4138      0.508  0 0.000 0.384 0.616 0.000
#&gt; GSM11277     4  0.4138      0.508  0 0.000 0.384 0.616 0.000
#&gt; GSM11272     3  0.0000      0.795  0 0.000 1.000 0.000 0.000
#&gt; GSM11285     5  0.4835      0.351  0 0.028 0.000 0.380 0.592
#&gt; GSM28753     2  0.4555      0.753  0 0.636 0.020 0.000 0.344
#&gt; GSM28773     2  0.6408      0.606  0 0.508 0.220 0.000 0.272
#&gt; GSM28765     2  0.4074      0.759  0 0.636 0.000 0.000 0.364
#&gt; GSM28768     2  0.4074      0.759  0 0.636 0.000 0.000 0.364
#&gt; GSM28754     2  0.2852      0.584  0 0.828 0.000 0.000 0.172
#&gt; GSM28769     2  0.4074      0.759  0 0.636 0.000 0.000 0.364
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.3966      0.470  0 0.336 0.000 0.000 0.664
#&gt; GSM11271     5  0.3966     -0.239  0 0.336 0.000 0.000 0.664
#&gt; GSM11288     2  0.6465      0.592  0 0.492 0.220 0.000 0.288
#&gt; GSM11273     5  0.4060      0.445  0 0.360 0.000 0.000 0.640
#&gt; GSM28757     2  0.0794      0.390  0 0.972 0.000 0.000 0.028
#&gt; GSM11282     5  0.3966      0.470  0 0.336 0.000 0.000 0.664
#&gt; GSM28756     2  0.4306      0.577  0 0.508 0.000 0.000 0.492
#&gt; GSM11276     5  0.4227     -0.437  0 0.420 0.000 0.000 0.580
#&gt; GSM28752     2  0.4307      0.570  0 0.504 0.000 0.000 0.496
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2   p3    p4    p5    p6
#&gt; GSM28762     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28764     2  0.3515      0.346  0 0.676 0.00 0.000 0.324 0.000
#&gt; GSM11274     3  0.3578      0.549  0 0.000 0.66 0.000 0.340 0.000
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11292     5  0.3578      0.661  0 0.340 0.00 0.000 0.660 0.000
#&gt; GSM28766     5  0.3578      0.661  0 0.340 0.00 0.000 0.660 0.000
#&gt; GSM11268     6  0.0000      0.905  0 0.000 0.00 0.000 0.000 1.000
#&gt; GSM28767     5  0.3578      0.661  0 0.340 0.00 0.000 0.660 0.000
#&gt; GSM11286     2  0.0146      0.812  0 0.996 0.00 0.000 0.000 0.004
#&gt; GSM28751     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28770     5  0.3578      0.661  0 0.340 0.00 0.000 0.660 0.000
#&gt; GSM11283     4  0.0000      0.825  0 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11289     5  0.3578      0.661  0 0.340 0.00 0.000 0.660 0.000
#&gt; GSM11280     2  0.3422      0.700  0 0.788 0.00 0.036 0.000 0.176
#&gt; GSM28749     2  0.5093      0.472  0 0.632 0.00 0.000 0.192 0.176
#&gt; GSM28750     6  0.2793      0.713  0 0.000 0.20 0.000 0.000 0.800
#&gt; GSM11290     3  0.0000      0.892  0 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.892  0 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28771     4  0.0000      0.825  0 0.000 0.00 1.000 0.000 0.000
#&gt; GSM28760     4  0.0000      0.825  0 0.000 0.00 1.000 0.000 0.000
#&gt; GSM28774     5  0.1663      0.667  0 0.088 0.00 0.000 0.912 0.000
#&gt; GSM11284     4  0.4660      0.444  0 0.304 0.00 0.644 0.024 0.028
#&gt; GSM28761     6  0.0000      0.905  0 0.000 0.00 0.000 0.000 1.000
#&gt; GSM11278     5  0.0000      0.683  0 0.000 0.00 0.000 1.000 0.000
#&gt; GSM11291     3  0.0000      0.892  0 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.892  0 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11272     6  0.0937      0.893  0 0.000 0.04 0.000 0.000 0.960
#&gt; GSM11285     4  0.1995      0.785  0 0.036 0.00 0.912 0.052 0.000
#&gt; GSM28753     2  0.0260      0.811  0 0.992 0.00 0.000 0.000 0.008
#&gt; GSM28773     2  0.2597      0.725  0 0.824 0.00 0.000 0.000 0.176
#&gt; GSM28765     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28768     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28754     2  0.2730      0.640  0 0.808 0.00 0.000 0.192 0.000
#&gt; GSM28769     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.00 0.000 0.000 0.000
#&gt; GSM11270     5  0.0000      0.683  0 0.000 0.00 0.000 1.000 0.000
#&gt; GSM11271     2  0.3647      0.255  0 0.640 0.00 0.000 0.360 0.000
#&gt; GSM11288     2  0.2597      0.725  0 0.824 0.00 0.000 0.000 0.176
#&gt; GSM11273     5  0.0000      0.683  0 0.000 0.00 0.000 1.000 0.000
#&gt; GSM28757     2  0.3578      0.437  0 0.660 0.00 0.000 0.340 0.000
#&gt; GSM11282     5  0.0000      0.683  0 0.000 0.00 0.000 1.000 0.000
#&gt; GSM28756     2  0.0000      0.813  0 1.000 0.00 0.000 0.000 0.000
#&gt; GSM11276     2  0.3351      0.423  0 0.712 0.00 0.000 0.288 0.000
#&gt; GSM28752     2  0.1556      0.756  0 0.920 0.00 0.000 0.080 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:pam 53     0.397 2
#> SD:pam 53     0.373 3
#> SD:pam 47     0.422 4
#> SD:pam 37     0.393 5
#> SD:pam 48     0.458 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.849           0.901       0.942         0.4788 0.525   0.525
#> 3 3 0.757           0.881       0.933         0.3258 0.735   0.539
#> 4 4 0.847           0.923       0.947         0.0438 0.888   0.731
#> 5 5 0.775           0.751       0.877         0.1397 0.857   0.602
#> 6 6 0.795           0.802       0.886         0.0540 0.962   0.824
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0376      0.941 0.004 0.996
#&gt; GSM28763     2  0.0672      0.940 0.008 0.992
#&gt; GSM28764     2  0.0000      0.942 0.000 1.000
#&gt; GSM11274     1  0.8955      0.537 0.688 0.312
#&gt; GSM28772     1  0.2603      0.954 0.956 0.044
#&gt; GSM11269     1  0.2603      0.954 0.956 0.044
#&gt; GSM28775     1  0.2603      0.954 0.956 0.044
#&gt; GSM11293     1  0.2603      0.954 0.956 0.044
#&gt; GSM28755     1  0.2603      0.954 0.956 0.044
#&gt; GSM11279     1  0.2603      0.954 0.956 0.044
#&gt; GSM28758     1  0.2603      0.954 0.956 0.044
#&gt; GSM11281     1  0.2603      0.954 0.956 0.044
#&gt; GSM11287     1  0.2603      0.954 0.956 0.044
#&gt; GSM28759     1  0.2603      0.954 0.956 0.044
#&gt; GSM11292     2  0.0000      0.942 0.000 1.000
#&gt; GSM28766     2  0.0000      0.942 0.000 1.000
#&gt; GSM11268     1  0.2423      0.944 0.960 0.040
#&gt; GSM28767     2  0.0000      0.942 0.000 1.000
#&gt; GSM11286     2  0.0000      0.942 0.000 1.000
#&gt; GSM28751     2  0.3431      0.917 0.064 0.936
#&gt; GSM28770     2  0.0000      0.942 0.000 1.000
#&gt; GSM11283     2  0.5842      0.879 0.140 0.860
#&gt; GSM11289     2  0.0376      0.941 0.004 0.996
#&gt; GSM11280     2  0.4562      0.907 0.096 0.904
#&gt; GSM28749     2  0.3274      0.925 0.060 0.940
#&gt; GSM28750     1  0.2423      0.944 0.960 0.040
#&gt; GSM11290     1  0.2423      0.944 0.960 0.040
#&gt; GSM11294     1  0.2423      0.944 0.960 0.040
#&gt; GSM28771     2  0.5842      0.879 0.140 0.860
#&gt; GSM28760     2  0.5842      0.879 0.140 0.860
#&gt; GSM28774     2  0.0000      0.942 0.000 1.000
#&gt; GSM11284     2  0.4298      0.910 0.088 0.912
#&gt; GSM28761     1  0.2423      0.944 0.960 0.040
#&gt; GSM11278     2  0.0376      0.941 0.004 0.996
#&gt; GSM11291     1  0.2423      0.944 0.960 0.040
#&gt; GSM11277     1  0.2423      0.944 0.960 0.040
#&gt; GSM11272     1  0.2423      0.944 0.960 0.040
#&gt; GSM11285     2  0.5842      0.879 0.140 0.860
#&gt; GSM28753     2  0.3114      0.924 0.056 0.944
#&gt; GSM28773     2  0.1414      0.937 0.020 0.980
#&gt; GSM28765     2  0.0000      0.942 0.000 1.000
#&gt; GSM28768     2  0.5408      0.875 0.124 0.876
#&gt; GSM28754     2  0.0000      0.942 0.000 1.000
#&gt; GSM28769     2  0.2423      0.926 0.040 0.960
#&gt; GSM11275     1  0.2603      0.954 0.956 0.044
#&gt; GSM11270     2  0.0376      0.941 0.004 0.996
#&gt; GSM11271     2  0.0000      0.942 0.000 1.000
#&gt; GSM11288     2  0.9909      0.219 0.444 0.556
#&gt; GSM11273     2  0.9933      0.205 0.452 0.548
#&gt; GSM28757     2  0.0000      0.942 0.000 1.000
#&gt; GSM11282     2  0.0376      0.941 0.004 0.996
#&gt; GSM28756     2  0.0000      0.942 0.000 1.000
#&gt; GSM11276     2  0.0000      0.942 0.000 1.000
#&gt; GSM28752     2  0.0000      0.942 0.000 1.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.3619      0.831 0.000 0.864 0.136
#&gt; GSM28763     2  0.3686      0.828 0.000 0.860 0.140
#&gt; GSM28764     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM28775     1  0.1964      0.944 0.944 0.000 0.056
#&gt; GSM11293     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM28758     1  0.1964      0.944 0.944 0.000 0.056
#&gt; GSM11281     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.980 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM28751     2  0.4099      0.828 0.008 0.852 0.140
#&gt; GSM28770     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11283     3  0.2537      0.929 0.000 0.080 0.920
#&gt; GSM11289     2  0.1163      0.873 0.000 0.972 0.028
#&gt; GSM11280     3  0.3340      0.885 0.000 0.120 0.880
#&gt; GSM28749     2  0.6192      0.411 0.000 0.580 0.420
#&gt; GSM28750     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28771     3  0.2537      0.929 0.000 0.080 0.920
#&gt; GSM28760     3  0.2537      0.929 0.000 0.080 0.920
#&gt; GSM28774     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11284     3  0.2878      0.915 0.000 0.096 0.904
#&gt; GSM28761     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11278     2  0.5431      0.692 0.000 0.716 0.284
#&gt; GSM11291     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11285     3  0.2537      0.929 0.000 0.080 0.920
#&gt; GSM28753     2  0.5926      0.568 0.000 0.644 0.356
#&gt; GSM28773     2  0.5650      0.653 0.000 0.688 0.312
#&gt; GSM28765     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM28768     2  0.6407      0.778 0.080 0.760 0.160
#&gt; GSM28754     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM28769     2  0.3752      0.826 0.000 0.856 0.144
#&gt; GSM11275     1  0.1964      0.944 0.944 0.000 0.056
#&gt; GSM11270     2  0.5650      0.654 0.000 0.688 0.312
#&gt; GSM11271     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11288     3  0.2356      0.933 0.000 0.072 0.928
#&gt; GSM11273     3  0.0424      0.952 0.000 0.008 0.992
#&gt; GSM28757     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11282     2  0.5678      0.648 0.000 0.684 0.316
#&gt; GSM28756     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.880 0.000 1.000 0.000
#&gt; GSM28752     2  0.0592      0.878 0.000 0.988 0.012
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.2081      0.913 0.000 0.916 0.000 0.084
#&gt; GSM28763     2  0.2011      0.914 0.000 0.920 0.000 0.080
#&gt; GSM28764     2  0.0707      0.922 0.000 0.980 0.000 0.020
#&gt; GSM11274     3  0.2739      0.851 0.000 0.036 0.904 0.060
#&gt; GSM28772     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0188      0.918 0.000 0.996 0.000 0.004
#&gt; GSM28766     2  0.0707      0.920 0.000 0.980 0.000 0.020
#&gt; GSM11268     3  0.0188      0.945 0.000 0.000 0.996 0.004
#&gt; GSM28767     2  0.0188      0.918 0.000 0.996 0.000 0.004
#&gt; GSM11286     2  0.1211      0.921 0.000 0.960 0.000 0.040
#&gt; GSM28751     2  0.3074      0.888 0.000 0.848 0.000 0.152
#&gt; GSM28770     2  0.0188      0.918 0.000 0.996 0.000 0.004
#&gt; GSM11283     4  0.0188      0.994 0.000 0.000 0.004 0.996
#&gt; GSM11289     2  0.1211      0.924 0.000 0.960 0.000 0.040
#&gt; GSM11280     2  0.4331      0.753 0.000 0.712 0.000 0.288
#&gt; GSM28749     2  0.3569      0.857 0.000 0.804 0.000 0.196
#&gt; GSM28750     3  0.0188      0.945 0.000 0.000 0.996 0.004
#&gt; GSM11290     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.0188      0.994 0.000 0.000 0.004 0.996
#&gt; GSM28760     4  0.0188      0.994 0.000 0.000 0.004 0.996
#&gt; GSM28774     2  0.0000      0.919 0.000 1.000 0.000 0.000
#&gt; GSM11284     2  0.4277      0.764 0.000 0.720 0.000 0.280
#&gt; GSM28761     3  0.0188      0.945 0.000 0.000 0.996 0.004
#&gt; GSM11278     2  0.2224      0.908 0.000 0.928 0.032 0.040
#&gt; GSM11291     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.0188      0.945 0.000 0.000 0.996 0.004
#&gt; GSM11285     4  0.0657      0.981 0.000 0.012 0.004 0.984
#&gt; GSM28753     2  0.3583      0.867 0.000 0.816 0.004 0.180
#&gt; GSM28773     2  0.3208      0.887 0.000 0.848 0.004 0.148
#&gt; GSM28765     2  0.0921      0.922 0.000 0.972 0.000 0.028
#&gt; GSM28768     2  0.3577      0.882 0.012 0.832 0.000 0.156
#&gt; GSM28754     2  0.0000      0.919 0.000 1.000 0.000 0.000
#&gt; GSM28769     2  0.3157      0.890 0.000 0.852 0.004 0.144
#&gt; GSM11275     1  0.1022      0.957 0.968 0.000 0.000 0.032
#&gt; GSM11270     2  0.2644      0.901 0.000 0.908 0.032 0.060
#&gt; GSM11271     2  0.0188      0.918 0.000 0.996 0.000 0.004
#&gt; GSM11288     2  0.4514      0.847 0.000 0.796 0.056 0.148
#&gt; GSM11273     3  0.5240      0.611 0.000 0.188 0.740 0.072
#&gt; GSM28757     2  0.0592      0.921 0.000 0.984 0.000 0.016
#&gt; GSM11282     2  0.2644      0.901 0.000 0.908 0.032 0.060
#&gt; GSM28756     2  0.0188      0.918 0.000 0.996 0.000 0.004
#&gt; GSM11276     2  0.0336      0.920 0.000 0.992 0.000 0.008
#&gt; GSM28752     2  0.1474      0.919 0.000 0.948 0.000 0.052
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     5  0.4074     0.7222 0.000 0.364 0.000 0.000 0.636
#&gt; GSM28763     5  0.4126     0.6997 0.000 0.380 0.000 0.000 0.620
#&gt; GSM28764     2  0.0404     0.8695 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11274     3  0.3551     0.7163 0.000 0.000 0.772 0.008 0.220
#&gt; GSM28772     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0162     0.9455 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11293     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9499 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0000     0.8681 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28766     2  0.0162     0.8659 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11268     3  0.0162     0.8949 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28767     2  0.0000     0.8681 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11286     2  0.1908     0.7879 0.000 0.908 0.000 0.000 0.092
#&gt; GSM28751     5  0.4074     0.7222 0.000 0.364 0.000 0.000 0.636
#&gt; GSM28770     2  0.0000     0.8681 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11283     4  0.0703     0.8401 0.000 0.000 0.000 0.976 0.024
#&gt; GSM11289     2  0.3999     0.2779 0.000 0.656 0.000 0.000 0.344
#&gt; GSM11280     5  0.3442     0.6991 0.000 0.104 0.000 0.060 0.836
#&gt; GSM28749     5  0.3276     0.7283 0.000 0.132 0.000 0.032 0.836
#&gt; GSM28750     3  0.0162     0.8949 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11290     3  0.0000     0.8946 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000     0.8946 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28771     4  0.0703     0.8401 0.000 0.000 0.000 0.976 0.024
#&gt; GSM28760     4  0.0703     0.8401 0.000 0.000 0.000 0.976 0.024
#&gt; GSM28774     2  0.0404     0.8695 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11284     5  0.3427     0.7053 0.000 0.108 0.000 0.056 0.836
#&gt; GSM28761     3  0.0162     0.8949 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11278     2  0.5029    -0.2537 0.000 0.528 0.004 0.024 0.444
#&gt; GSM11291     3  0.0000     0.8946 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000     0.8946 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.0162     0.8949 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11285     4  0.4182     0.2800 0.000 0.000 0.000 0.600 0.400
#&gt; GSM28753     5  0.3229     0.7282 0.000 0.128 0.000 0.032 0.840
#&gt; GSM28773     5  0.4360     0.7507 0.000 0.300 0.000 0.020 0.680
#&gt; GSM28765     2  0.0510     0.8688 0.000 0.984 0.000 0.000 0.016
#&gt; GSM28768     5  0.4891     0.7368 0.044 0.316 0.000 0.000 0.640
#&gt; GSM28754     2  0.0703     0.8629 0.000 0.976 0.000 0.000 0.024
#&gt; GSM28769     5  0.4015     0.7361 0.000 0.348 0.000 0.000 0.652
#&gt; GSM11275     1  0.4074     0.3720 0.636 0.000 0.000 0.000 0.364
#&gt; GSM11270     2  0.5050    -0.2954 0.000 0.496 0.004 0.024 0.476
#&gt; GSM11271     2  0.0000     0.8681 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11288     5  0.3317     0.7172 0.000 0.116 0.000 0.044 0.840
#&gt; GSM11273     3  0.7032     0.0702 0.000 0.384 0.400 0.020 0.196
#&gt; GSM28757     2  0.0609     0.8660 0.000 0.980 0.000 0.000 0.020
#&gt; GSM11282     5  0.5045     0.1912 0.000 0.464 0.004 0.024 0.508
#&gt; GSM28756     2  0.0000     0.8681 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11276     2  0.0404     0.8695 0.000 0.988 0.000 0.000 0.012
#&gt; GSM28752     2  0.0609     0.8671 0.000 0.980 0.000 0.000 0.020
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.3245      0.745 0.000 0.764 0.000 0.000 0.228 0.008
#&gt; GSM28763     2  0.3373      0.719 0.000 0.744 0.000 0.000 0.248 0.008
#&gt; GSM28764     5  0.0858      0.899 0.000 0.028 0.000 0.000 0.968 0.004
#&gt; GSM11274     3  0.3795      0.471 0.000 0.004 0.632 0.000 0.000 0.364
#&gt; GSM28772     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0363      0.967 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM11293     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0363      0.967 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM11281     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.973 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000      0.910 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28766     5  0.0000      0.910 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11268     3  0.0777      0.863 0.000 0.000 0.972 0.004 0.000 0.024
#&gt; GSM28767     5  0.0000      0.910 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11286     5  0.3265      0.574 0.000 0.248 0.000 0.000 0.748 0.004
#&gt; GSM28751     2  0.3171      0.758 0.000 0.784 0.000 0.000 0.204 0.012
#&gt; GSM28770     5  0.0000      0.910 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11283     4  0.0146      0.845 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM11289     5  0.3756      0.291 0.000 0.400 0.000 0.000 0.600 0.000
#&gt; GSM11280     2  0.1226      0.746 0.000 0.952 0.000 0.040 0.004 0.004
#&gt; GSM28749     2  0.1138      0.754 0.000 0.960 0.000 0.024 0.012 0.004
#&gt; GSM28750     3  0.0405      0.864 0.000 0.000 0.988 0.004 0.000 0.008
#&gt; GSM11290     3  0.2340      0.859 0.000 0.000 0.852 0.000 0.000 0.148
#&gt; GSM11294     3  0.2378      0.859 0.000 0.000 0.848 0.000 0.000 0.152
#&gt; GSM28771     4  0.0146      0.845 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM28760     4  0.0146      0.845 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM28774     5  0.0260      0.909 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; GSM11284     2  0.1857      0.744 0.000 0.924 0.000 0.044 0.028 0.004
#&gt; GSM28761     3  0.0777      0.863 0.000 0.000 0.972 0.004 0.000 0.024
#&gt; GSM11278     6  0.5702      0.658 0.000 0.180 0.000 0.000 0.324 0.496
#&gt; GSM11291     3  0.2378      0.859 0.000 0.000 0.848 0.000 0.000 0.152
#&gt; GSM11277     3  0.2378      0.859 0.000 0.000 0.848 0.000 0.000 0.152
#&gt; GSM11272     3  0.0777      0.863 0.000 0.000 0.972 0.004 0.000 0.024
#&gt; GSM11285     4  0.3714      0.439 0.000 0.340 0.004 0.656 0.000 0.000
#&gt; GSM28753     2  0.1138      0.754 0.000 0.960 0.000 0.024 0.012 0.004
#&gt; GSM28773     2  0.4014      0.651 0.000 0.696 0.000 0.024 0.276 0.004
#&gt; GSM28765     5  0.1007      0.889 0.000 0.044 0.000 0.000 0.956 0.000
#&gt; GSM28768     2  0.3470      0.759 0.020 0.792 0.000 0.000 0.176 0.012
#&gt; GSM28754     5  0.0937      0.883 0.000 0.040 0.000 0.000 0.960 0.000
#&gt; GSM28769     2  0.3201      0.760 0.000 0.780 0.000 0.000 0.208 0.012
#&gt; GSM11275     1  0.3121      0.718 0.804 0.180 0.004 0.000 0.000 0.012
#&gt; GSM11270     6  0.5624      0.688 0.000 0.180 0.000 0.000 0.296 0.524
#&gt; GSM11271     5  0.0000      0.910 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11288     2  0.1553      0.743 0.000 0.944 0.004 0.032 0.008 0.012
#&gt; GSM11273     6  0.4569     -0.199 0.000 0.016 0.396 0.000 0.016 0.572
#&gt; GSM28757     5  0.0363      0.908 0.000 0.012 0.000 0.000 0.988 0.000
#&gt; GSM11282     6  0.5731      0.689 0.000 0.176 0.004 0.000 0.296 0.524
#&gt; GSM28756     5  0.0000      0.910 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11276     5  0.0260      0.909 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; GSM28752     5  0.1663      0.840 0.000 0.088 0.000 0.000 0.912 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:mclust 52     0.396 2
#> SD:mclust 53     0.443 3
#> SD:mclust 54     0.523 4
#> SD:mclust 47     0.404 5
#> SD:mclust 50     0.400 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.972       0.988         0.3830 0.628   0.628
#> 3 3 1.000           0.978       0.990         0.5362 0.740   0.599
#> 4 4 0.779           0.796       0.885         0.1837 0.859   0.660
#> 5 5 0.959           0.920       0.960         0.0866 0.943   0.804
#> 6 6 0.805           0.787       0.868         0.0844 0.916   0.661
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.985 0.000 1.000
#&gt; GSM28763     2  0.2423      0.948 0.040 0.960
#&gt; GSM28764     2  0.0000      0.985 0.000 1.000
#&gt; GSM11274     2  0.0000      0.985 0.000 1.000
#&gt; GSM28772     1  0.0000      0.996 1.000 0.000
#&gt; GSM11269     1  0.0000      0.996 1.000 0.000
#&gt; GSM28775     1  0.0000      0.996 1.000 0.000
#&gt; GSM11293     1  0.0000      0.996 1.000 0.000
#&gt; GSM28755     1  0.0000      0.996 1.000 0.000
#&gt; GSM11279     1  0.0000      0.996 1.000 0.000
#&gt; GSM28758     1  0.0000      0.996 1.000 0.000
#&gt; GSM11281     1  0.0000      0.996 1.000 0.000
#&gt; GSM11287     1  0.0000      0.996 1.000 0.000
#&gt; GSM28759     1  0.0000      0.996 1.000 0.000
#&gt; GSM11292     2  0.0000      0.985 0.000 1.000
#&gt; GSM28766     2  0.0000      0.985 0.000 1.000
#&gt; GSM11268     2  0.0000      0.985 0.000 1.000
#&gt; GSM28767     2  0.0000      0.985 0.000 1.000
#&gt; GSM11286     2  0.0000      0.985 0.000 1.000
#&gt; GSM28751     1  0.2948      0.943 0.948 0.052
#&gt; GSM28770     2  0.0000      0.985 0.000 1.000
#&gt; GSM11283     2  0.0000      0.985 0.000 1.000
#&gt; GSM11289     2  0.0000      0.985 0.000 1.000
#&gt; GSM11280     2  0.0000      0.985 0.000 1.000
#&gt; GSM28749     2  0.0000      0.985 0.000 1.000
#&gt; GSM28750     2  0.0000      0.985 0.000 1.000
#&gt; GSM11290     2  0.0000      0.985 0.000 1.000
#&gt; GSM11294     2  0.0000      0.985 0.000 1.000
#&gt; GSM28771     2  0.0000      0.985 0.000 1.000
#&gt; GSM28760     2  0.0000      0.985 0.000 1.000
#&gt; GSM28774     2  0.0000      0.985 0.000 1.000
#&gt; GSM11284     2  0.0000      0.985 0.000 1.000
#&gt; GSM28761     2  0.0000      0.985 0.000 1.000
#&gt; GSM11278     2  0.0000      0.985 0.000 1.000
#&gt; GSM11291     2  0.0000      0.985 0.000 1.000
#&gt; GSM11277     2  0.0000      0.985 0.000 1.000
#&gt; GSM11272     2  0.7815      0.703 0.232 0.768
#&gt; GSM11285     2  0.0000      0.985 0.000 1.000
#&gt; GSM28753     2  0.0000      0.985 0.000 1.000
#&gt; GSM28773     2  0.0000      0.985 0.000 1.000
#&gt; GSM28765     2  0.0000      0.985 0.000 1.000
#&gt; GSM28768     1  0.0000      0.996 1.000 0.000
#&gt; GSM28754     2  0.0000      0.985 0.000 1.000
#&gt; GSM28769     2  0.9087      0.532 0.324 0.676
#&gt; GSM11275     1  0.0000      0.996 1.000 0.000
#&gt; GSM11270     2  0.0000      0.985 0.000 1.000
#&gt; GSM11271     2  0.0000      0.985 0.000 1.000
#&gt; GSM11288     2  0.0672      0.978 0.008 0.992
#&gt; GSM11273     2  0.0000      0.985 0.000 1.000
#&gt; GSM28757     2  0.0000      0.985 0.000 1.000
#&gt; GSM11282     2  0.0000      0.985 0.000 1.000
#&gt; GSM28756     2  0.0000      0.985 0.000 1.000
#&gt; GSM11276     2  0.0000      0.985 0.000 1.000
#&gt; GSM28752     2  0.0000      0.985 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28751     2  0.1753      0.948 0.048 0.952 0.000
#&gt; GSM28770     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28749     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28750     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM28771     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28760     2  0.1964      0.939 0.000 0.944 0.056
#&gt; GSM28774     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM11278     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11291     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM11272     3  0.0237      0.963 0.004 0.000 0.996
#&gt; GSM11285     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28768     1  0.2537      0.882 0.920 0.080 0.000
#&gt; GSM28754     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28769     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11275     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11270     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11271     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11288     3  0.7930      0.578 0.172 0.164 0.664
#&gt; GSM11273     3  0.0000      0.966 0.000 0.000 1.000
#&gt; GSM28757     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11282     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28756     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.996 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.0188      0.932 0.000 0.996 0.000 0.004
#&gt; GSM28763     2  0.0707      0.924 0.000 0.980 0.000 0.020
#&gt; GSM28764     2  0.0188      0.933 0.000 0.996 0.000 0.004
#&gt; GSM11274     3  0.0469      0.785 0.000 0.000 0.988 0.012
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.1211      0.909 0.000 0.960 0.000 0.040
#&gt; GSM28766     2  0.1389      0.900 0.000 0.952 0.000 0.048
#&gt; GSM11268     3  0.4992      0.588 0.000 0.000 0.524 0.476
#&gt; GSM28767     2  0.0336      0.932 0.000 0.992 0.000 0.008
#&gt; GSM11286     2  0.0336      0.932 0.000 0.992 0.000 0.008
#&gt; GSM28751     2  0.2924      0.792 0.100 0.884 0.000 0.016
#&gt; GSM28770     2  0.0188      0.933 0.000 0.996 0.000 0.004
#&gt; GSM11283     4  0.4843      0.559 0.000 0.396 0.000 0.604
#&gt; GSM11289     2  0.1211      0.904 0.000 0.960 0.000 0.040
#&gt; GSM11280     4  0.4564      0.619 0.000 0.328 0.000 0.672
#&gt; GSM28749     4  0.4804      0.394 0.000 0.384 0.000 0.616
#&gt; GSM28750     3  0.4866      0.638 0.000 0.000 0.596 0.404
#&gt; GSM11290     3  0.0000      0.790 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.790 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.4313      0.614 0.000 0.260 0.004 0.736
#&gt; GSM28760     4  0.5229      0.356 0.000 0.084 0.168 0.748
#&gt; GSM28774     2  0.0188      0.932 0.000 0.996 0.000 0.004
#&gt; GSM11284     2  0.4331      0.350 0.000 0.712 0.000 0.288
#&gt; GSM28761     3  0.5165      0.573 0.000 0.004 0.512 0.484
#&gt; GSM11278     2  0.1059      0.920 0.000 0.972 0.012 0.016
#&gt; GSM11291     3  0.0000      0.790 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.790 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.4955      0.616 0.000 0.000 0.556 0.444
#&gt; GSM11285     4  0.4916      0.517 0.000 0.424 0.000 0.576
#&gt; GSM28753     4  0.4985      0.446 0.000 0.468 0.000 0.532
#&gt; GSM28773     4  0.4981      0.247 0.000 0.464 0.000 0.536
#&gt; GSM28765     2  0.0817      0.924 0.000 0.976 0.000 0.024
#&gt; GSM28768     1  0.0336      0.989 0.992 0.008 0.000 0.000
#&gt; GSM28754     2  0.0188      0.932 0.000 0.996 0.000 0.004
#&gt; GSM28769     2  0.2814      0.781 0.000 0.868 0.000 0.132
#&gt; GSM11275     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.3335      0.746 0.000 0.856 0.128 0.016
#&gt; GSM11271     2  0.0336      0.932 0.000 0.992 0.000 0.008
#&gt; GSM11288     4  0.6091     -0.126 0.124 0.004 0.180 0.692
#&gt; GSM11273     3  0.0469      0.785 0.000 0.000 0.988 0.012
#&gt; GSM28757     2  0.0336      0.932 0.000 0.992 0.000 0.008
#&gt; GSM11282     2  0.0937      0.923 0.000 0.976 0.012 0.012
#&gt; GSM28756     2  0.0188      0.932 0.000 0.996 0.000 0.004
#&gt; GSM11276     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM28752     2  0.0707      0.926 0.000 0.980 0.000 0.020
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0898      0.938 0.000 0.972 0.000 0.020 0.008
#&gt; GSM28763     2  0.0880      0.934 0.000 0.968 0.000 0.032 0.000
#&gt; GSM28764     2  0.0671      0.939 0.000 0.980 0.000 0.004 0.016
#&gt; GSM11274     3  0.0162      0.978 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28772     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0955      0.936 0.000 0.968 0.000 0.004 0.028
#&gt; GSM28766     2  0.1430      0.925 0.000 0.944 0.000 0.004 0.052
#&gt; GSM11268     5  0.0290      0.967 0.000 0.000 0.008 0.000 0.992
#&gt; GSM28767     2  0.0451      0.940 0.000 0.988 0.000 0.004 0.008
#&gt; GSM11286     2  0.1043      0.936 0.000 0.960 0.000 0.000 0.040
#&gt; GSM28751     2  0.3458      0.817 0.120 0.840 0.000 0.016 0.024
#&gt; GSM28770     2  0.0162      0.939 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11283     4  0.0000      0.865 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11289     2  0.1544      0.901 0.000 0.932 0.000 0.068 0.000
#&gt; GSM11280     4  0.0898      0.858 0.000 0.008 0.000 0.972 0.020
#&gt; GSM28749     5  0.0880      0.947 0.000 0.032 0.000 0.000 0.968
#&gt; GSM28750     5  0.1478      0.925 0.000 0.000 0.064 0.000 0.936
#&gt; GSM11290     3  0.1043      0.974 0.000 0.000 0.960 0.000 0.040
#&gt; GSM11294     3  0.0703      0.986 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28771     4  0.0000      0.865 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28760     4  0.0000      0.865 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28774     2  0.0451      0.938 0.000 0.988 0.008 0.000 0.004
#&gt; GSM11284     4  0.4415      0.162 0.000 0.444 0.000 0.552 0.004
#&gt; GSM28761     5  0.0290      0.967 0.000 0.000 0.008 0.000 0.992
#&gt; GSM11278     2  0.2017      0.894 0.000 0.912 0.080 0.000 0.008
#&gt; GSM11291     3  0.0703      0.986 0.000 0.000 0.976 0.000 0.024
#&gt; GSM11277     3  0.0703      0.986 0.000 0.000 0.976 0.000 0.024
#&gt; GSM11272     5  0.0609      0.963 0.000 0.000 0.020 0.000 0.980
#&gt; GSM11285     4  0.0162      0.865 0.000 0.004 0.000 0.996 0.000
#&gt; GSM28753     4  0.2069      0.809 0.000 0.076 0.000 0.912 0.012
#&gt; GSM28773     5  0.0880      0.947 0.000 0.032 0.000 0.000 0.968
#&gt; GSM28765     2  0.0963      0.937 0.000 0.964 0.000 0.000 0.036
#&gt; GSM28768     1  0.0404      0.983 0.988 0.012 0.000 0.000 0.000
#&gt; GSM28754     2  0.0693      0.937 0.000 0.980 0.008 0.000 0.012
#&gt; GSM28769     2  0.4734      0.672 0.000 0.724 0.000 0.088 0.188
#&gt; GSM11275     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.3980      0.636 0.000 0.708 0.284 0.000 0.008
#&gt; GSM11271     2  0.0162      0.939 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11288     5  0.0740      0.965 0.000 0.008 0.004 0.008 0.980
#&gt; GSM11273     3  0.0162      0.973 0.000 0.004 0.996 0.000 0.000
#&gt; GSM28757     2  0.0880      0.936 0.000 0.968 0.000 0.000 0.032
#&gt; GSM11282     2  0.1408      0.921 0.000 0.948 0.044 0.000 0.008
#&gt; GSM28756     2  0.0290      0.938 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11276     2  0.0162      0.939 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28752     2  0.0510      0.939 0.000 0.984 0.000 0.000 0.016
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     5  0.3995    -0.2367 0.000 0.480 0.000 0.000 0.516 0.004
#&gt; GSM28763     2  0.4058     0.5827 0.000 0.616 0.000 0.008 0.372 0.004
#&gt; GSM28764     5  0.1007     0.8001 0.000 0.044 0.000 0.000 0.956 0.000
#&gt; GSM11274     3  0.0547     0.9653 0.000 0.020 0.980 0.000 0.000 0.000
#&gt; GSM28772     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0146     0.9785 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0146     0.9785 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0146     0.9785 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0146     0.9785 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0777     0.7901 0.000 0.004 0.000 0.000 0.972 0.024
#&gt; GSM28766     5  0.2743     0.6653 0.000 0.008 0.000 0.000 0.828 0.164
#&gt; GSM11268     6  0.0713     0.8836 0.000 0.028 0.000 0.000 0.000 0.972
#&gt; GSM28767     5  0.0547     0.8040 0.000 0.020 0.000 0.000 0.980 0.000
#&gt; GSM11286     2  0.2744     0.7344 0.000 0.840 0.000 0.000 0.144 0.016
#&gt; GSM28751     5  0.4594     0.6265 0.056 0.236 0.000 0.000 0.692 0.016
#&gt; GSM28770     5  0.0547     0.8033 0.000 0.020 0.000 0.000 0.980 0.000
#&gt; GSM11283     4  0.0000     0.7860 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11289     5  0.0692     0.7902 0.000 0.004 0.000 0.020 0.976 0.000
#&gt; GSM11280     4  0.3717     0.6505 0.000 0.276 0.000 0.708 0.000 0.016
#&gt; GSM28749     6  0.3161     0.7797 0.000 0.216 0.000 0.000 0.008 0.776
#&gt; GSM28750     6  0.1321     0.8735 0.000 0.024 0.020 0.000 0.004 0.952
#&gt; GSM11290     3  0.0865     0.9698 0.000 0.000 0.964 0.000 0.000 0.036
#&gt; GSM11294     3  0.0547     0.9789 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM28771     4  0.0000     0.7860 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28760     4  0.0000     0.7860 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28774     2  0.3547     0.7617 0.000 0.668 0.000 0.000 0.332 0.000
#&gt; GSM11284     4  0.5167     0.0635 0.000 0.412 0.000 0.500 0.088 0.000
#&gt; GSM28761     6  0.0547     0.8847 0.000 0.020 0.000 0.000 0.000 0.980
#&gt; GSM11278     2  0.4764     0.7485 0.000 0.640 0.088 0.000 0.272 0.000
#&gt; GSM11291     3  0.0547     0.9789 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM11277     3  0.0458     0.9788 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11272     6  0.1321     0.8719 0.000 0.024 0.004 0.000 0.020 0.952
#&gt; GSM11285     4  0.0146     0.7849 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28753     4  0.5643     0.5382 0.000 0.140 0.000 0.624 0.200 0.036
#&gt; GSM28773     6  0.4178     0.6082 0.000 0.372 0.000 0.000 0.020 0.608
#&gt; GSM28765     2  0.4252     0.6108 0.000 0.604 0.000 0.000 0.372 0.024
#&gt; GSM28768     1  0.3088     0.7541 0.808 0.172 0.000 0.000 0.020 0.000
#&gt; GSM28754     2  0.3290     0.7765 0.000 0.744 0.000 0.000 0.252 0.004
#&gt; GSM28769     5  0.5100     0.5949 0.008 0.220 0.000 0.012 0.668 0.092
#&gt; GSM11275     1  0.0146     0.9785 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.4845     0.5847 0.000 0.628 0.280 0.000 0.092 0.000
#&gt; GSM11271     5  0.1075     0.7984 0.000 0.048 0.000 0.000 0.952 0.000
#&gt; GSM11288     6  0.1138     0.8832 0.000 0.024 0.004 0.000 0.012 0.960
#&gt; GSM11273     3  0.0777     0.9604 0.000 0.024 0.972 0.000 0.004 0.000
#&gt; GSM28757     2  0.2432     0.7046 0.000 0.876 0.000 0.000 0.100 0.024
#&gt; GSM11282     2  0.4670     0.7558 0.000 0.636 0.072 0.000 0.292 0.000
#&gt; GSM28756     2  0.3714     0.7568 0.000 0.656 0.000 0.000 0.340 0.004
#&gt; GSM11276     5  0.1444     0.7870 0.000 0.072 0.000 0.000 0.928 0.000
#&gt; GSM28752     5  0.2340     0.7292 0.000 0.148 0.000 0.000 0.852 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:NMF 54     0.398 2
#> SD:NMF 54     0.374 3
#> SD:NMF 48     0.510 4
#> SD:NMF 53     0.440 5
#> SD:NMF 52     0.430 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.988       0.986         0.3343 0.669   0.669
#> 3 3 0.863           0.912       0.951         0.6661 0.786   0.681
#> 4 4 0.767           0.871       0.892         0.1742 0.855   0.681
#> 5 5 0.799           0.873       0.912         0.0671 0.980   0.938
#> 6 6 0.789           0.704       0.750         0.0866 0.957   0.867
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.1843      0.988 0.028 0.972
#&gt; GSM28763     2  0.1843      0.988 0.028 0.972
#&gt; GSM28764     2  0.1843      0.988 0.028 0.972
#&gt; GSM11274     2  0.0000      0.982 0.000 1.000
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000
#&gt; GSM28758     1  0.0376      0.996 0.996 0.004
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000
#&gt; GSM11292     2  0.1843      0.988 0.028 0.972
#&gt; GSM28766     2  0.1843      0.988 0.028 0.972
#&gt; GSM11268     2  0.0000      0.982 0.000 1.000
#&gt; GSM28767     2  0.1843      0.988 0.028 0.972
#&gt; GSM11286     2  0.1843      0.988 0.028 0.972
#&gt; GSM28751     2  0.1843      0.988 0.028 0.972
#&gt; GSM28770     2  0.1843      0.988 0.028 0.972
#&gt; GSM11283     2  0.0000      0.982 0.000 1.000
#&gt; GSM11289     2  0.1843      0.988 0.028 0.972
#&gt; GSM11280     2  0.1843      0.988 0.028 0.972
#&gt; GSM28749     2  0.1843      0.988 0.028 0.972
#&gt; GSM28750     2  0.0000      0.982 0.000 1.000
#&gt; GSM11290     2  0.0000      0.982 0.000 1.000
#&gt; GSM11294     2  0.0000      0.982 0.000 1.000
#&gt; GSM28771     2  0.0000      0.982 0.000 1.000
#&gt; GSM28760     2  0.0000      0.982 0.000 1.000
#&gt; GSM28774     2  0.1843      0.988 0.028 0.972
#&gt; GSM11284     2  0.1843      0.988 0.028 0.972
#&gt; GSM28761     2  0.0000      0.982 0.000 1.000
#&gt; GSM11278     2  0.0000      0.982 0.000 1.000
#&gt; GSM11291     2  0.0000      0.982 0.000 1.000
#&gt; GSM11277     2  0.0000      0.982 0.000 1.000
#&gt; GSM11272     2  0.0000      0.982 0.000 1.000
#&gt; GSM11285     2  0.0000      0.982 0.000 1.000
#&gt; GSM28753     2  0.1843      0.988 0.028 0.972
#&gt; GSM28773     2  0.1843      0.988 0.028 0.972
#&gt; GSM28765     2  0.1843      0.988 0.028 0.972
#&gt; GSM28768     2  0.2603      0.976 0.044 0.956
#&gt; GSM28754     2  0.1843      0.988 0.028 0.972
#&gt; GSM28769     2  0.1843      0.988 0.028 0.972
#&gt; GSM11275     1  0.0376      0.996 0.996 0.004
#&gt; GSM11270     2  0.0000      0.982 0.000 1.000
#&gt; GSM11271     2  0.1843      0.988 0.028 0.972
#&gt; GSM11288     2  0.1843      0.988 0.028 0.972
#&gt; GSM11273     2  0.0000      0.982 0.000 1.000
#&gt; GSM28757     2  0.1843      0.988 0.028 0.972
#&gt; GSM11282     2  0.0000      0.982 0.000 1.000
#&gt; GSM28756     2  0.1843      0.988 0.028 0.972
#&gt; GSM11276     2  0.1843      0.988 0.028 0.972
#&gt; GSM28752     2  0.1843      0.988 0.028 0.972
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28763     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28764     2  0.0424      0.941 0.000 0.992 0.008
#&gt; GSM11274     3  0.7050      0.292 0.028 0.372 0.600
#&gt; GSM28772     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM11269     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM28775     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM11293     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM28755     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM11279     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM28758     1  0.1289      0.995 0.968 0.032 0.000
#&gt; GSM11281     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM11287     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM28759     1  0.1163      0.999 0.972 0.028 0.000
#&gt; GSM11292     2  0.1643      0.926 0.000 0.956 0.044
#&gt; GSM28766     2  0.1643      0.926 0.000 0.956 0.044
#&gt; GSM11268     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM28767     2  0.1529      0.928 0.000 0.960 0.040
#&gt; GSM11286     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28751     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28770     2  0.1643      0.926 0.000 0.956 0.044
#&gt; GSM11283     2  0.1399      0.928 0.028 0.968 0.004
#&gt; GSM11289     2  0.1643      0.926 0.000 0.956 0.044
#&gt; GSM11280     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28749     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28750     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM11290     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM11294     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM28771     2  0.1399      0.928 0.028 0.968 0.004
#&gt; GSM28760     2  0.1399      0.928 0.028 0.968 0.004
#&gt; GSM28774     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM28761     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM11278     2  0.6337      0.655 0.028 0.708 0.264
#&gt; GSM11291     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM11277     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM11272     3  0.0237      0.931 0.000 0.004 0.996
#&gt; GSM11285     2  0.1399      0.928 0.028 0.968 0.004
#&gt; GSM28753     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28773     2  0.2066      0.916 0.000 0.940 0.060
#&gt; GSM28765     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28768     2  0.0983      0.933 0.016 0.980 0.004
#&gt; GSM28754     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28769     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM11275     1  0.1289      0.995 0.968 0.032 0.000
#&gt; GSM11270     2  0.6337      0.655 0.028 0.708 0.264
#&gt; GSM11271     2  0.1411      0.930 0.000 0.964 0.036
#&gt; GSM11288     2  0.4750      0.761 0.000 0.784 0.216
#&gt; GSM11273     2  0.6337      0.655 0.028 0.708 0.264
#&gt; GSM28757     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM11282     2  0.6108      0.692 0.028 0.732 0.240
#&gt; GSM28756     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM11276     2  0.0237      0.941 0.000 0.996 0.004
#&gt; GSM28752     2  0.0237      0.941 0.000 0.996 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28764     2  0.0524      0.931 0.000 0.988 0.004 0.008
#&gt; GSM11274     3  0.5861      0.149 0.000 0.032 0.488 0.480
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0188      0.996 0.996 0.004 0.000 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.2996      0.860 0.000 0.892 0.044 0.064
#&gt; GSM28766     2  0.2996      0.860 0.000 0.892 0.044 0.064
#&gt; GSM11268     3  0.3172      0.826 0.000 0.000 0.840 0.160
#&gt; GSM28767     2  0.2586      0.881 0.000 0.912 0.040 0.048
#&gt; GSM11286     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28770     2  0.2996      0.860 0.000 0.892 0.044 0.064
#&gt; GSM11283     4  0.4406      0.748 0.000 0.300 0.000 0.700
#&gt; GSM11289     2  0.2996      0.860 0.000 0.892 0.044 0.064
#&gt; GSM11280     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28750     3  0.3123      0.827 0.000 0.000 0.844 0.156
#&gt; GSM11290     3  0.1940      0.833 0.000 0.000 0.924 0.076
#&gt; GSM11294     3  0.1940      0.833 0.000 0.000 0.924 0.076
#&gt; GSM28771     4  0.4406      0.748 0.000 0.300 0.000 0.700
#&gt; GSM28760     4  0.4406      0.748 0.000 0.300 0.000 0.700
#&gt; GSM28774     2  0.1716      0.899 0.000 0.936 0.000 0.064
#&gt; GSM11284     2  0.1716      0.899 0.000 0.936 0.000 0.064
#&gt; GSM28761     3  0.3172      0.826 0.000 0.000 0.840 0.160
#&gt; GSM11278     4  0.7205      0.716 0.000 0.344 0.152 0.504
#&gt; GSM11291     3  0.1940      0.833 0.000 0.000 0.924 0.076
#&gt; GSM11277     3  0.1940      0.833 0.000 0.000 0.924 0.076
#&gt; GSM11272     3  0.3172      0.826 0.000 0.000 0.840 0.160
#&gt; GSM11285     4  0.4406      0.748 0.000 0.300 0.000 0.700
#&gt; GSM28753     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28773     2  0.2466      0.859 0.000 0.916 0.056 0.028
#&gt; GSM28765     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28768     2  0.0592      0.921 0.016 0.984 0.000 0.000
#&gt; GSM28754     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM11275     1  0.0188      0.996 0.996 0.004 0.000 0.000
#&gt; GSM11270     4  0.7205      0.716 0.000 0.344 0.152 0.504
#&gt; GSM11271     2  0.2319      0.892 0.000 0.924 0.036 0.040
#&gt; GSM11288     2  0.4086      0.591 0.000 0.776 0.216 0.008
#&gt; GSM11273     4  0.7205      0.716 0.000 0.344 0.152 0.504
#&gt; GSM28757     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM11282     4  0.7012      0.697 0.000 0.372 0.124 0.504
#&gt; GSM28756     2  0.0000      0.935 0.000 1.000 0.000 0.000
#&gt; GSM11276     2  0.0188      0.934 0.000 0.996 0.000 0.004
#&gt; GSM28752     2  0.0000      0.935 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.0807      0.915 0.000 0.976 0.000 0.012 0.012
#&gt; GSM11274     5  0.0000      0.149 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0162      0.995 0.996 0.004 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.3165      0.821 0.000 0.848 0.000 0.036 0.116
#&gt; GSM28766     2  0.3165      0.821 0.000 0.848 0.000 0.036 0.116
#&gt; GSM11268     3  0.0000      0.719 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28767     2  0.2574      0.850 0.000 0.876 0.000 0.012 0.112
#&gt; GSM11286     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28770     2  0.3165      0.821 0.000 0.848 0.000 0.036 0.116
#&gt; GSM11283     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11289     2  0.3165      0.821 0.000 0.848 0.000 0.036 0.116
#&gt; GSM11280     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28750     3  0.0162      0.719 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11290     3  0.4302      0.709 0.000 0.000 0.520 0.000 0.480
#&gt; GSM11294     3  0.4305      0.706 0.000 0.000 0.512 0.000 0.488
#&gt; GSM28771     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28760     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28774     2  0.2554      0.864 0.000 0.892 0.000 0.036 0.072
#&gt; GSM11284     2  0.2554      0.864 0.000 0.892 0.000 0.036 0.072
#&gt; GSM28761     3  0.0000      0.719 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11278     5  0.4677      0.845 0.000 0.300 0.000 0.036 0.664
#&gt; GSM11291     3  0.4305      0.706 0.000 0.000 0.512 0.000 0.488
#&gt; GSM11277     3  0.4305      0.706 0.000 0.000 0.512 0.000 0.488
#&gt; GSM11272     3  0.0000      0.719 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11285     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.2313      0.858 0.000 0.912 0.044 0.004 0.040
#&gt; GSM28765     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.0510      0.911 0.016 0.984 0.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.0162      0.995 0.996 0.004 0.000 0.000 0.000
#&gt; GSM11270     5  0.4677      0.845 0.000 0.300 0.000 0.036 0.664
#&gt; GSM11271     2  0.2361      0.865 0.000 0.892 0.000 0.012 0.096
#&gt; GSM11288     2  0.3628      0.639 0.000 0.772 0.216 0.000 0.012
#&gt; GSM11273     5  0.4677      0.845 0.000 0.300 0.000 0.036 0.664
#&gt; GSM28757     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11282     5  0.4804      0.804 0.000 0.328 0.000 0.036 0.636
#&gt; GSM28756     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11276     2  0.0162      0.922 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28752     2  0.0000      0.923 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM28762     5  0.4141      0.737 0.000 NA 0.000 0.000 0.556 0.012
#&gt; GSM28763     5  0.4141      0.737 0.000 NA 0.000 0.000 0.556 0.012
#&gt; GSM28764     5  0.2595      0.685 0.000 NA 0.000 0.000 0.836 0.004
#&gt; GSM11274     4  0.5581     -0.126 0.000 NA 0.424 0.460 0.108 0.008
#&gt; GSM28772     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0146      0.995 0.996 NA 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 NA 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0260      0.598 0.000 NA 0.000 0.008 0.992 0.000
#&gt; GSM28766     5  0.0260      0.598 0.000 NA 0.000 0.008 0.992 0.000
#&gt; GSM11268     6  0.3706      0.998 0.000 NA 0.380 0.000 0.000 0.620
#&gt; GSM28767     5  0.0777      0.620 0.000 NA 0.000 0.004 0.972 0.000
#&gt; GSM11286     5  0.4513      0.728 0.000 NA 0.000 0.000 0.528 0.032
#&gt; GSM28751     5  0.4294      0.736 0.000 NA 0.000 0.000 0.552 0.020
#&gt; GSM28770     5  0.0260      0.598 0.000 NA 0.000 0.008 0.992 0.000
#&gt; GSM11283     4  0.3851      0.401 0.000 NA 0.000 0.540 0.000 0.000
#&gt; GSM11289     5  0.0260      0.598 0.000 NA 0.000 0.008 0.992 0.000
#&gt; GSM11280     5  0.4685      0.724 0.000 NA 0.000 0.000 0.520 0.044
#&gt; GSM28749     5  0.4685      0.724 0.000 NA 0.000 0.000 0.520 0.044
#&gt; GSM28750     6  0.3717      0.994 0.000 NA 0.384 0.000 0.000 0.616
#&gt; GSM11290     3  0.0260      0.983 0.000 NA 0.992 0.000 0.000 0.008
#&gt; GSM11294     3  0.0000      0.994 0.000 NA 1.000 0.000 0.000 0.000
#&gt; GSM28771     4  0.3851      0.401 0.000 NA 0.000 0.540 0.000 0.000
#&gt; GSM28760     4  0.3851      0.401 0.000 NA 0.000 0.540 0.000 0.000
#&gt; GSM28774     5  0.0935      0.627 0.000 NA 0.000 0.000 0.964 0.004
#&gt; GSM11284     5  0.0935      0.627 0.000 NA 0.000 0.000 0.964 0.004
#&gt; GSM28761     6  0.3706      0.998 0.000 NA 0.380 0.000 0.000 0.620
#&gt; GSM11278     4  0.5415      0.325 0.000 NA 0.088 0.460 0.444 0.008
#&gt; GSM11291     3  0.0000      0.994 0.000 NA 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.994 0.000 NA 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.3706      0.998 0.000 NA 0.380 0.000 0.000 0.620
#&gt; GSM11285     4  0.3851      0.401 0.000 NA 0.000 0.540 0.000 0.000
#&gt; GSM28753     5  0.4218      0.737 0.000 NA 0.000 0.000 0.556 0.016
#&gt; GSM28773     5  0.5718      0.046 0.000 NA 0.000 0.000 0.440 0.396
#&gt; GSM28765     5  0.3409      0.732 0.000 NA 0.000 0.000 0.700 0.000
#&gt; GSM28768     5  0.5182      0.713 0.016 NA 0.000 0.000 0.504 0.052
#&gt; GSM28754     5  0.3409      0.732 0.000 NA 0.000 0.000 0.700 0.000
#&gt; GSM28769     5  0.4294      0.736 0.000 NA 0.000 0.000 0.552 0.020
#&gt; GSM11275     1  0.0146      0.995 0.996 NA 0.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.5415      0.325 0.000 NA 0.088 0.460 0.444 0.008
#&gt; GSM11271     5  0.0865      0.630 0.000 NA 0.000 0.000 0.964 0.000
#&gt; GSM11288     5  0.7215      0.513 0.000 NA 0.120 0.000 0.400 0.184
#&gt; GSM11273     4  0.5415      0.325 0.000 NA 0.088 0.460 0.444 0.008
#&gt; GSM28757     5  0.4685      0.724 0.000 NA 0.000 0.000 0.520 0.044
#&gt; GSM11282     5  0.5129     -0.471 0.000 NA 0.060 0.460 0.472 0.008
#&gt; GSM28756     5  0.3409      0.732 0.000 NA 0.000 0.000 0.700 0.000
#&gt; GSM11276     5  0.3923      0.741 0.000 NA 0.000 0.000 0.580 0.004
#&gt; GSM28752     5  0.3950      0.738 0.000 NA 0.000 0.000 0.564 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:hclust 54     0.398 2
#> CV:hclust 53     0.373 3
#> CV:hclust 53     0.447 4
#> CV:hclust 53     0.480 5
#> CV:hclust 44     0.410 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.443           0.830       0.861         0.3419 0.669   0.669
#> 3 3 0.955           0.963       0.956         0.6241 0.769   0.656
#> 4 4 0.715           0.736       0.861         0.2103 0.919   0.815
#> 5 5 0.707           0.794       0.832         0.1168 0.843   0.571
#> 6 6 0.695           0.668       0.792         0.0593 0.974   0.886
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2   0.891      0.867 0.308 0.692
#&gt; GSM28763     2   0.891      0.867 0.308 0.692
#&gt; GSM28764     2   0.891      0.867 0.308 0.692
#&gt; GSM11274     2   0.000      0.610 0.000 1.000
#&gt; GSM28772     1   0.000      1.000 1.000 0.000
#&gt; GSM11269     1   0.000      1.000 1.000 0.000
#&gt; GSM28775     1   0.000      1.000 1.000 0.000
#&gt; GSM11293     1   0.000      1.000 1.000 0.000
#&gt; GSM28755     1   0.000      1.000 1.000 0.000
#&gt; GSM11279     1   0.000      1.000 1.000 0.000
#&gt; GSM28758     1   0.000      1.000 1.000 0.000
#&gt; GSM11281     1   0.000      1.000 1.000 0.000
#&gt; GSM11287     1   0.000      1.000 1.000 0.000
#&gt; GSM28759     1   0.000      1.000 1.000 0.000
#&gt; GSM11292     2   0.891      0.867 0.308 0.692
#&gt; GSM28766     2   0.891      0.867 0.308 0.692
#&gt; GSM11268     2   0.494      0.513 0.108 0.892
#&gt; GSM28767     2   0.891      0.867 0.308 0.692
#&gt; GSM11286     2   0.891      0.867 0.308 0.692
#&gt; GSM28751     2   0.891      0.867 0.308 0.692
#&gt; GSM28770     2   0.891      0.867 0.308 0.692
#&gt; GSM11283     2   0.891      0.867 0.308 0.692
#&gt; GSM11289     2   0.891      0.867 0.308 0.692
#&gt; GSM11280     2   0.891      0.867 0.308 0.692
#&gt; GSM28749     2   0.891      0.867 0.308 0.692
#&gt; GSM28750     2   0.494      0.513 0.108 0.892
#&gt; GSM11290     2   0.494      0.513 0.108 0.892
#&gt; GSM11294     2   0.494      0.513 0.108 0.892
#&gt; GSM28771     2   0.881      0.863 0.300 0.700
#&gt; GSM28760     2   0.844      0.846 0.272 0.728
#&gt; GSM28774     2   0.891      0.867 0.308 0.692
#&gt; GSM11284     2   0.891      0.867 0.308 0.692
#&gt; GSM28761     2   0.494      0.513 0.108 0.892
#&gt; GSM11278     2   0.844      0.846 0.272 0.728
#&gt; GSM11291     2   0.494      0.513 0.108 0.892
#&gt; GSM11277     2   0.494      0.513 0.108 0.892
#&gt; GSM11272     2   0.494      0.513 0.108 0.892
#&gt; GSM11285     2   0.881      0.863 0.300 0.700
#&gt; GSM28753     2   0.891      0.867 0.308 0.692
#&gt; GSM28773     2   0.886      0.865 0.304 0.696
#&gt; GSM28765     2   0.891      0.867 0.308 0.692
#&gt; GSM28768     2   0.891      0.867 0.308 0.692
#&gt; GSM28754     2   0.891      0.867 0.308 0.692
#&gt; GSM28769     2   0.891      0.867 0.308 0.692
#&gt; GSM11275     1   0.000      1.000 1.000 0.000
#&gt; GSM11270     2   0.844      0.846 0.272 0.728
#&gt; GSM11271     2   0.891      0.867 0.308 0.692
#&gt; GSM11288     2   0.891      0.867 0.308 0.692
#&gt; GSM11273     2   0.000      0.610 0.000 1.000
#&gt; GSM28757     2   0.891      0.867 0.308 0.692
#&gt; GSM11282     2   0.844      0.846 0.272 0.728
#&gt; GSM28756     2   0.891      0.867 0.308 0.692
#&gt; GSM11276     2   0.891      0.867 0.308 0.692
#&gt; GSM28752     2   0.891      0.867 0.308 0.692
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM11274     3  0.1860      0.975 0.000 0.052 0.948
#&gt; GSM28772     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11269     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM28775     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11293     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM28755     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11279     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM28758     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11281     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11287     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM28759     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11292     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM28766     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11268     3  0.3009      0.988 0.028 0.052 0.920
#&gt; GSM28767     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11286     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28751     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28770     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11283     2  0.3983      0.885 0.048 0.884 0.068
#&gt; GSM11289     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11280     2  0.0424      0.965 0.000 0.992 0.008
#&gt; GSM28749     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28750     3  0.3009      0.988 0.028 0.052 0.920
#&gt; GSM11290     3  0.2743      0.988 0.020 0.052 0.928
#&gt; GSM11294     3  0.2743      0.988 0.020 0.052 0.928
#&gt; GSM28771     2  0.4165      0.879 0.048 0.876 0.076
#&gt; GSM28760     2  0.6606      0.689 0.048 0.716 0.236
#&gt; GSM28774     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11284     2  0.2564      0.926 0.036 0.936 0.028
#&gt; GSM28761     3  0.3009      0.988 0.028 0.052 0.920
#&gt; GSM11278     2  0.0747      0.963 0.000 0.984 0.016
#&gt; GSM11291     3  0.2743      0.988 0.020 0.052 0.928
#&gt; GSM11277     3  0.2743      0.988 0.020 0.052 0.928
#&gt; GSM11272     3  0.3009      0.988 0.028 0.052 0.920
#&gt; GSM11285     2  0.4165      0.882 0.048 0.876 0.076
#&gt; GSM28753     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28773     2  0.0237      0.968 0.004 0.996 0.000
#&gt; GSM28765     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28768     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28754     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM28769     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM11275     1  0.1964      1.000 0.944 0.056 0.000
#&gt; GSM11270     2  0.0747      0.963 0.000 0.984 0.016
#&gt; GSM11271     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11288     2  0.4629      0.753 0.004 0.808 0.188
#&gt; GSM11273     3  0.2711      0.945 0.000 0.088 0.912
#&gt; GSM28757     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM11282     2  0.0747      0.963 0.000 0.984 0.016
#&gt; GSM28756     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM11276     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.970 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.2011      0.684 0.000 0.920 0.000 0.080
#&gt; GSM28763     2  0.2011      0.684 0.000 0.920 0.000 0.080
#&gt; GSM28764     2  0.2345      0.698 0.000 0.900 0.000 0.100
#&gt; GSM11274     3  0.2081      0.838 0.000 0.000 0.916 0.084
#&gt; GSM28772     1  0.0592      0.983 0.984 0.016 0.000 0.000
#&gt; GSM11269     1  0.0592      0.983 0.984 0.016 0.000 0.000
#&gt; GSM28775     1  0.0927      0.981 0.976 0.016 0.000 0.008
#&gt; GSM11293     1  0.1406      0.978 0.960 0.016 0.000 0.024
#&gt; GSM28755     1  0.1182      0.978 0.968 0.016 0.000 0.016
#&gt; GSM11279     1  0.0927      0.981 0.976 0.016 0.000 0.008
#&gt; GSM28758     1  0.2522      0.951 0.908 0.016 0.000 0.076
#&gt; GSM11281     1  0.0592      0.983 0.984 0.016 0.000 0.000
#&gt; GSM11287     1  0.0592      0.983 0.984 0.016 0.000 0.000
#&gt; GSM28759     1  0.1406      0.978 0.960 0.016 0.000 0.024
#&gt; GSM11292     2  0.4431      0.546 0.000 0.696 0.000 0.304
#&gt; GSM28766     2  0.4431      0.546 0.000 0.696 0.000 0.304
#&gt; GSM11268     3  0.2987      0.877 0.016 0.000 0.880 0.104
#&gt; GSM28767     2  0.4431      0.546 0.000 0.696 0.000 0.304
#&gt; GSM11286     2  0.1022      0.705 0.000 0.968 0.000 0.032
#&gt; GSM28751     2  0.2011      0.684 0.000 0.920 0.000 0.080
#&gt; GSM28770     2  0.4431      0.546 0.000 0.696 0.000 0.304
#&gt; GSM11283     4  0.4713      0.781 0.000 0.360 0.000 0.640
#&gt; GSM11289     2  0.4431      0.546 0.000 0.696 0.000 0.304
#&gt; GSM11280     2  0.2216      0.676 0.000 0.908 0.000 0.092
#&gt; GSM28749     2  0.2149      0.680 0.000 0.912 0.000 0.088
#&gt; GSM28750     3  0.2987      0.877 0.016 0.000 0.880 0.104
#&gt; GSM11290     3  0.0000      0.883 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0592      0.883 0.000 0.000 0.984 0.016
#&gt; GSM28771     4  0.4585      0.823 0.000 0.332 0.000 0.668
#&gt; GSM28760     4  0.4574      0.820 0.000 0.220 0.024 0.756
#&gt; GSM28774     2  0.3266      0.665 0.000 0.832 0.000 0.168
#&gt; GSM11284     2  0.4790      0.328 0.000 0.620 0.000 0.380
#&gt; GSM28761     3  0.2987      0.877 0.016 0.000 0.880 0.104
#&gt; GSM11278     2  0.4781      0.489 0.000 0.660 0.004 0.336
#&gt; GSM11291     3  0.0592      0.883 0.000 0.000 0.984 0.016
#&gt; GSM11277     3  0.0592      0.883 0.000 0.000 0.984 0.016
#&gt; GSM11272     3  0.2987      0.877 0.016 0.000 0.880 0.104
#&gt; GSM11285     4  0.3837      0.780 0.000 0.224 0.000 0.776
#&gt; GSM28753     2  0.2011      0.684 0.000 0.920 0.000 0.080
#&gt; GSM28773     2  0.2922      0.658 0.004 0.884 0.008 0.104
#&gt; GSM28765     2  0.0188      0.712 0.000 0.996 0.000 0.004
#&gt; GSM28768     2  0.2814      0.632 0.000 0.868 0.000 0.132
#&gt; GSM28754     2  0.2345      0.698 0.000 0.900 0.000 0.100
#&gt; GSM28769     2  0.2011      0.684 0.000 0.920 0.000 0.080
#&gt; GSM11275     1  0.2522      0.951 0.908 0.016 0.000 0.076
#&gt; GSM11270     2  0.4781      0.489 0.000 0.660 0.004 0.336
#&gt; GSM11271     2  0.4277      0.572 0.000 0.720 0.000 0.280
#&gt; GSM11288     2  0.5962      0.342 0.004 0.696 0.200 0.100
#&gt; GSM11273     3  0.7054      0.120 0.000 0.144 0.536 0.320
#&gt; GSM28757     2  0.0336      0.712 0.000 0.992 0.000 0.008
#&gt; GSM11282     2  0.4781      0.489 0.000 0.660 0.004 0.336
#&gt; GSM28756     2  0.2760      0.687 0.000 0.872 0.000 0.128
#&gt; GSM11276     2  0.0921      0.711 0.000 0.972 0.000 0.028
#&gt; GSM28752     2  0.0188      0.712 0.000 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0324     0.8608 0.000 0.992 0.000 0.004 0.004
#&gt; GSM28763     2  0.0324     0.8608 0.000 0.992 0.000 0.004 0.004
#&gt; GSM28764     2  0.4734     0.0195 0.000 0.652 0.000 0.036 0.312
#&gt; GSM11274     3  0.5423     0.7771 0.000 0.000 0.644 0.112 0.244
#&gt; GSM28772     1  0.0162     0.9585 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11269     1  0.0162     0.9585 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28775     1  0.1012     0.9503 0.968 0.000 0.000 0.012 0.020
#&gt; GSM11293     1  0.1818     0.9427 0.932 0.000 0.000 0.044 0.024
#&gt; GSM28755     1  0.1117     0.9489 0.964 0.000 0.000 0.016 0.020
#&gt; GSM11279     1  0.0451     0.9579 0.988 0.000 0.000 0.008 0.004
#&gt; GSM28758     1  0.3169     0.9026 0.856 0.000 0.000 0.084 0.060
#&gt; GSM11281     1  0.0324     0.9584 0.992 0.000 0.000 0.004 0.004
#&gt; GSM11287     1  0.0162     0.9585 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28759     1  0.1818     0.9427 0.932 0.000 0.000 0.044 0.024
#&gt; GSM11292     5  0.5268     0.8051 0.000 0.320 0.000 0.068 0.612
#&gt; GSM28766     5  0.5268     0.8051 0.000 0.320 0.000 0.068 0.612
#&gt; GSM11268     3  0.0000     0.8645 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28767     5  0.5268     0.8051 0.000 0.320 0.000 0.068 0.612
#&gt; GSM11286     2  0.1211     0.8556 0.000 0.960 0.000 0.016 0.024
#&gt; GSM28751     2  0.0162     0.8603 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28770     5  0.5268     0.8051 0.000 0.320 0.000 0.068 0.612
#&gt; GSM11283     4  0.4591     0.9264 0.000 0.132 0.000 0.748 0.120
#&gt; GSM11289     5  0.5268     0.8051 0.000 0.320 0.000 0.068 0.612
#&gt; GSM11280     2  0.1041     0.8517 0.000 0.964 0.000 0.032 0.004
#&gt; GSM28749     2  0.1026     0.8532 0.000 0.968 0.004 0.024 0.004
#&gt; GSM28750     3  0.0000     0.8645 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11290     3  0.4020     0.8761 0.000 0.000 0.796 0.108 0.096
#&gt; GSM11294     3  0.4406     0.8747 0.000 0.000 0.764 0.108 0.128
#&gt; GSM28771     4  0.4593     0.9352 0.000 0.124 0.000 0.748 0.128
#&gt; GSM28760     4  0.4718     0.9316 0.000 0.092 0.000 0.728 0.180
#&gt; GSM28774     5  0.4415     0.5715 0.000 0.444 0.000 0.004 0.552
#&gt; GSM11284     5  0.6569     0.5222 0.000 0.272 0.000 0.256 0.472
#&gt; GSM28761     3  0.0000     0.8645 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11278     5  0.3231     0.7358 0.000 0.196 0.000 0.004 0.800
#&gt; GSM11291     3  0.4406     0.8747 0.000 0.000 0.764 0.108 0.128
#&gt; GSM11277     3  0.4406     0.8747 0.000 0.000 0.764 0.108 0.128
#&gt; GSM11272     3  0.0000     0.8645 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11285     4  0.4555     0.9066 0.000 0.068 0.000 0.732 0.200
#&gt; GSM28753     2  0.0290     0.8601 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28773     2  0.2430     0.8186 0.000 0.912 0.020 0.040 0.028
#&gt; GSM28765     2  0.0955     0.8512 0.000 0.968 0.000 0.004 0.028
#&gt; GSM28768     2  0.2438     0.8052 0.000 0.900 0.000 0.060 0.040
#&gt; GSM28754     2  0.4446    -0.4303 0.000 0.520 0.000 0.004 0.476
#&gt; GSM28769     2  0.0162     0.8603 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11275     1  0.3169     0.9026 0.856 0.000 0.000 0.084 0.060
#&gt; GSM11270     5  0.3231     0.7358 0.000 0.196 0.000 0.004 0.800
#&gt; GSM11271     5  0.4787     0.7961 0.000 0.324 0.000 0.036 0.640
#&gt; GSM11288     2  0.3720     0.5966 0.000 0.760 0.228 0.012 0.000
#&gt; GSM11273     5  0.3183     0.4061 0.000 0.028 0.108 0.008 0.856
#&gt; GSM28757     2  0.1914     0.8347 0.000 0.924 0.000 0.016 0.060
#&gt; GSM11282     5  0.3231     0.7358 0.000 0.196 0.000 0.004 0.800
#&gt; GSM28756     5  0.4443     0.5028 0.000 0.472 0.000 0.004 0.524
#&gt; GSM11276     2  0.1704     0.8065 0.000 0.928 0.000 0.004 0.068
#&gt; GSM28752     2  0.0609     0.8550 0.000 0.980 0.000 0.000 0.020
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.0881     0.8372 0.000 0.972 0.012 0.008 0.008 0.000
#&gt; GSM28763     2  0.0881     0.8372 0.000 0.972 0.012 0.008 0.008 0.000
#&gt; GSM28764     2  0.4591    -0.1799 0.000 0.552 0.000 0.040 0.408 0.000
#&gt; GSM11274     3  0.5614     0.0000 0.000 0.000 0.488 0.008 0.116 0.388
#&gt; GSM28772     1  0.0000     0.9231 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9231 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.2328     0.8953 0.904 0.000 0.020 0.044 0.032 0.000
#&gt; GSM11293     1  0.2454     0.8986 0.884 0.000 0.088 0.008 0.020 0.000
#&gt; GSM28755     1  0.2538     0.8919 0.892 0.000 0.020 0.048 0.040 0.000
#&gt; GSM11279     1  0.0405     0.9216 0.988 0.000 0.000 0.004 0.008 0.000
#&gt; GSM28758     1  0.4109     0.8250 0.748 0.000 0.196 0.032 0.024 0.000
#&gt; GSM11281     1  0.0000     0.9231 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9231 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.2454     0.8986 0.884 0.000 0.088 0.008 0.020 0.000
#&gt; GSM11292     5  0.3971     0.7281 0.000 0.184 0.000 0.068 0.748 0.000
#&gt; GSM28766     5  0.3971     0.7281 0.000 0.184 0.000 0.068 0.748 0.000
#&gt; GSM11268     6  0.0000     0.5226 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     5  0.3971     0.7281 0.000 0.184 0.000 0.068 0.748 0.000
#&gt; GSM11286     2  0.2274     0.8288 0.000 0.892 0.088 0.012 0.008 0.000
#&gt; GSM28751     2  0.1624     0.8354 0.000 0.936 0.044 0.012 0.008 0.000
#&gt; GSM28770     5  0.3939     0.7279 0.000 0.180 0.000 0.068 0.752 0.000
#&gt; GSM11283     4  0.2507     0.9330 0.000 0.072 0.004 0.884 0.040 0.000
#&gt; GSM11289     5  0.3939     0.7279 0.000 0.180 0.000 0.068 0.752 0.000
#&gt; GSM11280     2  0.2748     0.8149 0.000 0.848 0.128 0.024 0.000 0.000
#&gt; GSM28749     2  0.3056     0.8112 0.000 0.832 0.140 0.016 0.000 0.012
#&gt; GSM28750     6  0.0000     0.5226 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11290     6  0.3915     0.0399 0.000 0.000 0.412 0.000 0.004 0.584
#&gt; GSM11294     6  0.4136    -0.0365 0.000 0.000 0.428 0.000 0.012 0.560
#&gt; GSM28771     4  0.2519     0.9376 0.000 0.068 0.004 0.884 0.044 0.000
#&gt; GSM28760     4  0.2630     0.9356 0.000 0.032 0.004 0.872 0.092 0.000
#&gt; GSM28774     5  0.5317     0.5697 0.000 0.332 0.096 0.008 0.564 0.000
#&gt; GSM11284     5  0.6853     0.5110 0.000 0.164 0.100 0.256 0.480 0.000
#&gt; GSM28761     6  0.0000     0.5226 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     5  0.4553     0.6285 0.000 0.064 0.180 0.028 0.728 0.000
#&gt; GSM11291     6  0.4136    -0.0365 0.000 0.000 0.428 0.000 0.012 0.560
#&gt; GSM11277     6  0.4136    -0.0365 0.000 0.000 0.428 0.000 0.012 0.560
#&gt; GSM11272     6  0.0000     0.5226 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11285     4  0.2540     0.9224 0.000 0.020 0.004 0.872 0.104 0.000
#&gt; GSM28753     2  0.1657     0.8402 0.000 0.928 0.056 0.016 0.000 0.000
#&gt; GSM28773     2  0.4731     0.7526 0.000 0.732 0.176 0.020 0.024 0.048
#&gt; GSM28765     2  0.1230     0.8273 0.000 0.956 0.008 0.008 0.028 0.000
#&gt; GSM28768     2  0.2892     0.7919 0.000 0.840 0.136 0.020 0.004 0.000
#&gt; GSM28754     5  0.5450     0.3518 0.000 0.440 0.092 0.008 0.460 0.000
#&gt; GSM28769     2  0.1624     0.8354 0.000 0.936 0.044 0.012 0.008 0.000
#&gt; GSM11275     1  0.4109     0.8250 0.748 0.000 0.196 0.032 0.024 0.000
#&gt; GSM11270     5  0.4584     0.6267 0.000 0.064 0.184 0.028 0.724 0.000
#&gt; GSM11271     5  0.3529     0.7281 0.000 0.208 0.000 0.028 0.764 0.000
#&gt; GSM11288     2  0.5194     0.6029 0.000 0.632 0.128 0.008 0.000 0.232
#&gt; GSM11273     5  0.4794     0.4735 0.000 0.004 0.248 0.028 0.680 0.040
#&gt; GSM28757     2  0.3299     0.7921 0.000 0.836 0.092 0.012 0.060 0.000
#&gt; GSM11282     5  0.4553     0.6285 0.000 0.064 0.180 0.028 0.728 0.000
#&gt; GSM28756     5  0.5359     0.5218 0.000 0.364 0.092 0.008 0.536 0.000
#&gt; GSM11276     2  0.2431     0.7300 0.000 0.860 0.000 0.008 0.132 0.000
#&gt; GSM28752     2  0.1500     0.8197 0.000 0.936 0.012 0.000 0.052 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:kmeans 54     0.398 2
#> CV:kmeans 54     0.374 3
#> CV:kmeans 48     0.508 4
#> CV:kmeans 51     0.497 5
#> CV:kmeans 46     0.479 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.730           0.918       0.955         0.4748 0.516   0.516
#> 3 3 0.944           0.935       0.974         0.3402 0.764   0.576
#> 4 4 0.707           0.682       0.861         0.1839 0.822   0.544
#> 5 5 0.796           0.796       0.886         0.0707 0.936   0.744
#> 6 6 0.805           0.734       0.838         0.0380 0.960   0.798
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.972 0.000 1.000
#&gt; GSM28763     2  0.6887      0.762 0.184 0.816
#&gt; GSM28764     2  0.0000      0.972 0.000 1.000
#&gt; GSM11274     2  0.0000      0.972 0.000 1.000
#&gt; GSM28772     1  0.0000      0.913 1.000 0.000
#&gt; GSM11269     1  0.0000      0.913 1.000 0.000
#&gt; GSM28775     1  0.0000      0.913 1.000 0.000
#&gt; GSM11293     1  0.0000      0.913 1.000 0.000
#&gt; GSM28755     1  0.0000      0.913 1.000 0.000
#&gt; GSM11279     1  0.0000      0.913 1.000 0.000
#&gt; GSM28758     1  0.0000      0.913 1.000 0.000
#&gt; GSM11281     1  0.0000      0.913 1.000 0.000
#&gt; GSM11287     1  0.0000      0.913 1.000 0.000
#&gt; GSM28759     1  0.0000      0.913 1.000 0.000
#&gt; GSM11292     2  0.0000      0.972 0.000 1.000
#&gt; GSM28766     2  0.0000      0.972 0.000 1.000
#&gt; GSM11268     1  0.7219      0.832 0.800 0.200
#&gt; GSM28767     2  0.0000      0.972 0.000 1.000
#&gt; GSM11286     2  0.0000      0.972 0.000 1.000
#&gt; GSM28751     2  0.9170      0.541 0.332 0.668
#&gt; GSM28770     2  0.0000      0.972 0.000 1.000
#&gt; GSM11283     2  0.0000      0.972 0.000 1.000
#&gt; GSM11289     2  0.0000      0.972 0.000 1.000
#&gt; GSM11280     2  0.0000      0.972 0.000 1.000
#&gt; GSM28749     2  0.0000      0.972 0.000 1.000
#&gt; GSM28750     1  0.7219      0.832 0.800 0.200
#&gt; GSM11290     1  0.7056      0.838 0.808 0.192
#&gt; GSM11294     1  0.7219      0.832 0.800 0.200
#&gt; GSM28771     2  0.0000      0.972 0.000 1.000
#&gt; GSM28760     2  0.0000      0.972 0.000 1.000
#&gt; GSM28774     2  0.0000      0.972 0.000 1.000
#&gt; GSM11284     2  0.0000      0.972 0.000 1.000
#&gt; GSM28761     1  0.7056      0.838 0.808 0.192
#&gt; GSM11278     2  0.0000      0.972 0.000 1.000
#&gt; GSM11291     1  0.7219      0.832 0.800 0.200
#&gt; GSM11277     1  0.7219      0.832 0.800 0.200
#&gt; GSM11272     1  0.0000      0.913 1.000 0.000
#&gt; GSM11285     2  0.0000      0.972 0.000 1.000
#&gt; GSM28753     2  0.0000      0.972 0.000 1.000
#&gt; GSM28773     2  0.0000      0.972 0.000 1.000
#&gt; GSM28765     2  0.0000      0.972 0.000 1.000
#&gt; GSM28768     1  0.0376      0.911 0.996 0.004
#&gt; GSM28754     2  0.0000      0.972 0.000 1.000
#&gt; GSM28769     2  0.9087      0.557 0.324 0.676
#&gt; GSM11275     1  0.0000      0.913 1.000 0.000
#&gt; GSM11270     2  0.0000      0.972 0.000 1.000
#&gt; GSM11271     2  0.0000      0.972 0.000 1.000
#&gt; GSM11288     1  0.7139      0.835 0.804 0.196
#&gt; GSM11273     2  0.0000      0.972 0.000 1.000
#&gt; GSM28757     2  0.0000      0.972 0.000 1.000
#&gt; GSM11282     2  0.0000      0.972 0.000 1.000
#&gt; GSM28756     2  0.0000      0.972 0.000 1.000
#&gt; GSM11276     2  0.0000      0.972 0.000 1.000
#&gt; GSM28752     2  0.0000      0.972 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28751     1  0.2796      0.873 0.908 0.092 0.000
#&gt; GSM28770     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28749     2  0.4555      0.742 0.000 0.800 0.200
#&gt; GSM28750     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM28771     2  0.5926      0.424 0.000 0.644 0.356
#&gt; GSM28760     3  0.5678      0.509 0.000 0.316 0.684
#&gt; GSM28774     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM11278     2  0.0424      0.965 0.000 0.992 0.008
#&gt; GSM11291     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM11285     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28773     3  0.0892      0.946 0.000 0.020 0.980
#&gt; GSM28765     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28768     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28769     1  0.5291      0.650 0.732 0.268 0.000
#&gt; GSM11275     1  0.0000      0.966 1.000 0.000 0.000
#&gt; GSM11270     2  0.3482      0.841 0.000 0.872 0.128
#&gt; GSM11271     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11288     3  0.0424      0.958 0.008 0.000 0.992
#&gt; GSM11273     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM28757     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11282     2  0.0237      0.968 0.000 0.996 0.004
#&gt; GSM28756     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.972 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.2589     0.6220 0.000 0.884 0.000 0.116
#&gt; GSM28763     2  0.2589     0.6220 0.000 0.884 0.000 0.116
#&gt; GSM28764     4  0.4406     0.3623 0.000 0.300 0.000 0.700
#&gt; GSM11274     3  0.2814     0.8164 0.000 0.000 0.868 0.132
#&gt; GSM28772     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11292     4  0.0188     0.7621 0.000 0.004 0.000 0.996
#&gt; GSM28766     4  0.0188     0.7621 0.000 0.004 0.000 0.996
#&gt; GSM11268     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM28767     4  0.0188     0.7621 0.000 0.004 0.000 0.996
#&gt; GSM11286     2  0.4804     0.4373 0.000 0.616 0.000 0.384
#&gt; GSM28751     2  0.3335     0.5947 0.128 0.856 0.000 0.016
#&gt; GSM28770     4  0.0188     0.7621 0.000 0.004 0.000 0.996
#&gt; GSM11283     2  0.4585     0.2152 0.000 0.668 0.000 0.332
#&gt; GSM11289     4  0.0188     0.7621 0.000 0.004 0.000 0.996
#&gt; GSM11280     2  0.0592     0.5997 0.000 0.984 0.000 0.016
#&gt; GSM28749     2  0.5484     0.4620 0.000 0.732 0.164 0.104
#&gt; GSM28750     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM11290     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM28771     2  0.5493    -0.0738 0.000 0.528 0.016 0.456
#&gt; GSM28760     4  0.6125     0.1062 0.000 0.436 0.048 0.516
#&gt; GSM28774     4  0.3726     0.5262 0.000 0.212 0.000 0.788
#&gt; GSM11284     4  0.3266     0.6032 0.000 0.168 0.000 0.832
#&gt; GSM28761     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM11278     4  0.0336     0.7586 0.000 0.000 0.008 0.992
#&gt; GSM11291     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.0000     0.9135 0.000 0.000 1.000 0.000
#&gt; GSM11285     4  0.4679     0.3272 0.000 0.352 0.000 0.648
#&gt; GSM28753     2  0.0188     0.6061 0.000 0.996 0.000 0.004
#&gt; GSM28773     3  0.5294     0.1792 0.000 0.484 0.508 0.008
#&gt; GSM28765     2  0.4898     0.3971 0.000 0.584 0.000 0.416
#&gt; GSM28768     1  0.2469     0.8698 0.892 0.108 0.000 0.000
#&gt; GSM28754     4  0.4955    -0.0995 0.000 0.444 0.000 0.556
#&gt; GSM28769     2  0.3080     0.6103 0.096 0.880 0.000 0.024
#&gt; GSM11275     1  0.0000     0.9893 1.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.0524     0.7572 0.000 0.004 0.008 0.988
#&gt; GSM11271     4  0.0188     0.7621 0.000 0.004 0.000 0.996
#&gt; GSM11288     3  0.2773     0.8332 0.004 0.116 0.880 0.000
#&gt; GSM11273     3  0.3074     0.7979 0.000 0.000 0.848 0.152
#&gt; GSM28757     2  0.4888     0.4035 0.000 0.588 0.000 0.412
#&gt; GSM11282     4  0.0336     0.7586 0.000 0.000 0.008 0.992
#&gt; GSM28756     4  0.4585     0.2750 0.000 0.332 0.000 0.668
#&gt; GSM11276     2  0.4967     0.3230 0.000 0.548 0.000 0.452
#&gt; GSM28752     2  0.4933     0.3678 0.000 0.568 0.000 0.432
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.1952     0.8145 0.000 0.912 0.000 0.084 0.004
#&gt; GSM28763     2  0.2011     0.8136 0.000 0.908 0.000 0.088 0.004
#&gt; GSM28764     5  0.4794     0.4712 0.000 0.344 0.000 0.032 0.624
#&gt; GSM11274     3  0.0740     0.8802 0.000 0.004 0.980 0.008 0.008
#&gt; GSM28772     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.1918     0.8313 0.000 0.036 0.000 0.036 0.928
#&gt; GSM28766     5  0.1918     0.8313 0.000 0.036 0.000 0.036 0.928
#&gt; GSM11268     3  0.0451     0.8893 0.000 0.004 0.988 0.008 0.000
#&gt; GSM28767     5  0.1918     0.8313 0.000 0.036 0.000 0.036 0.928
#&gt; GSM11286     2  0.2871     0.8056 0.000 0.872 0.000 0.040 0.088
#&gt; GSM28751     2  0.2981     0.8029 0.024 0.876 0.000 0.084 0.016
#&gt; GSM28770     5  0.1918     0.8313 0.000 0.036 0.000 0.036 0.928
#&gt; GSM11283     4  0.1281     0.8593 0.000 0.032 0.000 0.956 0.012
#&gt; GSM11289     5  0.1918     0.8313 0.000 0.036 0.000 0.036 0.928
#&gt; GSM11280     4  0.3496     0.7319 0.000 0.200 0.000 0.788 0.012
#&gt; GSM28749     4  0.4150     0.7264 0.000 0.216 0.000 0.748 0.036
#&gt; GSM28750     3  0.0451     0.8893 0.000 0.004 0.988 0.008 0.000
#&gt; GSM11290     3  0.0000     0.8900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000     0.8900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28771     4  0.1300     0.8606 0.000 0.028 0.000 0.956 0.016
#&gt; GSM28760     4  0.1285     0.8598 0.000 0.004 0.004 0.956 0.036
#&gt; GSM28774     5  0.3527     0.7197 0.000 0.192 0.000 0.016 0.792
#&gt; GSM11284     4  0.3950     0.7751 0.000 0.068 0.000 0.796 0.136
#&gt; GSM28761     3  0.0451     0.8893 0.000 0.004 0.988 0.008 0.000
#&gt; GSM11278     5  0.2656     0.8002 0.000 0.064 0.012 0.028 0.896
#&gt; GSM11291     3  0.0000     0.8900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000     0.8900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.0451     0.8893 0.000 0.004 0.988 0.008 0.000
#&gt; GSM11285     4  0.2233     0.8417 0.000 0.016 0.000 0.904 0.080
#&gt; GSM28753     2  0.4029     0.5255 0.000 0.680 0.000 0.316 0.004
#&gt; GSM28773     3  0.7113    -0.0484 0.000 0.304 0.380 0.304 0.012
#&gt; GSM28765     2  0.3106     0.7790 0.000 0.840 0.000 0.020 0.140
#&gt; GSM28768     1  0.4416     0.4338 0.632 0.356 0.000 0.012 0.000
#&gt; GSM28754     5  0.5036     0.3213 0.000 0.404 0.000 0.036 0.560
#&gt; GSM28769     2  0.2518     0.8129 0.008 0.896 0.000 0.080 0.016
#&gt; GSM11275     1  0.0000     0.9642 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.2693     0.7995 0.000 0.060 0.016 0.028 0.896
#&gt; GSM11271     5  0.1568     0.8304 0.000 0.036 0.000 0.020 0.944
#&gt; GSM11288     3  0.5176     0.5108 0.020 0.036 0.656 0.288 0.000
#&gt; GSM11273     3  0.3463     0.7428 0.000 0.008 0.820 0.016 0.156
#&gt; GSM28757     2  0.3883     0.7307 0.000 0.780 0.000 0.036 0.184
#&gt; GSM11282     5  0.2409     0.8032 0.000 0.060 0.012 0.020 0.908
#&gt; GSM28756     5  0.4268     0.6183 0.000 0.268 0.000 0.024 0.708
#&gt; GSM11276     2  0.3508     0.6705 0.000 0.748 0.000 0.000 0.252
#&gt; GSM28752     2  0.2377     0.8028 0.000 0.872 0.000 0.000 0.128
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.0993      0.710 0.000 0.964 0.000 0.012 0.000 0.024
#&gt; GSM28763     2  0.0993      0.710 0.000 0.964 0.000 0.012 0.000 0.024
#&gt; GSM28764     5  0.3190      0.713 0.000 0.136 0.000 0.000 0.820 0.044
#&gt; GSM11274     3  0.3053      0.772 0.000 0.000 0.812 0.020 0.000 0.168
#&gt; GSM28772     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000      0.953 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28766     5  0.0000      0.953 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11268     3  0.0146      0.814 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; GSM28767     5  0.0000      0.953 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11286     2  0.5462      0.368 0.000 0.472 0.000 0.032 0.052 0.444
#&gt; GSM28751     2  0.0912      0.711 0.004 0.972 0.000 0.008 0.012 0.004
#&gt; GSM28770     5  0.0000      0.953 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11283     4  0.0858      0.806 0.000 0.028 0.000 0.968 0.004 0.000
#&gt; GSM11289     5  0.0000      0.953 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11280     4  0.5011      0.571 0.000 0.116 0.000 0.620 0.000 0.264
#&gt; GSM28749     4  0.6081      0.543 0.000 0.112 0.020 0.568 0.024 0.276
#&gt; GSM28750     3  0.0000      0.814 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11290     3  0.1471      0.818 0.000 0.000 0.932 0.004 0.000 0.064
#&gt; GSM11294     3  0.2100      0.812 0.000 0.000 0.884 0.004 0.000 0.112
#&gt; GSM28771     4  0.0858      0.806 0.000 0.028 0.000 0.968 0.004 0.000
#&gt; GSM28760     4  0.0806      0.806 0.000 0.020 0.000 0.972 0.008 0.000
#&gt; GSM28774     6  0.5521      0.647 0.000 0.112 0.000 0.012 0.320 0.556
#&gt; GSM11284     4  0.3929      0.669 0.000 0.008 0.000 0.776 0.072 0.144
#&gt; GSM28761     3  0.0146      0.814 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; GSM11278     6  0.4170      0.616 0.000 0.004 0.000 0.020 0.328 0.648
#&gt; GSM11291     3  0.2100      0.812 0.000 0.000 0.884 0.004 0.000 0.112
#&gt; GSM11277     3  0.2100      0.812 0.000 0.000 0.884 0.004 0.000 0.112
#&gt; GSM11272     3  0.0146      0.814 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; GSM11285     4  0.1444      0.782 0.000 0.000 0.000 0.928 0.072 0.000
#&gt; GSM28753     2  0.5927      0.343 0.000 0.540 0.000 0.256 0.016 0.188
#&gt; GSM28773     3  0.7503     -0.129 0.000 0.316 0.324 0.160 0.000 0.200
#&gt; GSM28765     2  0.5497      0.438 0.000 0.556 0.000 0.020 0.088 0.336
#&gt; GSM28768     1  0.4609      0.496 0.648 0.296 0.000 0.008 0.000 0.048
#&gt; GSM28754     6  0.4988      0.628 0.000 0.136 0.000 0.008 0.188 0.668
#&gt; GSM28769     2  0.0767      0.711 0.000 0.976 0.000 0.008 0.012 0.004
#&gt; GSM11275     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     6  0.4290      0.617 0.000 0.004 0.004 0.020 0.324 0.648
#&gt; GSM11271     5  0.0146      0.949 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11288     3  0.5533      0.462 0.008 0.052 0.656 0.208 0.000 0.076
#&gt; GSM11273     3  0.5028      0.375 0.000 0.000 0.524 0.020 0.036 0.420
#&gt; GSM28757     6  0.4178      0.358 0.000 0.184 0.000 0.016 0.052 0.748
#&gt; GSM11282     6  0.4290      0.599 0.000 0.004 0.000 0.020 0.364 0.612
#&gt; GSM28756     6  0.5204      0.641 0.000 0.112 0.000 0.012 0.244 0.632
#&gt; GSM11276     2  0.5243      0.367 0.000 0.532 0.000 0.004 0.376 0.088
#&gt; GSM28752     2  0.3992      0.621 0.000 0.756 0.000 0.008 0.184 0.052
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> CV:skmeans 54     0.398 2
#> CV:skmeans 53     0.373 3
#> CV:skmeans 40     0.406 4
#> CV:skmeans 50     0.443 5
#> CV:skmeans 45     0.419 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.984       0.994         0.3387 0.669   0.669
#> 3 3 0.968           0.971       0.987         0.6385 0.786   0.681
#> 4 4 0.938           0.925       0.967         0.3303 0.801   0.563
#> 5 5 0.868           0.877       0.930         0.0550 0.947   0.796
#> 6 6 0.928           0.905       0.946         0.0331 0.986   0.934
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.992 0.000 1.000
#&gt; GSM28763     2  0.0000      0.992 0.000 1.000
#&gt; GSM28764     2  0.0000      0.992 0.000 1.000
#&gt; GSM11274     2  0.0000      0.992 0.000 1.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000
#&gt; GSM11292     2  0.0000      0.992 0.000 1.000
#&gt; GSM28766     2  0.0000      0.992 0.000 1.000
#&gt; GSM11268     2  0.0000      0.992 0.000 1.000
#&gt; GSM28767     2  0.0000      0.992 0.000 1.000
#&gt; GSM11286     2  0.0000      0.992 0.000 1.000
#&gt; GSM28751     2  0.0000      0.992 0.000 1.000
#&gt; GSM28770     2  0.0000      0.992 0.000 1.000
#&gt; GSM11283     2  0.0000      0.992 0.000 1.000
#&gt; GSM11289     2  0.0000      0.992 0.000 1.000
#&gt; GSM11280     2  0.0000      0.992 0.000 1.000
#&gt; GSM28749     2  0.0000      0.992 0.000 1.000
#&gt; GSM28750     2  0.0000      0.992 0.000 1.000
#&gt; GSM11290     2  0.0000      0.992 0.000 1.000
#&gt; GSM11294     2  0.0000      0.992 0.000 1.000
#&gt; GSM28771     2  0.0000      0.992 0.000 1.000
#&gt; GSM28760     2  0.0000      0.992 0.000 1.000
#&gt; GSM28774     2  0.0000      0.992 0.000 1.000
#&gt; GSM11284     2  0.0000      0.992 0.000 1.000
#&gt; GSM28761     2  0.0000      0.992 0.000 1.000
#&gt; GSM11278     2  0.0000      0.992 0.000 1.000
#&gt; GSM11291     2  0.0000      0.992 0.000 1.000
#&gt; GSM11277     2  0.0000      0.992 0.000 1.000
#&gt; GSM11272     2  0.0376      0.988 0.004 0.996
#&gt; GSM11285     2  0.0000      0.992 0.000 1.000
#&gt; GSM28753     2  0.0000      0.992 0.000 1.000
#&gt; GSM28773     2  0.0000      0.992 0.000 1.000
#&gt; GSM28765     2  0.0000      0.992 0.000 1.000
#&gt; GSM28768     2  0.9286      0.476 0.344 0.656
#&gt; GSM28754     2  0.0000      0.992 0.000 1.000
#&gt; GSM28769     2  0.0000      0.992 0.000 1.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000
#&gt; GSM11270     2  0.0000      0.992 0.000 1.000
#&gt; GSM11271     2  0.0000      0.992 0.000 1.000
#&gt; GSM11288     2  0.0000      0.992 0.000 1.000
#&gt; GSM11273     2  0.0000      0.992 0.000 1.000
#&gt; GSM28757     2  0.0000      0.992 0.000 1.000
#&gt; GSM11282     2  0.0000      0.992 0.000 1.000
#&gt; GSM28756     2  0.0000      0.992 0.000 1.000
#&gt; GSM11276     2  0.0000      0.992 0.000 1.000
#&gt; GSM28752     2  0.0000      0.992 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28751     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28770     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28749     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28750     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM28771     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28760     2  0.4121      0.808 0.000 0.832 0.168
#&gt; GSM28774     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28761     3  0.1163      0.959 0.000 0.028 0.972
#&gt; GSM11278     2  0.0237      0.976 0.000 0.996 0.004
#&gt; GSM11291     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000      0.995 0.000 0.000 1.000
#&gt; GSM11285     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28768     2  0.3038      0.877 0.104 0.896 0.000
#&gt; GSM28754     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28769     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11270     2  0.4178      0.802 0.000 0.828 0.172
#&gt; GSM11271     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11288     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11273     2  0.5016      0.704 0.000 0.760 0.240
#&gt; GSM28757     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11282     2  0.0237      0.976 0.000 0.996 0.004
#&gt; GSM28756     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.979 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.979 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4
#&gt; GSM28762     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28764     2  0.1716      0.924  0 0.936 0.000 0.064
#&gt; GSM11274     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11292     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM28766     4  0.0000      0.875  0 0.000 0.000 1.000
#&gt; GSM11268     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM28767     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM11286     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28770     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM11283     2  0.0188      0.979  0 0.996 0.000 0.004
#&gt; GSM11289     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM11280     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28749     4  0.4072      0.695  0 0.252 0.000 0.748
#&gt; GSM28750     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM11290     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM28771     2  0.0592      0.972  0 0.984 0.000 0.016
#&gt; GSM28760     4  0.3962      0.774  0 0.044 0.124 0.832
#&gt; GSM28774     4  0.3024      0.795  0 0.148 0.000 0.852
#&gt; GSM11284     4  0.4925      0.338  0 0.428 0.000 0.572
#&gt; GSM28761     3  0.1004      0.968  0 0.024 0.972 0.004
#&gt; GSM11278     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM11291     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM11272     3  0.0000      0.996  0 0.000 1.000 0.000
#&gt; GSM11285     4  0.0817      0.870  0 0.024 0.000 0.976
#&gt; GSM28753     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28773     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28765     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28768     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11270     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM11271     4  0.4933      0.262  0 0.432 0.000 0.568
#&gt; GSM11288     2  0.3074      0.795  0 0.848 0.000 0.152
#&gt; GSM11273     4  0.0657      0.873  0 0.004 0.012 0.984
#&gt; GSM28757     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM11282     4  0.0188      0.878  0 0.004 0.000 0.996
#&gt; GSM28756     2  0.0000      0.982  0 1.000 0.000 0.000
#&gt; GSM11276     2  0.1302      0.944  0 0.956 0.000 0.044
#&gt; GSM28752     2  0.0000      0.982  0 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0566      0.942  0 0.984 0.000 0.004 0.012
#&gt; GSM28763     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.3368      0.745  0 0.820 0.000 0.024 0.156
#&gt; GSM11274     3  0.0798      0.889  0 0.000 0.976 0.008 0.016
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0865      0.864  0 0.004 0.000 0.024 0.972
#&gt; GSM28766     5  0.0794      0.861  0 0.000 0.000 0.028 0.972
#&gt; GSM11268     3  0.3366      0.876  0 0.000 0.784 0.212 0.004
#&gt; GSM28767     5  0.0865      0.864  0 0.004 0.000 0.024 0.972
#&gt; GSM11286     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM28770     5  0.0865      0.864  0 0.004 0.000 0.024 0.972
#&gt; GSM11283     4  0.3561      0.740  0 0.260 0.000 0.740 0.000
#&gt; GSM11289     5  0.0865      0.864  0 0.004 0.000 0.024 0.972
#&gt; GSM11280     2  0.0290      0.947  0 0.992 0.000 0.008 0.000
#&gt; GSM28749     5  0.4063      0.530  0 0.280 0.000 0.012 0.708
#&gt; GSM28750     3  0.3366      0.876  0 0.000 0.784 0.212 0.004
#&gt; GSM11290     3  0.0000      0.902  0 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.902  0 0.000 1.000 0.000 0.000
#&gt; GSM28771     4  0.3274      0.779  0 0.220 0.000 0.780 0.000
#&gt; GSM28760     4  0.3210      0.685  0 0.000 0.000 0.788 0.212
#&gt; GSM28774     5  0.3242      0.686  0 0.172 0.000 0.012 0.816
#&gt; GSM11284     4  0.4495      0.788  0 0.200 0.000 0.736 0.064
#&gt; GSM28761     3  0.4033      0.859  0 0.024 0.760 0.212 0.004
#&gt; GSM11278     5  0.0451      0.862  0 0.004 0.000 0.008 0.988
#&gt; GSM11291     3  0.0000      0.902  0 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.902  0 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.3366      0.876  0 0.000 0.784 0.212 0.004
#&gt; GSM11285     4  0.3274      0.685  0 0.000 0.000 0.780 0.220
#&gt; GSM28753     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.0162      0.949  0 0.996 0.000 0.004 0.000
#&gt; GSM28765     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM28754     2  0.0162      0.950  0 0.996 0.000 0.004 0.000
#&gt; GSM28769     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.0451      0.862  0 0.004 0.000 0.008 0.988
#&gt; GSM11271     5  0.4649      0.246  0 0.404 0.000 0.016 0.580
#&gt; GSM11288     2  0.4567      0.647  0 0.760 0.004 0.116 0.120
#&gt; GSM11273     5  0.0740      0.859  0 0.004 0.008 0.008 0.980
#&gt; GSM28757     2  0.0290      0.947  0 0.992 0.000 0.008 0.000
#&gt; GSM11282     5  0.0451      0.862  0 0.004 0.000 0.008 0.988
#&gt; GSM28756     2  0.0162      0.950  0 0.996 0.000 0.000 0.004
#&gt; GSM11276     2  0.2873      0.790  0 0.856 0.000 0.016 0.128
#&gt; GSM28752     2  0.0000      0.951  0 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.1096      0.927  0 0.964 0.020 0.004 0.008 0.004
#&gt; GSM28763     2  0.0000      0.938  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.3351      0.753  0 0.800 0.000 0.040 0.160 0.000
#&gt; GSM11274     3  0.0146      0.889  0 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0937      0.852  0 0.000 0.000 0.040 0.960 0.000
#&gt; GSM28766     5  0.0937      0.852  0 0.000 0.000 0.040 0.960 0.000
#&gt; GSM11268     6  0.0547      1.000  0 0.000 0.020 0.000 0.000 0.980
#&gt; GSM28767     5  0.0937      0.852  0 0.000 0.000 0.040 0.960 0.000
#&gt; GSM11286     2  0.0146      0.938  0 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28751     2  0.0000      0.938  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28770     5  0.0937      0.852  0 0.000 0.000 0.040 0.960 0.000
#&gt; GSM11283     4  0.1007      0.934  0 0.044 0.000 0.956 0.000 0.000
#&gt; GSM11289     5  0.0937      0.852  0 0.000 0.000 0.040 0.960 0.000
#&gt; GSM11280     2  0.0914      0.931  0 0.968 0.000 0.016 0.000 0.016
#&gt; GSM28749     5  0.4080      0.614  0 0.264 0.000 0.016 0.704 0.016
#&gt; GSM28750     6  0.0547      1.000  0 0.000 0.020 0.000 0.000 0.980
#&gt; GSM11290     3  0.1501      0.972  0 0.000 0.924 0.000 0.000 0.076
#&gt; GSM11294     3  0.1501      0.972  0 0.000 0.924 0.000 0.000 0.076
#&gt; GSM28771     4  0.0000      0.961  0 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28760     4  0.0000      0.961  0 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28774     5  0.3590      0.765  0 0.116 0.076 0.000 0.804 0.004
#&gt; GSM11284     4  0.1844      0.921  0 0.040 0.016 0.928 0.000 0.016
#&gt; GSM28761     6  0.0547      1.000  0 0.000 0.020 0.000 0.000 0.980
#&gt; GSM11278     5  0.1644      0.847  0 0.000 0.076 0.000 0.920 0.004
#&gt; GSM11291     3  0.1501      0.972  0 0.000 0.924 0.000 0.000 0.076
#&gt; GSM11277     3  0.1501      0.972  0 0.000 0.924 0.000 0.000 0.076
#&gt; GSM11272     6  0.0547      1.000  0 0.000 0.020 0.000 0.000 0.980
#&gt; GSM11285     4  0.0146      0.960  0 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28753     2  0.0000      0.938  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.0458      0.935  0 0.984 0.000 0.000 0.000 0.016
#&gt; GSM28765     2  0.0000      0.938  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.0458      0.935  0 0.984 0.000 0.000 0.000 0.016
#&gt; GSM28754     2  0.0935      0.926  0 0.964 0.032 0.000 0.000 0.004
#&gt; GSM28769     2  0.0146      0.938  0 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.1644      0.847  0 0.000 0.076 0.000 0.920 0.004
#&gt; GSM11271     5  0.4326      0.232  0 0.404 0.000 0.024 0.572 0.000
#&gt; GSM11288     2  0.4375      0.507  0 0.648 0.000 0.028 0.008 0.316
#&gt; GSM11273     5  0.1644      0.847  0 0.000 0.076 0.000 0.920 0.004
#&gt; GSM28757     2  0.1858      0.893  0 0.912 0.076 0.000 0.000 0.012
#&gt; GSM11282     5  0.1644      0.847  0 0.000 0.076 0.000 0.920 0.004
#&gt; GSM28756     2  0.0458      0.935  0 0.984 0.016 0.000 0.000 0.000
#&gt; GSM11276     2  0.2706      0.808  0 0.852 0.000 0.024 0.124 0.000
#&gt; GSM28752     2  0.0000      0.938  0 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:pam 53     0.397 2
#> CV:pam 54     0.374 3
#> CV:pam 52     0.425 4
#> CV:pam 53     0.481 5
#> CV:pam 53     0.448 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.476           0.849       0.912         0.4670 0.525   0.525
#> 3 3 0.744           0.906       0.946         0.3585 0.717   0.513
#> 4 4 0.949           0.927       0.962         0.0228 0.863   0.678
#> 5 5 0.803           0.849       0.918         0.1825 0.839   0.565
#> 6 6 0.853           0.861       0.917         0.0533 0.961   0.823
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0376      0.914 0.004 0.996
#&gt; GSM28763     2  0.0938      0.911 0.012 0.988
#&gt; GSM28764     2  0.0000      0.914 0.000 1.000
#&gt; GSM11274     1  0.8144      0.801 0.748 0.252
#&gt; GSM28772     1  0.0672      0.868 0.992 0.008
#&gt; GSM11269     1  0.0672      0.868 0.992 0.008
#&gt; GSM28775     1  0.0672      0.868 0.992 0.008
#&gt; GSM11293     1  0.0672      0.868 0.992 0.008
#&gt; GSM28755     1  0.0672      0.868 0.992 0.008
#&gt; GSM11279     1  0.0672      0.868 0.992 0.008
#&gt; GSM28758     1  0.0672      0.868 0.992 0.008
#&gt; GSM11281     1  0.0672      0.868 0.992 0.008
#&gt; GSM11287     1  0.0672      0.868 0.992 0.008
#&gt; GSM28759     1  0.0672      0.868 0.992 0.008
#&gt; GSM11292     2  0.0000      0.914 0.000 1.000
#&gt; GSM28766     2  0.0000      0.914 0.000 1.000
#&gt; GSM11268     1  0.8081      0.807 0.752 0.248
#&gt; GSM28767     2  0.0000      0.914 0.000 1.000
#&gt; GSM11286     2  0.0000      0.914 0.000 1.000
#&gt; GSM28751     2  0.8555      0.657 0.280 0.720
#&gt; GSM28770     2  0.0000      0.914 0.000 1.000
#&gt; GSM11283     2  0.6148      0.829 0.152 0.848
#&gt; GSM11289     2  0.1633      0.906 0.024 0.976
#&gt; GSM11280     2  0.5842      0.832 0.140 0.860
#&gt; GSM28749     2  0.1633      0.906 0.024 0.976
#&gt; GSM28750     1  0.8081      0.807 0.752 0.248
#&gt; GSM11290     1  0.8081      0.807 0.752 0.248
#&gt; GSM11294     1  0.8081      0.807 0.752 0.248
#&gt; GSM28771     2  0.6148      0.829 0.152 0.848
#&gt; GSM28760     2  0.6148      0.829 0.152 0.848
#&gt; GSM28774     2  0.0000      0.914 0.000 1.000
#&gt; GSM11284     2  0.4562      0.866 0.096 0.904
#&gt; GSM28761     1  0.8081      0.807 0.752 0.248
#&gt; GSM11278     2  0.0000      0.914 0.000 1.000
#&gt; GSM11291     1  0.8081      0.807 0.752 0.248
#&gt; GSM11277     1  0.8081      0.807 0.752 0.248
#&gt; GSM11272     1  0.8081      0.807 0.752 0.248
#&gt; GSM11285     2  0.6148      0.829 0.152 0.848
#&gt; GSM28753     2  0.4022      0.876 0.080 0.920
#&gt; GSM28773     2  0.7602      0.674 0.220 0.780
#&gt; GSM28765     2  0.0000      0.914 0.000 1.000
#&gt; GSM28768     2  0.9286      0.549 0.344 0.656
#&gt; GSM28754     2  0.0000      0.914 0.000 1.000
#&gt; GSM28769     2  0.7674      0.699 0.224 0.776
#&gt; GSM11275     1  0.0672      0.868 0.992 0.008
#&gt; GSM11270     2  0.0000      0.914 0.000 1.000
#&gt; GSM11271     2  0.0000      0.914 0.000 1.000
#&gt; GSM11288     2  0.7674      0.661 0.224 0.776
#&gt; GSM11273     2  0.6438      0.762 0.164 0.836
#&gt; GSM28757     2  0.0000      0.914 0.000 1.000
#&gt; GSM11282     2  0.0000      0.914 0.000 1.000
#&gt; GSM28756     2  0.0000      0.914 0.000 1.000
#&gt; GSM11276     2  0.0000      0.914 0.000 1.000
#&gt; GSM28752     2  0.0000      0.914 0.000 1.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3
#&gt; GSM28762     2  0.3192      0.880  0 0.888 0.112
#&gt; GSM28763     2  0.3192      0.880  0 0.888 0.112
#&gt; GSM28764     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11274     3  0.2625      0.918  0 0.084 0.916
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11292     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM28766     2  0.0424      0.924  0 0.992 0.008
#&gt; GSM11268     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM28767     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11286     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM28751     2  0.3686      0.861  0 0.860 0.140
#&gt; GSM28770     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11283     3  0.2625      0.918  0 0.084 0.916
#&gt; GSM11289     2  0.1031      0.919  0 0.976 0.024
#&gt; GSM11280     3  0.3941      0.848  0 0.156 0.844
#&gt; GSM28749     2  0.4002      0.844  0 0.840 0.160
#&gt; GSM28750     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM11290     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM11294     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM28771     3  0.2625      0.918  0 0.084 0.916
#&gt; GSM28760     3  0.2625      0.918  0 0.084 0.916
#&gt; GSM28774     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11284     3  0.2878      0.911  0 0.096 0.904
#&gt; GSM28761     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM11278     2  0.3116      0.879  0 0.892 0.108
#&gt; GSM11291     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM11277     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM11272     3  0.0000      0.910  0 0.000 1.000
#&gt; GSM11285     3  0.2625      0.918  0 0.084 0.916
#&gt; GSM28753     2  0.5058      0.723  0 0.756 0.244
#&gt; GSM28773     3  0.6192      0.274  0 0.420 0.580
#&gt; GSM28765     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM28768     2  0.4605      0.796  0 0.796 0.204
#&gt; GSM28754     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM28769     2  0.3340      0.874  0 0.880 0.120
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM11270     2  0.4555      0.784  0 0.800 0.200
#&gt; GSM11271     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11288     3  0.2796      0.913  0 0.092 0.908
#&gt; GSM11273     3  0.2878      0.912  0 0.096 0.904
#&gt; GSM28757     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11282     2  0.4555      0.784  0 0.800 0.200
#&gt; GSM28756     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM11276     2  0.0000      0.926  0 1.000 0.000
#&gt; GSM28752     2  0.0000      0.926  0 1.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.1118      0.952 0.000 0.964 0.000 0.036
#&gt; GSM28763     2  0.1118      0.952 0.000 0.964 0.000 0.036
#&gt; GSM28764     2  0.0707      0.956 0.000 0.980 0.000 0.020
#&gt; GSM11274     3  0.3962      0.674 0.000 0.152 0.820 0.028
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM28766     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11268     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM28767     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11286     2  0.0817      0.955 0.000 0.976 0.000 0.024
#&gt; GSM28751     2  0.1211      0.951 0.000 0.960 0.000 0.040
#&gt; GSM28770     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11283     4  0.0592      1.000 0.000 0.016 0.000 0.984
#&gt; GSM11289     2  0.0592      0.956 0.000 0.984 0.000 0.016
#&gt; GSM11280     2  0.3907      0.746 0.000 0.768 0.000 0.232
#&gt; GSM28749     2  0.1211      0.951 0.000 0.960 0.000 0.040
#&gt; GSM28750     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11290     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.0592      1.000 0.000 0.016 0.000 0.984
#&gt; GSM28760     4  0.0592      1.000 0.000 0.016 0.000 0.984
#&gt; GSM28774     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11284     2  0.3873      0.752 0.000 0.772 0.000 0.228
#&gt; GSM28761     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11278     2  0.0895      0.951 0.000 0.976 0.004 0.020
#&gt; GSM11291     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11285     4  0.0592      1.000 0.000 0.016 0.000 0.984
#&gt; GSM28753     2  0.1211      0.951 0.000 0.960 0.000 0.040
#&gt; GSM28773     2  0.1833      0.948 0.000 0.944 0.024 0.032
#&gt; GSM28765     2  0.0707      0.956 0.000 0.980 0.000 0.020
#&gt; GSM28768     2  0.1890      0.938 0.008 0.936 0.000 0.056
#&gt; GSM28754     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; GSM28769     2  0.1211      0.951 0.000 0.960 0.000 0.040
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.0895      0.951 0.000 0.976 0.004 0.020
#&gt; GSM11271     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11288     2  0.4356      0.795 0.000 0.804 0.148 0.048
#&gt; GSM11273     3  0.5576      0.216 0.000 0.444 0.536 0.020
#&gt; GSM28757     2  0.0592      0.956 0.000 0.984 0.000 0.016
#&gt; GSM11282     2  0.0895      0.951 0.000 0.976 0.004 0.020
#&gt; GSM28756     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11276     2  0.0000      0.955 0.000 1.000 0.000 0.000
#&gt; GSM28752     2  0.1118      0.952 0.000 0.964 0.000 0.036
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5
#&gt; GSM28762     5  0.3274     0.8212 0.00 0.220 0.000 0.000 0.780
#&gt; GSM28763     5  0.3274     0.8212 0.00 0.220 0.000 0.000 0.780
#&gt; GSM28764     2  0.4235     0.0179 0.00 0.576 0.000 0.000 0.424
#&gt; GSM11274     3  0.3280     0.7908 0.00 0.004 0.808 0.004 0.184
#&gt; GSM28772     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9837 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0162     0.8755 0.00 0.996 0.000 0.000 0.004
#&gt; GSM28766     2  0.0609     0.8621 0.00 0.980 0.000 0.000 0.020
#&gt; GSM11268     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28767     2  0.0162     0.8755 0.00 0.996 0.000 0.000 0.004
#&gt; GSM11286     2  0.2179     0.8215 0.00 0.888 0.000 0.000 0.112
#&gt; GSM28751     5  0.3109     0.8368 0.00 0.200 0.000 0.000 0.800
#&gt; GSM28770     2  0.0162     0.8755 0.00 0.996 0.000 0.000 0.004
#&gt; GSM11283     4  0.0290     0.9293 0.00 0.000 0.000 0.992 0.008
#&gt; GSM11289     5  0.4307     0.0149 0.00 0.496 0.000 0.000 0.504
#&gt; GSM11280     5  0.1197     0.7998 0.00 0.048 0.000 0.000 0.952
#&gt; GSM28749     5  0.1410     0.8102 0.00 0.060 0.000 0.000 0.940
#&gt; GSM28750     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11290     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28771     4  0.0290     0.9293 0.00 0.000 0.000 0.992 0.008
#&gt; GSM28760     4  0.0290     0.9293 0.00 0.000 0.000 0.992 0.008
#&gt; GSM28774     2  0.0794     0.8780 0.00 0.972 0.000 0.000 0.028
#&gt; GSM11284     5  0.1270     0.8024 0.00 0.052 0.000 0.000 0.948
#&gt; GSM28761     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11278     2  0.3756     0.6975 0.00 0.744 0.000 0.008 0.248
#&gt; GSM11291     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.0000     0.9765 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11285     4  0.3086     0.7610 0.00 0.004 0.000 0.816 0.180
#&gt; GSM28753     5  0.1478     0.8123 0.00 0.064 0.000 0.000 0.936
#&gt; GSM28773     5  0.3160     0.8396 0.00 0.188 0.004 0.000 0.808
#&gt; GSM28765     2  0.0963     0.8777 0.00 0.964 0.000 0.000 0.036
#&gt; GSM28768     5  0.3143     0.8330 0.00 0.204 0.000 0.000 0.796
#&gt; GSM28754     2  0.0963     0.8777 0.00 0.964 0.000 0.000 0.036
#&gt; GSM28769     5  0.3074     0.8378 0.00 0.196 0.000 0.000 0.804
#&gt; GSM11275     1  0.2280     0.8233 0.88 0.000 0.000 0.000 0.120
#&gt; GSM11270     2  0.3756     0.6975 0.00 0.744 0.000 0.008 0.248
#&gt; GSM11271     2  0.0000     0.8740 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11288     5  0.1638     0.7344 0.00 0.004 0.064 0.000 0.932
#&gt; GSM11273     2  0.5282     0.6445 0.00 0.700 0.144 0.008 0.148
#&gt; GSM28757     2  0.0963     0.8777 0.00 0.964 0.000 0.000 0.036
#&gt; GSM11282     2  0.3756     0.6975 0.00 0.744 0.000 0.008 0.248
#&gt; GSM28756     2  0.0162     0.8755 0.00 0.996 0.000 0.000 0.004
#&gt; GSM11276     2  0.0963     0.8777 0.00 0.964 0.000 0.000 0.036
#&gt; GSM28752     2  0.0963     0.8777 0.00 0.964 0.000 0.000 0.036
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.2260     0.8521 0.000 0.860 0.000 0.000 0.140 0.000
#&gt; GSM28763     2  0.2558     0.8362 0.000 0.840 0.000 0.000 0.156 0.004
#&gt; GSM28764     5  0.3864     0.0326 0.000 0.480 0.000 0.000 0.520 0.000
#&gt; GSM11274     3  0.2772     0.8103 0.000 0.004 0.816 0.000 0.000 0.180
#&gt; GSM28772     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9995 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0865     0.8091 0.000 0.000 0.000 0.000 0.964 0.036
#&gt; GSM28766     5  0.1444     0.7722 0.000 0.000 0.000 0.000 0.928 0.072
#&gt; GSM11268     3  0.0146     0.9760 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28767     5  0.0713     0.8138 0.000 0.000 0.000 0.000 0.972 0.028
#&gt; GSM11286     5  0.3351     0.6181 0.000 0.288 0.000 0.000 0.712 0.000
#&gt; GSM28751     2  0.1910     0.8662 0.000 0.892 0.000 0.000 0.108 0.000
#&gt; GSM28770     5  0.0790     0.8117 0.000 0.000 0.000 0.000 0.968 0.032
#&gt; GSM11283     4  0.0146     0.9570 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM11289     2  0.4141     0.2074 0.000 0.596 0.000 0.000 0.388 0.016
#&gt; GSM11280     2  0.1152     0.8360 0.000 0.952 0.000 0.004 0.000 0.044
#&gt; GSM28749     2  0.1794     0.8490 0.000 0.924 0.000 0.000 0.040 0.036
#&gt; GSM28750     3  0.0146     0.9760 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11290     3  0.0000     0.9757 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0146     0.9750 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28771     4  0.0146     0.9570 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM28760     4  0.0146     0.9570 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM28774     5  0.0790     0.8251 0.000 0.032 0.000 0.000 0.968 0.000
#&gt; GSM11284     2  0.1410     0.8370 0.000 0.944 0.000 0.008 0.004 0.044
#&gt; GSM28761     3  0.0146     0.9760 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11278     6  0.3012     0.8830 0.000 0.008 0.000 0.000 0.196 0.796
#&gt; GSM11291     3  0.0146     0.9750 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11277     3  0.0146     0.9750 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11272     3  0.0146     0.9760 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11285     4  0.1714     0.8682 0.000 0.092 0.000 0.908 0.000 0.000
#&gt; GSM28753     2  0.0806     0.8488 0.000 0.972 0.000 0.000 0.008 0.020
#&gt; GSM28773     2  0.2357     0.8622 0.000 0.872 0.000 0.000 0.116 0.012
#&gt; GSM28765     5  0.2520     0.7917 0.000 0.152 0.000 0.000 0.844 0.004
#&gt; GSM28768     2  0.1814     0.8667 0.000 0.900 0.000 0.000 0.100 0.000
#&gt; GSM28754     5  0.2006     0.8240 0.000 0.080 0.000 0.000 0.904 0.016
#&gt; GSM28769     2  0.2402     0.8492 0.000 0.856 0.000 0.000 0.140 0.004
#&gt; GSM11275     1  0.0146     0.9948 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11270     6  0.2948     0.8809 0.000 0.008 0.000 0.000 0.188 0.804
#&gt; GSM11271     5  0.0790     0.8117 0.000 0.000 0.000 0.000 0.968 0.032
#&gt; GSM11288     2  0.1657     0.8344 0.000 0.928 0.016 0.000 0.000 0.056
#&gt; GSM11273     6  0.3409     0.6367 0.000 0.004 0.144 0.000 0.044 0.808
#&gt; GSM28757     5  0.2146     0.8125 0.000 0.116 0.000 0.000 0.880 0.004
#&gt; GSM11282     6  0.3012     0.8830 0.000 0.008 0.000 0.000 0.196 0.796
#&gt; GSM28756     5  0.0713     0.8138 0.000 0.000 0.000 0.000 0.972 0.028
#&gt; GSM11276     5  0.2135     0.8076 0.000 0.128 0.000 0.000 0.872 0.000
#&gt; GSM28752     5  0.2520     0.7917 0.000 0.152 0.000 0.000 0.844 0.004
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:mclust 54     0.398 2
#> CV:mclust 53     0.443 3
#> CV:mclust 53     0.520 4
#> CV:mclust 52     0.409 5
#> CV:mclust 52     0.402 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.973       0.988         0.3838 0.628   0.628
#> 3 3 0.970           0.941       0.974         0.5704 0.740   0.599
#> 4 4 0.700           0.761       0.871         0.2014 0.839   0.614
#> 5 5 0.777           0.744       0.880         0.0795 0.899   0.650
#> 6 6 0.745           0.570       0.753         0.0527 0.905   0.595
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.984 0.000 1.000
#&gt; GSM28763     2  0.6531      0.804 0.168 0.832
#&gt; GSM28764     2  0.0000      0.984 0.000 1.000
#&gt; GSM11274     2  0.0000      0.984 0.000 1.000
#&gt; GSM28772     1  0.0000      0.998 1.000 0.000
#&gt; GSM11269     1  0.0000      0.998 1.000 0.000
#&gt; GSM28775     1  0.0000      0.998 1.000 0.000
#&gt; GSM11293     1  0.0000      0.998 1.000 0.000
#&gt; GSM28755     1  0.0000      0.998 1.000 0.000
#&gt; GSM11279     1  0.0000      0.998 1.000 0.000
#&gt; GSM28758     1  0.0000      0.998 1.000 0.000
#&gt; GSM11281     1  0.0000      0.998 1.000 0.000
#&gt; GSM11287     1  0.0000      0.998 1.000 0.000
#&gt; GSM28759     1  0.0000      0.998 1.000 0.000
#&gt; GSM11292     2  0.0000      0.984 0.000 1.000
#&gt; GSM28766     2  0.0000      0.984 0.000 1.000
#&gt; GSM11268     2  0.0000      0.984 0.000 1.000
#&gt; GSM28767     2  0.0000      0.984 0.000 1.000
#&gt; GSM11286     2  0.0938      0.974 0.012 0.988
#&gt; GSM28751     1  0.1843      0.970 0.972 0.028
#&gt; GSM28770     2  0.0000      0.984 0.000 1.000
#&gt; GSM11283     2  0.0000      0.984 0.000 1.000
#&gt; GSM11289     2  0.0000      0.984 0.000 1.000
#&gt; GSM11280     2  0.0000      0.984 0.000 1.000
#&gt; GSM28749     2  0.0000      0.984 0.000 1.000
#&gt; GSM28750     2  0.0000      0.984 0.000 1.000
#&gt; GSM11290     2  0.0000      0.984 0.000 1.000
#&gt; GSM11294     2  0.0000      0.984 0.000 1.000
#&gt; GSM28771     2  0.0000      0.984 0.000 1.000
#&gt; GSM28760     2  0.0000      0.984 0.000 1.000
#&gt; GSM28774     2  0.0000      0.984 0.000 1.000
#&gt; GSM11284     2  0.0000      0.984 0.000 1.000
#&gt; GSM28761     2  0.0000      0.984 0.000 1.000
#&gt; GSM11278     2  0.0000      0.984 0.000 1.000
#&gt; GSM11291     2  0.0000      0.984 0.000 1.000
#&gt; GSM11277     2  0.0000      0.984 0.000 1.000
#&gt; GSM11272     2  0.5408      0.859 0.124 0.876
#&gt; GSM11285     2  0.0000      0.984 0.000 1.000
#&gt; GSM28753     2  0.0000      0.984 0.000 1.000
#&gt; GSM28773     2  0.0000      0.984 0.000 1.000
#&gt; GSM28765     2  0.0938      0.974 0.012 0.988
#&gt; GSM28768     1  0.0000      0.998 1.000 0.000
#&gt; GSM28754     2  0.0000      0.984 0.000 1.000
#&gt; GSM28769     2  0.9044      0.549 0.320 0.680
#&gt; GSM11275     1  0.0000      0.998 1.000 0.000
#&gt; GSM11270     2  0.0000      0.984 0.000 1.000
#&gt; GSM11271     2  0.0000      0.984 0.000 1.000
#&gt; GSM11288     2  0.0000      0.984 0.000 1.000
#&gt; GSM11273     2  0.0000      0.984 0.000 1.000
#&gt; GSM28757     2  0.0000      0.984 0.000 1.000
#&gt; GSM11282     2  0.0000      0.984 0.000 1.000
#&gt; GSM28756     2  0.0000      0.984 0.000 1.000
#&gt; GSM11276     2  0.0000      0.984 0.000 1.000
#&gt; GSM28752     2  0.0376      0.981 0.004 0.996
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28766     2  0.0237      0.957 0.000 0.996 0.004
#&gt; GSM11268     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28751     2  0.5882      0.484 0.348 0.652 0.000
#&gt; GSM28770     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28749     2  0.0592      0.952 0.000 0.988 0.012
#&gt; GSM28750     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28771     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28760     2  0.5529      0.608 0.000 0.704 0.296
#&gt; GSM28774     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11278     2  0.0424      0.954 0.000 0.992 0.008
#&gt; GSM11291     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11285     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28773     2  0.5810      0.529 0.000 0.664 0.336
#&gt; GSM28765     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28768     1  0.2796      0.869 0.908 0.092 0.000
#&gt; GSM28754     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28769     2  0.0237      0.957 0.004 0.996 0.000
#&gt; GSM11275     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11270     2  0.4291      0.781 0.000 0.820 0.180
#&gt; GSM11271     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11288     3  0.3406      0.889 0.028 0.068 0.904
#&gt; GSM11273     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28757     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11282     2  0.0747      0.949 0.000 0.984 0.016
#&gt; GSM28756     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.959 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.959 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.2704      0.770 0.000 0.876 0.000 0.124
#&gt; GSM28763     2  0.2814      0.757 0.000 0.868 0.000 0.132
#&gt; GSM28764     2  0.0592      0.873 0.000 0.984 0.000 0.016
#&gt; GSM11274     3  0.0707      0.862 0.000 0.000 0.980 0.020
#&gt; GSM28772     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0921      0.873 0.000 0.972 0.000 0.028
#&gt; GSM28766     2  0.0921      0.873 0.000 0.972 0.000 0.028
#&gt; GSM11268     3  0.4543      0.694 0.000 0.000 0.676 0.324
#&gt; GSM28767     2  0.0707      0.873 0.000 0.980 0.000 0.020
#&gt; GSM11286     2  0.3356      0.747 0.000 0.824 0.000 0.176
#&gt; GSM28751     4  0.7566      0.480 0.292 0.228 0.000 0.480
#&gt; GSM28770     2  0.0592      0.873 0.000 0.984 0.000 0.016
#&gt; GSM11283     4  0.4304      0.609 0.000 0.284 0.000 0.716
#&gt; GSM11289     2  0.0469      0.872 0.000 0.988 0.000 0.012
#&gt; GSM11280     4  0.3764      0.647 0.000 0.216 0.000 0.784
#&gt; GSM28749     4  0.5386      0.423 0.000 0.344 0.024 0.632
#&gt; GSM28750     3  0.3219      0.814 0.000 0.000 0.836 0.164
#&gt; GSM11290     3  0.0188      0.867 0.000 0.000 0.996 0.004
#&gt; GSM11294     3  0.0000      0.867 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.4808      0.634 0.000 0.236 0.028 0.736
#&gt; GSM28760     4  0.5292      0.471 0.000 0.064 0.208 0.728
#&gt; GSM28774     2  0.0469      0.872 0.000 0.988 0.000 0.012
#&gt; GSM11284     2  0.4730      0.220 0.000 0.636 0.000 0.364
#&gt; GSM28761     3  0.4564      0.687 0.000 0.000 0.672 0.328
#&gt; GSM11278     2  0.2882      0.786 0.000 0.892 0.084 0.024
#&gt; GSM11291     3  0.0000      0.867 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.867 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.4040      0.763 0.000 0.000 0.752 0.248
#&gt; GSM11285     4  0.5183      0.421 0.000 0.408 0.008 0.584
#&gt; GSM28753     4  0.4331      0.620 0.000 0.288 0.000 0.712
#&gt; GSM28773     4  0.6546      0.370 0.000 0.172 0.192 0.636
#&gt; GSM28765     2  0.2868      0.779 0.000 0.864 0.000 0.136
#&gt; GSM28768     1  0.1936      0.927 0.940 0.032 0.000 0.028
#&gt; GSM28754     2  0.0707      0.873 0.000 0.980 0.000 0.020
#&gt; GSM28769     4  0.5244      0.428 0.012 0.388 0.000 0.600
#&gt; GSM11275     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.5560      0.222 0.000 0.584 0.392 0.024
#&gt; GSM11271     2  0.0336      0.874 0.000 0.992 0.000 0.008
#&gt; GSM11288     4  0.6316     -0.387 0.048 0.004 0.472 0.476
#&gt; GSM11273     3  0.1042      0.851 0.000 0.008 0.972 0.020
#&gt; GSM28757     2  0.2647      0.810 0.000 0.880 0.000 0.120
#&gt; GSM11282     2  0.2174      0.829 0.000 0.928 0.052 0.020
#&gt; GSM28756     2  0.0592      0.873 0.000 0.984 0.000 0.016
#&gt; GSM11276     2  0.0336      0.873 0.000 0.992 0.000 0.008
#&gt; GSM28752     2  0.1389      0.859 0.000 0.952 0.000 0.048
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.1965    0.87311 0.000 0.924 0.000 0.052 0.024
#&gt; GSM28763     2  0.2685    0.84776 0.000 0.880 0.000 0.092 0.028
#&gt; GSM28764     2  0.0771    0.88884 0.000 0.976 0.000 0.004 0.020
#&gt; GSM11274     3  0.0451    0.80760 0.000 0.000 0.988 0.004 0.008
#&gt; GSM28772     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.1651    0.88386 0.000 0.944 0.012 0.008 0.036
#&gt; GSM28766     2  0.2158    0.87440 0.000 0.920 0.020 0.008 0.052
#&gt; GSM11268     5  0.3109    0.61573 0.000 0.000 0.200 0.000 0.800
#&gt; GSM28767     2  0.1280    0.88675 0.000 0.960 0.008 0.008 0.024
#&gt; GSM11286     2  0.4299    0.45958 0.000 0.608 0.000 0.004 0.388
#&gt; GSM28751     5  0.6743    0.44395 0.148 0.124 0.000 0.112 0.616
#&gt; GSM28770     2  0.1299    0.88567 0.000 0.960 0.012 0.008 0.020
#&gt; GSM11283     4  0.0290    0.78889 0.000 0.008 0.000 0.992 0.000
#&gt; GSM11289     2  0.1413    0.88545 0.000 0.956 0.012 0.012 0.020
#&gt; GSM11280     4  0.2516    0.70507 0.000 0.000 0.000 0.860 0.140
#&gt; GSM28749     5  0.1357    0.65216 0.000 0.048 0.004 0.000 0.948
#&gt; GSM28750     3  0.4273    0.03682 0.000 0.000 0.552 0.000 0.448
#&gt; GSM11290     3  0.1121    0.81189 0.000 0.000 0.956 0.000 0.044
#&gt; GSM11294     3  0.0963    0.81648 0.000 0.000 0.964 0.000 0.036
#&gt; GSM28771     4  0.0290    0.78889 0.000 0.008 0.000 0.992 0.000
#&gt; GSM28760     4  0.0290    0.78889 0.000 0.008 0.000 0.992 0.000
#&gt; GSM28774     2  0.1197    0.88347 0.000 0.952 0.000 0.000 0.048
#&gt; GSM11284     4  0.4798    0.10095 0.000 0.440 0.000 0.540 0.020
#&gt; GSM28761     5  0.3305    0.59954 0.000 0.000 0.224 0.000 0.776
#&gt; GSM11278     2  0.3399    0.75686 0.000 0.812 0.168 0.000 0.020
#&gt; GSM11291     3  0.0963    0.81648 0.000 0.000 0.964 0.000 0.036
#&gt; GSM11277     3  0.0963    0.81648 0.000 0.000 0.964 0.000 0.036
#&gt; GSM11272     5  0.4030    0.40571 0.000 0.000 0.352 0.000 0.648
#&gt; GSM11285     4  0.0404    0.78781 0.000 0.012 0.000 0.988 0.000
#&gt; GSM28753     4  0.4969    0.26424 0.000 0.036 0.000 0.588 0.376
#&gt; GSM28773     5  0.1498    0.65935 0.000 0.016 0.024 0.008 0.952
#&gt; GSM28765     5  0.4446    0.00679 0.000 0.476 0.000 0.004 0.520
#&gt; GSM28768     1  0.4231    0.70627 0.776 0.060 0.000 0.004 0.160
#&gt; GSM28754     2  0.1732    0.86906 0.000 0.920 0.000 0.000 0.080
#&gt; GSM28769     5  0.4010    0.58059 0.000 0.160 0.000 0.056 0.784
#&gt; GSM11275     1  0.0000    0.97837 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.4746    0.27248 0.000 0.376 0.600 0.000 0.024
#&gt; GSM11271     2  0.0510    0.88861 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11288     5  0.3715    0.55620 0.000 0.000 0.260 0.004 0.736
#&gt; GSM11273     3  0.0579    0.79259 0.000 0.008 0.984 0.000 0.008
#&gt; GSM28757     2  0.4288    0.46739 0.000 0.612 0.000 0.004 0.384
#&gt; GSM11282     2  0.1907    0.87073 0.000 0.928 0.044 0.000 0.028
#&gt; GSM28756     2  0.1197    0.88086 0.000 0.952 0.000 0.000 0.048
#&gt; GSM11276     2  0.0290    0.88799 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28752     2  0.3123    0.78580 0.000 0.812 0.000 0.004 0.184
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.4614    0.24162 0.000 0.568 0.008 0.004 0.400 0.020
#&gt; GSM28763     2  0.4399    0.25999 0.000 0.604 0.008 0.008 0.372 0.008
#&gt; GSM28764     5  0.0363    0.64487 0.000 0.012 0.000 0.000 0.988 0.000
#&gt; GSM11274     3  0.0692    0.75482 0.000 0.004 0.976 0.000 0.000 0.020
#&gt; GSM28772     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0146    0.95806 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0260    0.95601 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000    0.95916 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0146    0.95806 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.1897    0.58505 0.000 0.004 0.004 0.000 0.908 0.084
#&gt; GSM28766     5  0.3411    0.42580 0.000 0.004 0.008 0.000 0.756 0.232
#&gt; GSM11268     6  0.1500    0.81511 0.000 0.052 0.012 0.000 0.000 0.936
#&gt; GSM28767     5  0.0146    0.64497 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11286     2  0.4176    0.45903 0.000 0.720 0.000 0.000 0.212 0.068
#&gt; GSM28751     2  0.7040    0.29367 0.088 0.536 0.000 0.032 0.152 0.192
#&gt; GSM28770     5  0.0291    0.64583 0.000 0.004 0.004 0.000 0.992 0.000
#&gt; GSM11283     4  0.0000    0.73743 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11289     5  0.0291    0.64403 0.000 0.000 0.004 0.000 0.992 0.004
#&gt; GSM11280     4  0.4193    0.55858 0.000 0.272 0.000 0.684 0.000 0.044
#&gt; GSM28749     6  0.4687    0.45861 0.000 0.336 0.000 0.000 0.060 0.604
#&gt; GSM28750     6  0.3014    0.71619 0.000 0.012 0.184 0.000 0.000 0.804
#&gt; GSM11290     3  0.2520    0.70267 0.000 0.004 0.844 0.000 0.000 0.152
#&gt; GSM11294     3  0.1970    0.75602 0.000 0.008 0.900 0.000 0.000 0.092
#&gt; GSM28771     4  0.0000    0.73743 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28760     4  0.0000    0.73743 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28774     5  0.4473   -0.02940 0.000 0.480 0.028 0.000 0.492 0.000
#&gt; GSM11284     4  0.5711    0.07402 0.000 0.380 0.000 0.472 0.144 0.004
#&gt; GSM28761     6  0.1059    0.82474 0.000 0.016 0.016 0.000 0.004 0.964
#&gt; GSM11278     3  0.5852    0.08329 0.000 0.328 0.464 0.000 0.208 0.000
#&gt; GSM11291     3  0.1918    0.75820 0.000 0.008 0.904 0.000 0.000 0.088
#&gt; GSM11277     3  0.1753    0.75994 0.000 0.004 0.912 0.000 0.000 0.084
#&gt; GSM11272     6  0.2997    0.82058 0.000 0.060 0.096 0.000 0.000 0.844
#&gt; GSM11285     4  0.1219    0.72104 0.000 0.004 0.000 0.948 0.048 0.000
#&gt; GSM28753     4  0.7060    0.25520 0.000 0.264 0.000 0.440 0.104 0.192
#&gt; GSM28773     2  0.3864   -0.25239 0.000 0.520 0.000 0.000 0.000 0.480
#&gt; GSM28765     2  0.5595    0.43523 0.000 0.540 0.000 0.000 0.268 0.192
#&gt; GSM28768     1  0.4511    0.37526 0.620 0.332 0.000 0.000 0.048 0.000
#&gt; GSM28754     2  0.4385    0.00275 0.000 0.532 0.024 0.000 0.444 0.000
#&gt; GSM28769     2  0.5483    0.20987 0.008 0.584 0.000 0.012 0.088 0.308
#&gt; GSM11275     1  0.0260    0.95601 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.4406    0.40225 0.000 0.336 0.624 0.000 0.040 0.000
#&gt; GSM11271     5  0.0547    0.64325 0.000 0.020 0.000 0.000 0.980 0.000
#&gt; GSM11288     6  0.2326    0.82873 0.000 0.028 0.060 0.000 0.012 0.900
#&gt; GSM11273     3  0.0458    0.74684 0.000 0.016 0.984 0.000 0.000 0.000
#&gt; GSM28757     2  0.3766    0.44260 0.000 0.748 0.000 0.000 0.212 0.040
#&gt; GSM11282     5  0.5997    0.03651 0.000 0.344 0.240 0.000 0.416 0.000
#&gt; GSM28756     5  0.4262   -0.01974 0.000 0.476 0.016 0.000 0.508 0.000
#&gt; GSM11276     5  0.2668    0.51211 0.000 0.168 0.000 0.000 0.828 0.004
#&gt; GSM28752     5  0.4535   -0.19083 0.000 0.480 0.000 0.000 0.488 0.032
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:NMF 54     0.398 2
#> CV:NMF 53     0.373 3
#> CV:NMF 45     0.504 4
#> CV:NMF 45     0.441 5
#> CV:NMF 34     0.430 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3313 0.669   0.669
#> 3 3 0.841           0.894       0.950         0.7438 0.716   0.576
#> 4 4 0.782           0.820       0.912         0.1627 0.969   0.918
#> 5 5 0.835           0.847       0.915         0.0695 0.930   0.803
#> 6 6 0.891           0.912       0.959         0.0281 0.986   0.951
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      1.000 0.000 1.000
#&gt; GSM28763     2  0.0000      1.000 0.000 1.000
#&gt; GSM28764     2  0.0000      1.000 0.000 1.000
#&gt; GSM11274     2  0.0000      1.000 0.000 1.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000
#&gt; GSM11292     2  0.0000      1.000 0.000 1.000
#&gt; GSM28766     2  0.0000      1.000 0.000 1.000
#&gt; GSM11268     2  0.0000      1.000 0.000 1.000
#&gt; GSM28767     2  0.0000      1.000 0.000 1.000
#&gt; GSM11286     2  0.0000      1.000 0.000 1.000
#&gt; GSM28751     2  0.0000      1.000 0.000 1.000
#&gt; GSM28770     2  0.0000      1.000 0.000 1.000
#&gt; GSM11283     2  0.0000      1.000 0.000 1.000
#&gt; GSM11289     2  0.0000      1.000 0.000 1.000
#&gt; GSM11280     2  0.0000      1.000 0.000 1.000
#&gt; GSM28749     2  0.0000      1.000 0.000 1.000
#&gt; GSM28750     2  0.0000      1.000 0.000 1.000
#&gt; GSM11290     2  0.0000      1.000 0.000 1.000
#&gt; GSM11294     2  0.0000      1.000 0.000 1.000
#&gt; GSM28771     2  0.0000      1.000 0.000 1.000
#&gt; GSM28760     2  0.0000      1.000 0.000 1.000
#&gt; GSM28774     2  0.0000      1.000 0.000 1.000
#&gt; GSM11284     2  0.0000      1.000 0.000 1.000
#&gt; GSM28761     2  0.0000      1.000 0.000 1.000
#&gt; GSM11278     2  0.0000      1.000 0.000 1.000
#&gt; GSM11291     2  0.0000      1.000 0.000 1.000
#&gt; GSM11277     2  0.0000      1.000 0.000 1.000
#&gt; GSM11272     2  0.0000      1.000 0.000 1.000
#&gt; GSM11285     2  0.0000      1.000 0.000 1.000
#&gt; GSM28753     2  0.0000      1.000 0.000 1.000
#&gt; GSM28773     2  0.0000      1.000 0.000 1.000
#&gt; GSM28765     2  0.0000      1.000 0.000 1.000
#&gt; GSM28768     2  0.0376      0.996 0.004 0.996
#&gt; GSM28754     2  0.0000      1.000 0.000 1.000
#&gt; GSM28769     2  0.0000      1.000 0.000 1.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000
#&gt; GSM11270     2  0.0000      1.000 0.000 1.000
#&gt; GSM11271     2  0.0000      1.000 0.000 1.000
#&gt; GSM11288     2  0.0000      1.000 0.000 1.000
#&gt; GSM11273     2  0.0000      1.000 0.000 1.000
#&gt; GSM28757     2  0.0000      1.000 0.000 1.000
#&gt; GSM11282     2  0.0000      1.000 0.000 1.000
#&gt; GSM28756     2  0.0000      1.000 0.000 1.000
#&gt; GSM11276     2  0.0000      1.000 0.000 1.000
#&gt; GSM28752     2  0.0000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28763     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28764     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM11274     3  0.0424      0.436 0.008 0.000 0.992
#&gt; GSM28772     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11269     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM28775     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11293     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM28755     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11279     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM28758     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11281     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11287     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM28759     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11292     2  0.6299      0.991 0.000 0.524 0.476
#&gt; GSM28766     2  0.6299      0.991 0.000 0.524 0.476
#&gt; GSM11268     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM28767     2  0.6299      0.991 0.000 0.524 0.476
#&gt; GSM11286     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28751     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28770     2  0.6299      0.991 0.000 0.524 0.476
#&gt; GSM11283     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM11289     2  0.6299      0.991 0.000 0.524 0.476
#&gt; GSM11280     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28749     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28750     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM11290     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM11294     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM28771     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28760     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28774     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM11284     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28761     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM11278     3  0.0000      0.423 0.000 0.000 1.000
#&gt; GSM11291     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM11277     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM11272     3  0.6291      0.712 0.468 0.000 0.532
#&gt; GSM11285     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28753     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28773     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28765     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28768     2  0.6286      0.993 0.000 0.536 0.464
#&gt; GSM28754     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28769     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM11275     1  0.6291      1.000 0.532 0.468 0.000
#&gt; GSM11270     3  0.0000      0.423 0.000 0.000 1.000
#&gt; GSM11271     2  0.6299      0.991 0.000 0.524 0.476
#&gt; GSM11288     3  0.8894      0.578 0.300 0.152 0.548
#&gt; GSM11273     3  0.0000      0.423 0.000 0.000 1.000
#&gt; GSM28757     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM11282     3  0.0000      0.423 0.000 0.000 1.000
#&gt; GSM28756     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM11276     2  0.6291      0.998 0.000 0.532 0.468
#&gt; GSM28752     2  0.6291      0.998 0.000 0.532 0.468
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28764     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM11274     3  0.0000      0.688 0.000 0.000 1.000 0.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.3688      0.791 0.000 0.792 0.208 0.000
#&gt; GSM28766     2  0.3688      0.791 0.000 0.792 0.208 0.000
#&gt; GSM11268     4  0.0000      0.817 0.000 0.000 0.000 1.000
#&gt; GSM28767     2  0.3688      0.791 0.000 0.792 0.208 0.000
#&gt; GSM11286     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28770     2  0.3688      0.791 0.000 0.792 0.208 0.000
#&gt; GSM11283     2  0.4454      0.648 0.000 0.692 0.308 0.000
#&gt; GSM11289     2  0.3688      0.791 0.000 0.792 0.208 0.000
#&gt; GSM11280     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28750     4  0.1022      0.788 0.000 0.000 0.032 0.968
#&gt; GSM11290     3  0.4989      0.407 0.000 0.000 0.528 0.472
#&gt; GSM11294     3  0.4989      0.407 0.000 0.000 0.528 0.472
#&gt; GSM28771     2  0.4454      0.648 0.000 0.692 0.308 0.000
#&gt; GSM28760     2  0.4454      0.648 0.000 0.692 0.308 0.000
#&gt; GSM28774     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM11284     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28761     4  0.0000      0.817 0.000 0.000 0.000 1.000
#&gt; GSM11278     3  0.0336      0.691 0.000 0.008 0.992 0.000
#&gt; GSM11291     3  0.4989      0.407 0.000 0.000 0.528 0.472
#&gt; GSM11277     3  0.4989      0.407 0.000 0.000 0.528 0.472
#&gt; GSM11272     4  0.0000      0.817 0.000 0.000 0.000 1.000
#&gt; GSM11285     2  0.4134      0.712 0.000 0.740 0.260 0.000
#&gt; GSM28753     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28773     2  0.1118      0.890 0.000 0.964 0.036 0.000
#&gt; GSM28765     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28768     2  0.0188      0.903 0.004 0.996 0.000 0.000
#&gt; GSM28754     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.0336      0.691 0.000 0.008 0.992 0.000
#&gt; GSM11271     2  0.3688      0.791 0.000 0.792 0.208 0.000
#&gt; GSM11288     4  0.4792      0.373 0.000 0.312 0.008 0.680
#&gt; GSM11273     3  0.0336      0.691 0.000 0.008 0.992 0.000
#&gt; GSM28757     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM11282     3  0.0336      0.691 0.000 0.008 0.992 0.000
#&gt; GSM28756     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM11276     2  0.0000      0.905 0.000 1.000 0.000 0.000
#&gt; GSM28752     2  0.0000      0.905 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11274     3  0.3895      0.630 0.000 0.000 0.680 0.320 0.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.4159      0.766 0.000 0.776 0.156 0.068 0.000
#&gt; GSM28766     2  0.4159      0.766 0.000 0.776 0.156 0.068 0.000
#&gt; GSM11268     5  0.0000      0.832 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     2  0.4159      0.766 0.000 0.776 0.156 0.068 0.000
#&gt; GSM11286     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28751     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28770     2  0.4159      0.766 0.000 0.776 0.156 0.068 0.000
#&gt; GSM11283     4  0.1478      0.901 0.000 0.064 0.000 0.936 0.000
#&gt; GSM11289     2  0.4159      0.766 0.000 0.776 0.156 0.068 0.000
#&gt; GSM11280     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28749     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28750     5  0.1732      0.782 0.000 0.000 0.080 0.000 0.920
#&gt; GSM11290     3  0.3074      0.563 0.000 0.000 0.804 0.000 0.196
#&gt; GSM11294     3  0.3074      0.563 0.000 0.000 0.804 0.000 0.196
#&gt; GSM28771     4  0.1478      0.901 0.000 0.064 0.000 0.936 0.000
#&gt; GSM28760     4  0.1478      0.901 0.000 0.064 0.000 0.936 0.000
#&gt; GSM28774     2  0.0162      0.931 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11284     2  0.0162      0.931 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28761     5  0.0000      0.832 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     3  0.4165      0.632 0.000 0.008 0.672 0.320 0.000
#&gt; GSM11291     3  0.3074      0.563 0.000 0.000 0.804 0.000 0.196
#&gt; GSM11277     3  0.3074      0.563 0.000 0.000 0.804 0.000 0.196
#&gt; GSM11272     5  0.0000      0.832 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11285     4  0.3143      0.714 0.000 0.204 0.000 0.796 0.000
#&gt; GSM28753     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.1041      0.911 0.000 0.964 0.032 0.004 0.000
#&gt; GSM28765     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.0162      0.930 0.004 0.996 0.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28769     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.4165      0.632 0.000 0.008 0.672 0.320 0.000
#&gt; GSM11271     2  0.4159      0.766 0.000 0.776 0.156 0.068 0.000
#&gt; GSM11288     5  0.4213      0.377 0.000 0.308 0.000 0.012 0.680
#&gt; GSM11273     3  0.4165      0.632 0.000 0.008 0.672 0.320 0.000
#&gt; GSM28757     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11282     3  0.4165      0.632 0.000 0.008 0.672 0.320 0.000
#&gt; GSM28756     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11276     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28752     2  0.0000      0.932 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5   p6
#&gt; GSM28762     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28763     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28764     2  0.0260      0.926 0.000 0.992 0.000 0.000 0.008 0.00
#&gt; GSM11274     5  0.0260      0.988 0.000 0.000 0.008 0.000 0.992 0.00
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11292     2  0.3892      0.761 0.000 0.752 0.000 0.060 0.188 0.00
#&gt; GSM28766     2  0.3892      0.761 0.000 0.752 0.000 0.060 0.188 0.00
#&gt; GSM11268     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.00
#&gt; GSM28767     2  0.3892      0.761 0.000 0.752 0.000 0.060 0.188 0.00
#&gt; GSM11286     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28751     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28770     2  0.3892      0.761 0.000 0.752 0.000 0.060 0.188 0.00
#&gt; GSM11283     4  0.0000      0.906 0.000 0.000 0.000 1.000 0.000 0.00
#&gt; GSM11289     2  0.3892      0.761 0.000 0.752 0.000 0.060 0.188 0.00
#&gt; GSM11280     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28749     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28750     6  0.1556      0.782 0.000 0.000 0.080 0.000 0.000 0.92
#&gt; GSM11290     3  0.0146      1.000 0.000 0.000 0.996 0.000 0.004 0.00
#&gt; GSM11294     3  0.0146      1.000 0.000 0.000 0.996 0.000 0.004 0.00
#&gt; GSM28771     4  0.0000      0.906 0.000 0.000 0.000 1.000 0.000 0.00
#&gt; GSM28760     4  0.0000      0.906 0.000 0.000 0.000 1.000 0.000 0.00
#&gt; GSM28774     2  0.0405      0.924 0.000 0.988 0.000 0.004 0.008 0.00
#&gt; GSM11284     2  0.0405      0.924 0.000 0.988 0.000 0.004 0.008 0.00
#&gt; GSM28761     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.00
#&gt; GSM11278     5  0.0000      0.997 0.000 0.000 0.000 0.000 1.000 0.00
#&gt; GSM11291     3  0.0146      1.000 0.000 0.000 0.996 0.000 0.004 0.00
#&gt; GSM11277     3  0.0146      1.000 0.000 0.000 0.996 0.000 0.004 0.00
#&gt; GSM11272     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.00
#&gt; GSM11285     4  0.2442      0.713 0.000 0.144 0.000 0.852 0.004 0.00
#&gt; GSM28753     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM28773     2  0.1152      0.907 0.000 0.952 0.004 0.000 0.044 0.00
#&gt; GSM28765     2  0.0000      0.928 0.000 1.000 0.000 0.000 0.000 0.00
#&gt; GSM28768     2  0.0291      0.926 0.004 0.992 0.004 0.000 0.000 0.00
#&gt; GSM28754     2  0.0000      0.928 0.000 1.000 0.000 0.000 0.000 0.00
#&gt; GSM28769     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.00
#&gt; GSM11270     5  0.0000      0.997 0.000 0.000 0.000 0.000 1.000 0.00
#&gt; GSM11271     2  0.3892      0.761 0.000 0.752 0.000 0.060 0.188 0.00
#&gt; GSM11288     6  0.3853      0.400 0.000 0.304 0.000 0.016 0.000 0.68
#&gt; GSM11273     5  0.0000      0.997 0.000 0.000 0.000 0.000 1.000 0.00
#&gt; GSM28757     2  0.0146      0.928 0.000 0.996 0.004 0.000 0.000 0.00
#&gt; GSM11282     5  0.0000      0.997 0.000 0.000 0.000 0.000 1.000 0.00
#&gt; GSM28756     2  0.0000      0.928 0.000 1.000 0.000 0.000 0.000 0.00
#&gt; GSM11276     2  0.0000      0.928 0.000 1.000 0.000 0.000 0.000 0.00
#&gt; GSM28752     2  0.0000      0.928 0.000 1.000 0.000 0.000 0.000 0.00
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:hclust 54     0.398 2
#> MAD:hclust 49     0.368 3
#> MAD:hclust 49     0.348 4
#> MAD:hclust 53     0.481 5
#> MAD:hclust 53     0.448 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.399           0.798       0.852         0.3659 0.669   0.669
#> 3 3 1.000           0.983       0.980         0.5290 0.769   0.656
#> 4 4 0.682           0.712       0.795         0.2510 0.811   0.570
#> 5 5 0.656           0.657       0.766         0.0832 0.919   0.722
#> 6 6 0.726           0.679       0.788         0.0651 0.910   0.669
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.1843      0.837 0.028 0.972
#&gt; GSM28763     2  0.1843      0.837 0.028 0.972
#&gt; GSM28764     2  0.1843      0.837 0.028 0.972
#&gt; GSM11274     2  0.8207      0.646 0.256 0.744
#&gt; GSM28772     1  0.8207      1.000 0.744 0.256
#&gt; GSM11269     1  0.8207      1.000 0.744 0.256
#&gt; GSM28775     1  0.8207      1.000 0.744 0.256
#&gt; GSM11293     1  0.8207      1.000 0.744 0.256
#&gt; GSM28755     1  0.8207      1.000 0.744 0.256
#&gt; GSM11279     1  0.8207      1.000 0.744 0.256
#&gt; GSM28758     1  0.8207      1.000 0.744 0.256
#&gt; GSM11281     1  0.8207      1.000 0.744 0.256
#&gt; GSM11287     1  0.8207      1.000 0.744 0.256
#&gt; GSM28759     1  0.8207      1.000 0.744 0.256
#&gt; GSM11292     2  0.0000      0.840 0.000 1.000
#&gt; GSM28766     2  0.0376      0.838 0.004 0.996
#&gt; GSM11268     2  0.9933      0.449 0.452 0.548
#&gt; GSM28767     2  0.0000      0.840 0.000 1.000
#&gt; GSM11286     2  0.1843      0.837 0.028 0.972
#&gt; GSM28751     2  0.1843      0.837 0.028 0.972
#&gt; GSM28770     2  0.0000      0.840 0.000 1.000
#&gt; GSM11283     2  0.1843      0.837 0.028 0.972
#&gt; GSM11289     2  0.0000      0.840 0.000 1.000
#&gt; GSM11280     2  0.1843      0.837 0.028 0.972
#&gt; GSM28749     2  0.0000      0.840 0.000 1.000
#&gt; GSM28750     2  0.9933      0.449 0.452 0.548
#&gt; GSM11290     2  0.9933      0.449 0.452 0.548
#&gt; GSM11294     2  0.9933      0.449 0.452 0.548
#&gt; GSM28771     2  0.0000      0.840 0.000 1.000
#&gt; GSM28760     2  0.4298      0.791 0.088 0.912
#&gt; GSM28774     2  0.1843      0.837 0.028 0.972
#&gt; GSM11284     2  0.0000      0.840 0.000 1.000
#&gt; GSM28761     2  0.9933      0.449 0.452 0.548
#&gt; GSM11278     2  0.3114      0.814 0.056 0.944
#&gt; GSM11291     2  0.9933      0.449 0.452 0.548
#&gt; GSM11277     2  0.9933      0.449 0.452 0.548
#&gt; GSM11272     2  0.9933      0.449 0.452 0.548
#&gt; GSM11285     2  0.0672      0.837 0.008 0.992
#&gt; GSM28753     2  0.1843      0.837 0.028 0.972
#&gt; GSM28773     2  0.0000      0.840 0.000 1.000
#&gt; GSM28765     2  0.1843      0.837 0.028 0.972
#&gt; GSM28768     2  0.5059      0.744 0.112 0.888
#&gt; GSM28754     2  0.1843      0.837 0.028 0.972
#&gt; GSM28769     2  0.1843      0.837 0.028 0.972
#&gt; GSM11275     1  0.8207      1.000 0.744 0.256
#&gt; GSM11270     2  0.3114      0.814 0.056 0.944
#&gt; GSM11271     2  0.0000      0.840 0.000 1.000
#&gt; GSM11288     2  0.7299      0.611 0.204 0.796
#&gt; GSM11273     2  0.8207      0.646 0.256 0.744
#&gt; GSM28757     2  0.1843      0.837 0.028 0.972
#&gt; GSM11282     2  0.3114      0.814 0.056 0.944
#&gt; GSM28756     2  0.1843      0.837 0.028 0.972
#&gt; GSM11276     2  0.1843      0.837 0.028 0.972
#&gt; GSM28752     2  0.1843      0.837 0.028 0.972
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11274     3  0.0424      0.983 0.000 0.008 0.992
#&gt; GSM28772     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM11269     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM28775     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM11293     1  0.1525      0.998 0.964 0.032 0.004
#&gt; GSM28755     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM11279     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM28758     1  0.1525      0.998 0.964 0.032 0.004
#&gt; GSM11281     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM11287     1  0.1289      0.999 0.968 0.032 0.000
#&gt; GSM28759     1  0.1525      0.998 0.964 0.032 0.004
#&gt; GSM11292     2  0.0592      0.984 0.000 0.988 0.012
#&gt; GSM28766     2  0.0592      0.984 0.000 0.988 0.012
#&gt; GSM11268     3  0.1711      0.988 0.032 0.008 0.960
#&gt; GSM28767     2  0.0592      0.984 0.000 0.988 0.012
#&gt; GSM11286     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28751     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28770     2  0.0592      0.984 0.000 0.988 0.012
#&gt; GSM11283     2  0.0829      0.978 0.012 0.984 0.004
#&gt; GSM11289     2  0.0592      0.984 0.000 0.988 0.012
#&gt; GSM11280     2  0.0237      0.985 0.000 0.996 0.004
#&gt; GSM28749     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28750     3  0.1711      0.988 0.032 0.008 0.960
#&gt; GSM11290     3  0.1015      0.990 0.012 0.008 0.980
#&gt; GSM11294     3  0.1015      0.990 0.012 0.008 0.980
#&gt; GSM28771     2  0.0829      0.978 0.012 0.984 0.004
#&gt; GSM28760     2  0.1999      0.961 0.012 0.952 0.036
#&gt; GSM28774     2  0.0237      0.986 0.000 0.996 0.004
#&gt; GSM11284     2  0.0237      0.986 0.000 0.996 0.004
#&gt; GSM28761     3  0.1711      0.988 0.032 0.008 0.960
#&gt; GSM11278     2  0.0892      0.980 0.000 0.980 0.020
#&gt; GSM11291     3  0.1015      0.990 0.012 0.008 0.980
#&gt; GSM11277     3  0.1015      0.990 0.012 0.008 0.980
#&gt; GSM11272     3  0.1711      0.988 0.032 0.008 0.960
#&gt; GSM11285     2  0.1337      0.976 0.012 0.972 0.016
#&gt; GSM28753     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28768     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28754     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28769     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11275     1  0.1525      0.998 0.964 0.032 0.004
#&gt; GSM11270     2  0.0892      0.980 0.000 0.980 0.020
#&gt; GSM11271     2  0.0592      0.984 0.000 0.988 0.012
#&gt; GSM11288     2  0.5122      0.736 0.012 0.788 0.200
#&gt; GSM11273     3  0.0424      0.983 0.000 0.008 0.992
#&gt; GSM28757     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11282     2  0.0892      0.980 0.000 0.980 0.020
#&gt; GSM28756     2  0.0237      0.986 0.000 0.996 0.004
#&gt; GSM11276     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.987 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     4  0.4790     0.7701 0.000 0.380 0.000 0.620
#&gt; GSM28763     4  0.4790     0.7701 0.000 0.380 0.000 0.620
#&gt; GSM28764     2  0.4500     0.3237 0.000 0.684 0.000 0.316
#&gt; GSM11274     3  0.2647     0.8298 0.000 0.120 0.880 0.000
#&gt; GSM28772     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0707     0.9884 0.980 0.000 0.000 0.020
#&gt; GSM28755     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0817     0.9869 0.976 0.000 0.000 0.024
#&gt; GSM11281     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9933 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0707     0.9884 0.980 0.000 0.000 0.020
#&gt; GSM11292     2  0.2149     0.7177 0.000 0.912 0.000 0.088
#&gt; GSM28766     2  0.2149     0.7177 0.000 0.912 0.000 0.088
#&gt; GSM11268     3  0.3486     0.8701 0.000 0.000 0.812 0.188
#&gt; GSM28767     2  0.2149     0.7177 0.000 0.912 0.000 0.088
#&gt; GSM11286     4  0.4804     0.7647 0.000 0.384 0.000 0.616
#&gt; GSM28751     4  0.4790     0.7701 0.000 0.380 0.000 0.620
#&gt; GSM28770     2  0.2149     0.7177 0.000 0.912 0.000 0.088
#&gt; GSM11283     4  0.4605     0.5233 0.000 0.336 0.000 0.664
#&gt; GSM11289     2  0.2149     0.7177 0.000 0.912 0.000 0.088
#&gt; GSM11280     4  0.4304     0.7448 0.000 0.284 0.000 0.716
#&gt; GSM28749     4  0.4008     0.7170 0.000 0.244 0.000 0.756
#&gt; GSM28750     3  0.3486     0.8701 0.000 0.000 0.812 0.188
#&gt; GSM11290     3  0.0000     0.8857 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000     0.8857 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.4643     0.5107 0.000 0.344 0.000 0.656
#&gt; GSM28760     2  0.5039     0.0690 0.000 0.592 0.004 0.404
#&gt; GSM28774     2  0.3801     0.5668 0.000 0.780 0.000 0.220
#&gt; GSM11284     2  0.3172     0.6833 0.000 0.840 0.000 0.160
#&gt; GSM28761     3  0.3486     0.8701 0.000 0.000 0.812 0.188
#&gt; GSM11278     2  0.0376     0.6667 0.000 0.992 0.004 0.004
#&gt; GSM11291     3  0.0000     0.8857 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000     0.8857 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.3486     0.8701 0.000 0.000 0.812 0.188
#&gt; GSM11285     2  0.3400     0.4985 0.000 0.820 0.000 0.180
#&gt; GSM28753     4  0.4454     0.7584 0.000 0.308 0.000 0.692
#&gt; GSM28773     4  0.4222     0.7391 0.000 0.272 0.000 0.728
#&gt; GSM28765     4  0.4996     0.4901 0.000 0.484 0.000 0.516
#&gt; GSM28768     4  0.4776     0.7698 0.000 0.376 0.000 0.624
#&gt; GSM28754     2  0.4941    -0.2585 0.000 0.564 0.000 0.436
#&gt; GSM28769     4  0.4790     0.7701 0.000 0.380 0.000 0.620
#&gt; GSM11275     1  0.0817     0.9869 0.976 0.000 0.000 0.024
#&gt; GSM11270     2  0.0376     0.6667 0.000 0.992 0.004 0.004
#&gt; GSM11271     2  0.2149     0.7177 0.000 0.912 0.000 0.088
#&gt; GSM11288     4  0.4888     0.3704 0.000 0.096 0.124 0.780
#&gt; GSM11273     3  0.4624     0.6031 0.000 0.340 0.660 0.000
#&gt; GSM28757     4  0.4817     0.7634 0.000 0.388 0.000 0.612
#&gt; GSM11282     2  0.0376     0.6667 0.000 0.992 0.004 0.004
#&gt; GSM28756     2  0.3837     0.5595 0.000 0.776 0.000 0.224
#&gt; GSM11276     2  0.4790     0.0920 0.000 0.620 0.000 0.380
#&gt; GSM28752     2  0.4843     0.0132 0.000 0.604 0.000 0.396
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3 p4    p5
#&gt; GSM28762     2   0.112     0.7014 0.000 0.964 0.000 NA 0.016
#&gt; GSM28763     2   0.112     0.7014 0.000 0.964 0.000 NA 0.016
#&gt; GSM28764     2   0.464    -0.2226 0.000 0.528 0.000 NA 0.460
#&gt; GSM11274     3   0.410     0.6808 0.000 0.000 0.788 NA 0.124
#&gt; GSM28772     1   0.000     0.9701 1.000 0.000 0.000 NA 0.000
#&gt; GSM11269     1   0.000     0.9701 1.000 0.000 0.000 NA 0.000
#&gt; GSM28775     1   0.051     0.9654 0.984 0.000 0.000 NA 0.000
#&gt; GSM11293     1   0.128     0.9587 0.956 0.000 0.000 NA 0.012
#&gt; GSM28755     1   0.051     0.9654 0.984 0.000 0.000 NA 0.000
#&gt; GSM11279     1   0.000     0.9701 1.000 0.000 0.000 NA 0.000
#&gt; GSM28758     1   0.285     0.9122 0.872 0.000 0.000 NA 0.036
#&gt; GSM11281     1   0.000     0.9701 1.000 0.000 0.000 NA 0.000
#&gt; GSM11287     1   0.000     0.9701 1.000 0.000 0.000 NA 0.000
#&gt; GSM28759     1   0.128     0.9587 0.956 0.000 0.000 NA 0.012
#&gt; GSM11292     5   0.356     0.7284 0.000 0.260 0.000 NA 0.740
#&gt; GSM28766     5   0.356     0.7284 0.000 0.260 0.000 NA 0.740
#&gt; GSM11268     3   0.429     0.7309 0.000 0.000 0.540 NA 0.000
#&gt; GSM28767     5   0.356     0.7284 0.000 0.260 0.000 NA 0.740
#&gt; GSM11286     2   0.194     0.6798 0.000 0.920 0.000 NA 0.068
#&gt; GSM28751     2   0.140     0.7003 0.000 0.952 0.000 NA 0.024
#&gt; GSM28770     5   0.356     0.7284 0.000 0.260 0.000 NA 0.740
#&gt; GSM11283     2   0.626     0.3324 0.000 0.512 0.000 NA 0.168
#&gt; GSM11289     5   0.356     0.7284 0.000 0.260 0.000 NA 0.740
#&gt; GSM11280     2   0.257     0.6657 0.000 0.880 0.000 NA 0.016
#&gt; GSM28749     2   0.311     0.6540 0.000 0.840 0.000 NA 0.020
#&gt; GSM28750     3   0.429     0.7309 0.000 0.000 0.540 NA 0.000
#&gt; GSM11290     3   0.000     0.7764 0.000 0.000 1.000 NA 0.000
#&gt; GSM11294     3   0.000     0.7764 0.000 0.000 1.000 NA 0.000
#&gt; GSM28771     2   0.634     0.3149 0.000 0.500 0.000 NA 0.180
#&gt; GSM28760     5   0.674     0.0480 0.000 0.256 0.000 NA 0.380
#&gt; GSM28774     5   0.514     0.4341 0.000 0.424 0.000 NA 0.536
#&gt; GSM11284     5   0.535     0.6333 0.000 0.280 0.000 NA 0.632
#&gt; GSM28761     3   0.429     0.7309 0.000 0.000 0.540 NA 0.000
#&gt; GSM11278     5   0.468     0.6484 0.000 0.140 0.000 NA 0.740
#&gt; GSM11291     3   0.000     0.7764 0.000 0.000 1.000 NA 0.000
#&gt; GSM11277     3   0.000     0.7764 0.000 0.000 1.000 NA 0.000
#&gt; GSM11272     3   0.429     0.7309 0.000 0.000 0.540 NA 0.000
#&gt; GSM11285     5   0.484     0.4655 0.000 0.084 0.000 NA 0.708
#&gt; GSM28753     2   0.112     0.6941 0.000 0.960 0.000 NA 0.004
#&gt; GSM28773     2   0.297     0.6574 0.000 0.852 0.000 NA 0.020
#&gt; GSM28765     2   0.258     0.6193 0.000 0.864 0.000 NA 0.132
#&gt; GSM28768     2   0.284     0.6723 0.000 0.876 0.000 NA 0.048
#&gt; GSM28754     2   0.450     0.4024 0.000 0.704 0.000 NA 0.256
#&gt; GSM28769     2   0.140     0.7003 0.000 0.952 0.000 NA 0.024
#&gt; GSM11275     1   0.285     0.9122 0.872 0.000 0.000 NA 0.036
#&gt; GSM11270     5   0.468     0.6484 0.000 0.140 0.000 NA 0.740
#&gt; GSM11271     5   0.356     0.7284 0.000 0.260 0.000 NA 0.740
#&gt; GSM11288     2   0.555     0.1817 0.000 0.496 0.068 NA 0.000
#&gt; GSM11273     3   0.607     0.4117 0.000 0.008 0.560 NA 0.316
#&gt; GSM28757     2   0.271     0.6664 0.000 0.880 0.000 NA 0.088
#&gt; GSM11282     5   0.455     0.6509 0.000 0.132 0.000 NA 0.752
#&gt; GSM28756     5   0.515     0.4061 0.000 0.436 0.000 NA 0.524
#&gt; GSM11276     2   0.447     0.0912 0.000 0.616 0.000 NA 0.372
#&gt; GSM28752     2   0.411     0.3423 0.000 0.700 0.000 NA 0.288
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.1737      0.714 0.000 0.932 0.020 0.008 0.040 0.000
#&gt; GSM28763     2  0.1737      0.714 0.000 0.932 0.020 0.008 0.040 0.000
#&gt; GSM28764     5  0.4193      0.532 0.000 0.272 0.044 0.000 0.684 0.000
#&gt; GSM11274     3  0.5512      0.552 0.000 0.000 0.604 0.084 0.036 0.276
#&gt; GSM28772     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0725      0.935 0.976 0.000 0.012 0.012 0.000 0.000
#&gt; GSM11293     1  0.1864      0.919 0.924 0.000 0.032 0.040 0.004 0.000
#&gt; GSM28755     1  0.0725      0.935 0.976 0.000 0.012 0.012 0.000 0.000
#&gt; GSM11279     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.4414      0.805 0.752 0.012 0.072 0.156 0.004 0.004
#&gt; GSM11281     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.941 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.1864      0.919 0.924 0.000 0.032 0.040 0.004 0.000
#&gt; GSM11292     5  0.1204      0.700 0.000 0.056 0.000 0.000 0.944 0.000
#&gt; GSM28766     5  0.1204      0.700 0.000 0.056 0.000 0.000 0.944 0.000
#&gt; GSM11268     6  0.0146      0.997 0.000 0.004 0.000 0.000 0.000 0.996
#&gt; GSM28767     5  0.1204      0.700 0.000 0.056 0.000 0.000 0.944 0.000
#&gt; GSM11286     2  0.3306      0.700 0.000 0.848 0.044 0.052 0.056 0.000
#&gt; GSM28751     2  0.2415      0.708 0.000 0.900 0.040 0.024 0.036 0.000
#&gt; GSM28770     5  0.1204      0.700 0.000 0.056 0.000 0.000 0.944 0.000
#&gt; GSM11283     4  0.4227      0.862 0.000 0.256 0.000 0.692 0.052 0.000
#&gt; GSM11289     5  0.1204      0.700 0.000 0.056 0.000 0.000 0.944 0.000
#&gt; GSM11280     2  0.4037      0.574 0.000 0.752 0.028 0.196 0.024 0.000
#&gt; GSM28749     2  0.4723      0.571 0.000 0.732 0.028 0.180 0.024 0.036
#&gt; GSM28750     6  0.0146      0.992 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; GSM11290     3  0.3789      0.714 0.000 0.000 0.584 0.000 0.000 0.416
#&gt; GSM11294     3  0.3789      0.714 0.000 0.000 0.584 0.000 0.000 0.416
#&gt; GSM28771     4  0.4261      0.867 0.000 0.252 0.000 0.692 0.056 0.000
#&gt; GSM28760     4  0.5097      0.766 0.000 0.124 0.044 0.700 0.132 0.000
#&gt; GSM28774     5  0.6304      0.567 0.000 0.200 0.120 0.104 0.576 0.000
#&gt; GSM11284     5  0.5630      0.584 0.000 0.084 0.084 0.184 0.648 0.000
#&gt; GSM28761     6  0.0146      0.997 0.000 0.004 0.000 0.000 0.000 0.996
#&gt; GSM11278     5  0.5648      0.513 0.000 0.020 0.232 0.152 0.596 0.000
#&gt; GSM11291     3  0.3789      0.714 0.000 0.000 0.584 0.000 0.000 0.416
#&gt; GSM11277     3  0.3789      0.714 0.000 0.000 0.584 0.000 0.000 0.416
#&gt; GSM11272     6  0.0146      0.997 0.000 0.004 0.000 0.000 0.000 0.996
#&gt; GSM11285     5  0.3971     -0.104 0.000 0.004 0.000 0.448 0.548 0.000
#&gt; GSM28753     2  0.2668      0.694 0.000 0.884 0.028 0.060 0.028 0.000
#&gt; GSM28773     2  0.4924      0.550 0.000 0.716 0.036 0.188 0.024 0.036
#&gt; GSM28765     2  0.3918      0.668 0.000 0.800 0.044 0.048 0.108 0.000
#&gt; GSM28768     2  0.3916      0.605 0.000 0.792 0.056 0.132 0.016 0.004
#&gt; GSM28754     2  0.6726      0.129 0.000 0.488 0.128 0.104 0.280 0.000
#&gt; GSM28769     2  0.2342      0.708 0.000 0.904 0.040 0.024 0.032 0.000
#&gt; GSM11275     1  0.4414      0.805 0.752 0.012 0.072 0.156 0.004 0.004
#&gt; GSM11270     5  0.5648      0.513 0.000 0.020 0.232 0.152 0.596 0.000
#&gt; GSM11271     5  0.1411      0.700 0.000 0.060 0.000 0.004 0.936 0.000
#&gt; GSM11288     2  0.5969      0.219 0.000 0.536 0.032 0.128 0.000 0.304
#&gt; GSM11273     3  0.5792      0.337 0.000 0.000 0.632 0.108 0.184 0.076
#&gt; GSM28757     2  0.4065      0.676 0.000 0.796 0.076 0.072 0.056 0.000
#&gt; GSM11282     5  0.5564      0.519 0.000 0.020 0.228 0.144 0.608 0.000
#&gt; GSM28756     5  0.6540      0.522 0.000 0.232 0.128 0.104 0.536 0.000
#&gt; GSM11276     5  0.5238      0.108 0.000 0.460 0.044 0.024 0.472 0.000
#&gt; GSM28752     2  0.5210      0.244 0.000 0.576 0.040 0.036 0.348 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:kmeans 46     0.389 2
#> MAD:kmeans 54     0.374 3
#> MAD:kmeans 46     0.428 4
#> MAD:kmeans 42     0.408 5
#> MAD:kmeans 48     0.485 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.969       0.982         0.4865 0.516   0.516
#> 3 3 0.941           0.919       0.968         0.3033 0.764   0.576
#> 4 4 0.858           0.841       0.931         0.1905 0.834   0.569
#> 5 5 0.743           0.670       0.820         0.0659 0.915   0.673
#> 6 6 0.770           0.609       0.784         0.0398 0.930   0.671
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2   0.242      0.956 0.040 0.960
#&gt; GSM28763     2   0.242      0.956 0.040 0.960
#&gt; GSM28764     2   0.000      0.981 0.000 1.000
#&gt; GSM11274     2   0.000      0.981 0.000 1.000
#&gt; GSM28772     1   0.000      0.981 1.000 0.000
#&gt; GSM11269     1   0.000      0.981 1.000 0.000
#&gt; GSM28775     1   0.000      0.981 1.000 0.000
#&gt; GSM11293     1   0.000      0.981 1.000 0.000
#&gt; GSM28755     1   0.000      0.981 1.000 0.000
#&gt; GSM11279     1   0.000      0.981 1.000 0.000
#&gt; GSM28758     1   0.000      0.981 1.000 0.000
#&gt; GSM11281     1   0.000      0.981 1.000 0.000
#&gt; GSM11287     1   0.000      0.981 1.000 0.000
#&gt; GSM28759     1   0.000      0.981 1.000 0.000
#&gt; GSM11292     2   0.000      0.981 0.000 1.000
#&gt; GSM28766     2   0.000      0.981 0.000 1.000
#&gt; GSM11268     1   0.242      0.972 0.960 0.040
#&gt; GSM28767     2   0.000      0.981 0.000 1.000
#&gt; GSM11286     2   0.204      0.962 0.032 0.968
#&gt; GSM28751     2   0.730      0.774 0.204 0.796
#&gt; GSM28770     2   0.000      0.981 0.000 1.000
#&gt; GSM11283     2   0.000      0.981 0.000 1.000
#&gt; GSM11289     2   0.000      0.981 0.000 1.000
#&gt; GSM11280     2   0.000      0.981 0.000 1.000
#&gt; GSM28749     2   0.000      0.981 0.000 1.000
#&gt; GSM28750     1   0.242      0.972 0.960 0.040
#&gt; GSM11290     1   0.242      0.972 0.960 0.040
#&gt; GSM11294     1   0.242      0.972 0.960 0.040
#&gt; GSM28771     2   0.000      0.981 0.000 1.000
#&gt; GSM28760     2   0.000      0.981 0.000 1.000
#&gt; GSM28774     2   0.000      0.981 0.000 1.000
#&gt; GSM11284     2   0.000      0.981 0.000 1.000
#&gt; GSM28761     1   0.242      0.972 0.960 0.040
#&gt; GSM11278     2   0.000      0.981 0.000 1.000
#&gt; GSM11291     1   0.242      0.972 0.960 0.040
#&gt; GSM11277     1   0.242      0.972 0.960 0.040
#&gt; GSM11272     1   0.000      0.981 1.000 0.000
#&gt; GSM11285     2   0.000      0.981 0.000 1.000
#&gt; GSM28753     2   0.204      0.962 0.032 0.968
#&gt; GSM28773     2   0.000      0.981 0.000 1.000
#&gt; GSM28765     2   0.204      0.962 0.032 0.968
#&gt; GSM28768     1   0.260      0.951 0.956 0.044
#&gt; GSM28754     2   0.000      0.981 0.000 1.000
#&gt; GSM28769     2   0.730      0.774 0.204 0.796
#&gt; GSM11275     1   0.000      0.981 1.000 0.000
#&gt; GSM11270     2   0.000      0.981 0.000 1.000
#&gt; GSM11271     2   0.000      0.981 0.000 1.000
#&gt; GSM11288     1   0.242      0.972 0.960 0.040
#&gt; GSM11273     2   0.000      0.981 0.000 1.000
#&gt; GSM28757     2   0.000      0.981 0.000 1.000
#&gt; GSM11282     2   0.000      0.981 0.000 1.000
#&gt; GSM28756     2   0.000      0.981 0.000 1.000
#&gt; GSM11276     2   0.000      0.981 0.000 1.000
#&gt; GSM28752     2   0.242      0.956 0.040 0.960
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28751     1  0.5465      0.616 0.712 0.288 0.000
#&gt; GSM28770     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28749     2  0.5098      0.662 0.000 0.752 0.248
#&gt; GSM28750     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28771     3  0.6168      0.287 0.000 0.412 0.588
#&gt; GSM28760     3  0.0424      0.946 0.000 0.008 0.992
#&gt; GSM28774     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11278     2  0.1163      0.949 0.000 0.972 0.028
#&gt; GSM11291     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM11285     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28773     2  0.5905      0.456 0.000 0.648 0.352
#&gt; GSM28765     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28768     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM28754     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28769     1  0.5098      0.683 0.752 0.248 0.000
#&gt; GSM11275     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM11270     2  0.2066      0.918 0.000 0.940 0.060
#&gt; GSM11271     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11288     3  0.1643      0.914 0.044 0.000 0.956
#&gt; GSM11273     3  0.0000      0.953 0.000 0.000 1.000
#&gt; GSM28757     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11282     2  0.1031      0.952 0.000 0.976 0.024
#&gt; GSM28756     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.971 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     4  0.0188      0.846 0.000 0.004 0.000 0.996
#&gt; GSM28763     4  0.0188      0.846 0.000 0.004 0.000 0.996
#&gt; GSM28764     2  0.2149      0.846 0.000 0.912 0.000 0.088
#&gt; GSM11274     3  0.0188      0.947 0.000 0.000 0.996 0.004
#&gt; GSM28772     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.893 0.000 1.000 0.000 0.000
#&gt; GSM28766     2  0.0000      0.893 0.000 1.000 0.000 0.000
#&gt; GSM11268     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM28767     2  0.0000      0.893 0.000 1.000 0.000 0.000
#&gt; GSM11286     4  0.4477      0.506 0.000 0.312 0.000 0.688
#&gt; GSM28751     4  0.0336      0.845 0.000 0.008 0.000 0.992
#&gt; GSM28770     2  0.0000      0.893 0.000 1.000 0.000 0.000
#&gt; GSM11283     4  0.1022      0.832 0.000 0.032 0.000 0.968
#&gt; GSM11289     2  0.0000      0.893 0.000 1.000 0.000 0.000
#&gt; GSM11280     4  0.0188      0.846 0.000 0.004 0.000 0.996
#&gt; GSM28749     4  0.4214      0.658 0.000 0.016 0.204 0.780
#&gt; GSM28750     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11290     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.2589      0.777 0.000 0.116 0.000 0.884
#&gt; GSM28760     3  0.7264      0.117 0.000 0.148 0.460 0.392
#&gt; GSM28774     2  0.2216      0.843 0.000 0.908 0.000 0.092
#&gt; GSM11284     2  0.0188      0.892 0.000 0.996 0.000 0.004
#&gt; GSM28761     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11278     2  0.0336      0.891 0.000 0.992 0.000 0.008
#&gt; GSM11291     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11285     2  0.0336      0.891 0.000 0.992 0.000 0.008
#&gt; GSM28753     4  0.0188      0.846 0.000 0.004 0.000 0.996
#&gt; GSM28773     4  0.4225      0.686 0.000 0.024 0.184 0.792
#&gt; GSM28765     4  0.4746      0.367 0.000 0.368 0.000 0.632
#&gt; GSM28768     1  0.1716      0.930 0.936 0.000 0.000 0.064
#&gt; GSM28754     2  0.4925      0.275 0.000 0.572 0.000 0.428
#&gt; GSM28769     4  0.0336      0.845 0.000 0.008 0.000 0.992
#&gt; GSM11275     1  0.0000      0.994 1.000 0.000 0.000 0.000
#&gt; GSM11270     2  0.0336      0.891 0.000 0.992 0.000 0.008
#&gt; GSM11271     2  0.0000      0.893 0.000 1.000 0.000 0.000
#&gt; GSM11288     3  0.1209      0.920 0.032 0.000 0.964 0.004
#&gt; GSM11273     3  0.0376      0.944 0.000 0.004 0.992 0.004
#&gt; GSM28757     4  0.4331      0.548 0.000 0.288 0.000 0.712
#&gt; GSM11282     2  0.0336      0.891 0.000 0.992 0.000 0.008
#&gt; GSM28756     2  0.2814      0.810 0.000 0.868 0.000 0.132
#&gt; GSM11276     2  0.4564      0.542 0.000 0.672 0.000 0.328
#&gt; GSM28752     2  0.4776      0.437 0.000 0.624 0.000 0.376
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0451     0.4812 0.000 0.988 0.000 0.008 0.004
#&gt; GSM28763     2  0.0451     0.4812 0.000 0.988 0.000 0.008 0.004
#&gt; GSM28764     5  0.3495     0.6042 0.000 0.160 0.000 0.028 0.812
#&gt; GSM11274     3  0.2230     0.8265 0.000 0.000 0.884 0.116 0.000
#&gt; GSM28772     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000     0.7765 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28766     5  0.0000     0.7765 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11268     3  0.2074     0.8990 0.000 0.000 0.896 0.104 0.000
#&gt; GSM28767     5  0.0000     0.7765 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11286     2  0.5538     0.4765 0.000 0.644 0.000 0.212 0.144
#&gt; GSM28751     2  0.0898     0.4903 0.008 0.972 0.000 0.000 0.020
#&gt; GSM28770     5  0.0000     0.7765 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11283     4  0.4415     0.4819 0.000 0.444 0.000 0.552 0.004
#&gt; GSM11289     5  0.0000     0.7765 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11280     4  0.4306     0.3389 0.000 0.492 0.000 0.508 0.000
#&gt; GSM28749     4  0.6127     0.4114 0.000 0.292 0.040 0.596 0.072
#&gt; GSM28750     3  0.2074     0.8990 0.000 0.000 0.896 0.104 0.000
#&gt; GSM11290     3  0.0000     0.9061 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000     0.9061 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28771     4  0.4397     0.4862 0.000 0.432 0.000 0.564 0.004
#&gt; GSM28760     4  0.6719     0.4325 0.000 0.224 0.184 0.560 0.032
#&gt; GSM28774     5  0.5408     0.5996 0.000 0.120 0.000 0.228 0.652
#&gt; GSM11284     5  0.4269     0.6143 0.000 0.016 0.000 0.300 0.684
#&gt; GSM28761     3  0.2074     0.8990 0.000 0.000 0.896 0.104 0.000
#&gt; GSM11278     5  0.5836     0.5734 0.000 0.004 0.104 0.316 0.576
#&gt; GSM11291     3  0.0000     0.9061 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000     0.9061 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.2074     0.8990 0.000 0.000 0.896 0.104 0.000
#&gt; GSM11285     5  0.2890     0.6795 0.000 0.004 0.000 0.160 0.836
#&gt; GSM28753     2  0.3895    -0.0884 0.000 0.680 0.000 0.320 0.000
#&gt; GSM28773     4  0.5430     0.3456 0.000 0.372 0.032 0.576 0.020
#&gt; GSM28765     2  0.5844     0.4653 0.000 0.608 0.000 0.208 0.184
#&gt; GSM28768     1  0.4252     0.5791 0.700 0.280 0.000 0.020 0.000
#&gt; GSM28754     4  0.6818    -0.2610 0.000 0.336 0.000 0.352 0.312
#&gt; GSM28769     2  0.0671     0.4914 0.004 0.980 0.000 0.000 0.016
#&gt; GSM11275     1  0.0000     0.9706 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.5836     0.5734 0.000 0.004 0.104 0.316 0.576
#&gt; GSM11271     5  0.0000     0.7765 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11288     3  0.3394     0.8537 0.020 0.004 0.824 0.152 0.000
#&gt; GSM11273     3  0.3123     0.7562 0.000 0.000 0.812 0.184 0.004
#&gt; GSM28757     2  0.5297     0.3493 0.000 0.580 0.000 0.360 0.060
#&gt; GSM11282     5  0.5351     0.6109 0.000 0.004 0.068 0.304 0.624
#&gt; GSM28756     5  0.6203     0.4202 0.000 0.188 0.000 0.268 0.544
#&gt; GSM11276     2  0.5406     0.2008 0.000 0.480 0.000 0.056 0.464
#&gt; GSM28752     2  0.5401     0.3483 0.000 0.536 0.000 0.060 0.404
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.1434     0.5917 0.000 0.940 0.000 0.048 0.000 0.012
#&gt; GSM28763     2  0.1434     0.5917 0.000 0.940 0.000 0.048 0.000 0.012
#&gt; GSM28764     5  0.2197     0.7298 0.000 0.056 0.000 0.000 0.900 0.044
#&gt; GSM11274     3  0.5150     0.5268 0.000 0.000 0.608 0.136 0.000 0.256
#&gt; GSM28772     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000     0.7976 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28766     5  0.0000     0.7976 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11268     3  0.2170     0.7879 0.000 0.000 0.888 0.100 0.000 0.012
#&gt; GSM28767     5  0.0000     0.7976 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11286     6  0.6453    -0.1931 0.000 0.396 0.000 0.076 0.100 0.428
#&gt; GSM28751     2  0.0405     0.6003 0.000 0.988 0.000 0.008 0.004 0.000
#&gt; GSM28770     5  0.0000     0.7976 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11283     4  0.4091     0.6151 0.000 0.212 0.000 0.732 0.004 0.052
#&gt; GSM11289     5  0.0000     0.7976 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11280     4  0.5438     0.5154 0.000 0.172 0.000 0.568 0.000 0.260
#&gt; GSM28749     4  0.5873     0.5117 0.000 0.100 0.044 0.620 0.012 0.224
#&gt; GSM28750     3  0.1663     0.7925 0.000 0.000 0.912 0.088 0.000 0.000
#&gt; GSM11290     3  0.1531     0.8041 0.000 0.000 0.928 0.004 0.000 0.068
#&gt; GSM11294     3  0.1812     0.8025 0.000 0.000 0.912 0.008 0.000 0.080
#&gt; GSM28771     4  0.3839     0.6113 0.000 0.212 0.000 0.748 0.004 0.036
#&gt; GSM28760     4  0.4020     0.5608 0.000 0.080 0.072 0.808 0.020 0.020
#&gt; GSM28774     6  0.5319     0.3068 0.000 0.072 0.000 0.016 0.364 0.548
#&gt; GSM11284     5  0.6361    -0.0993 0.000 0.012 0.000 0.280 0.396 0.312
#&gt; GSM28761     3  0.2170     0.7879 0.000 0.000 0.888 0.100 0.000 0.012
#&gt; GSM11278     6  0.6109     0.4122 0.000 0.000 0.080 0.152 0.168 0.600
#&gt; GSM11291     3  0.1812     0.8025 0.000 0.000 0.912 0.008 0.000 0.080
#&gt; GSM11277     3  0.1812     0.8025 0.000 0.000 0.912 0.008 0.000 0.080
#&gt; GSM11272     3  0.2170     0.7879 0.000 0.000 0.888 0.100 0.000 0.012
#&gt; GSM11285     5  0.3309     0.5367 0.000 0.000 0.000 0.280 0.720 0.000
#&gt; GSM28753     2  0.5779    -0.1778 0.000 0.452 0.000 0.368 0.000 0.180
#&gt; GSM28773     4  0.6873     0.3741 0.000 0.248 0.044 0.464 0.012 0.232
#&gt; GSM28765     2  0.6358    -0.0209 0.000 0.424 0.000 0.048 0.128 0.400
#&gt; GSM28768     1  0.4767     0.4260 0.616 0.332 0.000 0.024 0.000 0.028
#&gt; GSM28754     6  0.4808     0.4081 0.000 0.156 0.000 0.024 0.108 0.712
#&gt; GSM28769     2  0.0508     0.5999 0.000 0.984 0.000 0.012 0.004 0.000
#&gt; GSM11275     1  0.0000     0.9640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     6  0.6168     0.4069 0.000 0.000 0.088 0.152 0.164 0.596
#&gt; GSM11271     5  0.0260     0.7932 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM11288     3  0.4249     0.7006 0.044 0.012 0.768 0.156 0.000 0.020
#&gt; GSM11273     3  0.5667     0.3198 0.000 0.000 0.488 0.140 0.004 0.368
#&gt; GSM28757     6  0.4974     0.3249 0.000 0.200 0.000 0.048 0.060 0.692
#&gt; GSM11282     6  0.6098     0.4139 0.000 0.000 0.056 0.152 0.212 0.580
#&gt; GSM28756     6  0.4941     0.4427 0.000 0.104 0.000 0.008 0.228 0.660
#&gt; GSM11276     5  0.5509     0.0719 0.000 0.364 0.000 0.004 0.512 0.120
#&gt; GSM28752     2  0.5387     0.0503 0.000 0.500 0.000 0.008 0.404 0.088
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> MAD:skmeans 54     0.398 2
#> MAD:skmeans 52     0.372 3
#> MAD:skmeans 50     0.432 4
#> MAD:skmeans 36     0.411 5
#> MAD:skmeans 38     0.442 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.955       0.982          0.352 0.669   0.669
#> 3 3 1.000           0.968       0.986          0.574 0.786   0.681
#> 4 4 0.768           0.794       0.834          0.205 0.816   0.595
#> 5 5 0.755           0.742       0.877          0.140 0.913   0.703
#> 6 6 0.752           0.573       0.757          0.058 0.874   0.525
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2   0.000      0.976 0.000 1.000
#&gt; GSM28763     2   0.000      0.976 0.000 1.000
#&gt; GSM28764     2   0.000      0.976 0.000 1.000
#&gt; GSM11274     2   0.000      0.976 0.000 1.000
#&gt; GSM28772     1   0.000      1.000 1.000 0.000
#&gt; GSM11269     1   0.000      1.000 1.000 0.000
#&gt; GSM28775     1   0.000      1.000 1.000 0.000
#&gt; GSM11293     1   0.000      1.000 1.000 0.000
#&gt; GSM28755     1   0.000      1.000 1.000 0.000
#&gt; GSM11279     1   0.000      1.000 1.000 0.000
#&gt; GSM28758     1   0.000      1.000 1.000 0.000
#&gt; GSM11281     1   0.000      1.000 1.000 0.000
#&gt; GSM11287     1   0.000      1.000 1.000 0.000
#&gt; GSM28759     1   0.000      1.000 1.000 0.000
#&gt; GSM11292     2   0.000      0.976 0.000 1.000
#&gt; GSM28766     2   0.000      0.976 0.000 1.000
#&gt; GSM11268     2   0.000      0.976 0.000 1.000
#&gt; GSM28767     2   0.000      0.976 0.000 1.000
#&gt; GSM11286     2   0.000      0.976 0.000 1.000
#&gt; GSM28751     2   0.706      0.760 0.192 0.808
#&gt; GSM28770     2   0.000      0.976 0.000 1.000
#&gt; GSM11283     2   0.000      0.976 0.000 1.000
#&gt; GSM11289     2   0.000      0.976 0.000 1.000
#&gt; GSM11280     2   0.000      0.976 0.000 1.000
#&gt; GSM28749     2   0.000      0.976 0.000 1.000
#&gt; GSM28750     2   0.000      0.976 0.000 1.000
#&gt; GSM11290     2   0.204      0.947 0.032 0.968
#&gt; GSM11294     2   0.000      0.976 0.000 1.000
#&gt; GSM28771     2   0.000      0.976 0.000 1.000
#&gt; GSM28760     2   0.000      0.976 0.000 1.000
#&gt; GSM28774     2   0.000      0.976 0.000 1.000
#&gt; GSM11284     2   0.000      0.976 0.000 1.000
#&gt; GSM28761     2   0.000      0.976 0.000 1.000
#&gt; GSM11278     2   0.000      0.976 0.000 1.000
#&gt; GSM11291     2   0.000      0.976 0.000 1.000
#&gt; GSM11277     2   0.000      0.976 0.000 1.000
#&gt; GSM11272     2   0.993      0.212 0.452 0.548
#&gt; GSM11285     2   0.000      0.976 0.000 1.000
#&gt; GSM28753     2   0.000      0.976 0.000 1.000
#&gt; GSM28773     2   0.000      0.976 0.000 1.000
#&gt; GSM28765     2   0.000      0.976 0.000 1.000
#&gt; GSM28768     2   0.900      0.555 0.316 0.684
#&gt; GSM28754     2   0.000      0.976 0.000 1.000
#&gt; GSM28769     2   0.000      0.976 0.000 1.000
#&gt; GSM11275     1   0.000      1.000 1.000 0.000
#&gt; GSM11270     2   0.000      0.976 0.000 1.000
#&gt; GSM11271     2   0.000      0.976 0.000 1.000
#&gt; GSM11288     2   0.000      0.976 0.000 1.000
#&gt; GSM11273     2   0.000      0.976 0.000 1.000
#&gt; GSM28757     2   0.000      0.976 0.000 1.000
#&gt; GSM11282     2   0.000      0.976 0.000 1.000
#&gt; GSM28756     2   0.000      0.976 0.000 1.000
#&gt; GSM11276     2   0.000      0.976 0.000 1.000
#&gt; GSM28752     2   0.000      0.976 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11274     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11268     3  0.1860      0.926 0.000 0.052 0.948
#&gt; GSM28767     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28751     2  0.3412      0.855 0.124 0.876 0.000
#&gt; GSM28770     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28749     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28750     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM28771     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28760     2  0.0892      0.966 0.000 0.980 0.020
#&gt; GSM28774     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28761     3  0.0747      0.972 0.000 0.016 0.984
#&gt; GSM11278     2  0.0892      0.966 0.000 0.980 0.020
#&gt; GSM11291     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM11272     3  0.0892      0.969 0.020 0.000 0.980
#&gt; GSM11285     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28768     2  0.6026      0.412 0.376 0.624 0.000
#&gt; GSM28754     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28769     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11270     2  0.0892      0.966 0.000 0.980 0.020
#&gt; GSM11271     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11288     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11273     2  0.2448      0.914 0.000 0.924 0.076
#&gt; GSM28757     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11282     2  0.0892      0.966 0.000 0.980 0.020
#&gt; GSM28756     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.980 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.980 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28764     2  0.2589      0.696 0.000 0.884 0.000 0.116
#&gt; GSM11274     3  0.0000      0.850 0.000 0.000 1.000 0.000
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11292     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM28766     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM11268     3  0.4916      0.805 0.000 0.000 0.576 0.424
#&gt; GSM28767     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM11286     2  0.0469      0.822 0.000 0.988 0.000 0.012
#&gt; GSM28751     2  0.0779      0.809 0.004 0.980 0.000 0.016
#&gt; GSM28770     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM11283     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11289     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM11280     2  0.0921      0.812 0.000 0.972 0.000 0.028
#&gt; GSM28749     2  0.4972     -0.623 0.000 0.544 0.000 0.456
#&gt; GSM28750     3  0.4866      0.808 0.000 0.000 0.596 0.404
#&gt; GSM11290     3  0.0000      0.850 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.850 0.000 0.000 1.000 0.000
#&gt; GSM28771     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28760     4  0.4866      0.947 0.000 0.404 0.000 0.596
#&gt; GSM28774     2  0.4925     -0.595 0.000 0.572 0.000 0.428
#&gt; GSM11284     4  0.4992      0.871 0.000 0.476 0.000 0.524
#&gt; GSM28761     3  0.4916      0.805 0.000 0.000 0.576 0.424
#&gt; GSM11278     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM11291     3  0.0000      0.850 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.850 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.4916      0.805 0.000 0.000 0.576 0.424
#&gt; GSM11285     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM28753     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28773     2  0.0921      0.812 0.000 0.972 0.000 0.028
#&gt; GSM28765     2  0.0336      0.822 0.000 0.992 0.000 0.008
#&gt; GSM28768     2  0.2335      0.753 0.060 0.920 0.000 0.020
#&gt; GSM28754     2  0.0336      0.822 0.000 0.992 0.000 0.008
#&gt; GSM28769     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM11271     2  0.4992     -0.743 0.000 0.524 0.000 0.476
#&gt; GSM11288     2  0.4008      0.364 0.000 0.756 0.000 0.244
#&gt; GSM11273     4  0.5337      0.967 0.000 0.424 0.012 0.564
#&gt; GSM28757     2  0.0336      0.822 0.000 0.992 0.000 0.008
#&gt; GSM11282     4  0.4916      0.983 0.000 0.424 0.000 0.576
#&gt; GSM28756     2  0.1940      0.757 0.000 0.924 0.000 0.076
#&gt; GSM11276     2  0.2589      0.696 0.000 0.884 0.000 0.116
#&gt; GSM28752     2  0.1302      0.791 0.000 0.956 0.000 0.044
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0000      0.797 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.797 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.3480      0.648 0.00 0.752 0.000 0.000 0.248
#&gt; GSM11274     3  0.0162      0.908 0.00 0.000 0.996 0.000 0.004
#&gt; GSM28772     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.1952      0.862 0.00 0.084 0.000 0.004 0.912
#&gt; GSM28766     5  0.1952      0.862 0.00 0.084 0.000 0.004 0.912
#&gt; GSM11268     4  0.0290      0.639 0.00 0.000 0.008 0.992 0.000
#&gt; GSM28767     5  0.1908      0.862 0.00 0.092 0.000 0.000 0.908
#&gt; GSM11286     2  0.2249      0.812 0.00 0.896 0.000 0.008 0.096
#&gt; GSM28751     2  0.0000      0.797 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28770     5  0.1908      0.862 0.00 0.092 0.000 0.000 0.908
#&gt; GSM11283     2  0.2136      0.717 0.00 0.904 0.000 0.008 0.088
#&gt; GSM11289     5  0.1908      0.862 0.00 0.092 0.000 0.000 0.908
#&gt; GSM11280     2  0.4818     -0.192 0.00 0.520 0.000 0.460 0.020
#&gt; GSM28749     4  0.6564      0.133 0.00 0.224 0.000 0.460 0.316
#&gt; GSM28750     3  0.4294      0.378 0.00 0.000 0.532 0.468 0.000
#&gt; GSM11290     3  0.0000      0.911 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.911 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28771     2  0.2136      0.717 0.00 0.904 0.000 0.008 0.088
#&gt; GSM28760     5  0.4294     -0.162 0.00 0.000 0.000 0.468 0.532
#&gt; GSM28774     5  0.3305      0.728 0.00 0.224 0.000 0.000 0.776
#&gt; GSM11284     5  0.6004      0.229 0.00 0.120 0.000 0.372 0.508
#&gt; GSM28761     4  0.0290      0.639 0.00 0.000 0.008 0.992 0.000
#&gt; GSM11278     5  0.1851      0.863 0.00 0.088 0.000 0.000 0.912
#&gt; GSM11291     3  0.0000      0.911 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.911 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11272     4  0.0290      0.639 0.00 0.000 0.008 0.992 0.000
#&gt; GSM11285     5  0.1952      0.862 0.00 0.084 0.000 0.004 0.912
#&gt; GSM28753     2  0.1908      0.814 0.00 0.908 0.000 0.000 0.092
#&gt; GSM28773     2  0.4897     -0.167 0.00 0.516 0.000 0.460 0.024
#&gt; GSM28765     2  0.1965      0.814 0.00 0.904 0.000 0.000 0.096
#&gt; GSM28768     2  0.1809      0.756 0.06 0.928 0.000 0.012 0.000
#&gt; GSM28754     2  0.1965      0.814 0.00 0.904 0.000 0.000 0.096
#&gt; GSM28769     2  0.0000      0.797 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.1851      0.863 0.00 0.088 0.000 0.000 0.912
#&gt; GSM11271     5  0.3999      0.479 0.00 0.344 0.000 0.000 0.656
#&gt; GSM11288     4  0.4989      0.258 0.00 0.416 0.000 0.552 0.032
#&gt; GSM11273     5  0.2077      0.859 0.00 0.084 0.008 0.000 0.908
#&gt; GSM28757     2  0.2124      0.813 0.00 0.900 0.000 0.004 0.096
#&gt; GSM11282     5  0.1792      0.862 0.00 0.084 0.000 0.000 0.916
#&gt; GSM28756     2  0.2424      0.793 0.00 0.868 0.000 0.000 0.132
#&gt; GSM11276     2  0.3480      0.648 0.00 0.752 0.000 0.000 0.248
#&gt; GSM28752     2  0.2471      0.790 0.00 0.864 0.000 0.000 0.136
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.3531    0.50311 0.00 0.672 0.000 0.000 0.328 0.000
#&gt; GSM28763     2  0.3531    0.50311 0.00 0.672 0.000 0.000 0.328 0.000
#&gt; GSM28764     2  0.2454    0.23214 0.00 0.840 0.000 0.000 0.160 0.000
#&gt; GSM11274     4  0.3828   -0.16204 0.00 0.000 0.440 0.560 0.000 0.000
#&gt; GSM28772     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.3592    0.87498 0.00 0.344 0.000 0.000 0.656 0.000
#&gt; GSM28766     5  0.3592    0.87498 0.00 0.344 0.000 0.000 0.656 0.000
#&gt; GSM11268     6  0.0000    0.48248 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     5  0.3804    0.90139 0.00 0.424 0.000 0.000 0.576 0.000
#&gt; GSM11286     2  0.0790    0.54194 0.00 0.968 0.000 0.000 0.000 0.032
#&gt; GSM28751     2  0.3531    0.50311 0.00 0.672 0.000 0.000 0.328 0.000
#&gt; GSM28770     5  0.3804    0.90139 0.00 0.424 0.000 0.000 0.576 0.000
#&gt; GSM11283     4  0.5656   -0.13353 0.00 0.152 0.000 0.440 0.408 0.000
#&gt; GSM11289     5  0.3804    0.90139 0.00 0.424 0.000 0.000 0.576 0.000
#&gt; GSM11280     6  0.6825    0.33639 0.00 0.248 0.000 0.048 0.316 0.388
#&gt; GSM28749     2  0.4666    0.00541 0.00 0.564 0.000 0.000 0.048 0.388
#&gt; GSM28750     3  0.3868    0.36569 0.00 0.000 0.504 0.000 0.000 0.496
#&gt; GSM11290     3  0.0000    0.88082 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000    0.88082 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     4  0.5561   -0.12817 0.00 0.136 0.000 0.440 0.424 0.000
#&gt; GSM28760     4  0.6871   -0.11230 0.00 0.292 0.000 0.460 0.104 0.144
#&gt; GSM28774     4  0.4855    0.37622 0.00 0.064 0.000 0.556 0.380 0.000
#&gt; GSM11284     2  0.6117   -0.43636 0.00 0.352 0.000 0.000 0.300 0.348
#&gt; GSM28761     6  0.0000    0.48248 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     4  0.4212    0.39426 0.00 0.016 0.000 0.560 0.424 0.000
#&gt; GSM11291     3  0.0000    0.88082 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000    0.88082 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.0000    0.48248 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11285     5  0.3592    0.87498 0.00 0.344 0.000 0.000 0.656 0.000
#&gt; GSM28753     2  0.3428    0.51586 0.00 0.696 0.000 0.000 0.304 0.000
#&gt; GSM28773     6  0.6093    0.26225 0.00 0.296 0.000 0.000 0.316 0.388
#&gt; GSM28765     2  0.0000    0.54637 0.00 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28768     2  0.5152    0.43189 0.06 0.592 0.000 0.000 0.328 0.020
#&gt; GSM28754     2  0.0777    0.53238 0.00 0.972 0.000 0.024 0.004 0.000
#&gt; GSM28769     2  0.3531    0.50311 0.00 0.672 0.000 0.000 0.328 0.000
#&gt; GSM11275     1  0.0000    1.00000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.4212    0.39426 0.00 0.016 0.000 0.560 0.424 0.000
#&gt; GSM11271     5  0.3862    0.82726 0.00 0.476 0.000 0.000 0.524 0.000
#&gt; GSM11288     6  0.5902    0.38037 0.00 0.204 0.000 0.000 0.392 0.404
#&gt; GSM11273     4  0.4433    0.39700 0.00 0.016 0.008 0.560 0.416 0.000
#&gt; GSM28757     2  0.5524    0.38407 0.00 0.560 0.000 0.204 0.236 0.000
#&gt; GSM11282     4  0.4212    0.39426 0.00 0.016 0.000 0.560 0.424 0.000
#&gt; GSM28756     2  0.1007    0.50613 0.00 0.956 0.000 0.000 0.044 0.000
#&gt; GSM11276     2  0.2454    0.23214 0.00 0.840 0.000 0.000 0.160 0.000
#&gt; GSM28752     2  0.0547    0.54969 0.00 0.980 0.000 0.000 0.020 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:pam 53     0.397 2
#> MAD:pam 53     0.373 3
#> MAD:pam 50     0.426 4
#> MAD:pam 46     0.393 5
#> MAD:pam 32     0.395 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.851           0.925       0.958         0.4123 0.560   0.560
#> 3 3 0.995           0.953       0.975         0.5234 0.723   0.541
#> 4 4 0.649           0.702       0.785         0.1369 0.762   0.452
#> 5 5 0.731           0.543       0.737         0.0693 0.783   0.439
#> 6 6 0.842           0.857       0.922         0.0478 0.832   0.493
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0000      0.984 0.000 1.000
#&gt; GSM28763     2  0.0000      0.984 0.000 1.000
#&gt; GSM28764     2  0.0000      0.984 0.000 1.000
#&gt; GSM11274     1  0.9522      0.544 0.628 0.372
#&gt; GSM28772     1  0.0000      0.884 1.000 0.000
#&gt; GSM11269     1  0.0000      0.884 1.000 0.000
#&gt; GSM28775     1  0.2948      0.868 0.948 0.052
#&gt; GSM11293     1  0.0000      0.884 1.000 0.000
#&gt; GSM28755     1  0.0000      0.884 1.000 0.000
#&gt; GSM11279     1  0.0000      0.884 1.000 0.000
#&gt; GSM28758     1  0.0000      0.884 1.000 0.000
#&gt; GSM11281     1  0.0000      0.884 1.000 0.000
#&gt; GSM11287     1  0.0000      0.884 1.000 0.000
#&gt; GSM28759     1  0.0000      0.884 1.000 0.000
#&gt; GSM11292     2  0.0000      0.984 0.000 1.000
#&gt; GSM28766     2  0.0000      0.984 0.000 1.000
#&gt; GSM11268     2  0.2948      0.955 0.052 0.948
#&gt; GSM28767     2  0.0000      0.984 0.000 1.000
#&gt; GSM11286     2  0.0000      0.984 0.000 1.000
#&gt; GSM28751     2  0.0000      0.984 0.000 1.000
#&gt; GSM28770     2  0.0000      0.984 0.000 1.000
#&gt; GSM11283     2  0.0000      0.984 0.000 1.000
#&gt; GSM11289     2  0.0000      0.984 0.000 1.000
#&gt; GSM11280     2  0.1184      0.977 0.016 0.984
#&gt; GSM28749     2  0.2236      0.967 0.036 0.964
#&gt; GSM28750     2  0.2948      0.955 0.052 0.948
#&gt; GSM11290     1  0.7883      0.761 0.764 0.236
#&gt; GSM11294     1  0.7883      0.761 0.764 0.236
#&gt; GSM28771     2  0.0000      0.984 0.000 1.000
#&gt; GSM28760     2  0.2236      0.967 0.036 0.964
#&gt; GSM28774     2  0.0000      0.984 0.000 1.000
#&gt; GSM11284     2  0.0000      0.984 0.000 1.000
#&gt; GSM28761     2  0.2948      0.955 0.052 0.948
#&gt; GSM11278     2  0.2778      0.958 0.048 0.952
#&gt; GSM11291     1  0.7883      0.761 0.764 0.236
#&gt; GSM11277     1  0.7883      0.761 0.764 0.236
#&gt; GSM11272     2  0.2948      0.955 0.052 0.948
#&gt; GSM11285     2  0.0000      0.984 0.000 1.000
#&gt; GSM28753     2  0.0000      0.984 0.000 1.000
#&gt; GSM28773     2  0.2236      0.967 0.036 0.964
#&gt; GSM28765     2  0.0000      0.984 0.000 1.000
#&gt; GSM28768     2  0.0672      0.980 0.008 0.992
#&gt; GSM28754     2  0.0000      0.984 0.000 1.000
#&gt; GSM28769     2  0.0000      0.984 0.000 1.000
#&gt; GSM11275     1  0.1184      0.880 0.984 0.016
#&gt; GSM11270     2  0.2778      0.958 0.048 0.952
#&gt; GSM11271     2  0.0000      0.984 0.000 1.000
#&gt; GSM11288     2  0.2603      0.961 0.044 0.956
#&gt; GSM11273     1  0.9580      0.526 0.620 0.380
#&gt; GSM28757     2  0.0000      0.984 0.000 1.000
#&gt; GSM11282     2  0.2423      0.964 0.040 0.960
#&gt; GSM28756     2  0.0000      0.984 0.000 1.000
#&gt; GSM11276     2  0.0000      0.984 0.000 1.000
#&gt; GSM28752     2  0.0000      0.984 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.1289      0.972 0.032 0.968 0.000
#&gt; GSM28763     2  0.1289      0.972 0.032 0.968 0.000
#&gt; GSM28764     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11274     3  0.1289      0.930 0.000 0.032 0.968
#&gt; GSM28772     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28775     1  0.1753      0.948 0.952 0.000 0.048
#&gt; GSM11293     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11268     3  0.0424      0.934 0.008 0.000 0.992
#&gt; GSM28767     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11286     2  0.1031      0.975 0.024 0.976 0.000
#&gt; GSM28751     2  0.1289      0.972 0.032 0.968 0.000
#&gt; GSM28770     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11283     2  0.0424      0.978 0.000 0.992 0.008
#&gt; GSM11289     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11280     2  0.0237      0.981 0.000 0.996 0.004
#&gt; GSM28749     2  0.2384      0.935 0.008 0.936 0.056
#&gt; GSM28750     3  0.0000      0.934 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.934 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.934 0.000 0.000 1.000
#&gt; GSM28771     3  0.6168      0.372 0.000 0.412 0.588
#&gt; GSM28760     3  0.1964      0.921 0.000 0.056 0.944
#&gt; GSM28774     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM28761     3  0.0424      0.934 0.008 0.000 0.992
#&gt; GSM11278     3  0.2796      0.894 0.000 0.092 0.908
#&gt; GSM11291     3  0.0000      0.934 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.934 0.000 0.000 1.000
#&gt; GSM11272     3  0.0424      0.934 0.008 0.000 0.992
#&gt; GSM11285     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM28753     2  0.1289      0.972 0.032 0.968 0.000
#&gt; GSM28773     2  0.2774      0.917 0.008 0.920 0.072
#&gt; GSM28765     2  0.1163      0.974 0.028 0.972 0.000
#&gt; GSM28768     2  0.1289      0.972 0.032 0.968 0.000
#&gt; GSM28754     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM28769     2  0.1289      0.972 0.032 0.968 0.000
#&gt; GSM11275     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11270     3  0.2537      0.905 0.000 0.080 0.920
#&gt; GSM11271     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11288     3  0.1170      0.931 0.016 0.008 0.976
#&gt; GSM11273     3  0.1529      0.928 0.000 0.040 0.960
#&gt; GSM28757     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11282     3  0.2625      0.902 0.000 0.084 0.916
#&gt; GSM28756     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.983 0.000 1.000 0.000
#&gt; GSM28752     2  0.1289      0.972 0.032 0.968 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.0000     0.8034 0.000 1.000 0.000 0.000
#&gt; GSM28763     2  0.0188     0.8039 0.000 0.996 0.000 0.004
#&gt; GSM28764     2  0.3873     0.5925 0.000 0.772 0.000 0.228
#&gt; GSM11274     4  0.4967     0.0921 0.000 0.000 0.452 0.548
#&gt; GSM28772     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0336     0.9908 0.992 0.000 0.000 0.008
#&gt; GSM11293     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9985 1.000 0.000 0.000 0.000
#&gt; GSM11292     4  0.4907     0.5324 0.000 0.420 0.000 0.580
#&gt; GSM28766     4  0.5070     0.5374 0.000 0.416 0.004 0.580
#&gt; GSM11268     3  0.4228     0.8674 0.000 0.008 0.760 0.232
#&gt; GSM28767     4  0.4830     0.5743 0.000 0.392 0.000 0.608
#&gt; GSM11286     2  0.0188     0.8044 0.000 0.996 0.000 0.004
#&gt; GSM28751     2  0.0336     0.8040 0.000 0.992 0.000 0.008
#&gt; GSM28770     4  0.4730     0.5821 0.000 0.364 0.000 0.636
#&gt; GSM11283     2  0.4008     0.5913 0.000 0.756 0.000 0.244
#&gt; GSM11289     4  0.4830     0.5739 0.000 0.392 0.000 0.608
#&gt; GSM11280     2  0.2589     0.7452 0.000 0.884 0.000 0.116
#&gt; GSM28749     2  0.3266     0.6983 0.000 0.832 0.000 0.168
#&gt; GSM28750     3  0.4228     0.8674 0.000 0.008 0.760 0.232
#&gt; GSM11290     3  0.0188     0.8643 0.000 0.000 0.996 0.004
#&gt; GSM11294     3  0.0000     0.8644 0.000 0.000 1.000 0.000
#&gt; GSM28771     2  0.4784     0.6671 0.000 0.788 0.112 0.100
#&gt; GSM28760     4  0.6850     0.3526 0.000 0.188 0.212 0.600
#&gt; GSM28774     4  0.4713     0.5819 0.000 0.360 0.000 0.640
#&gt; GSM11284     4  0.4925     0.5196 0.000 0.428 0.000 0.572
#&gt; GSM28761     3  0.4228     0.8674 0.000 0.008 0.760 0.232
#&gt; GSM11278     4  0.6315     0.2485 0.000 0.064 0.396 0.540
#&gt; GSM11291     3  0.0000     0.8644 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000     0.8644 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.4228     0.8674 0.000 0.008 0.760 0.232
#&gt; GSM11285     4  0.4843     0.5649 0.000 0.396 0.000 0.604
#&gt; GSM28753     2  0.1474     0.7888 0.000 0.948 0.000 0.052
#&gt; GSM28773     2  0.3266     0.6996 0.000 0.832 0.000 0.168
#&gt; GSM28765     2  0.0000     0.8034 0.000 1.000 0.000 0.000
#&gt; GSM28768     2  0.0804     0.8023 0.008 0.980 0.000 0.012
#&gt; GSM28754     2  0.4661     0.2711 0.000 0.652 0.000 0.348
#&gt; GSM28769     2  0.0817     0.8007 0.000 0.976 0.000 0.024
#&gt; GSM11275     1  0.0188     0.9951 0.996 0.000 0.000 0.004
#&gt; GSM11270     4  0.6315     0.2485 0.000 0.064 0.396 0.540
#&gt; GSM11271     4  0.4830     0.5743 0.000 0.392 0.000 0.608
#&gt; GSM11288     2  0.6889     0.4358 0.000 0.592 0.176 0.232
#&gt; GSM11273     4  0.4967     0.0921 0.000 0.000 0.452 0.548
#&gt; GSM28757     2  0.3486     0.6459 0.000 0.812 0.000 0.188
#&gt; GSM11282     4  0.6367     0.2550 0.000 0.068 0.392 0.540
#&gt; GSM28756     4  0.4843     0.5700 0.000 0.396 0.000 0.604
#&gt; GSM11276     2  0.4103     0.5430 0.000 0.744 0.000 0.256
#&gt; GSM28752     2  0.0469     0.7993 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.2966      0.485  0 0.816 0.000 0.184 0.000
#&gt; GSM28763     2  0.4126      0.236  0 0.620 0.000 0.380 0.000
#&gt; GSM28764     2  0.2719      0.601  0 0.852 0.000 0.004 0.144
#&gt; GSM11274     5  0.4305      0.316  0 0.000 0.000 0.488 0.512
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.4300      0.477  0 0.524 0.000 0.000 0.476
#&gt; GSM28766     2  0.4304      0.467  0 0.516 0.000 0.000 0.484
#&gt; GSM11268     3  0.0000      0.968  0 0.000 1.000 0.000 0.000
#&gt; GSM28767     2  0.4300      0.477  0 0.524 0.000 0.000 0.476
#&gt; GSM11286     2  0.0162      0.603  0 0.996 0.000 0.004 0.000
#&gt; GSM28751     2  0.1792      0.568  0 0.916 0.000 0.084 0.000
#&gt; GSM28770     2  0.4446      0.477  0 0.520 0.000 0.004 0.476
#&gt; GSM11283     2  0.2416      0.609  0 0.888 0.000 0.012 0.100
#&gt; GSM11289     2  0.4446      0.477  0 0.520 0.000 0.004 0.476
#&gt; GSM11280     2  0.1041      0.592  0 0.964 0.004 0.032 0.000
#&gt; GSM28749     2  0.2011      0.560  0 0.908 0.004 0.088 0.000
#&gt; GSM28750     3  0.1341      0.900  0 0.000 0.944 0.056 0.000
#&gt; GSM11290     4  0.4798      0.106  0 0.000 0.440 0.540 0.020
#&gt; GSM11294     4  0.4942      0.122  0 0.000 0.432 0.540 0.028
#&gt; GSM28771     4  0.5434     -0.150  0 0.452 0.004 0.496 0.048
#&gt; GSM28760     4  0.6035     -0.144  0 0.092 0.012 0.544 0.352
#&gt; GSM28774     2  0.4440      0.483  0 0.528 0.000 0.004 0.468
#&gt; GSM11284     2  0.4249      0.493  0 0.568 0.000 0.000 0.432
#&gt; GSM28761     3  0.0000      0.968  0 0.000 1.000 0.000 0.000
#&gt; GSM11278     5  0.3003      0.581  0 0.000 0.000 0.188 0.812
#&gt; GSM11291     4  0.4942      0.122  0 0.000 0.432 0.540 0.028
#&gt; GSM11277     4  0.4942      0.122  0 0.000 0.432 0.540 0.028
#&gt; GSM11272     3  0.0000      0.968  0 0.000 1.000 0.000 0.000
#&gt; GSM11285     2  0.4446      0.468  0 0.520 0.004 0.000 0.476
#&gt; GSM28753     2  0.4242      0.172  0 0.572 0.000 0.428 0.000
#&gt; GSM28773     2  0.2970      0.493  0 0.828 0.004 0.168 0.000
#&gt; GSM28765     2  0.0162      0.603  0 0.996 0.000 0.004 0.000
#&gt; GSM28768     2  0.4161      0.219  0 0.608 0.000 0.392 0.000
#&gt; GSM28754     5  0.6676     -0.397  0 0.344 0.000 0.240 0.416
#&gt; GSM28769     2  0.4227      0.179  0 0.580 0.000 0.420 0.000
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11270     5  0.3003      0.581  0 0.000 0.000 0.188 0.812
#&gt; GSM11271     2  0.4300      0.477  0 0.524 0.000 0.000 0.476
#&gt; GSM11288     4  0.6240     -0.036  0 0.152 0.360 0.488 0.000
#&gt; GSM11273     5  0.4297      0.331  0 0.000 0.000 0.472 0.528
#&gt; GSM28757     2  0.5270      0.502  0 0.672 0.000 0.208 0.120
#&gt; GSM11282     5  0.3039      0.579  0 0.000 0.000 0.192 0.808
#&gt; GSM28756     2  0.4287      0.487  0 0.540 0.000 0.000 0.460
#&gt; GSM11276     2  0.2848      0.599  0 0.840 0.000 0.004 0.156
#&gt; GSM28752     2  0.0451      0.605  0 0.988 0.000 0.004 0.008
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.1610     0.8363  0 0.916 0.000 0.000 0.084 0.000
#&gt; GSM28763     2  0.1501     0.8299  0 0.924 0.000 0.000 0.076 0.000
#&gt; GSM28764     5  0.2260     0.7967  0 0.140 0.000 0.000 0.860 0.000
#&gt; GSM11274     4  0.2340     0.8392  0 0.000 0.148 0.852 0.000 0.000
#&gt; GSM28772     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28766     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11268     6  0.0146     0.9969  0 0.000 0.004 0.000 0.000 0.996
#&gt; GSM28767     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11286     2  0.2092     0.8341  0 0.876 0.000 0.000 0.124 0.000
#&gt; GSM28751     2  0.1957     0.8374  0 0.888 0.000 0.000 0.112 0.000
#&gt; GSM28770     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11283     2  0.4184     0.1094  0 0.500 0.000 0.012 0.488 0.000
#&gt; GSM11289     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11280     2  0.2095     0.8314  0 0.904 0.000 0.016 0.076 0.004
#&gt; GSM28749     2  0.2998     0.8126  0 0.856 0.000 0.064 0.072 0.008
#&gt; GSM28750     6  0.0260     0.9969  0 0.000 0.008 0.000 0.000 0.992
#&gt; GSM11290     3  0.0000     1.0000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000     1.0000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     2  0.5025     0.4163  0 0.608 0.000 0.108 0.284 0.000
#&gt; GSM28760     5  0.6058     0.0876  0 0.260 0.000 0.356 0.384 0.000
#&gt; GSM28774     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11284     5  0.0363     0.8921  0 0.012 0.000 0.000 0.988 0.000
#&gt; GSM28761     6  0.0146     0.9969  0 0.000 0.004 0.000 0.000 0.996
#&gt; GSM11278     4  0.1327     0.9352  0 0.000 0.000 0.936 0.064 0.000
#&gt; GSM11291     3  0.0000     1.0000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000     1.0000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.0260     0.9969  0 0.000 0.008 0.000 0.000 0.992
#&gt; GSM11285     5  0.0405     0.8893  0 0.004 0.000 0.008 0.988 0.000
#&gt; GSM28753     2  0.0551     0.8118  0 0.984 0.000 0.008 0.004 0.004
#&gt; GSM28773     2  0.2765     0.8102  0 0.872 0.000 0.064 0.056 0.008
#&gt; GSM28765     2  0.2048     0.8354  0 0.880 0.000 0.000 0.120 0.000
#&gt; GSM28768     2  0.1007     0.8298  0 0.956 0.000 0.000 0.044 0.000
#&gt; GSM28754     5  0.2823     0.7370  0 0.204 0.000 0.000 0.796 0.000
#&gt; GSM28769     2  0.0260     0.8157  0 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11275     1  0.0000     1.0000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.1327     0.9352  0 0.000 0.000 0.936 0.064 0.000
#&gt; GSM11271     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11288     2  0.4575     0.3057  0 0.600 0.000 0.048 0.000 0.352
#&gt; GSM11273     4  0.1327     0.9007  0 0.000 0.064 0.936 0.000 0.000
#&gt; GSM28757     5  0.3288     0.6467  0 0.276 0.000 0.000 0.724 0.000
#&gt; GSM11282     4  0.1327     0.9352  0 0.000 0.000 0.936 0.064 0.000
#&gt; GSM28756     5  0.0000     0.8952  0 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11276     5  0.2219     0.8000  0 0.136 0.000 0.000 0.864 0.000
#&gt; GSM28752     2  0.2454     0.8125  0 0.840 0.000 0.000 0.160 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:mclust 54     0.398 2
#> MAD:mclust 53     0.373 3
#> MAD:mclust 46     0.427 4
#> MAD:mclust 28     0.388 5
#> MAD:mclust 50     0.400 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.962           0.959       0.982         0.3825 0.628   0.628
#> 3 3 0.969           0.947       0.981         0.5617 0.728   0.580
#> 4 4 0.753           0.790       0.892         0.1742 0.883   0.717
#> 5 5 0.811           0.807       0.896         0.1168 0.878   0.625
#> 6 6 0.767           0.671       0.822         0.0526 0.886   0.537
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.0376      0.978 0.004 0.996
#&gt; GSM28763     2  0.3733      0.916 0.072 0.928
#&gt; GSM28764     2  0.0000      0.980 0.000 1.000
#&gt; GSM11274     2  0.0000      0.980 0.000 1.000
#&gt; GSM28772     1  0.0000      0.982 1.000 0.000
#&gt; GSM11269     1  0.0000      0.982 1.000 0.000
#&gt; GSM28775     1  0.0000      0.982 1.000 0.000
#&gt; GSM11293     1  0.0000      0.982 1.000 0.000
#&gt; GSM28755     1  0.0000      0.982 1.000 0.000
#&gt; GSM11279     1  0.0000      0.982 1.000 0.000
#&gt; GSM28758     1  0.0000      0.982 1.000 0.000
#&gt; GSM11281     1  0.0000      0.982 1.000 0.000
#&gt; GSM11287     1  0.0000      0.982 1.000 0.000
#&gt; GSM28759     1  0.0000      0.982 1.000 0.000
#&gt; GSM11292     2  0.0000      0.980 0.000 1.000
#&gt; GSM28766     2  0.0000      0.980 0.000 1.000
#&gt; GSM11268     2  0.0000      0.980 0.000 1.000
#&gt; GSM28767     2  0.0000      0.980 0.000 1.000
#&gt; GSM11286     2  0.1184      0.968 0.016 0.984
#&gt; GSM28751     1  0.7376      0.726 0.792 0.208
#&gt; GSM28770     2  0.0000      0.980 0.000 1.000
#&gt; GSM11283     2  0.0000      0.980 0.000 1.000
#&gt; GSM11289     2  0.0000      0.980 0.000 1.000
#&gt; GSM11280     2  0.0000      0.980 0.000 1.000
#&gt; GSM28749     2  0.0000      0.980 0.000 1.000
#&gt; GSM28750     2  0.0000      0.980 0.000 1.000
#&gt; GSM11290     2  0.0000      0.980 0.000 1.000
#&gt; GSM11294     2  0.0000      0.980 0.000 1.000
#&gt; GSM28771     2  0.0000      0.980 0.000 1.000
#&gt; GSM28760     2  0.0000      0.980 0.000 1.000
#&gt; GSM28774     2  0.0000      0.980 0.000 1.000
#&gt; GSM11284     2  0.0000      0.980 0.000 1.000
#&gt; GSM28761     2  0.0000      0.980 0.000 1.000
#&gt; GSM11278     2  0.0000      0.980 0.000 1.000
#&gt; GSM11291     2  0.0000      0.980 0.000 1.000
#&gt; GSM11277     2  0.0000      0.980 0.000 1.000
#&gt; GSM11272     2  0.5737      0.843 0.136 0.864
#&gt; GSM11285     2  0.0000      0.980 0.000 1.000
#&gt; GSM28753     2  0.0000      0.980 0.000 1.000
#&gt; GSM28773     2  0.0000      0.980 0.000 1.000
#&gt; GSM28765     2  0.0376      0.978 0.004 0.996
#&gt; GSM28768     1  0.0000      0.982 1.000 0.000
#&gt; GSM28754     2  0.0000      0.980 0.000 1.000
#&gt; GSM28769     2  0.9427      0.444 0.360 0.640
#&gt; GSM11275     1  0.0000      0.982 1.000 0.000
#&gt; GSM11270     2  0.0000      0.980 0.000 1.000
#&gt; GSM11271     2  0.0000      0.980 0.000 1.000
#&gt; GSM11288     2  0.6148      0.819 0.152 0.848
#&gt; GSM11273     2  0.0000      0.980 0.000 1.000
#&gt; GSM28757     2  0.0000      0.980 0.000 1.000
#&gt; GSM11282     2  0.0000      0.980 0.000 1.000
#&gt; GSM28756     2  0.0000      0.980 0.000 1.000
#&gt; GSM11276     2  0.0000      0.980 0.000 1.000
#&gt; GSM28752     2  0.1414      0.965 0.020 0.980
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28763     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28764     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11274     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM28772     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11269     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM28775     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11293     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM28755     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11279     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM28758     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11281     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11287     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM28759     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11292     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28766     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11268     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM28767     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11286     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28751     2   0.296     0.8858 0.100 0.900 0.000
#&gt; GSM28770     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11283     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11289     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11280     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28749     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28750     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM11290     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM11294     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM28771     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28760     3   0.630     0.0897 0.000 0.476 0.524
#&gt; GSM28774     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11284     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28761     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM11278     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11291     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM11277     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM11272     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM11285     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28753     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28773     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28765     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28768     1   0.334     0.8219 0.880 0.120 0.000
#&gt; GSM28754     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28769     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11275     1   0.000     0.9846 1.000 0.000 0.000
#&gt; GSM11270     2   0.116     0.9672 0.000 0.972 0.028
#&gt; GSM11271     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11288     3   0.546     0.5544 0.288 0.000 0.712
#&gt; GSM11273     3   0.000     0.9142 0.000 0.000 1.000
#&gt; GSM28757     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11282     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28756     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM11276     2   0.000     0.9953 0.000 1.000 0.000
#&gt; GSM28752     2   0.000     0.9953 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.2760      0.830 0.000 0.872 0.000 0.128
#&gt; GSM28763     2  0.2589      0.839 0.000 0.884 0.000 0.116
#&gt; GSM28764     2  0.0817      0.871 0.000 0.976 0.000 0.024
#&gt; GSM11274     3  0.0000      0.892 0.000 0.000 1.000 0.000
#&gt; GSM28772     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.1474      0.866 0.000 0.948 0.000 0.052
#&gt; GSM28766     2  0.1637      0.864 0.000 0.940 0.000 0.060
#&gt; GSM11268     4  0.3569      0.629 0.000 0.000 0.196 0.804
#&gt; GSM28767     2  0.1389      0.867 0.000 0.952 0.000 0.048
#&gt; GSM11286     2  0.2647      0.858 0.000 0.880 0.000 0.120
#&gt; GSM28751     2  0.5434      0.718 0.084 0.728 0.000 0.188
#&gt; GSM28770     2  0.0817      0.871 0.000 0.976 0.000 0.024
#&gt; GSM11283     2  0.4088      0.733 0.000 0.764 0.004 0.232
#&gt; GSM11289     2  0.0469      0.871 0.000 0.988 0.000 0.012
#&gt; GSM11280     4  0.4989     -0.196 0.000 0.472 0.000 0.528
#&gt; GSM28749     4  0.2342      0.648 0.000 0.080 0.008 0.912
#&gt; GSM28750     4  0.4564      0.476 0.000 0.000 0.328 0.672
#&gt; GSM11290     3  0.0921      0.889 0.000 0.000 0.972 0.028
#&gt; GSM11294     3  0.0707      0.896 0.000 0.000 0.980 0.020
#&gt; GSM28771     2  0.5368      0.538 0.000 0.636 0.024 0.340
#&gt; GSM28760     4  0.7553      0.231 0.000 0.324 0.208 0.468
#&gt; GSM28774     2  0.1022      0.866 0.000 0.968 0.000 0.032
#&gt; GSM11284     2  0.2345      0.854 0.000 0.900 0.000 0.100
#&gt; GSM28761     4  0.3444      0.635 0.000 0.000 0.184 0.816
#&gt; GSM11278     2  0.4244      0.720 0.000 0.800 0.168 0.032
#&gt; GSM11291     3  0.0707      0.896 0.000 0.000 0.980 0.020
#&gt; GSM11277     3  0.0592      0.896 0.000 0.000 0.984 0.016
#&gt; GSM11272     4  0.4072      0.580 0.000 0.000 0.252 0.748
#&gt; GSM11285     2  0.1792      0.862 0.000 0.932 0.000 0.068
#&gt; GSM28753     2  0.4996      0.235 0.000 0.516 0.000 0.484
#&gt; GSM28773     4  0.1211      0.646 0.000 0.040 0.000 0.960
#&gt; GSM28765     2  0.2081      0.867 0.000 0.916 0.000 0.084
#&gt; GSM28768     1  0.1209      0.949 0.964 0.032 0.000 0.004
#&gt; GSM28754     2  0.1022      0.866 0.000 0.968 0.000 0.032
#&gt; GSM28769     2  0.4697      0.521 0.000 0.644 0.000 0.356
#&gt; GSM11275     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.5411      0.414 0.000 0.312 0.656 0.032
#&gt; GSM11271     2  0.1118      0.869 0.000 0.964 0.000 0.036
#&gt; GSM11288     4  0.3016      0.653 0.040 0.004 0.060 0.896
#&gt; GSM11273     3  0.0188      0.889 0.000 0.000 0.996 0.004
#&gt; GSM28757     2  0.2281      0.857 0.000 0.904 0.000 0.096
#&gt; GSM11282     2  0.3149      0.812 0.000 0.880 0.088 0.032
#&gt; GSM28756     2  0.0817      0.868 0.000 0.976 0.000 0.024
#&gt; GSM11276     2  0.0469      0.871 0.000 0.988 0.000 0.012
#&gt; GSM28752     2  0.1474      0.867 0.000 0.948 0.000 0.052
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     4  0.3561      0.695 0.000 0.260 0.000 0.740 0.000
#&gt; GSM28763     4  0.3857      0.652 0.000 0.312 0.000 0.688 0.000
#&gt; GSM28764     2  0.1628      0.839 0.000 0.936 0.000 0.056 0.008
#&gt; GSM11274     3  0.0290      0.944 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.2284      0.831 0.000 0.912 0.004 0.056 0.028
#&gt; GSM28766     2  0.3493      0.774 0.000 0.832 0.000 0.060 0.108
#&gt; GSM11268     5  0.0566      0.864 0.000 0.000 0.004 0.012 0.984
#&gt; GSM28767     2  0.1697      0.836 0.000 0.932 0.000 0.060 0.008
#&gt; GSM11286     2  0.3508      0.612 0.000 0.748 0.000 0.252 0.000
#&gt; GSM28751     4  0.5435      0.633 0.188 0.152 0.000 0.660 0.000
#&gt; GSM28770     2  0.1518      0.841 0.000 0.944 0.004 0.048 0.004
#&gt; GSM11283     4  0.0510      0.762 0.000 0.016 0.000 0.984 0.000
#&gt; GSM11289     2  0.1764      0.835 0.000 0.928 0.000 0.064 0.008
#&gt; GSM11280     4  0.1661      0.720 0.000 0.024 0.000 0.940 0.036
#&gt; GSM28749     5  0.3355      0.775 0.000 0.036 0.000 0.132 0.832
#&gt; GSM28750     5  0.2077      0.812 0.000 0.000 0.084 0.008 0.908
#&gt; GSM11290     3  0.1671      0.924 0.000 0.000 0.924 0.000 0.076
#&gt; GSM11294     3  0.1043      0.952 0.000 0.000 0.960 0.000 0.040
#&gt; GSM28771     4  0.0609      0.764 0.000 0.020 0.000 0.980 0.000
#&gt; GSM28760     4  0.0992      0.742 0.000 0.000 0.008 0.968 0.024
#&gt; GSM28774     2  0.1331      0.825 0.000 0.952 0.008 0.040 0.000
#&gt; GSM11284     2  0.4210      0.315 0.000 0.588 0.000 0.412 0.000
#&gt; GSM28761     5  0.0162      0.861 0.000 0.004 0.000 0.000 0.996
#&gt; GSM11278     2  0.4268      0.498 0.000 0.648 0.344 0.008 0.000
#&gt; GSM11291     3  0.1043      0.952 0.000 0.000 0.960 0.000 0.040
#&gt; GSM11277     3  0.1043      0.952 0.000 0.000 0.960 0.000 0.040
#&gt; GSM11272     5  0.0404      0.862 0.000 0.000 0.012 0.000 0.988
#&gt; GSM11285     4  0.4653      0.138 0.000 0.472 0.000 0.516 0.012
#&gt; GSM28753     4  0.1364      0.766 0.000 0.036 0.000 0.952 0.012
#&gt; GSM28773     5  0.4300      0.205 0.000 0.000 0.000 0.476 0.524
#&gt; GSM28765     2  0.0912      0.838 0.000 0.972 0.000 0.012 0.016
#&gt; GSM28768     1  0.0324      0.991 0.992 0.004 0.000 0.004 0.000
#&gt; GSM28754     2  0.1830      0.811 0.000 0.924 0.008 0.068 0.000
#&gt; GSM28769     4  0.4342      0.711 0.000 0.232 0.000 0.728 0.040
#&gt; GSM11275     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.2411      0.834 0.000 0.108 0.884 0.008 0.000
#&gt; GSM11271     2  0.1408      0.841 0.000 0.948 0.000 0.044 0.008
#&gt; GSM11288     5  0.0609      0.863 0.000 0.000 0.000 0.020 0.980
#&gt; GSM11273     3  0.0000      0.941 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28757     2  0.4276      0.357 0.000 0.616 0.004 0.380 0.000
#&gt; GSM11282     2  0.3300      0.693 0.000 0.792 0.204 0.004 0.000
#&gt; GSM28756     2  0.0693      0.832 0.000 0.980 0.008 0.012 0.000
#&gt; GSM11276     2  0.1270      0.841 0.000 0.948 0.000 0.052 0.000
#&gt; GSM28752     2  0.0798      0.839 0.000 0.976 0.000 0.016 0.008
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.5926     0.4373 0.000 0.460 0.000 0.296 0.244 0.000
#&gt; GSM28763     2  0.5676     0.4850 0.000 0.528 0.000 0.256 0.216 0.000
#&gt; GSM28764     5  0.0790     0.7954 0.000 0.032 0.000 0.000 0.968 0.000
#&gt; GSM11274     3  0.0260     0.7996 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; GSM28772     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.1471     0.7417 0.000 0.004 0.000 0.000 0.932 0.064
#&gt; GSM28766     5  0.3189     0.5170 0.000 0.004 0.000 0.000 0.760 0.236
#&gt; GSM11268     6  0.2003     0.8646 0.000 0.116 0.000 0.000 0.000 0.884
#&gt; GSM28767     5  0.0260     0.7932 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM11286     2  0.3372     0.5361 0.000 0.796 0.000 0.008 0.176 0.020
#&gt; GSM28751     2  0.7386     0.1669 0.192 0.392 0.000 0.124 0.288 0.004
#&gt; GSM28770     5  0.0547     0.7982 0.000 0.020 0.000 0.000 0.980 0.000
#&gt; GSM11283     4  0.0146     0.8148 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11289     5  0.0291     0.7945 0.000 0.004 0.000 0.000 0.992 0.004
#&gt; GSM11280     4  0.3445     0.6485 0.000 0.260 0.000 0.732 0.000 0.008
#&gt; GSM28749     6  0.3954     0.6301 0.000 0.352 0.000 0.000 0.012 0.636
#&gt; GSM28750     6  0.1931     0.8286 0.000 0.004 0.068 0.008 0.004 0.916
#&gt; GSM11290     3  0.1765     0.7704 0.000 0.000 0.904 0.000 0.000 0.096
#&gt; GSM11294     3  0.1219     0.8026 0.000 0.004 0.948 0.000 0.000 0.048
#&gt; GSM28771     4  0.0000     0.8156 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28760     4  0.0000     0.8156 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28774     2  0.4310     0.2855 0.000 0.512 0.012 0.004 0.472 0.000
#&gt; GSM11284     2  0.5521     0.4308 0.000 0.536 0.000 0.132 0.328 0.004
#&gt; GSM28761     6  0.1349     0.8801 0.000 0.056 0.000 0.000 0.004 0.940
#&gt; GSM11278     3  0.5898    -0.0585 0.000 0.380 0.416 0.000 0.204 0.000
#&gt; GSM11291     3  0.1007     0.8040 0.000 0.000 0.956 0.000 0.000 0.044
#&gt; GSM11277     3  0.1152     0.8040 0.000 0.004 0.952 0.000 0.000 0.044
#&gt; GSM11272     6  0.1493     0.8783 0.000 0.056 0.004 0.000 0.004 0.936
#&gt; GSM11285     4  0.4452     0.4741 0.000 0.004 0.000 0.644 0.312 0.040
#&gt; GSM28753     4  0.3194     0.7304 0.000 0.168 0.000 0.808 0.004 0.020
#&gt; GSM28773     2  0.4218    -0.3237 0.000 0.584 0.000 0.012 0.004 0.400
#&gt; GSM28765     2  0.4736     0.3850 0.000 0.552 0.000 0.000 0.396 0.052
#&gt; GSM28768     1  0.2915     0.7558 0.808 0.184 0.000 0.000 0.008 0.000
#&gt; GSM28754     2  0.3727     0.4177 0.000 0.612 0.000 0.000 0.388 0.000
#&gt; GSM28769     2  0.6323     0.1855 0.000 0.376 0.000 0.244 0.368 0.012
#&gt; GSM11275     1  0.0000     0.9809 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.4026     0.4463 0.000 0.348 0.636 0.000 0.016 0.000
#&gt; GSM11271     5  0.1007     0.7902 0.000 0.044 0.000 0.000 0.956 0.000
#&gt; GSM11288     6  0.0520     0.8728 0.000 0.008 0.000 0.008 0.000 0.984
#&gt; GSM11273     3  0.0713     0.7957 0.000 0.028 0.972 0.000 0.000 0.000
#&gt; GSM28757     2  0.2612     0.5326 0.000 0.868 0.000 0.016 0.108 0.008
#&gt; GSM11282     5  0.6024    -0.2928 0.000 0.368 0.244 0.000 0.388 0.000
#&gt; GSM28756     2  0.3868     0.2800 0.000 0.508 0.000 0.000 0.492 0.000
#&gt; GSM11276     5  0.1141     0.7829 0.000 0.052 0.000 0.000 0.948 0.000
#&gt; GSM28752     5  0.1910     0.7239 0.000 0.108 0.000 0.000 0.892 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:NMF 53     0.397 2
#> MAD:NMF 53     0.373 3
#> MAD:NMF 49     0.348 4
#> MAD:NMF 49     0.413 5
#> MAD:NMF 40     0.410 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.961           0.922       0.972         0.3741 0.648   0.648
#> 3 3 0.937           0.912       0.968         0.4959 0.810   0.707
#> 4 4 1.000           0.973       0.987         0.0986 0.935   0.858
#> 5 5 0.922           0.964       0.971         0.1482 0.895   0.733
#> 6 6 0.951           0.954       0.949         0.0208 0.989   0.961
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2   0.000      0.966 0.000 1.000
#&gt; GSM28763     2   0.000      0.966 0.000 1.000
#&gt; GSM28764     2   0.000      0.966 0.000 1.000
#&gt; GSM11274     2   0.000      0.966 0.000 1.000
#&gt; GSM28772     1   0.000      0.982 1.000 0.000
#&gt; GSM11269     1   0.000      0.982 1.000 0.000
#&gt; GSM28775     1   0.000      0.982 1.000 0.000
#&gt; GSM11293     1   0.000      0.982 1.000 0.000
#&gt; GSM28755     1   0.000      0.982 1.000 0.000
#&gt; GSM11279     1   0.000      0.982 1.000 0.000
#&gt; GSM28758     1   0.000      0.982 1.000 0.000
#&gt; GSM11281     1   0.000      0.982 1.000 0.000
#&gt; GSM11287     1   0.000      0.982 1.000 0.000
#&gt; GSM28759     1   0.000      0.982 1.000 0.000
#&gt; GSM11292     2   0.000      0.966 0.000 1.000
#&gt; GSM28766     2   0.000      0.966 0.000 1.000
#&gt; GSM11268     2   0.000      0.966 0.000 1.000
#&gt; GSM28767     2   0.000      0.966 0.000 1.000
#&gt; GSM11286     2   0.000      0.966 0.000 1.000
#&gt; GSM28751     2   0.992      0.199 0.448 0.552
#&gt; GSM28770     2   0.000      0.966 0.000 1.000
#&gt; GSM11283     2   0.000      0.966 0.000 1.000
#&gt; GSM11289     2   0.000      0.966 0.000 1.000
#&gt; GSM11280     2   0.000      0.966 0.000 1.000
#&gt; GSM28749     2   0.000      0.966 0.000 1.000
#&gt; GSM28750     2   0.000      0.966 0.000 1.000
#&gt; GSM11290     2   0.000      0.966 0.000 1.000
#&gt; GSM11294     2   0.000      0.966 0.000 1.000
#&gt; GSM28771     2   0.000      0.966 0.000 1.000
#&gt; GSM28760     2   0.000      0.966 0.000 1.000
#&gt; GSM28774     2   0.000      0.966 0.000 1.000
#&gt; GSM11284     2   0.000      0.966 0.000 1.000
#&gt; GSM28761     2   0.000      0.966 0.000 1.000
#&gt; GSM11278     2   0.000      0.966 0.000 1.000
#&gt; GSM11291     2   0.000      0.966 0.000 1.000
#&gt; GSM11277     2   0.000      0.966 0.000 1.000
#&gt; GSM11272     2   0.000      0.966 0.000 1.000
#&gt; GSM11285     2   0.000      0.966 0.000 1.000
#&gt; GSM28753     2   0.000      0.966 0.000 1.000
#&gt; GSM28773     2   0.000      0.966 0.000 1.000
#&gt; GSM28765     2   0.000      0.966 0.000 1.000
#&gt; GSM28768     1   0.697      0.752 0.812 0.188
#&gt; GSM28754     2   0.000      0.966 0.000 1.000
#&gt; GSM28769     2   0.992      0.199 0.448 0.552
#&gt; GSM11275     1   0.000      0.982 1.000 0.000
#&gt; GSM11270     2   0.000      0.966 0.000 1.000
#&gt; GSM11271     2   0.000      0.966 0.000 1.000
#&gt; GSM11288     2   0.992      0.199 0.448 0.552
#&gt; GSM11273     2   0.000      0.966 0.000 1.000
#&gt; GSM28757     2   0.000      0.966 0.000 1.000
#&gt; GSM11282     2   0.000      0.966 0.000 1.000
#&gt; GSM28756     2   0.000      0.966 0.000 1.000
#&gt; GSM11276     2   0.000      0.966 0.000 1.000
#&gt; GSM28752     2   0.000      0.966 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11274     2  0.3686      0.803 0.000 0.860 0.140
#&gt; GSM28772     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11268     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28751     2  0.6260      0.208 0.448 0.552 0.000
#&gt; GSM28770     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28749     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28750     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM28771     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28760     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28774     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM11278     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11291     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.993 0.000 0.000 1.000
#&gt; GSM11272     3  0.1411      0.947 0.000 0.036 0.964
#&gt; GSM11285     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28768     1  0.4399      0.705 0.812 0.188 0.000
#&gt; GSM28754     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28769     2  0.6260      0.208 0.448 0.552 0.000
#&gt; GSM11275     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM11270     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11271     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11288     2  0.6260      0.208 0.448 0.552 0.000
#&gt; GSM11273     2  0.0747      0.937 0.000 0.984 0.016
#&gt; GSM28757     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11282     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28756     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.951 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.951 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4
#&gt; GSM28762     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28763     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28764     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11274     2   0.400      0.800 0.00 0.824 0.140 0.036
#&gt; GSM28772     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11269     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM28775     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11293     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM28755     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11279     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM28758     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11281     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11287     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM28759     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11292     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28766     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11268     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM28767     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11286     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28751     4   0.000      1.000 0.00 0.000 0.000 1.000
#&gt; GSM28770     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11283     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11289     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11280     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28749     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28750     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM11290     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM11294     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM28771     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28760     2   0.112      0.963 0.00 0.964 0.000 0.036
#&gt; GSM28774     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11284     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28761     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM11278     2   0.112      0.963 0.00 0.964 0.000 0.036
#&gt; GSM11291     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM11277     3   0.000      0.994 0.00 0.000 1.000 0.000
#&gt; GSM11272     3   0.112      0.954 0.00 0.000 0.964 0.036
#&gt; GSM11285     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28753     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28773     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28765     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28768     1   0.428      0.611 0.72 0.000 0.000 0.280
#&gt; GSM28754     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28769     4   0.000      1.000 0.00 0.000 0.000 1.000
#&gt; GSM11275     1   0.000      0.974 1.00 0.000 0.000 0.000
#&gt; GSM11270     2   0.112      0.963 0.00 0.964 0.000 0.036
#&gt; GSM11271     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11288     4   0.000      1.000 0.00 0.000 0.000 1.000
#&gt; GSM11273     2   0.171      0.949 0.00 0.948 0.016 0.036
#&gt; GSM28757     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11282     2   0.112      0.963 0.00 0.964 0.000 0.036
#&gt; GSM28756     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM11276     2   0.000      0.988 0.00 1.000 0.000 0.000
#&gt; GSM28752     2   0.000      0.988 0.00 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28763     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28764     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11274     5  0.2377      0.722 0.000 0.000 0.128 0.000 0.872
#&gt; GSM28772     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.1270      0.942 0.948 0.000 0.000 0.000 0.052
#&gt; GSM11293     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.1270      0.942 0.948 0.000 0.000 0.000 0.052
#&gt; GSM11279     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.1197      0.944 0.952 0.000 0.000 0.000 0.048
#&gt; GSM11281     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.955 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28766     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11268     3  0.0880      0.973 0.000 0.000 0.968 0.000 0.032
#&gt; GSM28767     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11286     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28751     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28770     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11283     2  0.0510      0.983 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11289     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11280     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28749     2  0.0162      0.994 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28750     3  0.0000      0.984 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11290     3  0.0000      0.984 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.984 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28771     2  0.0703      0.979 0.000 0.976 0.000 0.000 0.024
#&gt; GSM28760     5  0.2377      0.936 0.000 0.128 0.000 0.000 0.872
#&gt; GSM28774     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11284     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28761     3  0.0880      0.973 0.000 0.000 0.968 0.000 0.032
#&gt; GSM11278     5  0.2377      0.940 0.000 0.128 0.000 0.000 0.872
#&gt; GSM11291     3  0.0000      0.984 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.984 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.1544      0.947 0.000 0.000 0.932 0.000 0.068
#&gt; GSM11285     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28753     2  0.0404      0.990 0.000 0.988 0.000 0.000 0.012
#&gt; GSM28773     2  0.0162      0.993 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28765     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28768     1  0.4995      0.587 0.668 0.000 0.000 0.264 0.068
#&gt; GSM28754     2  0.0404      0.990 0.000 0.988 0.000 0.000 0.012
#&gt; GSM28769     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11275     1  0.1197      0.944 0.952 0.000 0.000 0.000 0.048
#&gt; GSM11270     5  0.2377      0.940 0.000 0.128 0.000 0.000 0.872
#&gt; GSM11271     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11288     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11273     5  0.2462      0.928 0.000 0.112 0.008 0.000 0.880
#&gt; GSM28757     2  0.0404      0.990 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11282     5  0.2377      0.940 0.000 0.128 0.000 0.000 0.872
#&gt; GSM28756     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11276     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28752     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28763     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28764     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11274     4  0.2048      0.769 0.000 0.000 0.120 0.880 0.000 0.000
#&gt; GSM28772     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.1564      0.936 0.936 0.000 0.000 0.024 0.000 0.040
#&gt; GSM11293     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.1564      0.936 0.936 0.000 0.000 0.024 0.000 0.040
#&gt; GSM11279     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.1564      0.936 0.936 0.000 0.000 0.024 0.000 0.040
#&gt; GSM11281     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.951 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28766     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11268     6  0.2762      0.912 0.000 0.000 0.196 0.000 0.000 0.804
#&gt; GSM28767     5  0.0146      0.988 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; GSM11286     5  0.0363      0.985 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; GSM28751     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28770     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11283     5  0.0972      0.962 0.000 0.000 0.000 0.028 0.964 0.008
#&gt; GSM11289     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11280     5  0.0363      0.985 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; GSM28749     5  0.0363      0.985 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; GSM28750     6  0.3499      0.787 0.000 0.000 0.320 0.000 0.000 0.680
#&gt; GSM11290     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     5  0.1124      0.959 0.000 0.000 0.000 0.036 0.956 0.008
#&gt; GSM28760     4  0.1863      0.929 0.000 0.000 0.000 0.896 0.104 0.000
#&gt; GSM28774     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11284     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28761     6  0.2730      0.912 0.000 0.000 0.192 0.000 0.000 0.808
#&gt; GSM11278     4  0.1714      0.946 0.000 0.000 0.000 0.908 0.092 0.000
#&gt; GSM11291     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.2048      0.866 0.000 0.000 0.120 0.000 0.000 0.880
#&gt; GSM11285     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28753     5  0.0713      0.976 0.000 0.000 0.000 0.028 0.972 0.000
#&gt; GSM28773     5  0.0146      0.987 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; GSM28765     5  0.0363      0.985 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; GSM28768     1  0.5460      0.604 0.652 0.192 0.000 0.044 0.000 0.112
#&gt; GSM28754     5  0.0790      0.973 0.000 0.000 0.000 0.032 0.968 0.000
#&gt; GSM28769     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11275     1  0.1564      0.936 0.936 0.000 0.000 0.024 0.000 0.040
#&gt; GSM11270     4  0.1714      0.946 0.000 0.000 0.000 0.908 0.092 0.000
#&gt; GSM11271     5  0.0146      0.988 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; GSM11288     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11273     4  0.1501      0.933 0.000 0.000 0.000 0.924 0.076 0.000
#&gt; GSM28757     5  0.1075      0.959 0.000 0.000 0.000 0.048 0.952 0.000
#&gt; GSM11282     4  0.1714      0.946 0.000 0.000 0.000 0.908 0.092 0.000
#&gt; GSM28756     5  0.0632      0.978 0.000 0.000 0.000 0.024 0.976 0.000
#&gt; GSM11276     5  0.0146      0.988 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; GSM28752     5  0.0000      0.989 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:hclust 51     0.395 2
#> ATC:hclust 51     0.371 3
#> ATC:hclust 54     0.355 4
#> ATC:hclust 54     0.337 5
#> ATC:hclust 54     0.322 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.500           0.815       0.847         0.3484 0.648   0.648
#> 3 3 1.000           0.976       0.967         0.5232 0.762   0.645
#> 4 4 0.707           0.717       0.861         0.2532 0.935   0.857
#> 5 5 0.657           0.803       0.861         0.0951 0.867   0.664
#> 6 6 0.734           0.717       0.803         0.0640 0.998   0.992
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2   0.000      0.852 0.000 1.000
#&gt; GSM28763     2   0.000      0.852 0.000 1.000
#&gt; GSM28764     2   0.000      0.852 0.000 1.000
#&gt; GSM11274     2   0.921      0.566 0.336 0.664
#&gt; GSM28772     1   0.921      0.988 0.664 0.336
#&gt; GSM11269     1   0.921      0.988 0.664 0.336
#&gt; GSM28775     1   0.921      0.988 0.664 0.336
#&gt; GSM11293     1   0.921      0.988 0.664 0.336
#&gt; GSM28755     1   0.921      0.988 0.664 0.336
#&gt; GSM11279     1   0.921      0.988 0.664 0.336
#&gt; GSM28758     1   0.921      0.988 0.664 0.336
#&gt; GSM11281     1   0.921      0.988 0.664 0.336
#&gt; GSM11287     1   0.921      0.988 0.664 0.336
#&gt; GSM28759     1   0.921      0.988 0.664 0.336
#&gt; GSM11292     2   0.000      0.852 0.000 1.000
#&gt; GSM28766     2   0.000      0.852 0.000 1.000
#&gt; GSM11268     2   0.985      0.495 0.428 0.572
#&gt; GSM28767     2   0.000      0.852 0.000 1.000
#&gt; GSM11286     2   0.000      0.852 0.000 1.000
#&gt; GSM28751     2   0.000      0.852 0.000 1.000
#&gt; GSM28770     2   0.000      0.852 0.000 1.000
#&gt; GSM11283     2   0.000      0.852 0.000 1.000
#&gt; GSM11289     2   0.000      0.852 0.000 1.000
#&gt; GSM11280     2   0.000      0.852 0.000 1.000
#&gt; GSM28749     2   0.000      0.852 0.000 1.000
#&gt; GSM28750     2   0.985      0.495 0.428 0.572
#&gt; GSM11290     2   0.985      0.495 0.428 0.572
#&gt; GSM11294     2   0.985      0.495 0.428 0.572
#&gt; GSM28771     2   0.000      0.852 0.000 1.000
#&gt; GSM28760     2   0.184      0.832 0.028 0.972
#&gt; GSM28774     2   0.000      0.852 0.000 1.000
#&gt; GSM11284     2   0.000      0.852 0.000 1.000
#&gt; GSM28761     2   0.985      0.495 0.428 0.572
#&gt; GSM11278     2   0.000      0.852 0.000 1.000
#&gt; GSM11291     2   0.985      0.495 0.428 0.572
#&gt; GSM11277     2   0.985      0.495 0.428 0.572
#&gt; GSM11272     2   0.985      0.495 0.428 0.572
#&gt; GSM11285     2   0.000      0.852 0.000 1.000
#&gt; GSM28753     2   0.000      0.852 0.000 1.000
#&gt; GSM28773     2   0.000      0.852 0.000 1.000
#&gt; GSM28765     2   0.000      0.852 0.000 1.000
#&gt; GSM28768     1   0.983      0.851 0.576 0.424
#&gt; GSM28754     2   0.000      0.852 0.000 1.000
#&gt; GSM28769     2   0.000      0.852 0.000 1.000
#&gt; GSM11275     1   0.921      0.988 0.664 0.336
#&gt; GSM11270     2   0.000      0.852 0.000 1.000
#&gt; GSM11271     2   0.000      0.852 0.000 1.000
#&gt; GSM11288     2   0.443      0.742 0.092 0.908
#&gt; GSM11273     2   0.821      0.635 0.256 0.744
#&gt; GSM28757     2   0.000      0.852 0.000 1.000
#&gt; GSM11282     2   0.184      0.832 0.028 0.972
#&gt; GSM28756     2   0.000      0.852 0.000 1.000
#&gt; GSM11276     2   0.000      0.852 0.000 1.000
#&gt; GSM28752     2   0.000      0.852 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28763     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28764     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11274     3  0.2625      0.991 0.000 0.084 0.916
#&gt; GSM28772     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM11269     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM28775     1  0.3181      0.965 0.912 0.024 0.064
#&gt; GSM11293     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM28755     1  0.3181      0.965 0.912 0.024 0.064
#&gt; GSM11279     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM28758     1  0.3181      0.965 0.912 0.024 0.064
#&gt; GSM11281     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM11287     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM28759     1  0.1031      0.980 0.976 0.024 0.000
#&gt; GSM11292     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28766     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11268     3  0.2625      0.991 0.000 0.084 0.916
#&gt; GSM28767     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11286     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28751     2  0.0892      0.971 0.000 0.980 0.020
#&gt; GSM28770     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11283     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11289     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11280     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28749     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28750     3  0.2625      0.991 0.000 0.084 0.916
#&gt; GSM11290     3  0.3637      0.989 0.024 0.084 0.892
#&gt; GSM11294     3  0.3637      0.989 0.024 0.084 0.892
#&gt; GSM28771     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28760     2  0.1031      0.963 0.000 0.976 0.024
#&gt; GSM28774     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28761     3  0.2625      0.991 0.000 0.084 0.916
#&gt; GSM11278     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11291     3  0.3637      0.989 0.024 0.084 0.892
#&gt; GSM11277     3  0.3637      0.989 0.024 0.084 0.892
#&gt; GSM11272     3  0.2625      0.991 0.000 0.084 0.916
#&gt; GSM11285     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28753     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28773     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28765     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28768     2  0.7001      0.617 0.200 0.716 0.084
#&gt; GSM28754     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28769     2  0.0892      0.971 0.000 0.980 0.020
#&gt; GSM11275     1  0.3181      0.965 0.912 0.024 0.064
#&gt; GSM11270     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11271     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11288     2  0.0892      0.971 0.000 0.980 0.020
#&gt; GSM11273     2  0.1031      0.963 0.000 0.976 0.024
#&gt; GSM28757     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11282     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28756     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.987 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.3610      0.541 0.000 0.800 0.000 0.200
#&gt; GSM28763     2  0.3444      0.547 0.000 0.816 0.000 0.184
#&gt; GSM28764     2  0.0921      0.716 0.000 0.972 0.000 0.028
#&gt; GSM11274     3  0.4746      0.641 0.000 0.000 0.632 0.368
#&gt; GSM28772     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.1867      0.958 0.928 0.000 0.000 0.072
#&gt; GSM11293     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.1792      0.959 0.932 0.000 0.000 0.068
#&gt; GSM11279     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.1867      0.958 0.928 0.000 0.000 0.072
#&gt; GSM11281     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.977 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.1940      0.715 0.000 0.924 0.000 0.076
#&gt; GSM28766     2  0.2011      0.715 0.000 0.920 0.000 0.080
#&gt; GSM11268     3  0.1302      0.934 0.000 0.000 0.956 0.044
#&gt; GSM28767     2  0.1022      0.724 0.000 0.968 0.000 0.032
#&gt; GSM11286     2  0.3074      0.625 0.000 0.848 0.000 0.152
#&gt; GSM28751     2  0.5000     -0.664 0.000 0.500 0.000 0.500
#&gt; GSM28770     2  0.1022      0.724 0.000 0.968 0.000 0.032
#&gt; GSM11283     2  0.3074      0.636 0.000 0.848 0.000 0.152
#&gt; GSM11289     2  0.0469      0.722 0.000 0.988 0.000 0.012
#&gt; GSM11280     2  0.3219      0.626 0.000 0.836 0.000 0.164
#&gt; GSM28749     2  0.3726      0.622 0.000 0.788 0.000 0.212
#&gt; GSM28750     3  0.1302      0.934 0.000 0.000 0.956 0.044
#&gt; GSM11290     3  0.0000      0.935 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.935 0.000 0.000 1.000 0.000
#&gt; GSM28771     2  0.3688      0.592 0.000 0.792 0.000 0.208
#&gt; GSM28760     2  0.4898      0.354 0.000 0.584 0.000 0.416
#&gt; GSM28774     2  0.0921      0.725 0.000 0.972 0.000 0.028
#&gt; GSM11284     2  0.1637      0.722 0.000 0.940 0.000 0.060
#&gt; GSM28761     3  0.2081      0.922 0.000 0.000 0.916 0.084
#&gt; GSM11278     2  0.4790      0.416 0.000 0.620 0.000 0.380
#&gt; GSM11291     3  0.0000      0.935 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.935 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.2081      0.922 0.000 0.000 0.916 0.084
#&gt; GSM11285     2  0.3569      0.616 0.000 0.804 0.000 0.196
#&gt; GSM28753     2  0.2647      0.672 0.000 0.880 0.000 0.120
#&gt; GSM28773     2  0.2530      0.699 0.000 0.888 0.000 0.112
#&gt; GSM28765     2  0.2345      0.665 0.000 0.900 0.000 0.100
#&gt; GSM28768     4  0.5671      0.705 0.028 0.400 0.000 0.572
#&gt; GSM28754     2  0.2149      0.696 0.000 0.912 0.000 0.088
#&gt; GSM28769     4  0.4933      0.688 0.000 0.432 0.000 0.568
#&gt; GSM11275     1  0.1867      0.958 0.928 0.000 0.000 0.072
#&gt; GSM11270     2  0.4790      0.416 0.000 0.620 0.000 0.380
#&gt; GSM11271     2  0.1022      0.724 0.000 0.968 0.000 0.032
#&gt; GSM11288     4  0.3975      0.662 0.000 0.240 0.000 0.760
#&gt; GSM11273     2  0.4981      0.258 0.000 0.536 0.000 0.464
#&gt; GSM28757     2  0.3311      0.622 0.000 0.828 0.000 0.172
#&gt; GSM11282     2  0.4817      0.404 0.000 0.612 0.000 0.388
#&gt; GSM28756     2  0.0469      0.724 0.000 0.988 0.000 0.012
#&gt; GSM11276     2  0.0817      0.717 0.000 0.976 0.000 0.024
#&gt; GSM28752     2  0.2814      0.648 0.000 0.868 0.000 0.132
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.4114      0.634 0.000 0.732 0.000 0.244 0.024
#&gt; GSM28763     2  0.3942      0.651 0.000 0.748 0.000 0.232 0.020
#&gt; GSM28764     2  0.0162      0.824 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11274     5  0.4734      0.277 0.000 0.000 0.232 0.064 0.704
#&gt; GSM28772     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.2654      0.917 0.884 0.000 0.000 0.032 0.084
#&gt; GSM11293     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.2654      0.917 0.884 0.000 0.000 0.032 0.084
#&gt; GSM11279     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.3289      0.895 0.844 0.000 0.000 0.048 0.108
#&gt; GSM11281     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.954 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.2338      0.792 0.000 0.884 0.000 0.004 0.112
#&gt; GSM28766     2  0.2338      0.792 0.000 0.884 0.000 0.004 0.112
#&gt; GSM11268     3  0.3620      0.895 0.000 0.000 0.824 0.108 0.068
#&gt; GSM28767     2  0.1121      0.824 0.000 0.956 0.000 0.000 0.044
#&gt; GSM11286     2  0.3530      0.703 0.000 0.784 0.000 0.204 0.012
#&gt; GSM28751     4  0.4315      0.773 0.000 0.276 0.000 0.700 0.024
#&gt; GSM28770     2  0.1121      0.824 0.000 0.956 0.000 0.000 0.044
#&gt; GSM11283     2  0.3702      0.690 0.000 0.820 0.000 0.096 0.084
#&gt; GSM11289     2  0.0404      0.825 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11280     2  0.3993      0.681 0.000 0.756 0.000 0.216 0.028
#&gt; GSM28749     2  0.5341      0.647 0.000 0.664 0.000 0.212 0.124
#&gt; GSM28750     3  0.3493      0.897 0.000 0.000 0.832 0.108 0.060
#&gt; GSM11290     3  0.0000      0.904 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11294     3  0.0000      0.904 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28771     5  0.5821      0.472 0.000 0.400 0.000 0.096 0.504
#&gt; GSM28760     5  0.3123      0.789 0.000 0.184 0.000 0.004 0.812
#&gt; GSM28774     2  0.1282      0.824 0.000 0.952 0.000 0.004 0.044
#&gt; GSM11284     2  0.2338      0.791 0.000 0.884 0.000 0.004 0.112
#&gt; GSM28761     3  0.4428      0.870 0.000 0.000 0.756 0.160 0.084
#&gt; GSM11278     5  0.3461      0.800 0.000 0.224 0.000 0.004 0.772
#&gt; GSM11291     3  0.0000      0.904 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11277     3  0.0000      0.904 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11272     3  0.4428      0.870 0.000 0.000 0.756 0.160 0.084
#&gt; GSM11285     2  0.2763      0.749 0.000 0.848 0.000 0.004 0.148
#&gt; GSM28753     2  0.3409      0.742 0.000 0.816 0.000 0.160 0.024
#&gt; GSM28773     2  0.2563      0.783 0.000 0.872 0.000 0.008 0.120
#&gt; GSM28765     2  0.2612      0.774 0.000 0.868 0.000 0.124 0.008
#&gt; GSM28768     4  0.5711      0.758 0.012 0.224 0.000 0.648 0.116
#&gt; GSM28754     2  0.3012      0.771 0.000 0.852 0.000 0.124 0.024
#&gt; GSM28769     4  0.4708      0.798 0.000 0.220 0.000 0.712 0.068
#&gt; GSM11275     1  0.3289      0.895 0.844 0.000 0.000 0.048 0.108
#&gt; GSM11270     5  0.3461      0.800 0.000 0.224 0.000 0.004 0.772
#&gt; GSM11271     2  0.1121      0.824 0.000 0.956 0.000 0.000 0.044
#&gt; GSM11288     4  0.4234      0.594 0.000 0.056 0.000 0.760 0.184
#&gt; GSM11273     5  0.3399      0.761 0.000 0.168 0.000 0.020 0.812
#&gt; GSM28757     2  0.4224      0.675 0.000 0.744 0.000 0.216 0.040
#&gt; GSM11282     5  0.3461      0.800 0.000 0.224 0.000 0.004 0.772
#&gt; GSM28756     2  0.0693      0.823 0.000 0.980 0.000 0.008 0.012
#&gt; GSM11276     2  0.0162      0.824 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28752     2  0.1648      0.821 0.000 0.940 0.000 0.040 0.020
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     5  0.3817      0.568 0.000 0.432 0.000 0.000 0.568 0.000
#&gt; GSM28763     5  0.3823      0.568 0.000 0.436 0.000 0.000 0.564 0.000
#&gt; GSM28764     5  0.0713      0.759 0.000 0.028 0.000 0.000 0.972 0.000
#&gt; GSM11274     4  0.3073      0.596 0.000 0.004 0.164 0.816 0.000 0.016
#&gt; GSM28772     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.3641      0.755 0.732 0.000 0.000 0.020 0.000 0.248
#&gt; GSM11293     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.3641      0.755 0.732 0.000 0.000 0.020 0.000 0.248
#&gt; GSM11279     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.4078      0.701 0.676 0.016 0.000 0.008 0.000 0.300
#&gt; GSM11281     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.2006      0.724 0.000 0.004 0.000 0.104 0.892 0.000
#&gt; GSM28766     5  0.2006      0.724 0.000 0.004 0.000 0.104 0.892 0.000
#&gt; GSM11268     3  0.1088      0.833 0.000 0.000 0.960 0.024 0.000 0.016
#&gt; GSM28767     5  0.1124      0.756 0.000 0.008 0.000 0.036 0.956 0.000
#&gt; GSM11286     5  0.3659      0.632 0.000 0.364 0.000 0.000 0.636 0.000
#&gt; GSM28751     2  0.4947      0.621 0.000 0.596 0.000 0.000 0.088 0.316
#&gt; GSM28770     5  0.1572      0.752 0.000 0.028 0.000 0.036 0.936 0.000
#&gt; GSM11283     5  0.5673      0.464 0.000 0.344 0.000 0.040 0.544 0.072
#&gt; GSM11289     5  0.0935      0.756 0.000 0.032 0.000 0.004 0.964 0.000
#&gt; GSM11280     5  0.3923      0.589 0.000 0.416 0.000 0.004 0.580 0.000
#&gt; GSM28749     5  0.4746      0.645 0.000 0.236 0.000 0.104 0.660 0.000
#&gt; GSM28750     3  0.0458      0.838 0.000 0.000 0.984 0.016 0.000 0.000
#&gt; GSM11290     3  0.3809      0.844 0.000 0.016 0.756 0.020 0.000 0.208
#&gt; GSM11294     3  0.3809      0.844 0.000 0.016 0.756 0.020 0.000 0.208
#&gt; GSM28771     4  0.7018      0.234 0.000 0.344 0.000 0.364 0.220 0.072
#&gt; GSM28760     4  0.2013      0.809 0.000 0.008 0.000 0.908 0.076 0.008
#&gt; GSM28774     5  0.1196      0.756 0.000 0.008 0.000 0.040 0.952 0.000
#&gt; GSM11284     5  0.1970      0.732 0.000 0.008 0.000 0.092 0.900 0.000
#&gt; GSM28761     3  0.1867      0.819 0.000 0.004 0.924 0.036 0.000 0.036
#&gt; GSM11278     4  0.2053      0.816 0.000 0.004 0.000 0.888 0.108 0.000
#&gt; GSM11291     3  0.3809      0.844 0.000 0.016 0.756 0.020 0.000 0.208
#&gt; GSM11277     3  0.3809      0.844 0.000 0.016 0.756 0.020 0.000 0.208
#&gt; GSM11272     3  0.1867      0.819 0.000 0.004 0.924 0.036 0.000 0.036
#&gt; GSM11285     5  0.2513      0.693 0.000 0.008 0.000 0.140 0.852 0.000
#&gt; GSM28753     5  0.3881      0.611 0.000 0.396 0.000 0.004 0.600 0.000
#&gt; GSM28773     5  0.2302      0.713 0.000 0.008 0.000 0.120 0.872 0.000
#&gt; GSM28765     5  0.3547      0.656 0.000 0.332 0.000 0.000 0.668 0.000
#&gt; GSM28768     6  0.4278      0.000 0.000 0.336 0.000 0.000 0.032 0.632
#&gt; GSM28754     5  0.3819      0.631 0.000 0.372 0.000 0.004 0.624 0.000
#&gt; GSM28769     2  0.5212      0.679 0.000 0.592 0.000 0.024 0.060 0.324
#&gt; GSM11275     1  0.4078      0.701 0.676 0.016 0.000 0.008 0.000 0.300
#&gt; GSM11270     4  0.2053      0.816 0.000 0.004 0.000 0.888 0.108 0.000
#&gt; GSM11271     5  0.0937      0.756 0.000 0.000 0.000 0.040 0.960 0.000
#&gt; GSM11288     2  0.5568      0.424 0.000 0.524 0.000 0.096 0.016 0.364
#&gt; GSM11273     4  0.1801      0.786 0.000 0.004 0.000 0.924 0.056 0.016
#&gt; GSM28757     5  0.3945      0.615 0.000 0.380 0.000 0.008 0.612 0.000
#&gt; GSM11282     4  0.2053      0.816 0.000 0.004 0.000 0.888 0.108 0.000
#&gt; GSM28756     5  0.2772      0.725 0.000 0.180 0.000 0.004 0.816 0.000
#&gt; GSM11276     5  0.0865      0.759 0.000 0.036 0.000 0.000 0.964 0.000
#&gt; GSM28752     5  0.1204      0.756 0.000 0.056 0.000 0.000 0.944 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:kmeans 46     0.389 2
#> ATC:kmeans 54     0.374 3
#> ATC:kmeans 48     0.346 4
#> ATC:kmeans 52     0.334 5
#> ATC:kmeans 50     0.407 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.739           0.857       0.930         0.4576 0.508   0.508
#> 3 3 1.000           0.988       0.995         0.3627 0.810   0.645
#> 4 4 0.747           0.775       0.849         0.1681 0.827   0.561
#> 5 5 0.734           0.577       0.759         0.0647 0.906   0.657
#> 6 6 0.787           0.832       0.889         0.0516 0.916   0.651
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1   p2
#&gt; GSM28762     2   0.000      0.983 0.00 1.00
#&gt; GSM28763     2   0.000      0.983 0.00 1.00
#&gt; GSM28764     2   0.000      0.983 0.00 1.00
#&gt; GSM11274     2   0.000      0.983 0.00 1.00
#&gt; GSM28772     1   0.000      0.823 1.00 0.00
#&gt; GSM11269     1   0.000      0.823 1.00 0.00
#&gt; GSM28775     1   0.000      0.823 1.00 0.00
#&gt; GSM11293     1   0.000      0.823 1.00 0.00
#&gt; GSM28755     1   0.000      0.823 1.00 0.00
#&gt; GSM11279     1   0.000      0.823 1.00 0.00
#&gt; GSM28758     1   0.000      0.823 1.00 0.00
#&gt; GSM11281     1   0.000      0.823 1.00 0.00
#&gt; GSM11287     1   0.000      0.823 1.00 0.00
#&gt; GSM28759     1   0.000      0.823 1.00 0.00
#&gt; GSM11292     2   0.000      0.983 0.00 1.00
#&gt; GSM28766     2   0.000      0.983 0.00 1.00
#&gt; GSM11268     1   0.981      0.513 0.58 0.42
#&gt; GSM28767     2   0.000      0.983 0.00 1.00
#&gt; GSM11286     2   0.000      0.983 0.00 1.00
#&gt; GSM28751     2   0.981      0.203 0.42 0.58
#&gt; GSM28770     2   0.000      0.983 0.00 1.00
#&gt; GSM11283     2   0.000      0.983 0.00 1.00
#&gt; GSM11289     2   0.000      0.983 0.00 1.00
#&gt; GSM11280     2   0.000      0.983 0.00 1.00
#&gt; GSM28749     2   0.000      0.983 0.00 1.00
#&gt; GSM28750     1   0.981      0.513 0.58 0.42
#&gt; GSM11290     1   0.981      0.513 0.58 0.42
#&gt; GSM11294     1   0.981      0.513 0.58 0.42
#&gt; GSM28771     2   0.000      0.983 0.00 1.00
#&gt; GSM28760     2   0.000      0.983 0.00 1.00
#&gt; GSM28774     2   0.000      0.983 0.00 1.00
#&gt; GSM11284     2   0.000      0.983 0.00 1.00
#&gt; GSM28761     1   0.981      0.513 0.58 0.42
#&gt; GSM11278     2   0.000      0.983 0.00 1.00
#&gt; GSM11291     1   0.981      0.513 0.58 0.42
#&gt; GSM11277     1   0.981      0.513 0.58 0.42
#&gt; GSM11272     1   0.981      0.513 0.58 0.42
#&gt; GSM11285     2   0.000      0.983 0.00 1.00
#&gt; GSM28753     2   0.000      0.983 0.00 1.00
#&gt; GSM28773     2   0.000      0.983 0.00 1.00
#&gt; GSM28765     2   0.000      0.983 0.00 1.00
#&gt; GSM28768     1   0.000      0.823 1.00 0.00
#&gt; GSM28754     2   0.000      0.983 0.00 1.00
#&gt; GSM28769     1   0.000      0.823 1.00 0.00
#&gt; GSM11275     1   0.000      0.823 1.00 0.00
#&gt; GSM11270     2   0.000      0.983 0.00 1.00
#&gt; GSM11271     2   0.000      0.983 0.00 1.00
#&gt; GSM11288     1   0.000      0.823 1.00 0.00
#&gt; GSM11273     2   0.000      0.983 0.00 1.00
#&gt; GSM28757     2   0.000      0.983 0.00 1.00
#&gt; GSM11282     2   0.000      0.983 0.00 1.00
#&gt; GSM28756     2   0.000      0.983 0.00 1.00
#&gt; GSM11276     2   0.000      0.983 0.00 1.00
#&gt; GSM28752     2   0.000      0.983 0.00 1.00
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28763     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28764     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11274     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM28772     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11269     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM28775     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11293     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM28755     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11279     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM28758     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11281     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11287     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM28759     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11292     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28766     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11268     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM28767     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11286     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28751     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM28770     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11283     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11289     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11280     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28749     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28750     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11290     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11294     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM28771     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28760     3   0.440      0.751 0.000 0.188 0.812
#&gt; GSM28774     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11284     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28761     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11278     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11291     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11277     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11272     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11285     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28753     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28773     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28765     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28768     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM28754     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28769     1   0.216      0.914 0.936 0.064 0.000
#&gt; GSM11275     1   0.000      0.994 1.000 0.000 0.000
#&gt; GSM11270     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11271     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11288     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM11273     3   0.000      0.979 0.000 0.000 1.000
#&gt; GSM28757     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11282     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28756     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11276     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28752     2   0.000      1.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.1940      0.727 0.000 0.924 0.000 0.076
#&gt; GSM28763     2  0.1940      0.727 0.000 0.924 0.000 0.076
#&gt; GSM28764     2  0.2704      0.672 0.000 0.876 0.000 0.124
#&gt; GSM11274     3  0.3486      0.844 0.000 0.000 0.812 0.188
#&gt; GSM28772     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11292     4  0.4746      0.812 0.000 0.368 0.000 0.632
#&gt; GSM28766     4  0.4746      0.812 0.000 0.368 0.000 0.632
#&gt; GSM11268     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM28767     4  0.4961      0.732 0.000 0.448 0.000 0.552
#&gt; GSM11286     2  0.0592      0.780 0.000 0.984 0.000 0.016
#&gt; GSM28751     2  0.6826      0.378 0.228 0.600 0.000 0.172
#&gt; GSM28770     4  0.4948      0.745 0.000 0.440 0.000 0.560
#&gt; GSM11283     2  0.3444      0.546 0.000 0.816 0.000 0.184
#&gt; GSM11289     2  0.4817     -0.294 0.000 0.612 0.000 0.388
#&gt; GSM11280     2  0.0336      0.780 0.000 0.992 0.000 0.008
#&gt; GSM28749     4  0.4761      0.811 0.000 0.372 0.000 0.628
#&gt; GSM28750     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM11290     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM28771     2  0.4866      0.205 0.000 0.596 0.000 0.404
#&gt; GSM28760     4  0.5199      0.573 0.000 0.144 0.100 0.756
#&gt; GSM28774     4  0.4998      0.628 0.000 0.488 0.000 0.512
#&gt; GSM11284     4  0.4776      0.808 0.000 0.376 0.000 0.624
#&gt; GSM28761     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM11278     4  0.3311      0.689 0.000 0.172 0.000 0.828
#&gt; GSM11291     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.0000      0.959 0.000 0.000 1.000 0.000
#&gt; GSM11285     4  0.4730      0.812 0.000 0.364 0.000 0.636
#&gt; GSM28753     2  0.0469      0.780 0.000 0.988 0.000 0.012
#&gt; GSM28773     4  0.4746      0.812 0.000 0.368 0.000 0.632
#&gt; GSM28765     2  0.0592      0.780 0.000 0.984 0.000 0.016
#&gt; GSM28768     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM28754     2  0.0469      0.781 0.000 0.988 0.000 0.012
#&gt; GSM28769     1  0.9637      0.144 0.356 0.296 0.164 0.184
#&gt; GSM11275     1  0.0000      0.953 1.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.3311      0.689 0.000 0.172 0.000 0.828
#&gt; GSM11271     4  0.4955      0.739 0.000 0.444 0.000 0.556
#&gt; GSM11288     3  0.2281      0.903 0.000 0.000 0.904 0.096
#&gt; GSM11273     3  0.3569      0.838 0.000 0.000 0.804 0.196
#&gt; GSM28757     2  0.0592      0.781 0.000 0.984 0.000 0.016
#&gt; GSM11282     4  0.3311      0.689 0.000 0.172 0.000 0.828
#&gt; GSM28756     2  0.1474      0.765 0.000 0.948 0.000 0.052
#&gt; GSM11276     2  0.1940      0.736 0.000 0.924 0.000 0.076
#&gt; GSM28752     2  0.3569      0.521 0.000 0.804 0.000 0.196
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     4   0.324     0.6121 0.000 0.136 0.000 0.836 0.028
#&gt; GSM28763     4   0.315     0.6163 0.000 0.136 0.000 0.840 0.024
#&gt; GSM28764     4   0.445    -0.1856 0.000 0.476 0.000 0.520 0.004
#&gt; GSM11274     3   0.187     0.2482 0.000 0.052 0.928 0.000 0.020
#&gt; GSM28772     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2   0.311     0.6506 0.000 0.800 0.000 0.200 0.000
#&gt; GSM28766     2   0.311     0.6506 0.000 0.800 0.000 0.200 0.000
#&gt; GSM11268     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM28767     2   0.398     0.5796 0.000 0.660 0.000 0.340 0.000
#&gt; GSM11286     4   0.312     0.6134 0.000 0.184 0.000 0.812 0.004
#&gt; GSM28751     5   0.707     0.5462 0.060 0.136 0.000 0.284 0.520
#&gt; GSM28770     2   0.395     0.5913 0.000 0.668 0.000 0.332 0.000
#&gt; GSM11283     4   0.392     0.5571 0.000 0.180 0.032 0.784 0.004
#&gt; GSM11289     2   0.429     0.3046 0.000 0.536 0.000 0.464 0.000
#&gt; GSM11280     4   0.096     0.6822 0.000 0.016 0.004 0.972 0.008
#&gt; GSM28749     2   0.450     0.6045 0.000 0.664 0.024 0.312 0.000
#&gt; GSM28750     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM11290     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM11294     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM28771     4   0.646     0.1767 0.000 0.212 0.260 0.524 0.004
#&gt; GSM28760     3   0.542    -0.1146 0.000 0.416 0.524 0.060 0.000
#&gt; GSM28774     2   0.421     0.5774 0.000 0.636 0.004 0.360 0.000
#&gt; GSM11284     2   0.376     0.6481 0.000 0.744 0.008 0.248 0.000
#&gt; GSM28761     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM11278     2   0.541     0.0865 0.000 0.472 0.472 0.056 0.000
#&gt; GSM11291     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM11277     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM11272     3   0.430     0.5999 0.000 0.000 0.528 0.000 0.472
#&gt; GSM11285     2   0.352     0.6373 0.000 0.776 0.008 0.216 0.000
#&gt; GSM28753     4   0.154     0.6876 0.000 0.036 0.008 0.948 0.008
#&gt; GSM28773     2   0.386     0.6376 0.000 0.728 0.008 0.264 0.000
#&gt; GSM28765     4   0.273     0.6403 0.000 0.160 0.000 0.840 0.000
#&gt; GSM28768     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28754     4   0.172     0.6889 0.000 0.052 0.004 0.936 0.008
#&gt; GSM28769     5   0.666     0.6099 0.056 0.132 0.000 0.220 0.592
#&gt; GSM11275     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     2   0.541     0.0865 0.000 0.472 0.472 0.056 0.000
#&gt; GSM11271     2   0.402     0.5716 0.000 0.652 0.000 0.348 0.000
#&gt; GSM11288     5   0.269    -0.1801 0.000 0.000 0.156 0.000 0.844
#&gt; GSM11273     3   0.207     0.2280 0.000 0.076 0.912 0.000 0.012
#&gt; GSM28757     4   0.205     0.6827 0.000 0.072 0.004 0.916 0.008
#&gt; GSM11282     3   0.541    -0.2224 0.000 0.472 0.472 0.056 0.000
#&gt; GSM28756     4   0.349     0.5523 0.000 0.228 0.000 0.768 0.004
#&gt; GSM11276     4   0.440    -0.0144 0.000 0.432 0.000 0.564 0.004
#&gt; GSM28752     2   0.444     0.3057 0.000 0.532 0.000 0.464 0.004
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.3147      0.733 0.000 0.844 0.012 0.100 0.044 0.000
#&gt; GSM28763     2  0.2951      0.737 0.000 0.856 0.008 0.092 0.044 0.000
#&gt; GSM28764     5  0.2946      0.767 0.000 0.176 0.000 0.012 0.812 0.000
#&gt; GSM11274     3  0.3547      0.564 0.000 0.000 0.668 0.000 0.000 0.332
#&gt; GSM28772     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.1970      0.812 0.000 0.000 0.092 0.008 0.900 0.000
#&gt; GSM28766     5  0.2020      0.810 0.000 0.000 0.096 0.008 0.896 0.000
#&gt; GSM11268     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     5  0.1950      0.837 0.000 0.064 0.024 0.000 0.912 0.000
#&gt; GSM11286     2  0.3927      0.648 0.000 0.712 0.004 0.024 0.260 0.000
#&gt; GSM28751     4  0.0508      0.974 0.000 0.012 0.000 0.984 0.004 0.000
#&gt; GSM28770     5  0.1982      0.835 0.000 0.068 0.016 0.004 0.912 0.000
#&gt; GSM11283     2  0.4697      0.682 0.000 0.712 0.136 0.012 0.140 0.000
#&gt; GSM11289     5  0.2101      0.820 0.000 0.100 0.004 0.004 0.892 0.000
#&gt; GSM11280     2  0.2394      0.760 0.000 0.900 0.048 0.020 0.032 0.000
#&gt; GSM28749     5  0.5028      0.644 0.000 0.196 0.124 0.012 0.668 0.000
#&gt; GSM28750     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11290     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11294     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28771     2  0.4780      0.375 0.000 0.580 0.372 0.012 0.036 0.000
#&gt; GSM28760     3  0.2395      0.785 0.000 0.012 0.896 0.016 0.072 0.004
#&gt; GSM28774     5  0.3135      0.816 0.000 0.124 0.028 0.012 0.836 0.000
#&gt; GSM11284     5  0.3257      0.816 0.000 0.064 0.084 0.012 0.840 0.000
#&gt; GSM28761     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     3  0.1075      0.817 0.000 0.000 0.952 0.000 0.048 0.000
#&gt; GSM11291     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11277     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11272     6  0.0000      0.950 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11285     5  0.2389      0.798 0.000 0.000 0.128 0.008 0.864 0.000
#&gt; GSM28753     2  0.2407      0.770 0.000 0.892 0.048 0.004 0.056 0.000
#&gt; GSM28773     5  0.4493      0.751 0.000 0.096 0.144 0.020 0.740 0.000
#&gt; GSM28765     2  0.3541      0.712 0.000 0.748 0.000 0.020 0.232 0.000
#&gt; GSM28768     1  0.0146      0.996 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM28754     2  0.2513      0.775 0.000 0.888 0.044 0.008 0.060 0.000
#&gt; GSM28769     4  0.0717      0.974 0.000 0.008 0.000 0.976 0.000 0.016
#&gt; GSM11275     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11270     3  0.1075      0.817 0.000 0.000 0.952 0.000 0.048 0.000
#&gt; GSM11271     5  0.2094      0.838 0.000 0.064 0.024 0.004 0.908 0.000
#&gt; GSM11288     6  0.4426      0.377 0.008 0.004 0.016 0.356 0.000 0.616
#&gt; GSM11273     3  0.3076      0.668 0.000 0.000 0.760 0.000 0.000 0.240
#&gt; GSM28757     2  0.4309      0.740 0.000 0.768 0.044 0.060 0.128 0.000
#&gt; GSM11282     3  0.1204      0.814 0.000 0.000 0.944 0.000 0.056 0.000
#&gt; GSM28756     2  0.3634      0.635 0.000 0.696 0.008 0.000 0.296 0.000
#&gt; GSM11276     5  0.3652      0.638 0.000 0.264 0.000 0.016 0.720 0.000
#&gt; GSM28752     5  0.3232      0.785 0.000 0.160 0.008 0.020 0.812 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> ATC:skmeans 53     0.397 2
#> ATC:skmeans 54     0.374 3
#> ATC:skmeans 50     0.416 4
#> ATC:skmeans 42     0.399 5
#> ATC:skmeans 52     0.391 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3312 0.669   0.669
#> 3 3 1.000           0.973       0.991         0.6192 0.786   0.681
#> 4 4 0.711           0.822       0.801         0.1611 1.000   1.000
#> 5 5 0.671           0.752       0.853         0.1351 0.846   0.666
#> 6 6 0.770           0.777       0.895         0.0874 0.954   0.853
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM28762     2       0          1  0  1
#&gt; GSM28763     2       0          1  0  1
#&gt; GSM28764     2       0          1  0  1
#&gt; GSM11274     2       0          1  0  1
#&gt; GSM28772     1       0          1  1  0
#&gt; GSM11269     1       0          1  1  0
#&gt; GSM28775     1       0          1  1  0
#&gt; GSM11293     1       0          1  1  0
#&gt; GSM28755     1       0          1  1  0
#&gt; GSM11279     1       0          1  1  0
#&gt; GSM28758     1       0          1  1  0
#&gt; GSM11281     1       0          1  1  0
#&gt; GSM11287     1       0          1  1  0
#&gt; GSM28759     1       0          1  1  0
#&gt; GSM11292     2       0          1  0  1
#&gt; GSM28766     2       0          1  0  1
#&gt; GSM11268     2       0          1  0  1
#&gt; GSM28767     2       0          1  0  1
#&gt; GSM11286     2       0          1  0  1
#&gt; GSM28751     2       0          1  0  1
#&gt; GSM28770     2       0          1  0  1
#&gt; GSM11283     2       0          1  0  1
#&gt; GSM11289     2       0          1  0  1
#&gt; GSM11280     2       0          1  0  1
#&gt; GSM28749     2       0          1  0  1
#&gt; GSM28750     2       0          1  0  1
#&gt; GSM11290     2       0          1  0  1
#&gt; GSM11294     2       0          1  0  1
#&gt; GSM28771     2       0          1  0  1
#&gt; GSM28760     2       0          1  0  1
#&gt; GSM28774     2       0          1  0  1
#&gt; GSM11284     2       0          1  0  1
#&gt; GSM28761     2       0          1  0  1
#&gt; GSM11278     2       0          1  0  1
#&gt; GSM11291     2       0          1  0  1
#&gt; GSM11277     2       0          1  0  1
#&gt; GSM11272     2       0          1  0  1
#&gt; GSM11285     2       0          1  0  1
#&gt; GSM28753     2       0          1  0  1
#&gt; GSM28773     2       0          1  0  1
#&gt; GSM28765     2       0          1  0  1
#&gt; GSM28768     2       0          1  0  1
#&gt; GSM28754     2       0          1  0  1
#&gt; GSM28769     2       0          1  0  1
#&gt; GSM11275     1       0          1  1  0
#&gt; GSM11270     2       0          1  0  1
#&gt; GSM11271     2       0          1  0  1
#&gt; GSM11288     2       0          1  0  1
#&gt; GSM11273     2       0          1  0  1
#&gt; GSM28757     2       0          1  0  1
#&gt; GSM11282     2       0          1  0  1
#&gt; GSM28756     2       0          1  0  1
#&gt; GSM11276     2       0          1  0  1
#&gt; GSM28752     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3
#&gt; GSM28762     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28763     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28764     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11274     3   0.621      0.263  0 0.428 0.572
#&gt; GSM28772     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11269     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28775     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11293     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28755     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11279     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28758     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11281     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11287     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28759     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11292     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28766     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11268     3   0.000      0.913  0 0.000 1.000
#&gt; GSM28767     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11286     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28751     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28770     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11283     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11289     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11280     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28749     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28750     3   0.000      0.913  0 0.000 1.000
#&gt; GSM11290     3   0.000      0.913  0 0.000 1.000
#&gt; GSM11294     3   0.000      0.913  0 0.000 1.000
#&gt; GSM28771     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28760     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28774     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11284     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28761     3   0.000      0.913  0 0.000 1.000
#&gt; GSM11278     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11291     3   0.000      0.913  0 0.000 1.000
#&gt; GSM11277     3   0.000      0.913  0 0.000 1.000
#&gt; GSM11272     3   0.153      0.874  0 0.040 0.960
#&gt; GSM11285     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28753     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28773     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28765     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28768     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28754     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28769     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11275     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11270     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11271     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11288     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11273     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28757     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11282     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28756     2   0.000      1.000  0 1.000 0.000
#&gt; GSM11276     2   0.000      1.000  0 1.000 0.000
#&gt; GSM28752     2   0.000      1.000  0 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4
#&gt; GSM28762     2  0.0336      0.858  0 0.992 0.000 0.008
#&gt; GSM28763     2  0.0336      0.858  0 0.992 0.000 0.008
#&gt; GSM28764     2  0.0000      0.860  0 1.000 0.000 0.000
#&gt; GSM11274     3  0.7500      0.246  0 0.180 0.416 0.404
#&gt; GSM28772     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11292     2  0.3486      0.829  0 0.812 0.000 0.188
#&gt; GSM28766     2  0.3837      0.815  0 0.776 0.000 0.224
#&gt; GSM11268     3  0.0000      0.710  0 0.000 1.000 0.000
#&gt; GSM28767     2  0.2408      0.854  0 0.896 0.000 0.104
#&gt; GSM11286     2  0.0000      0.860  0 1.000 0.000 0.000
#&gt; GSM28751     2  0.2345      0.805  0 0.900 0.000 0.100
#&gt; GSM28770     2  0.2868      0.846  0 0.864 0.000 0.136
#&gt; GSM11283     2  0.3400      0.832  0 0.820 0.000 0.180
#&gt; GSM11289     2  0.0707      0.861  0 0.980 0.000 0.020
#&gt; GSM11280     2  0.0336      0.858  0 0.992 0.000 0.008
#&gt; GSM28749     2  0.2149      0.859  0 0.912 0.000 0.088
#&gt; GSM28750     3  0.1637      0.719  0 0.000 0.940 0.060
#&gt; GSM11290     3  0.5000      0.724  0 0.000 0.504 0.496
#&gt; GSM11294     3  0.5000      0.724  0 0.000 0.504 0.496
#&gt; GSM28771     2  0.3837      0.815  0 0.776 0.000 0.224
#&gt; GSM28760     2  0.4866      0.679  0 0.596 0.000 0.404
#&gt; GSM28774     2  0.0188      0.860  0 0.996 0.000 0.004
#&gt; GSM11284     2  0.3400      0.832  0 0.820 0.000 0.180
#&gt; GSM28761     3  0.0000      0.710  0 0.000 1.000 0.000
#&gt; GSM11278     2  0.4866      0.679  0 0.596 0.000 0.404
#&gt; GSM11291     3  0.5000      0.724  0 0.000 0.504 0.496
#&gt; GSM11277     3  0.5000      0.724  0 0.000 0.504 0.496
#&gt; GSM11272     3  0.4158      0.606  0 0.008 0.768 0.224
#&gt; GSM11285     2  0.3801      0.817  0 0.780 0.000 0.220
#&gt; GSM28753     2  0.0000      0.860  0 1.000 0.000 0.000
#&gt; GSM28773     2  0.3400      0.832  0 0.820 0.000 0.180
#&gt; GSM28765     2  0.0000      0.860  0 1.000 0.000 0.000
#&gt; GSM28768     2  0.2345      0.805  0 0.900 0.000 0.100
#&gt; GSM28754     2  0.1118      0.855  0 0.964 0.000 0.036
#&gt; GSM28769     2  0.4522      0.649  0 0.680 0.000 0.320
#&gt; GSM11275     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11270     2  0.4877      0.677  0 0.592 0.000 0.408
#&gt; GSM11271     2  0.2011      0.858  0 0.920 0.000 0.080
#&gt; GSM11288     2  0.4830      0.637  0 0.608 0.000 0.392
#&gt; GSM11273     2  0.4866      0.679  0 0.596 0.000 0.404
#&gt; GSM28757     2  0.0469      0.858  0 0.988 0.000 0.012
#&gt; GSM11282     2  0.4866      0.679  0 0.596 0.000 0.404
#&gt; GSM28756     2  0.0000      0.860  0 1.000 0.000 0.000
#&gt; GSM11276     2  0.0000      0.860  0 1.000 0.000 0.000
#&gt; GSM28752     2  0.0000      0.860  0 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.3246      0.632 0.000 0.808 0.008 0.000 0.184
#&gt; GSM28763     2  0.3246      0.632 0.000 0.808 0.008 0.000 0.184
#&gt; GSM28764     2  0.0000      0.757 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11274     5  0.7511      0.599 0.000 0.156 0.172 0.144 0.528
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0162      0.997 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.4636      0.602 0.000 0.744 0.000 0.124 0.132
#&gt; GSM28766     2  0.4833      0.590 0.000 0.736 0.004 0.124 0.136
#&gt; GSM11268     3  0.1121      0.904 0.000 0.000 0.956 0.044 0.000
#&gt; GSM28767     2  0.2915      0.701 0.000 0.860 0.000 0.024 0.116
#&gt; GSM11286     2  0.0703      0.752 0.000 0.976 0.000 0.000 0.024
#&gt; GSM28751     2  0.4826      0.239 0.000 0.508 0.020 0.000 0.472
#&gt; GSM28770     2  0.3780      0.667 0.000 0.812 0.000 0.072 0.116
#&gt; GSM11283     2  0.4593      0.607 0.000 0.748 0.000 0.124 0.128
#&gt; GSM11289     2  0.0880      0.750 0.000 0.968 0.000 0.000 0.032
#&gt; GSM11280     2  0.3246      0.632 0.000 0.808 0.008 0.000 0.184
#&gt; GSM28749     2  0.2773      0.723 0.000 0.836 0.000 0.000 0.164
#&gt; GSM28750     3  0.3210      0.728 0.000 0.000 0.788 0.212 0.000
#&gt; GSM11290     4  0.2424      1.000 0.000 0.000 0.132 0.868 0.000
#&gt; GSM11294     4  0.2424      1.000 0.000 0.000 0.132 0.868 0.000
#&gt; GSM28771     2  0.4833      0.590 0.000 0.736 0.004 0.124 0.136
#&gt; GSM28760     5  0.6485      0.698 0.000 0.324 0.020 0.128 0.528
#&gt; GSM28774     2  0.0955      0.754 0.000 0.968 0.004 0.000 0.028
#&gt; GSM11284     2  0.4593      0.607 0.000 0.748 0.000 0.124 0.128
#&gt; GSM28761     3  0.1121      0.904 0.000 0.000 0.956 0.044 0.000
#&gt; GSM11278     5  0.6552      0.703 0.000 0.320 0.024 0.128 0.528
#&gt; GSM11291     4  0.2424      1.000 0.000 0.000 0.132 0.868 0.000
#&gt; GSM11277     4  0.2424      1.000 0.000 0.000 0.132 0.868 0.000
#&gt; GSM11272     3  0.0609      0.870 0.000 0.000 0.980 0.000 0.020
#&gt; GSM11285     2  0.4679      0.596 0.000 0.740 0.000 0.124 0.136
#&gt; GSM28753     2  0.0000      0.757 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28773     2  0.4593      0.607 0.000 0.748 0.000 0.124 0.128
#&gt; GSM28765     2  0.0703      0.752 0.000 0.976 0.000 0.000 0.024
#&gt; GSM28768     2  0.4971      0.235 0.000 0.504 0.020 0.004 0.472
#&gt; GSM28754     2  0.1671      0.725 0.000 0.924 0.000 0.000 0.076
#&gt; GSM28769     5  0.2969      0.214 0.000 0.128 0.020 0.000 0.852
#&gt; GSM11275     1  0.0162      0.997 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11270     5  0.6102      0.686 0.000 0.224 0.024 0.128 0.624
#&gt; GSM11271     2  0.2127      0.719 0.000 0.892 0.000 0.000 0.108
#&gt; GSM11288     5  0.1908      0.380 0.000 0.092 0.000 0.000 0.908
#&gt; GSM11273     5  0.6552      0.703 0.000 0.320 0.024 0.128 0.528
#&gt; GSM28757     2  0.3282      0.629 0.000 0.804 0.008 0.000 0.188
#&gt; GSM11282     5  0.6552      0.703 0.000 0.320 0.024 0.128 0.528
#&gt; GSM28756     2  0.0000      0.757 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11276     2  0.0000      0.757 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28752     2  0.0703      0.752 0.000 0.976 0.000 0.000 0.024
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     5  0.3864      0.169 0.000 0.480 0.000 0.000 0.520 0.000
#&gt; GSM28763     5  0.3864      0.169 0.000 0.480 0.000 0.000 0.520 0.000
#&gt; GSM28764     5  0.0000      0.773 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11274     4  0.0291      0.918 0.000 0.000 0.004 0.992 0.000 0.004
#&gt; GSM28772     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0146      0.997 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.3023      0.680 0.000 0.000 0.000 0.232 0.768 0.000
#&gt; GSM28766     5  0.3023      0.680 0.000 0.000 0.000 0.232 0.768 0.000
#&gt; GSM11268     6  0.0000      0.890 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28767     5  0.1501      0.761 0.000 0.000 0.000 0.076 0.924 0.000
#&gt; GSM11286     5  0.1556      0.750 0.000 0.080 0.000 0.000 0.920 0.000
#&gt; GSM28751     2  0.0146      0.813 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28770     5  0.2491      0.723 0.000 0.000 0.000 0.164 0.836 0.000
#&gt; GSM11283     5  0.3023      0.680 0.000 0.000 0.000 0.232 0.768 0.000
#&gt; GSM11289     5  0.0000      0.773 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11280     5  0.3864      0.169 0.000 0.480 0.000 0.000 0.520 0.000
#&gt; GSM28749     5  0.3588      0.718 0.000 0.152 0.000 0.060 0.788 0.000
#&gt; GSM28750     6  0.3464      0.547 0.000 0.000 0.312 0.000 0.000 0.688
#&gt; GSM11290     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     5  0.3050      0.676 0.000 0.000 0.000 0.236 0.764 0.000
#&gt; GSM28760     4  0.2793      0.642 0.000 0.000 0.000 0.800 0.200 0.000
#&gt; GSM28774     5  0.4244      0.588 0.000 0.080 0.000 0.200 0.720 0.000
#&gt; GSM11284     5  0.3023      0.680 0.000 0.000 0.000 0.232 0.768 0.000
#&gt; GSM28761     6  0.0000      0.890 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11278     4  0.0000      0.925 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11291     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.0000      0.890 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11285     5  0.3023      0.680 0.000 0.000 0.000 0.232 0.768 0.000
#&gt; GSM28753     5  0.0146      0.772 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28773     5  0.3023      0.680 0.000 0.000 0.000 0.232 0.768 0.000
#&gt; GSM28765     5  0.1556      0.750 0.000 0.080 0.000 0.000 0.920 0.000
#&gt; GSM28768     2  0.0000      0.811 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28754     5  0.2094      0.744 0.000 0.080 0.000 0.020 0.900 0.000
#&gt; GSM28769     2  0.0146      0.813 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11275     1  0.0146      0.997 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; GSM11270     4  0.0000      0.925 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11271     5  0.1075      0.769 0.000 0.000 0.000 0.048 0.952 0.000
#&gt; GSM11288     2  0.3986      0.193 0.000 0.532 0.000 0.464 0.004 0.000
#&gt; GSM11273     4  0.0000      0.925 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28757     5  0.3864      0.169 0.000 0.480 0.000 0.000 0.520 0.000
#&gt; GSM11282     4  0.0000      0.925 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28756     5  0.0000      0.773 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11276     5  0.0000      0.773 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28752     5  0.1556      0.750 0.000 0.080 0.000 0.000 0.920 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:pam 54     0.398 2
#> ATC:pam 53     0.373 3
#> ATC:pam 53     0.373 4
#> ATC:pam 50     0.331 5
#> ATC:pam 49     0.314 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.704           0.887       0.943         0.4214 0.591   0.591
#> 3 3 0.764           0.884       0.942         0.4623 0.797   0.657
#> 4 4 0.687           0.580       0.793         0.1721 0.828   0.591
#> 5 5 0.716           0.738       0.827         0.0279 0.955   0.847
#> 6 6 0.729           0.780       0.822         0.0276 0.878   0.592
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.3879      0.886 0.076 0.924
#&gt; GSM28763     2  0.3879      0.886 0.076 0.924
#&gt; GSM28764     2  0.0000      0.937 0.000 1.000
#&gt; GSM11274     2  0.0000      0.937 0.000 1.000
#&gt; GSM28772     1  0.0000      0.926 1.000 0.000
#&gt; GSM11269     1  0.0000      0.926 1.000 0.000
#&gt; GSM28775     1  0.0000      0.926 1.000 0.000
#&gt; GSM11293     1  0.0000      0.926 1.000 0.000
#&gt; GSM28755     1  0.0000      0.926 1.000 0.000
#&gt; GSM11279     1  0.0000      0.926 1.000 0.000
#&gt; GSM28758     1  0.0000      0.926 1.000 0.000
#&gt; GSM11281     1  0.0000      0.926 1.000 0.000
#&gt; GSM11287     1  0.0000      0.926 1.000 0.000
#&gt; GSM28759     1  0.0000      0.926 1.000 0.000
#&gt; GSM11292     2  0.0672      0.935 0.008 0.992
#&gt; GSM28766     2  0.2236      0.919 0.036 0.964
#&gt; GSM11268     2  0.7674      0.754 0.224 0.776
#&gt; GSM28767     2  0.0672      0.935 0.008 0.992
#&gt; GSM11286     2  0.2043      0.922 0.032 0.968
#&gt; GSM28751     1  0.8207      0.705 0.744 0.256
#&gt; GSM28770     2  0.0000      0.937 0.000 1.000
#&gt; GSM11283     2  0.0000      0.937 0.000 1.000
#&gt; GSM11289     2  0.0000      0.937 0.000 1.000
#&gt; GSM11280     2  0.0672      0.935 0.008 0.992
#&gt; GSM28749     2  0.1843      0.925 0.028 0.972
#&gt; GSM28750     2  0.7674      0.754 0.224 0.776
#&gt; GSM11290     2  0.7674      0.754 0.224 0.776
#&gt; GSM11294     2  0.7674      0.754 0.224 0.776
#&gt; GSM28771     2  0.0000      0.937 0.000 1.000
#&gt; GSM28760     2  0.0000      0.937 0.000 1.000
#&gt; GSM28774     2  0.0000      0.937 0.000 1.000
#&gt; GSM11284     2  0.0672      0.935 0.008 0.992
#&gt; GSM28761     2  0.7674      0.754 0.224 0.776
#&gt; GSM11278     2  0.0000      0.937 0.000 1.000
#&gt; GSM11291     2  0.7674      0.754 0.224 0.776
#&gt; GSM11277     2  0.7674      0.754 0.224 0.776
#&gt; GSM11272     2  0.7674      0.754 0.224 0.776
#&gt; GSM11285     2  0.1414      0.929 0.020 0.980
#&gt; GSM28753     2  0.0000      0.937 0.000 1.000
#&gt; GSM28773     2  0.0000      0.937 0.000 1.000
#&gt; GSM28765     2  0.1843      0.925 0.028 0.972
#&gt; GSM28768     1  0.5946      0.818 0.856 0.144
#&gt; GSM28754     2  0.0000      0.937 0.000 1.000
#&gt; GSM28769     1  0.8861      0.629 0.696 0.304
#&gt; GSM11275     1  0.0000      0.926 1.000 0.000
#&gt; GSM11270     2  0.0000      0.937 0.000 1.000
#&gt; GSM11271     2  0.0000      0.937 0.000 1.000
#&gt; GSM11288     1  0.8386      0.676 0.732 0.268
#&gt; GSM11273     2  0.0000      0.937 0.000 1.000
#&gt; GSM28757     2  0.0672      0.935 0.008 0.992
#&gt; GSM11282     2  0.0000      0.937 0.000 1.000
#&gt; GSM28756     2  0.0000      0.937 0.000 1.000
#&gt; GSM11276     2  0.0000      0.937 0.000 1.000
#&gt; GSM28752     2  0.0000      0.937 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28762     2  0.1529      0.923 0.000 0.960 0.040
#&gt; GSM28763     2  0.1529      0.923 0.000 0.960 0.040
#&gt; GSM28764     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11274     3  0.3879      0.802 0.000 0.152 0.848
#&gt; GSM28772     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11269     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM28755     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11279     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11292     2  0.1529      0.913 0.040 0.960 0.000
#&gt; GSM28766     2  0.1289      0.918 0.032 0.968 0.000
#&gt; GSM11268     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM28767     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11286     2  0.0747      0.931 0.000 0.984 0.016
#&gt; GSM28751     1  0.6722      0.640 0.720 0.220 0.060
#&gt; GSM28770     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11283     2  0.5431      0.678 0.000 0.716 0.284
#&gt; GSM11289     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11280     2  0.1031      0.929 0.000 0.976 0.024
#&gt; GSM28749     2  0.4799      0.824 0.132 0.836 0.032
#&gt; GSM28750     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM11290     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM11294     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM28771     2  0.5431      0.678 0.000 0.716 0.284
#&gt; GSM28760     2  0.5529      0.657 0.000 0.704 0.296
#&gt; GSM28774     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11284     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM28761     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM11278     2  0.4002      0.833 0.000 0.840 0.160
#&gt; GSM11291     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM11277     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM11272     3  0.0000      0.946 0.000 0.000 1.000
#&gt; GSM11285     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM28753     2  0.1289      0.927 0.000 0.968 0.032
#&gt; GSM28773     2  0.0237      0.931 0.000 0.996 0.004
#&gt; GSM28765     2  0.1031      0.929 0.000 0.976 0.024
#&gt; GSM28768     1  0.0747      0.915 0.984 0.016 0.000
#&gt; GSM28754     2  0.1163      0.928 0.000 0.972 0.028
#&gt; GSM28769     1  0.7739      0.605 0.672 0.204 0.124
#&gt; GSM11275     1  0.0000      0.927 1.000 0.000 0.000
#&gt; GSM11270     2  0.4178      0.821 0.000 0.828 0.172
#&gt; GSM11271     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11288     1  0.6684      0.584 0.676 0.032 0.292
#&gt; GSM11273     3  0.4605      0.728 0.000 0.204 0.796
#&gt; GSM28757     2  0.0892      0.930 0.000 0.980 0.020
#&gt; GSM11282     2  0.4399      0.803 0.000 0.812 0.188
#&gt; GSM28756     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM11276     2  0.0000      0.931 0.000 1.000 0.000
#&gt; GSM28752     2  0.0000      0.931 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     2  0.1398   0.832463 0.000 0.956 0.004 0.040
#&gt; GSM28763     2  0.1716   0.826046 0.000 0.936 0.000 0.064
#&gt; GSM28764     2  0.1940   0.815352 0.000 0.924 0.000 0.076
#&gt; GSM11274     4  0.5862  -0.222011 0.000 0.032 0.484 0.484
#&gt; GSM28772     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0188   0.996237 0.996 0.000 0.000 0.004
#&gt; GSM28775     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0336   0.993874 0.992 0.000 0.000 0.008
#&gt; GSM11287     1  0.0188   0.996237 0.996 0.000 0.000 0.004
#&gt; GSM28759     1  0.0000   0.997530 1.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.4907   0.383787 0.000 0.580 0.000 0.420
#&gt; GSM28766     2  0.4916   0.380179 0.000 0.576 0.000 0.424
#&gt; GSM11268     3  0.1557   0.555601 0.000 0.000 0.944 0.056
#&gt; GSM28767     2  0.0188   0.840163 0.000 0.996 0.000 0.004
#&gt; GSM11286     2  0.1389   0.828733 0.000 0.952 0.000 0.048
#&gt; GSM28751     4  0.6343   0.106161 0.068 0.332 0.004 0.596
#&gt; GSM28770     2  0.0592   0.839680 0.000 0.984 0.000 0.016
#&gt; GSM11283     3  0.6506  -0.000593 0.000 0.072 0.468 0.460
#&gt; GSM11289     2  0.1867   0.818503 0.000 0.928 0.000 0.072
#&gt; GSM11280     2  0.1867   0.821052 0.000 0.928 0.000 0.072
#&gt; GSM28749     2  0.5155   0.280574 0.000 0.528 0.004 0.468
#&gt; GSM28750     3  0.0707   0.579434 0.000 0.000 0.980 0.020
#&gt; GSM11290     3  0.0000   0.584828 0.000 0.000 1.000 0.000
#&gt; GSM11294     3  0.0000   0.584828 0.000 0.000 1.000 0.000
#&gt; GSM28771     4  0.6396  -0.194619 0.000 0.064 0.468 0.468
#&gt; GSM28760     4  0.6826  -0.140021 0.000 0.100 0.416 0.484
#&gt; GSM28774     2  0.0188   0.840163 0.000 0.996 0.000 0.004
#&gt; GSM11284     2  0.0592   0.839115 0.000 0.984 0.000 0.016
#&gt; GSM28761     3  0.4907   0.211987 0.000 0.000 0.580 0.420
#&gt; GSM11278     3  0.7779   0.004912 0.000 0.244 0.400 0.356
#&gt; GSM11291     3  0.0000   0.584828 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000   0.584828 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.4907   0.211987 0.000 0.000 0.580 0.420
#&gt; GSM11285     2  0.0817   0.839648 0.000 0.976 0.000 0.024
#&gt; GSM28753     2  0.3024   0.743140 0.000 0.852 0.000 0.148
#&gt; GSM28773     2  0.1302   0.832453 0.000 0.956 0.000 0.044
#&gt; GSM28765     2  0.0188   0.840697 0.000 0.996 0.000 0.004
#&gt; GSM28768     4  0.6055   0.035554 0.372 0.052 0.000 0.576
#&gt; GSM28754     2  0.4158   0.639519 0.000 0.768 0.008 0.224
#&gt; GSM28769     4  0.6605   0.151448 0.020 0.316 0.060 0.604
#&gt; GSM11275     1  0.0469   0.991252 0.988 0.000 0.000 0.012
#&gt; GSM11270     3  0.7629  -0.000801 0.000 0.204 0.400 0.396
#&gt; GSM11271     2  0.0000   0.840312 0.000 1.000 0.000 0.000
#&gt; GSM11288     4  0.7160   0.071964 0.020 0.104 0.300 0.576
#&gt; GSM11273     3  0.5862   0.030039 0.000 0.032 0.484 0.484
#&gt; GSM28757     2  0.3837   0.670370 0.000 0.776 0.000 0.224
#&gt; GSM11282     4  0.7030  -0.131900 0.000 0.120 0.408 0.472
#&gt; GSM28756     2  0.1118   0.837004 0.000 0.964 0.000 0.036
#&gt; GSM11276     2  0.1118   0.835951 0.000 0.964 0.000 0.036
#&gt; GSM28752     2  0.4999   0.320866 0.000 0.508 0.000 0.492
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     2  0.2358      0.811 0.000 0.888 0.000 0.008 0.104
#&gt; GSM28763     2  0.1697      0.813 0.000 0.932 0.000 0.008 0.060
#&gt; GSM28764     2  0.1205      0.807 0.000 0.956 0.000 0.040 0.004
#&gt; GSM11274     4  0.2690      0.623 0.000 0.156 0.000 0.844 0.000
#&gt; GSM28772     1  0.0162      0.980 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11269     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0510      0.970 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11293     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0162      0.980 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11279     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000      0.982 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.5228      0.507 0.000 0.588 0.000 0.056 0.356
#&gt; GSM28766     2  0.5228      0.499 0.000 0.588 0.000 0.056 0.356
#&gt; GSM11268     3  0.3728      0.700 0.000 0.000 0.748 0.008 0.244
#&gt; GSM28767     2  0.1041      0.820 0.000 0.964 0.000 0.004 0.032
#&gt; GSM11286     2  0.3728      0.699 0.000 0.748 0.000 0.008 0.244
#&gt; GSM28751     5  0.4452      0.639 0.040 0.100 0.000 0.064 0.796
#&gt; GSM28770     2  0.1851      0.803 0.000 0.912 0.000 0.000 0.088
#&gt; GSM11283     4  0.1725      0.452 0.000 0.044 0.000 0.936 0.020
#&gt; GSM11289     2  0.1211      0.814 0.000 0.960 0.000 0.024 0.016
#&gt; GSM11280     2  0.1701      0.810 0.000 0.936 0.000 0.016 0.048
#&gt; GSM28749     2  0.5456      0.255 0.000 0.484 0.000 0.060 0.456
#&gt; GSM28750     3  0.2612      0.751 0.000 0.000 0.868 0.008 0.124
#&gt; GSM11290     3  0.1908      0.783 0.000 0.000 0.908 0.092 0.000
#&gt; GSM11294     3  0.1965      0.783 0.000 0.000 0.904 0.096 0.000
#&gt; GSM28771     4  0.0771      0.444 0.000 0.004 0.000 0.976 0.020
#&gt; GSM28760     4  0.4015      0.677 0.000 0.348 0.000 0.652 0.000
#&gt; GSM28774     2  0.1251      0.821 0.000 0.956 0.000 0.008 0.036
#&gt; GSM11284     2  0.2605      0.768 0.000 0.852 0.000 0.000 0.148
#&gt; GSM28761     3  0.4238      0.576 0.000 0.000 0.628 0.004 0.368
#&gt; GSM11278     4  0.5088      0.596 0.000 0.436 0.000 0.528 0.036
#&gt; GSM11291     3  0.1965      0.783 0.000 0.000 0.904 0.096 0.000
#&gt; GSM11277     3  0.1965      0.783 0.000 0.000 0.904 0.096 0.000
#&gt; GSM11272     3  0.4238      0.576 0.000 0.000 0.628 0.004 0.368
#&gt; GSM11285     2  0.1493      0.806 0.000 0.948 0.000 0.028 0.024
#&gt; GSM28753     2  0.2446      0.786 0.000 0.900 0.000 0.056 0.044
#&gt; GSM28773     2  0.1750      0.801 0.000 0.936 0.000 0.028 0.036
#&gt; GSM28765     2  0.1597      0.816 0.000 0.940 0.000 0.012 0.048
#&gt; GSM28768     5  0.6128      0.293 0.388 0.032 0.000 0.060 0.520
#&gt; GSM28754     2  0.2520      0.789 0.000 0.896 0.000 0.048 0.056
#&gt; GSM28769     5  0.3223      0.634 0.000 0.052 0.016 0.064 0.868
#&gt; GSM11275     1  0.2424      0.822 0.868 0.000 0.000 0.000 0.132
#&gt; GSM11270     4  0.5077      0.609 0.000 0.428 0.000 0.536 0.036
#&gt; GSM11271     2  0.1124      0.820 0.000 0.960 0.000 0.004 0.036
#&gt; GSM11288     5  0.5428      0.268 0.000 0.024 0.248 0.060 0.668
#&gt; GSM11273     4  0.3242      0.656 0.000 0.216 0.000 0.784 0.000
#&gt; GSM28757     2  0.3910      0.684 0.000 0.720 0.000 0.008 0.272
#&gt; GSM11282     4  0.5153      0.585 0.000 0.436 0.000 0.524 0.040
#&gt; GSM28756     2  0.0693      0.816 0.000 0.980 0.000 0.012 0.008
#&gt; GSM11276     2  0.1211      0.814 0.000 0.960 0.000 0.024 0.016
#&gt; GSM28752     2  0.4109      0.662 0.000 0.700 0.000 0.012 0.288
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     5  0.3535     0.8178 0.000 0.040 0.000 0.060 0.832 0.068
#&gt; GSM28763     5  0.3063     0.8381 0.000 0.024 0.000 0.064 0.860 0.052
#&gt; GSM28764     5  0.0458     0.8658 0.000 0.000 0.000 0.016 0.984 0.000
#&gt; GSM11274     4  0.1738     0.6446 0.000 0.000 0.004 0.928 0.052 0.016
#&gt; GSM28772     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0858     0.9574 0.968 0.028 0.000 0.000 0.000 0.004
#&gt; GSM11293     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0260     0.9742 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; GSM11279     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28758     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9789 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.4534     0.6750 0.000 0.612 0.000 0.020 0.352 0.016
#&gt; GSM28766     2  0.4468     0.6728 0.000 0.612 0.000 0.016 0.356 0.016
#&gt; GSM11268     6  0.3482     0.9403 0.000 0.000 0.316 0.000 0.000 0.684
#&gt; GSM28767     5  0.0291     0.8637 0.000 0.004 0.000 0.004 0.992 0.000
#&gt; GSM11286     5  0.4546     0.4176 0.000 0.240 0.000 0.028 0.696 0.036
#&gt; GSM28751     2  0.3594     0.6334 0.000 0.796 0.000 0.008 0.152 0.044
#&gt; GSM28770     5  0.0713     0.8538 0.000 0.028 0.000 0.000 0.972 0.000
#&gt; GSM11283     4  0.4771     0.6223 0.000 0.044 0.000 0.712 0.056 0.188
#&gt; GSM11289     5  0.0260     0.8648 0.000 0.000 0.000 0.008 0.992 0.000
#&gt; GSM11280     5  0.2602     0.8458 0.000 0.024 0.000 0.072 0.884 0.020
#&gt; GSM28749     2  0.4042     0.6956 0.000 0.664 0.000 0.004 0.316 0.016
#&gt; GSM28750     6  0.3531     0.9292 0.000 0.000 0.328 0.000 0.000 0.672
#&gt; GSM11290     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11294     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28771     4  0.4654     0.6201 0.000 0.044 0.000 0.720 0.048 0.188
#&gt; GSM28760     5  0.4238     0.1135 0.000 0.000 0.000 0.444 0.540 0.016
#&gt; GSM28774     5  0.0665     0.8628 0.000 0.004 0.000 0.008 0.980 0.008
#&gt; GSM11284     5  0.2070     0.7689 0.000 0.092 0.000 0.000 0.896 0.012
#&gt; GSM28761     6  0.3221     0.9438 0.000 0.000 0.264 0.000 0.000 0.736
#&gt; GSM11278     4  0.4601     0.4294 0.000 0.020 0.000 0.588 0.376 0.016
#&gt; GSM11291     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11272     6  0.3221     0.9438 0.000 0.000 0.264 0.000 0.000 0.736
#&gt; GSM11285     5  0.0748     0.8655 0.000 0.004 0.000 0.016 0.976 0.004
#&gt; GSM28753     5  0.2575     0.8451 0.000 0.020 0.000 0.076 0.884 0.020
#&gt; GSM28773     5  0.1882     0.8572 0.000 0.024 0.000 0.020 0.928 0.028
#&gt; GSM28765     5  0.2684     0.8475 0.000 0.024 0.000 0.072 0.880 0.024
#&gt; GSM28768     2  0.2726     0.5289 0.016 0.884 0.000 0.004 0.044 0.052
#&gt; GSM28754     5  0.2784     0.8433 0.000 0.020 0.000 0.064 0.876 0.040
#&gt; GSM28769     2  0.3646     0.6089 0.000 0.804 0.000 0.008 0.116 0.072
#&gt; GSM11275     1  0.3065     0.8122 0.820 0.152 0.000 0.000 0.000 0.028
#&gt; GSM11270     4  0.4601     0.4291 0.000 0.020 0.000 0.588 0.376 0.016
#&gt; GSM11271     5  0.0000     0.8637 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11288     2  0.5239    -0.0822 0.000 0.596 0.096 0.004 0.004 0.300
#&gt; GSM11273     4  0.1866     0.6551 0.000 0.000 0.000 0.908 0.084 0.008
#&gt; GSM28757     2  0.5003     0.4611 0.000 0.496 0.000 0.028 0.452 0.024
#&gt; GSM11282     5  0.4194     0.5462 0.000 0.024 0.000 0.272 0.692 0.012
#&gt; GSM28756     5  0.0508     0.8673 0.000 0.000 0.000 0.012 0.984 0.004
#&gt; GSM11276     5  0.0260     0.8648 0.000 0.000 0.000 0.008 0.992 0.000
#&gt; GSM28752     2  0.4710     0.6665 0.000 0.596 0.000 0.024 0.360 0.020
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:mclust 54     0.398 2
#> ATC:mclust 54     0.374 3
#> ATC:mclust 36     0.411 4
#> ATC:mclust 48     0.405 5
#> ATC:mclust 48     0.438 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21586 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.851           0.900       0.956         0.4113 0.609   0.609
#> 3 3 0.970           0.935       0.975         0.4685 0.720   0.568
#> 4 4 0.737           0.773       0.884         0.1750 0.848   0.638
#> 5 5 0.700           0.723       0.839         0.0851 0.874   0.604
#> 6 6 0.714           0.670       0.812         0.0452 0.958   0.823
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28762     2  0.8327      0.676 0.264 0.736
#&gt; GSM28763     1  0.9686      0.242 0.604 0.396
#&gt; GSM28764     2  0.9129      0.562 0.328 0.672
#&gt; GSM11274     2  0.0000      0.948 0.000 1.000
#&gt; GSM28772     1  0.0000      0.966 1.000 0.000
#&gt; GSM11269     1  0.0000      0.966 1.000 0.000
#&gt; GSM28775     1  0.0000      0.966 1.000 0.000
#&gt; GSM11293     1  0.0000      0.966 1.000 0.000
#&gt; GSM28755     1  0.0000      0.966 1.000 0.000
#&gt; GSM11279     1  0.0000      0.966 1.000 0.000
#&gt; GSM28758     1  0.0000      0.966 1.000 0.000
#&gt; GSM11281     1  0.0000      0.966 1.000 0.000
#&gt; GSM11287     1  0.0000      0.966 1.000 0.000
#&gt; GSM28759     1  0.0000      0.966 1.000 0.000
#&gt; GSM11292     2  0.0000      0.948 0.000 1.000
#&gt; GSM28766     2  0.0000      0.948 0.000 1.000
#&gt; GSM11268     2  0.0000      0.948 0.000 1.000
#&gt; GSM28767     2  0.0000      0.948 0.000 1.000
#&gt; GSM11286     2  0.4298      0.887 0.088 0.912
#&gt; GSM28751     1  0.0376      0.962 0.996 0.004
#&gt; GSM28770     2  0.0000      0.948 0.000 1.000
#&gt; GSM11283     2  0.0376      0.946 0.004 0.996
#&gt; GSM11289     2  0.8661      0.636 0.288 0.712
#&gt; GSM11280     2  0.1633      0.934 0.024 0.976
#&gt; GSM28749     2  0.0000      0.948 0.000 1.000
#&gt; GSM28750     2  0.0000      0.948 0.000 1.000
#&gt; GSM11290     2  0.0000      0.948 0.000 1.000
#&gt; GSM11294     2  0.0000      0.948 0.000 1.000
#&gt; GSM28771     2  0.0000      0.948 0.000 1.000
#&gt; GSM28760     2  0.0000      0.948 0.000 1.000
#&gt; GSM28774     2  0.0376      0.946 0.004 0.996
#&gt; GSM11284     2  0.0000      0.948 0.000 1.000
#&gt; GSM28761     2  0.0000      0.948 0.000 1.000
#&gt; GSM11278     2  0.0000      0.948 0.000 1.000
#&gt; GSM11291     2  0.0000      0.948 0.000 1.000
#&gt; GSM11277     2  0.0000      0.948 0.000 1.000
#&gt; GSM11272     2  0.0000      0.948 0.000 1.000
#&gt; GSM11285     2  0.0000      0.948 0.000 1.000
#&gt; GSM28753     2  0.0000      0.948 0.000 1.000
#&gt; GSM28773     2  0.0000      0.948 0.000 1.000
#&gt; GSM28765     2  0.6712      0.798 0.176 0.824
#&gt; GSM28768     1  0.0000      0.966 1.000 0.000
#&gt; GSM28754     2  0.2603      0.921 0.044 0.956
#&gt; GSM28769     2  0.9881      0.296 0.436 0.564
#&gt; GSM11275     1  0.0000      0.966 1.000 0.000
#&gt; GSM11270     2  0.0000      0.948 0.000 1.000
#&gt; GSM11271     2  0.0000      0.948 0.000 1.000
#&gt; GSM11288     2  0.6531      0.811 0.168 0.832
#&gt; GSM11273     2  0.0000      0.948 0.000 1.000
#&gt; GSM28757     2  0.0000      0.948 0.000 1.000
#&gt; GSM11282     2  0.0000      0.948 0.000 1.000
#&gt; GSM28756     2  0.0000      0.948 0.000 1.000
#&gt; GSM11276     2  0.3114      0.913 0.056 0.944
#&gt; GSM28752     2  0.3879      0.897 0.076 0.924
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2   p3
#&gt; GSM28762     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28763     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28764     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11274     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM28772     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11269     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM28775     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11293     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM28755     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11279     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM28758     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11281     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11287     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM28759     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11292     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28766     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11268     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM28767     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11286     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28751     2   0.608      0.386 0.388 0.612 0.00
#&gt; GSM28770     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11283     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11289     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11280     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28749     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28750     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM11290     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM11294     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM28771     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28760     2   0.502      0.676 0.000 0.760 0.24
#&gt; GSM28774     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11284     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28761     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM11278     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11291     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM11277     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM11272     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM11285     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28753     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28773     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28765     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28768     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM28754     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28769     2   0.706      0.122 0.464 0.516 0.02
#&gt; GSM11275     1   0.000      1.000 1.000 0.000 0.00
#&gt; GSM11270     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11271     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11288     3   0.522      0.647 0.260 0.000 0.74
#&gt; GSM11273     3   0.000      0.973 0.000 0.000 1.00
#&gt; GSM28757     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11282     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28756     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM11276     2   0.000      0.962 0.000 1.000 0.00
#&gt; GSM28752     2   0.000      0.962 0.000 1.000 0.00
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28762     4  0.4992      0.245 0.000 0.476 0.000 0.524
#&gt; GSM28763     2  0.1557      0.867 0.000 0.944 0.000 0.056
#&gt; GSM28764     2  0.0469      0.875 0.000 0.988 0.000 0.012
#&gt; GSM11274     3  0.0336      0.920 0.000 0.000 0.992 0.008
#&gt; GSM28772     1  0.0188      0.993 0.996 0.000 0.000 0.004
#&gt; GSM11269     1  0.0000      0.993 1.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000      0.993 1.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000      0.993 1.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0188      0.993 0.996 0.000 0.000 0.004
#&gt; GSM11279     1  0.0336      0.991 0.992 0.000 0.000 0.008
#&gt; GSM28758     1  0.0336      0.992 0.992 0.000 0.000 0.008
#&gt; GSM11281     1  0.0000      0.993 1.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0188      0.993 0.996 0.000 0.000 0.004
#&gt; GSM28759     1  0.0188      0.992 0.996 0.000 0.000 0.004
#&gt; GSM11292     2  0.3311      0.799 0.000 0.828 0.000 0.172
#&gt; GSM28766     2  0.3123      0.810 0.000 0.844 0.000 0.156
#&gt; GSM11268     3  0.2011      0.873 0.000 0.000 0.920 0.080
#&gt; GSM28767     2  0.1557      0.873 0.000 0.944 0.000 0.056
#&gt; GSM11286     4  0.4985      0.255 0.000 0.468 0.000 0.532
#&gt; GSM28751     4  0.4036      0.598 0.076 0.088 0.000 0.836
#&gt; GSM28770     2  0.1557      0.871 0.000 0.944 0.000 0.056
#&gt; GSM11283     2  0.1867      0.838 0.000 0.928 0.000 0.072
#&gt; GSM11289     2  0.1389      0.872 0.000 0.952 0.000 0.048
#&gt; GSM11280     4  0.3837      0.647 0.000 0.224 0.000 0.776
#&gt; GSM28749     4  0.4382      0.581 0.000 0.296 0.000 0.704
#&gt; GSM28750     3  0.1118      0.906 0.000 0.000 0.964 0.036
#&gt; GSM11290     3  0.0188      0.920 0.000 0.000 0.996 0.004
#&gt; GSM11294     3  0.0000      0.921 0.000 0.000 1.000 0.000
#&gt; GSM28771     2  0.1940      0.836 0.000 0.924 0.000 0.076
#&gt; GSM28760     2  0.5927      0.405 0.000 0.660 0.264 0.076
#&gt; GSM28774     2  0.1792      0.870 0.000 0.932 0.000 0.068
#&gt; GSM11284     2  0.1302      0.875 0.000 0.956 0.000 0.044
#&gt; GSM28761     4  0.4985     -0.257 0.000 0.000 0.468 0.532
#&gt; GSM11278     2  0.1284      0.866 0.000 0.964 0.012 0.024
#&gt; GSM11291     3  0.0000      0.921 0.000 0.000 1.000 0.000
#&gt; GSM11277     3  0.0000      0.921 0.000 0.000 1.000 0.000
#&gt; GSM11272     3  0.4994      0.226 0.000 0.000 0.520 0.480
#&gt; GSM11285     2  0.0592      0.868 0.000 0.984 0.000 0.016
#&gt; GSM28753     4  0.4998      0.189 0.000 0.488 0.000 0.512
#&gt; GSM28773     2  0.0921      0.864 0.000 0.972 0.000 0.028
#&gt; GSM28765     2  0.3907      0.700 0.000 0.768 0.000 0.232
#&gt; GSM28768     1  0.1474      0.957 0.948 0.000 0.000 0.052
#&gt; GSM28754     2  0.4250      0.568 0.000 0.724 0.000 0.276
#&gt; GSM28769     4  0.3272      0.580 0.036 0.052 0.020 0.892
#&gt; GSM11275     1  0.0469      0.988 0.988 0.000 0.000 0.012
#&gt; GSM11270     2  0.1929      0.868 0.000 0.940 0.024 0.036
#&gt; GSM11271     2  0.2408      0.849 0.000 0.896 0.000 0.104
#&gt; GSM11288     4  0.5464      0.194 0.020 0.008 0.316 0.656
#&gt; GSM11273     3  0.0188      0.919 0.000 0.000 0.996 0.004
#&gt; GSM28757     4  0.3610      0.658 0.000 0.200 0.000 0.800
#&gt; GSM11282     2  0.1297      0.872 0.000 0.964 0.016 0.020
#&gt; GSM28756     2  0.2281      0.854 0.000 0.904 0.000 0.096
#&gt; GSM11276     2  0.1792      0.868 0.000 0.932 0.000 0.068
#&gt; GSM28752     2  0.3837      0.711 0.000 0.776 0.000 0.224
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28762     5  0.5136     0.5040 0.000 0.180 0.000 0.128 0.692
#&gt; GSM28763     4  0.4760     0.6342 0.000 0.416 0.000 0.564 0.020
#&gt; GSM28764     2  0.0703     0.7597 0.000 0.976 0.000 0.024 0.000
#&gt; GSM11274     3  0.0290     0.9327 0.000 0.000 0.992 0.008 0.000
#&gt; GSM28772     1  0.0162     0.9914 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11269     1  0.0000     0.9921 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28775     1  0.0000     0.9921 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11293     1  0.0000     0.9921 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28755     1  0.0162     0.9914 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11279     1  0.0290     0.9897 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28758     1  0.0000     0.9921 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11281     1  0.0000     0.9921 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11287     1  0.0162     0.9914 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28759     1  0.0000     0.9921 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     2  0.3165     0.7163 0.000 0.848 0.000 0.116 0.036
#&gt; GSM28766     2  0.3930     0.6582 0.000 0.792 0.000 0.152 0.056
#&gt; GSM11268     3  0.4469     0.7296 0.000 0.000 0.756 0.096 0.148
#&gt; GSM28767     2  0.1670     0.7527 0.000 0.936 0.000 0.052 0.012
#&gt; GSM11286     2  0.4590     0.3125 0.000 0.568 0.000 0.012 0.420
#&gt; GSM28751     5  0.3454     0.6352 0.024 0.076 0.000 0.044 0.856
#&gt; GSM28770     2  0.2351     0.7389 0.000 0.896 0.000 0.088 0.016
#&gt; GSM11283     4  0.4270     0.7598 0.000 0.320 0.000 0.668 0.012
#&gt; GSM11289     2  0.2561     0.7346 0.000 0.884 0.000 0.096 0.020
#&gt; GSM11280     5  0.3307     0.6355 0.000 0.104 0.000 0.052 0.844
#&gt; GSM28749     2  0.6100     0.1944 0.000 0.484 0.000 0.128 0.388
#&gt; GSM28750     3  0.3043     0.8520 0.000 0.000 0.864 0.080 0.056
#&gt; GSM11290     3  0.0290     0.9354 0.000 0.000 0.992 0.008 0.000
#&gt; GSM11294     3  0.0290     0.9359 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28771     4  0.4329     0.7633 0.000 0.312 0.000 0.672 0.016
#&gt; GSM28760     4  0.5788     0.6693 0.000 0.236 0.116 0.636 0.012
#&gt; GSM28774     2  0.3239     0.7332 0.000 0.852 0.000 0.068 0.080
#&gt; GSM11284     2  0.2519     0.7297 0.000 0.884 0.000 0.100 0.016
#&gt; GSM28761     5  0.5739     0.3688 0.000 0.000 0.280 0.124 0.596
#&gt; GSM11278     2  0.3267     0.7021 0.000 0.844 0.044 0.112 0.000
#&gt; GSM11291     3  0.0162     0.9365 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11277     3  0.0162     0.9365 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11272     5  0.6347    -0.0115 0.000 0.000 0.408 0.160 0.432
#&gt; GSM11285     2  0.1965     0.7285 0.000 0.904 0.000 0.096 0.000
#&gt; GSM28753     4  0.6536     0.2754 0.000 0.216 0.000 0.464 0.320
#&gt; GSM28773     2  0.3928     0.3935 0.000 0.700 0.000 0.296 0.004
#&gt; GSM28765     2  0.4384     0.5850 0.000 0.728 0.000 0.044 0.228
#&gt; GSM28768     1  0.1774     0.9422 0.932 0.000 0.000 0.052 0.016
#&gt; GSM28754     5  0.6529     0.0858 0.000 0.228 0.000 0.296 0.476
#&gt; GSM28769     5  0.2313     0.6393 0.008 0.040 0.008 0.024 0.920
#&gt; GSM11275     1  0.0510     0.9833 0.984 0.000 0.000 0.016 0.000
#&gt; GSM11270     2  0.4801     0.6579 0.000 0.768 0.076 0.120 0.036
#&gt; GSM11271     2  0.1117     0.7617 0.000 0.964 0.000 0.016 0.020
#&gt; GSM11288     5  0.6469     0.3615 0.016 0.016 0.284 0.104 0.580
#&gt; GSM11273     3  0.0807     0.9222 0.000 0.012 0.976 0.012 0.000
#&gt; GSM28757     5  0.2727     0.6323 0.000 0.116 0.000 0.016 0.868
#&gt; GSM11282     2  0.3717     0.7122 0.000 0.836 0.040 0.100 0.024
#&gt; GSM28756     2  0.2300     0.7420 0.000 0.904 0.000 0.072 0.024
#&gt; GSM11276     2  0.1403     0.7611 0.000 0.952 0.000 0.024 0.024
#&gt; GSM28752     2  0.2971     0.6928 0.000 0.836 0.000 0.008 0.156
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28762     2  0.5000     0.5687 0.000 0.704 0.000 0.156 0.100 0.040
#&gt; GSM28763     4  0.4686     0.6364 0.000 0.088 0.000 0.676 0.232 0.004
#&gt; GSM28764     5  0.0767     0.7388 0.000 0.012 0.000 0.008 0.976 0.004
#&gt; GSM11274     3  0.1176     0.8318 0.000 0.000 0.956 0.020 0.000 0.024
#&gt; GSM28772     1  0.0000     0.9881 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11269     1  0.0146     0.9882 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM28775     1  0.0260     0.9875 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM11293     1  0.0260     0.9875 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM28755     1  0.0260     0.9858 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM11279     1  0.0405     0.9839 0.988 0.000 0.000 0.008 0.000 0.004
#&gt; GSM28758     1  0.0146     0.9881 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM11281     1  0.0146     0.9882 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM11287     1  0.0146     0.9872 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM28759     1  0.0000     0.9881 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11292     5  0.2766     0.6935 0.000 0.004 0.000 0.020 0.852 0.124
#&gt; GSM28766     5  0.4087     0.4879 0.000 0.000 0.000 0.036 0.688 0.276
#&gt; GSM11268     3  0.4415     0.1871 0.000 0.020 0.556 0.004 0.000 0.420
#&gt; GSM28767     5  0.1398     0.7252 0.000 0.000 0.000 0.008 0.940 0.052
#&gt; GSM11286     2  0.4176     0.1196 0.000 0.580 0.000 0.000 0.404 0.016
#&gt; GSM28751     2  0.2094     0.6070 0.004 0.908 0.000 0.000 0.024 0.064
#&gt; GSM28770     5  0.2006     0.7110 0.000 0.000 0.000 0.016 0.904 0.080
#&gt; GSM11283     4  0.2772     0.7475 0.000 0.004 0.000 0.816 0.180 0.000
#&gt; GSM11289     5  0.2263     0.7042 0.000 0.000 0.000 0.016 0.884 0.100
#&gt; GSM11280     2  0.4584     0.5533 0.000 0.732 0.000 0.056 0.040 0.172
#&gt; GSM28749     6  0.5935     0.0170 0.000 0.244 0.000 0.000 0.300 0.456
#&gt; GSM28750     3  0.3859     0.5741 0.000 0.012 0.720 0.012 0.000 0.256
#&gt; GSM11290     3  0.1155     0.8304 0.000 0.004 0.956 0.004 0.000 0.036
#&gt; GSM11294     3  0.0405     0.8403 0.000 0.004 0.988 0.000 0.000 0.008
#&gt; GSM28771     4  0.2632     0.7468 0.000 0.004 0.000 0.832 0.164 0.000
#&gt; GSM28760     4  0.4672     0.6832 0.000 0.000 0.092 0.728 0.152 0.028
#&gt; GSM28774     5  0.4525     0.6612 0.000 0.220 0.000 0.072 0.700 0.008
#&gt; GSM11284     5  0.3842     0.7050 0.000 0.100 0.000 0.112 0.784 0.004
#&gt; GSM28761     6  0.5556     0.4171 0.000 0.148 0.252 0.012 0.000 0.588
#&gt; GSM11278     5  0.5377     0.6403 0.000 0.048 0.096 0.132 0.704 0.020
#&gt; GSM11291     3  0.0000     0.8408 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11277     3  0.0547     0.8365 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM11272     6  0.4295     0.4609 0.000 0.052 0.212 0.012 0.000 0.724
#&gt; GSM11285     5  0.2182     0.7225 0.000 0.004 0.000 0.076 0.900 0.020
#&gt; GSM28753     4  0.6534     0.0885 0.000 0.116 0.000 0.420 0.072 0.392
#&gt; GSM28773     5  0.4581     0.3278 0.000 0.016 0.000 0.368 0.596 0.020
#&gt; GSM28765     5  0.6507     0.3170 0.000 0.268 0.000 0.052 0.496 0.184
#&gt; GSM28768     1  0.2137     0.9255 0.912 0.012 0.000 0.048 0.000 0.028
#&gt; GSM28754     2  0.6815     0.1672 0.000 0.428 0.000 0.344 0.092 0.136
#&gt; GSM28769     2  0.1644     0.5818 0.000 0.920 0.000 0.000 0.004 0.076
#&gt; GSM11275     1  0.0547     0.9818 0.980 0.000 0.000 0.020 0.000 0.000
#&gt; GSM11270     5  0.7558     0.4110 0.000 0.176 0.172 0.116 0.488 0.048
#&gt; GSM11271     5  0.1151     0.7421 0.000 0.032 0.000 0.012 0.956 0.000
#&gt; GSM11288     2  0.4314     0.4211 0.004 0.716 0.036 0.012 0.000 0.232
#&gt; GSM11273     3  0.1944     0.8010 0.000 0.000 0.924 0.024 0.016 0.036
#&gt; GSM28757     2  0.3047     0.6148 0.000 0.848 0.000 0.004 0.064 0.084
#&gt; GSM11282     5  0.5915     0.6369 0.000 0.100 0.096 0.120 0.664 0.020
#&gt; GSM28756     5  0.3727     0.7036 0.000 0.128 0.000 0.088 0.784 0.000
#&gt; GSM11276     5  0.1982     0.7413 0.000 0.068 0.000 0.016 0.912 0.004
#&gt; GSM28752     5  0.4184     0.5544 0.000 0.296 0.000 0.004 0.672 0.028
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:NMF 52     0.396 2
#> ATC:NMF 52     0.372 3
#> ATC:NMF 47     0.344 4
#> ATC:NMF 46     0.467 5
#> ATC:NMF 42     0.457 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      parallel  stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0     ComplexHeatmap_2.1.1  markdown_1.1          knitr_1.26           
#>  [5] preprocessCore_1.46.0 cola_1.3.2            GEOquery_2.52.0       Biobase_2.44.0       
#>  [9] BiocGenerics_0.30.0   GetoptLong_0.1.7     
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6         matrixStats_0.55.0   bit64_0.9-7          doParallel_1.0.15   
#>  [5] RColorBrewer_1.1-2   httr_1.4.1           tools_3.6.0          backports_1.1.5     
#>  [9] R6_2.4.1             DBI_1.0.0            lazyeval_0.2.2       colorspace_1.4-1    
#> [13] withr_2.1.2          tidyselect_0.2.5     gridExtra_2.3        bit_1.1-14          
#> [17] compiler_3.6.0       xml2_1.2.2           microbenchmark_1.4-7 pkgmaker_0.28       
#> [21] slam_0.1-46          scales_1.1.0         readr_1.3.1          NMF_0.23.6          
#> [25] stringr_1.4.0        digest_0.6.23        pkgconfig_2.0.3      bibtex_0.4.2        
#> [29] highr_0.8            limma_3.40.6         rlang_0.4.2          GlobalOptions_0.1.1 
#> [33] RSQLite_2.1.2        impute_1.58.0        shape_1.4.4          mclust_5.4.5        
#> [37] dendextend_1.12.0    dplyr_0.8.3          RCurl_1.95-4.12      magrittr_1.5        
#> [41] Matrix_1.2-17        Rcpp_1.0.3           munsell_0.5.0        S4Vectors_0.22.1    
#> [45] viridis_0.5.1        lifecycle_0.1.0      stringi_1.4.3        plyr_1.8.4          
#> [49] blob_1.2.0           crayon_1.3.4         lattice_0.20-38      splines_3.6.0       
#> [53] annotate_1.62.0      circlize_0.4.9       hms_0.5.2            zeallot_0.1.0       
#> [57] pillar_1.4.2         rjson_0.2.20         rngtools_1.4         reshape2_1.4.3      
#> [61] codetools_0.2-16     stats4_3.6.0         XML_3.98-1.20        glue_1.3.1          
#> [65] evaluate_0.14        png_0.1-7            vctrs_0.2.0          foreach_1.4.7       
#> [69] polyclip_1.10-0      gtable_0.3.0         purrr_0.3.3          tidyr_1.0.0         
#> [73] clue_0.3-57          assertthat_0.2.1     ggplot2_3.2.1        xfun_0.11           
#> [77] gridBase_0.4-7       eulerr_6.0.0         xtable_1.8-4         skmeans_0.2-11      
#> [81] survival_2.44-1.1    viridisLite_0.3.0    tibble_2.1.3         iterators_1.0.12    
#> [85] AnnotationDbi_1.46.1 registry_0.5-1       memoise_1.1.0        IRanges_2.18.3      
#> [89] cluster_2.1.0        brew_1.0-6
```

