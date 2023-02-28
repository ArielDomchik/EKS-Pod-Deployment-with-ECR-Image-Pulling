FROM python:alpine
#RUN adduser -D myuser && chown -R myuser /templates
#USER myuser
COPY . .
RUN pip install -r requirements.txt
EXPOSE 8080
CMD ["python3", "app.py"]
