# webserver-ansible

Ansible-rooli joka asentaa ja konfiguroi valitun webpalvelimen (Apache2 tai Nginx) automaattisesti — valintasi mukaan interaktiivisesti asennuksen aikana. Tukee myös HTTPS-sertifikaatin hankkimista Certbotin avulla.

**Tekijä:** Joonas Laine | [joonaslaine.com](https://joonaslaine.com) | [GitHub: Havikki](https://github.com/Havikki)  
**Lisenssi:** GNU General Public License v3.0

---

## Lopputulos

![Kuvakaappaus asennetusta sivusta](https://github.com/Havikki/webserver-ansible/issues/1#issue-4372489483)

---

## Vaatimukset

- Ansible 2.9+
- Kohdekone: Debian/Ubuntu
- Certbot-asennusta varten: oikea domain joka osoittaa kohdekoneen IP:hen

## Asennus

```bash
git clone https://github.com/Havikki/webserver-ansible
cd webserver-ansible
```

## Käyttöönotto

1. Muokkaa `inventory`-tiedostoon kohdekoneesi osoite
2. Aja playbook:

```bash
ansible-playbook -i inventory site.yml
```

Playbook kysyy asennuksen alussa:
- **Webpalvelin:** `apache2` tai `nginx`
- **Certbot:** haluatko HTTPS-sertifikaatin (`yes`/`no`)
- **Domain:** domain-nimi (tarvitaan vain certbotille)
- **Sähköposti:** certbot-ilmoituksia varten (tarvitaan vain certbotille)

Asennus on idempotentti — voit ajaa playbookin uudelleen ilman sivuvaikutuksia.

## Rakenne

```
webserver-ansible/
├── site.yml                        # pääplaybook (vars_prompt)
├── inventory                       # kohdekone(et)
└── roles/
    └── webserver/
        ├── tasks/
        │   ├── main.yml            # package-file-service -runko
        │   ├── apache.yml          # apache2-spesifi asennus
        │   ├── nginx.yml           # nginx-spesifi asennus
        │   └── certbot.yml         # HTTPS-sertifikaatti
        ├── handlers/
        │   └── main.yml            # reload webserver
        └── templates/
            ├── index.html.j2       # dynaaminen etusivu
            ├── apache.conf.j2      # VirtualHost-konfiguraatio
            └── nginx.conf.j2       # server block -konfiguraatio
```

## Lisenssi

GNU General Public License v3.0 — katso [LICENSE](LICENSE)
