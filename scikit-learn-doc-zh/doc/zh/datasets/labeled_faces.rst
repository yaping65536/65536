.. _labeled_faces_in_the_wild:

带标签的人脸识别数据集
============================================

这个数据集是一个在互联网上收集的名人 JPEG 图片集，所有详细信息都可以在官方网站上获得:

    http://vis-www.cs.umass.edu/lfw/

每张图片的居中部分都是一张脸。典型人物人脸验证是给定两幅图片，二元分类器必须能够预测这两幅图片是否是同一个人。

另一项任务人脸识别或面部识别说的是给定一个未知的面孔，通过参考一系列已经学习经过鉴定的人的照片来识别此人的名字。

人脸验证和人脸识别都是基于经过训练用于人脸检测的模型的输出所进行的任务。 最流行的人脸检测模型叫作 Viola Jones是在 OpenCV 库中实现的。 LFW 人脸数据库中的人脸是使用该人脸检测器从各种在线网站上提取的。


用法
--------

``scikit-learn`` 提供两个可以自动下载、缓存、解析元数据文件的 loader (加载器)，解码 JPEG
并且将 slices 转换成内存映射过的 NumPy 数组(numpy.memmap)。
这个数据集大小超过 200 MB。第一个加载器通常需要超过几分钟才能完全解码 JPEG 文件的相关部分为 NumPy 数组。
如果数据集已经被加载过，通过在磁盘上采用内存映射版( memmaped version )的 memoized，
即 ``~/scikit_learn_data/lfw_home/`` 文件夹使用 ``joblib``，再次加载时间会小于 200ms。

第一个 loader (加载器)用于人脸识别任务:一个多类分类任务(属于监督学习)::

  >>> from sklearn.datasets import fetch_lfw_people
  >>> lfw_people = fetch_lfw_people(min_faces_per_person=70, resize=0.4)

  >>> for name in lfw_people.target_names:
  ...     print(name)
  ...
  Ariel Sharon
  Colin Powell
  Donald Rumsfeld
  George W Bush
  Gerhard Schroeder
  Hugo Chavez
  Tony Blair

默认的 slice 是一个删除掉大部分背景，只剩下围绕着脸周围的长方形的形状::

  >>> lfw_people.data.dtype
  dtype('float32')

  >>> lfw_people.data.shape
  (1288, 1850)

  >>> lfw_people.images.shape
  (1288, 50, 37)

在 ``target(目标)`` 数组中，``1140``个人脸图片中的每一个图都分配一个属于某人的 id::

  >>> lfw_people.target.shape
  (1288,)

  >>> list(lfw_people.target[:10])
  [5, 6, 3, 1, 0, 1, 3, 4, 3, 0]

第二个 loader (加载器)通常用于人脸验证任务: 每个样本是属于或不属于同一个人的两张图片::

  >>> from sklearn.datasets import fetch_lfw_pairs
  >>> lfw_pairs_train = fetch_lfw_pairs(subset='train')

  >>> list(lfw_pairs_train.target_names)
  ['Different persons', 'Same person']

  >>> lfw_pairs_train.pairs.shape
  (2200, 2, 62, 47)

  >>> lfw_pairs_train.data.shape
  (2200, 5828)

  >>> lfw_pairs_train.target.shape
  (2200,)

 :func:`sklearn.datasets.fetch_lfw_people` 和 :func:`sklearn.datasets.fetch_lfw_pairs` 函数，都可以通过 ``color=True`` 来获得 RGB 颜色通道的维度，在这种情况下尺寸将为 ``(2200, 2, 62, 47, 3)`` 。

:func:`sklearn.datasets.fetch_lfw_pairs` 数据集细分为 3 类: 
``train`` set(训练集)、``test`` set(测试集)和一个 ``10_folds`` 评估集, ``10_folds`` 评估集意味着性能的计算指标使用 10 折交叉验证( 10-folds cross validation )方案。

.. topic:: 参考文献:

 * `Labeled Faces in the Wild: A Database for Studying Face Recognition
   in Unconstrained Environments.
   <http://vis-www.cs.umass.edu/lfw/lfw.pdf>`_
   Gary B. Huang, Manu Ramesh, Tamara Berg, and Erik Learned-Miller.
   University of Massachusetts, Amherst, Technical Report 07-49, October, 2007.


示例
--------------

:ref:`sphx_glr_auto_examples_applications_plot_face_recognition.py`
