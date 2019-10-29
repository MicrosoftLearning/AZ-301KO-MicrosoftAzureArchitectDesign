---
lab:
    title: 'Azure의 비밀 보안'
    module: '모듈 1: Azure 솔루션의 보안 및 ID 관리'
---

# Azure 솔루션의 보안 및 ID 관리 

# 랩 답변 키: Azure의 비밀 보안

## 시작하기 전에

1. 다음 자격 증명을 사용하여 Windows 10 랩 가상 컴퓨터에 로그인되어 있는지 확인합니다:

    - 사용자 이름: **관리자**

    - 암호: **Pa55w.rd**

1. Windows 10 바탕 화면 하단에 있는 작업 표시줄을 검토합니다. 작업 표시줄에는 랩에서 사용할 일반적인 응용 프로그램에 대한 아이콘이 포함되어 있습니다:

    - Microsoft Edge

    - 파일 탐색기

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **참고**: 또한 **시작 메뉴** 에서 이러한 응용 프로그램에 대한 바로 가기를 찾을 수 있습니다.

## 연습 1: Key Vault 리소스 배포

#### 작업 1: Azure 포털 열기

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열린 브라우저 창에서 **Azure 포털** (&https://portal.azure.com>)으로 이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정으로 인증합니다.

#### 작업 2: key vault 배포

1. Azure 포털의 왼쪽 위 모서리에서 **리소스 만들기** 를 클릭합니다.

1. **새** 블레이드의 맨 위에 **마켓플레이스 검색** 텍스트 상자에서 **템플릿 배포** 를 입력하고 **Enter** 를 누릅니다.

1. **모든** 블레이드에서 검색 결과에서 **템플릿 배포** 를 클릭합니다.

1. **Key Vault** 블레이드에서 **만들기** 단추를 클릭합니다.

1. **Key Vault 만들기** 블레이드에서 다음 작업을  수행합니다:

    - **이름** 텍스트 상자에 전역적으로 고유한 이름을 입력합니다.

    - **구독** 드롭다운 목록 항목을 기본값으로 설정합니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택한 다음 텍스트 상자에 **AADesignLab0901-RG** 를 입력합니다.

    - **위치** 드롭다운 목록에서 이 랩에서 리소스를 배포할 Azure 지역을 선택합니다.

    - **가격 책정 계층** 을 클릭하고 **가격 책정 계층** 블레이드에서 **A1 표준** 을 클릭한 다음 **선택** 을 클릭합니다.

    - 나머지 모든 설정을 기본값으로 둡니다.

    - **만들기** 단추를 클릭합니다.

1. 프로비전이 완료될 때까지 기다리고 다음 작업으로 진행합니다.

#### 작업 3: Azure portal를 사용하여 키 자격 증명 모음에 비밀 추가

1. Azure 포털의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0901-RG** 를 클릭합니다.

1. **AADesignLab0901-RG** 블레이드에서 새로 생성된 key vault를 나타내는 항목을 클릭합니다. 

1. Key Vault 블레이드에서 **비밀** 을 클릭합니다.

1. Key Vault 비밀 블레이드에서 창 상단의 **생성/가져오기** 단추를 클릭합니다.

1. **비밀 만들기** 블레이드 에서 다음 작업을 수행합니다:

    - **업로드 옵션** 드롭다운 목록에서 **수동** 항목을 선택해야 합니다.

    - **이름** 텍스트 상자에 **thirdPartyKey** 를 입력합니다.

    - **값** 텍스트 상자에 값 **56d95961e597ed0f04b76e58** 을 입력합니다.

    - 나머지 모든 설정을 기본값으로 둡니다.

    - **만들기** 단추를 클릭합니다.

#### 작업 4: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 셸 인스턴스를 엽니다.

    > **참고**: **Cloud Shell** 아이콘은 *보다 큰* 문자와 *밑줄* 문자의 조합으로 구성된 기호입니다.

1. 구독을 사용하여 **Cloud Shell** 을 처음 여는 경우 처음 사용할 수 있도록 **Cloud Shell** 을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell에 오신 것을 환영합니다** 창에서 **Bash (Linux)** 를 클릭합니다.

    > **참고**: **Cloud Shell** 에 대한 구성 옵션이 표시되지 않으면 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다.  그렇다면 다음 작업으로 직접 진행합니다. 

1. **마운트된 저장소가 없습니다** 창에서 **고급 설정 표시** 를 클릭하고 다음 작업을 수행합니다:

    - **구독** 드롭다운 목록 항목을 기본값으로 설정합니다.

    - **Cloud Shell 지역** 드롭다운 목록에서 일치하는 Azure 지역 또는 이 랩에서 리소스를 배포할 위치  근처를 선택합니다.

    - **리소스 그룹** 섹션에서 **기존 사용** 옵션을 선택한 다음 드롭다운 목록에서 **AADesignLab0901-RG** 를 선택합니다.

    - **저장소 계정** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 3~24자및 숫자로 구성된 고유한 이름을 입력합니다. 

    - **파일 공유** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell** 을 입력합니다.

    - **저장소 만들기** 단추를 클릭합니다.

1. 다음 작업을 진행하기 전에 **Cloud Shell** 이 처음 설치 절차를 완료할 때까지 기다립니다.

#### 작업 5: CLI를 사용하여 키 자격 증명 모음에 비밀 추가

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 새로 배포된 Azure VM을 포함하는 리소스 그룹의 이름을 지정하는 변수를 만듭니다:

    ```sh
    RESOURCE_GROUP='AADesignLab0901-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이 연습의 앞에서 만든 Azure 키 자격 증명 모음의 이름을 검색합니다:

    ```sh
    KEY_VAULT_NAME=$(az keyvault list --resource-group $RESOURCE_GROUP --query "[0].name" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 키 자격 증명 모음에 비밀을 나열합니다:

    ```sh
    az keyvault secret list --vault-name $KEY_VAULT_NAME
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 **thirdPartyKey** 보안  의 값을 표시합니다:

    ```sh
    az keyvault secret show --vault-name $KEY_VAULT_NAME --name thirdPartyKey --query value --output tsv
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 키 자격 증명 모음에 비밀을 나열합니다:

    ```sh
    az keyvault secret set --vault-name $KEY_VAULT_NAME --name firstPartyKey --value 56f8a55119845511c81de488
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 키 자격 증명 모음에 비밀을 나열합니다:

    ```sh
    az keyvault secret list --vault-name $KEY_VAULT_NAME --query "[*].{Id:id,Created:attributes.created}" --out table
    ```

1. **Cloud Shell** 창을 닫습니다.

#### 작업 6: Azure 리소스 관리자 템플릿을 사용하여 키 자격 증명 모음에 비밀 추가

1. Azure 포털의 왼쪽 위 모서리에서 **리소스 만들기** 를 클릭합니다.

1. **새** 블레이드의 맨 위에 **마켓플레이스** 검색 텍스트 상자에서 **템플릿 배포** 를 입력하고 **Enter** 를 누릅니다.

1. **모든** 블레이드에서 검색 결과에서 **템플릿 배포** 를 클릭합니다.

1. **템플릿 배포** 블레이드에서 **만들기** 단추를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿 빌드** 링크를 클릭합니다.

1. **템플릿 편집** 블레이드에서 **파일 로드** 를 클릭합니다.

1. 대화 상자 **열기** 에서 **\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** 폴더로 이동하여 **secret-template.json** 파일을 선택하고 **열기** 를 클릭합니다. 이렇게 하면 다음 콘텐츠가 템플릿 편집기 창에 로드됩니다:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vaultName": {
                "type": "string"
            }
        },
        "variables": {
            "secretName": "vmPassword"
        },
        "resources": [
            {
                "apiVersion": "2016-10-01",
                "type": "Microsoft.KeyVault/vaults/secrets",
                "name": "[concat(parameters('vaultName'), '/', variables('secretName'))]",
                "properties": {
                    "contentType": "text/plain",
                    "value": "StudentPa$$w.rd"
                }
            }
        ]
    }
    ```

1. **저장** 단추를 클릭하여 템플릿을 유지합니다.

1. **사용자 지정 배포** 블레이드로 돌아가서 다음 작업을 수행합니다:

    - **구독** 드롭다운 목록 항목을 기본값으로 설정합니다.

    - **리소스 그룹** 섹션에서 **기존 사용** 옵션을 선택한 다음 드롭다운 목록에서 **AADesignLab0901-RG** 를 선택합니다.

    - **Vault 이름** 텍스트 상자에 이 연습의 앞에서 만든 키 자격 증명 모음의 이름을 입력합니다.

    - **이용 약관** 섹션에서 확인란 **위에 명시된 이용 약관에 동의하는 것** 을 선택합니다.

    - **구입** 단추를 클릭합니다.

1. 배포가 완료될 때까지 기다리지 말고 다음 작업으로 진행합니다.

1. Azure 포털의 왼쪽 위 모서리에서 **리소스 만들기** 를 클릭합니다.

1. **새** 블레이드의 맨 위에 **마켓플레이스** 검색 텍스트 상자에서 **템플릿 배포** 를 입력하고 **Enter** 를 누릅니다.

1. **모든** 블레이드에서 검색 결과에서 **템플릿 배포** 를 클릭합니다.

1. **템플릿 배포** 블레이드에서 **만들기** 단추를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿 빌드** 링크를 클릭합니다.

1. **템플릿 편집** 블레이드에서 **파일 로드** 를 클릭합니다.

1. 대화 상자 **열기** 에서 **\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** 폴더로 이동하여 **storage-template.json** 파일을 선택하고 **열기** 를 클릭합니다. 이렇게 하면 다음 콘텐츠가 템플릿 편집기 창에 로드됩니다:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vaultName": {
                "type": "string"
            }
        },
        "variables": {
            "secretName": "storageConnectionString",
            "storageName": "[concat('stor', uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2017-10-01",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[variables('storageName')]",
                "location": "[resourceGroup().location]",
                "kind": "Storage",
                "sku": {
                    "name": "Standard_LRS"
                },
                "properties": {                
                }
            },
            {
                "apiVersion": "2016-10-01",
                "type": "Microsoft.KeyVault/vaults/secrets",
                "name": "[concat(parameters('vaultName'), '/', variables('secretName'))]",
                "dependsOn": [
                    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]"
                ],
                "properties": {
                    "contentType": "text/plain",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageName'), ';', 'AccountKey=', listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value, ';')]"
                }
            }
        ]
    }
    ```

1. **저장** 단추를 클릭하여 템플릿을 유지합니다.

1. **사용자 지정 배포** 블레이드로 돌아가서 다음 작업을 수행합니다:

    - **구독** 드롭다운 목록 항목을 기본값으로 설정합니다.

    - **리소스 그룹** 섹션에서 **기존 사용** 옵션을 선택한 다음 드롭다운 목록에서 **AADesignLab0901-RG** 를 선택합니다.

    - **Vault 이름** 필드에 이 연습의 앞에서 만든 키 자격 증명 모음의 이름을 입력합니다.

    - **이용 약관** 섹션에서 확인란 **위에 명시된 이용 약관에 동의하는 것** 을 선택합니다.

    - **구입** 단추를 클릭합니다.

1. 배포가 완료될 때까지 기다리고 다음 작업으로 진행합니다.

#### 작업 7: Key Vault 비밀 보기

1. Azure 포털의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0901-RG** 를 클릭합니다.

1. **AADesignLab0901-RG** 블레이드에서 이전 연습에서 만든 키 자격 증명 모음을 나타내는 항목을 클릭합니다.

1. Key Vault 블레이드에서 **비밀** 을 클릭합니다.

1. Key Vault 비밀 블레이드에서 이 랩에서 만든 비밀 목록을 검토합니다.

1. **vmPassword** 비밀을 나타내는 항목을 클릭합니다.

1. **vmPassword** 블레이드에서 비밀의 현재 버전을 나타내는 항목을 클릭합니다.

1. 비밀 버전 블레이드에서 **비밀 값 표시** 단추를 클릭합니다.

1. 비밀 값이 이전 작업에 배포한 템플릿에 포함된 값과 일치하는지 확인합니다.

> **검토**: 이 연습에서는 **Key Vault** 인스턴스를 만들고 여러 가지 방법을 사용하여 키 자격 증명 모음에 비밀을 추가했습니다.


## 연습 2: Key Vault 비밀을 사용하여 Azure VM 배포

#### 작업 1: Key Vault 리소스 ID 매개 변수의 값 검색

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Clould Shell 창을 엽니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 새로 배포된 Azure VM을 포함하는 리소스 그룹의 이름을 지정하는 변수를 만듭니다:

    ```
    RESOURCE_GROUP='AADesignLab0901-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이 연습의 앞에서 만든 Azure 키 자격 증명 모음의 리소스 ID를 검색합니다:

    ```sh
    KEY_VAULT_ID=$(az keyvault list --resource-group $RESOURCE_GROUP --query "[0].id" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 Azure 키 볼트 리소스ID의 이름을 지정하고 리소스 ID에 포함될 수 있는 특수 문자를 고려하는 변수를 만듭니다:

    ```sh
    KEY_VAULT_ID_REGEX="$(echo $KEY_VAULT_ID | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
    ```

#### 작업 2: Azure 리소스 관리자 배포 템플릿 및 매개 변수 파일 준비

1. **Cloud Shell** 창에서 **파일 업로드/다운로드** 아이콘을 클릭하고 드롭다운 메뉴에서 **업로드** 를 클릭합니다. 

1. 대화 상자 **열기** 에서 **\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** 폴더로 이동하여 **vm-template.json** 파일을 선택하고 **열기** 를 클릭합니다.  

1. **Cloud Shell** 창에서 **파일 업로드/다운로드** 아이콘을 클릭하고 드롭다운 메뉴에서 **업로드** 를 클릭합니다. 

1. 대화 상자 **열기** 에서 **\\allfiles\\AZ-301T01\\Module_01\\LabFiles\\Starter\\** 폴더로 이동하여 **vm-template.parameters.json** 파일을 선택하고 **열기** 를 클릭합니다. 

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 **vm-template.parameters.json** 매개 변수 파일의 **$KEY_VAULT_ID** 매개 변수의 자리 표시자를 **$KEY_VAULT_ID** 변수 값으로 바꿉니다:

    ```sh
    sed -i.bak1 's/"$KEY_VAULT_ID"/"'"$KEY_VAULT_ID_REGEX"'"/' ~/vm-template.parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 매개 변수 파일에서 자리 표시자가 성공적으로 대체되었는지 확인합니다:

    ```sh
    cat ~/vm-template.parameters.json
    ```

#### 작업 3: Azure 리소스 관리자 템플릿 배포를 위한 키 자격 증명 모음 구성

1. Azure 포털의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0901-RG** 를 클릭합니다.

1. **AADesignLab0901-RG** 블레이드에서 이전 연습에서 만든 키 자격 증명 모음을 나타내는 항목을 클릭합니다. 

1. Key Vault 블레이드에서 **액세스 정책** 을 클릭합니다.

1. **액세스 정책** 블레이드에서 **클릭하여 고급 액세스 정책 표시** 링크를 클릭합니다 

1. **템플릿 배포의 Azure 리소스 관리자에 대한 액세스 활성화** 확인란을 선택합니다.

1. 응용 프로그램 설정 탭 상단의 **저장** 단추를 클릭합니다.

#### 작업 4: Key Vault 암호를 사용하여 암호 매개 변수를 설정한 Linux VM을 배포합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 지정된 매개 변수 파일을 사용하여 Azure 리소스 관리자템플릿을 배포합니다:

    ```sh
    az 그룹 배포 생성 --리소스 그룹 $RESOURCE_GROUP --template-file ~/vm-template.json --parameters @~/vm-template.parameters.json
    ```

1. 배포가 완료될 때까지 기다리고 다음 작업으로 진행합니다.

#### 작업 5: 배포 결과 확인

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 새로 배포된 Azure VM을 포함하는 리소스 그룹의 이름을 지정하는 변수를 만듭니다:

    ```
    RESOURCE_GROUP='AADesignLab0901-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 로컬 관리자 계정의 암호 값을 저장하는 암호를 포함하는 Azure 키 자격 증명 모음의 이름을 검색합니다:

    ```sh
    KEY_VAULT_NAME=$(az keyvault list --resource-group $RESOURCE_GROUP --query "[0].name" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 비밀의 값을 검색합니다:

    ```sh
    az keyvault secret show --vault-name $KEY_VAULT_NAME --name vmPassword --query value --output tsv
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이전 작업에 배포한 Azure VM의 공용 IP 주소를 검색합니다:

    ```sh
    PUBLIC_IP=$(az network public-ip list --resource-group $RESOURCE_GROUP --query "[0].ipAddress" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 SSH를 통해 Azure VM에 연결합니다:

    ```sh
    ssh Student@$PUBLIC_IP
    ```

1. **Cloud Shell** 명령 프롬프트에서 계속 연결할지 묻는 메시지가 표시되면 `yes`를 입력하고 **Enter** 를 누릅니다.

1. **Cloud Shell** 명령 프롬프트에서 암호를 묻는 메시지가 표시되면 이 작업의 앞에서 검색한 암호 값을 입력하고 **Enter** 를 누릅니다.

1. 성공적으로 인증되었는지 확인합니다.

1. **Cloud Shell** 명령 프롬프트에서 `종료`를 입력하여 Azure VM에서 로그아웃합니다.

> **검토**: 이 연습에서는 Key Vault 암호로 저장된 암호를 사용하여 Linux VM을 배포했습니다.

## 연습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Clould Shell 창을 엽니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다:

    ```
    az group list --query "[?starts_with(name,'AADesignLab09')]".name --output tsv
    ```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab09')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. 포털 하단의 **Cloud Shell** 프롬프트를 닫습니다.

> **검토**: 이 연습에서는 이 랩에서 사용된 리소스를 제거했습니다.
