FROM python:3.7-slim

ENV PYTHONUNBUFFERED=1
ENV ROOT=/usr/src/app

WORKDIR ${ROOT}

COPY stilio/config.py stilio/config.py
COPY stilio/__init__.py stilio/__init__.py
COPY stilio/frontend stilio/frontend
COPY stilio/persistence stilio/persistence

RUN echo 'deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main\n\
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main\n\        
    deb http://security.debian.org/debian-security buster/updates main\n\     
    deb-src http://security.debian.org/debian-security buster/updates main\n\ 
    deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main\n\    
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main\n'\
    > /etc/apt/sources.list

RUN apt-get update \
    && apt-get install -y build-essential

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

RUN apt-get remove -y build-essential \
    && apt-get -y autoremove

CMD ["gunicorn", "-k", "uvicorn.workers.UvicornWorker", "-b", "0.0.0.0:8000", "stilio.frontend.main:app"]
