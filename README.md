# ImpliedVolProject
Implied Volatility Around Corporate Earnings: 

Data.zip: data import

Results.pdf: includes most important plots from the following files that do not appear on this platform

Smile.ipynb: First version with reading in of files, calculation of implied volatility smile, and implementation of SVI to entire curve
  Failed to capture W-shape, fitted normal parabola instead

Cubic Spline 4.ipynb: 
  Includes SVI and SVI_fit functions for calculating and fitting SVI parameterization
  In order to better capture the W shape a 2 stage approach is taken to fit a separate parameterization to each half of the data
  fit1 is the SVI parameterization for the left half of the data (where the options are in the money) 
  fit2 is for the right half (out of the money)
  temp dataframe for comparison of SVI calculated volatility vs actual implied volatility in Comb_vol column
  df427 is the application of this methodology using the fits from the AMZN option data maturing on April 27
  this creates an even grid of strike prices that range the scope of the data and are separated by $0.50
  after calculating the SVI volatility for this grid of strikes, call prices are calculated with Black-Scholes, and the Breeden-Litzenberger formula is applied to yield the probability densitity function of the underlying asset (AMZN stock)
  The fit is not continuous and differentiable at the point where the two parameterizations are joined meaning the PDF is not continuous
  The Cubic Spline implementation is intended to solve this by fitting a smooth parameterization over this double SVI fit that is ensured to be continuous and differentiable
  CS function fits cubic spline coefficients to given x and y data
  Selection of points is vital to preventing overfitting
  Interpolate function applies those coefficients to range of x values (strike prices) to output the cubic spline fit to the curve
  df427CS calculates call prices and PDF using cubic spline interpolation
  It is later redfined and recalculated using a different selection of points and therefore different number of coefficients in order to determine the optimal fit
  
Final SVI Cubic Spline.ipynb: Final version of code
  data input, cs, interpolation, and SVI functions are same as above version
  CS_dfs is defined and populated with cubic spline interpolations for each maturity
  process is same as above but consolidated into for loop: fit SVI to each half of data, fit CS over double SVI fit, calculate call prices and PDF from that fit
  ind_delta determines how many points are used in the cubic spline interpolation for each maturity since each has a different amount of data
  the cell at output[7] includes a dataframe that justifies the selection for each delta including the number of points for each maturity and the resulting delta and number of points to be included in CS fit
  sum_table dataframe calculates an expected value of the underlying stock price for each maturity using its PDF
  
  
FE_800_Final_Report.pdf: final group report including outputs and conclusions from this model in Section 5.1
