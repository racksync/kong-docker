# 🐳 Kong Docker deployment
---

Kong API Gateway deployment with docker and docker-compose

## Status

This deployment is production ready.

## Change default value

Copy `default.env` to `.env`

```
cp default.env .env
```

then edit the `.env` file to change default values.

| Variable name | Default value |
|---------------|---------------|
| `POSTGRES_VERSION`    | 14-alpine |
| `POSTGRES_USER`       | kong |
| `POSTGRES_PASSWORD`   | kong |
| `POSTGRES_DB`         | kong |
| `KONG_ADMIN_LISTEN`   | 0.0.0.0:8001 |
| `KONG_PROXY_LISTEN`   | 0.0.0.0:8000, 0.0.0.0:8443 ssl http2 |
| `KONG_PG_HOST`        | kong-database |
| `KONG_PG_PASSWORD`    | kong == `env POSTGRES_PASSWORD` |
| `COMPOSE_PROJECT_NAME`| kong |
| `KONG_DATABASE`       | postgres |
| `KONG_CASSANDRA_CONTACT_POINTS` | kong-database |

## Deploy Kong step by step

### Deploy kong-database

```
docker-compose up -d kong-database
```

### Run kong-database migrations

```
docker-compose run --rm kong-migrations
```

### Start Kong

```
docker-compose up -d kong
```

**Now Kong is running**

- Kong Admin API http://127.0.0.1:8001
- Kong Proxy http://127.0.0.1:8000

### Explanation of Dependency Flow

- `kong-database` is foundational because both `kong-migrations` and `kong` rely on the database to function.
- `kong-migrations` ensures the database is initialized and ready for use by Kong.
- `kong` is the last to start, as it requires the database and migrations to be fully prepared.

By setting `depends_on`, Docker Compose handles these dependencies automatically, starting the containers in the correct order as long as all services are defined in the same file and executed using `docker-compose up`. However, note that `depends_on` only checks if a container starts, not if it is fully ready, so the health checks ensure readiness.

### Upgrading Kong version

> **WARNING** : This may take some downtime to start a new Kong version.

1. Change `KONG_VERSION` in `.env` file to a newer version.

	> If running kong version 2.2 change to 2.3 (or 2.3.0). [Suggested upgrade path](https://github.com/Kong/kong/blob/master/UPGRADE.md)

2. Run migrations upgrade

	```
	docker-compose run --rm kong kong migrations up --vv
	```

3. Confirm finish migrations upgrade

	```
	docker-compose run --rm kong kong migrations finish --vv
	```
	
4. Start new Kong version

	```
	docker-compose up -d kong
	```

### 📚 Automation Training

- [สินค้าและบริการ](http://racksync.com)
- [เทรนนิ่งคอร์ส](https://facebook.com/racksync)

### 👥 Community

- [Home Automation Thailand](https://www.facebook.com/groups/hathailand)
- [Home Automation Marketplace](https://www.facebook.com/groups/hatmarketplace)
- [Home Automation Thailand Discord](https://discord.gg/Wc5CwnWkp4)

## 🏢 [RACKSYNC CO., LTD.](https://racksync.com)

RACKSYNC Co., Ltd. specializes in automation and smart solutions of all scales. We are experts in designing, implementing, and monitoring sophisticated automation systems. Our team of specialists provides comprehensive consulting services and technical implementation for both residential and commercial projects. Beyond automation, we offer full-cycle Software as a Service (SaaS) development, helping businesses transform their operations through custom digital solutions. With our deep expertise in IoT, home automation, and enterprise systems, we deliver reliable and innovative solutions tailored to each client's unique requirements.

📍 RACKSYNC COMPANY LIMITED    
🌏 Suratthani, Thailand 84000   
📧 Email : devops@racksync.com   
📞 Tel : +66 85 880 8885   

[![Home Automation Thailand Discord](https://img.shields.io/discord/986181205504438345?style=for-the-badge)](https://discord.gg/Wc5CwnWkp4) [![Github](https://img.shields.io/github/followers/racksync?style=for-the-badge)](https://github.com/racksync) 
[![WebsiteStatus](https://img.shields.io/website?down_color=grey&down_message=Offline&style=for-the-badge&up_color=green&up_message=Online&url=https%3A%2F%2Fracksync.com)](https://racksync.com)



