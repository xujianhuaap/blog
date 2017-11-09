---
title: android_test_espresso
date: 2017-11-01 13:53:01
tags: test
---

```
1. Espresso 判定相关概念 
	1> ViewMatcher
	ViewMatcher.withId(id);返回一个Matcher<View>
	ViewMatcher.withText(str);
	ViewMatcher.withTag(tag);
	ViewMatcher.hasBackground(int);
	ViewMatcher.hasFocus();
	ViewMatcher.hasTextColor(colorResId);
	ViewMatcher.isClickable();
	ViewMatcher.isCheckable();

	ViewMatcher.assertThat();
	2> ViewAssertion
	ViewAssertion.matches(Matcher<View>)返回一个ViewAssertion;
	
	ViewInterface.perform(ViewAction...).check(ViewAssertion);
	ViewAssertion 是一些判定，但要判定的内容是由MatcherView 来决定的
	
```
