ARG BASE_IMAGE=m1k1o/neko:base
FROM $BASE_IMAGE

ARG SRC_URL="https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=en-US"

#
# Install dependencies
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends openbox xz-utils bzip2 libgtk-3-0 libdbus-glib-1-2; \
    \
    # Fetch latest Firefox release
    wget -O /tmp/firefox-setup.tar.xz "${SRC_URL}"; \
    mkdir -p /usr/lib/firefox; \
    tar -xJf /tmp/firefox-setup.tar.xz -C /usr/lib/firefox --strip-components=1; \
    rm -f /tmp/firefox-setup.tar.xz; \
    ln -s /usr/lib/firefox/firefox /usr/bin/firefox; \
    \
    # Create a profile directory
    mkdir -p /home/neko/.mozilla/firefox/profile.default/extensions; \
    chown -R neko:neko /home/neko/.mozilla/firefox/profile.default; \
    \
    # Clean up
    apt-get --purge autoremove -y xz-utils bzip2; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*

#
# Ensure necessary directories exist before copying
RUN mkdir -p /usr/lib/firefox/defaults/pref /usr/lib/firefox/distribution

#
# Copy configuration files
COPY supervisord.conf /etc/neko/supervisord/firefox.conf
COPY neko.js /usr/lib/firefox/mozilla.cfg
COPY autoconfig.js /usr/lib/firefox/defaults/pref/autoconfig.js
COPY policies.json /usr/lib/firefox/distribution/policies.json
COPY --chown=neko profiles.ini /home/neko/.mozilla/firefox/profiles.ini
COPY openbox.xml /etc/neko/openbox.xml
