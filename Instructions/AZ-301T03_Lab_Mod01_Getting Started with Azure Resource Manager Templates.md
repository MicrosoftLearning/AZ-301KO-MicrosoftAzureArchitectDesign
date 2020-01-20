---
lab:
    title: 'Azure Resource Manager 템플릿 및 Azure Building Blocks 작업 시작'
    module: '모듈 1: Azure Resource Manager를 사용하여 리소스 배포'
---

# Azure Resource Manager를 사용하여 리소스 배포
# 랩 답변 키: Azure Resource Manager 템플릿 및 Azure Building Blocks 작업 시작

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


## 연습 1: Azure Portal에서 Azure Resource Manager 템플릿을 사용하여 핵심 Azure 리소스 배포

#### 작업 1: Azure Portal 열기

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 브라우저 창이 열리면 **Azure Portal**(<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정으로 인증합니다.

#### 작업 2: Azure Resource Manager 템플릿을 사용하여 Azure Portal에서 Azure 가상 네트워크 배포

1. Azure Portal의 왼쪽 상단에서 **리소스 만들기**를 클릭합니다.
s
1. **새로 만들기** 블레이드 상단에 있는 **마켓플레이스 검색** 텍스트 상자에 **템플릿 배포**를 입력하고 **Enter** 키를 누릅니다.

1. **모두** 블레이드의 검색 결과에서 **템플릿 배포**를 클릭합니다.

1. **템플릿 배포** 블레이드에서 **만들기** 단추를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다.

1. **템플릿 편집** 블레이드에서 **파일 로드**를 클릭합니다.

1. **업로드할 파일 선택** 대화 상자에서 **\\allfiles\\AZ-301T03\\Module_01\\Labfiles\\Starter\\** 폴더로 이동하고 **vnet-simple-template.json** 파일을 선택한 다음 **열기**를 클릭합니다. 이렇게 하면 다음 콘텐츠가 템플릿 편집기 창에 로드됩니다.

```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vnetNamePrefix": {
                "type": "string",
                "defaultValue": "vnet-",
                "metadata": {
                    "description": "Name prefix of the vnet"
                 }
            },
            "vnetIPPrefix": {
                "type": "string",
                "defaultValue": "10.2.0.0/16",
                "metadata": {
                    "description": "IP address prefix of the vnet"
                }
            },
            "subnetNamePrefix": {
                "type": "string",
                "defaultValue": "subnet-",
                "metadata": {
                    "description": "Name prefix of the subnets"
                }
            },
            "subnetIPPrefix": {
                "type": "string",
                "defaultValue": "10.2.0.0/24",
                "metadata": {
                    "description": "IP address prefix of the first subnet"
                }
            }
        },
        "variables": {
            "vnetName": "[concat(parameters('vnetNamePrefix'), resourceGroup().name)]",
            "subnetNameSuffix": "0"
        },
        "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[variables('vnetName')]",
            "type": "Microsoft.Network/virtualNetworks",
            "location": "[resourceGroup().location]",
            "scale": null,
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[parameters('vnetIPPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[concat(parameters('subnetNamePrefix'), variables('subnetNameSuffix'))]",
                        "properties": {
                        "addressPrefix": "[parameters('subnetIPPrefix')]"
                        }
                    }
                ],
                "virtualNetworkPeerings": [],
                "enableDdosProtection": false,
                "enableVmProtection": false
            },
            "dependsOn": []
            }
        ]
    }
```

1. **저장** 단추를 클릭하여 템플릿을 유지합니다.

1. **사용자 지정 배포** 블레이드로 돌아가서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값으로 설정된 채로 둡니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택하고 텍스트 상자에 **AADesignLab0201-RG**를 입력합니다.

    - **위치** 드롭다운 목록에서 이 랩의 리소스를 배포할 Azure 지역을 선택합니다.

    - **vnetNamePrefix** 텍스트 상자를 기본값으로 설정된 채로 둡니다.

    - **vnetIPPrefix** 텍스트 상자를 기본값으로 설정된 채로 둡니다.

    - **subnetNamePrefix** 텍스트 상자를 기본값으로 설정된 채로 둡니다.

    - **subnetIPPrefix** 텍스트 상자를 기본값으로 설정된 채로 둡니다.

    - **사용 약관** 섹션에서 **위에 명시된 사용 약관에 동의함** 확인란을 선택합니다.

    - **구매** 단추를 클릭합니다.

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.


#### 작업 3: 배포 메타데이터 보기

1. Azure Portal의 허브 메뉴에서 **리소스 그룹**을 클릭합니다.

1. **리소스 그룹** 블레이드에서 이전 작업에서 템플릿을 배포한 리소스 그룹을 나타내는 항목을 클릭합니다.

1. **개요** 선택 항목이 활성화되어 있으면 리소스 그룹 블레이드에서 **배포** 링크를 클릭합니다. 

1. 결과 블레이드에서 최신 배포를 클릭하여 해당 메타데이터를 새 블레이드에 표시합니다.

1. 배포 블레이드 내에서 **작업 정보** 섹션에 표시된 정보를 관찰합니다.

> **복습**: 이 연습에서는 Azure Portal에서 Azure Resource Manager 템플릿을 사용하여 Azure 가상 네트워크를 배포했습니다.



## 연습 2: Azure Cloud Shell에서 Azure Building Blocks를 사용하여 핵심 Azure 리소스 배포

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

    > **참고**: **Cloud Shell** 아이콘은 *보다 큼* 문자와 *밑줄* 문자의 조합으로 구성된 기호입니다.

1. 구독을 사용하여 **Cloud Shell**을 처음 연 경우라면 처음 사용할 수 있도록 **Cloud Shell**을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell 시작** 창에서 **Bash(Linux)**를 클릭합니다.

    > **참고**: **Cloud Shell**에 대한 구성 옵션이 표시되지 않으면 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다. 이 경우 다음 작업을 바로 진행합니다. 

1. **탑재된 스토리지가 없음** 창에서 **고급 설정 표시**를 클릭하고 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값으로 설정된 채로 둡니다.

    - **Cloud Shell 지역** 드롭다운 목록에서 이 랩에서 리소스를 배포한 위치와 일치하거나 가까운 Azure 지역을 선택합니다.

    - 리소스 그룹: **기존 항목 사용** 옵션이 선택되어 있는지 확인하고 **AADesignLab0201-RG** 를 선택합니다.

    - **스토리지 계정** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 3~24자 길이의 문자와 숫자 조합으로 구성된 고유한 이름을 입력합니다. 

    - **파일 공유** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell**을 입력합니다.

    - **스토리지 만들기** 단추를 클릭합니다.

1. 다음 작업을 계속하기 전에 **Cloud Shell**이 첫 번째 설치 절차를 완료할 때까지 기다립니다.


#### 작업 2: Azure Cloud Shell에서 Azure Building Blocks npm 패키지 설치

1. 포털 하단에 있는 **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Azure Building Blocks npm 패키지를 설치하기 위한 로컬 디렉터리를 만듭니다.

```sh
    mkdir ~/.npm-global
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 로컬 디렉터리를 포함하도록 npm 구성을 업데이트합니다.

```sh
    npm config set prefix '~/.npm-global'
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 ~./bashrc 구성 파일을 편집할 수 있도록 엽니다.

```sh
    vi ~/.bashrc
```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 파일 맨 아래로 스크롤하고(또는 **G**를 입력하고), 마지막 줄의 맨 오른쪽 문자 다음으로 스크롤하고(또는 **$**를 입력하고), **a**를 입력하여 **INSERT** 모드로 진입하고, **Enter** 키를 눌러 새 줄을 시작한 후 다음을 입력하여 새로 만든 디렉터리를 시스템 경로에 추가합니다.

```sh
    export PATH="$HOME/.npm-global/bin:$PATH"
```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 변경 내용을 저장하고 파일을 닫기 위해 **Esc** 키를 누르고, **:**을 누르고, **wq!**를 입력한 후 **Enter** 키를 누릅니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Azure Building Blocks npm 패키지를 설치합니다.

```sh
    npm install -g @mspnp/azure-building-blocks
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 셸을 종료합니다.

```sh
    exit
```

1. **Cloud Shell 시간 초과됨** 창에서 **다시 연결**을 클릭합니다. 

    > **참고**: Buliding Blocks npm 패키지 설치를 적용하려면 Cloud Shell을 다시 시작해야 합니다.


#### 작업 3: Azure Building Blocks를 사용하여 Cloud Shell에서 Azure 가상 네트워크 배포

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Azure Building Blocks 템플릿이 포함된 GitHub 리포지토리를 다운로드합니다.

```sh
    git clone https://github.com/mspnp/template-building-blocks.git
```

1.  **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 배포에 사용할 Azure Building Block 매개 변수 파일의 내용을 표시합니다.

```sh
    cat ./template-building-blocks/scenarios/vnet/vnet-simple.json 
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Azure 구독의 이름을 지정하는 값에 대한 변수를 만듭니다.

```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 연습의 앞부분에서 만든 리소스 그룹의 이름을 지정하는 값에 대한 변수를 만듭니다.

```sh
    RESOURCE_GROUP='AADesignLab0202-RG'
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 Azure 지역을 지정하는 값에 대한 변수를 만듭니다.

```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0201-RG'].location" --output tsv)
```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **AADesignLab0202-RG** 리소스 그룹을 만듭니다.

```sh
    az group create --location $LOCATION --name $RESOURCE_GROUP
```
1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Azure Building Blocks를 사용하여 가상 네트워크를 배포합니다.

```sh
    azbb -g $RESOURCE_GROUP -s $SUBSCRIPTION_ID -l $LOCATION -p ./template-building-blocks/scenarios/vnet/vnet-simple.json --deploy
```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.


#### 작업 4: 배포 메타데이터 보기

1. 포털 왼쪽에서 **리소스 그룹** 링크를 클릭합니다.

1. **리소스 그룹** 블레이드에서 이 연습의 앞부분에서 만든 리소스 그룹을 나타내는 항목을 클릭합니다.

1. **개요** 선택 항목이 활성화되어 있으면 리소스 그룹 블레이드에서 **배포** 링크를 클릭합니다. 

1. 결과 블레이드에서 최신 배포를 클릭하여 해당 메타데이터를 새 블레이드에 표시합니다.

1. 배포 블레이드 내에서 **작업 정보** 섹션에 표시된 정보를 관찰합니다.

1. **Cloud Shell** 창을 닫습니다.

> **복습**: 이 연습에서는 Cloud Shell에서 Azure Building Blocks 템플릿을 사용하여 Azure 가상 네트워크를 배포했습니다.


## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. 포털 하단에 있는 **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.
```sh
    az group list --query "[?starts_with(name,'AADesignLab02')]".name --output tsv
```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제
1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

```sh
    az group list --query "[?starts_with(name,'AADesignLab02')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.


> **복습**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
