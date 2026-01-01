# Purple Theme for WiFi Pineapple Pager

A custom purple/green theme for the Hak5 WiFi Pineapple Pager device.

## Features

- Purple color scheme with green highlights when selected
- 7-button dashboard layout with zigzag pattern
- Custom icons for ALERTS, PAYLOADS, RECON, PINEAP, SETTINGS, GPSD, and GEIGER
- GPSD restart button for quick GPS daemon restart
- GEIGER toggle for recon audio
- Dark green toggle background when highlighted

## Installation

1. Copy the theme to your pager:
```bash
scp -r purple root@172.16.52.1:/root/themes/
```

2. Restart the pager UI:
```bash
ssh root@172.16.52.1 "/etc/init.d/pineapplepager restart"
```

3. Select the theme from Settings > Display on your pager

## Dashboard Buttons

| Button | Function |
|--------|----------|
| ALERTS | View alerts |
| PAYLOADS | Access payloads |
| RECON | Reconnaissance tools |
| PINEAP | PineAP settings |
| SETTINGS | Device settings |
| GPSD | Restart GPS daemon |
| GEIGER | Toggle recon audio |

## Customization

- Edit `theme.json` for color palette
- Edit `components/dashboards/wargames_dashboard.json` for button layout
- Icons are in `assets/dashboard/`

## Author

brAinphreAk

## License

MIT
