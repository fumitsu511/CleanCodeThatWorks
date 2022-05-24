# 宣言的な記載とFacadeパターン

## 1. 何を意識してプログラミングをするか

コードにおいて重要なことは何か。
それは**仕様通りの動作**をすることである。  
ところが、数か月後まで意識を向けてみると、保守性の高いコードであることが重要な要素であることがわかる。  
今回は、保守性の高いコードを書くための一歩目としてのマインドについて記載する。  
それは「**ソースコードはドキュメントである。**」ということである。  
ソースコードを読むのは誰か？まずはコンパイラ...そして保守をする人である。  
私たちは、お客様に説明する資料や、設計書は分かりやすくことを書くことを意識する。  
同様に、読み物であるソースコードも分かりやすく記載することに妥協をしてはいけないのではないか。
<br>
<br>

## 2. 宣言的な記載とFacadeパターン

それでは、どうやってソースコード分かりやすく記載するか。
その一歩目は、Why,What,Howを意識して記載をすること。

- コメントにWhy
- 左辺にWhat
- 右辺にHow

を記載することを意識する。  

パターン①　（良くない例）Mainのメソッドに演算や分岐が乱立している。
```
public string Service(payAmount){

    //残高を取得する。
    int balance = getBalance();
    //残高 - 支払額
    int newBalance = balance - payAmount;
    
    if(newBalance < 0){
        return "残高が足りません。"
    }
    
    return $"支払いが完了しました。残りの残高は{newBalance}です。" 

}

```
パターン②　（良い例）Mainのメソッドをシンプルにして、プライベート変数に処理を移譲している。
```
public string Service(int payAmount){

    int balance = getBalance();
    int newBalance = Payment(balance, payAmount);
    
    //画面表示のために、残高を表示するためのメッセージを返却する。
    return CreatePaymentMessage(newBalance);
    
}


private int Payment(int balance, int payAmount){
    return balance - payAmount; 
}

private string CreatePaymentMessage(int newBalance){
    if(newBalance < 0){
        return "残高が足りません。"
    }

    return $"支払いが完了しました。残りの残高は{newBalance}です。" 
}

```

パターン①とパターン②では、Main処理であるServiceメソッドだけ見ると、
パターン②の方が理解をしやすい。

その理由は、パターン①はService処理内に、計算式やif文が入っており、
ソースコードを理解しなければ、Serviceが何をしているのかを理解できない状態になっている。

一方で、パターン②は、左辺にWhat、右辺にHowが書いてあるため、
Service処理が何をしているのかを大まかに把握することが容易である。

1. まず、balance(残高)を取得している。
2. 次に、balanceと引数のpayAmount(支払額)を使用して、newBalance(新しい残高)を取得している。
3. newBalanceを使用して、PaymentMessageを作成して返却している。

という大まかな内容が、すぐに理解できる。
パワーポイントで資料を作成する際に、まず目次を作り、全体を理解させてから、
内容を説明することで、わかりやすい発表にする。という考え方と同じである。
左辺にWhat、右辺にHowを書くことによって、ソースコードを目次と詳細のように、分けて記載することができる。
この方法をパターン化した記載方法をFacadeパターンという。(ファサードパターン)

```
public outDto Service(){

    var keiyakuJoho = GetKeiyakuJoho();
    var seikyuJoho  = GetSeikyuJoho();
    
    return new outDto(){
        name = keiyakuJoho.name,
        bill = seikyuJoho.BillingAmount
    }
}

```

左辺にvar(型推論)を利用することで、左辺の記載が揃うので、よりMainの関数が読みやすくなる。

## 3. まとめ

1. コメントにWhy、左辺にWhat、右辺にHow を記載する。
2. Facadeパターンを使ってみる。(Main関数を目次のようにして、Main関数から呼び出した関数に詳細を記載する。)
3. ※補足　var(型推論)を使って左辺を読みやすくする。

