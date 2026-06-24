---  
layout: post
title: DP-LC-32-视频讲解词 
categories: LeetCode
description: just for practice
keywords: Algorithm
---  
# 题目描述  

32. Longest Valid Parentheses   

视频地址：https://youtu.be/D5b_JWlkXxw


Hello and welcome back to the Cracking FAANG YouTube channel. Today we're going to be solving LeetCode problem number 32: Longest Valid Parenthesis. Before we get into the problem, I would just like to kindly ask you to subscribe to my channel. I have the goal of reaching a thousand subscribers by the end of May and I need your help to get there. The more subscribers I have, the more videos I can make and the easier I can make it for you guys to get into FAANG. So if you want those high‑paying jobs, please subscribe to my channel so I can make more videos and you can pass your interviews with flying colors. Alright.

Given a string containing just the characters left parentheses and right parentheses, find the length of the longest valid well‑formed parentheses substring.

So for example, if we're given this input s as left parenthesis, right parenthesis, obviously we can see that this is the longest well‑formed pair, where well‑formed means that every left parenthesis is closed by a right parenthesis which also comes after it. Right? We can't have a left parenthesis that's closed by a right parenthesis that comes before it. That's not allowed.

So we also have this case here, and we can see that the longest is going to be this string because obviously this is not valid. So this is the only portion that's valid, so that's how they get this four.

So how do you actually solve this question? And this question, despite being a hard, is actually quite easy.

What we want to do is we want to iterate through our string from left to right, and what we're going to do is we're going to maintain a left count and we're going to maintain a right count.

And every time we see a left parenthesis or right parenthesis, we're going to increment this count. And we can think of it as basically creating a sliding window of increasing size as we go through our string here. But what we need to do is because we're just taking everything greedily, if we ever get to a point that our count here indicates that we don't have a well‑formed parenthesis string, and we're going to define well‑formed as basically left being greater than or equal to right. So if this is not true, then we need to reset our current progress in the window to be zero because that string that we've taken is no longer valid since we're taking lefts and rights greedily.

So what do I mean by this? Let's go through the example and walk through it.

So we're going to go from left to right, so we see that we have a right parenthesis. So I said we're just going to increment for every one we see. So we see that right is 1 and left is 0. Obviously this is not a valid string and it can never be a valid string. Right? There is no left parenthesis that will ever close this right parenthesis. So this is invalid and we must reset our progress back down to zero.

So we can think of it as kind of like ignoring that right parenthesis. Now we get to this left parenthesis and we can say okay, you know, let's take it. So this count now becomes one, and you know we have the case where left is greater than or equal to right so we are good to continue.

Now what we want to do is we see this right, so we're going to increment the right and we're going to see that left is 1 and right equals to one. So when the two are equal like this, so when left equals to right, then we know that we have a well‑formed parenthesis string because we're also taking care of the fact that if it's not well‑formed then we're just going to reset our progress.

So anytime left equals right that must mean that we have a well‑formed string, so we can update our maximum. So currently the maximum longest string is obviously going to be 0. So now it's going to be the max of you know 0 and one, so our new maximum length is—oh sorry it's gonna be two, my bad—because obviously that's length of two here. So our max is now two. Cool.

Now we're going to continue. So we now see another, and then I'll just erase some things here to kind of clean things up. Okay so this is a one. So now we see this left parenthesis so we increment our left, and remember that left is still greater than or equal to right so we're good to continue. And since left does not equal right we can't update our maximum, which you know at the moment is two.

And then we see this right parenthesis so we can increment our right, and you know left is still greater than or equal to right, but now you know left equals to right so that means that we can update our maximum. So obviously we have a new maximum length of four. So that's it.

And you know we continue. So now we see this second, you know third right so we update this and you know now left is no longer greater than or equal to right so we must reset everything back down to zero. And you know we have finished our loop through.

Now you may think that this is actually where the problem ends and this is where you would be wrong. In this problem what we need to do is okay so we have our global maximum of four that we found going right. But what we need to do is actually we need to also go through the string going to the left. And the reason that we want to do this is because there could be some parentheses that's formed correctly when going through the left side. It could be the case that our setup here on the right leading into whatever the maximum was will not actually be the global maximum.

So we need to go both left and right. So we've already gone left through it and seen that it's four, but we also need to go right to double check that there's no better parentheses string that occurs when you're going to the left here.

In this problem it's actually just going to be the same you know maximum, so this one is actually the global maximum. But you could have the case where when going to the left you're gonna have you know a better answer than what you found when going right. So you do have to be careful and make two passes through the string: one going to the left and one going to the right.

And the one going to the left is going to follow the exact same logic. You're going to take left and rights greedily, you're going to check whether or not left count is greater than or equal to right count. If it isn't then you have to reset back down to zero, and you know if they're equal then you can update your maximum.

So that's the algorithm that we want to take. We're basically just going to do two parses through our input and check based on you know the criteria that we spoke on earlier. And at the end of that whatever your you know maximum length is is going to be the solution to your problem.

So that's how you're gonna solve this question. Let's go to the code editor and write this out, and it's actually really simple to write so let's do that. I'll see you there.

We're back in the code editor and it's time to write the code. Remember that we're gonna need to keep track of our left count, our right count, and our maximum length.

~~~python
def longestValidParentheses(s: str) -> int:
    left = right = max_len = 0
    # 左→右遍历
    for char in s:
        if char == '(':
            left += 1
        else:
            right += 1
        if left == right:
            max_len = max(max_len, 2 * left)
        elif right > left:
            left = right = 0
    # 重置计数，右→左遍历
    left = right = 0
    for char in reversed(s):
        if char == '(':
            left += 1
        else:
            right += 1
        if left == right:
            max_len = max(max_len, 2 * left)
        elif left > right:
            left = right = 0
    return max_len
~~~   
So let's set up variables for that. So we'll say left count equals right count equals to the maximum length of the longest valid parentheses, and we're going to say all this is going to be initialized to zero.

Now remember that we need to parse through our string from left to right initially and then we'll go from right to left later. So let's do the left to right first. So we'll set up you know a pointer variable and we'll do a while loop. So while i less than len(s).

What we want to do is we want to say if s[i] is equal to a left parenthesis then we want to say l_count is going to be greater than or equal to 1. Otherwise it must be a right count because the problem tells us that there's only left and right parentheses. So if it's not a left parenthesis it must be right parenthesis, so we're going to increment that right count.

Now we need to check whether or not we have a valid string. So we're going to say if l_count equals to right count then we know that we have a valid formed string so we can update our maximum. So we can say our new max length, max_len is going to be equal to the maximum of the current max and you know l_count plus right count. Cool.

Otherwise, if the right count is greater than the left count, we know that we have an invalid parenthesis string because obviously if the right count is greater then there is no left parenthesis that can come after where we currently are that will close a right parenthesis. So this string isn't valid so we need to basically reset our progress back down to zero.

So we're going to reset our progress, and the last thing that we need to do here is simply increment our i so that the while loop can continue.

Now remember that I said that we may miss the global maximum if we're simply going from left to right. What we need to do is actually go from right to left, and we will do basically the exact same thing we did here except this time since we're going in reverse there's going to be a little subtle switch that we need to do this time.

Instead of checking whether or not the right count is greater than the left count in order to reset, because we're going backwards and we're essentially doing this in reverse, we actually need to check whether or not the left count is greater than the right count. Because when you're going in reverse everything's flipped, so left parenthesis will act like right parentheses in that if we have too many left parentheses then there's no right parentheses that can close it later. So we need to basically flip this logic here to account for the fact that we're going backwards and our you know operations are mirrored.

We're still going to take you know left and right counts every time we see them, and we're still going to update our maximum if the counts are equal, but we do need to change this part.

So this is where it's a little bit tricky, but once you have that in mind it's not too bad.

So we need to before we even do any processing reset our left and right counts because we're starting fresh, and we need to set up a new pointer for our while loop. So we're going to set it to the last index of our string here.

And now we need to process our string backwards. So we're going to say while i greater than equal to 0.

What we want to do is if s[i] equals to a left parenthesis then what we want to do is we want to say left count plus equals to one. Otherwise it must be a right parenthesis so we say right count plus equals to one.

And now what we're going to do is again we're going to check whether or not we have a valid string. So we're going to say if left count equals to right count then what we're going to do is we're going to say that the maximum length is going to be the max of max_len and you know l_count plus whatever the right count is.

And now we need to perform the check whether or not we need to reset, and remember because we're going backwards and everything's technically flipped we're going to say if left count this time is greater than the right count then we need to essentially reset our progress so our count equals to zero.

So let us continue. So we're going to decrement our i so the while loop can continue.

And at the end all we need to do is simply return our maximum length here.

Let's submit this, make sure that we haven't made any mistakes, and cool it works.

What is the time and space complexity for this algorithm?

Well, what is the time complexity? We know that we parse through our strings two times, one to go left and one to go right. So technically this is going to be big O of n plus big O of n which is equal to big O of 2n which asymptotically is just big O of n.

What is the space complexity here? Well we don't define any extra data structures to store our results. All we have are these basically three pointer variables—well I guess with the i pointer so four pointers—which are just constant space allocations. So we can say that the solution is actually going to be constant space because all we define is a left count, right count, max len, and i which are you know constant space allocations.

So this is actually going to be the most optimal solution for this problem. There are three other solutions to this one but this is the most optimal. It's going to give you the fastest run time and also a constant space solution, so definitely the one that you want to go with for your interview.

And it's also quite intuitive, it's quite simple, really easy to write, there's no like weird tricks to it. I think the only tricky part is just remembering to do the left count greater than right count when going back through the string the second time. But other than that it's really straightforward, and yeah you can really easily memorize this one or you know just have it off the top of your head, it's not complicated at all. I think just knowing this one line here will basically just save you through it.

So that's how you solve this problem. I hope you enjoyed this video. If you did please leave a like, comment whatever you thought, subscribe to the channel of course. And if there's any video ideas or topics or questions that you want me to cover please leave them in the comment section below. I'll be happy to get back to you guys.

Thank you for watching, have a nice day, bye.
