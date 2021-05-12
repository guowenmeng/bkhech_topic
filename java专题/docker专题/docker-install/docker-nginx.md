
mkdir -p /home/guowm/data/nginx/www /home/guowm/data/nginx/logs /home/guowm/data/nginx/conf

docker run --name nginx-test -p 8081:80 -d nginx

sudo docker cp 52f70a594bb1:/etc/nginx/nginx.conf /home/guowm/data/nginx/conf



docker run -d -p 52080:80 --name nginx-local-web -v /home/guowm/data/nginx/www:/usr/share/nginx/html -v /home/guowm/data/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/guowm/data/nginx/logs:/var/log/nginx nginx



docker run -d -p 52080:80 --name nginx-local-web -v /home/guowm/data/nginx/www:/usr/share/nginx/html nginx