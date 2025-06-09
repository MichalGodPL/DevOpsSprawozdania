# Sprawozdanie z Laboratorium 11 - Ansible

## Cel laboratorium
Celem laboratorium było zapoznanie się z narzędziem Ansible, skonfigurowanie architektury master-worker oraz automatyzacja instalacji pakietów za pomocą playbooków.

## 1. Aktualizacja repozytorium

### 1.1 Aktualizacja metadanych projektu
```bash
git fetch --all
```

### 1.2 Przełączenie na branch main
```bash
git checkout main
```

### 1.3 Pobranie zmian w kodzie
```bash
git pull
```

### 1.4 Utworzenie brancha roboczego
```bash
git checkout -b Lab11/nr_indeksu
```

### 1.5 Utworzenie folderu roboczego
```bash
mkdir env_nrIndeksu
cd env_nrIndeksu
```

## 2. Konfiguracja infrastruktury Docker Compose

### 2.1 Utworzenie pliku docker-compose.yml
Utworzono plik `docker-compose.yml` definiujący 1 mastera i 2 workery:

```yaml
version: '3.8'
services:
  ansible-master:
    image: ubuntu:20.04
    container_name: ansible-master
    networks:
      - ansible-network
    command: sleep infinity
    
  worker1:
    image: ubuntu:20.04
    container_name: worker1
    networks:
      - ansible-network
    command: sleep infinity
    
  worker2:
    image: ubuntu:20.04
    container_name: worker2
    networks:
      - ansible-network
    command: sleep infinity

networks:
  ansible-network:
    driver: bridge
```

### 2.2 Uruchomienie kontenerów
```bash
docker-compose up -d
```

### 2.3 Konfiguracja SSH na masterze
Wejście do kontenera mastera:
```bash
docker exec -it ansible-master bash
```

Aktualizacja systemu i instalacja SSH:
```bash
apt update
apt install -y openssh-server openssh-client
```

Generowanie klucza SSH:
```bash
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -N ""
```

### 2.4 Konfiguracja SSH na workerach
Dla każdego workera:
```bash
docker exec -it worker1 bash
apt update
apt install -y openssh-server
service ssh start
```

Skopiowanie klucza publicznego z mastera do authorized_keys na workerach:
```bash
# Na masterze
cat ~/.ssh/id_rsa.pub

# Na workerach
mkdir -p ~/.ssh
echo "KLUCZ_PUBLICZNY_Z_MASTERA" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

## 3. Weryfikacja połączenia SSH

### 3.1 Test połączenia z worker1
```bash
ssh root@worker1
```

### 3.2 Test połączenia z worker2
```bash
ssh root@worker2
```

![Screenshot połączenia SSH](images/ssh_connection_test.png)

## 4. Instalacja Ansible na masterze

### 4.1 Aktualizacja systemu
```bash
apt update
```

### 4.2 Instalacja Ansible
```bash
apt install -y software-properties-common
add-apt-repository --yes --update ppa:ansible/ansible
apt install -y ansible
```

### 4.3 Weryfikacja instalacji
```bash
ansible --version
```

![Screenshot wersji Ansible](images/ansible_version.png)

## 5. Konfiguracja Ansible i utworzenie playbooka

### 5.1 Konfiguracja pliku hosts
Utworzenie pliku `/etc/ansible/hosts`:

```ini
[WORKERS]
worker1 ansible_host=worker1 ansible_user=root
worker2 ansible_host=worker2 ansible_user=root

[WORKERS:vars]
ansible_ssh_private_key_file=/root/.ssh/id_rsa
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### 5.2 Test połączenia z workerami
```bash
ansible WORKERS -m ping
```

![Screenshot testu ping](images/ansible_ping_test.png)

### 5.3 Utworzenie playbooka
Plik `install_packages.yml`:

```yaml
---
- name: Install packages on workers
  hosts: WORKERS
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
    
    - name: Install nano
      apt:
        name: nano
        state: present
    
    - name: Install net-tools (ifconfig)
      apt:
        name: net-tools
        state: present
    
    - name: Verify ifconfig installation
      command: ifconfig
      register: ifconfig_output
    
    - name: Display ifconfig output
      debug:
        var: ifconfig_output.stdout_lines
```

### 5.4 Wykonanie playbooka
```bash
ansible-playbook install_packages.yml
```

![Screenshot wykonania playbooka](images/playbook_execution.png)

## 6. Weryfikacja instalacji

### 6.1 Test narzędzia nano na worker1
```bash
docker exec -it worker1 bash
nano --version
```

### 6.2 Test narzędzia ifconfig na worker1
```bash
ifconfig
```

![Screenshot weryfikacji ifconfig](images/ifconfig_verification.png)

### 6.3 Test narzędzi na worker2
Analogiczna weryfikacja dla worker2 potwierdza poprawną instalację pakietów.

## 7. Zadanie dodatkowe - Dodanie pliku do workerów

### 7.1 Playbook do kopiowania plików
Plik `copy_file.yml`:

```yaml
---
- name: Copy file to workers
  hosts: WORKERS
  tasks:
    - name: Create test file
      copy:
        content: "Test file created by Ansible\nDate: {{ ansible_date_time.date }}"
        dest: /tmp/ansible_test.txt
        mode: '0644'
    
    - name: Verify file creation
      stat:
        path: /tmp/ansible_test.txt
      register: file_stat
    
    - name: Display file status
      debug:
        msg: "File exists: {{ file_stat.stat.exists }}"
```

### 7.2 Wykonanie playbooka kopiującego
```bash
ansible-playbook copy_file.yml
```

## 8. Podsumowanie

Laboratorium zostało pomyślnie zakończone. Udało się:

1. Skonfigurować środowisko Docker z masterem i workerami
2. Ustanowić komunikację SSH między kontenerami
3. Zainstalować i skonfigurować Ansible
4. Utworzyć i wykonać playbooki automatyzujące instalację pakietów
5. Zweryfikować poprawność działania zainstalowanych narzędzi
6. Wykonać zadanie dodatkowe z kopiowaniem plików

### Wnioski
Ansible okazuje się być potężnym narzędziem do automatyzacji konfiguracji i zarządzania infrastrukturą. Dzięki playbookom można w prosty sposób automatyzować powtarzalne zadania administracyjne na wielu serwerach jednocześnie.

### Struktura katalogów projektu
```
env_nrIndeksu/
├── docker-compose.yml
├── install_packages.yml
├── copy_file.yml
└── images/
    ├── ssh_connection_test.png
    ├── ansible_version.png
    ├── ansible_ping_test.png
    ├── playbook_execution.png
    └── ifconfig_verification.png
```
