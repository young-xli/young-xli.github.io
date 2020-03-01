---
title: Stock Trading Sessions and Price Volatility Scaling
---

Trading Sessions
----------------

For both NYSE and NASDAQ in US, normal trading hours is from 9:30 a.m. to 4 p.m., lasting 6.5 hours. Extended-hours (after-hours and pre-market) tradings practically accounts for about 3.5 hours: 

<table>
<thead>
<tr> <th align="left">Session</th>  <th align="left"> Time Period</th> <th align="left"> Hours </th> </tr>
</thead>
<tbody>
<tr> <td> Normal </td> <td> 9:30 AM - 4:00 PM </td> <td> 6.5 </td></tr>
<tr> <td> After-hours </td> <td> 4:00 PM - 6:00 PM <sup>[1]</sup> </td> <td> 2 </td></tr>
<tr> <td> Pre-market </td> <td> 8:00 AM - 9:30 AM <sup>[2]</sup> </td> <td> 1.5 </td> </tr>
</tbody>
</table>

<p style = "font-family:georgia,garamond,serif;font-size:11px;font-style:italic;"> <sup>[1]</sup> After-hours trading starts at 4 PM and ends at around 8 PM, but the amount of volume generally slows significantly by 6 PM ("After-Hours Trading". Investopedia).<br/>
<sup>[2]</sup> Pre-market trading occurs from 4:00 AM to 9:30 AM, but the majority of the volume and liquidity come to the pre-market at 8:00 AM ( "Pre-Market Trading". Investors Underground. Retrieved 11 November 2016 ).</p>


The 30-day volatility measure is defined as the annualized standard deviation of the relative price change for the 30 most recent trading days closing price (this is the definition by FLDS on Blooomberg Terminal). It's therefore a measure of close-to-close price fluctuations. 

A question naturally arises: can I expect the same amount of price volatility if I trade in after-hours session, compared to the normal session?

Let's break the time interval between two consecutive market closes down to the following two sessions:

- (t-1) CLOSE --- t OPEN
- t OPEN --- t CLOSE

Variances of Log-Price Increments
---------------------------------

Denote by $$C_{t-1}$$, $$O_t$$ acd $$C_t$$ respectively the close price at (t-1), open price at t and close price at t. We then calculate the following variances: <br/>
$$
\begin{equation}
\begin{aligned}
V_{co} &= \text{Var} [ log(O_t / C_{t-1}) ] \\
V_{oc} &= \text{Var} [ log(C_t / O_t) ]  \\
V_{cc} &= \text{Var} [ log(C_t / C_{t-1}) ] \\
\end{aligned}
\label{eq:1}
\tag{1}
\end{equation}
$$

Six stocks, two from top, middle and bottom SP500 index member section respectively, and SP500 index ETF (SPY) are picked for variance decomposition analysis. Date range is from 2000-01-01 to 2020-02-21.

| **Section** | **Tickers** |
|-------------|-------------|
| top SP500 members | AAPL, MSFT |
| middle SP500 members | MCHP, KR |
| bottom SP500 memebrs | HOG, GPS |
| ETF | SPY |

$$(V_{co} + V_{oc})$$ / $$V_{cc}$$ timeseries for the seven securities (top figure). Except for a few spikes, this fraction varies about 1, averaging to 1.03: $$\frac{(V_{co} + V_{oc})}{V_{cc}} \approx 1$$. 

This is expected, as $$log(C_t/C_{t-1}) = log(O_t/C_{t-1}) + log(C_t/O_t)$$ and log-price increments ($$log(O_t) - log(C_{t-1})$$) and
($$log(C_t) - log(O_t)$$) can be considered approximately independent of each other based on Brownian motion theory.

$$V_{co} / V_{oc}$$ time series (bottom figure).The average level of this fraction is higher during 2011-2020 than during 2000-2010

<table>
<thead>  
  <tr> <th>Period</th>  <th> $$\overline{V_{co} / V_{oc}}$$ </th> </tr>
</thead>
<tbody>  
  <tr> <td> 2000-2010 </td> <td> 0.4 </td> </tr>
  <tr> <td> 2011-2020 </td> <td> 0.6 </td> </tr>
</tbody>  
</table>


![](<../images/stock_vol_decom.png>)

Scaling of Price Volatility
---------------------------

Using recent period 2011-2020 $V_{co} / V_{oc}$ value of 0.6, we can express daily volatility in terms of normal-session volatility: <br/>
$$
\begin{equation}
V_{cc} = V_{co} + V_{oc} = 1.6 V_{oc}
\label{eq:2}
\tag{2}
\end{equation}
$$
Considering the normal session lasts for 6.5 hrs, we can say from price volatility perspective that one day = 1.6 * 6.5 = 10.4 normal-session hours, and therefore <br/>

<table>
  <tr> <th>Calendar Hour</th>  <th> Volatility-Equivalent Days </th> </tr>
  <tr> <td> 1 normal-session trading hour </td>  <td>  1/10.4 = 0.096 days </td> </tr>
  <tr> <td> 1 extended-hours trading hour </td>  <td> (0.6 * 6.5/3.5) * 0.096 = 0.107 days </td> </tr>
</table>

Using the above conversion, we can calculate price volatility for a given trading period using realized 30-day volatility, say $\sigma_d$ = 30\% annually = 30\%/$\sqrt{252}$ daily = 1.9\% daily.

<table>
  <tr> <th>Trading Hours</th>  <th> Volatility Using Volatility-Equivalent Time</th> <th> Volatility Using Calendar Time </th></tr>
  <tr> <td> 1 normal-session trading hours </td>  <td> $$\sigma_d * \sqrt{1 * 0.096} = 0.59\%$$ </td> <td> $$\sigma_d * \sqrt{1/24} = 0.39\%$$ </td> </tr>
  <tr> <td> 1 after-hours trading hours </td> <td> $$\sigma_d * \sqrt{1 * 0.107} = 0.62\%$$ </td> <td> $$\sigma_d * \sqrt{1/24} = 0.39\%$$ </td> </tr>
</table>

As shown above, volatilities calculated using calendar time are significantly lower than using volatility-equivalent time. Using the former can greatly underestimate the real price volatility in practice.
