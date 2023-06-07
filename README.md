# Purpose of Repository
A detailed step-by-step of how to setup, build, and deploy EfficientDet network into i.MX8M Plus platform (as provided by NXP official repository).

# Prerequisite
Please clone the following repository:
```
$ git clone https://github.com/nxp-imx/efficientdet-imx.git
$ git clone https://github.com/google/automl.git
```
[Optional] If you are looking to deploy EfficientDet other than D0 variant, you'll need to modify the following code:
```
$ cd efficientdet-imx/automl/efficientdet/
$ gedit inference.py
    -> "Find" def export()
    -> "Locate" converter.target_spec.supported_ops = [tf.lite.OpsSet.TFLITE_BUILTINS]
    -> "Replace" converter.target_spec.supported_ops = [tf.lite.OpsSet.TFLITE_BUILTINS, tf.lite.OpsSet.SELECT_TF_OPS]
```
