# brAinphreAk's Pineapple Pager Themes

Custom themes for the Hak5 WiFi Pineapple Pager device.

## Available Themes

| Theme | Description |
|-------|-------------|
| [Purple Pineapple](purple_pineapple/) | Purple/green color scheme with custom dashboard |

## Installation

1. Copy a theme folder to your pager:
```bash
scp -r purple_pineapple root@172.16.52.1:/root/themes/
```

2. Restart the pager UI:
```bash
ssh root@172.16.52.1 "/etc/init.d/pineapplepager restart"
```

3. Select the theme from Settings > Display on your pager

## Creating Your Own Theme

Each theme folder should contain:
- `theme.json` - Theme configuration
- `assets/` - Images and icons
- `components/` - UI component definitions

## Author

brAinphreAk

## License

MIT
