FROM python:3.7-slim-buster

ENV PYTHONUNBUFFERED=1
ENV ROOT=/usr/src/app

WORKDIR ${ROOT}

COPY stilio/config.py stilio/config.py
COPY stilio/__init__.py stilio/__init__.py
COPY stilio/crawler stilio/crawler
COPY stilio/persistence stilio/persistence

RUN echo 'deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main\n\
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main\n\        
    deb http://security.debian.org/debian-security buster/updates main\n\     
    deb-src http://security.debian.org/debian-security buster/updates main\n\ 
    deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main\n\    
    deb-src http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main\n'\
    > /etc/apt/sources.list

RUN apt-get update \
    # psycopg2 dependencies
    && apt-get install -y gcc python3-dev musl-dev

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

RUN apt-get remove -y build-essential \
    && apt-get -y autoremove

COPY docker/crawler/entrypoint.sh entrypoint.sh
RUN sed -i 's/\r$//g' entrypoint.sh

ENTRYPOINT ["bash", "entrypoint.sh"]

CMD ["python", "-m", "stilio.crawler.main"]
