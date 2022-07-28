FROM python:3.10

RUN pip install flask

EXPOSE 8080

WORKDIR /app
ADD server.py /app

CMD python server.py
