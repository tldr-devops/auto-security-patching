# auto-updates

[Stand with Belarus against dictatorship <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Presidential_Standard_of_Belarus_%28fictional%29.svg/240px-Presidential_Standard_of_Belarus_%28fictional%29.svg.png" width="20" height="20" alt="Voices From Belarus" />](https://bysol.org/en/) [![Stand With Ukraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://vshymanskyy.github.io/StandWithUkraine)

Apply updates to your servers automatically and securely

Warning: I'm living in Belarus - country between EU and Russia. [And today we are fighting for our freedom against 'the last dictator of the Europe'](https://www.euronews.com/2020/06/25/belarus-is-no-longer-scared-of-lukashenko-europe-s-last-dictator-will-fall-view).
So I can't guarantee that I'll be able to maintain this repo scrupulously. Sorry, guys

**Motivation**: I manage hundreds of hosts and I know that manually patching is hard even with automation like ansible or puppet or whatever. Infrastructure can work for months or even years without properly security updates. Docker containers with planned rebuilding and delivery can solve the problem, but most of the internet still work not in docker :) So in my opinion limited automatic updates with monitoring, notifications and canary tests on the stage environment is the less evil, than unpatched servers :)

I added into stop list databases and services like docker, which restart can seriously affect your production. However, I left web servers and programming languages as I think that it should be patched anyway even with restart. Anyway, you should review stop list, pin necessary packages and choose date and time for your servers based on roles. I prefer automatic updates during the daytime on Monday for test and on Wednesday and Thurday for prod, when whole team can response to the problems.

Time track:
- [Filipp Frizzy](https://github.com/Friz-zy/) 2.5h

## Support

You can support this or any other of my projects
* by sending your PRs with improving my configs or english texts 😂
* by sending me donations:
  - [donationalerts.com/r/filipp_frizzy](https://www.donationalerts.com/r/filipp_frizzy)
  - ETH 0xCD9fC1719b9E174E911f343CA2B391060F931ff7
  - BTC bc1q8fhsj24f5ncv3995zk9v3jhwwmscecc6w0tdw3

## DEB based

1. `apt update && apt install unattended-upgrades`
2. `nano /etc/apt/apt.conf.d/20auto-upgrades`
```
APT::Periodic::Update-Package-Lists "0";
APT::Periodic::Unattended-Upgrade "0";
```
3. `systemctl disable --now apt-daily{,-upgrade}.{timer,service}`
4. `cp unattended-upgrades /etc/apt/apt.conf.d/50unattended-upgrades`
5. `crontab -e`
```
# update list of packages on all servers at the same time
0 12 * * 1 /usr/bin/apt update
# install updates on the TEST
10 12 * * 1 /usr/bin/unattended-upgrades
# install updates on the half of PROD
10 12 * * 3 /usr/bin/unattended-upgrades
# install updates on the other half of PROD
10 12 * * 4 /usr/bin/unattended-upgrades
```

## RPM based
https://serverfault.com/questions/567195/how-can-i-exclude-a-package-from-yum-cron-but-not-from-manual-yum-upgrade
