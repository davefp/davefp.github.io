---
comments: true
date: 2012-06-16 16:15:38
layout: post
slug: regular-expressions-that-arent
title: Regular Expressions That Aren't
wordpress_id: 1049
categories:
- rants
tags:
- regex
---

Today there appeared an article on [Reddit](http://www.reddit.com/r/programming) titled '[The true power of regular expressions](http://nikic.github.com/2012/06/15/The-true-power-of-regular-expressions.html)'. The author goes into quite a lot of detail explaining that 'modern' regular expression engines are much more powerful than their traditional counterparts and finishes up by saying that they're NP-complete. What?



The trouble with all the assertions made in the article is this: The author isn't talking about true regular expressions for the bulk of the time. To his credit he makes this point early in the piece:


> When programmers talk about “regular expressions” they aren’t talking about formal grammars. They are talking about the regular expression _derivative_ which their language implements. And those regex implementations are only very slightly related to the original notion of regularity.


However, calling your article 'The true power of regular expressions' makes the CS graduate part of me cry. Classification IS important, and the expressions required for the subsequent examples in the post are definitely not regular. Saying 'you can totally parse HTML with regular expressions' and the like is misleading and I guarantee that if you go round telling people that it's ok to do this they'll create some monstrous code.

So what's to be done? The capabilities presented in the article aren't false, but the continued classification of the engines involved as 'regular expressions' is. Reddit user [one-half](http://www.reddit.com/user/one-half) makes this excellent comment:


> "Regular expressions" need to be renamed. They're no longer regular, which annoys the CS folks. Keeping the same name just serves to confuse the issue. Calling them "EREs" or "PCREs" is no better, because it still pretends they are basically regular expressions. Start calling the modern variant Compact Language Descriptions or something like that. - [link](http://www.reddit.com/r/programming/comments/v3en5/the_true_power_of_regular_expressions/c512tqy)


I think he's right. Regular expressions, by definition, parse regular languages. If you can do more than that with them then you should start calling them as such. Is this going to happen? Probably not. But writing misleading and provocative titles doesn't help, so kindly knock it off.
