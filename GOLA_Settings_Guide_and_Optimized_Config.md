# GOLA .set File — Complete Variable Guide + Optimized Configuration

---

## PART 1: Every Variable Explained (Simple Analogies)

---

### 🏷️ IDENTITY SETTINGS

#### `InpSecTrade = GOLD SCALPER`
**What it does:** Just a label/name for the EA shown on your chart.
**Analogy:** The name tag on your shop's door. Doesn't affect trading.

#### `InpMagic = 10001`
**What it does:** A unique ID number so the EA can recognize which trades belong to it.
**Analogy:** Like a restaurant order number. If you run multiple EAs on the same account, each needs its own magic number so they don't interfere with each other's trades.
**Risk:** None — just an identifier.

#### `InpComment = GOLD SCALPER`
**What it does:** A text comment attached to every trade in your history.
**Analogy:** A sticky note on each order — helps you identify GOLA trades when reviewing history.

#### `InpSlippage = 3`
**What it does:** Maximum price difference (in points) the EA will accept when opening/closing a trade. If the broker gives you a price more than 3 points worse than requested, the EA rejects the trade.
**Analogy:** You want to buy a phone for ₹50,000. You tell the shop: "I'll accept up to ₹50,030 but not more." If they quote ₹50,050, you walk away.
**Risk:** Too tight (1) = many rejected trades. Too loose (10) = bad fills accepted.
**Your current: 3** → ✅ Good. Conservative but not too tight.

---

### 💰 MONEY MANAGEMENT

#### `InpLotsPer1000 = 0.01`
**What it does:** For every $1,000 in your account, the EA uses 0.01 lot size.
- $500 account → 0.01 lots (minimum)
- $1,000 → 0.01 lots
- $2,000 → 0.02 lots
- $5,000 → 0.05 lots
- $10,000 → 0.10 lots

**Analogy:** It's your "bet size per ₹1,000 in your wallet." If you have ₹5,000, you bet 5× the base amount.
**Risk:** Higher value = bigger bets = bigger wins AND bigger losses.
**Your current: 0.01** → ✅ Very conservative. Good for testing.

#### `InpMinLots = 0.01`
**What it does:** The absolute smallest trade the EA will ever place.
**Analogy:** "I will never bet less than ₹10, even if my formula says ₹5."
**Your current: 0.01** → ✅ Good. This is the broker minimum for most.

#### `InpMaxLots = 5.0`
**What it does:** The absolute largest trade the EA will ever place.
**Analogy:** "Even if my account has ₹10 crore, I will never bet more than ₹5 lakh on a single trade."
**Your current: 5.0** → ⚠️ **Too high.** On a $500K account, 5.0 lots on XAUUSD = $500/point movement. With 4 recovery positions = 20 lots = $2,000/point. That's nuclear.
**Better: 0.10** for accounts under $10K.

#### `InpFixedLots = 0.0`
**What it does:** When set to 0.0, the EA uses the proportional formula (LotsPer1000). If you set it to, say, 0.02, the EA ALWAYS uses 0.02 lots regardless of balance.
**Analogy:** "0.0 = automatic gear" vs "0.02 = manual — always use 2nd gear."
**Your current: 0.0** → ✅ Good. Automatic scaling.

---

### 🧺 BASKET RISK / TP / BREAK-EVEN

> **What is a "Basket"?**
> A basket = the INITIAL trade + all its recovery trades grouped together. If you enter 1 sell, then add 2 recovery sells, all 3 together = 1 basket.

#### `InpMaxDDUSD = 10.0`
**What it does:** The maximum loss (in USD) the EA will allow on a single basket before force-closing everything in that basket.
**Analogy:** You walk into a casino table and say: "If I lose $10 on this hand (including all re-bets), I'm walking away from THIS hand."
**Risk:** This is your **per-basket safety net**. Without it, a single basket could lose your entire account.
**Your current: $10** → This is for a $1,000 account (1% risk). For $500, it's 2% risk. For $300, it's 3.3%.

> [!IMPORTANT]
> **This does NOT limit your DAILY loss.** You can lose $10 on basket 1, then $10 on basket 2, then $10 on basket 3... and so on all day. That's the dangerous part.

#### `InpTakeProfitUSD = 5.0`
**What it does:** When the basket's combined profit reaches $5, close all positions in the basket = take the win.
**Analogy:** "If this hand at the casino makes me $5, I pocket it and move to the next hand."
**Risk:** Too small = you miss big moves. Too large = you hold too long and give back profit.
**Your current: $5** → OK for 0.01 lot. This means price needs to move about $5 / 0.01 lot = 500 points ($5.00) in your favor. But with multiple recovery positions it can be reached sooner.

#### `InpBreakEvenUSD = 2.0`
**What it does:** When the basket's unrealized profit reaches $2, the EA moves the stop-loss to BREAKEVEN (entry price + tiny buffer). This locks in a $0.11 minimum profit.
**Analogy:** You're playing a card game. Once you're $2 up, you lock in your original bet so you CAN'T lose it. You might miss a bigger win, but you also can't lose.
**This is why 31% of your trades show exactly $0.11 profit** — they hit breakeven and got stopped out.
**Your current: $2.0** → ✅ Good. This is the main capital preservation tool.

#### `InpTrailActivateUSD = 3.0`
**What it does:** When basket profit reaches $3, a trailing stop activates. Instead of just sitting at breakeven, the stop-loss now FOLLOWS the price upward.
**Analogy:** You're climbing a mountain with a safety rope. Once you reach 3 meters up, the rope starts following you — if you slip back $1, the rope catches you at $2+ profit instead of $0.
**Your current: $3.0** → ✅ Good. Trails after $3 profit.

#### `InpTrailPullbackUSD = 1.0`
**What it does:** How much the trailing stop allows price to pull back before closing. With $1.0, if your basket reached $4 profit, it closes if profit drops to $3.
**Analogy:** The length of your safety rope — shorter = tighter protection but more frequent stops. Longer = more room to breathe but you give back more profit.
**Your current: $1.0** → ✅ Good. Allows $1 pullback.

#### `InpUseSafetySL = true`
**What it does:** Places an emergency hard stop-loss on every trade as backup protection.
**Analogy:** The airbag in your car. The seatbelt (MaxDDUSD) is your primary protection, but the airbag (SafetySL) is there if everything else fails.
**Your current: true** → ✅ **NEVER set this to false.** This is your last line of defense.

---

### 🔄 RECOVERY PENDINGS

> **Recovery = the EA's strategy for rescuing a losing trade by adding more positions at better prices and hoping for a reversal.**

#### `InpMaxRecovery = 3`
**What it does:** Maximum number of additional recovery trades the EA can open after the first trade goes wrong.
- 0 = no recovery (pure single-trade scalper)
- 1 = one rescue attempt
- 2 = two rescue attempts
- **3 = three rescue attempts (current)**

Total positions per basket = 1 (original) + 3 (recovery) = **4 maximum**

**Analogy:** You buy a stock at ₹100. It drops.
- Recovery 1: Buy more at ₹96.50 (average down)
- Recovery 2: Buy more at ₹93.00 (average down more)
- Recovery 3: Buy more at ₹89.50 (last chance)
- If it drops further → all 4 positions hit the $10 MaxDD and close at a loss

**Risk:** More recovery = better chance of surviving small dips, BUT also means more positions open = more exposure = bigger loss if price KEEPS going against you.
**Your current: 3** → ⚠️ Moderate risk. The forensic found that R3 (third recovery) was NEVER triggered in 9 days — so it's untested.

#### `InpRecoveryTriggerUSD = 3.0`
**What it does:** How much money (in USD) the basket must lose before a recovery trade is placed.
**Analogy:** "I won't call for backup until I've lost $3 on this basket."
**Your current: $3.0** → ✅ Reasonable. At 0.01 lot, $3 = 300 points of adverse movement.

#### `InpRecoveryOffsetPoints = 35`
**What it does:** How far (in points) from the current price the recovery pending order is placed. 35 points = $0.35 on XAUUSD.
**Analogy:** If price is at 4350 and you're in a losing buy, the recovery buy order is placed at 4350 - 35 = 4349.65 (hoping to buy even lower).
**Your current: 35** → This creates a mini-grid with 35-point spacing.

---

### 🛑 OPTIONAL EXTRA STOPS

#### `InpStopLossPips = 0.0`
**What it does:** An additional per-trade stop-loss in pips. 0.0 = disabled (relying on basket MaxDD instead).
**Your current: 0.0** → The EA uses basket-level risk management instead of per-trade SL.

#### `InpTakeProfitPips = 0.0`
**What it does:** An additional per-trade take-profit in pips. 0.0 = disabled.
**Your current: 0.0** → The EA uses basket-level TP instead.

---

### 🎯 FIRST ENTRY SIGNAL

#### `InpLimitPoints = 35`
**What it does:** The EA places LIMIT orders 35 points away from the current price.
- For a BUY: limit order 35 points BELOW current price
- For a SELL: limit order 35 points ABOVE current price

**Analogy:** Instead of buying right now, you say: "I'll only buy if the price drops a bit first." Like placing a bid in an auction that's slightly below the current ask.
**This is a mean-reversion entry** — it assumes price will dip and then bounce back.
**Your current: 35** → This is the core entry parameter.

#### `InpRangeFilter = 70`
**What it does:** If the recent price range exceeds 70 points, the EA STOPS placing new entries. This filters out highly volatile moments.
**Analogy:** "If the market is swinging more than $0.70 up and down, I'll sit this one out — it's too wild."
**Your current: 70** → ⚠️ 70 points might be too low for XAUUSD in 2026 (gold often moves $1-2 in minutes).

---

### 📶 SPREAD / SESSION FILTER

#### `InpMaxSpreadPips = 12.0`
**What it does:** If the broker's spread exceeds 12 pips/points, the EA will NOT open new trades.
**Analogy:** "If the shop's markup is more than ₹12 above wholesale, I'm not buying."
**Your current: 12** → ⚠️ The author says max 7, but the .set allows 12. Tighter is safer.

#### `InpUseSessionFilter = true`
**What it does:** Only trade during specified hours.
**Your current: true** → ✅ Good. Prevents trading during dead/dangerous hours.

#### `InpStartHour = 1` / `InpStartMinute = 0`
**Start trading at:** 01:00 server time

#### `InpEndHour = 20` / `InpEndMinute = 59`
**Stop trading at:** 20:59 server time

**Analogy:** "The shop is open from 1 AM to 9 PM. No business outside these hours."
**Your current: 1:00–20:59** → ⚠️ This is 20 hours per day — almost always-on. The losing hours in the forensic analysis (5, 6, 7, 17) are all within this window.

---

### 📊 MA FILTER (TREND PROTECTION)

#### `InpEnableMAFilter = false`

> [!CAUTION]
> **THIS IS THE MOST IMPORTANT SETTING THAT IS CURRENTLY WRONG.**

**What it does:** When enabled, the EA checks a Moving Average (trend indicator) before entering:
- If price is ABOVE the MA → only BUY (trade with the trend)
- If price is BELOW the MA → only SELL (trade with the trend)
- This PREVENTS the EA from trading AGAINST the trend

**Analogy:** Imagine you're swimming in a river.
- **MA filter OFF (current):** You swim in any direction — sometimes against the current. You can make it, but if the current is strong, you drown.
- **MA filter ON:** You ONLY swim WITH the current. You miss some opportunities, but you never fight a strong current.

**Your current: false** → ❌ **This is why the base entry has NEGATIVE edge.** The EA is placing sell orders during uptrends and buy orders during downtrends. The recovery system has to rescue these bad entries.

**With MA filter ON:** The EA will:
- Stop selling during bullish moves → fewer recovery-needed baskets
- Stop buying during bearish moves → fewer failed baskets
- Result: Fewer total trades, but HIGHER quality trades

#### `InpMAPeriod = 50`
**What it does:** The number of candles used to calculate the moving average.
- 50 = looks at the last 50 candles
- Shorter (20) = more responsive, more signals, more false signals
- Longer (100) = smoother, fewer signals, misses quick reversals

**Analogy:** 50-period = checking the weather forecast for the last 50 hours. It tells you the general trend, not minute-by-minute changes.
**Your current: 50** → ✅ Good starting point.

#### `InpMATimeframe = 15`
**What it does:** Which chart timeframe to check the MA on. 15 = M15 (15-minute chart).
**Analogy:** The EA trades on M1 (1-minute), but checks the "bigger picture" on the 15-minute chart. Like a day-trader who checks the weekly chart for the overall trend before making intra-day trades.
**Your current: 15** → ✅ Good. M15 provides a reasonable trend view for an M1 scalper.

#### `InpMAMethod = 0`
**What it does:** MA calculation method: 0 = Simple Moving Average (SMA).
Options: 0=SMA, 1=EMA (Exponential), 2=SMMA, 3=LWMA
**Your current: 0 (SMA)** → OK. EMA (1) would be more responsive.

#### `InpMAPrice = 1`
**What it does:** Which price to use for MA calculation: 1 = High price.
Options: 0=Close, 1=High, 2=Low, 3=Open, etc.
**Your current: 1 (High)** → Unusual. Close (0) is more standard.

---

### 📅 DAILY LIMITS

#### `InpMaxDailyTrades = 10000`
**What it does:** Maximum trades per day.
**Your current: 10,000** → ❌ **DANGEROUSLY HIGH.** This is effectively NO limit. On Oct 1, the EA did 88 trades and lost $70. With no limit, it could do 500+ trades in a bad day.
**Analogy:** Telling yourself "I'll stop gambling after 10,000 hands" is the same as saying "I'll never stop."

#### `InpDailyProfitLimit = 150000.0`
**What it does:** EA stops trading when daily profit reaches this amount. Also applies as a loss limit in some implementations.
**Your current: $150,000** → ❌ **DANGEROUSLY HIGH.** This is effectively NO limit. Your account has $624. A $150,000 limit is meaningless.
**Analogy:** Setting your credit card limit to ₹1.5 crore when your salary is ₹50,000/month.

---

### 🖥️ UI

#### `InpShowComment = true`
**What it does:** Shows EA status information on the chart.
**Your current: true** → ✅ Good for monitoring.

---

---

## PART 2: OPTIMIZED .set FOR ALL MARKET CYCLES

Based on the forensic analysis, here is a configuration designed to:
1. ✅ **ADD TREND PROTECTION** (MA filter ON)
2. ✅ **ADD DAILY LOSS LIMIT** (prevent catastrophic days)
3. ✅ **TIGHTER SESSION WINDOW** (avoid losing hours)
4. ✅ **TIGHTER SPREAD LIMIT** (match author's recommendation)
5. ✅ **SAFER LOT LIMITS** (prevent oversizing)

### Changes Summary

| Parameter | OLD | NEW | Why |
|-----------|-----|-----|-----|
| `InpMaxLots` | 5.0 | **0.10** | Prevents oversized positions on larger accounts |
| `InpMaxDDUSD` | 10.0 | **10.0** | Keep — this is per basket and works well |
| **`InpEnableMAFilter`** | **false** | **true** | **THE #1 CHANGE — adds trend protection** |
| `InpMAMethod` | 0 (SMA) | **1 (EMA)** | EMA responds faster to trend changes |
| `InpMAPrice` | 1 (High) | **0 (Close)** | Close price is more standard/reliable |
| `InpMaxSpreadPips` | 12.0 | **8.0** | Tighter — closer to author's "max 7" |
| `InpStartHour` | 1 | **2** | Skip hour 1 (low liquidity) |
| `InpEndHour` | 20 | **19** | Stop before hour 20 (avoid late session risks) |
| **`InpMaxDailyTrades`** | **10000** | **80** | **Critical safety limit** |
| **`InpDailyProfitLimit`** | **150000** | **40.0** | **Daily loss/profit cap** |

---

### The Optimized .set File

Copy this exactly and save as your `.set` file:

```ini
InpSecTrade=GOLD SCALPER
InpMagic=10001
InpComment=GOLD SCALPER
InpSlippage=3
InpSecLots==== MONEY MANAGEMENT (per $1000) ===
InpLotsPer1000=0.01
InpMinLots=0.01
InpMaxLots=0.10
InpFixedLots=0.0
InpSecBasket==== BASKET RISK / TP / BE (USD) ===
InpMaxDDUSD=10.0
InpTakeProfitUSD=5.0
InpBreakEvenUSD=2.0
InpTrailActivateUSD=3.0
InpTrailPullbackUSD=1.0
InpUseSafetySL=true
InpSecRecover==== RECOVERY PENDINGS ===
InpMaxRecovery=2
InpRecoveryTriggerUSD=3.0
InpRecoveryOffsetPoints=35
InpSecStops==== OPTIONAL EXTRA STOPS ===
InpStopLossPips=0.0
InpTakeProfitPips=0.0
InpSecEntry==== FIRST ENTRY SIGNAL ===
InpLimitPoints=35
InpRangeFilter=70
InpSecSpread==== SPREAD / SESSION ===
InpMaxSpreadPips=8.0
InpUseSessionFilter=true
InpStartHour=2
InpStartMinute=0
InpEndHour=19
InpEndMinute=30
InpSecMA==== MA FILTER ===
InpEnableMAFilter=true
InpMAPeriod=50
InpMATimeframe=15
InpMAMethod=1
InpMAPrice=0
InpSecDaily==== DAILY LIMITS ===
InpMaxDailyTrades=80
InpDailyProfitLimit=40.0
InpSecUI==== UI ===
InpShowComment=true
```

---

### What Each Change Does For You

#### 🛡️ MA Filter ON = Trend Protection
```
BEFORE (filter OFF):
  Gold trending UP → EA places SELL → price keeps rising → LOSS
  Gold trending DOWN → EA places BUY → price keeps falling → LOSS
  Recovery tries to save → sometimes works, sometimes fails → FRAGILE

AFTER (filter ON):
  Gold trending UP → EA only places BUY → trading WITH the trend → BETTER ODDS
  Gold trending DOWN → EA only places SELL → trading WITH the trend → BETTER ODDS
  Fewer recovery-needed baskets → MORE STABLE
```

**Expected impact:**
- Fewer total trades (maybe 30-40% fewer)
- HIGHER win rate (removing counter-trend losers)
- FEWER recovery baskets needed
- LOWER drawdowns
- Net profit per trade should INCREASE even if total trades decrease

#### 🛡️ MaxRecovery 3 → 2
```
BEFORE: Up to 4 simultaneous positions (1 + 3 recovery)
AFTER:  Up to 3 simultaneous positions (1 + 2 recovery)

Why: R3 was never triggered anyway, and with MA filter ON,
     fewer baskets should need recovery at all.
     Removing R3 reduces maximum exposure by 25%.
```

#### 🛡️ Daily Limits = Parachute
```
BEFORE: EA can trade 10,000 times and lose $150,000 in a day
         → On a $500 account, the EA would blow the account

AFTER:  EA stops after 80 trades OR $40 daily profit/loss
         → Maximum daily damage is capped
         → On a $500 account: worst day = 8% loss (survivable)
```

#### 🛡️ Tighter Session = Avoid Bad Hours
```
BEFORE: 01:00 to 20:59 (20 hours)
AFTER:  02:00 to 19:30 (17.5 hours)

Hours removed:
  - 01:00-02:00 → low liquidity, wide spreads
  - 19:30-20:59 → end-of-session risks

Hours 5-7 are still included but the MA filter
should now prevent the worst counter-trend trades
during these hours.
```

---

### How This Configuration Handles Each Market Cycle

| Market Condition | OLD Config | NEW Config |
|-----------------|------------|------------|
| **Strong uptrend** | Sells into uptrend → repeated losses | MA filter blocks sells → only buys → profit |
| **Strong downtrend** | Buys into downtrend → repeated losses | MA filter blocks buys → only sells → profit |
| **Ranging/choppy** | Works well (mean-reversion) | Works well + fewer false signals |
| **Spike + reversal** | Takes both sides → mixed results | Takes trend side → cleaner entries |
| **News spike** | No protection → big losses | Daily limit caps damage + spread filter blocks entries |
| **Low volatility** | Many small wins | Same behavior, fewer trades |
| **High volatility** | Range filter blocks some, but still risky | Range filter + MA filter = double protection |

---

### ⚠️ CRITICAL: What You MUST Do Manually

The .set file cannot protect against everything. You must:

1. **DISABLE the EA 30 minutes before major news:**
   - NFP (first Friday of month)
   - FOMC rate decisions
   - CPI releases
   - Fed Chair speeches
   - Major geopolitical events

2. **Monitor weekly:**
   - If PF drops below 1.10 over 500 trades → reduce lot or pause
   - If daily loss hits $40 → EA stops automatically, but check WHY
   - If 5+ consecutive basket losses → pause and evaluate

3. **Keep on DEMO for 30+ more days** before going live

4. **When going live, start with $500-$1000, 0.01 lots ONLY**

---

### Expected Difference (Estimated)

| Metric | OLD Config (observed) | NEW Config (estimated) |
|--------|----------------------|----------------------|
| Trades/day | ~230 | ~140-170 (fewer, but higher quality) |
| Win rate | 74% | **76-80%** (counter-trend losers removed) |
| Profit factor | 1.18 | **1.25-1.40** (if MA filter adds edge) |
| Base trade PF | 0.896 (LOSING) | **1.05-1.15** (should become positive) |
| Recovery dependency | 100% of profit | **50-70%** (less dependent) |
| Worst day | -$70.13 | **-$40 max** (daily limit) |
| Expectancy/trade | $0.12 | **$0.15-0.25** (higher quality trades) |

> [!NOTE]
> These estimates are PROJECTED based on the forensic analysis. They are NOT guaranteed.
> The MA filter should improve results, but HOW MUCH can only be determined by testing.
> Run this configuration on demo for 30+ days and compare with the old results.

---

### 🎯 Quick Reference Card

```
PROTECTION LAYERS IN THE NEW CONFIG:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Layer 1: MA Filter      → Don't trade against the trend
Layer 2: Range Filter   → Don't trade in wild markets
Layer 3: Spread Filter  → Don't trade with expensive spreads
Layer 4: Session Filter → Don't trade in dangerous hours
Layer 5: Break-Even SL  → Lock in profit early
Layer 6: Trailing Stop  → Let winners run (a little)
Layer 7: Basket MaxDD   → Cap loss per basket at $10
Layer 8: Daily Limit    → Cap total daily loss at $40
Layer 9: Safety SL      → Emergency hard stop (airbag)
Layer 10: MaxRecovery=2 → Limit recovery to 2 levels
```

**Think of it like driving a car:**
- MA Filter = GPS (keeps you on the right road)
- Range Filter = speed limit sign (slow down in danger zones)
- Spread Filter = fuel price check (don't buy expensive fuel)
- Session Filter = driving hours (no night driving)
- Break-Even = seatbelt (locks you in)
- Trailing = cruise control (maintains speed)
- Basket MaxDD = brakes (stops you at each intersection)
- Daily Limit = fuel tank limit (you can only drive so far per day)
- Safety SL = airbag (emergency only)
