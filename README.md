# Market Model Introduction

The volatility of the model’s states (the spanning Libors) can be specified through calibration or by inputting an instantaneous volatility surface and an instantaneous correlation matrix or by inputting parameters for a functional form volatility and correlation.

For both piecewise constant and linear-exponential (a.k.a. parametric) volatility – the volatility of a cap or floorlet fixing at time t has volatility up to and including time t; that is, a cap or floorlet still has volatility on its reset date.

Instantaneous log normal volatility matrix – a square matrix; first column is observation dates; first row is caplet termination dates.  The start date of the first caplet is the valuation date.  The start date of the ith caplet is the termination date of the (i-1)th caplet.  The observation dates must match the termination dates.

Instantaneous correlation matrix – a square matrix; the same size as volatility matrix; the first column and first row are caplet termination dates. Axis 2 is ignored. The dates must match the volatility matrix dates.

The build method “Tenor” truncates the volatilities, correlation and dates if the caplet termination dates exceed the specified model length. These caplet dates specify the rates that will be simulated. The accrual convention for the accrual periods of the simulated rates is taken from the index.

The data conventions of the caplet volatilities and correlation must be identified in the model build configuration. The example below is for a model that will simulate 3M USD LIBOR rates with up to 4 factors.

In order to drive the model with fewer factors, the rank-reduced pseudo square roots of the states’ integrated covariance are required. The rank-reduced integrated states’ covariance is also required for calculations during calibration. However, calibration varies the states covariance and is it not practical to repeatedly perform rank reduction. Instead, the states’ instantaneous correlation (which is not varied) is rank reduced and used to generate an approximation to the rank reduced integrated covariance.

It is possible to calculate the instantaneous correlation of all states at each of the discrete times and then rank reduce each matrix. However, this has proved to be impractical during calibration as it requires storing and accessing a large, three-dimensional set of data. Of course, if correlation is time-independent, then only one correlation matrix is needed. In the time-homogeneous case a transform is applied to enable instantaneous correlation to be handled in a single matrix.

In order to present rank-reduced, time-homogeneous, instantaneous correlation is a single matrix, the time-homogeneous parametric correlation is discretised by creating matrix of instantaneous correlation against time to expiry, (Tk-t), for a set of equi-spaced times.  The frequency of this spacing is chosen to match the specified model frequency.  For example, a 6M model will have a spacing of 0.5 years.  This matrix is then rank-reduced by PCA to the specified number of factors. 

Valuation-time rank reduction takes only the number of states required for the given product valuation and rank reduces their correlation and covariance accordingly. This could mean that the correlation of 10 states is reduced to 5 factors or that the correlation of 140 states is reduced to 5 factors, depending upon the length of the product being valued.

References:

https://finpricing.com/lib/EqBarrier.html

