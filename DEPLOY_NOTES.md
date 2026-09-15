# MY NOTES ON DEPLOYING NESWEAR TO A CLOUD VM WITH HTTPS

1. SSH into the VM:

```bash
ssh -i key.pem username@host
```

2. Update packages & install Docker and Git.

3. Install certbot (to get HTTPS cert) & create a symlink:

```bash
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/local/bin/certbot
```

4. Get a standalone HTTPS cert:

```bash
sudo certbot certonly --standalone
```

5. Set up a cron job to renew the cert every day:

```bash
sudo crontab -e

0 3 * * * certbot renew --pre-hook "docker stop neswear_nginx" --post-hook "docker start neswear_nginx"
```

6. Clone the neswear-deploy repository using Git:

```bash
git clone https://github.com/levanvux/neswear-deploy.git
```

7. Go to the cloned repository directory:

```bash
cd neswear-deploy
```

8. Create .env from .env.example & modify the values:

```bash
cp .env.example .env
vi .env
```

9. Go to the GitHub repository (levanvux/neswear-deploy), then:

- Open `Actions`
- Run the `Deploy` workflow
- After the `Deploy` workflow is complete, run the `Seed database` workflow
