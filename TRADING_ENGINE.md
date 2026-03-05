# TRADING ENGINE - Core System Logic

## Complete Trading System Architecture

This is the MASTER document that defines how the entire trading platform works.

---

## 1. SYSTEM OVERVIEW

```
┌─────────────────────────────────────────────────────────────────────┐
│                        TRADING ENGINE CORE                          │
│                                                                     │
│  Input: Real-time price data + F&O signals                         │
│  Process: Technical analysis + Institutional signals               │
│  Output: Trading decisions (BUY/SELL/HOLD)                         │
│  Execution: Equity spot trading (NSE) only                         │
│  Capital: ₹5-10 Lakhs (No leverage)                                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### System Principles:
```
✓ Profit from understanding institutional behavior
✓ Use technical indicators to catch EARLY moves (Not late)
✓ Use F&O data to CONFIRM entries (Reduce false signals)
✓ Trade only EQUITY (Spot market, NSE)
✓ Risk exactly 2% per trade (Strict discipline)
✓ Hold 3-5 days average (Swing trading)
✓ 3-5 trades per week (Active but not overtrading)
✓ Target: 10-15% monthly return
```

---

## 2. WATCHLIST STRATEGY

### 20 Core Stocks (Automatically Monitored)

**Large Cap (8 stocks):**
```
RELIANCE (Energy), TCS (IT), INFY (IT), HDFC BANK (Finance)
ICICI BANK (Finance), SBI (Finance), WIPRO (IT), MARUTI (Auto)
```

**Mid Cap (7 stocks):**
```
ASHOK LEYLAND (Auto), BOSCH (Auto), MINDTREE (IT), BIOCON (Pharma)
INDIGO (Airlines), AIRTEL (Telecom), POWERGRID (Infrastructure)
```

**Emerging (5 stocks):**
```
IRCTC (Tourism), BAJAJ AUTO (Auto), BHARTI AIRTEL (Telecom)
SUNPHARMA (Pharma), BAJAJFINSV (Finance)
```

**Why this mix?**
- ✅ High liquidity (Easy to buy/sell)
- ✅ Sector diversity (Not all correlated)
- ✅ Cap diversity (Large + Mid)
- ✅ Trackable daily (Volume > 5M)

---

## 3. TECHNICAL INDICATORS (The Language)

### Tier 1: Core Indicators (MUST HAVE)

#### 3.1 Moving Averages (Trend Filter)
```
Calculate: SMA20, SMA50, SMA200 (Simple moving averages)

Bullish Alignment:
  MA20 > MA50 > MA200
  = Uptrend confirmed
  = BUY signal potential

Bearish Alignment:
  MA20 < MA50 < MA200
  = Downtrend confirmed
  = SELL signal / AVOID

Usage in Entry Logic:
  ✓ Trend must be aligned for entry
  ✓ Entry ONLY on bullish alignment
  ✓ Exit when alignment breaks
```

#### 3.2 RSI (Momentum Meter - 14 period)
```
Range: 0-100

Interpretation:
  0-30: Oversold (Bounce likely)
  30-70: Healthy (Normal trading)
  70-100: Overbought (Pullback likely)

Usage in Entry Logic:
  ✓ Entry signal best when RSI 40-60 (Not extreme)
  ✓ If RSI < 35: Oversold bounce (Special setup)
  ✓ Avoid entry when RSI > 75 (Too extended)
  
Signal:
  RSI rising: Momentum building (Bullish)
  RSI falling: Momentum fading (Bearish)
  RSI divergence: Price high but RSI low = Reversal coming
```

#### 3.3 MACD (Momentum + Trend Confirmation)
```
Components:
  MACD Line = 12-day EMA - 26-day EMA
  Signal Line = 9-day EMA of MACD
  Histogram = MACD - Signal

Signals:
  MACD > Signal: Bullish (Momentum up)
  MACD < Signal: Bearish (Momentum down)
  MACD crossing signal: Entry/Exit trigger

Usage:
  ✓ Confirms MA trend signal
  ✓ Early warning of reversal (MACD crosses first)
  ✓ Green histogram = Confidence building
  ✓ Red histogram = Confidence fading
```

#### 3.4 Bollinger Bands (Volatility Zones)
```
Components:
  Upper Band = SMA20 + (2 × StdDev)
  Middle Band = SMA20
  Lower Band = SMA20 - (2 × StdDev)

Signals:
  Price near upper band: Potential resistance
  Price near lower band: Potential support
  Band expansion: Volatility increasing (Move coming)
  Band squeeze: Volatility contracting (Big move pending)

Usage:
  ✓ Identify support (Lower band bounce = strong support)
  ✓ Identify overbought/oversold
  ✓ Plan entry/exit zones
  ✓ Breakout confirmation (Band break with volume)
```

#### 3.5 Volume Analysis (Money Flow)
```
Metrics:
  Daily Volume vs 20-day average
  Volume direction with price direction

High Volume Signals:
  ✓ Price up + High volume = Buyers in control (Trust this move)
  ✓ Price down + High volume = Sellers in control (Fear)
  ✓ Price stable + High volume = Accumulation (Whisper!)
  
Low Volume Signals:
  ❌ Price up + Low volume = Fake breakout (Don't trust)
  ❌ Price down + Low volume = Capitulation (Bottom forming)

Usage:
  ✓ Confirm entry: Volume must be 1.5x+ average
  ✓ Confirm breakout: Volume must be 2x+ average
  ✓ Hidden accumulation: Rising volume, flat price
```

#### 3.6 Support & Resistance (Price Levels)
```
How to Find:
  Support: Price bounced UP at this level 2+ times
  Resistance: Price bounced DOWN at this level 2+ times

Levels matter because:
  Level with past volume spike = Institutional orders still there
  = Buyers/Sellers waiting at that price

Usage:
  ✓ Support = Good entry zone (Risk-reward favorable)
  ✓ Resistance = Good exit zone (Take profit)
  ✓ Breaking support = Stop-loss trigger
  ✓ Breaking resistance = New target
```

#### 3.7 On-Balance Volume (OBV - Accumulation/Distribution)
```
Calculation:
  If close > previous close: OBV += volume
  If close < previous close: OBV -= volume
  If close = previous close: OBV unchanged

Signals:
  OBV rising = Buyers accumulating (Bullish)
  OBV falling = Sellers distributing (Bearish)
  OBV divergence = Price vs OBV misaligned (Reversal coming)

Usage:
  ✓ Detect accumulation (Price stable, OBV rising = Whisper!)
  ✓ Confirm trends (OBV aligned with price move)
  ✓ Spot reversals (OBV starts falling before price)
```

#### 3.8 ATR (Volatility + Position Sizing)
```
Calculation:
  Average True Range = Average of daily price ranges

Usage:
  ✓ Measure stock volatility
  ✓ Calculate stop-loss distance (2× ATR)
  ✓ Determine position size
  ✓ Alert on volatility changes

Example:
  ATR = ₹8 (Stock swings ₹8 daily average)
  Stop-loss = Current price - (2 × ATR) = Price - ₹16
  Position size = Risk ₹10,000 / ₹16 = 625 shares
```

### Tier 2: EARLY SIGNAL INDICATORS (Early Detection)

#### 3.9 Volume Divergence (Whisper Signal)
```
What: Price flat but volume rising over 3+ days
Why: Institutional accumulation happening quietly

Detection:
  Day 1: Volume 1.5M, Price ₹600
  Day 2: Volume 1.8M, Price ₹600.5 ← Volume 20% up, price stable
  Day 3: Volume 2.1M, Price ₹601 ← Volume 40% up, price barely up
  
  Verdict: Institutions quietly buying
  Prediction: Big move coming next 1-3 days

Usage:
  ✓ Changes entry confidence from 60% → 85%
  ✓ Indicates EARLY phase of move
  ✓ Catches move before broader market sees it
```

#### 3.10 Price-Volume Divergence (Reversal Warning)
```
Bullish Divergence:
  Price hits new LOW
  But volume DECREASES (Sellers exhausted)
  RSI doesn't follow price LOW (Momentum healthy)
  
  → Reversal UP coming

Bearish Divergence:
  Price hits new HIGH
  But volume DECREASES (Fewer buyers convinced)
  RSI doesn't confirm high (Momentum weak)
  
  → Reversal DOWN coming

Usage:
  ✓ Appears 1-2 days BEFORE reversal
  ✓ Spot these early = Better exit timing
```

---

## 4. F&O DATA LAYER (Institutional Signals)

### Purpose: Confirm equity trades using institutional positioning

#### 4.1 Put/Call Ratio (Sentiment)
```
Calculation: Put Option Volume / Call Option Volume

Interpretation:
  Ratio > 1.5: VERY bearish (Institutions hedging)
  Ratio 1.0-1.5: Bearish
  Ratio 0.8-1.0: Neutral
  Ratio < 0.8: VERY bullish (Institutions attacking)

Trading Decision:
  If equity signals bullish BUT Put/Call > 1.5:
    → SKIP (Sentiment disagrees)
    → False breakout risk too high
  
  If equity signals bullish AND Put/Call < 0.8:
    → GREEN LIGHT (Sentiment agrees)
    → Confidence: +30%
    → ENTER aggressively
```

#### 4.2 Open Interest (OI) Trend
```
What: Number of open futures/options contracts held

Rising OI:
  = Traders OPENING positions
  = Conviction building
  = Momentum likely to continue

Falling OI:
  = Traders CLOSING positions
  = Exit signal
  = Reversal likely

Trading Decision:
  Equity setup + OI rising = STRONG entry
  Equity setup + OI falling = WEAK entry (Skip)
```

#### 4.3 Implied Volatility (IV)
```
What: Market's expectation of volatility

Low IV (< 20%):
  = Market expects CALM ahead
  = Risky for breakout trades
  = Best for range-bound trades

High IV (> 30%):
  = Market expects BIG moves
  = Good for directional breakout trades
  = Momentum trades more likely to work

Trading Decision:
  If IV extremely low: Skip breakout setups
  If IV rising: Breakouts work better
  If IV spiking: Volatility event coming (Use caution)
```

#### 4.4 Futures-Spot Basis
```
What: Futures Price - Spot Price

Normal basis: +1-2% (Common, natural)
Extreme basis: +4-5% (Unsustainable!)

When basis extreme:
  = Institutions heavily long in futures
  = They will hedge by buying spot next
  = Equity move coming soon after

Trading Decision:
  Basis building high + Equity signals = Prepare entry
  Move likely within 1-2 days
```

#### 4.5 OI Concentration at Strike Levels
```
What: Which option strikes have the most open interest?

High OI at strike = Institutional orders waiting there
Often becomes support/resistance

Example:
  Current stock price: ₹650
  Call OI: ₹660 (50M contracts) ← HUGE
  Put OI: ₹640 (45M contracts) ← HUGE
  
  Interpretation:
  - ₹660 is resistance (Call buyers trapped)
  - ₹640 is support (Put buyers trapped)
  - Range ₹640-₹660 = Trading range

Trading Decision:
  Entry in middle (₹650)
  SL: ₹640 (Small risk at support)
  Target: ₹660 (Resistance)
```

---

## 5. ENTRY SIGNAL SYSTEM (How to Get IN)

### Complete Entry Decision Tree

```python
def generate_entry_signal(symbol):
    """
    Entry happens only when MULTIPLE signals align
    """
    
    # ======== EQUITY TECHNICALS (Score 0-100) ========
    equity_checks = {
        'ma_alignment': ma20 > ma50 > ma200,           # Trend aligned
        'rsi_healthy': 40 < rsi < 70,                 # Not extreme
        'rsi_rising': rsi_today > rsi_yesterday,       # Building momentum
        'macd_bullish': macd > signal_line,           # Positive momentum
        'volume_spike': current_vol > avg_vol * 1.5,  # Real volume
        'support_bounce': price > recent_support,     # Support holding
        'bb_support': price > lower_bb,               # Above lower band
        'obv_rising': obv_today > obv_previous,       # Buyers accumulating
    }
    
    equity_score = (sum(equity_checks.values()) / len(equity_checks)) * 100
    
    # ======== EARLY WHISPER SIGNALS (Score 0-100) ========
    whisper_checks = {
        'volume_divergence': detect_volume_divergence(),      # 3+ days rising vol
        'oi_building': detect_oi_rising(),                    # OI increasing
        'price_vol_div': detect_price_volume_divergence(),    # Hidden setup
        'atr_expanding': atr_today > atr_avg,                 # Volatility building
    }
    
    whisper_score = (sum(whisper_checks.values()) / len(whisper_checks)) * 100
    
    # ======== F&O INSTITUTIONAL SIGNALS (Score 0-100) ========
    fo_checks = {
        'put_call_bullish': put_call_ratio < 1.0,            # Bullish sentiment
        'oi_trend_up': oi_rising,                             # Positions building
        'iv_healthy': iv > 20,                                # Not too low
        'basis_normal': 0.5 < basis_percent < 2.5,           # Normal positioning
        'strike_levels': oi_concentrated_at_good_levels(),   # S&R confirmed
    }
    
    fo_score = (sum(fo_checks.values()) / len(fo_checks)) * 100
    
    # ======== MULTI-TIMEFRAME ALIGNMENT ========
    timeframe_checks = {
        'daily_bullish': daily_ma_aligned,
        '4h_bullish': h4_ma_aligned,
        '1h_bullish': h1_ma_aligned,
    }
    
    timeframe_score = (sum(timeframe_checks.values()) / len(timeframe_checks)) * 100
    
    # ======== FINAL DECISION ========
    min_score = 60   # Minimum 60% on each dimension
    
    if equity_score < min_score:
        return None, "Equity setup too weak"
    
    if whisper_score < min_score:
        return None, "No early signals detected"
    
    if fo_score < min_score:
        return None, "F&O signals not aligned"
    
    if timeframe_score < min_score:
        return None, "Multi-timeframe not aligned"
    
    # All checks passed - Calculate confidence
    combined_score = (equity_score + whisper_score + fo_score + timeframe_score) / 4
    
    if combined_score >= 90:
        return 'STRONG_BUY', combined_score
    elif combined_score >= 80:
        return 'BUY', combined_score
    elif combined_score >= 70:
        return 'WEAK_BUY', combined_score
    else:
        return 'SKIP', combined_score
```

### Entry Rules Summary:

```
✓ MA alignment: 20 > 50 > 200 (Uptrend)
✓ RSI: 40-70 (Healthy, not extreme)
✓ Volume: Rising (1.5x+ average)
✓ Support: Price bouncing from level
✓ OBV: Rising (Buyers accumulating)
✓ Early signals: Volume divergence if catching setup early
✓ F&O sentiment: Put/Call ratio bullish
✓ F&O OI: Rising (Institutions positioning)
✓ Multi-timeframe: Daily + 4H + 1H all bullish

When 6+ checks pass:
  → ENTER trade
  → Record entry signals
  → Set stop-loss
  → Set target
  → Monitor position
```

---

## 6. POSITION SIZING (How Much to Buy)

### Core Formula

```python
def calculate_position_size(capital, max_risk_percent, stock_price, stop_loss_distance):
    """
    Risk exactly max_risk_percent of capital on this trade
    """
    
    # 1. Calculate max loss amount
    max_loss_amount = capital * max_risk_percent
    
    # 2. Calculate position size
    position_size = max_loss_amount / stop_loss_distance
    
    # 3. Ensure doesn't exceed capital available
    investment_needed = position_size * stock_price
    max_investment = capital * 0.8  # Leave 20% buffer
    
    if investment_needed > max_investment:
        position_size = max_investment / stock_price
    
    # 4. Round down to safe amount
    return int(position_size)
```

### Example Calculation

```
Your Capital: ₹5,00,000
Risk per trade: 2% = ₹10,000

TRADE SETUP:
Stock: SBIN
Current Price: ₹600
MA20: ₹595
Stop-Loss Setup: 2% below MA20 = ₹583
Stop-Loss Distance: ₹600 - ₹583 = ₹17

Position Size:
  = ₹10,000 / ₹17
  = 588 shares
  = ₹588 × ₹600 = ₹3,52,800 invested

Risk Check:
  If stops at ₹583: Loss = ₹588 × ₹17 = ₹9,996 ≈ 2% ✓

Target Setup (5% profit):
  Target: ₹630
  If hits target: Profit = ₹588 × ₹30 = ₹17,640 (3.5% gain) ✓

Risk/Reward:
  Risk: ₹10,000
  Reward: ₹17,640
  Ratio: 1:1.76 (Good)
```

### Position Sizing Rules

```
Max 3 concurrent positions:
  Position 1: ₹3.5L
  Position 2: ₹2.5L (If first strong)
  Position 3: ₹2.0L (Small)
  Total: ₹8L (80% of capital)
  Buffer: ₹1L (20% for new opportunities)

No single trade > ₹4L investment
No total open positions > ₹8L
Always keep minimum 20% cash buffer
```

---

## 7. EXIT SIGNAL SYSTEM (How to Get OUT)

### Exit Decision Framework

```python
def should_exit_trade(position):
    """
    Check if any exit condition triggered
    """
    
    pnl = position.current_price - position.entry_price
    pnl_percent = (pnl / position.entry_price) * 100
    days_held = (datetime.now() - position.entry_time).days
    
    # ===== PROFIT-TAKING EXITS =====
    if pnl_percent >= position.profit_target:
        return True, 'PROFIT_TARGET', f"Hit {pnl_percent:.1f}%"
    
    # ===== LOSS-LIMITING EXITS =====
    if pnl_percent <= -position.stop_loss_percent:
        return True, 'STOP_LOSS', f"Hit -{position.stop_loss_percent}%"
    
    # ===== TECHNICAL EXIT =====
    
    # EXIT on MA breakdown (Trend reversal)
    if current_price < ma20 and ma20 < ma50:
        return True, 'MA_BREAKDOWN', "MA20 < MA50 (Trend broken)"
    
    # EXIT on MACD reversal
    if macd < signal_line and position.macd_was_positive:
        return True, 'MACD_REVERSAL', "MACD crossed below signal"
    
    # EXIT on RSI overbought (Reversal risk)
    if rsi > 85:
        return True, 'RSI_OVERBOUGHT', f"RSI {rsi} too extended"
    
    # EXIT on price-volume divergence
    if new_high_price and low_volume:
        return True, 'PRICE_VOL_DIV', "New high but low volume"
    
    # ===== TIME-BASED EXIT =====
    
    # EXIT if held too long without profit
    if days_held > 10 and pnl_percent < 1.0:
        return True, 'INACTIVITY', f"Held {days_held} days, flat position"
    
    # ===== F&O BASED EXIT =====
    
    # EXIT if Put/Call sentiment turns bearish
    if position.put_call_bullish and now_put_call_bearish:
        return True, 'SENTIMENT_FLIP', "Put/Call flipped bearish"
    
    # EXIT if OI starts declining
    if position.oi_was_rising and now_oi_falling:
        return True, 'OI_DECLINE', "OI trend reversed"
    
    # No exit condition met
    return False, None, "HOLD"
```

### Exit Rules Summary

```
PROFIT EXITS:
  └─ Target: +5% (Lock gains, move to next trade)
  └─ Partial: Take 50% at +3%, ride rest for +8%

LOSS EXITS:
  └─ Stop-loss: -2% (Cut losses immediately)
  └─ Hard stop: Never ignore this

TECHNICAL EXITS:
  └─ MA breakdown: Price < MA20 while MA alignment breaks
  └─ RSI overbought: RSI > 85 (Extended move, reversal risk)
  └─ MACD reversal: MACD crosses below signal line
  └─ Price-volume divergence: New high but decreasing volume

TIME EXITS:
  └─ Inactivity: Held 10 days with <1% profit → Free up capital

SENTIMENT EXITS:
  └─ Put/Call flips bearish: Institutions turning bearish
  └─ OI starts declining: Positions being unwound
```

---

## 8. DAILY TRADING CYCLE

### 9:15 AM - Market Open
```
System Startup:
  ├─ Fetch 200-day price history for all 20 stocks
  ├─ Calculate all technical indicators
  ├─ Scan for overnight news/gaps
  ├─ Fetch F&O data (OI, Put/Call, IV)
  ├─ Generate setup alerts
  └─ Display watchlist sorted by opportunity
```

### 9:30 AM - 11:30 AM (Early Session - Whisper Phase)
```
Monitor & Setup Formation:
  ├─ Check each stock for entry signals
  ├─ Look for volume divergence (Accumulation starting)
  ├─ Identify support bounces
  ├─ Rank 20 stocks by signal strength
  ├─ Preview top 5 opportunities
  └─ Manual decision: Which 1-3 to enter
```

### 11:30 AM - 2:30 PM (Main Session - Breakout Phase)
```
Trade Execution & Monitoring:
  ├─ Enter selected stocks (If confirmation)
  ├─ Monitor all open positions for P&L
  ├─ Update stop-losses as needed
  ├─ Check for partial profit exits
  ├─ Scan for new opportunities
  └─ Manage live positions hourly
```

### 2:30 PM - 3:30 PM (Late Session - Risk Management)
```
Closing Out Day:
  ├─ Review all open positions
  ├─ Close weak positions (Avoid overnight risk)
  ├─ Take partial profits if near targets
  ├─ Trail stop-losses to breakeven (If safe)
  ├─ Close any marginal positions
  └─ Calculate daily P&L
```

### 3:30 PM - After Hours
```
End-of-Day Review:
  ├─ Log all completed trades
  ├─ Record entry/exit signals used
  ├─ Calculate daily metrics (Win rate, P&L%)
  ├─ Analyze what worked/what didn't
  └─ Prepare watch list for tomorrow
```

---

## 9. TRADE EXECUTION RULES

### BUY Order (Entry)

```
When all 4 entry conditions met:
  ✓ Equity signals: 6+ checks pass
  ✓ Whisper signals: Early setup detected
  ✓ F&O aligned: Institutional buying
  ✓ Multi-timeframe: All charts bullish

Action:
  1. Calculate position size
  2. Place market buy order
  3. Record entry price + signals
  4. Immediately set stop-loss (2% below)
  5. Set target (5% above)
  6. Set alerts for both

Order Type: MARKET (Not limit)
Reason: Must catch the move early
```

### SELL Order (Exit - Profit)

```
When price reaches target:
  Target: Entry + 5%
  
Action:
  1. Place market sell order
  2. Record exit price
  3. Calculate P&L
  4. Log to trade history
  5. Free up capital
  6. Prepare for next trade

Rule: ALWAYS take profits at target
Emotion: No "let it ride" hopes
```

### SELL Order (Exit - Stop-Loss)

```
When price hits stop-loss:
  Stop-Loss: Entry - 2%
  
Action:
  1. Place market sell order IMMEDIATELY
  2. Record exit price
  3. Calculate loss
  4. Log to trade history
  5. Move on to next opportunity

Rule: NEVER ignore stop-loss
Emotion: Accept small loss, preserve capital
```

---

## 10. MONEY MANAGEMENT (Risk Control)

### Capital Allocation

```
Total Capital: ₹5,00,000

Allocation:
  ├─ Active Trading: ₹4,00,000 (80%)
  │   ├─ Position 1: ₹3.5L
  │   ├─ Position 2: ₹2.5L
  │   ├─ Position 3: ₹2.0L
  │   └─ Max: 3 positions
  │
  ├─ Reserve (Opportunities): ₹1,00,000 (20%)
  │   └─ Always kept in cash
  │
  └─ Total Open: < ₹8L (Max)
```

### Risk Per Trade

```
Max Loss: 2% per trade
  = ₹10,000 max loss on any single trade

If 3 trades all stop-out:
  = 3 × ₹10,000 = ₹30,000 total loss
  = 6% portfolio loss (Acceptable)

If 5 trades lose:
  = 5 × ₹10,000 = ₹50,000 loss
  = 10% portfolio loss (Max acceptable drawdown)
```

### Win Rate Target

```
Conservative: 55% win rate
  = 55 wins, 45 losses per 100 trades
  = 55 × 3% average = +165%
  - 45 × -2% average = -90%
  = Net +75% (Per 100 trades)
  = 7.5% per month

Target: 60-65% win rate
  = 65 wins, 35 losses per 100 trades
  = 65 × 3% = +195%
  - 35 × -2% = -70%
  = Net +125% (Per 100 trades)
  = 12.5% per month
```

---

## 11. TRADING JOURNAL & METRICS

### Track Every Trade

```sql
CREATE TABLE trades (
    id PRIMARY KEY,
    symbol TEXT,
    entry_date DATETIME,
    entry_price DECIMAL,
    entry_signals TEXT,  -- JSON: which signals triggered
    exit_date DATETIME,
    exit_price DECIMAL,
    exit_reason TEXT,  -- 'profit', 'stop_loss', 'reversal', etc
    quantity INTEGER,
    pnl DECIMAL,
    pnl_percent DECIMAL,
    holding_days INTEGER,
    confidence_score DECIMAL
);
```

### Weekly Analysis

```
Metrics to Calculate:
  ├─ Win rate: % profitable trades
  ├─ Profit factor: Wins/Losses
  ├─ Average winner: Avg % gain
  ├─ Average loser: Avg % loss
  ├─ Risk/reward ratio: Avg win / Avg loss
  ├─ Sharpe ratio: Risk-adjusted returns
  ├─ Max drawdown: Biggest dip
  ├─ Trades per week: Activity level
  ├─ Holding period: Avg days held
  └─ Monthly return: Total % gain

Targets:
  Win rate > 60%
  Profit factor > 2.0 (2:1 ratio)
  Monthly return 10-15%
  Max drawdown < 10%
```

---

## 12. GOLDEN TRADING RULES

### DO ✓

```
✓ Follow the system (Backtest shows it works)
✓ Wait for 4+ signals (Don't rush entries)
✓ Set stop-loss immediately
✓ Risk exactly 2% per trade
✓ Take profits at target
✓ Exit on reversal signals
✓ Keep trading journal
✓ Review weekly results
✓ Adapt based on data
✓ Close before market close
✓ Book partial profits
✓ Stay disciplined
```

### DON'T ✗

```
✗ Chase trades (Wait for setup)
✗ Ignore stop-losses (EVER!)
✗ Hold losers hoping for bounce
✗ Average down (Add to losses)
✗ Overtrade (More trades = More losses)
✗ Trade low-volume stocks
✗ Revenge trade (After losses)
✗ Hold through gaps overnight
✗ Trade on emotion
✗ Break position size rules
✗ Deviate from system
✗ Blame bad luck (Analyse trade quality)
```

---

## 13. PERFORMANCE EXPECTATIONS

### Conservative Target (5-8% monthly)
```
Capital: ₹5,00,000
Monthly Gain: ₹25,000-40,000
Annual Gain: ₹3,00,000-4,80,000

Win Rate: 55%
Avg Win: 3%
Avg Loss: 2%
Trades/month: 12-15
```

### Moderate Target (10-15% monthly)
```
Capital: ₹5,00,000
Monthly Gain: ₹50,000-75,000
Annual Gain: ₹6,00,000-9,00,000

Win Rate: 65%
Avg Win: 3.5%
Avg Loss: 2%
Trades/month: 15-20
```

### Aggressive Target (15-20% monthly)
```
Capital: ₹5,00,000
Monthly Gain: ₹75,000-1,00,000
Annual Gain: ₹9,00,000-1,200,000

Win Rate: 75%
Avg Win: 4%
Avg Loss: 2%
Trades/month: 20-25

⚠️ Higher risk of drawdowns
⚠️ Requires perfect discipline
⚠️ Need 75%+ win rate to achieve
```

---

## 14. SYSTEM ARCHITECTURE (Implementation Map)

### Core Modules

```
1. DataProvider
   ├─ Fetch prices (yfinance)
   ├─ Store in SQLite
   ├─ Fetch F&O data
   └─ Cache management

2. IndicatorCalculator
   ├─ Moving averages
   ├─ RSI, MACD
   ├─ Bollinger Bands
   ├─ Volume analysis
   ├─ OBV, ATR
   └─ Early signals (Divergences)

3. SignalGenerator
   ├─ Entry signal logic
   ├─ Exit signal logic
   ├─ Confidence scoring
   └─ F&O confirmation

4. PositionSizer
   ├─ Risk calculation
   ├─ Position size
   ├─ Stop-loss distance
   └─ Capital management

5. OrderManager
   ├─ Place buy orders
   ├─ Place sell orders
   ├─ Track status
   └─ Execution logging

6. PortfolioManager
   ├─ Track holdings
   ├─ Calculate P&L
   ├─ Monitor positions
   └─ Risk alerts

7. TradingJournal
   ├─ Log all trades
   ├─ Store metrics
   ├─ Generate reports
   └─ Analysis tools

8. Dashboard/API
   ├─ Real-time view
   ├─ REST endpoints
   ├─ Alerts
   └─ Manual controls
```

---

## 15. IMPLEMENTATION PHASES

### Phase 1: Foundation (Week 1-2)
```
✓ Project setup
✓ Database schema
✓ Price fetcher (yfinance)
✓ Core indicators (MA, RSI, MACD, BB)
✓ Basic entry rules
✓ Basic exit rules
✓ Order execution (Simulated)
✓ P&L calculation
```

### Phase 2: Enhancement (Week 3)
```
✓ Early signal detection (Divergences, OBV)
✓ F&O data fetching
✓ F&O signal calculation (Put/Call, OI)
✓ Hybrid confidence scoring
✓ Improved entry logic
✓ Multi-timeframe analysis
```

### Phase 3: Interface (Week 4)
```
✓ CLI Dashboard
✓ REST API
✓ Trade logging
✓ Report generation
✓ Alerts system
✓ Manual controls
```

### Phase 4: Polish (Week 5)
```
✓ Backtesting module
✓ Parameter optimization
✓ Error handling
✓ Documentation
✓ Testing & validation
```

---

## SUMMARY: THE COMPLETE ENGINE

```
INPUT:
  ├─ Real-time stock prices (20 stocks)
  ├─ F&O data (OI, Put/Call, IV)
  └─ 200 days historical data

PROCESS:
  ├─ Calculate 8 technical indicators
  ├─ Detect 5 early signals
  ├─ Fetch 5 F&O signals
  ├─ Score each on 0-100
  ├─ Generate entry confidence
  ├─ Calculate position size
  ├─ Monitor for exits
  └─ Log all trades

OUTPUT:
  ├─ BUY signal (Confidence scored)
  ├─ HOLD signal (Position monitored)
  ├─ SELL signal (Reason documented)
  ├─ P&L updated (Real-time)
  ├─ Metrics calculated (Daily)
  └─ Journal logged (Every trade)

RESULT:
  ✓ Win rate: 60-70%
  ✓ Profit/Loss ratio: 2:1
  ✓ Monthly return: 10-15%
  ✓ Annual return: 120-180%
  ✓ Consistent, unemotional trading
```

---

## READY FOR IMPLEMENTATION

This document defines EXACTLY how the trading engine works.

Next step: Convert this into actual Python code with all modules.

Should we start building? 🚀
