# Modified docker-duplicity setup to fix pyDrive and Google drive backup

  * [`latest`](https://github.com/wernight/docker-duplicity)
  Refer to documentation at original repo for information on authenticating to Google Drive

# Can be run like so:
## Backup

```
docker run --rm --user $UID \
	     -e PASSPHRASE=mypassword \
      -e GOOGLE_DRIVE_ACCOUNT_KEY="$(cat pydriveprivatekey.pem)" \
      -v $PWD/.cache:/home/duplicity/.cache/duplicity \
      -v $PWD/.gnupg:/home/duplicity/.gnupg \
      -v ~/backup:/data:ro \
      -e github='https://github.com/maxcrossan/docker-duplicity.git' -t dupmax \
      duplicity --full-if-older-than=6M --allow-source-mismatch --no-encryption /data pydrive://duplicity@123.iam.gserviceaccount.com/backup/
```

## Restore

```
docker run --rm --user $UID \
	     -e PASSPHRASE=mypassword \
      -e GOOGLE_DRIVE_ACCOUNT_KEY="$(cat pydriveprivatekey.pem)" \
      -v $PWD/.cache:/home/duplicity/.cache/duplicity \
      -v $PWD/.gnupg:/home/duplicity/.gnupg \
      -v ~/restore:/data \
      -e github='https://github.com/maxcrossan/docker-duplicity.git' -t dupmax \
      duplicity --allow-source-mismatch restore pydrive://duplicity@123.iam.gserviceaccount.com/backup/ /data
```
