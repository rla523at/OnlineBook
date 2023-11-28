# Derivatives
`파생상품(derivatives)`이란 주식과 채권 등 전통적인 금융상품을 기초자산으로 하여 기초자산의 가치변동에 따라 가격이 결정되는 금융상품을 말한다.

> Reference  
> [시사경제용어사전](https://www.moef.go.kr/sisa/dictionary/detail?idx=2718)

## Futures
`선물(futures)`거래란 장래 일정 시점에 미리 정한 가격으로 매매할 것을 현재 시점에서 약정하는 거래로, 미래의 가치를 사고 파는 것이다.

미리 정한 가격으로 매매를 약속한 것이기 때문에 가격변동 위험의 회피가 가능하다는 특징이 있다.

> Reference  
> [시사경제용어사전](https://www.moef.go.kr/sisa/dictionary/detail?idx=1437)

## Option
선택할 수 있는 권리를 뜻하는 ‘`옵션(option)`’이 파생금융시장에서 사용될 경우, 살 수 있는 권리를 `콜옵션(call option)`, 팔 수 있는 권리를 `풋옵션(put option)`이라고 한다.

### Call option
매입자가 매도자로부터 만기일 또는 그 이전에 미리 정한 권리strike price으로 대상자산을 매입할 수 있는 권리이다. 

콜옵션을 매입한 사람은 옵션의 만기 내에 `약정한 가격(Strike Price)`으로 해당 기초자산을 구매할 수 있는 권리를 갖게 되고, 콜옵션을 매도한 사람은 매입자에게 기초자산을 인도해야 할 의무를 갖는다. 

그런데 옵션은 선물과 달리 권리만 있고 의무가 없으므로 매입자는 해당 옵션을 매도한 사람에게 일정한 `대가(premium)`를 미리 지불해야 한다. 

옵션 매입자의 현재가격이 strike price보다 높을 경우 매입자는 권리를 행사함으로써 그 차액만큼 이익을 얻을 수 있으며, 현재가격이 strike price보다 낮을 경우에는 권리행사를 포기할 수 있다. 

따라서 가격상승 정도에 따라 매입자의 이익은 확대될 수 있으며, 가격이 하락하더라도 손실을 계약 당시에 지급한 premium에 한정시킬 수 있다. 

또 옵션 매도자의 손익은 현재가격이 strike price보다 낮을 경우 매입자가 권리행사를 포기하게 되므로 이미 지불받은 premium만큼 이익이 발생하지만, 현재가격이 strike price보다 높을 경우에는 가격수준에 관계없이 기초자산을 strike price으로 인도해야 하므로 가격상승 정도에 따라 큰 손실을 감수해야 한다.

> Reference  
> [시사경제용어사전](https://www.moef.go.kr/sisa/dictionary/detail?idx=2583)

#### 예시
현재 5만원인 주식이 있다고 하자.

call option 매입자 A는 1년 뒤 주식이 10만원으로 오를 것이라고 예상한다. 따라서, A는 2만원의 premium을 지불하여 1년 뒤 6만원에 주식을 구입할 수 있는, strike price가 6만원인 call option을 매입하려고 한다.

이 떄, call option 매도자 B는 1년 뒤에도 주식이 5만원일것 이라고 예상한다. 따라서, B는 2만원의 premium을 받는것이 이득이라고 생각한다. 

따라서 B는 A에게 주당 2만원의 premium을 받으며 1년 뒤에 5만원에 주식을 살 수 있는 권리인 call option을 매도한다.

만약 A의 예상대로 주식이 10만원이 됐다고 해보자. A는 strike price인 주당 6만원을 주고 B에게 주식을 달라고 call option의 권리를 행사하게 된다. 그러면 A는 미리 지급한 premium 값을 제외한 주당 2만원의 수익을 얻게 된다. 이 때, B는 10만원짜리 주식을 strike price인 6만원에 팔아야 함으로 주당 4만원의 손해를 보게 된다. 

이번에는 B의 예상대로 주식이 5만원이 됐다고 해보자. call option에는 권리만 있고 의무가 없기 때문에 A는 5만원짜리 주식을 strike price인 6만원에 살 권리를 포기하면 된다. 이 때 A는 미리 지급한 premium값을 손해보게 됨으로 주당 2만원의 손해를 보게 된다. 반대로 B는 A가 미리 지급한 premium값을 이득보게 됨으로 주당 2만원의 이득을 보게 된다.

따라서 이론적으로 call option 매수자의 이익은 무한대이고 손해는 premium에 한정되 있는 상품이다. 위와 반대로 call option 매도자는 이익은 premium에 한정되고 손실은 무한대가 되는 상품이다. 

> Reference  
> [더밸류뉴스](http://www.thevaluenews.co.kr/news/view.php?idx=4017&mcode=msub1)  

### Put option
Put option은 거래 당사자들이 미리 정한 가격으로 장래의 특정시점 또는 그 이전에 특정 대상물을 팔 수 있는 권리를 매매하는 계약이다. 

Put option 매입자는 시장에서 해당 상품이 사전에 정한 가격보다 낮은 가격에서 거래될 경우, 그 권리를 행사함으로써 비싼 값에 상품을 팔 수 있다. 

Put option 매입자는 옵션의 만기 내에 strike price로 해당 기초자산을 판매할 수 있는 권리를 갖게 되고, put option 매도자는 strike price에 기초자산을 매입자로부터 인도해야 할 의무를 갖는다. 

그런데 option은 futures와 달리 권리만 있고 의무가 없으므로 매입자는 해당 옵션을 매도한 사람에게 일정한 premium을 미리 지불해야 한다. 

put option 매입자는 현재가격이 strike price보다 낮을 경우 권리를 행사함으로써 그 차액만큼 이익을 얻을 수 있으며, 현재가격이 strike price보다 높을 경우에는 권리행사를 포기할 수 있다. 

따라서 가격하락 정도에 따라 매입자의 이익은 확대될 수 있으며, 가격이 상승하더라도 손실을 계약 당시에 지급한 premium에 한정시킬 수 있다. 

반면 put option 매도자는 현재가격이 strike price보다 높을 경우, put option 매수자가 권리행사를 포기하게 되므로 이미 지불받은 premium만큼 이익이 발생하지만, 현재가격이 strike price보다 낮을 경우에는 기초자산을 strike price로 구입해야 하므로 가격하락 정도에 따라 큰 손실을 감수해야 한다.

## Cap & Floor

Cap은 금리의 상한선을 정하는 계약이고 Floor은 금리의 하한선을 정하는 계약이다.

> Reference  
> [Blog](https://m.blog.naver.com/playmobile1/221909855416)