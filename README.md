# AzureFirewallRule-Structured-ForSentinel
> 目的
本レポジトリは Microsoft Sentinel で提供されている Azure Firewall のテンプレートルール (Analytics Rule) を　Structured 形式に対応した形式に書き換えた分析ルールサンプルを提供しています。

AzureFirewall 構造型ファイアウォールログとは
 - 構造化ログ (Structured Log) は、特定の形式で構成されたログデータの一種です。これまでの AzureFirewall ログは AzureDiagnostic テーブルに含まれていたため、5-tuples (送受信 IP, ポート, プロトコル) などの情報は AzureDiagnostic テーブルのカラムをユーザー側で正規化して抽出する作業が必要でした。<BR>
 - Azure Firewall の構造化ログでは、ネットワークルール、アプリケーションルール専用に Log Analytics ワークスペースのテーブルが作成され、標準で送信/宛先の IP アドレス、プロトコル、ポート番号、ファイアウォールによって実行されたアクションなどの情報が含まれます。また、イベントの時刻や Azure Firewall インスタンスの名前など、さらに多くのメタデータも含まれます。
 - Microsoft Sentinel には AzureFirewall 向けのテンプレートルールが提供されていますが、2023.7.1 現在 Structured Log 形式のルールが提供されていません。本レポジトリは有志で　Structured Log 向けのテンプレートルールを変換しています。
<img width="872" alt="image" src="https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/assets/55295601/cb247e61-713d-4b1d-8abc-a4ad185763aa">

# テンプレートルールリスト
> Microsoft Sentinel で提供されているテンプレートルールのマッピング表です

|  テンプレートルール名  | リンク |
| ---- | ---- |
| TI map IP entity to AzureFirewall | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/TImapIPentitytoAzureFirewall.json) |
| Several deny actions registered | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/Several%20deny%20actions%20registered.json) |
| Multiple Sources Affected by the Same TI Destination | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/Multiple%20Sources%20Affected%20by%20the%20Same%20TI%20Destination.json) |
| Port Sweep | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/PortSweep.json) |
| Abnormal Deny Rate for Source IP | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/AbnormalDenyRateforSourceIP.json) |
| Abnormal Port to Protocol | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/AbnormalPortProtocol.json) |
| Port Scan | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/PortScan.json) |

# サポートなど
> 不具合や問題があれば、Issues からご連絡下さい。

# 免責事項
> 本レポジトリの使用における Microsoft Sentinel の課金はユーザー側責任となります。

- 本レポジトリのコンテンツによって発生するコストについては、利用するユーザーが責任を負います。
- 本レポジトリのコンテンツによって作成される環境から出力される内容について、作成者は責任を負いません。
- 本レポジトリはオープンソースです。 

