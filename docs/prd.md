# PerpPilot-AI PRD

### 1. Product Overview

#### 1.1 Product Name

**PerpPilot-AI** - Perpetual Futures Leverage Ratio AI Analysis Assistant

#### 1.2 Product Positioning

An AI-driven, risk-first trading strategy planning tool designed to help novice crypto futures traders use leverage rationally and understand the inherent risks of their trading plans.

#### 1.3 Target Users

- Individuals who have a basic understanding of a specific cryptocurrency and have formed their own price judgment (e.g., key support/resistance levels).
- Users who know the principal (margin) they plan to invest and the maximum loss percentage they are willing to accept.
- Those who have a basic concept of futures leverage but are unsure how to select the most appropriate leverage ratio based on their trading strategy and risk appetite.
- Traders who are often trapped in the "high-leverage fallacy," prioritizing how much leverage they can use rather than focusing on risk control first.

### 2. Problem Statement

In the current futures trading market, novice investors face a classic and dangerous "trilemma." They often want to achieve three conflicting goals simultaneously:

1.  **Ideal Stop-Loss Placement**: Setting a stop-loss at the most logical technical level based on their analysis.
2.  **Fixed Loss Limit**: Strictly controlling the loss on a single trade to an acceptable amount.
3.  **High Capital Efficiency**: Desiring to use high leverage to amplify potential returns.

However, these three objectives are mathematically irreconcilable. Novices, failing to understand this intrinsic relationship, often mistakenly prioritize **high leverage**. This leads to calculated stop-loss levels that are meaningless or, worse, premature liquidation due to insufficient margin long before their ideal stop-loss price is hit.

**Core Pain Point:** The market lacks a tool that helps users **"think in reverse."** Users need a tool that can, based on their **"market judgment"** and **"risk commitment,"** reverse-calculate the **"most rational leverage ratio."** Furthermore, it should validate the **survivability** of this leverage under **extreme market conditions**, thereby shifting their mindset from **"leverage-first"** to **"risk-first."**

**Secondary Pain Points:**

- To validate the logic of this reverse-thinking algorithm, this tool will position Gemini as a neutral, objective trading analysis assistant. It will be required to provide a brief, educational, and qualitative analysis of the user's trading plan based on the given data, from perspectives like **Risk/Reward Ratio, Stop-Loss Distance, and Leverage Level.** **The model will be explicitly instructed not to provide direct investment advice.**
- For advanced users: They can use their own API Key to get a more stable and uninterrupted AI analysis experience.

### 3. Product Goals & Value

- **Core Goal**: To help users calculate the **optimal leverage ratio** that satisfies their custom stop-loss strategy and risk tolerance, and to evaluate the **health** of that strategy.
- **User Value**:
  - **Risk Education**: Intuitively educate users on the relationship between leverage, stop-loss, position size, and risk through visualized calculation results.
  - **Strategy Validation**: Allow users to verify the viability of their trading plan _before_ opening a position, avoiding unnecessary losses caused by incorrect parameter settings.
  - **Confidence Boost**: Provide novices with a clear, rational decision-making framework, helping them execute trades with greater confidence.
- **Business Value**: As a practical utility, this tool can attract and retain risk-conscious users for exchanges, market analysis platforms, or Key Opinion Leaders (KOLs).

### 4. Features & Functionality

#### 4.1 Calculator Core Functionality

**User Inputs:**

| **Field Name**             | **Field Type** | **Description / Example**                                                                                       | **Notes**                                 |
| :------------------------- | :------------- | :-------------------------------------------------------------------------------------------------------------- | :---------------------------------------- |
| **Trade Direction**        | Radio Button   | Long / Short                                                                                                    | Required                                  |
| **Entry Price**            | Number Input   | The market price at which the user plans to enter.                                                              | Required                                  |
| **Stop-Loss Price**        | Number Input   | The stop-loss price set by the user based on analysis.                                                          | Required                                  |
| **Principal (Margin)**     | Number Input   | The margin the user plans to invest.                                                                            | Required                                  |
| **Acceptable Loss %**      | Slider / Input | The maximum percentage of the principal the user is willing to lose.                                            | Required                                  |
| **Key Resistance/Support** | Number Input   | **(Dynamic Label)** Label changes based on Trade Direction. "Key Resistance" for Short, "Key Support" for Long. | Optional, but highly recommended          |
| **Take-Profit Price**      | Number Input   | (Optional) The user's target price for taking profit.                                                           | Used to calculate R/R Ratio & Est. Profit |

**Calculation Outputs:**

| **Field Name**                  | **Description**                                                                                                  | **Priority** |
| :------------------------------ | :--------------------------------------------------------------------------------------------------------------- | :----------- |
| **🥇 Suggested Max Leverage**   | **Core Output.** The optimal leverage ratio calculated from the inputs above.                                    | Highest      |
| **📊 Strategy Health Analysis** | Risk rating based on the comparison between the "Est. Liquidation Price" and the "Key Resistance/Support Level." | Highest      |
| **Position Value (USDT)**       | `Principal × Suggested Leverage`. The user's actual position size in the market.                                 | High         |
| **Position Size (in Coins)**    | `Position Value / Entry Price`. The actual quantity of the asset held.                                           | High         |
| **Estimated Loss Amount**       | `Principal × Acceptable Loss %`. Verifies the calculation matches user's expectation.                            | High         |
| **Estimated Profit Amount**     | The potential profit in USDT based on the Take-Profit price. (Only displayed if TP is entered).                  | Medium       |
| **Est. Liquidation Price**      | The estimated price of liquidation based on the calculated leverage. Used as a risk warning.                     | Medium       |
| **Risk/Reward Ratio**           | `(Entry Price - Take-Profit Price) / (Stop-Loss Price - Entry Price)`. (Only displayed if TP is entered).        | Medium       |

#### 4.2 AI-Enhanced Functionality

- **Feature Name:** ✨ AI Strategy Analysis
- **Trigger:** A prominent button, e.g., "✨ AI, Analyze My Strategy," available in the results area.
- **Core Logic:**
  1.  When the user clicks the "AI, Analyze" button, the front-end gathers all entered parameters and calculated results.
  2.  The system first attempts to read a user-defined API Key from the browser's `localStorage`. If it exists, that key is used; otherwise, it defaults to the environment variable key (i.e., an empty string).
  3.  The front-end structures this data into a detailed, context-rich Prompt and sends it, along with the retrieved API Key, to the Gemini Pro model.
  4.  **Prompt Design Philosophy:** Instruct Gemini to act as a neutral and objective trading analysis assistant. It must use the provided data to offer a brief, easy-to-understand, and educational qualitative analysis of the user's trading plan, focusing on aspects like **R/R ratio, stop-loss distance, and leverage level.** **Explicitly instruct the model NOT to give direct investment advice.**
  5.  Receive the text-based analysis generated by Gemini.
- **🔑 API Key Configuration:**
  - **UI Element:** An "Settings" or "Key" icon button located to the right of the "AI, Analyze My Strategy" button.
  - **Interaction Flow:**
    1.  Clicking the icon button opens a simple modal dialog.
    2.  The modal contains a title (e.g., "Configure Your Gemini API Key"), an input field for the API Key, a "Save" button, and a "Close" button.
    3.  After the user pastes their key and clicks "Save."
    4.  The system stores the key in the browser's `localStorage` (e.g., under the key name `user_gemini_api_key`) for persistence.
    5.  Upon successful save, the modal closes automatically, and a brief success confirmation can be shown.
- **Interaction Behavior:**
  1.  After clicking the "AI, Analyze" button, the button's state changes to "Analyzing..." and displays a loading spinner.
  2.  A new card section appears below to display the AI's analysis, also showing a loading state.
  3.  Once the API call succeeds, the loading animations disappear, and the text returned by Gemini is displayed clearly and formatted within the AI analysis card.
  4.  If the API call fails (e.g., due to an invalid key), a user-friendly error message is shown, potentially guiding the user to check their custom API key.

#### 4.3 UI/UX

- **Responsive Layout:** The UI must be adaptive for PC, tablet, and mobile devices, ensuring good readability and usability across different screen sizes. The main content area should automatically switch from a two-column layout to a single-column stack on smaller screens.
- **Header & Navigation:**
  - **Social Links:** The top-right corner should include two clickable icon links pointing to X (Twitter: `x.com/szkane`) and GitHub (`github.com/szkane`).
  - **Language Switcher:** Provide an icon button that allows users to manually toggle the interface language between Chinese and English.
- **Disclaimer Banner:** A visually prominent yellow-colored alert box should be placed below the sub-header with the text: "Futures leverage trading involves high risk. This tool is for educational and testing purposes only."
- **Input Styling:**
  - All price-related inputs should use an "Input Group" style, with a clear "USDT" or "$" unit label on the right side of the input field to inform the user.
  - The "Acceptable Loss %" slider should have tick mark labels below it, including at least `0%`, `20%`, `40%`, `60%`, `80%`, `100%`, for a more intuitive reference.
- **Real-time Calculation:** When any input value changes, the calculation results on the right should update in real-time without requiring a "Calculate" button click.
- **Interaction Guidance:**
  - Input fields should have clear labels and placeholder text.
  - **The label for "Key Resistance/Support" should dynamically switch based on the selected "Trade Direction."**
  - **Key Level Description Enhancement:** The description text below the "Key Resistance/Support" input should be refined to: "Your ultimate line of defense. Used to evaluate survivability during extreme market events like stop-loss hunting wicks."
- **Input Logic Validation:**
  - **Purpose:** To guide users in setting logically sound "Stop-Loss Price" and "Key Level" values.
  - **Trigger:** Real-time validation occurs after both "Stop-Loss Price" and "Key Resistance/Support" have been entered.
  - **Rules:**
    - **For Short:** A warning is triggered if `Stop-Loss Price > Key Resistance Level`.
    - **For Long:** A warning is triggered if `Stop-Loss Price < Key Support Level`.
  - **Interaction Behavior:**
    - A yellow, non-blocking warning message appears below the "Key Resistance/Support" input field.
    - **Warning Text (Short):** "**Logic Suggestion:** Your Key Resistance Level ({keyLevel}) should typically be above your Stop-Loss Price ({stopLossPrice}). Please check if your strategic defense line is set correctly."
    - **Warning Text (Long):** "**Logic Suggestion:** Your Key Support Level ({keyLevel}) should typically be below your Stop-Loss Price ({stopLossPrice}). Please check if your strategic defense line is set correctly."
    - The warning message disappears automatically when the user corrects the input to be logically consistent.
- **AI Button Group:** The "AI, Analyze My Strategy" button and the new "API Key Config" icon button should be grouped together to maintain visual association.
- **AI Results Card:** Design a distinct card style, perhaps with a gradient border or a unique background color, to differentiate it from the standard calculation results and clearly label it as "Generated by Gemini AI."
- **Strategy Health Analysis:**
  - Displayed in a highly intuitive module directly below the "Suggested Max Leverage."
  - **Safe (Green):** "✅ **Healthy Strategy**: Your estimated liquidation price is far beyond your key resistance/support level. At this leverage, your strategy has sufficient breathing room."
  - **Warning (Orange):** "⚠️ **Caution Advised**: Your estimated liquidation price is approaching your key resistance/support level. A sharp market wick could potentially invalidate this strategy."
  - **Dangerous (Red):** "❌ **Dangerous Strategy**: Your estimated liquidation price is _worse_ than your key resistance/support level! This means your position will be liquidated _before_ the market truly tests your analysis. **It is strongly recommended to lower your leverage or add more margin!**"

#### 4.4 Internationalization (i18n)

- **Supported Languages:** The product must support Simplified Chinese (zh-CN) and English (en).
- **Auto-Detection:** On initial load, the system should detect the user's browser language (`navigator.language`) and display the matching language by default. If the browser language is not Chinese, it should default to English.
- **Manual Toggling:** Users can cycle through supported languages by clicking the language switcher icon in the header. This choice should be saved (e.g., using `localStorage`) to persist for the user's next visit.
- **API Key:** All text elements within the configuration modal (title, buttons, etc.) also need to be translated into both Chinese and English.
- The Prompt sent to Gemini should also be switched based on the current UI language to ensure the returned analysis matches the user interface language.
