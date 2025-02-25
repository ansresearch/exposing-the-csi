# Exposing the CSI (dataset)

This repository contains the links to the dataset + ground-truth videos for our paper [Exposing the CSI: A systematic Investigation of CSI-based Wi-Fi Sensing Capabilities and Limitations](https://ieeexplore.ieee.org/document/10099368).

The code used to process the dataset is available [here](https://github.com/ansresearch/SHARP).

## Main features

The dataset contains CSI sequences collected in indoor environments while a target person performs different activities. The main features of the dataset are the following:

* 12 target activities, 80 seconds for each activity
* 7 different scenarios (3 people, 3 environments)
* CSI collected using 160-MHz 802.11ax devices with 4 antennas
* 3 Wi-Fi receivers collecting the same frames in different locations
* anonymized video ground-truth with reference keypoints

---

## Dataset

We split our dataset into multiple repositories hosted on Zenodo.
Each repository has size of approx 11 GB and contains the CSI data of a different scenario, from S1 to S7.

| Scenario | Candidate | Environment | Day |
|---|---|---|---|
| [S1](https://doi.org/10.5281/zenodo.7732595) | A | Lab | 1 | 
| [S2](https://doi.org/10.5281/zenodo.7732781) | B | Lab | 1 |
| [S3](https://doi.org/10.5281/zenodo.7751893) | C | Lab | 1 |
| [S4](https://doi.org/10.5281/zenodo.7751897) | A | Lab | 1 |
| [S5](https://doi.org/10.5281/zenodo.7751907) | A | Lab | 2 |
| [S6](https://doi.org/10.5281/zenodo.7751909) | A | Office | 2 |
| [S7](https://doi.org/10.5281/zenodo.7751915) | A | Hall | 2 |

## Activities

This is the list of activities considered in every scenario:

| | | | |
|---|---|---|---|
| A. Walk | D. Sitting | G. Wave hands | J. Wiping |
| B. Run | E. Empty room | H. Clapping | K. Squat |
| C. Jump | F. Standing | I. Lay down | L. Stretching |

---

## Videos

We recorded every experiment with a camera and used [VideoPose3D](https://github.com/facebookresearch/VideoPose3D) to extract the reference keypoints of the people moving.
The keypoints files are in the `video_keypoints` directory in this repository.
They do **not** contain personal information about the candidates, but can be used to re-create an anonymized video ground truth of the experiments showing how the candidates are moving.

- > Note #1: Keypoints for static activities (like sitting or laying down) are not included in this repository because there is no movement to show.
- > Note #2: Unfortunately, videos for activities A and B for the scenario S3 are missing due to a data pre-processing issue.

### Instructions to generate the videos

0. (optional) The system was tested on Ubuntu using Python 3.8 only, so it is suggested to install all the dependencies in a `conda` environment to avoid polluting your workspace.
```
conda create --name csi python=3.8
conda activate csi
pip install numpy torch matplotlib
```

1. Clone the [VideoPose3D](https://github.com/facebookresearch/VideoPose3D) repository:
```
git clone https://github.com/facebookresearch/VideoPose3D
```

2. Download the pre-trained model in the `checkpoint` directory of VideoPose3D:
```
cd VideoPose3D
mkdir checkpoint
cd checkpoint
wget https://dl.fbaipublicfiles.com/video-pose-3d/pretrained_h36m_detectron_coco.bin
cd ..
```

3. Then, copy our keypoint files into the `data` directory of VideoPose3D:
```
cp /path/to/exposing-the-csi/video_keypoints/* data/
```

4. Finally, generate the anonymized video starting from the keypoints.
**Note**: the following command generates the video for the activity A (walk) in the scenario S1 (see above). Change the value of the variable `activity` to generate the other videos accordingly.
```
activity=S1_A; python run.py -d custom -k ${activity} -arc 3,3,3,3,3 -c checkpoint --evaluate pretrained_h36m_detectron_coco.bin --render --viz-subject ${activity}.mp4 --viz-action custom --viz-camera 0 --viz-video data/blank.mp4 --viz-output ${activity}.mp4 --viz-size 6
```

---

## References

If you find these resources useful, please consider citing our paper:

*M. Cominelli, F. Gringoli and F. Restuccia,
"Exposing the CSI: A Systematic Investigation of CSI-based Wi-Fi Sensing Capabilities and Limitations,"
2023 IEEE International Conference on Pervasive Computing and Communications (PerCom), Atlanta, GA, USA, 2023,
pp. 81-90, doi: 10.1109/PERCOM56429.2023.10099368.*
