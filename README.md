// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © AstrideUnicorn

//@version=5
indicator('Diluted EPS Signal', shorttitle='DEPSS' ,overlay=true)

// Input earnings surprise thershold
threshold1 = input.float(defval=20, title='Earnings Surprise Threshold (%)', step=0.01)

// Get Eanings Per Share Estimate and Earnings Per Share Diluted
EPS_estimate = request.earnings(syminfo.tickerid, earnings.estimate)
EPS_diluted = request.financial(syminfo.tickerid, 'EARNINGS_PER_SHARE_DILUTED', 'FQ')

// Convert percentafe value of the treshold to a decimal one
threshold = threshold1/100

// Calcuate the absolute value of eranings suprise
absolute_earnings_surprise = math.abs((EPS_diluted - EPS_estimate) / EPS_estimate)

// Condition that determines if the current earnings signal is significant
significant = absolute_earnings_surprise >= threshold

// Define BUY and SELL signals
buy_signal = EPS_diluted - EPS_estimate >= 0 and significant
sell_signal = EPS_diluted - EPS_estimate < 0 and significant

// Calculate eranings date
Earnings_date = ta.barssince(EPS_estimate - EPS_estimate[1] != 0) == 0

// plot signal arrows
plotshape(Earnings_date and buy_signal, style=shape.triangleup, color=color.new(color.green, 0), location=location.belowbar, size=size.normal)
plotshape(Earnings_date and sell_signal, style=shape.triangledown, color=color.new(color.red, 0), location=location.abovebar, size=size.normal)

