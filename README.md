LRA Market Maker Strategy for TradingView
Overview
The LRA (Liquidity Range Analysis) Market Maker Strategy is a sophisticated Pine Script designed for TradingView. Its core purpose is to identify periods of market consolidation, or "locked-in ranges," and to systematically trade the resolution of these ranges.

The strategy operates on the "trapped trader" thesis. It hypothesizes that when price breaks out of a well-defined range, one group of participants is inevitably caught offside. By identifying these scenarios with precision, the strategy aims to enter trades that capitalize on the subsequent stop-loss cascades and momentum squeezes.

Core Strategic Concepts
The strategy is built upon two primary entry scenarios, both designed to exploit different groups of trapped traders.

1. Locked-in Range (LR) Identification
First, the script identifies a "Locked-in Range" where buyers and sellers have reached a temporary equilibrium. This is defined by a recent high and low within which the price has consolidated for a user-defined number of bars. This range represents a battlefield where liquidity is building.

2. The Trapped Trader Thesis
a) Standard Breakout (Trapping Range Traders)
Logic: When the price closes decisively above the established range, it's assumed that traders who were shorting within the range are now trapped. Their stop-losses, likely placed just above the range high, are triggered, adding fuel to the upward move. The strategy enters LONG.

Conversely: When the price closes decisively below the range, range-bound buyers are trapped. The strategy enters SHORT to capitalize on their capitulation.

b) False Breakout (FB) (Trapping Breakout Traders)
Logic: This is a more nuanced, mean-reversion setup. Price will first poke outside the range, luring in early breakout traders. However, it fails to find acceptance and quickly reverses back inside the range.

Result: The early breakout traders are now instantly trapped. The strategy identifies this failure and enters a trade against them (e.g., if it was a false breakout to the upside, the strategy enters SHORT), anticipating a powerful squeeze.

Script Features
The Pine Script is highly customizable to allow for precise calibration across different assets and timeframes.

Key Features:
Dynamic Range Identification: Uses a lookback period and consolidation bar count to dynamically identify significant ranges.

Dual Entry Logic: Implements both Standard Breakout and False Breakout entry models.

Dynamic Take Profit & Stop Loss:

Stop Losses are intelligently placed on the opposite side of the identified range, buffered by a user-defined ATR multiple.

Take Profits are calculated dynamically, using a choice between prior market structure (swing highs/lows) or a projection based on the range height.

Volume Confirmation: Optional filter to ensure that breakouts are not occurring on unusually low or high volume, which can signal a lack of conviction or an exhaustive move.

Risk Management & Position Sizing: Automatically calculates position size based on a fixed-fractional risk model (% of equity per trade).

Session Filtering: Allows traders to confine the strategy's operation to specific market sessions (e.g., London, New York).

Automated Alert System: Designed to send clean, structured JSON data directly to a personal server endpoint via webhooks for full automation.

Webhook Integration Guide
The script is built to send trade signals directly to your own server endpoint, giving you full control over alert formatting and delivery.

1. Configure the Script Inputs
In the script settings on TradingView, fill in the following fields under the "Alerts" group:

Your Server Webhook URL: The full URL of your bot's endpoint on your server (e.g., https://your-server.com/api/tradingview-signal).

Asset Name: A human-readable name for the asset (e.g., "Bitcoin").

Asset Image URL: A public URL to a logo for the asset, for use in formatted messages.

Asset Type (for Topic/Channel): A category string (e.g., "CRYPTO", "FX") that your server can use to route the message to the correct Discord channel or Telegram topic.

2. The JSON Payload
When a trade is triggered, the script will send a POST request to your webhook URL with the following JSON body:

{
  "asset_name": "Bitcoin",
  "asset_type": "CRYPTO",
  "symbol": "BINANCE:BTCUSDT",
  "image_url": "[https://s3.tradingview.com/userpics/622435-wsSp_orig.png](https://s3.tradingview.com/userpics/622435-wsSp_orig.png)",
  "trade_type": "False Breakout",
  "direction": "LONG",
  "entry_price": 102500.5,
  "take_profit": 105300.0,
  "stop_loss": 101800.0,
  "timeframe": "4H"
}

Your server-side code should be set up to parse this JSON and handle the dispatch to Discord, Telegram, or any other service.

3. Create the Alert in TradingView
Click the "Alert" icon on your TradingView chart.

Condition: Select the "LRA MM Strategy..." script.

Notifications Tab: Tick the "Webhook URL" box. The URL from your script settings will be used.

Settings Tab: In the Message box, you MUST use this exact placeholder:

{{strategy.order.alert_message}}

Create a separate alert for each of the four entry conditions (LRA Std Long, LRA Std Short, LRA FB Long, LRA FB Short).

Calibration
This is not a "plug-and-play" strategy. Its performance is highly dependent on calibrating the input parameters to the specific asset and timeframe you are trading. Use the Strategy Tester extensively to optimize settings.

Key parameters to focus on during calibration:

lr_detect_lookback

lr_min_bars_consolidation

lr_min_height_atr / lr_max_height_atr

fb_max_penetration_pct_lr

fb_return_bars

Disclaimer
Trading involves significant risk. This strategy is provided for educational and informational purposes only. Past performance is not indicative of future results. Do not risk capital that you cannot afford to lose.
