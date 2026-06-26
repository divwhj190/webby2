FROM mcr.microsoft.com/devcontainers/base:ubuntu

ENV DEBIAN_FRONTEND=noninteractive

# Install XFCE Desktop, VNC server, Chromium browser, and helper tools
RUN apt-get update && apt-get install -y \
    xfce4 \
    xfce4-goodies \
    tigervnc-standalone-server \
    chromium-browser \
    novnc \
    websockify \
    dbus-x11 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Create the desktop startup script safely inside the container path
RUN echo '#!/bin/bash\n\
# Clean up any lingering VNC locks\n\
vncserver -kill :1 2>/dev/null\n\
rm -f /tmp/.X11-unix/X1\n\
\n\
# Start VNC Server with basic settings (No password needed for local workspace)\n\
vncserver :1 -geometry 1280x800 -depth 24 -SecurityTypes None -xstartup /usr/bin/startxfce4 &\n\
sleep 2\n\
\n\
# Launch Chromium within the VNC display frame\n\
DISPLAY=:1 chromium-browser --no-sandbox --disable-gpu --disable-dev-shm-usage &\n\
\n\
# Run noVNC proxy to translate VNC traffic to port 6080 natively\n\
/usr/share/novnc/utils/launch.sh --vnc localhost:5901 --listen 6080\n\
' > /usr/local/bin/start-desktop.sh && chmod +x /usr/local/bin/start-desktop.sh
