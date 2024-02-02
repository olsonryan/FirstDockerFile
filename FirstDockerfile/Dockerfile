# Use rocker/tidyverse as the base image
FROM rocker/tidyverse:latest

# Install Ubuntu required library dependencies
RUN apt-get update && apt-get install -y \
    libssl-dev \
    libcurl4-gnutls-dev \
    libxml2-dev \
    libfontconfig1-dev

# Install R packages
RUN R -e "install.packages(c('tidyquant', 'plotly'), dependencies = TRUE, repos = 'https://packagemanager.rstudio.com/cran/latest')"

# Add user for RStudio if it doesn't exist
RUN if [ ! $(id -u rstudio 2>/dev/null) ]; then \
    useradd rstudio && \
    echo "rstudio:rstudio" | chpasswd && \
    mkdir /home/rstudio; \
fi

# Change ownership to rstudio
RUN chown rstudio:rstudio /home/rstudio
ENV PASSWORD rstudio

# Copy the rstudio-prefs.json file into the appropriate directory
COPY rstudio-prefs.json /home/rstudio/.config/rstudio/rstudio-prefs.json

# Expose port for RStudio
EXPOSE 8787

CMD ["/init"]