# Walkthrough - Delivery Subscription Service Confidence Interval Simulator

This document provides a summary of the completed features, layout design, mathematical models, and verification details of the educational simulator.

## Features Implemented

1. **Elegant & Premium Pink Dashboard Design**:
   - Stylized in a "Cherry Blossom & Rose Gold" theme with modern glassmorphism panels.
   - Applied `'GanaChocolate'` for primary headings, logo, and action buttons.
   - Applied `'Presentation'` (Freesentation) for core body texts, labels, and statistical indices.
   - Clean layout: Left sidebar for input configurations, right panel for outcomes and graphs.

2. **Full Statistical Simulation System**:
   - Pre-modeled 6 administrative regions (Metropolitan, Gyeongsang, Chungcheong, Jeolla, Gangwon, Jeju) with weighted population proportions and custom normal distribution parameters ($\mu_r$, $\sigma_r$).
   - Dynamically calculates the combined true population mean ($\mu_{true}$) and standard deviation ($\sigma_{true}$) according to the user's checked regions:
     $$\mu_{true} = \sum p_r \mu_r, \quad \sigma_{true} = \sqrt{\sum p_r (\sigma_r^2 + \mu_r^2) - \mu_{true}^2}$$
   - Generates random monthly spending samples using the **Box-Muller Transform** according to normalized region weights.
   - Computes:
     - Sample mean ($\bar{x}$)
     - Sample standard deviation ($s$) using the unbiased $n-1$ denominator
     - Standard error of the mean ($SE = s / \sqrt{n}$)
     - $95\%$ Confidence Interval: $\bar{x} \pm 1.96 \times SE$
     - $99\%$ Confidence Interval: $\bar{x} \pm 2.576 \times SE$
     - Success/failure flags indicating if the true population mean is contained inside the bounds.

3. **Multiple Chart.js Visualizations**:
   - **Chart 1 (Distribution Comparison)**: A dual-axis chart showing the sample relative frequency (bars) overlaid with the true population mixture probability density function (line).
   - **Chart 2 (CI History)**: Floating horizontal error bars mapping the latest 30 draws. Bars turn green when successful and red when the true mean is missed. A vertical dashed line traces the true mean.
   - **Chart 3 (Regional Comparison)**: Column bar chart comparing individual region means against the global mean and the current sample mean line.

4. **Secret Easter Egg: Cherry Blossom Gacha & Fortune Machine**:
   - Triggered by clicking the top header or the floating footer blossom emoji (🌸) 7 times.
   - Starts a lovely CSS-based cherry blossom falling snow animation across the browser viewport.
   - Displays a retro pink Gacha machine modal where students can pull a lever to draw random samples and receive statistical fortunes (e.g., "Cosmic Legendary sample with $< 0.15\%$ error!").

---

## Verification & Testing

- **Font rendering**: Verified both `'GanaChocolate'` and `'Presentation'` web fonts render correctly.
- **Statistical precision**: Checked sample variance and CI coverage rates. When running auto-sampling, the coverage rates converge closely to $95.0\%$ and $99.0\%$, verifying the mathematical model.
- **Responsiveness**: Tested on mobile and desktop viewports; the sidebar collapses cleanly into a stacked view.
