Protecting the security of

Remebering passwords, dealing with two-factor authentication,
answering security questions, having our credit cards frozen. These
are all a feature of everyday lives.

Our digital accounts are under constant attack.

Huge security breaches, from the Equifax breach (estimated XX social
security numbers and other personal information stolen), two security
breaches at Yahoo (500 million and over 1 billion accounts
compromised, respectively),


Whether you realize it or not, it is more than likely your own
personal information has been compromised. And that is only
considering the security breaches that we know about.


Castle is a cybersecurity startup that provides account security for
businesses that operate web or mobile apps. Castle monitors the users
of those apps, and looks for suspicious activity that may suggest that
a user's account may have been taken over. If they notice something
suspicious they send a recommendation to the customer that the user's
account should be frozen, or that the user should be challenged in
some way -- perhaps by asking a security question.

For my project I consulted with Castle to look into different ways to
approach the problem of identifying account compromise. A key problem
that we were interested in is, how can feedback data -- data about
whether or not a user account that had been flagged as suspicious
turned out to actually have been compromised or not -- be used to make
account security smarter over time.

Broadly speaking, there are a number of challenges in detecting
account compromise. One is the needle-in-a-haystack effect: the number
of uncompromised accounts will generally be dramatically higher than
the number of compromiesd accounts.

A second difficulty has to do with the lack of feedback data. For
young companies the range of legitimate user behavioral patterns will
not be known, so this becomes an unsupervised classification
problem. Over time, of course, a company can build up feedback that
can be used to train their model -- but since Castle itself is a
recent startup, and because at this point they have only a limited
amount of feedback data, currently their model is largely
unsupervised.

What are the possible ways to detect compromised user accounts in an
unsupervised fashion? A straightforward, common-sense approach would
be to apply a set of rules and heuristics to whatever features of user
activity are being logged. For instance, if a user is suddenly logging
in using a new device, or is logging in from a new country, how should
you handle that situation? What if the user is logging in using a new
device *and* from a new country? For a completely unsupervised
approach, we can imagine making common-sense decisions to account for
each case.

**** decision slide + caption

This approach, while a good (and probably necessary) starting point,
has many limitations. These include:

 * Since this type of rule-based system would (at least initially) be
set up by hand, it is difficult to imagine how to optimize it in an
algorithmic way once feedback data become avaible.

 * This is not flexible -- related to the previous point, this type of
   system is not very flexible or generalizable. Castle, which has
   multiple customers with differing user bases -- for instance, some
   customers may have users spread throughout the world whereas others
   may be highly localized -- it's important to have a model that can
   be applied and optimized in a more general way.


 * Only works for categorical variables. Although most of the types of
   information that Castle deals with are categorical (e.g. type of
   device used), in principle there may be continuous features that
   are relevant (e.g. the geographical distance between login events).

 * Prior information -- in some cases larger patterns may appear, for
   instance that many accounts are being attacked from a certain
   geographical example. We would like to take this information into account 

 * Doesn't give probability estimates

Once even a fairly small amount of data are available on user behavior
patterns, it is possible to use standard outlier detection techniques
-- for instance using XX. But the data considered by Castle is
primarily categorical (e.g. the type of device being used) ...

As a first step toward taking a more flexible approach, we can
formulate a rule-based system as a linear model:

'''
P = w_1*f_1 + w_2*f_2 + ...
'''

where each *w* term is a weight, and each *f* term is a binary feature
(i.e. if the user is logging in using a new device). The parameter *P*
can be interpreted as a score, and thresholds can be set, e.g. an
account can be frozen if P>0.9 or a user can be challenged if
0.5>P>0.9. The features and the weights can be chosen to closely
mimic, or even possibly to exactly reproduce, the rules-based system
described above. This formulation has the signficant advantage








Binary / categorical data
 findwhoflexfits  findwhoflexfits
Different customers and different users have different patterns.

This will lead to a high false-positive rate. That may not be a problem though, and some customers may actually appreciate it. In principle at least this is something that can be tested observationally, or experimentally.

Why choose logistic regression?
