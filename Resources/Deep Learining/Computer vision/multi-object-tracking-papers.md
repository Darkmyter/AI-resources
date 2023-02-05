# Multi Object Tracking papers explained <!-- omit in toc -->

This is a collection of blogs, videos and personal notes that explain and dive into major Multi Object Tracking publications.
These materials helped grasped the concepts and ideas proposed in each paper.

- [ByteTrack: Multi-Object Tracking by Associating Every Detection Box, **ByteTrack 2021**](#bytetrack-multi-object-tracking-by-associating-every-detection-box-bytetrack-2021)

## [ByteTrack: Multi-Object Tracking by Associating Every Detection Box](https://arxiv.org/abs/2110.06864), **ByteTrack 2021**
* Articles:
  * [ByteTrack: A Simple Yet Effective Multi-Object Tracking Technique](https://arshren.medium.com/bytetrack-a-simple-yet-effective-multi-object-tracking-technique-86f1f3632a85)
* Notes:
  * ByteTrack is a Multi Object Tracker, it identifies the detected objects and tracks their trajectory in the video. The algorithm uses tracklets, representation of tracked objects, to store the identity of detections.
  * The main idea of BYTE (the algorithm behind ByteTrack), is to consider both high and low confidence detections.  
  * For each frame the position of the bounding boxes are predicted using a Kalman filter from the previous positions. The high confidence detections $D^{high}$ are matched with these predicted tracklets by iou and are identified.  
  * The low confidence detection $D^{low}$ are compared with unmatched tracklets (identified objects are not associated to any bounding box in that frame). This helps identity occulted objects which usually have low confidence.
  * A bin of unmatched tracklets is kept for $n$ frames to handle object rebirth. They are deleted beyond $n$ is they remain unmatched.