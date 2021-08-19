# ハイブリッドID (hybrid identity)

■ハイブリッドIDとは？

- 共通のユーザーIDで、オンプレミスとクラウドの両方のアプリにアクセスできるしくみ。
- Azure AD Connectを使用して実現できる。
- ライセンス: Freeで利用可能。
  - テナントで扱うオブジェクト数が500000を超える場合は、P1/P2を有効化したテナントが必要
  - パスワードライトバック(SSPRのページで説明)はユーザーごとにP1が必要

■Azure AD Connect

https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/whatis-azure-ad-connect

オンプレミスAD DSとAzure ADを同期するソフトウェア。オンプレミスのWindows Serverにインストールする。

```
Azure AD
↑同期
オンプレミス
├ Windows Server + Azure AD Connect
└ Windows Server（AD DS機能を追加）
```

詳しくは[次のページ](mod02-02-connect.md)で説明。

■同期

Azure AD Connectを使用して、オンプレミス側のID情報をAzure ADへ伝達すること。

同期は、Azure AD Connectに含まれる「[Azure AD Connect Sync](https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/how-to-connect-sync-whatis)」コンポーネントで実行される。

**同期は、原則として、オンプレミスAD DS → Azure AD の方向で行われる**。

Azure ADに登録されたID情報が、オンプレミスAD DSに反映されることは**ない**。

```
Azure AD
↑
Azure AD Connect
↑同期
オンプレミスAD DS
└ユーザーID
```


※パスワード ライトバックはこの原則の例外。

■パスワード ライトバック

「[パスワードライトバック](https://docs.microsoft.com/ja-jp/azure/active-directory/authentication/tutorial-enable-sspr-writeback)」を設定すると、Azure AD側のパスワード変更を行った際、オンプレミスAD DS側に反映させることができる。

```
Azure AD ユーザーがパスワードを変更
↑      ↓ 変更後のパスワード
Azure AD Connect
↑同期  ↓
オンプレミスAD DS
```

使用するユーザーごとにP1またはP2ライセンスが必要。

サポートチームによる解説
https://jpazureid.github.io/blog/azure-active-directory-connect/password-writeback-overview/

■3つの認証方式

https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/whatis-hybrid-identity

Azure AD Connectのインストール中に、認証方式を選択する。

- パスワードハッシュ同期(Passsword Hash Sync: PHS)
  - Azure 側に、パスワードのハッシュ値を保存する方式。
- パススルー認証(Pass Through Auth: PTA)
  - Azure 側で入力されたID/パスワードをオンプレミスのAD DSに送信して検証する方式。
- フェデレーション統合(Federation, fed)
  - オンプレミスの「AD FS」や「PingFederate」に、認証プロセスを引き継ぐ方式。

[まとめPDF](../AZ-500/pdf/mod1/Azure%20AD%20Connect.pdf)

■参考: フェデレーションとは

https://www.atmarkit.co.jp/fwin2k/operation/adfs2sso03/adfs2sso03_01.html

- オンプレミスに 以下をデプロイ
  - AD DS
  - 「Active Directory Federation Service (AD FS) 」または 「PingFederate(ピンフェデレート）」
- AD DSを使用して、IDを管理
- AD FSまたはPingFederateを使用して、クラウドアプリケーションへのシングルサインオンを実現


■参考: PingFederate(ピンフェデレート）とは

https://www.ntt.com/business/services/application/authentication/idf/pingfederate.html

- シングルサインオン製品
- オンプレミスのAD DSと連携できる
- Azure AD Connect でサポートされている。

■3つの認証方式の選択

https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/whatis-hybrid-identity

- Azure 側にパスワードハッシュ値を保存してもよい場合は、**パスワードハッシュ同期(PHS)** を使用。
- Azure 側にパスワードハッシュ値を保存したくない場合は、**パススルー認証(PTA)** または **フェデレーション統合** を使用
  - オンプレミスの AD DS のセキュリティとパスワード ポリシーを適用する必要がある場合は、**パススルー認証** を使用
  - オンプレミスに Active Directory Federation Service (AD FS) がデプロイされていて、引き続きそれを使用したい場合、オンプレのサードパーティMFAソリューションを使う場合、スマートカード認証をサポートするなどは、**フェデレーション統合** を使用。

■認証方式1: パスワードハッシュ同期 (Password hash synchronization, PHS)

https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/whatis-phs

- パスワードのハッシュ値を同期
- ダークウェブ等に漏洩した資格情報の検出にも役立つ

※参考: ハッシュ値

```
パスワード
 ↓ ハッシュ関数
ハッシュ値
```

ハッシュ値からパスワードを逆算することはできない.

Azure側には、生パスワードではなく、ハッシュ値が保存される。

■認証方式2: パススルー同期 (Pass-through authentication, PTA)

https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/how-to-connect-pta

- Azure ADに送信されたパスワードを、オンプレミスAD DSに送信して検証
- オンプレミスのインフラストラクチャに障害が発生した場合に、パスワード ハッシュ同期へフェイルオーバー（切り替え）することもできる
  - 自動でフェイルオーバーはしない。手動で切り替えが必要

■認証方式3: フェデレーション (federation)

https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/whatis-fed

- オンプレミスの「AD FS」や「PingFederate」とAzure ADでフェデレーションを構成
- オンプレミスの「AD FS」や「PingFederate」に、認証プロセスを引き継ぐ仕組み
- オンプレのサードパーティMFAソリューションを使う場合や、スマートカード認証をサポートするなど、高度な機能を利用できる
- AD FSで障害が発生した場合に備えて、[PHSを組み合わせる](https://docs.microsoft.com/ja-jp/azure/active-directory/hybrid/tutorial-phs-backup)こともできる
