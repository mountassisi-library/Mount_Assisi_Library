# Use the official Python image as the base image
FROM python:3.9

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Set the working directory in the container
WORKDIR /library

# Copy the dependencies file to the working directory
COPY requirements.txt /library/

# Install dependencies
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

# Copy the current directory contents into the container at /app
COPY . /library/

# ADD THIS LINE:
RUN mkdir -p /library/staticfiles

# Expose the port on which the Django app will run
EXPOSE 8000

# Create startup script
RUN echo '#!/bin/bash\n\
python manage.py migrate\n\
python manage.py collectstatic --noinput\n\
echo "from django.contrib.auth import get_user_model; User = get_user_model(); User.objects.filter(username='"'"'admin'"'"').exists() or User.objects.create_superuser('"'"'admin'"'"', '"'"'admin@example.com'"'"', '"'"'admin123'"'"')" | python manage.py shell\n\
gunicorn library_management_backend.wsgi --bind 0.0.0.0:8000 --workers 1 --threads 2 --timeout 120 --log-file -' > /start.sh

RUN chmod +x /start.sh

CMD ["/start.sh"]