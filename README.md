# Face Clustering and Automated Dispatch System

## Overview

This project demonstrates a face-based clustering, refinement, and automated dispatch system built on the LFW (Labeled Faces in the Wild) dataset or its subset.  

It combines unsupervised learning methods (K-Means, DBSCAN, HDBSCAN) with active semi-supervised refinement and an automated email-based dispatch workflow.

## Key Features

- Facial embeddings generated using DeepFace (Facenet model)
- Baseline clustering via K-Means, DBSCAN, and HDBSCAN
- Evaluation metrics: Adjusted Rand Index (ARI) and Normalized Mutual Information (NMI)
- PCA-based visualization of embeddings
- Active semi-supervised learning with COP-KMeans for feedback refinement
- Automated email dispatch system with dry-run support

## Related Notebook: Clustering_Algorithms.ipynb

A separate notebook, `Clustering_Algorithms.ipynb`, provides a comparative analysis of the clustering algorithms used in this project.

### Comparison Details

- Algorithms compared: K-Means, DBSCAN, and HDBSCAN  
- Evaluation metrics: Adjusted Rand Index (ARI) and Normalized Mutual Information (NMI)  
- Visualization using PCA for all methods  
- Additional analysis includes:  
  - HDBSCAN on both raw and PCA-reduced embeddings  
  - Cluster stability and noise handling  
  - Qualitative inspection of clustered samples  

This notebook supports the main dispatch system by validating the clustering techniques before automation.

## Dependencies

Install all required Python libraries before execution:

```bash
pip install deepface scikit-learn pandas numpy tqdm matplotlib pillow active-semi-supervised-clustering secure-smtplib hdbscan
```

## Environment Setup

Platform: Google Colab (recommended)  
Storage: Google Drive integration required  

Dataset Path:
```
/content/drive/MyDrive/lfw_subset_small
```

Execution Flags:
- `DRY_RUN = True` → Safe testing (no emails sent)
- `DRY_RUN = False` → Enables actual email dispatch

## Execution Steps

1. **Load Dataset**  
   Recursively scan and list all image paths from the dataset.

2. **Generate Facial Embeddings**  
   Use DeepFace (Facenet) to generate 128-dimensional embeddings per image.

3. **Baseline Clustering**  
   - Apply K-Means, DBSCAN, and HDBSCAN for comparison  
   - Compute Silhouette Score, ARI, and NMI  
   - Visualize embeddings using PCA

4. **Cluster Validation**  
   - Evaluate inter-cluster distances and purity  
   - Detect and log misclassified images

5. **Active Learning Refinement**  
   - Generate `review_manifest.csv` for user feedback  
   - Use COP-KMeans to refine clusters based on corrections

6. **Manual Correction (Optional)**  
   Review and relabel misclassified samples; recompute ARI and NMI.

7. **Representative Creation**  
   - Generate `representatives.csv` for each cluster center  
   - Create `representatives_montage.png` showing representative samples

8. **Dispatch Automation**  
   - Zip clustered images and generate manifests  
   - Send clusters automatically via email  
   - Use `DRY_RUN` for testing without sending emails

## SMTP Details

| Parameter | Value |
|------------|--------|
| Host | smtp.gmail.com |
| Port | 465 |
| Authentication | Gmail App Password required |
| Max Attachment | 20 MB per ZIP file |

Large ZIP files are skipped automatically and logged.

## Output Files

| File | Description |
|------|--------------|
| `review_manifest.csv` | Image pairs for manual review |
| `dispatch_manifest_inmem.csv` | Dispatch progress tracker |
| `representatives.csv` | Cluster representative information |
| `representatives_montage.png` | Visual grid of representative faces |
| `dispatch_log.csv` | Log of email dispatch attempts |

## Evaluation Metrics

| Metric | Description |
|---------|--------------|
| Adjusted Rand Index (ARI) | Measures similarity between predicted and true labels |
| Normalized Mutual Information (NMI) | Quantifies information shared between clusters and ground truth |

Both metrics are used to evaluate unsupervised clustering accuracy.

## Notebook Cell Summary

| Section | Description |
|----------|--------------|
| Cells A–C | Initialization, path setup, and manifest creation |
| Cell D | Representative image and montage generation |
| Cell E | Helper utilities for ZIP creation and logging |
| Cell F | Email dispatch loop with retries and logging |
| Cell G | Final report generation and summary |

## How to Run Dispatch

1. Ensure `refined_labels` and `refined_clusters` exist.  
2. Set `DRY_RUN = True` for a test run.  
3. Run the dispatch loop cell (`Cell F`) and verify logs.  
4. Once verified, set `DRY_RUN = False` and provide SMTP credentials.  
5. Re-run dispatch for actual email delivery.

## Troubleshooting

| Issue | Cause | Solution |
|--------|--------|-----------|
| No face detected | Low-quality or non-face image | Automatically skipped |
| Empty clusters | Incorrect parameters or data issue | Adjust K or DBSCAN parameters |
| Large ZIP skipped | File exceeds 20MB | Compress manually if needed |
| SMTP Authentication error | Gmail blocks standard password | Use a Google App Password |

## Authors

- Divyashree S  
- Prasanna Kumar KS  
- Ishanni JH  

Environment: Google Colab  
Purpose: Automated face clustering and dispatch system  
Mode: Supports both dry-run testing and real dispatch  

## Related Resources

- `Clustering_Algorithms.ipynb` – Comparative study of clustering algorithms  
- `active_learning.ipynb` – Main automation and dispatch workflow  
