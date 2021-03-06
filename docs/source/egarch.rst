EGARCH models
==================================

Example
----------

.. code-block:: python
   :linenos:

   import numpy as np
   import pyflux as pf
   from pandas.io.data import DataReader
   from datetime import datetime

   ibm = DataReader('IBM',  'yahoo', datetime(2000,1,1), datetime(2016,3,10))
   ibm['Logged Open'] = np.log(ibm['Open'].values)
   model = pf.EGARCH(np.diff(ibm['Logged Open']),p=1,q=1)

Class Arguments
----------

The **EGARCH()** model class has the following arguments:

* *data* : requires a pd.DataFrame object or an np.array
* *p* : the number of GARCH terms
* *q* : the number of score terms
* *target* : (default: None) specify the pandas column name or numpy index if the input is a matrix. If None, the first column will be chosen as the data.

Class Attributes
----------

An **EGARCH()** object holds the following attributes:

Model Attributes:

* *p* : the number of GARCH terms
* *q* : the number of score terms
* *param_no* : number of model parameters
* *index* : the timescale of the time-series
* *data* : the dependent variable held as a np.array
* *data_name* : string variable containing name of the time series
* *data_type* : whether original datatype is numpy or pandas

Parameter Attributes:

The attribute *param.desc* is a dictionary holding information about individual parameters:

* *name* : name of the parameter
* *index* : index of the parameter (begins with 0)
* *prior* : the prior specification for the parameter
* *q* : the variational distribution approximation

Inference Attributes:

* *params* : holds any estimated parameters
* *ses* : holds any estimated standard errors for parameters (MLE/MAP)
* *ihessian* : holds any estimated inverse Hessian (MLE/MAP)
* *chains* : holds trace information for MCMC runs
* *supported_methods* : which inference methods are supported 
* *default_method* : default inference method

Class Methods
----------

**adjust_prior(index,prior)**

Adjusts a prior with the given parameter index. Arguments are:

* *index* : taking a value in range(0,no of parameters)
* *prior* : one of the prior objects listed in the Bayesian Inference section

.. code-block:: python
   :linenos:

   model.list_priors()
   model.adjust_prior(2,ifr.Normal(0,1))

**fit(method)**

Fits parameters for the model. Arguments are:

* *method* : one of ['BBVI',MLE','MAP','M-H','Laplace']
* *printed* : (default: True) whether to print output
* *nsims* : (default: 100000) how many simulations if M-H is chosen
* *cov_matrix* (default: None) covariance matrix for M-H
* *iterations* : (default: 30000) how many iterations if BBVI is chosen
* *step* : (default: 0.001) step size for BBVI

.. code-block:: python
   :linenos:

   model.fit("M-H",nsims=20000)

**list_priors()**

Lists the current prior specification.

**plot_fit()**

Graphs the fit of the model.

**plot_predict(h)**

Predicts h timesteps ahead and plots results. Arguments are:

* *h* : (default: 5) how many timesteps to predict ahead
* *past_values* : (default: 20) how many past observations to plot
* *intervals* : (default: True) whether to plot prediction intervals

**plot_predict_is(h)**

Predicts rolling in-sample prediction for h past timestamps and plots results. Arguments are:

* *h* : (default: 5) how many timesteps to predict
* *past_values* : (default: 20) how many past observations to plot
* *intervals* : (default: True) whether to plot prediction intervals

**predict(h)**

Predicts h timesteps ahead and outputs pd.DataFrame. Arguments are:

* *h* : (default: 5) how many timesteps to predict ahead

**predict_is(h)**

Predicts h timesteps ahead and outputs pd.DataFrame. Arguments are:

* *h* : (default: 5) how many timesteps to predict ahead

.. code-block:: python
   :linenos:

   model.plot_predict(h=12,past_values=36)