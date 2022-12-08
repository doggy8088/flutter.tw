---
title: Build and release a Windows desktop app
title: 建構和釋出為 Windows 應用
description: How to release a Flutter app to the Microsoft Store.
description: 如何釋出一個 Flutter 應用到 Microsoft Store。
short-title: windows
---

One convenient approach to distributing Windows apps
is the [Microsoft Store][microsoftstore].
This guide provides a step-by-step walkthrough
of packaging and deploying a Flutter app in this way.

釋出 Windows 桌面應用程式的便捷方法是將其釋出至
[微軟應用商店][microsoftstore]，本指南提供了
將 Flutter 應用釋出到 [微軟應用商店][microsoftstore]
的分步操作指南。

{{site.alert.note}}

  You are not required to publish Windows apps through the
  Microsoft Store, particularly if you prefer more control
  over the distribution experience or don't want to deal
  with the certification process. The Microsoft documentation
  includes more information about traditional installation
  approaches, including [Windows Installer][msidocs].

  透過微軟應用商店釋出 Windows 桌面應用程式並非必選項，
  尤其是當你希望掌握更多對釋出體驗方面的控制權，
  亦或是你不想處理認證過程。
  微軟提供的文件包括了關於使用 [Windows Installer][msidocs]
  進行傳統安裝的更多內容。
  
{{site.alert.end}}


## Preliminaries

## 預先準備

Before beginning the process of releasing
a Flutter Windows desktop app to the Microsoft Store,
first confirm that it satisfies [Microsoft Store Policies][storepolicies].

在開始釋出 Flutter Windows 桌面應用程式之前，
有必要確認你的應用滿足 [微軟應用商店政策][storepolicies]。

Also, you must join the
[Microsoft Partner Network][microsoftpartner] to be able to submit apps.

提交應用前加入 [微軟合作伙伴網路][microsoftpartner] 也是必要的。

## Set up your application in the Partner Center

## 在合作伙伴中心申請應用

Manage an application's life cycle in the
[Microsoft Partner Center][microsoftpartner].

在 [微軟合作伙伴中心][microsoftpartner] 中管理一個應用程式的生命週期。

First, reserve the application name and
ensure that the required rights to the name exist.
Once the name is reserved, the application
will be provisioned for services (such as
push notifications), and you can start adding add-ons.

首先，有必要預約應用名稱，並且確定有使用這個名稱的權利。
一旦名稱確定可用，則該應用程式將可以提供推送通知等服務，並且可以開始新增擴充功能。

Options such as pricing, availability,
age ratings, and category have to be
configured together with the first submission
and are automatically retained
for the subsequent submissions.

類似於價格、可用性、年齡等級和類別等選項必須在第一次提交時一併配置，
這些選項在後續提交時將自動保留。

## Packaging and deployment

## 打包並部署

In order to publish an application to Microsoft Store,
you must first package it.
The valid formats are **.msix**, **.msixbundle**,
**.msixupload**, **.appx**, **.appxbundle**,
**.appxupload**, and **.xap**.

應用程式釋出到微軟商店前必須要打套件。
有效的格式是 **.msix**、**.msixbundle**、**.msixupload**、**.appx**、
**.appxbundle**、**.appxupload** 和 **.xap**。

### Manual packaging and deployment

### 手動打套件和部署

Check out [MSIX packaging][msix packaging] to learn about packaging 
Flutter Windows Desktop applications.

檢視 [MSIX 打包][msix packaging]，學習如何打包 Flutter Windows 桌面應用程式。

Note that each product has a unique identity, which the Store assigns.

請注意，每個產品都有一個唯一的 ID，這是應用商店分配的。

If the package is being built manually, it is necessary to include its 
identity details manually during the packaging. The essential information
can be retrieved from the Partner Center:

如果應用包是手動建構的，在打包過程中必須手動新增 ID 詳情。
這些基本資訊可以從合作伙伴中心檢索到。

1. In the Partner Center, navigate to the application.

   在合作伙伴中心中，導航到應用程式。

2. Select **Product management**.

   選擇 **Product management**。

3. Retrieve the package identity name, publisher, 
and publisher display name by clicking on **Product identity**.

   點選 **Product identity**，檢查應用套件的 ID，釋出者，釋出者的顯示名稱。

After manually packaging the application, it will also have to be 
manually submitted to the [Microsoft Partner Center][microsoftpartner].
This can be done by creating a new submission, navigating to **Packages**, 
and uploading the created application package.

在手動打包應用程式後，還需要手動提交應用程式包到 [微軟合作伙伴中心][microsoftpartner]。
這個步驟可以透過建立新的提交來完成，導航到 **Packages**，並上傳建立的應用程式套件。

### Continuous deployment

### 持續部署

In addition to manually creating and deploying the package,
you can automate the build, package, versioning,
and deployment process using CI/CD tooling after having submitted
the application to the Microsoft Store for the first time.

除了手動建立和部署軟體包外，在第一次提交應用程式到微軟商店後，
還可以使用持續整合/持續部署 (CI/CD) 工具自動化建構、打包、版本管理、和部署應用程式。

#### Codemagic CI/CD

#### Codemagic 持續整合/持續部署 (CI/CD)

[Codemagic CI/CD][codemagic] uses the
[`msix` pub package][msix package] to package 
Flutter Windows Desktop applications. 

[Codemagic 持續整合/持續部署 (CI/CD)][codemagic]
使用 [Pub 上的 `msix` package][msix package]
來打包 Flutter Windows 桌面應用程式。

For Flutter applications, use either the
[Codemagic Workflow Editor][cmworkfloweditor]
or [codemagic.yaml][cmyaml]
to package the application and deploy it
to the Microsoft Partner Center.
Additional options (such as the list of
capabilites and language resources
contained in the package)
can be configured using this package.

對於 Flutter 應用程式，無論是 [Codemagic Workflow Editor][cmworkfloweditor] 還是 [codemagic.yaml][cmyaml]
都可以用來打包應用程式並部署到微軟合作伙伴中心。
其他選項（如軟體套件中包含的功能列表和語言資源）可以使用上述的軟體套件進行配置。

For publishing, Codemagic uses the
[Partner Center submission API][partnercenterapi];
so, Codemagic requires
[associating the Azure Active Directory
and Partner Center accounts][azureadassociation].

對於釋出應用來說，Codemagic 使用 [合作伙伴中心提交 API][partnercenterapi]；
因此，Codemagic 需要 [關聯 Azure Active Directory 和合作夥伴中心賬戶][azureadassociation]。

## Updating the app's version number

## 更新應用程式的版本號

For apps published to the Microsoft Store,
the version number must be set during the
packaging process.

對於釋出到 Microsoft Store 的 Flutter 桌面版應用，
版本號必須在打包過程中設定，而不能透過
`pubspec.yaml` 或命令列引數設定。

The default version number of the app is `1.0.0.0`.

預設的應用版本號為 `1.0.0.0`。

{{site.alert.note}}

  Microsoft Store apps are not allowed to have a
  Version with a revision number other than zero.
  Therefore, the last number of the version must
  remain zero for all releases.
  Ensure that you follow Microsoft's
  [versioning guidelines][windowspackageversioning].

  請注意，Microsoft Store 的應用程式不允許出現
  有一個修訂版本號（第四段）不為零的版本。
  因此，在所有的版本中，版本的最後一個數字必須保持為零。
  請注意遵循微軟的 [版本指南][windowspackageversioning]。 

{{site.alert.end}}

For apps not published to the Microsoft Store, you
can set the app's executable's file and product versions.
The executable's default file version is `1.0.0.1`,
and its default product version is `1.0.0+1`. To update these,
navigate to the `pubspec.yaml` file and update the
following line:

```yaml
version: 1.0.0+1
```

The build name is three numbers separated by dots,
followed by an optional build number that is separated
by a `+`. In the example above, the build name is `1.0.0`
and the build number is `1`.

The build name becomes the first three numbers of the
file and product versions, while the build number becomes
the fourth number of the file and product versions.

Both the build name and number can be overridden in
`flutter build windows` by specifying `--build-name` and
`--build-number`, respectively.

{{site.alert.note}}
  Flutter projects created before Flutter 3.3
  need to be updated to set the executable's version
  information. For more information,
  refer to the [version migration guide][].
{{site.alert.end}}

## Add app icons

## 新增應用圖示

To update the icon of a Flutter Windows
desktop application before packaging use the
following instructions:

在打包前更新 Flutter Windows 桌面應用程式的圖示：

1. In the Flutter project, navigate to
   **windows\runner\resources**.

   導航到 Flutter 專案中的 **windows\runner\resources** 目錄。

2. Replace the **app_icon.ico** with the wanted icon.

   替換 **app_icon.ico** 為想要的圖示。

3. If the name of the icon is other than **app_icon.ico**, proceed to
change the **IDI_APP_ICON** value in the **windows\runner\Runner.rc** file to
point to the new path.

   如果圖示檔案的名稱不是 **app_icon.ico**，
   請將 **windows\runner\Runner.rc** 檔案中的 **IDI_APP_ICON** 值指向新的路徑。

When packaging with the [`msix` pub package][msix package], the logo path can
also be configured inside the `pubspec.yaml` file.

在使用 [`msix` pub package][msix package] 打套件時，
可以在 `pubspec.yaml` 檔案中配置 logo 路徑。

To update the application image in the Store listing,
navigate to the Store listing step of the submission
and select Store logos.
From there, you can upload the logo with
the size of 300 x 300 pixels.

要更新商店列表中的應用程式圖示，請導航到提交的商店列表並選擇商店 logo。
在那裡可以上傳尺寸為 300 x 300 畫素的圖片。

All uploaded images are retained for subsequent submissions.

所有上傳的圖片將被保留，以便於以後提交使用。

## Validating the application package

## 驗證應用程式包

Before publication to the Microsoft Store,
first validate the application package locally.

在釋出到微軟商店之前，建議先在本地驗證應用程式套件。

[Windows App Certification Kit][windowsappcertification] is a tool that is 
included in the Windows Software Development Kit (SDK).

[Windows 應用認證工具套件][windowsappcertification] 是一個包含在 Windows 軟體開發工具套件（SDK）中的工具。

To validate the application:

驗證應用程式：

1. Launch Windows App Cert Kit.

   啟動 Windows 應用認證工具套件。

2. Select the Flutter Windows Desktop package (**.msix**, **.msixbundle** etc).

   選擇 Flutter Windows 桌面應用程式套件（**.msix**、**.msixbundle** 等）。

3. Choose a destination for the test report.

   選擇測試報告的輸出目錄。

The report may contain important warnings and information, 
even if the certification passes. 

即使認證透過，報告也可能包含重要警告和資訊。

[azureadassociation]: https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center
[cmworkfloweditor]: https://docs.codemagic.io/flutter-publishing/publishing-to-microsoft-store/
[cmyaml]: https://docs.codemagic.io/yaml-publishing/microsoft-store/
[codemagic]: https://codemagic.io/start/
[microsoftstore]: https://www.microsoft.com/store/apps/windows
[msidocs]: https://docs.microsoft.com/en-us/windows/win32/msi/windows-installer-portal
[microsoftpartner]: https://partner.microsoft.com/
[msix package]: {{site.pub}}/packages/msix
[msix packaging]: {{site.url}}/development/platform-integration/windows/building#msix-packaging
[partnercenterapi]: https://docs.microsoft.com/azure/marketplace/azure-app-apis
[storepolicies]: https://docs.microsoft.com/windows/uwp/publish/store-policies/
[visualstudiopackaging]: https://docs.microsoft.com/windows/msix/package/packaging-uwp-apps
[visualstudiosubmission]: https://docs.microsoft.com/windows/msix/package/packaging-uwp-apps#automate-store-submissions
[windowspackageversioning]: https://docs.microsoft.com/windows/uwp/publish/package-version-numbering
[windowsappcertification]: https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit
[version migration guide]: {{site.url}}/development/platform-integration/windows/version-migration
