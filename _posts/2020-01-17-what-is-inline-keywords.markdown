---
layout: post
title: "inline function?"
date: 2020-01-17 12:51:20 +0900
description: inline function에 대한 고찰 # Add post description (optional)
img: inline-fun.jpg # Add image post (optional)
---

고차함수(high-order-function)을 구현하다가 모르는 키워드를 검색하게 되면 열에 아홉은 나오는 키워드 inline에 대해 조금 더 자세히 알아가기위해 포스팅 하였습니다.

## inline 함수?

보통 고차함수라고 함은 파라미터를 함수로 가지는 함수를 말합니다.
ex)
{ % highlight kotlin % }
private fun setReactiveButton(isCart: Boolean, reactButton: (addCart: Button, buyButton: Button) -> Unit) {
when (isCart) {
true -> {
...
}
false -> {
...
}
}
}
{ % endhighlight % }


위의 예제와 같이 reactButton 파라미터는 함수로 가지고있어 setReactiveButton 고차함수가 됩니다.

그 고차함수를 사용하는 loadReactiveButton() 입니다.


{ % highlight kotlin % }
private fun loadReactiveButton() {
setReactiveButton(intent.getBooleanExtra(ADD_CART, false)) { addCart, buyButton ->
anything...
}
}
{ % endhighlight % }

이 때 setReactiveButton를 사용하는 함수 loadReactiveButton()를 디컴파일 해보면 다음과 같이 코드가 생성됩니다.

{ % highlight kotlin % }
private final void loadReactiveButton() {
this.setReactiveButton(this.getIntent().getBooleanExtra("add-cart", false), (Function2)(new Function2() {
// $FF: synthetic method
         // $FF: bridge method
public Object invoke(Object var1, Object var2) {
this.invoke((Button)var1, (Button)var2);
return Unit.INSTANCE;
}
}
}
{ % endhighlight % }

이처럼 고차함수 즉 파라미터로 함수를 function으로 많이 받을 경우 Function 오브젝트가 계속해서 생겨나게됩니다.
이를 해결하고자 짧은 고차함수 코드는 inline 키워드를 붙히면 Function 오브젝트가 따로 생겨나지않고 그 코드 그대로 들어가게 됩니다.
아래 inline을 사용한 디컴파일 코드를 보겠습니다.


{ % highlight kotlin % }
private final void loadReactiveButton() {
boolean isCart$iv = this.getIntent().getBooleanExtra("add-cart", false);
      int $i$f$setReactiveButton = false;
Button var10000;
Button var10001;
Button buyButton;
Button addCart;
boolean var7;
if (isCart$iv) {
         var10000 = (Button)this._$_findCachedViewById(id.nav_order_btn);
Intrinsics.checkExpressionValueIsNotNull(var10000, "nav_order_btn");
var10001 = (Button)this._$_findCachedViewById(id.add_cart_btn);
         Intrinsics.checkExpressionValueIsNotNull(var10001, "add_cart_btn");
         buyButton = var10001;
         addCart = var10000;
         var7 = false;
         addCart.setText((CharSequence)this.getString(-1900565));
         addCart.setOnClickListener((OnClickListener)(new ProductOptionDialog$loadReactiveButton\$$inlined$setReactiveButton$lambda$1(this)));
buyButton.setText((CharSequence)this.getString(-1900358));
buyButton.setOnClickListener((OnClickListener)(new ProductOptionDialog$loadReactiveButton$$inlined$setReactiveButton$lambda$2(this)));
} else if (!isCart$iv) {
         var10000 = (Button)this._$_findCachedViewById(id.add_cart_btn);
Intrinsics.checkExpressionValueIsNotNull(var10000, "add_cart_btn");
var10001 = (Button)this._$_findCachedViewById(id.nav_order_btn);
         Intrinsics.checkExpressionValueIsNotNull(var10001, "nav_order_btn");
         buyButton = var10001;
         addCart = var10000;
         var7 = false;
         addCart.setText((CharSequence)this.getString(-1900565));
         addCart.setOnClickListener((OnClickListener)(new ProductOptionDialog$loadReactiveButton\$$inlined$setReactiveButton$lambda$3(this)));
buyButton.setText((CharSequence)this.getString(-1900358));
buyButton.setOnClickListener((OnClickListener)(new ProductOptionDialog$loadReactiveButton$$inlined$setReactiveButton$lambda$4(this)));
}

}


{ % endhighlight % }

이처럼 따로 Function 오브젝트가 생겨나지 않고 파라미터로 넘겨준 함수가 그대로 해당 함수에 스며들어 모든코드가 들어있습니다.

처음엔 Function 오브젝트가 생성되지 않아 모든 고차함수에는 inline을 적용시키면 좋은거 아닌가? 라고 생각을 했지만 여러 구글 검색끝에 inline함수는 비용이 높아 모든 고차함수에 inline을 넣게되면 비용이 상당하다는 것을 알게되었습니다.

ex)
![인라인함수1](../assets/img/inline-fun-first.png)
![인라인함수2](../assets/img/inline-fun-second.png)
![인라인함수3](../assets/img/inline-fun-third.png)

이와같이 inline함수는 코드의 길이가 상당히 급격하게 길어지게 됩니다. 디컴파일하기 전 코드가 짧은 함수에만 사용하는것을 권장하는듯 했습니다.

noinline은 inline 키워드를 사용했지만 인라인을 사용하고싶지 않을 때 정하는 키워드입니다.

crossinline은 inline 키워드를 사용한 함수 몸체에서 해당 고차함수를 inline카워드를 붙힌 함수 몸체에서 직접적으로 호출하지 않고 inline 함수 몸체에서 다른 실행 컨텍스트(로컬객체, 중첩함수) 에서 호출이 될때 crossinline 키워드를 사용한다.


후에 inline에 대해 여러 지식을 습득하게 되면 추가로 포스팅 하겠습니다.

이상 inline 함수 포스팅을 마치도록 하겠습니다.
