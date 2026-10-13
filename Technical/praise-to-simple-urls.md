# Praise to simple URLs

It can't get simpler than this!

We're shopping for microwave oven and ended up deciding on a particular brand for its familiarity and closeness to a physical store+service center.

I took note of the model no.s by reaching out to relatives and consulting Reddit[1]. Just as I was about to share the info over Signal app to my family, I wanted to bring in the URLs to mae it complete.

IFB 30FRC2 - https://www.ifbappliances.com/30brc2
IFB 30BRC2 - https://www.ifbappliances.com/30frc2

mind blown! That's as simple as it can be. Isn't it?

There are downsides to this URL scheme, which we'll get to shortly, but let's take some time to investigate a few other players. My goal is to determine whether they make me go to tinyurl etc. because reasons (!) or are simple enough that I can hand-code the URL in any message

LG MC2846BV - https://www.lg.com/in/microwave-ovens/convection-microwave/mc2846bv/ -- Not so bad. There is a pattern here with region identifier, product line, product-type and then the product identifier.

Samsung MC28A5145VR/TL - https://www.samsung.com/in/microwave-ovens/convection/mw5100h-mc28a5135ck-mc28a5145vr-tl/ -- region identifier , product line along with product type and then woah, what is this?

Midea EM720C2GS - https://www.midea.com/sa/microwave-oven/solo-microwave-oven/20l-solo-microwave-oven-with-membrane-control.em720c2gs -- a region identifier (South Africa) but change it to 'in' (for India, yes, this holds in this website), you get a 404 (but the product is marketed in India!). The product line, product type follow and then the abomination! Why is there a dot again?

Enough, let's get to the downsides:

1. Be careful when you try this with sensitive stuff! Say, you log into your bank website, access your account no. 52738 and the URL is www.bank.com/52738 where you see balance, make transactions etc. How does the person with account no. 52739 feel if you just change the URL😂😂 and land in their account?

Now, the authentication etc. can certainly take care of the disallowed access part but again your bank can have the URL scheme rather as www.bank.com/account alone and never expose the actual identifier

Any other downsides to this 'Clean URL' obsession?

References:
1. https://www.reddit.com/r/DesiKitchenGear/s/JDfbnYlz47

