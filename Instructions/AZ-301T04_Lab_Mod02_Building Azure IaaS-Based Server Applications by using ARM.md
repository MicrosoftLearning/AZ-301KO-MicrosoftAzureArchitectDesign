---
lab:
    title: 'Azure ARM 템플릿 및 Azure Building Blocks를 사용하여 Azure IaaS 기반 서버 응용 프로그램 구축'
    module: '모듈 2: Azure laaS 기반 서버 응용 프로그램(ADSK) 작성'
---

# Azure IaaS 기반 서버 응용 프로그램 구축.
# 랩 답변 키: Azure 리소스 관리자 템플릿 및 Azure Building Blocks을 사용한 Azure IaaS 기반 서버 응용 프로그램 구축.

## 시작하기 전에

1. 다음 자격 증명을 사용하여 Windows 10 랩 가상 컴퓨터에 로그인되어 있는지 확인합니다.

    - 사용자 이름: **Admin**

    - 암호: **Pa55w.rd**

1. Windows 10 바탕 화면 하단에 있는 작업 표시줄을 검토합니다. 작업 표시줄에는 랩에서 사용할 일반적인 응용 프로그램 아이콘이 포함되어 있습니다.

    - Microsoft Edge

    - 파일 탐색기

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **참고**: **시작 메뉴** 에서 이러한 응용 프로그램에 대한 바로 가기를 찾을 수도 있습니다.


## 실습 1: Azure Portal에서 PowerShell Desired State Configuration(DSC) 확장자로 된 Azure 리소스 관리자 템플릿을 사용하여 Azure VM을 배포합니다.

#### 작업 1: Azure Portal 엽니다.

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열려 있는 브라우저 창에서 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정으로 인증합니다.

#### 작업 2: Windows Server 2016 데이터 센터를 실행하는 Azure VM을 만듭니다.

1. Azure Portal의 왼쪽 상단 모서리에서 **리소스 생성** 을 클릭합니다.

1. **Marketplace 검색** 텍스트 상자의 **신규** 블레이드 상단에서 **Windows Server**를 입력하고 **Enter** 키를 누릅니다.

1. **전체** 블레이드의 검색 결과에서 **Windows Server**를 클릭합니다.

1. **Windows Server** 블레이드에서 **[smalldisk] Windows Server 2016 Datacenter** 소프트웨어 플랜을 선택한 다음 **만들기** 단추를 클릭합니다.

1. **기본** 탭에서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.
    
    - **리소스 그룹** 섹션에서 **새로 생성** 을 클릭하고 텍스트 상자에서 **AADesignLab0301-RG** 를 입력하고 **확인** 을 클릭합니다.
      
    - **이름** 텍스트 상자에 값 **lab03vm0** 을 입력합니다.

    - **지역** 드롭다운 목록에서 이 랩에서 리소스를 배포할 Azure 지역을 선택합니다.
    
    - **가용성 옵션** 드롭다운 목록에서 **가용성 모음** 을 선택합니다.

    - **가용성 모음** 섹션에서 **새로 생성** 상자를 클릭하고 값을 **lab03avset0** 로 입력하고 **결함 도메인** 을 최대값으로 설정하고 **업데이트 도메인** 를 기본값 설정으로 유지한 다음 **확인** 을 클릭합니다.

    - **이미지** 드롭다운 목록의 항목을 기본값 설정으로 유지합니다.

    - 크기가 **Standard DS1 v2** 로 설정되어 있는지 확인합니다

    - **사용자 이름** 텍스트 상자에 값을 "학생"으로 입력합니다.

    - **암호** 및 **암호 확인** 텍스트 상자에 **Pa55w.rd1234** 값을 입력합니다.

    - **공개 인바운드 포트** 섹션에서 **선택한 포트 허용** 옵션을 선택하고 **인바운드 포트 선택** 드롭다운 목록에서 **HTTP** 를 선택합니다.

    - **Windows 라이선스가 이미 있습니까?** 옵션을 **아니요** 설정으로 유지합니다.
    
    - **다음: 디스크 >** 1을 클릭합니다.
    
1. **디스크** 탭에서 다음 작업을 수행합니다.

    - **OS 디스크 유형** 드롭다운 목록 항목이 **프리미엄 SSD** 로 설정되어 있는지 확인합니다.

    - **다음: 네트워킹 >** 1을 클릭합니다.
    
1. **네트워킹** 탭에서 다음 작업을 수행합니다. 

    - **가상 네트워크** 섹션에서 **새로 생성** 을 클릭합니다. 
    
    - **가상 네트워크 생성** 블레이드에서 다음 설정을 지정하고 **확인** 을 클릭합니다.

        - **이름** 텍스트 상자에 **lab03vnet0** 값을 입력합니다.

        - **주소 범위** 텍스트 상자에 **10.3.0.0/16** 값을 입력합니다.

        - **서브넷 이름** 텍스트 상자에 **subnet-0** 값을 입력합니다.

        - **서브넷 주소 범위** 텍스트 상자에서 **10.3.0.0/24** 값을 입력한 다음, **확인** 을 클릭합니다.

    - **공개 IP** 항목을 기본값 설정으로 유지합니다.
    
    - **NIC 네트워크 보안 그룹** 옵션을 **기본** 설정으로 유지합니다.
    
    - **공개 인바운드 포트** 옵션을 **선택된 포트 허용** 설정으로 유지합니다.
    
    - **인바운드 포트 선택** 항목을 **HTTP** 설정으로 유지합니다.

    - **가속화 네트워킹** 항목을 기본값 설정으로 유지합니다.

    - **다음: 관리 >** 클릭합니다.
    
1. **관리** 탭에서 다음 작업을 수행합니다. 

    - **부팅 진단** 옵션을 기본값으로 설정한 상태로 유지합니다.

    - **OS 게스트 진단** 옵션을 기본값 설정으로 유지합니다.

    - **진단 스토리지 계정** 항목을 기본값 설정으로 유지합니다.
    
    - **시스템이 배정한 관리형 ID** 옵션을 기본값 설정으로 유지합니다.
    
    - **자동 차단 기능 활성화** 옵션을 기본값 설정으로 유지합니다.

    - **백업 활성화** 옵션을 기본값 설정으로 유지합니다.

    - ** 검토 + 생성** 버튼을 클릭합니다.
    
1. **가상 컴퓨터 생성** 블레이드에서 새 가상 컴퓨터의 설정을 검토한 다음, **생성** 버튼을 클릭합니다.

1. 배포가 완료되기까지 기다리지 말고 다음 작업으로 진행하십시오.


#### 작업 3: DSC 구성 봅니다.

1. 작업 표시줄에서 **파일 탐색기** 아이콘을 클릭합니다.

1. 표시되는 **파일 탐색기** 창에서 **\\\allfiles\\AZ-301T04\\Module_02\LabFiles\\\Starter\\** 폴더로 이동합니다.

1. **IISWebServer.zip** 파일을 마우스 오른쪽 단추로 클릭하고 **모두 추출...** 옵션을 선택합니다.

1. **압축(집 모음) 폴더 추출** 대화 상자에서 다음 작업을 수행합니다.

    - **파일이 이 폴더로 추출됩니다:** 필드에 추출하고 싶은 파일이 있는 폴더의 이름을 입력합니다.

    - **완료 시 추출된 파일 표시** 확인란이 선택되어 있는지 확인합니다.
    
    - **추출** 버튼을 클릭합니다.

1. 표시되는 새로운 **파일 탐색기** 창에서 **IISWebServer.ps1** 파일을 마우스 오른쪽 단추로 클릭하고, **코드로 열기** 옵션을 선택하여 **비주얼 스튜디오 코드** 응용 프로그램을 시작합니다.

1. 표시되는 **비주얼 스튜디오 코드** 창에서 PowerShell 스크립트의 내용을 검토합니다.

1. **비주얼 스튜디오 코드** 창 상단에서 **파일** 메뉴를 클릭하고 **창 닫기** 옵션을 선택합니다.

1. **파일 탐색기** 창을 모두 닫습니다.

1. **Azure Portal** 이 열려 있는 상태에서 **Microsoft Edge** 창으로 돌아갑니다.


#### 작업 4: Azure 스토리지 계정 만듭니다.

1. Azure Portal의 왼쪽 상단 모서리에서 **리소스 생성** 을 클릭합니다.

1. **마켓플레이스 검색** 텍스트 상자의 **신규** 블레이드 상단에 **스토리지 계정** 을 입력하고 **엔터** 를 누릅니다.

1. **모든** 블레이드의 검색 결과에서 **스토리지 계정** 를 클릭합니다.

1. **스토리지 계정** 블레이드에서 **생성** 버튼을 클릭합니다.

1. **스토리지 계정 생성** 블레이드에서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.
    
    - **리소스 그룹** 섹션에서 **기존의 것 사용** 옵션이 선택되었는지 확인한 다음, 아래의 드롭다운 목록에서 이전에 진행한 실습에서 만들어놓은 리소스 그룹을 선택하십시오.

    - **이름** 텍스트 상자에 3~24개 글자와 숫자로 구성된 고유한 이름을 입력합니다.
    
    - **위치** 항목을 이 실습 초반에 선택한 것과 동일한 Azure 지역 설정으로 유지합니다.
    
    - **성능** 섹션에서 **표준** 옵션이 선택되었는지 확인합니다.
       
    - **계정 종류** 드롭다운 목록에서 **스토리지(범용 v1)** 옵션을 선택해야 합니다.
    
    - **복제** 드롭다운 목록에서 **LRS(Locally-redundant storage)** 항목을 선택합니다.

    - **검토 + 생성** 버튼을 클릭한 다음 **생성** 을 클릭합니다.

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 이 작업은 약 2분 정도 걸릴 수 있습니다.


#### 작업 5: DSC 구성을 Azure 스토리지 업로드합니다.

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 스토리지 계정을 배포한 리소스 그룹을 나타내는 항목을 클릭합니다.

1. 리소스 그룹 블레이드에서 새로 만든 스토리지 계정을 나타내는 항목을 클릭합니다.

1. 스토리지 계정 블레이드에서 **개요** 선택이 활성화된 상태로 **Blob** 을 클릭합니다.

1. 블레이드 상단의 **컨테이너** 버튼을 클릭합니다.

1. 표시되는 **새 컨테이너** 창에서 다음 설정을 지정하고 **확인** 을 클릭합니다.

    - **이름** 텍스트 상자에 **config** 값을 입력합니다.

    - **공개 액세스 레벨** 목록에서 **Blob(Blob 전용 익명 읽기 액세스)** 옵션을 선택합니다.

1. **Blob 서비스** 블레이드로 돌아가서 새 **구성** 컨테이너를 나타내는 항목을 클릭합니다.

1. **구성** 블레이드에서 블레이드 상단의 **업로드** 버튼을 클릭합니다.

1. **blob 업로드** 창에서 다음 작업을 수행합니다.

    - **파일** 필드에서 필드 오른쪽에 있는 파란색 폴더 버튼을 클릭합니다.

    - 표시되는 **파일 열기** 대화 상자에서 **\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\** 폴더로 이동합니다.

    - **IISWebServer.zip** 파일을 선택합니다.

    - **열기** 버튼을 클릭하여 대화 상자를 닫고 **blob 업로드** 팝업으로 돌아갑니다.

    - **업로드** 버튼을 클릭합니다.

1. **구성** 블레이드로 이동하여 **IISWebServer.zip** Blob을 나타내는 항목을 클릭합니다.

1. 표시되는 **Blob 속성** 팝업에서 **URL** 속성 값을 찾아서 기록합니다. 이 URL은 이 랩에서 나중에 사용합니다.


#### 작업 6: Azure Portal의 PowerShell DSC 확장이 포함된 Azure Resource Manager 템플릿을 사용하여 Azure VM을 배포합니다.

1. Azure Portal의 왼쪽 상단 모서리에서 **리소스 생성** 을 클릭합니다.

1. **마켓플레이스 검색** 텍스트 상자의 **신규** 블레이드 상단에 **템플릿 배치** 를 입력하고 **엔터** 를 누릅니다.

1. **모든** 블레이드의 검색 결과에서 **템플릿 배치** 를 클릭합니다.

1. **템플릿 배치** 블레이드에서 **생성** 버튼을 클릭합니다.

1. **사용자 지정 배치** 블레이드에서 **편집기에서 자체적인 템플릿 구축** 링크를 클릭합니다.

1. **템플릿 편집** 블레이드에서 **파일 로드** 를 클릭합니다.

1. **업로드할 파일 선택** 대화 상자에서 **\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\** 폴더로 이동하여 **dsc-extension-template.json** 파일을 선택하고 **열기** 를 클릭합니다. 이렇게 하면 다음 콘텐츠가 템플릿 편집기 창에 로드됩니다.

```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "virtualMachineName": {
                "type": "string",
                "defaultValue": "lab03vm0"
            },
            "configurationModuleUrl": {
                "type": "string"
            },
            "extensionFunction": {
                "type": "string",
                "defaultValue": "IISWebServer.ps1\\IISWebServer"
            }
        },
        "resources": [
            {
                "apiVersion": "2018-06-01",
                "type": "Microsoft.Compute/virtualMachines/extensions",
                "name": "[concat(parameters('virtualMachineName'), '/dscExtension')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "publisher": "Microsoft.Powershell",
                    "type": "DSC",
                    "typeHandlerVersion": "2.75",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "ModulesUrl": "[parameters('configurationModuleUrl')]",
                        "ConfigurationFunction": "[parameters('extensionFunction')]",
                        "Properties": {
                            "MachineName": "[parameters('virtualMachineName')]"
                        }
                    },
                    "protectedSettings": null
                }
            }
        ]
    }
```

1. **저장** 버튼을 클릭하여 템플릿을 유지합니다.

1. **사용자 지정 배치** 블레이드로 돌아가서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.

    - **리소스 그룹** 섹션에서 **기존의 것 사용** 옵션을 선택하고 드롭다운 목록에서 이 실습의 앞에서 만든 리소스 그룹을 선택합니다.

    - **위치** 드롭다운 목록을 기본값으로 설정합니다.

    - **가상 컴퓨터 이름** 필드를 기본값 설정으로 유지합니다. **lab03vm0**.

    - **구성 모듈 Url** 필드에 이전 작업에서 기록해 둔 URL 값을 입력합니다.

    - **확장 기능** 필드를 기본값 설정으로 유지합니다. **IISWebServer.ps1\\IISWebServer**.

    - **이용 약관** 섹션에서 **위에 명시된 이용 약관에 동의하는 바입니다** 확인란을 선택하십시오.

    - **구입** 버튼을 클릭합니다.

1. **구입** 버튼을 클릭합니다.

    > **참고**: DSC 구성 배포에는 최대 10분이 걸릴 수 있습니다.


#### 작업 7: Azure VM이 웹 콘텐츠 제공하고 있는지 확인합니다

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 가상 컴퓨터를 배포한 리소스 그룹을 나타내는 항목을 클릭합니다.

1. 리소스 그룹 블레이드에서 배포한 **가상 컴퓨터** 를 나타내는 항목을 클릭합니다.

1. **가상 컴퓨터** 블레이드에서 **공개 IP 주소** 항목을 찾아 해당 값을 식별합니다.

1. 새 Microsoft Edge 탭을 열고 이전 단계에서 식별한 IP 주소로 이동합니다.

1. 기본 인터넷 정보 서비스 웹 페이지에 액세스할 수 있는지 확인합니다.

1. 새 브라우저 탭을 닫습니다.

> **검토**: 이 실습에서는 Azure Portal에서 **가상 컴퓨터** 을 배포한 다음 **PowerShell DSC** 확장자를 사용하여 독자적인 방식으로 가상 컴퓨터에 변경 사용을 적용했습니다.


## 실습 2: Azure Portal에서 PowerShell Desired State Configuration(DSC) 확장자를 사용하는 Azure 리소스 관리자 템플릿을 사용하여 Azure Virtual Machine Scale Set (VMSS)을 배포합니다.

#### 작업 1: Azure 리소스 관리자 템플릿을 보십시오.

1. 작업 표시줄에서 **파일 탐색기** 아이콘을 클릭합니다.

1. 표시되는 파일 **열기** 대화 상자에서 **\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\*** 폴더로 이동합니다.

1. **vmss-template.json** 파일을 마우스 오른쪽 단추로 클릭하고 **코드로 열기** 옵션을 선택하여 **비주얼 스튜디오 코드** 응용 프로그램을 시작합니다.

1. 표시되는 **비주얼 스튜디오 코드** 창에서 JSON 파일의 내용을 검토합니다.

1. **비주얼 스튜디오 코드** 창 상단에서 **파일** 메뉴를 클릭하고 **창 닫기** 옵션을 선택합니다.

1. **파일 탐색기** 창을 닫습니다.

1. **Azure Portal** 이 열려 있는 상태에서 **Microsoft Edge** 창으로 돌아갑니다.


#### 작업 2: ARM 사용하여 VMSS를 배포합니다

1. Azure Portal의 허브 메뉴에서 **리소스 생성** 을 클릭합니다.

1. **마켓플레이스 검색** 텍스트 상자의 **신규** 블레이드 상단에 **템플릿 배치** 를 입력하고 **엔터** 를 누릅니다.

1. **모든** 블레이드의 검색 결과에서 **템플릿 배치** 를 클릭합니다.

1. **템플릿 배치** 블레이드에서 **생성** 버튼을 클릭합니다.

1. **사용자 지정 배치** 블레이드에서 **편집기에서 자체적인 템플릿 구축** 를 클릭합니다.

1. **템플릿 편집** 블레이드에서 **파일 로드** 를 클릭합니다.

1. 표시되는 **열기** 파일 대화 상자에서 **\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\*** 폴더로 이동합니다.

1. **vmss-template.json** 파일을 선택합니다.

1. **열기** 버튼을 클릭합니다.

1. **템플릿 편집** 블레이드로 돌아가서 **저장** 버튼을 클릭하여 템플릿을 유지합니다.

1. **사용자 지정 배치** 블레이드로 돌아가서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.

    - **리소스 그룹** 섹션에서 **새로 생성** 옵션을 선택하고 텍스트 상자에 **AADesignLab0302-RG** 를 입력합니다.

    - **위치** 항목을 기본값 설정으로 유지합니다.

    - **관리자 사용자 이름** 텍스트 상자에 **학생** 값을 입력합니다.

    - **관리자 암호** 텍스트 상자에 **Pa55w.rd1234** 값을 입력합니다.

    - **인스턴스 카운트** 텍스트 상자에 **2** 값을 입력합니다.

    - **오버프로비전** 텍스트 상자를 기본값 설정으로 유지합니다. **true**.

    - **구성 모듈 Url** 텍스트 상자에 이 랩의 이전 실습에서 업로드된 Blob에 대해 기록한 URL을 입력합니다.

    - **이용 약관** 섹션에서 **위에 명시된 이용 약관에 동의하는 바입니다** 확인란을 선택하십시오.

    - **구입** 버튼을 클릭합니다.

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.


#### 작업 3: VMSS 인스턴스가 웹 콘텐츠 제공하고 있는지 확인합니다

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 가상 컴퓨터 확장 설정을 배포한 리소스 그룹을 나타내는 항목을 클릭합니다.

1. 리소스 그룹 블레이드에서 **공개 IP 주소** 유형의 리소스를 클릭합니다.

1. **필수 항목** 섹션의 공용 IP 주소 리소스 블레이드에서 **IP 주소** 항목 값을 식별합니다.

1. O새 Microsoft Edge 탭을 열고 이전 단계에서 식별한 IP 주소로 이동합니다.

1. 기본 인터넷 정보 서비스 웹 페이지에 액세스할 수 있는지 확인합니다.

1. 새 브라우저 탭을 닫고 현재 활성 상태인 **Azure Portal** 을 사용하여 브라우저 탭으로 돌아갑니다.

> **검토**: 이 실습에서는 가상 컴퓨터 확장 설정을 만들고 PowerShell DSC를 사용하여 개별 인스턴스를 구성했습니다.


## 실습 3: Azure Cloud Shell에서 PowerShell DSC(Desired State Configuration) 확장을 지원하는 Azure Building Blocks를 사용하여 Windows Server 2016 및 Linux를 실행하는 Azure VM을 배치할 수 있습니다.

#### 작업 1: Cloud Shell 엽니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

    > **참고**: **Cloud Shell** 아이콘은 *초과* 와 *밑줄* 문자의 조합으로 구성된 기호입니다.

1. 구독을 사용하여 **Cloud Shell** 을 처음 여는 경우, 처음에만 사용할 수 있도록 **Cloud Shell** 을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell 환영 인사** 창에서 **Bash (Linux)** 를 클릭합니다.

    > **참고**: **Cloud Shell** 에 대한 구성 옵션이 표시되지 않는 경우, 대부분은 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다. 그런 경우, 다음 작업으로 직접 진행합니다. 

1. **장착 스토리지가 없습니다** 창에서 **고급 설정 보기** 를 클릭한 다음, 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.

    - **Cloud Shell 지역** 드롭다운 목록에서 이 실습에 리소스를 배포할 위치와 일치 또는 근처에 있는 Azure 지역을 선택합니다.

    - **리소스 그룹** 섹션에서 **기존 항목 사용** 옵션이 선택되어 있는지 확인한 다음 **AADesignLab0301-RG**를 선택합니다.

    - **스토리지 계정** 섹션에서 **새로 생성** 옵션을 선택한 다음, 아래 텍스트 상자에 3~24개 문자와 숫자로 구성된 고유한 이름을 입력합니다. 

    - **파일 공유하기** 섹션에서 **새로 생성** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell** 을 입력합니다.

    - **스토리지 생성** 버튼을 클릭합니다.

1. 다음 작업을 진행하기 전에 **Cloud Shell** 이 첫 번째 설치 절차를 완료할 때까지 기다립니다.


#### 작업 2: Azure Cloud Shell 에 Azure Building Blocks npm 패키지를 설치합니다

1. 포털 하단의 **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure Building Blocks npm 패키지를 설치하는 로컬 디렉터리를 만듭니다.

```sh
    mkdir ~/.npm-global
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 npm 구성을 업데이트하여 새 로컬 디렉터리를 포함시킵니다.

```sh
    npm config set prefix '~/.npm-global'
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 편집을 위해 ~./bashrc 구성 파일을 엽니다.

```sh
    vi ~/.bashrc
```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 파일을 아래쪽으로 스크롤(또는 **G** 를 입력), 마지막 줄의 오른쪽 가장 끝 문자까지 오른쪽으로 스크롤(또는 **$** 를 입력), **a** 를 입력하여 **삽입** 모드를 입력하고, **엔터** 를 눌러 새 줄을 시작한 다음, 다음을 입력하여 새로 만든 디렉터리를 시스템 경로에 추가합니다.

```sh
    export PATH="$HOME/.npm-global/bin:$PATH"
```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 변경 내용을 저장하고 파일을 닫기 위해 **Esc** 를 누르고 **:** 를 누른 다음, **wq!** 를 입력한 후 **엔터** 를 누릅니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure Buiding Blocks npm 패키지를 설치합니다.

```sh
    npm install -g @mspnp/azure-building-blocks
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 셸을 종료합니다.

```sh
    exit
```

1. **Cloud Shell 시간 초과** 창에서 **재연결** 을 클릭합니다. 

    > **참고**: Buliding Blocks npm 패키지를 설치하려면 Cloud Shell을 다시 시작해야 합니다.


#### 작업 3: Azure Building Blocks 사용하여 Cloud Shell에서 Windows Server 2016 Azure VM을 배포합니다

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure Building Blocks 참조 아키텍처 파일을 포함하는 GitHub 리포지토리를 다운로드합니다.

```
    git clone https://github.com/mspnp/reference-architectures.git
```

1.  **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 이 배포에 사용할 Azure Building Block 매개 변수 파일의 내용을 봅니다.

```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" --output tsv | tr -d '"')
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 이 실습 초반에 만든 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

```sh
    RESOURCE_GROUP='AADesignLab0303-RG'
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다.

```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0301-RG'].location" --output tsv)
```
    
1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 리소스 그룹을 만듭니다.

```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 **adminUsername** 매개 변수의 자리 표시자를 Building Blocks 매개 변수 파일의 **학생** 값으로 바꿉니다.

```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 **adminPassword** 매개 변수의 자리 표시자를 Building Blocks 매개 변수 파일에서 **Pa55w.rd1234** 값으로 바꾸십시오.

```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Building Block 매개 변수 파일에서 매개 변수 값이 성공적으로 변경되었는지 확인합니다.

```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure Building Block을 사용하여 Windows Server 2016 Azure VM을 배포합니다.

```sh
    azbb -g $RESOURCE_GROUP -s $SUBSCRIPTION_ID -l $LOCATION -p ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json --deploy
```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.


#### 작업 4: Windows Server 2016 Azure VM이 웹 콘텐츠 제공하고 있는지 확인합니다.

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 이 실습 초반에 Windows Server 2016 데이터 센터 가상 컴퓨터를 배포한 리소스 그룹을 나타내는 항목을 클릭합니다.

1. 리소스 그룹 블레이드에서 배포한 가상 컴퓨터를 나타내는 항목을 클릭합니다.

1. **가상 컴퓨터** 블레이드에서 **공개 IP 주소** 항목을 찾아 해당 값을 식별합니다.

1. 새 Microsoft Edge 탭을 열고 이전 단계에서 식별한 IP 주소로 이동합니다.

1. 기본 인터넷 정보 서비스 웹 페이지에 액세스할 수 있는지 확인합니다.

1. 새 브라우저 탭을 닫습니다.


#### 작업 5: Azure Building Blocks 사용하여 Cloud Shell의 Linux Azure VM을 배포합니다.

1.  **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 이 배포에 사용할 Azure Building Block 매개 변수 파일의 내용을 봅니다.

```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Linux VM에 액세스할 때 인증하는 데 사용할 SSH 키 쌍을 생성합니다.

```sh
    ssh-keygen -t rsa -b 2048
```

    - 키를 저장할 파일을 입력하라는 메시지가 표시되면 **엔터** 를 눌러 기본값 **(~/.ssh/id_rsa)** 을 수락합니다.

    - 암호를 입력하라는 메시지가 표시되면 **엔터** 를 두 번 누릅니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 새로 생성된 키 쌍의 공개 키를 지정하는 변수를 만듭니다.

```sh
    PUBLIC_KEY=$(cat ~/.ssh/id_rsa.pub)
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 새로 생성된 키 쌍의 공개 키를 지정하고 공개 키에 포함될 수 있는 특수 문자를 고려하는 변수를 만듭니다.

```sh
    PUBLIC_KEY_REGEX="$(echo $PUBLIC_KEY | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
```

    > **참고**: **sed** 유틸리티를 사용하여 이 문자열을 Azure Building Blocks 매개 변수 파일에 삽입할 때 필요합니다. 또는 파일을 열고 공개 키 문자열을 파일에 직접 입력할 수 있습니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" --output tsv | tr -d '"')
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 배포에 사용할 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

```sh
    RESOURCE_GROUP='AADesignLab0304-RG'
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다.

```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0301-RG'].location" --output tsv)
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 **adminUsername** 매개 변수의 자리 표시자를 Building Blocks 매개 변수 파일의 **학생** 값으로 바꿉니다.

```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 **sshPublicKey** 매개 변수의 자리 표시자를 Building Blocks 매개 변수 파일의 **$PUBLIC_KEY_REGEX** 변수 값으로 바꿉니다.

```sh
    sed -i.bak2 's/"sshPublicKey": ""/"sshPublicKey": "'"$PUBLIC_KEY_REGEX"'"/' ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Building Block 매개 변수 파일에서 매개 변수 값이 성공적으로 변경되었는지 확인합니다.

```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 새 리소스 그룹을 만듭니다.

```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 Azure Building Blocks를 사용함으로써 Linux Azure VM을 배포합니다.

```sh
    azbb -g $RESOURCE_GROUP -s $SUBSCRIPTION_ID -l $LOCATION -p ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json --deploy
```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.


#### 작업 7: Linux Azure VM이 웹 콘텐츠 제공하고 있는지 확인합니다

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 가상 컴퓨터를 배포한 리소스 그룹을 나타내는 항목을 클릭합니다.

1. 리소스 그룹 블레이드에서 배포한 가상 컴퓨터를 나타내는 항목을 클릭합니다.

1. **가상 컴퓨터** 블레이드에서 **공개 IP 주소** 항목을 찾아 해당 값을 식별합니다.

1. 새 Microsoft Edge 탭을 열고 이전 단계에서 식별한 IP 주소로 이동합니다.

1. 기본 Apache2 Ubuntu 웹 페이지에 액세스할 수 있는지 확인합니다.

1. 새 브라우저 탭을 닫습니다.

1. **Cloud Shell** 창을 닫습니다.

> **검토**: 이 실습에서는 Azure Building Blocks를 사용하여 Cloud Shell에서 Windows Server 2016 데이터 센터 및 Linux를 실행하는 Azure VM을 배포했습니다.


## 실습 4: 랩 리소스 제거

#### 작업 1: Cloud Shell 엽니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 이 랩에서 생성한 모든 리소스 그룹을 나열합니다.

```sh
    az group list --query "[?starts_with(name,'AADesignLab03')]".name --output tsv
```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제합니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터** 를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다. 

```sh
    az group list --query "[?starts_with(name,'AADesignLab03')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.


> **검토**: 이 실습에서는 이 랩에서 사용된 리소스를 제거했습니다.
