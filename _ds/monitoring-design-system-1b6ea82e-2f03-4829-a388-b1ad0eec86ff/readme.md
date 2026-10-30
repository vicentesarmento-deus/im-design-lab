# Monitoring Design System

Design system for **Influencer Monitor**, a social-media influencer/brand monitoring product. Built from Material UI's official Figma kit ("0. MUI for Figma v7.2.0 — Material UI — Standard") with Influencer Monitor's own theme layered on top via the kit's Figma Variables — specifically the **palette mode "IM/Light"** and the **typography mode "IM/desktop"**. This is the only mode this design system implements; MUI's stock light/dark palettes and other typography modes in the source file are out of scope.

## Sources
- Figma file: "0. MUI for Figma v7.2.0 - Material UI - Standard.fig" (mounted read-only; not a shareable URL). Scope: 238 frames across component pages (Button, Checkbox, Table, Dialog, Navigation, Layout, MUI‑X Data Grid / Date-Time / Charts / Tree View, and example Screens).
- Uploaded fonts: `uploads/Rubik-*.ttf` (14 weight/style files) — copied to `assets/fonts/`.
- No product codebase or existing Influencer Monitor screens were attached. The kit's "Screens" page (Log-in, Job-directory, User-management) is MUI's own generic documentation demo — not Influencer Monitor product screens — so the UI kit in `ui_kits/influencer-monitor/` is an original composition of Influencer Monitor's information (mentions, creators, alerts, sentiment) using this design system's tokens and components, not a copy of an existing screen.

## Component coverage
The Figma kit's "Component families" inventory (see its METADATA.md) lists **237 families** in the frames the user selected. This system implements **117 components** — every everyday Material primitive, the "advanced" form/navigation families, lightweight MUI‑X‑adjacent primitives (DataGrid + toolbar buttons, TreeView/TreeItem, a full Date/Time picker set — DateField/DatePicker/DateCalendar/TimePicker/TimeClock/DateRangeCalendar), and a chart set (BarChart/LineChart/PieChart/ScatterChart + axis labels) built as plain SVG/div rather than the full MUI‑X Charts engine.

**Intentionally out of scope** (the remaining ~120 families):
- **MUI‑X Data Grid's column-header interaction layer** (drag/resize headers, column grouping headers, column menu popover, ~10 families) — GridToolbar buttons and DataGrid's own sort-look rows/selection/density/quick-filter are built; the header drag/resize/menu chrome is not.
- **MUI‑X Charts' full axis/legend/tooltip system** (scale ticks, hover tooltips, legends, ~15 families) — ChartAxisX/Y give plain tick labels; interactive hover/legend layers are not built.
- **The kit's own docs-site scaffolding**: everything under `*Library/*`, `*Custom/*`, `_Cover`, `_File Types`, `*hidden`, and the "Screens" page's Log-in/Job-directory/User-management examples — MUI.com's documentation furniture, not reusable product primitives. These describe the Figma file's own authoring conventions (component headings, placeholder covers, hidden layers) and have no UI meaning to build.

None of the above were invented or approximated beyond what's stated. Ask if a specific one (e.g. column drag/resize, or a chart legend) becomes load-bearing for a screen.

### Intentional additions
- **ProgressCircular** / **ProgressLinear** (aliased as `ProgressCircular`/`ProgressLinear`, MUI's real names) — renamed to align with the kit's "Progress | Circular" and "Progress | Linear" family names; the MUI alias ships alongside since that's the name most consumers will look for.

## Content fundamentals
No product copy, marketing site, or writing-style guide was attached — Influencer Monitor is a name only. Placeholder/sample copy in the UI kit (mention text, creator names, alert labels) is invented for demonstration and is plain, factual, and lower-case-led (sentence case, no exclamation points, no emoji) as a neutral default for a monitoring/analytics tool. Flag if there's an existing voice/tone guide to align to instead.

## Visual foundations
- **Color**: Primary is **IM Blue** `#1C00B7` (`--im-blue-700`), a deep blue-violet — this is the palette's "IM/Light" primary, replacing MUI's default blue. Secondary is **IM Malachite** `#1BDE48` (`--im-green-500`), a saturated green. Both read as monitoring/status colors (a signal-blue + a health/positive green), reinforced by dedicated semantic error/warning/info/success colors and three social-platform accent pairs (Instagram, TikTok, YouTube) for platform tagging in chips/badges.
- **Ink & action states**: text and hover/selected/focus overlays are tinted off a near-navy ink (`rgb(22,29,40)`) rather than pure black — a small but consistent IM-specific deviation from stock MUI (which uses black-based action states).
- **Type**: Rubik (uploaded, self-hosted) for body and UI/accent text; Epilogue for display/title sizes (h1–h6) — see Typography caveat below. Type scale, weights and line-heights follow the kit's "IM/desktop" typography mode exactly (e.g. h1 96px/300, body1 16px/400, button 14px/500 uppercase).
- **Spacing**: 8px base unit (`--space-1`), with a 4px half-step (`--space-0-5`) — taken verbatim from the kit's "spacing" variable collection.
- **Radius**: a flat **4px** everywhere (buttons, fields, cards, chips use their own pill/circular shapes) — the kit's "shape" collection defines a single `borderRadius: 4`, no larger "friendly" radii.
- **Elevation**: MUI's standard tiered box-shadow system (elevation 1–24, triple rgba(0,0,0,…) layering) — verified against the kit's own Card sample shadow.
- **Buttons**: line-height fixed to 1 (brand override). Horizontal padding is always double the vertical padding at each size (small 4/8, medium 8/16, large 12/24) — this is a deliberate simplification of the kit's per-variant paddings, requested explicitly for this build. A button with a leading icon gets reduced left padding; one with a trailing icon gets reduced right padding.
- **Motion**: no keyframe/easing system found in the source; feedback components use short (120–150ms) linear transitions for hover/press and a simple 1.4s ease shimmer/spin for Skeleton/ProgressCircular — standard MUI defaults, not brand-specific.
- **Surfaces & imagery**: no photography, illustration, or texture assets exist in the scoped frames — this is a pure UI-component kit. No gradients, blur, or glassmorphism anywhere in the source.
- **Borders vs shadows**: Paper/Card default to elevation (shadow, no border); an explicit `variant="outlined"` swaps to a 1px `--divider` border with no shadow — both patterns exist side-by-side in MUI and are preserved here.

## Iconography
The kit ships Material Icons as individual Figma vector components (filled style, a few outlined). 110 of the most-used glyphs were extracted as real SVG path data into `assets/icons/icon-data.js` (materialized from the kit, not redrawn) and wrapped in a single `<Icon name="…" size={20}/>` component — see the Iconography card. Icons are single-color and paint with `currentColor`. No emoji or Unicode-glyph icons appear anywhere in the source. No logo/brand mark exists in the source file, so the Influencer Monitor name always renders as type (see thumbnail and AppBar in the UI kit) — never invent one.

## Components
Organized under `components/<group>/`:
- **forms/**: Button, IconButton, ButtonGroup, Checkbox, Radio, RadioGroup, Switch, TextField, Select, Slider, ToggleButton, ToggleButtonGroup, Autocomplete, Rating, TransferList, Fab, LoadingButton, FormLabel, InputLabel, FormHelperText, FormControlLabel, FormGroup, DateField, TimeField, DateTimeField, DateCalendar, DatePicker, StaticDateTimePicker, MobileDateTimePicker, DateRangeCalendar, TimeClock, TimePicker, DateTimePicker
- **data-display/**: Avatar, AvatarGroup, Badge, Chip, Divider, List, ListItem, ListItemIcon, ListItemText, Tooltip, Table, TableHead, TableBody, TableRow, TableCell, TableHeadRow, TableCellRow, Timeline, TimelineItem, TimelineSeparator, TimelineDot, TimelineConnector, TimelineContent, TimelineOppositeContent, ImageList, ImageListItem, ImageListItemBar, Typography, BarChart, LineChart, PieChart, ScatterChart, ChartAxisX, ChartAxisY, DataGrid, DataGridTable, GridToolbarQuickFilter, GridToolbarColumnsButton, GridToolbarFiltersButton, GridToolbarDensitySelector, GridToolbarExportButton, TreeView, TreeItem
- **feedback/**: Alert, ProgressCircular, ProgressLinear, Skeleton, Snackbar, Backdrop, Dialog, DialogTitle, DialogContent, DialogActions
- **surfaces/**: Paper, Card, CardHeader, CardContent, CardActions, CardMedia, Accordion, AccordionSummary, AccordionDetails, AppBar, Toolbar
- **navigation/**: Tabs, Tab, Breadcrumbs, Link, Pagination, PaginationItem, Menu, MenuItem, MenuList, Popover, Drawer, BottomNavigation, BottomNavigationAction, SpeedDial, Stepper, Step, MobileStepper
- **layout/**: Container, Stack
- **assets/icons/**: Icon (110 glyphs)

## UI kit
`ui_kits/influencer-monitor/` — Dashboard, Mentions feed, Creator profile, and Alerts/Settings screens for Influencer Monitor, composed entirely from the components above.

## Index
- `styles.css` — root stylesheet, imports `tokens/fonts.css`, `tokens/colors.css`, `tokens/typography.css`, `tokens/spacing.css`
- `tokens/` — colors, typography, spacing/shadows, `@font-face`
- `assets/fonts/` — Rubik (14 files); `assets/icons/` — Icon component + data
- `components/` — see above
- `ui_kits/influencer-monitor/` — product screens
- `SKILL.md` — Claude-Code-compatible skill wrapper

## Caveats
- **Epilogue** (title font) has no uploaded binary — substituted from Google Fonts via `@import` in `tokens/fonts.css`. If Influencer Monitor uses a specific Epilogue license/weight set, please attach the files and I'll swap the self-hosted `@font-face` in.
- 183 of 237 Figma component families are intentionally deferred (see "Component coverage" above) — ask to continue building any of them.
- Button padding uses the simplified doubling rule (see Visual foundations) rather than the kit's literal per-variant padding values, per explicit instruction for this build.

**Ask**: which deferred component families matter most for Influencer Monitor's actual screens (Autocomplete + Slider + ToggleButton for filters? Drawer + Stepper for onboarding? Data Grid for the mentions table?) — tell me and I'll build those next, and I'm happy to iterate on the IM Blue/Malachite palette pairing or the Rubik/Epilogue type pairing if you'd like to see alternatives.
