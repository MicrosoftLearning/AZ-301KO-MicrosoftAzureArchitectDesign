---
lab:
    title: 'Azure에 관리되는 컨테이너화된 워크로드 배포'
    module: '모듈 2: Azure에서 관리되는 서버 응용 프로그램 만들기'
---

# Azure에서 관리되는 서버 응용 프로그램 만들기

# 랩 답변 키: Azure에 관리되는 컨테이너화된 워크로드 배포

## 시작하기 전

1. 다음 자격 증명을 사용하여 Windows 10 랩 가상 머신에 로그인되어 있는지 확인합니다.

    - 사용자 이름: **Admin**

    - 암호: **Pa55w.rd**

1. Windows 10 바탕 화면 하단에 있는 작업 표시줄을 검토합니다. 작업 표시줄에는 랩에서 사용할 일반적인 응용 프로그램에 대한 아이콘이 포함되어 있습니다.

    - Microsoft Edge

    - 파일 탐색기

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **참고**: **시작 메뉴**에서 이러한 응용 프로그램의 바로 가기를 찾을 수 있습니다.

## 연습 1: AKS(Azure Kubernetes Service) 클러스터 만들기

#### 작업 1: Azure Portal 열기

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 브라우저 창이 열리면 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정으로 인증합니다.

#### 작업 2: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

    > **참고**: **Cloud Shell** 아이콘은 *보다 큼* 문자와 *밑줄* 문자의 조합으로 구성된 기호입니다.

1. I구독을 사용하여 **Cloud Shell**을 처음 연 경우라면 처음 사용할 수 있도록 **Cloud Shell**을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell 시작** 창에서 **Bash(Linux)**를 클릭합니다.

    > **참고**: **Cloud Shell**에 대한 구성 옵션이 표시되지 않으면 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다. 이 경우 다음 작업을 바로 진행합니다. 

1. **탑재된 스토리지가 없음** 창에서 **고급 설정 표시**를 클릭하고 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값으로 설정된 채로 둡니다.

    - **Cloud Shell 지역** 드롭다운 목록에서 이 랩에서 리소스를 배포하려는 위치와 일치하거나 가까운 Azure 지역을 선택합니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택한 후 텍스트 상자에 **AADesignLab0401-RG**를 입력합니다.

    -  **스토리지 계정** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 3~24자 길이의 문자와 숫자 조합으로 구성된 고유한 이름을 입력합니다. 

    - **파일 공유** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell**을 입력합니다.

    - **스토리지 만들기** 단추를 클릭합니다.

1. 다음 작업을 진행하기 전에 **Cloud Shell**이 첫 번째 설치 절차를 완료할 때까지 기다립니다.

#### 작업 3: Cloud Shell을 사용하여 AKS 클러스터 만들기

1. 포털 하단에 있는 **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 작업에 사용할 리소스 그룹의 이름을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP='AADesignLab0402-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 Azure 지역을 지정하는 값에 대한 변수를 만듭니다(자리 표시자 `<Azure region>` 을 이 랩에서 리소스를 배포하려는 Azure 지역의 이름으로 바꿈).

    ```sh
    LOCATION='<Azure region>'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 리소스 그룹을 만듭니다.

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 AKS 클러스터를 만듭니다.

    ```sh
    az aks create --resource-group $RESOURCE_GROUP --name aad0402-akscluster --node-count 1 --node-vm-size Standard_DS1_v2 --generate-ssh-keys
    ```

    > **참고**: 값이 `--node-vm-size` 매개 변수로 표시되는 VM 크기의 가용성에 관한 오류 메시지가 나타나면 메시지를 검토하고 다른 제안된 VM 크기를 사용해 보십시오.

    > **참고**: 또는 **Cloud Shell**의 **PowerShell**에서 다음 명령을 실행하고 **Restriction** 열의 값을 검토하여 지정된 지역에서 구독의 사용 가능한 VM 크기를 확인할 수 있습니다(`region` 자리 표시자를 대상 지역의 이름으로 바꾸어야 함).

    ```pwsh
    Get-AzComputeResourceSku | where {$_.Locations -icontains "region"} | Where-Object {($_.ResourceType -ilike "virtualMachines")}
    ```

    > **참고**: 구독에서 사용할 수 없는 VM 크기인 경우 **Restriction** 열에 **NotAvailableForSubscription** 값이 포함됩니다.

    > **참고**: 2019년 2월 21일 기준으로, **westeurope**에서 VM 크기 **Standard_DS2_V2**를 사용할 수 있었습니다. 


1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 이 작업은 최대 10분이 걸릴 수 있습니다.


#### 작업 4: AKS 클러스터 연결

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 AKS 클러스터에 액세스하기 위한 자격 증명을 검색합니다.

    ```sh
    az aks get-credentials --resource-group $RESOURCE_GROUP --name aad0402-akscluster 
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 AKS 클러스터에 대한 연결을 확인합니다.

    ```
    kubectl get nodes
    ```

1. **Cloud Shell** 명령 프롬프트에서 출력을 검토하여 노드가 **Ready** 상태를 보고하고 있는지 확인합니다. 올바른 상태가 표시될 때까지 명령을 반복해서 실행합니다.
  
> **결과**: 이 연습을 완료하면 새 AKS 클러스터가 배포되어 있어야 합니다. 


## 연습 2: AKS 클러스터 및 해당 컨테이너화된 워크로드 관리

#### 작업 1: 컨테이너화된 응용 프로그램을 AKS 클러스터에 배포 

1. Microsoft Edge 창에 있는 Azure Portal의 **Cloud Shell** 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Docker Hub에서 **nginx** 이미지를 배포합니다.

    ```
    kubectl run aad0402-akscluster --image=nginx --replicas=1 --port=80
    ```

    > **참고**: 배포 이름을 입력할 때 소문자를 사용해야 합니다. 또한 이 명령이 더 이상 사용되지 않으며 이후 버전에서 제거되지만 클러스터를 성공적으로 만들었다는 알림을 받게 됩니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Kubernetes Pod가 만들어졌는지 확인합니다.

    ```
    kubectl get pods
    ```

1. A**Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포 상태를 확인합니다.

    ```
    kubectl get deployment
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 인터넷에서 Pod를 사용할 수 있도록 합니다.

    ```
    kubectl expose deployment aad0402-akscluster --port=80 --type=LoadBalancer
    ```

    > **참고**: 배포 이름을 입력할 때 소문자를 사용해야 합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 공용 IP 주소가 프로비전되었는지 여부를 확인합니다.

    ```
    kubectl get service --watch
    ```

1. **aad0402-akscluster** 항목에 대한 **EXTERNAL-IP** 열의 값이 **<pending>**에서 공용 IP 주소로 변경될 때까지 기다린 후 **Ctrl-C** 키 조합을 누릅니다. **aad0402-akscluster**에 대한 **EXTERNAL-IP** 열의 공용 IP 주소를 기록합니다. 

1. Microsoft Edge를 시작하고 이전 단계에서 얻은 IP 주소로 이동합니다. Microsoft Edge에 **Welcome to nginx!** 메시지가 포함된 웹 페이지가 표시되는지 확인합니다. 


#### 작업 2: 컨테이너화된 응용 프로그램 및 AKS 클러스터 노드 크기 조정 

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포 크기를 조정합니다.

    ```
    kubectl scale --replicas=2 deployment/aad0402-akscluster
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포 크기를 조정한 결과를 확인합니다.

    ```
    kubectl get pods
    ```

    > **참고**: 명령의 출력을 검토하여 Pod 수가 2로 증가했는지 확인합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 클러스터 노드 수를 확장합니다.

    ```sh
    az aks scale --resource-group $RESOURCE_GROUP --name aad0402-akscluster --node-count 2
    ```

1. 추가 노드의 프로비전이 완료될 때까지 기다립니다.

    > **참고**: 이 작업은 최대 10분이 걸릴 수 있습니다. 실패한 경우 `az aks scale` 명령을 다시 실행합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 클러스터를 확장한 결과를 확인합니다.

    ```
    kubectl get nodes
    ```

    > **참고**: 명령의 출력을 검토하여 노드 수가 2로 증가했는지 확인합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포 크기를 조정합니다.

    ```
    kubectl scale --replicas=10 deployment/aad0402-akscluster
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포 크기를 조정한 결과를 확인합니다.

    ```
    kubectl get pods
    ```

    > **참고**: 명령의 출력을 검토하여 Pod 수가 10으로 증가했는지 확인합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 전체 클러스터 노드의 Pod 분포를 검토합니다. 

    ```
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **참고**: 명령의 출력을 검토하여 두 노드에 Pod가 분산되어 있는지 확인합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포를 삭제합니다.

    ```
    kubectl delete deployment aad0402-akscluster 
    ```

## 연습 3: AKS 클러스터의 Pod 자동 크기 조정

#### 작업 1: **yaml** 파일을 사용하여 Kubernetes Pod 배포

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 컨테이너화된 샘플 응용 프로그램을 다운로드합니다.

    ```
    git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 다운로드한 앱이 있는 위치로 이동합니다.

    ```sh
    cd azure-voting-app-redis
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 응용 프로그램 **yaml** 파일의 내용을 나열합니다.

    ```sh
    cat azure-vote-all-in-one-redis.yaml
    ```

1. 명령의 출력을 검토하여 Pod 정의에 다음과 같은 형식으로 requests 및 limits가 포함되어 있는지 확인합니다.

    ```yaml
    resources:
      requests:
         cpu: 250m
      limits:
         cpu: 500m
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **yaml** 파일을 기반으로 응용 프로그램을 배포합니다.

    ```
    kubectl apply -f azure-vote-all-in-one-redis.yaml
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Kubernetes Pod가 만들어졌는지 확인합니다.

    ```
    kubectl get pods
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 컨테이너화된 응용 프로그램의 공용 IP 주소가 프로비전되었는지 여부를 확인합니다.

    ```
    kubectl get service azure-vote-front --watch
    ```

1. **azure-vote-front** 항목에 대한 **EXTERNAL-IP** 열의 값이 **<pending>**에서 공용 IP 주소로 변경될 때까지 기다린 후 **Ctrl-C** 키 조합을 누릅니다. **azure-vote-front**에 대한 **EXTERNAL-IP** 열의 공용 IP 주소를 기록합니다. 

1. Microsoft Edge를 시작하고 이전 단계에서 얻은 IP 주소로 이동합니다. Microsoft Edge에 **Azure Voting App** 메시지가 포함된 웹 페이지가 표시되는지 확인합니다. 

#### 작업 2: Kubernetes Pod 자동 크기 조정

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 컨테이너화된 샘플 응용 프로그램을 다운로드합니다.

    ```
    git clone https://github.com/kubernetes-incubator/metrics-server.git
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **Metrics Server**를 설치합니다.

    ```
    kubectl create -f ~/metrics-server/deploy/1.8+/
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **azure-vote-front** 배포에 대한 자동 크기 조정을 구성합니다.

    ```
    kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 자동 크기 조정의 상태를 표시합니다.

    ```
    kubectl get hpa
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Pod를 표시합니다.

    ```
    kubectl get pods
    ```

    > **참고**: 복제본 수가 3개로 증가되었는지 확인합니다. 그렇지 않은 경우 1분 정도 기다렸다가 이전 두 단계를 다시 실행합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포를 삭제합니다.

    ```
    kubectl delete deployment azure-vote-front 
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포를 삭제합니다.

    ```
    kubectl delete deployment azure-vote-back 
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이전 단계에서 실행한 명령이 성공적으로 완료되었는지 확인합니다.

    ```
    kubectl get pods
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 AKS 클러스터를 삭제합니다.

    ```
    az aks delete --resource-group AADesignLab0402-RG --name aad0402-akscluster --yes --no-wait
    ```

1. **Cloud Shell** 창을 닫습니다.

> **복습**: 이 연습에서는 AKS 클러스터에서 Pod의 자동 크기 조정을 구현했습니다.


## 연습 4: DevOps with AKS 구현

#### 작업 1: DevOps with AKS 배포

    > **참고**: 이 솔루션은 https://docs.microsoft.com/en-us/azure/architecture/example-scenario/apps/devops-with-aks 페이지에 설명된 DevOps with Containers 솔루션에 기반합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Jenkins 인스턴스 및 Grafana 콘솔을 실행하는 Linux VM에 액세스할 때 인증하는 데 사용될 SSH 키 쌍을 생성합니다.

    ```
    ssh-keygen -t rsa -b 2048
    ```

    - 키를 저장할 파일을 입력하라는 메시지가 표시되면 **Enter** 키를 눌러 기본값**(~/.ssh/id_rsa)**을 적용합니다.

    - 암호를 입력하라는 메시지가 표시되면 **Enter** 키를 두 번 누릅니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새로 생성된 키 쌍의 공개 키를 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    PUBLIC_KEY=$(cat ~/.ssh/id_rsa.pub)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새로 생성된 키 쌍의 공개 키를 지정하는 값을 포함하고 공개 키에 포함될 수 있는 특수 문자를 고려하는 변수를 만듭니다.

    ```sh
    PUBLIC_KEY_REGEX="$(echo $PUBLIC_KEY | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
    ```

    > **참고**: 이 변수는 **sed** 유틸리티를 사용하여 이 문자열을 Azure Resource Manager 템플릿 매개 변수 파일에 삽입하기 위해 필요합니다. 또는 파일을 열고 공개 키 문자열을 파일에 직접 입력할 수 있습니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 리소스 그룹의 이름을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP='AADesignLab0403-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 Azure 지역을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0402-RG'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 리소스 그룹을 만듭니다.

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 샘플 솔루션에 포함된 서비스 및 리소스의 인증을 위한 Azure Active Directory 서비스 주체를 만듭니다.

    ```sh
    SERVICE_PRINCIPAL=$(az ad sp create-for-rbac --name AADesignLab0403-SP)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새로 만든 서비스 주체의 **appId** 특성을 검색합니다.

    ```sh
    APP_ID=$(echo $SERVICE_PRINCIPAL | jq -r .appId)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새로 만든 서비스 주체의 **password** 특성을 검색합니다.

    ```sh
    PASSWORD=$(echo $SERVICE_PRINCIPAL | jq -r .password)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 샘플 솔루션 배포에 사용할 매개 변수 파일을 만들고 vi 인터페이스에서 엽니다.

    ```sh
    vi ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 샘플 매개 변수 파일(**\\allfiles\\AZ-301T03\\Module_02\\Labfiles\\Starter\\parameters.json**)의 내용을 추가합니다.

    ```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "spClientId": {
          "value": "$APP_ID"
        },
        "spClientSecret": {		
          "value": "$PASSWORD"
        },
        "linuxAdminUsername": {
          "value": "Student"
        },
        "linuxAdminPassword": {		
          "value": "Pa55w.rd1234"
        },
        "linuxSSHPublicKey": {		
          "value": "$PUBLIC_KEY_REGEX"
        }
      }
    }
    ```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 변경 내용을 저장하고 파일을 닫기 위해 **Esc** 키를 누르고, **:**을 누르고, **wq!**를 입력한 후 **Enter** 키를 누릅니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **appId** 특성의 자리 표시자를 매개 변수 파일의 **$APP_ID** 변수 값으로 바꿉니다.

    ```sh
    sed -i.bak1 's/"$APP_ID"/"'"$APP_ID"'"/' ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **password** 특성의 자리 표시자를 매개 변수 파일의 **$PASSWORD** 변수 값으로 바꿉니다.

    ```sh
    sed -i.bak2 's/"$PASSWORD"/"'"$PASSWORD"'"/' ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **sshPublicKey** 매개 변수의 자리 표시자를 매개 변수 파일의 **$PUBLIC_KEY_REGEX** 변수 값으로 바꿉니다.

    ```sh
    sed -i.bak3 's/"$PUBLIC_KEY_REGEX"/"'"$PUBLIC_KEY_REGEX"'"/' ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 매개 변수 파일에서 자리 표시자가 성공적으로 바뀌었는지 확인합니다.

    ```sh
    cat ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 GitHub 리포지토리에 있는 Azure Resource Manager 템플릿을 사용하여 샘플 솔루션을 배포합니다.

    ```sh
    az group deployment create --resource-group $RESOURCE_GROUP --template-uri https://raw.githubusercontent.com/cjpluta/AZ-301-MicrosoftAzureArchitectDesign/master/allfiles/AZ-301T03/Module_02/LabFiles/Starter/azuredeploy.json --parameters @parameters.json
    ```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 배포에는 최대 15분이 걸릴 수 있습니다.


#### 작업 2: DevOps with AKS 아키텍처 검토

1. Azure Portal의 허브 메뉴에서 **리소스 그룹**을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0403-RG** 리소스 그룹을 나타내는 항목을 클릭합니다.

1. **AADesignLab0403-RG** 리소스 그룹 블레이드에서 리소스 목록을 검토하고 https://docs.microsoft.com/en-us/azure/architecture/example-scenario/apps/devops-with-aks 페이지에서 사용할 수 있는 정보와 비교합니다.

> **복습**: 이 연습에서는 Azure Building Blocks를 사용하여 Cloud Shell에서 Windows Server 2016 Datacenter 및 Linux를 실행하는 Azure VM을 배포했습니다.


## 연습 5: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. 포털 하단에 있는 **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab04')]".name --output tsv
    ```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab04')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.


> **복습**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
