# 5. HTML Tables
Tables are for **tabular data**, never for layouts.

| Element | Requirement |
| :--- | :--- |
| `<thead>` | Must be used for the header row. |
| `<th>` | Used within `<thead>` for column labels; include `scope="col"`. |
| `<caption>` | Provide a summary/title for screen readers. |
| **Responsive** | Wrap in a container with `overflow-x: auto`. |

## Data Table
```html
<div class="table-container" style="overflow-x: auto;">
  <table>
    <caption>Quarterly Sales Growth 2026</caption>
    <thead>
      <tr>
        <th scope="col">Region</th>
        <th scope="col">Revenue</th>
        <th scope="col">Growth (%)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">North America</th>
        <td>$450,000</td>
        <td>+12%</td>
      </tr>
      <tr>
        <th scope="row">Europe</th>
        <td>$320,000</td>
        <td>+8%</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <th scope="row">Total</th>
        <td colspan="2">$770,000</td>
      </tr>
    </tfoot>
  </table>
</div>