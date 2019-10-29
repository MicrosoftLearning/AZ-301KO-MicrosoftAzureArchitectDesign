---
lab:
    title: '구성 관리 솔루션을 Azure에 배포'
    module: '모듈 3: Azure 솔루션 모니터링 및 자동화'
---

# Azure 솔루션 모니터링 및 자동화

# 랩 답변 키: 구성 관리 솔루션을 Azure에 배포

## 시작하기 전에

1. 다음 자격 증명을 사용하여 Windows 10 랩 가상 머신에 로그인되어 있는지 확인합니다.

    - 사용자 이름: **Admin**

    - 암호: **Pa55w.rd**

1. Windows 10 데스크톱 하단에 있는 작업 표시줄을 검토합니다. 작업 표시줄에는 랩에서 사용할 일반적인 응용 프로그램에 대한 아이콘이 포함되어 있습니다.

    - Microsoft Edge

    - File Explorer

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **참고**: 또한 **시작 메뉴** 에서 이러한 응용 프로그램에 대한 바로 가기를 찾을 수 있습니다.

## 연습 1: 계산 리소스를 배포합니다.

#### 작업 1: Azure 포털 열기

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열린 브라우저 창에서 **Azure Portal**(<https://portal.azure.com>)으로  이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정 으로 인증합니다.

#### 작업 2: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 shell 인스턴스를 엽니다. 

    > **참고**:  **Cloud Shell** 아이콘은 *보다 큰* 문자와 *밑줄* 문자의 조합으로 구성된 기호입니다. 

1. 구독을 사용하여 **Cloud Shell** 을 처음 여는 경우 처음 사용을 위해 **Cloud Shell** 을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell 시작** 창에서 **Bash(Linux)** 를 클릭합니다. 

    > **참고**: **Cloud Shell** 에 대한 구성 옵션이 표시되지 않으면 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다.  그렇다면 다음 작업으로 직접 넘어갑니다. 

1.  **마운트된 저장소 없음** 창에서 **고급 설정 표시** 를 클릭하고 다음 작업을 수행합니다.   

    - **구독** 드롭다운 목록 항목을 기본값으로 설정합니다. 

    - **Cloud Shell 지역** 드롭다운 목록에서 이 랩에서 리소스를 배포할 위치와 일치하는 또는   가까운 Azure 지역을 선택합니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택한 다음 텍스트 상자에 **AADesignLab1201-RG** 를 입력합니다.   

    - **저장소 계정** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 3~24자 및 숫자로 구성된 고유한 이름을 입력합니다.    

    - **파일 공유** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell** 을 입력합니다.   

    - **저장소 만들기** 버튼을 선택합니다.

1. 다음 작업을 진행하기 전에 **Cloud Shell** 이 처음 설치 절차를 완료할  때까지 기다립니다.

#### 작업 3: Linux VM 배포

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 Clould Shell 인스턴스를 엽니다. 

1. **Cloud Shell** 창에서 **파일 업로드/다운로드** 아이콘을 클릭하고 드롭다운 메뉴에서 **업로드** 를 클릭합니다. 

1. **열기** 대화 상자에서 **\\allfiles\\AZ-301T02\\Module_03\\LabFiles\\Starter\\** 폴더로 이동하여 **linux-template.json** 파일을 선택하고 **열기** 를 클릭합니다. 이 화일에는 다음 템플릿이 포함되어 있습니다.

    ''json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "userName": {
                "type": "string",
                "defaultValue": "Student"
            },
            "password": {
                "type": "securestring"
            }
        },
        "variables": {
            "vmName": "[concat('lvm', uniqueString(resourceGroup().id))]",
            "nicName": "[concat('nic', uniqueString(resourceGroup().id))]",
            "publicIPAddressName": "[concat('pip', uniqueString(resourceGroup().id))]",
            "virtualNetworkName": "[concat('vnt', uniqueString(resourceGroup().id))]",
            "subnetName": "Linux",
            "imageReference": {
                "publisher": "suse",
                "offer": "opensuse-leap",
                "sku": "42.3",
                "version": "latest"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-06-01",
                "type": "Microsoft.Network/publicIPAddresses",
                "name": "[variables('publicIPAddressName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "publicIPAllocationMethod": "Dynamic"
                }
            },
            {
                "apiVersion": "2017-06-01",
                "type": "Microsoft.Network/virtualNetworks",
                "name": "[variables('virtualNetworkName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "addressSpace": {
                        "addressPrefixes": [
                            "10.0.0.0/16"
                        ]
                    },
                    "subnets": [
                        {
                            "name": "[variables('subnetName')]",
                            "properties": {
                                "addressPrefix": "10.0.0.0/16"
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2017-06-01",
                "type": "Microsoft.Network/networkInterfaces",
                "name": "[variables('nicName')]",
                "location": "[resourceGroup().location]",
                "dependsOn": [
                    "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                    "[resourceId('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
                ],
                "properties": {
                    "ipConfigurations": [
                        {
                            "name": "ipconfig1",
                            "properties": {
                                "privateIPAllocationMethod": "Dynamic",
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))]"
                                },
                                "subnet": {
                                    "id": "[concat(resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName')), '/subnets/', variables('subnetName'))]"
                                }
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2017-06-01",
                "type": "Microsoft.Compute/virtualMachines",
                "name": "[variables('vmName')]",
                "location": "[resourceGroup().location]",
                "dependsOn": [
                    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
                ],
                "properties": {
                    "hardwareProfile": {
                        "vmSize": "Standard_A1_v2"
                    },
                    "osProfile": {
                        "computerName": "[variables('vmName')]",
                        "adminUsername": "[parameters('username')]",
                        "adminPassword": "[parameters('password')]"
                    },
                    "storageProfile": {
                        "imageReference": "[variables('imageReference')]",
                        "osDisk": {
                            "createOption": "FromImage"
                        }
                    },
                    "networkProfile": {
                        "networkInterfaces": [
                            {
                                "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('nicName'))]"
                            }
                        ]
                    }
                }
            }
        ]
    }
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 허브 가상 네트워크를 포함하는 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP='AADesignLab1202-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다(자리 표시자 `<Azure region>`를 A이 랩에서 리소스를 배포하려는 Azure 영역의 이름으로 바꿉니다.)

    ```sh
    LOCATION='<Azure region>'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 새 리소스 그룹을 만듭니다.   

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 지정된 매개 변수 파일을 사용하여 Azure Resource Manager 템플릿을 배포합니다.

    ```sh
    az group deployment create --resource-group $RESOURCE_GROUP --template-file ~/linux-template.json --parameters password=Pa55w.rd1234
    ```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다리지 않아도 됩니다.

#### 작업 4: Azure Automation 계정 배포

1. Azure 포털의 왼쪽 위 모서리에서 **리소스 만들기** 를 클릭합니다.

1. **새** 블레이드 의 상단에 있는 **마켓플레이스 검색** 텍스트 상자에서 **Automation** 를 입력하고 **Enter** 를 누릅니다.

1. **모두** 블레이드의 검색 결과에서 **Automation** 를 클릭합니다.

1. **Automation** 블레이드에서 **만들기** 를 클릭합니다.

1. **Automation 계정 추가** 블레이드에서 다음 작업을  수행합니다.

    - **이름** 텍스트 상자에 **LinuxAutomation** 을 입력합니다.
    
    - **구독** 드롭다운 목록 항목을 기본값으로 설정합니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택한 다음 텍스트 상자에 **AADesignLab1203-RG** 를 입력합니다.   

    - **위치** 드롭다운 목록에서 이전 작업에서 Azure VM을 배치한 지역과 일치하는 또는 가까운 위치를 선택합니다.

    -  **Azure 실행 계정 만들기** 섹션에서 **예** 옵션이 선택되어 있는지 확인합니다. 

    - **만들기** 버튼을 클릭합니다.

1. 다음 작업을 진행하기 전에 프로비저닝이 완료될 때까지 기다립니다. 

> **복습**: 이 연습에서는 Azure Resource Manager 템플릿을 사용하여 Linux VM을 만들고 Azure Portal에서 Azure Automation 계정을 프로비전했습니다.


## 연습 2: Azure Automation DSC 구성

#### 작업 1: Linux PowerShell DSC 모듈 가져오기

1. Azure 포털의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab1203-RG** 를 클릭합니다. 

1.  **AADesignLab1203-RG** 블레이드에서 새로 만든 Azure Automation 계정을 클릭합니다. 

1.  **LinuxAutomation** 블레이드에서 블레이드 왼쪽의 **공유 리소스** 섹션에서 **모듈 갤러리** 를 클릭합니다.

1.  **LinuxAutomation - 모듈 갤러리** 블레이드에서 다음과 같은 작업을 수행합니다.

    - **검색** 텍스트 상자에서 **nx** 를 입력하고 **Enter** 를 누릅니다.

    - 검색 결과에서 **nx** 모듈을  클릭합니다.

1.  **nx** 블레이드에서 블레이드 상단의 **가져오기** 버튼을  클릭합니다. 

1. **가져오기** 블레이드에서 **확인** 버튼을 클릭합니다.

1. 다음 작업을 진행하기 전에 가져오기 프로세스가 완료될 때까지 기다립니다. **nx 모듈** 블레이드의 상태 메시지는 모듈을 성공적으로 가져왔음을 나타냅니다. 

    > **참고**: 이 프로세스는 약 2분 정도 소요됩니다.

#### 작업 2: Linux DSC 구성 만들기

1.  **LinuxAutomation** 블레이드로 돌아갑니다. 

1.  **LinuxAutomation** 블레이드로 돌아가 **구성 관리** 섹션에서, **상태 구성(DSC)** 를 클릭합니다.     

1.  **LinuxAutomation - 상태 구성(DSC)** 블레이드에서 **구성** 링크를 클릭합니다.

1.  **LinuxAutomation - 상태 구성(DSC)** 블레이드에서 창 상단의 **+ 추가** 버튼을 클릭합니다.

1. **가져오기** 블레이드에서 다음 작업을  수행합니다.

    - **구성 파일** 필드 옆의 폴더 아이콘이 있는 파란색 버튼을 클릭합니다. 

    - **업로드할 파일 선택** 대화 상자에서 **\\allfiles\\AZ-301T02\\Module_02\\LabFiles\\Starter\\** 폴더로 이동합니다. 

    - **lampserver.ps1** 파일을 선택합니다.

    - **열기** 버튼을 클릭하여  대화 상자를 닫고 **가져오기** 블레이드로 돌아갑니다.

    - **이름** 텍스트 상자에서 기본 항목 **lampserver** 를 수락합니다.

    - **설명** 텍스트 상자에서 **PHP 및 MySQL을 사용한 LAMP 서버 구성** 을 입력합니다.

    - **확인** 버튼을 클릭합니다.

1.  **DSC 구성** 창으로 돌아가서 **새로 고침** 을 클릭한 다음 새로 생성된 **lampserver** 구성을 클릭합니다.

1. **lampserver 구성** 블레이드에서 블레이드 상단의 **컴파일** 버튼을 클릭합니다.    확인 대화 상자에서 **예** 를 클릭하여 구성 컴파일을 진행합니다. 

1. 컴파일 작업이 완료될 때까지 기다립니다. 컴파일 작업의 상태를 확인하려면 **lampserver 구성** 블레이드의 **컴파일 작업** 섹션의 **STATUS** 열을 검토합니다.

    > **참고**: 최신 컴파일 상태를 보려면 블레이드를 닫고 다시 열어야 할 수 있습니다. 이 블레이드는 자동으로 새로 고쳐지지 않습니다.

#### 작업 3: 온보드 Linux VM

1.  **LinuxAutomation - 상태 구성(DSC)** 블레이드로  돌아갑니다.

1.  **LinuxAutomation - 상태 구성 (DSC)** 블레이드로 돌아가서 **노드** 링크를 클릭합니다.

1. **LinuxAutomation - 상태 구성(DSC)** 블레이드에서 창 상단의 **+ 추가** 버튼을 클릭합니다.

1. **가상 머신** 블레이드에서 이전 연습에서 배포한 Linux 가상 머신을 나타내는 항목을 클릭합니다. 

1. 가상 머신 블레이드에서 **+ 연결** 을 클릭합니다.

1. **등록** 블레이드에서 다음 작업을 수행합니다.

    - **등록 키** 설정을 기본값으로  둡니다.

    - **노드 구성 이름** 드롭다운 목록에서 **lampserver.localhost** 항목을  선택합니다. 

    - 나머지 모든 설정을 기본값으로 둡니다.

    - **확인** 버튼을 클릭합니다.

1. 다음 단계로 진행하기 전에 연결 프로세스가 완료될 때까지 기다립니다.

1. **LinuxAutomation - 상태 구성(DSC)** 블레이드로  돌아갑니다.

1.  **LinuxAutomation - 상태 구성(DSC)** 블레이드에서 **NODE** 섹션에서 이전 연습에서 배포한 가상 머신을 선택합니다. 

1. 가상 머신 블레이드에서 **노드 구성 할당** 을 클릭합니다.

1. 노드 구성 할당 블레이드에서 노드 구성 **lampserver.host** 를 선택하고 **확인** 버튼을 클릭합니다.

1.  **LinuxAutomation - 상태 구성 (DSC)** 블레이드로 돌아가, **새로 고침** 버튼을 클릭합니다.

1. lsDSC 노드 목록에서 Linux 가상 머신에 **준수** 상태가  있는지 확인합니다.

> **복습**: 이 연습에서는 PowerShell DSC 구성을 만들고 구성을 Linux 가상 머신에 적용했습니다.


## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다. 

1. 포털 하단의 **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab12')]".name --output tsv
    ```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이 랩에서 만든 리소스 그룹을    삭제합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab12')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. 포털 하단의 **Cloud Shell** 프롬프트를  닫습니다.


> **복습**: 이 연습에서는 이 랩에서 사용된 리소스를 제거했습니다.
