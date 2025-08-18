# GraphPad Prism guide

## Barplot of Top n UP and DOWN DEGs

-   New Data Table.

    -   Select "Column" data table.\
    -   For graph options: Enter and plot error values already calculated elsewhere \> Mean + SEM.

-   Enter Your Data: Use one column for all 20 genes (10 up, 10 down).

-   Paste your log2FC values into Y values.

-   Gene names go in the row titles on the left (these will show up on the X-axis).

    Tip: Sort your genes by log2FC before you paste (top 10 positive first, then 10 negative).

-   Graph Customization: Go to the graph created. On the X-axis tab, check "Categorical labels" if not already set.

    Make sure gene names show as labels (you can manually tweak font/angle if they're long).

    In `Format Axes`, you can center zero on Y-axis for visual symmetry (looks great for volcano-style bars).

## Repeated Measures Analysis (2 factors)

-   Replicate data from samples in an experimental setup including the two factors (reflecting treatment or condition and Time).

-   The two-way ANOVA accounts for within-subject variability and matching across factors. This requirement ensures accurate computation of effects, such as treatment differences, patterns in those differences, and trends over Time.

Step-by-step instructions for entering data and performing the analysis; Ensure your data are organized in a long format or spreadsheet beforehand for easy transfer.

### Create a new data table

Select `Grouped` as the table format (suitable for two-way designs). - Data table: Select `Enter or import data into a new table`.\
- Options: Enter <n> replicate values in side-by-side subcolumns. Here, <n> is the number of samples.\
- Click `Create` to generate a table with rows for one factor and columns for the other, with subcolumns for replicates.

### Label and enter data

-   Label rows: Click the row titles (leftmost column) and enter levels of the Time factor.\
-   Label column groups: Click column titles (top row that shows as Group A, Group B,...) and enter levels of the experimental condition factor.\
-   Label subcolumns: Click each subcolumn heading (A:Y1, A:Y2,...) within a group and rename them (e.g., "Sample 1", "Sample 2", "Sample 3", "Sample 4"). Repeat this for all column groups to ensure consistency, as the samples are the same across weeks.\
-   Enter the data based on the way your row and column factor are arranged.\
-   Leave a cell blank if there are missing values. Repeated measures ANOVA can be performed by fitting a mixed-effects model.

### Perform the 2-way repeated measures ANOVA

-   While on the worksheet/table under the `Data Table` on left panel, click `Analyze > Group Comparison > Two-Way ANOVA (or Mixed Model)`.\
-   In the `Analyze Data` dialog box, select the `Two-Way ANOVA (or Mixed Model)`; on right, data columns for the worksheet should have been selected by default. Click OK.\
-   In the `Parameters: Two-Way ANOVA (or Mixed Model)` dialog box, multiple tabs (Model, Repeated Measures, Factor Names,..) will need to be completed.
    -   Tab `Model`

        -   If the Time factor levels are in the rows, click on `Each row represents a different time point, so matched values are stacked into a subcolumn`. Or the `...column...` option.\
        -   Include interaction term.\
        -   Use `Geisser-Greenhouse correction. Recommended`. The Geisser-Greenhouse correction is used to adjust the results of a repeated measures ANOVA when the assumption of sphericity is violated. Sphericity assumes that the variances of the differences between all combinations of within-subject conditions are equal. If this assumption is not satisfied, the standard repeated measures ANOVA can lead to inflated Type I error rates (false positives). The Geisser-Greenhouse correction adjusts the degrees of freedom in the F-test, making the test more conservative and reducing the likelihood of a Type I error.

    -   Tab `Repeated Measures`

        -   Select `Mixed-effects model` if missing values are present, or `Repeated measures ANOVA (based on GLM)` if no missing values.
        -   Use recommended settings `Remove term(s) from model and fit a simpleer model` if random effect is zero; and check `Use initial values based on GLM`.

    -   Tab `Factor Names`: Name the factors

    -   Tab `Multiple Comparisons`: Choose `Compare cell means with others in its row and its column`.

    -   Tab `Options`: Choose the recommended multiple hypothesis test and other options as per your requirement.

    -   Tab `Residuals`: Choose all residual plots.

NOW, click **OK**!!!

If you need to change any paramter or option, click on `Analyze > Two -Way ANOVA (or Mixed Model)` again, and the dialog box will open with instructions. Click on `Change parameters of the existing analysis,...`. Your previous analysis options box will appear. You can change any parameter and click OK to re-run the analysis.

### Interpret results

-   Check model assumptions \

-   Review the ANOVA table: Interaction effect first; depending on the significance of interaction, interpret main effects or posthoc comparisons within levels of factors. \

-   Examine multiple comparisons (if selected).

-   Customize graphs
