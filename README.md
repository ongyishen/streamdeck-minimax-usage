# MiniMax Usage - Stream Deck/Mirabox Plugin

> Monitor your MiniMax Coding Plan API quota directly from your Stream Deck

## Overview

This plugin enables Stream Deck users to monitor their MiniMax AI Coding Plan API usage in real-time. It displays usage statistics, remaining quota, and time limits directly on Stream Deck buttons with color-coded indicators.

## Features

- **Real-time Monitoring**: Fetches and displays current API usage from MiniMax servers
- **Visual Indicators**: Color-coded usage display:
  - 🟢 **Green**: < 80% usage (healthy)
  - 🟡 **Yellow**: 80-90% usage (warning)
  - 🔴 **Red**: > 90% usage (critical)
- **Configurable**: Customize API key, model name and refresh interval
- **Multi-model Support**: Works with any MiniMax model (MiniMax-M2.7, MiniMax-M2.5, etc.)
- **Auto-refresh**: Configurable polling interval (1-60 minutes)
- **Manual Refresh**: Press button to instantly update usage data
- **Time Display**: Shows remaining time in hours and minutes

## Installation

### Prerequisites

- Stream Deck application (compatible with Elgato Stream Deck)
- MiniMax account with API key

### Steps

1. Download this plugin repository
2. Copy plugin folder to your Stream Deck plugins directory:
   - **Windows**: `C:\Users\[YourUsername]\AppData\Roaming\Elgato\StreamDeck\Plugins\`
   - **macOS**: `~/Library/Application Support/com.elgato.StreamDeck/Plugins/`
3. Restart Stream Deck application
4. The plugin will appear under "Minimax" category in Actions panel

## Configuration

### Accessing Settings

1. Drag "Minimax Usage" action to a Stream Deck key
2. Right-click key to open Property Inspector
3. Configure the following settings:

| Setting                 | Description                      | Default          |
| ----------------------- | -------------------------------- | ---------------- |
| **API Key**       | Your MiniMax API key (required)  | -                |
| **Model Name**    | Target MiniMax model name        | `MiniMax-M2.7` |
| **Refresh (min)** | Auto-refresh interval in minutes | `5`            |

### Getting Your MiniMax API Key

1. Visit [MiniMax Platform](https://api.minimax.io)
2. Sign in to your account
3. Navigate to API Key management
4. Copy your API key and paste it into the plugin settings

## Usage

### Display Information

The Stream Deck button displays three pieces of information:

```
┌─────────────┐
│  Model      │ ← Model name (top)
│             │
│  45.2%      │ ← Usage % (center, colored)
│             │
│  4 hr 47 min│ ← Reset in (bottom)
└─────────────┘
```

- **Top**: Model name (e.g., "MiniMax-M2.7")
- **Center**: Usage percentage with color indicator
- **Bottom**: Last update timestamp (Singapore time)

### Manual Refresh

Press the Stream Deck button to immediately fetch and update usage data without waiting for the auto-refresh timer.

### Error Messages

| Message        | Cause                        | Solution                             |
| -------------- | ---------------------------- | ------------------------------------ |
| `No API Key` | API key not configured       | Enter your API Key in settings       |
| `0 Quota`    | No quota data available      | Check your MiniMax plan              |
| `No Plan`    | Model not found in your plan | Verify model name setting            |
| `API Err`    | API request failed           | Check API key and network connection |
| `Net Err`    | Network connection issue     | Check internet connection            |

## Technical Details

### API Endpoint

- **URL**: `https://api.minimax.io/v1/api/openplatform/coding_plan/remains`
- **Method**: GET
- **Auth**: Bearer token (API Key)
- **Response**: JSON with usage statistics per model

### Event Handling

The plugin handles these Stream Deck events:

| Event                  | Description                      |
| ---------------------- | -------------------------------- |
| `willAppear`         | Initialize button, start polling |
| `willDisappear`      | Clean up, stop polling           |
| `keyUp`              | Manual refresh trigger           |
| `didReceiveSettings` | Apply new configuration          |

### Refresh Logic

- Auto-refresh runs on a configurable timer
- Timer persists across button appearances/disappearances
- Settings changes restart the polling timer with new interval

## References

- **MiraBox**: [Official Product Website](https://mirabox.key123.vip/home)
- **MiraBox Dock SDK Guide**: [Official Documentation](https://sdk.key123.vip/en/guide/overview.html)
- **MiniMax API Platform**: [api.minimax.io](https://api.minimax.io)

## File Structure

```
com.ongyishen.minimax.usage.sdPlugin/
├── manifest.json              # Plugin manifest
├── readme.md                 # Stream Dock API documentation (Chinese)
├── README.md                 # Plugin documentation (English)
├── zh_CN.json               # Localization file
├── plugin/
│   ├── index.html            # Main plugin page
│   ├── index.js              # Core plugin logic
│   └── utils/
│       ├── common.js         # Stream Deck utilities
│       └── worker.js         # Worker for timers
├── propertyInspector/
│   └── action1/
│       ├── index.html        # Settings UI
│       ├── index.js          # Settings logic
│       └── utils/
│           ├── common.js     # Shared utilities
│           └── action.js     # Action utilities
└── static/
    ├── css/
    │   └── sdpi.css       # Stream Deck PI styles
    └── icon.png            # Plugin icon
```

## Supported Platforms

- **macOS**: 10.11+
- **Windows**: 7+
- **Stream Deck Software**: 2.9+

## API Details

### MiniMax Coding Plan API Response

```json
{
  "base_resp": {
    "status_code": 0
  },
  "model_remains": [
    {
      "model_name": "MiniMax-M2.7",
      "current_interval_total_count": 1000000,
      "current_interval_usage_count": 550000,
      "remains_time": 259200000  // milliseconds
    }
  ]
}
```

### Usage Calculation

```
Percentage = ((total - usage) / total) * 100
```

### Time Formatting

```
Hours = totalMinutes / 60
Minutes = totalMinutes % 60
Display = "X hr Y min"
```

## Development

### Building

No build step required - plugin uses vanilla JavaScript and standard web technologies.

### Testing

1. Configure with a valid MiniMax API key
2. Set refresh interval to 1 minute for quick testing
3. Monitor browser console for debug logs:
   - `[MiniMax] willAppear` - Button initialized
   - `[MiniMax] Polling every X minutes` - Timer status
   - `[MiniMax] keyUp - refreshing` - Manual refresh
   - `[MiniMax] Response:` - API response data

### Debug Mode

All debug messages are prefixed with `[MiniMax]` and logged to the console.

## Troubleshooting

### Button shows "Loading..."

- Check that your API key is valid
- Verify internet connection
- Wait for the first API call to complete

### Usage stuck at old value

- Press button to trigger manual refresh
- Check refresh interval setting (may be too long)
- Restart Stream Deck application

### Cannot see settings panel

- Ensure Property Inspector is enabled in Stream Deck settings
- Right-click button to open settings (not just click)

## Credits

- **Author**: ONG YI SHEN
- **Repository**: [ongyishen](https://github.com/ongyishen)

## License

This plugin is provided as-is for use with MiniMax API.

## Version History

### v1.0

- Initial release
- MiniMax Coding Plan API integration
- Configurable refresh intervals
- Color-coded usage indicators
- Multi-model support

## Support

For issues or questions:

1. Check [Stream Dock SDK Guide](https://sdk.key123.vip/en/guide/overview.html) for development reference
2. Verify your MiniMax API key is valid
3. Check browser console for error messages

---

**Note**: This plugin requires an active MiniMax account with a valid API key to function. API usage is subject to MiniMax's terms of service.
