# **Ansible 다중 서버 프로젝트 배포 자동화**

“Ansible provides open-source automation that reduces complexity and runs everywhere. Using Ansible lets you automate virtually any task."

Ansible 공식 문서에 있는 소개 내용이다. Ansible은 거의 모든 작업을 자동화할 수 있다고 소개하고 있다. 오늘은 Ansible 에 대해서 다뤄보려고 한다.

담당 업무 특성상 많은 프로젝트를 개발하고 있다. 그리고 프로젝트 성능을 위해서 여러개의 노드를 설정해야할 수 도 있다. 늘어난 프로젝트와 노드를 한번에 관리할 수 있는 솔루션이 필요했다. 현재 많은 양의 노드를 사용하는 프로젝트를 다루진 않지만…

서버 노드가 적으면 상관이 없는데 10대가 넘어간다고 생각하면 아찔하다. 업데이트가 있을 때마다 매번 서버에 접속해서 동일한 배포를 진행해야 하고 실수가 발생할 수도 있다. 다중 서버를 자동 배포할 수 있는 솔루션이 필요했고, ansible 을 사용하게 되었다.

### **Ansible 이란?**

Ansible은 오픈소스 기반의 IT 자동화 툴로, **서버 관리, 애플리케이션 배포, 설정 관리** 등을 효율적으로 처리다. 복잡하지 않고 모든 환경에서 동작하며, 간결하고 유연성을 갖춘 솔루션이다.

### **Ansible 의 설계 철학**

Ansible은 아래와 같은 설계 철학을 가지고 있다. 어떤 솔루션이든 왜 설계 되었는지는 중요하다.

1. **단순함 (Simplicity)**
2. **에이전트리스 아키텍처 (Agent-less Architecture)**
3. **예측 가능성과 불변성 (Idempotence)**
4. **확장성과 호환성 (Scalability and Compatibility)**

### Ansible 을 사용하는 이유

**효율성 향상**  
다양한 작업을 효율적으로 관리할 수 있다. 반복적인 작업을 자동화해 시간을 절약할 수 있고 다중 서버를 동시에 작업할 수 있어서 서버 수가 늘어나게 되도 효율적으로 관리할 수 있다.

**일관성 보장**  
코드로 관리하기 때문에 일관성을 보장한다. 서버 설정을 코드로 정의하기 때문에 변경 내역을 추적할 수 있다.

**사용 편의성**  
Ansible은 SSH를 사용해서 별도의 에이전트가 필요 없어 간단하게 사용할 수 있고 설정 파일이 YAML 형식으로 되어 있어 가독성이 좋다.

**확장성과 유연성**  
Ansible은 다양한 모듈을 지원해 확장성과 유연성이 있다. 서버 관리, 애플리케이션 배포, 네트워크 장비 설정 등 다양한 작업을 수행할 수 있는 모듈을 제공한다. 그리고 원하는 대로 작업의 흐름을 정의할 수 있다.

## **Ansible** 시작하기

Ansible이 어떤 솔루션인지 알았으니 자동화 프로젝트를 만드어 인벤토리를 구축하고 Ansible을 시작해보려고 한다.

### ansible 설치

1. Ansible 설치한다.

```bash
pip install ansible
```

2. Ansible을 구성할 디렉토리를 생성한다.

```bash
mkdir ansible_quickstart && cd ansible_quickstart
```

### 인벤토리 구축

인벤토리를 통해 단순한 명령어로 많은 수의 노드를 관리할 수 있다.

테스트를 진행하려면 하나 이상의 호스트 IP가 필요하다. 그리고 각 호스트에 authorized_keys 파일에 사용자의 공개 ssh-key가 추가 되어 있어야 한다.

1. ansible_quickstart 디렉토리에 inventory.ini 파일을 생성한다.

```bash
touch inventory.ini
```

2. 새로운 그룹 [myhosts] 을 추가하고 IP 주소 혹은 정규화된 도메인 이름을 지정한다. 프로젝트 명을 group으로 지정했다.

```
[group_name]
10.10.10.101
10.10.10.102
```

3. inventory.ini 정상적으로 작동하는지 확인한다.

```bash
ansible-inventory -i inventory.ini --list

{
    "_meta": {
        "hostvars": {
            "10.10.10.101": {
                "ansible_user": "interbus"
            },
            "10.10.10.102": {
                "ansible_user": "interbus"
            }
        }
    },
    "adc": {
        "hosts": [
            "10.10.10.101",
            "10.10.10.102"
        ]
    },
    "all": {
        "children": [
            "ungrouped",
            "group_name"
        ]
    }
}
```

4. 인벤토리에서 adc 그룹에 ping을 실행해본다. 정상적으로 작동하는 것을 확인할 수 있다.

```bash
ansible_quickstart ansible adc -m ping -i inventory.ini

10.10.10.101 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
10.10.10.102 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
```

### Playbook 생성

플레이북은 관리 노드에 배포 및 구성을 수행하기 위해 Ansible이 사용하는 YAML 형식의 자동화 설계도입니다.

1. playbook.yaml 생성한다.

```yaml
- name: My first play
  hosts: adc
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:

   - name: Print message
     ansible.builtin.debug:
       msg: Hello world
```

2. 작성한 playbook을 실행한다.

```bash
ansible-playbook -i inventory.ini playbook.yaml

PLAY [My first play] *************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************
ok: [10.10.10.101]
ok: [10.10.10.102]

TASK [Ping my hosts] *************************************************************************************************************************************************************
ok: [10.10.10.101]
ok: [10.10.10.102]

TASK [Print message] *************************************************************************************************************************************************************
ok: [10.10.10.101] => {
    "msg": "Hello world"
}
ok: [10.10.10.102] => {
    "msg": "Hello world"
}

PLAY RECAP ***********************************************************************************************************************************************************************
10.10.10.101               : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
10.10.10.102               : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Ansible 주요 개념 정리 및 설명

**Control Node (제어 노드):** Ansible 명령을 실행하는 컴퓨터 (예: 노트북, 서버 등).

**Managed Nodes (관리 노드):** Ansible로 관리할 대상 장치 (서버, 네트워크 장비).

**Inventory (인벤토리):** 관리 노드의 목록과 정보를 담은 파일.

**Playbooks (플레이북):** Plays를 포함하는 YAML 형식의 실행 계획서.

**Plays (플레이):** 관리 노드와 태스크를 매핑하고 실행 순서를 정의.

**Roles (역할):** 재사용 가능한 Ansible 콘텐츠의 배포 단위.

**Tasks (태스크):** 관리 노드에서 실행할 단일 작업의 정의.

**Handlers (핸들러):** 상태 변경 시에만 실행되는 특수한 태스크.

**Modules (모듈):** 관리 노드에서 작업을 수행하는 코드 또는 바이너리.

**Plugins (플러그인):** Ansible 기능을 확장하는 코드 (연결, 데이터 처리, 출력 제어 등).

**Collections (컬렉션):** 플레이북, 모듈, 플러그인 등을 포함하는 Ansible 콘텐츠 패키지.

## 마무리

Ansible은 효율성과 일관성을 갖춘 자동화 도구이고, 다중 서버 관리와 복잡한 배포 작업을 단순화하며 실수를 줄일 수 있는 솔루션이다. 엔지니어의 역할은 개발만 하는 것이 아니라, 더 효율적으로 일하고 그러한 솔루션을 제공하는 것이라고 생각한다.