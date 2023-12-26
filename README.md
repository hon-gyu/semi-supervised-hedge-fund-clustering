# Semi-supervised Hedge Fund Clustering

This repo demonstrates a semi-supervised clustering algorithm for fund of funds analysis. It attempts to provide a method for hierarchical classification of funds. 

An innovation is the utilisation of a limited number of known cascade classifications, e.g. those obtained through due diligence, which is why it is semi-supervised. Another innovation is that it uses hierarchical tree distance in order to take advantage of a fund classification feature provided by data vendors, a feature whose taxonomy might be different from our system.

The implementation is based on my two other repo, [AKB-distance](https://github.com/hon-gyu/hierarchical-tree-distance) and [semi-supervised-chameleon-clustering](https://github.com/hon-gyu/semi-supervised-chameleon-clustering).

## Motivation

In fund of funds analysis, we often need to classify unknown funds. On the one hand this allows us to compare the fund more meaningfully with its counterparts in the same strategy. On the other hand we can use this to obtain the recent performance of different strategies in the market, e.g. to calculate a strategy index. The categorisation is often hierarchical, e.g. "CTA - Quantitative CTA - Medium to Long Term Momentum CTA", or "Equity - Discretionary Long - Sector Rotation". However, the way these strategies are grouped varies a lot and isn't standard, making it tough to apply typical supervised learning methods.

The challenge lies in implementing general supervised learning algorithms due to the scarce availability of accurate strategy classification labels, which are typically obtained through due diligence or from third-party data vendors. The number of true labels obtained by due diligence is small, which is not sufficient to use supervised learning models. The labels from third parties are often unverified and may not align with one's internal classification system. Additionally, the need for more granular classifications beyond the broad categories provided can be a problem. This repository addresses these issues through a semi-supervised clustering algorithm and hierarchical tree distance.

## AKB Distance

The clustering algorithm calculates the distance between instances using features. When dealing with a tree-structured fund classification from data vendors, traditional metrics struggle to accurately measure distances. Thus, the [AKB distance](https://github.com/hon-gyu/hierarchical-tree-distance) is used, a novel measure designed to quantify the similarity between classes within a hierarchical label tree. The AKB tree distance is particularly adept at emphasizing the importance of higher hierarchy errors, utilizing the taxonomy's inherent structure instead of simply flattening the hierarchy in traditional methods.

After calculating pairwise distances between instances for certain features, the final distance is obtained by summing the normalized distance for each feature, following the heterogeneous Euclidean-overlap metric (HEOM) approach.

## Semi-supervised Chameleon Clustering

Semi-supervised clustering is a technique that introduces knowledge-based constraints into the clustering process. In our context, these constraints are in the form of must-link (two instances must belong to the same cluster at a certain hierarchy level) and cannot-link (two instances must not belong to the same cluster). Chameleon clustering, a hierarchical algorithm, is used here; it initially segments the data into many small groups and then reassembles these groups, respecting the natural data groupings. We inject must-link and cannot-link constraints during the partitioning and merging phase. This helps to fasten the process and improve accuracy.

## Toy Example

Please refer to the provided jupyter notebook. It contains a toy example using synthetic data with hierarchiacl labels.