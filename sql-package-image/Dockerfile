FROM mcr.microsoft.com/dotnet/runtime:6.0

# Install dependencies
RUN apt-get update && \
    apt-get install -y wget unzip libunwind8 && \
    rm -rf /var/lib/apt/lists/*

# Download and install sqlpackage
RUN wget -O sqlpackage.zip https://aka.ms/sqlpackage-linux && \
    unzip sqlpackage.zip -d /opt/sqlpackage && \
    chmod +x /opt/sqlpackage/sqlpackage && \
    ln -s /opt/sqlpackage/sqlpackage /usr/local/bin/sqlpackage && \
    rm sqlpackage.zip

# Set working directory
WORKDIR /tmp
