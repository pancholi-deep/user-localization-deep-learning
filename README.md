# user-localization-deep-learning

User localization is an increasingly important feature of wireless communication systems enabling a wide range of applications, such as navigation, smart factories and cities, surveillance, security, and IoT. Moreover, accurate position information can be used for improved radio resource management, beamforming as well as channel estimation.
● Although machine learning-based methods can achieve state-of-the-art localization accuracy, their biggest drawback is the need for large quantities of labeled data, i.e., channel state information (CSI) and the corresponding coordinates. Moreover, whenever significant changes in the radio environment occur, new data must be acquired which renders supervised learning approaches impractical for most scenarios.
● Self-supervised learning methods have led to a tremendous reduction of the quantity of labeled data needed in computer vision and natural language processing.
● The goal is to evaluate the potential of self-supervised learning for user localization based on CSI.
Dataset
The dataset was acquired by the massive MIMO channel sounder described in [1].
● Channel responses were measured outdoors between a moving transmitter and an 8x8 antenna array (horizontally polarized patch antennas). As transmitter, an SDR-equipped cart, was used which was moved around in a residential area of several hundred square meters. The SDR transmitted uplink OFDM pilots with a bandwidth of 20 MHz and 1024 subcarriers at a carrier frequency of 1.27 GHz. Ten percent of the subcarriers were used as guard bands, leaving 924 usable subcarriers. 8 of the 64 antennas were perpetually malfunctioning, and hence, only 56 antennas provided useful measurements.
● Ground truth was acquired with the help of a differential GPS. For every location, 5 channel measurements were recorded. The GPS data was transformed to a local Cartesian coordinate system with the receiver placed at the origin on the XY–plane. The third coordinate represents the height. The covered area spans a substantial dimension of 646×943×41 meters.


● The left figure shows the UE positions (latitude/longitude) recorded using a differential GPS.
● The right figure shows the positions (on the XY-plane) transformed to a local coordinate system to have dimensions in meters, with the base station at (0,0).
The dataset has three zipped files:
1) “labelled_data.zip”. Each file in the folder contains the following:
a) H_Re: Real part of the estimated channel matrices, and is of shape (number of samples, number of working antennas, number of used subcarriers, number of measurements per location). In this case, it is of shape [number of samples, 56, 924, 5].
b) H_Im: Imaginary part of the estimated channel matrices, and of the same shape as H_Re.
c) SNR: Signal-to-noise ratio measured at each antenna during channel estimation.
This is of shape [number of samples, 56, 5].
d) Pos: Ground truth positions of the transmitter, represented in the Cartesian coordinate system. The shape is [number of samples, 3], with the second dimension representing [x,y,z] in that order.
There is a total of 4096 labelled samples.
2) “unlabelled_data.zip”. Each file in the folder contains the above variables apart from
“Pos”. There is a total of 36192 unlabelled samples
3) “test_data.zip”. Each file in the folder contains the same variables with unlabelled_data. There is a total of 883 samples.
Note: On purpose, the data associated with a part of the map (two streets, to be precise) has been omitted from the labelled dataset but will be a part of the test dataset. During the channel estimation, some of the estimates could not be obtained due to a very low SNR at the receiver, and hence they have been stored as zeros.


We designed and trained an algorithm that can determine the position of a user, based on estimated channel frequency responses between the user and an antenna array.
● Built a self-supervised learning model to learn the information from the large unlabelled data.
● With the use of learned information or extracted representation from unlabelled data, built a supervised learning model on the small labelled dataset which comprises position ground truth information. The dataset was partitioned into training and validation sets for the algorithm development and training.
● Ran the supervised learning model on the test data to predict the position for each user.
