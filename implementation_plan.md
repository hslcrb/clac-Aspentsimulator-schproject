# Implementation Plan - Delivery Subscription Service Monthly Spending Confidence Interval Simulator

This plan outlines the design and implementation details for generating a single-file, interactive HTML educational simulator for high school students. The simulator illustrates the confidence interval estimation of a population mean, with a pink-themed premium design and multiple charts.

## User Review Required

> [!NOTE]
> The design will use a modern, interactive dashboard layout styled with a premium pink/cherry blossom palette. 
> To make it highly engaging and visual, we will use Chart.js to render three interactive charts, violating the "no external libraries" constraint *only* for the Chart.js CDN, as requested by the user ("그래프도 넣거라 chartjs. 여러개.").

> [!IMPORTANT]
> The typography base is `'Presentation'` (Freesentation) and the point font is `'GanaChocolate'`. Both will be declared using the exact `@font-face` links.

## Open Questions
No immediate open questions. The user's requirements (pink theme, multiple Chart.js charts, GanaChocolate point font, Presentation base, easter egg) are clear.

---

## Proposed Changes

### Simulator Frontend

#### [NEW] [배달 구독 서비스 출시에 따른 잠재고객 1인당 월 평균 예상 소비액 추정.html](file:///c:/Users/user/clac/배달%20구독%20서비스%20출시에%20따른%20잠재고객%201인당%20월%20평균%20예상%20소비액%20추정.html)

This will be the single self-contained HTML file containing:
1. **Inline CSS Styles**:
   - Custom fonts (`Presentation` and `GanaChocolate`).
   - A pink glassmorphic theme palette (soft cherry blossoms, rose gold accents, dark slate-rose text colors, smooth transitions, pulsing animations).
   - Layout: Sticky top bar with titles, dual-column dashboard. Left sidebar for controls (sample size, regions), right panel for results and charts.
2. **Interactive Controls**:
   - Sample Size slider and input ($10 \le n \le 1000$).
   - Region checkboxes with checkboxes for 6 regions (수도권, 충청권, 경상권, 전라권, 강원권, 제주권) and a "전체 선택" (Select All) toggle.
   - Button "표본 추출" (Draw Sample) + "자동 추출" (Auto-sampling toggle with adjustable speed).
3. **Statistical Logic**:
   - Regional population parameters:
     - **수도권**: $\mu = 45,000$원, $\sigma = 12,000$원, weight = $50.1\%$ (26M)
     - **경상권**: $\mu = 41,000$원, $\sigma = 11,000$원, weight = $24.7\%$ (12.8M)
     - **충청권**: $\mu = 38,000$원, $\sigma = 9,500$원, weight = $10.8\%$ (5.6M)
     - **전라권**: $\mu = 37,000$원, $\sigma = 9,000$원, weight = $9.7\%$ (5M)
     - **강원권**: $\mu = 34,000$원, $\sigma = 8,000$원, weight = $2.9\%$ (1.5M)
     - **제주권**: $\mu = 43,000$원, $\sigma = 13,000$원, weight = $1.3\%$ (0.7M)
   - Dynamic Population Mean ($\mu_{true}$) calculation:
     $$\mu_{true} = \frac{\sum_{r \in \text{selected}} w_r \mu_r}{\sum_{r \in \text{selected}} w_r}$$
   - Sampling algorithm using Box-Muller transform:
     1. Select a region based on normalized weights of the selected regions.
     2. Generate a random spending amount using that region's $\mu_r$ and $\sigma_r$.
     3. Repeat $n$ times to form the sample array $X$.
   - Sample mean $\bar{x}$ and sample standard deviation $s = \sqrt{\frac{1}{n-1}\sum (x_i - \bar{x})^2}$.
   - Confidence Intervals calculation:
     - $95\%$ CI: $\bar{x} \pm 1.96 \times (s/\sqrt{n})$
     - $99\%$ CI: $\bar{x} \pm 2.576 \times (s/\sqrt{n})$
   - Check if $\mu_{true}$ falls inside the calculated interval bounds.
4. **Visual Charts (Chart.js)**:
   - **Chart 1: Population vs. Sample Density (Histogram)**:
     - Shows the actual distribution of the drawn sample as a pink histogram (bar chart).
     - Overlays a smooth curve representing the true population density (mixture model) for the selected regions.
   - **Chart 2: Confidence Interval History (Last 20 Runs)**:
     - Horizontal error bars showing the interval $[\text{lower}, \text{upper}]$ for each draw.
     - A central dot showing the sample mean $\bar{x}$.
     - A vertical reference line showing the true population mean $\mu_{true}$.
     - Color-coded: Green (contained) vs. Red (not contained).
   - **Chart 3: Regional Statistics Bar Chart**:
     - Comparing individual regional true means against the current sample mean.
5. **Easter Egg (벚꽃 뽑기 머신 & 벚꽃 비)**:
   - Floating pink cherry blossom petals on screen.
   - Click the title 7 times, or click a hidden "🌸" icon in the footer, to open the **"Cherry Blossom Gacha & Fortune Machine"**.
   - Lever pull animation that generates a "lucky sample" and prints a funny statistics fortune (e.g., "Legendary Draw! Your sample mean differs from the true mean by only 0.01%! You will have infinite confidence today!").

---

## Verification Plan

### Automated Tests
- Verification of Javascript sampling speed and statistical correctness via browser console testing.
- Verify standard deviation calculation matches the $n-1$ formula.
- Verify that standard error and confidence intervals match manual mathematical calculations for a given sample.

### Manual Verification
- Load the HTML file locally in Chrome/Edge/Firefox.
- Verify layout responsiveness (desktop and mobile viewports).
- Select/deselect regions and verify that the "True Mean" adjusts instantly.
- Draw samples and ensure charts update dynamically.
- Click the easter egg trigger and verify that the cherry blossom rain and gacha machine load and work flawlessly.
- Check font rendering for `'GanaChocolate'` and `'Presentation'`.
