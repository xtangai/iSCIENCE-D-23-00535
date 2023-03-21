# iSCIENCE-D-23-00535
Dataset for online battery EIS prediction

0. Description of the original dataset

The 'BatDat.mat' stores the testing data of 16 batteries.
    For each battery, we carried out 14 tests.
    In each test, there is one group of EIS data and five groups of dynamic pulse data.
    So, we have overall 1120=16*14*5 groups of dynamic load profiles and 224=16*14 groups of EIS trajectories
    In addition, we should be able to obtain the same EIS trajectories from different dynamic load profiles.
    For example, the EIS trajectory stored in Bat(2).Test(3).EIS should be recovered from Bat(2).Test(3).Pulse(1), ... ,Bat(2).Test(3).Pulse(5)
The 'BatDat.mat' is difficult to use, and it is converted to 'Load.mat' using 'Get_Load.m'
The data in 'Load.mat' is randomly re-arranged and restored in 'Train_Data_Set.mat --> load' using 'Gen_Load_Multiple_Time.m'.
The resutls are stored in the 'RESULT.mat'.

***********************************************************
1. Please use 'Get_Load.m' to convert the 'BatDat.mat' to 'Load.mat'

The Load.mat stores overall 1120 groups of dynamic profiles and the corresponding EIS trajectories.
    Curr: Current in A
    Volt: Voltage in V
    D: Voltage - OCV in V, here we assume OCV is stable during the dynamic test as the change in SoC is negligible
    EIS: stores the EIS trajectories. 
        EIS.freq: frequency in Hz
        EIS.real: real part in mohm
        EIS.imag: -imaginary part in mohm
        EIS.param: parameter of the fractional order model (uniformed), fitted offline.
        EIS.RMS: root-mean-squared error of the fitting in mohm
    param: = EIS.param
    OCV: Open circuit voltage corresponding to the starting time of each test (in V)

***********************************************************
2. Please use 'Gen_Load_Multiple_Time.m' to generate the training dataset.

The 'Train_Data_Set.mat' contains IN, OT, and load
The data in 'Load.mat' is randomly re-arranged and restored in 'load'.
The data used as the input of the network is stored in 'IN' -- high-order RC parameters
The data used as the output of the network is stored in 'OT' -- model parameters of the fractional order model
The sequence in 'load', 'IN',and 'OT' remains the same.

***********************************************************
3. Please use 'Test_multi_NN.m' to calculate the EIS. In the provided code, 1064 groups of data are used for training, and 56 groups of data for testing

The resutls are stored in 'RESULT.mat'
    EIS_pred: the predicted EIS result, 35*56 complex matrix. It contains 56 groups of complex vector, each vector contains 35 complex number, representing the impedance measured at 35 different frequencies of interest 
    EIS_ref: the referenced EIS result, 35*56 complex matrix.
    net: contains 15 neural network, and the median value of their output is used as EIS_pred
    RMS: stored the root-mean-squared error of the prediction

## Different initialisation and data rearranging strategies ('Gen_Load_Multiple_Time.m') may lead to different results ##
## You may use the provided .mat files to reproduce the results in the paper ##

***********************************************************
4. Use 'plot_EIS.m' to plot the best 8 groups of the predictions, followed by the worst 8 groups of the predictions (nyquist plot)
5. Use 'plot_ohmic.m' to plot the ohmic resistance calculated from the EIS trajectories.
6. Use 'plot_RMS.m' to plot the root-mean-squared error of all predictions, together with real and -imaginary impedance against the frequency. An example of the dynamic load profile is also given.




















