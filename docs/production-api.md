# Production API Field Reference

The production module data transfer objects and stub endpoints now mirror the database schemas for the `work_order`, `lot`, `bom`, and `defect` tables.

## Work Order (`/api/production/work_order`)

| Field | Type | Description |
| ----- | ---- | ----------- |
| `work_order_id` | `Long` | Unique identifier for the work order. |
| `stock_id` | `Long` | Finished goods or assembly stock identifier. |
| `emp_id` | `Long` | Employee assigned to the work order. |
| `start_date` | `LocalDate` | Planned production start date. |
| `due_date` | `LocalDate` | Planned completion date. |
| `target_qty` | `Integer` | Target production quantity. |
| `good_qty` | `Integer` | Produced good quantity. |
| `defect_qty` | `Integer` | Produced defective quantity. |
| `order_status` | `String` | Status flag (`대기`, `진행중`, `완료`, ...). |
| `request_date` | `LocalDateTime` | Timestamp when the work order was requested. |

### Request parameters
- `order_status` (optional)
- `stock_id` (optional)
- `start_date`, `due_date` (optional, ISO `yyyy-MM-dd` strings)

### Create Work Order (`POST /api/production/work_order/add`)
Required form fields: `stock_id`, `emp_id`, `start_date`, `due_date`, `target_qty`, `request_date` (`datetime-local`, serialized as ISO date-time).

## Lot (`/api/production/work_order/{work_order_id}/lots`)

| Field | Type | Description |
| ----- | ---- | ----------- |
| `lot_id` | `Long` | Identifier for the produced lot. |
| `work_order_id` | `Long` | Parent work order identifier. |
| `stock_id` | `Long` | Stock identifier produced by the lot. |
| `lot_qty` | `Integer` | Quantity produced in the lot. |
| `lot_date` | `LocalDate` | Date the lot was produced. |

## BOM (`/api/production/bom`)

| Field | Type | Description |
| ----- | ---- | ----------- |
| `parent_stock_id` | `Long` | Finished product stock identifier. |
| `child_stock_id` | `Long` | Component stock identifier. |
| `required_qty` | `Integer` | Quantity of the component required. |

Optional request parameter: `parent_stock_id`.

## Defect (`/api/production/work_order/{work_order_id}/defects`)

| Field | Type | Description |
| ----- | ---- | ----------- |
| `defect_code` | `Long` | Defect classification code. |
| `work_order_id` | `Long` | Parent work order identifier. |
| `lot_id` | `Long` | Related lot identifier. |
| `stock_id` | `Long` | Stock identifier associated with the defect. |
| `defect_qty` | `Integer` | Quantity of defective units. |
| `defect_date` | `LocalDate` | Date the defect was registered. |

These field names are also surfaced in the front-end table renderers and detail popups under `static/js/production.js` and related popup forms.
