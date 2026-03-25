FROM nginx:latest

WORKDIR /usr/share/nginx/html

# Copy frontend files
COPY index.html .
COPY style.css .

# Create entrypoint script with proper line endings (avoiding Windows CRLF issues)
RUN echo '#!/bin/bash' > /docker-entrypoint-custom.sh && \
    echo 'set -euo pipefail' >> /docker-entrypoint-custom.sh && \
    echo ': ${WS_BACK_URL:=http://localhost:8080}' >> /docker-entrypoint-custom.sh && \
    echo 'echo "Will start Nginx with the following configuration:"' >> /docker-entrypoint-custom.sh && \
    echo 'echo "  - WS_BACK_URL : $WS_BACK_URL"' >> /docker-entrypoint-custom.sh && \
    echo 'sed -i "s@TR_WS_BACK_URL@${WS_BACK_URL}@" /usr/share/nginx/html/index.html' >> /docker-entrypoint-custom.sh && \
    echo 'exec /docker-entrypoint.sh nginx -g "daemon off;"' >> /docker-entrypoint-custom.sh && \
    chmod +x /docker-entrypoint-custom.sh

# Set default environment variable for backend URL
ENV WS_BACK_URL=http://localhost:8080

# Expose port 80 for frontend
EXPOSE 80

# Use entrypoint script
ENTRYPOINT ["/docker-entrypoint-custom.sh"]
