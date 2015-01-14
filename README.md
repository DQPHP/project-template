# Chef

## Init

hosts:
```shell
192.168.33.5    cinra.dev
```

```
vagrant up
vagrant provision
```

## Dependencies

- Virtual Box
- Vagrant
- Chef
- Chef Solo

## Usage



## Issues

### Guest Additions対策

結構厄介な問題：未だ未解決。取り急ぎ現状

1. Guest Additionのバージョンを合わせなくてはならない → そのためのvagrant pluginがvagrant-vbguest
2. vagrant vbguestが動かない：kernelが必要。しかしkernel-develがインストール出来ない。 → ゲストマシンで、/etc/yum.confを編集。exclude=kernel*をコメントアウト（何故こんな設定が…？）
3. ```sudo yum install -y kernel-devel```が動くようになるので、インストール → しかし状況変わらず…。
4. kernel周りのインストール：KERN_DIRを設定しないといけない

```# rpm -qa|grep kernel```
で、インストールされたkernel-develのヴァージョンを調査して、
```# export KERN_DIR=/usr/src/kernels/2.6.32-504.3.3.el6.x86_64```
こういう感じで環境変数を設定。確認は「```# env```」で

この時点で、kernel周りのアップデート
```
# sudo yum -y update kernel
# sudo yum -y install kernel-devel kernel-headers dkms gcc gcc-c++
```

5. ~~# /etc/init.d/vboxadd setupで、virtual boxを再ビルド OR ~~```# vagrant reload```

~~# vagrant vbguest --status~~で確かめても

```
「GuestAdditions versions on your host (4.3.20) and guest (4.3.4) do not match.」
```

こんな感じで、一向に解決されない。マシンの問題？？（だと困る）

- CentOS専用になってるなあ…。