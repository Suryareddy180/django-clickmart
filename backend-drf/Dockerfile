# Purpose: A Dockerfile is a step-by-step instruction file that tells Docker how to build and run our application.
FROM python:3.10-slim 
#We are using the official Python 3.10 slim image as our base image, which is a lightweight version of Python that includes only the essential components needed to run Python applications.

ENV PYTHONUNBUFFERED=1 
#This ensures that Python output is sent straight to the terminal without being buffered, which is helpful for debugging and logging purposes.
ENV PYTHONDONTWRITEBYTECODE=1 
#This prevents Python from writing .pyc files, which are compiled bytecode files. This can save disk space and reduce clutter in the container.

WORKDIR /app
#This sets the working directory inside the container to /app. All subsequent commands will be run from this directory, and it is where we will copy our application code and install dependencies.

RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*
#This command updates the package list and installs the necessary system dependencies for our application. We install gcc for compiling any C extensions that might be required by our Python packages, and libpq-dev for PostgreSQL database support. After installing the dependencies, we clean up the package lists to reduce the image size.
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

# gunicorn = production server, clickmart_main.wsgi:application = Django entry point, --bind 0.0.0.0:8000 = external traffic. Reminaing: tuning options
# A worker is just one instance of your Django app running inside Gunicorn.
CMD ["gunicorn", "clickmart_main.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "3" , "--timeout", "180"]