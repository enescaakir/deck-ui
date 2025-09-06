# 🖥️ Deck UI

**Deck UI** is a modern and user-friendly control panel interface developed for Windows systems. It works integrated with **Deck API** and **Deck Media API** to provide real-time management of your system controls (volume, brightness, power management) and media player controls.

## 🌟 Features

- **🕐 Real-time Clock**: Live clock display in HH:MM format
- **🔊 Volume Control**: Interactive volume level control and mute/unmute functionality
- **💡 Brightness Control**: Monitor brightness adjustment with vertical slider
- **⚡ System Management**: One-click shutdown, sleep mode, screen lock
- **🎵 Universal Media Control**: Spotify, YouTube, VLC, Windows Media Player support
- **📊 Rich Media Information**: Artist, song title, duration and position display
- **🎨 Dynamic Background**: Auto-updating system wallpaper
- **📱 Modern Design**: Glassmorphism effects and smooth animations

## 📋 Requirements

For this UI to show full functionality, the following services must be installed and running:

### Required Dependencies
1. **[Deck API](https://github.com/enescaakir/deck-api)** - For system controls
   - Port: `2837`
   - Volume, brightness, power management and wallpaper control

2. **[Deck Media API](https://github.com/enescaakir/deck-media-api)** - For media controls  
   - Port: `3728`
   - Universal media player control and information retrieval

### WebSocket Connections
- `ws://localhost:5050` - Brightness level changes
- `ws://localhost:5051` - Volume level changes  
- `ws://localhost:5052` - Background image changes
- `ws://localhost:3728/ws/media` - Media status and information updates

## 🚀 Installation and Setup

### 1. Deck API Installation
```bash
git clone https://github.com/enescaakir/deck-api.git
cd deck-api
python -m venv venv
venv\Scripts\activate
pip install fastapi uvicorn websockets screen-brightness-control pycaw comtypes python-multipart
python main.py
```

### 2. Deck Media API Installation
```bash
git clone https://github.com/enescaakir/deck-media-api.git
cd deck-media-api  
python -m venv venv
venv\Scripts\activate
pip install fastapi uvicorn winrt
python main.py
```

### 3. Running Deck UI
- Open the `index.html` file in any modern web browser
- Recommended resolution: 1280x400 pixels
- Press F11 for full screen experience

## 🎮 Usage Guide

### 🔴 Main Controls (Left Panel)
- **⚡ Power Button**: Safely shuts down the system
- **🌙 Sleep Button**: Puts system into sleep mode (standby)  
- **🔒 Lock Button**: Locks the work screen
- **🔊 Volume Button**: Master volume on/off (mute toggle)

### 📊 Level Controls (Middle Panel)
- **💡 Brightness Slider**: 
  - Instant adjustment with vertical clicking
  - Yellow icon indicator above 10%
  - Multi-monitor support
- **🔊 Volume Slider**:
  - Volume level adjustment with vertical clicking
  - Blue icon indicator above 10%
  - Automatic icon change when muted

### 🎵 Media and Info Panel (Right Panel)
- **👤 Artist Info**: Currently playing song's artist name
- **🎵 Song Title**: Scrolling text animation (for long titles)
- **⏯️ Position Bar**: Current song progress
- **⏱️ Time Display**: Current position / Total duration (HH:MM:SS)
- **🎨 Visual Content**: Decorative GIF animation

## 🔧 Technical Details

### 🌐 CSS Dependencies
```html
<!-- Tailwind CSS CDN -->
<script src="https://cdn.tailwindcss.com"></script>
<!-- Material Icons -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/material-icons@1.13.14/iconfont/material-icons.min.css">
<!-- Roboto Mono Font -->
<link href="https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@700&display=swap" rel="stylesheet">
```

### ⚙️ JavaScript Features
- **Real-time WebSocket**: Instant data synchronization
- **REST API Integration**: HTTP endpoint calls
- **Custom Vertical Sliders**: Customized vertical slider controls
- **Smart Marquee**: Smart scrolling text for long content
- **Dynamic Icon Management**: Status-based icon changes

### 🔗 API Integrations

#### Deck API Endpoints
```javascript
// System controls
fetch("http://localhost:2837/system?action=shutdown")  // Shutdown
fetch("http://localhost:2837/system?action=standby")   // Sleep
fetch("http://localhost:2837/system?action=lock")      // Lock

// Volume controls
fetch("http://localhost:2837/volume?level=50")         // Volume level
fetch("http://localhost:2837/volume?level=mute")       // Mute toggle

// Brightness controls
fetch("http://localhost:2837/brightness?level=75")     // Brightness

// Background
fetch("http://localhost:2837/bg")                      // Wallpaper
```

#### Deck Media API Integration
```javascript
// Get media information
fetch("http://localhost:3728/media")

// Media controls (will be active in future versions)
fetch("http://localhost:3728/media?action=play")
fetch("http://localhost:3728/media?action=pause")
fetch("http://localhost:3728/media?action=next")
fetch("http://localhost:3728/media?action=previous")
```

#### 📡 WebSocket Usage
```javascript
// Listen to volume level changes
const audioWs = new WebSocket("ws://localhost:5051/");
audioWs.onmessage = ({ data }) => {
    volumeFill.style.height = `${data}%`;
    updateVolumeIcon(data);
};

// Get media information in real-time
const mediaWs = new WebSocket("ws://localhost:3728/ws/media");
mediaWs.onmessage = (event) => {
    const data = JSON.parse(event.data);
    if (!data.error) {
        setMediaInfo(data.artist, data.title, data.position, data.duration);
    }
};
```

## 🎨 Customization

### 🎯 Color Palette
```css
:root {
    --custom-red: #ff4757;      /* Power button */
    --custom-blue: #3742fa;     /* Sleep button & Volume */
    --custom-dark: #2f3542;     /* Lock button */
    --custom-gray: #57606f;     /* Mute button */
}
```

### 📐 Size Settings
- **Main Window**: 1280x400px
- **Clock Display**: 170px Roboto Mono
- **Icon Sizes**: 40-50px Material Icons
- **Control Buttons**: 80x80px circular
- **Slider Height**: 100% responsive

### 🖼️ Changing Visual Content
```html
<!-- Change the GIF in the right panel -->
<img src="https://your-gif-url-here.gif" 
     class="w-full h-full rounded-xl object-cover" />
```

## 🎵 Supported Media Players

Thanks to Deck Media API, it works fully compatible with the following applications:

- **🎵 Spotify** - Premium & Free
- **🌐 YouTube** - Web-based
- **🎬 Windows Media Player** - Native support  
- **🎵 Apple Music/iTunes** - Full integration
- **🎵 VLC Media Player** - Advanced control
- **🎵 Groove Music** - Windows built-in
- **🎵 Deezer** - Streaming service
- **🎵 Tidal** - Hi-Fi audio

## ⚠️ Troubleshooting

### 🔧 Common Issues and Solutions

#### 1. WebSocket Connection Errors
```bash
# Check if services are running
netstat -an | findstr :2837
netstat -an | findstr :3728
netstat -an | findstr :5050
netstat -an | findstr :5051
netstat -an | findstr :5052
```

#### 2. API Not Responding
- Check that Deck API is active on port 2837
- Check Windows Firewall settings
- Check if antivirus software is blocking the API

#### 3. No Media Information Coming
- Check that a media player is open and running
- Verify that Deck Media API is running on port 3728
- Check that Windows Media Controls are active

#### 4. Slider Not Working
- Check error messages in JavaScript console
- Verify that mouse event listeners are loaded

### 🐛 Debug Commands
```javascript
// Paste into console to check connection status
console.log('Audio WS:', audioWs.readyState);
console.log('Brightness WS:', brightnessWs.readyState);  
console.log('Media WS:', mediaWs.readyState);
console.log('Background WS:', backgroundImageWs.readyState);
// readyState: 0=CONNECTING, 1=OPEN, 2=CLOSING, 3=CLOSED
```

## 📊 Performance

- **Memory Usage**: ~15-25MB (browser dependent)
- **CPU Usage**: <1% (idle state)
- **WebSocket Latency**: <50ms (local network)
- **API Response Time**: <100ms (average)

## 🤝 Contributing

1. **Fork** - Copy the repository to your own account
2. **Create Branch** - `git checkout -b feature/new-feature`
3. **Commit** - `git commit -m 'Add new feature'`
4. **Push** - `git push origin feature/new-feature`
5. **Open Pull Request** - For us to review your changes

### 🎯 Contribution Guide
- Design compliant with Material Design rules
- Responsive and accessible code writing
- Comment lines in JSDoc format
- Pay attention to cross-browser compatibility

## 📄 License

This project is licensed under the **MIT License**. For details, please check the [LICENSE](LICENSE) file.

## 📞 Contact & Support

**👨‍💻 Developer**: Enes Çakır

🌐 **Website**: [enescakr.com](https://enescakr.com/)  
💼 **LinkedIn**: [linkedin.com/in/enescaakir](https://www.linkedin.com/in/enescaakir/)  
🐙 **GitHub**: [github.com/enescaakir](https://github.com/enescaakir)

---

⭐ **If you like this project, don't forget to star it on GitHub!**  
🔄 **Click the "Watch" button to stay updated!**  
🤝 **Visit the Discussions section to interact with the community!**
