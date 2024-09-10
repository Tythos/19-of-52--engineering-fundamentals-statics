Are you excited? I'm excited! Today we're going to dive into our first specific topical area in engineering. Specifically, we'll take a crack at "statics", which is a simple way of restating some basic Newtonian principles around problem solving that grow in the direction of structural engineering.

Statics isn't just about structures, as simple as most examples might look. Rather, this represents a key approach to engineering problems: First, we identify specific equilibrium points in a system. (Once those are defined, we can look at variations about those equilibrium points to learn how they can be stabilized and controlled--but that's later.)

## Forces

Recall our vector formulation of Newton's Second Law:

{% katex %}
\Sigma \vec{F} = m \frac{d^2}{dt^2} \vec{x}
{% endkatex %}

There is also a corresponding analogue for rotational dynamics, an important topic that we haven't dived into a lot yet--but there's no time like the present!

{% katex %}
\Sigma \vec{M} = \Sigma_i \vec{r}_i \times \vec{F}_i
{% endkatex %}

A "moment", in this case, you may recognize as an accumulation of torques. Torque, of course, is simply the leverage of a force about a pivot point, or centroid. We'll dive into these in more detail shortly.

### Systems

More importantly, though, is a fundamental assumption of statics (not to mention where "statics" gets its name from): Because the system is static (inertial, or non-accelerating), the forces must sum to zero. The moments, too, must sum to zero. If we repeat these constraints across all three dimensions, we have a set of six equations that can help us solve for the state of complex systems.

Each component of the force sum, of course, must add to zero separately:

{% katex %} \Sigma_i F_{xi} = 0 {% endkatex %}
{% katex %} \Sigma_i F_{yi} = 0 {% endkatex %}
{% katex %} \Sigma_i F_{zi} = 0 {% endkatex %}

Moments are trickier because the cross-product introduces a relationship across dimensions. But, recall from linear algebra that the cross product can be re-expressed as a function of the angle between the two vectors, which we will denote as `\theta`:

{% katex %}
\vec{r} \times \vec{F} = \left| \vec{r} \right| \left| \vec{F} \right| \sin(\theta)
{% endkatex %}

We can use this relationship to expand our moments-must-sum-to-zero into another three specific constraints:

{% katex %} \Sigma_i (y F_z - z F_y) + \Sigma_i (M \cos(\theta_x))_i = 0 {% endkatex %}
{% katex %} \Sigma_i (z F_x - x F_z) + \Sigma_i (M \cos(\theta_y))_i = 0 {% endkatex %}
{% katex %} \Sigma_i (x F_y - y F_x) + \Sigma_i (M \cos(\theta_z))_i = 0 {% endkatex %}

Together, we can use these constraints to encompass a wide variety of systems with the help of just a few tools. In particular, we'll often want to solve for the force or moment about specific elements within a system, like specific cables or specific pins of a truss. These are important ways to characterize the design of a system--but in statics it all comes back to the fact that the system itself is not changing over time.

### Centroids

Measuring a force is easy--especially with a known point of contact or point of analysis. Measuring a moment is a little bit trickier because it must be evaluated with respect to some centroid. In the case of a lever, this point is obvious, but in the case of a free body we need a way to compute this "centroid" purely from the geometry of the object.

Fortunately, this is just a mean (or average), and we can express that with THE POWER OF CALCULUS. But, this might look a little different depending on what geometry we are evaluating. Let's consider a linear geometry (like a beam), but to make things interesting let's consider the possibility that it may not have uniform density. Instead, there will be some function of the length along this geometry (typically mass) that we need to integrate infinitesimally-small increments of. Let `\vec{c}` be the position of the centroid; then,

{% katex %} c_x = \frac{1}{L} \int x dL {% endkatex %}
{% katex %} c_y = \frac{1}{L} \int y dL {% endkatex %}
{% katex %} c_z = \frac{1}{L} \int z dL {% endkatex %}

Typically, this comes down to two steps: First, evaluating the integral along the entire length (e.g., `L`); Second, evaluating the integral with respect to that length (after all, we are effectively looking for a weighted average.) We'll see exactly how this breaks out in more detail in one of our example problems. But we can evaluate this integral against all forms of geometries, including areas and volumes. For an area:

{% katex %} c_x = \frac{1}{A} \int x dA {% endkatex %}
{% katex %} c_y = \frac{1}{A} \int y dA {% endkatex %}
{% katex %} c_z = \frac{1}{A} \int z dA {% endkatex %}

And for a volume:

{% katex %} c_x = \frac{1}{V} \int x dV {% endkatex %}
{% katex %} c_y = \frac{1}{V} \int y dV {% endkatex %}
{% katex %} c_z = \frac{1}{V} \int z dV {% endkatex %}

### Moments of Inertia

Rotational dynamics are funny, no doubt about it. The moment of inertia, particularly important when we begin to consider bending and other torsional loads. In those cases, there is a polar axis about which deformation takes place, and two orthogonal axes (typically `x` and `y`). The moment of inertia about each axis is expressed as an integral against the cross-sectional area in question:

{% katex %} I_x = \int x^2 dA {% endkatex %}
{% katex %} I_y = \int y^2 dA {% endkatex %}
{% katex %} I_z = \int (x^2 + y^2) dA {% endkatex %}

In reality, while I've presented these as a unified set of relationships above, they are evaluated in different contexts. The `x` and `y` moments are specifically deformed when a geometry is "bent" (like a diving board), whereas the polar moment of inertia (e.g., `z`) is deformed when a geometry is "twisted" about the centroidal axis. We even have a key concept, the "perpendicular axis theorem", that observes these three are basically the sum of one another for mutually perpendicular axes.

## Machines

There are common mechanisms for capturing how forces, moments, and boundary conditions interact with one another in the context of simple systems. These systems are useful ways to construct more complicated systems, and if you've looked at any "simple machines" content before, most of them will be instantly recognizable.

We'll visit three specific types of systems and attempt to characterize how forces and moments are related within them.

### Beams, Cables, and Pins

Beams and cables are a key element in many static systems. Cables are static in tension (e.g., opposing forces at each end are opposite their displacement from the centroid), whereas beams are also static in compression (e.g., opposing forces at each end can be either in opposition to displacement from the centroid, or both aligned toward the centroid).

![beams, cables, and pins](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qeu9f4ev2r7x728qab0a.png)

Both beams and cables let us assume edges in a structure are in equilibrium for static systems, which lets us focus our analysis on pins (or vertices). At the pins, the forces in equilibrium along each edge can be combined to solve for a static system. This typically lets us then resolve the specific force or moment in each element of the system.

There are typically special "pins" or vertices in a system that have some form of boundary condition. We may assume weight of a system acts primarily upon a single point, for example. We may also have points where we assert there is no displacement or moment. We may also have points where we assume a normal "restorative" force is acting upon the system to maintain a static state. Each of these conditions have different representations and will be encountered in our example problems.

### Pulleys

The classic pulley is simply a way to redirect forces. Consider the following diagram:

![four examples of pulleys](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/smjqneejez4a5l6pwo8u.png)

In this context, the forces are related like so:

{% katex %}
F_1 = F_2
{% endkatex %}

There are other variations of pulley systems, however. In another classic example, the motion of the pulley effectively averages the two forces. In the second example from above, the forces are related like so:

{% katex %}
F_1 = \frac{1}{2} F_2
{% endkatex %}

Of course, you can combine pulleys one after the other in a "chain" that reduces the force in proportion to the scale of the system. Much like a bicycle chain, you are effectively "trading" displacement for dilution of force. In the third example from above, for `n` pulleys coupled in this manner, the forces are related like so:

{% katex %}
F_1 = \frac{1}{n} F_2
{% endkatex %}

Lastly, you can take advantage of (one might even say "leverage") the difference between a stepped pulley to effectively "differentiate" the displacement resulting from some intermediary force. This effectively leverages the different torques about a shared axis to once again trade displacement dilution for force dilution. In the fourth example from above, given an inner diameter `d` and an outer diameter `D`, the forces are related like so:

{% katex %}
F_1 = F_2 \frac{1}{2} \left(1 - \frac{d}{D}\right)
{% endkatex %}

### Trusses

Combining a number of the above concepts, we can introduce the idea of a "truss". Trusses are complex load-bearing systems that combine beams, cables, and pins, as well as other elements. Trusses effectively form a "graph" of relationships between these elements, and if the constraints equal the degrees of freedom (this should sound familiar by now!) we can "solve" the system by constructing a free body diagram about each pin.

Even if it is overconstrained, we recognize that some elements in this graph may be redundant and can be ignored when evaluating loads. An *undersolved* system, however is "indeterminate". We can combine our degrees of freedom and constraints observations to determine when a static system is determinate:

{% katex %}
n_{members} = 2 n_{joints} - 3
{% endkatex %}

But remember, this is a static system. *OFTEN WE ASSUME NO TORSION OR DEFORMATION OF THE SYSTEM*, even (or especially) if it is determinate.

## Friction

We've been dealing with ideal systems and ignoring friction, because *FRICTION EXISTS IN OPPOSITION TO MOTION*--and there shouldn't be motion because we are considering *STATIC* systems. However, friction is a surprisingly complex concept and there are a number of ways to model it. Doing so lets us expand our analysis to several additional interesting machines.

### Models of Friction

Classically, the force of friction is proportional to a normal force by some constant frictional coefficient, `\mu`. Because the normal (or restorative) force enforces a static condition normal to a boundary (like a wall, ramp, or other surface), the motion against which the force of friction opposes is typically orthogonal to this vector--but we can express a relationship of magnitudes:

{% katex %}
F_{friction} = \mu N
{% endkatex %}

In reality, the frictional coefficient is *not* constant. It is the result of a complex interchange of microscopic irregularities between two adjoining surfaces. This means we must recognize different dynamic conditions where these friction must be modeled with different coefficients. This is typically expressed as a series of inequalities.

{% katex %}
F \leq \mu_{static} N
{% endkatex %}

In the above condition, there is *NO SLIP* if the applied force is less than a product of a "static" frictional coefficient. As we approach this threshold, we cannot guarantee slip will occur--so we instead say it is "impending":

{% katex %}
F = \mu_{static} N
{% endkatex %}

Once slip occurs, we recognize that friction still exists--but is less in magnitude between moving boundaries. We model this with a different ("kinetic") frictional coefficient, which is typically smaller:

{% katex %}
F = \mu_{kinetic} N
{% endkatex %}

Let's assume a single, constant frictional coefficient for our subsequent analysis of statics, however.

![classic block-on-ramp friction model](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uw7m6rxch95cpf27revx.png)

### Belts

A belt is a cable-like element partially wrapped around a pulley. The friction at the boundary between these two machines "transfers" some force between the two. Consider the following diagram:

![friction model of belt about pulley moment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2zfi32wfeafq40pbwisr.png)

The forces in this machine are related like so, where `\theta` is the angle about the pulley for which the belt is in contact.

{% katex %}
F_1 = F_2 \exp{\mu \theta}
{% endkatex %}

The resulting torque about the axes of this pulley is simply the difference between these two forces about the radius `r` of the pulley---simple leverage, really.

{% katex %}
T = (F_1 - F_2) r
{% endkatex %}

### Ramps and Screws

Consider a free-body diagram of a classic "block-on-ramp" scenario:

Assuming a static system, we can observe (from a frame aligned with the angle of the ramp) a relationship between the weight and applied force:

{% katex %}
F_1 = F_2 \sin(\phi)
{% endkatex %}

A screw is a simple machine that extends this ramp principle about an axis. The screw bears some axial load (`P`) by resisting a moment (`M`) with this force of friction.

![screw relationship](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wav2bu2nd2z7smzypyzg.png)

We can express this relationship as a function of the radius `r` of the screw, the pitch angle of the threads `\alpha`, and a "thread friction angle" that expresses an opposing or augmenting (e.g., positive or negative offset) frictional force as a trigonometric function of the frictional coefficient:

{% katex %}
M = P r \tan\left(\alpha \pm \tan^{-1}(\mu) \right)
{% endkatex %}

Screws are, of course, incredibly useful. You might not see them when evaluating a truss, for example, but you *WILL* see them when evaluating structural designs and forces at pins (especially for failure modes).

## Example Problems

That's it for the core principles. But if your experiences are like mine, actually *solving* a statics problem can seem like a little bit of black magic until you've walked through the common techniques a couple of times. So, let's tackle a healthy distribution of examples together.

### Problem 1

*Consider a lever whose fulcrum is 40% of the distance from the left-hand side towards the right-hand side. Assume the mass of the lever is 100 kg. If Random Person, who weighs 50 kg, starts walking from the left to the right, at what point will the lever be balanced?*

Consider the following diagram:

![problem one](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y3uiaemw9uxpmghofa87.png)

When the lever is balanced, we have a statics problem because all forces and moments are zero. In particular, we need to assert two opposing moments are equal to each other. Choosing the fulcrum as the centroid of the system, we solve for two moments (with sign conventions such that they sum to zero, measuring displacement from the fulcrum point):

{% katex %}
0 = M_{weight} + M_{person}
{% endkatex %}

The moment of the lever's weight can be computed once we assume that the lever is uniformly dense. This lets us place the weight vector at the halfway point, or at `x = 0.1 L` to the right of the fulcrum. If positive moment is right-handed about the fulcrum, this means the moment due to lever weight is therefore negative:

{% katex %}
M_{weight} = - F_{weight} x_{weight} = - (100 g) (0.1 L) = - 10 g L
{% endkatex %}

The opposing moment will be displaced in the opposite direction, given some displacement from the centroid `x`, and will be positive in our right-handed reference about the fulcrum:

{% katex %}
M_{person} = F_{person} x_{person} = (50 g) (x L) = 50 g L x
{% endkatex %}

We can, of course, now solve for the displacement `x`:

{% katex %}
0 = - 10 g L + 50 g L x \Rightarrow x = 0.2
{% endkatex %}

In other words, the Random Person will be to the left of the fulcrum by 20% the length of the lever.

### Problem 2

*Consider a pole supporting a beam that bears two signal lights over a pair of train tracks. The first signal is half the length of the beam away from the pin, and the second signal is the full length of the beam away from the pin. What is the moment that must be supported at the pin? Assume both signals have a weight of 70 kg, that the beam is 10 m long, and that gravitational acceleration is 9.81 m/s^2.*

Consider the following diagram:

![train signal leverage](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/etawjlw4jg7dh5hd3she.png)

Let's first define our frame:

* Horizontal displacement will be 0 at the pin and positive towards the right

* Moment will be considered positive if right-handed about the pin

* We will denote the moment from the first signal's weight as `M_1`, and the moment from the second signals' weight as `M_2`

The net moment about the pin is the sum of the moments from each signal. (We ignore weight as the mass of the beam was not included in the problem statement.) For a statics problem, there is some restoring moment about the pin from the integrity of the structure; this lets us assert a net moment of 0 and solve for the restoring moment accordingly (which, you will note, must be positive).

{% katex %}
\Sigma M = 0 = M_R + M_1 + M_2
{% endkatex %}

Replace the signal moments with the signed product of their displacement from the pin and their weight.

{% katex %}
M_1 = - F_{g1} x_1 = -70 * 9.81 * 0.5 * 10 = -301
{% endkatex %}

{% katex %}
M_2 = - F_{g2} x_2 = -70 * 9.81 * 1.0 * 10 = -602
{% endkatex %}

We can now substitute into our static sum and solve for the restoring moment:

{% katex %}
0 = M_R - 301 - 602 \Rightarrow M_R = 903
{% endkatex %}

### Problem 3

*Consider the tire of a truck driving through mud. The powertrain is applying a moment of 250 [kg m^2/s^2] against the mud through a tire with a radius of 0.4 [m], and this tire supports a quarter of the truck's weight (the truck has a mass of 5000 [kg]). Assume gravitational acceleration is 9.81 [m/s^2]. What is the frictional coefficient if the slip condition is met under these conditions?*

Consider the following diagram:

![problem three](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o45tp5qw6swutioi1wtv.png)

The force applied by the tire against the ground can be computed from the moment and tire radius:

{% katex %}
F_M = \frac{1}{r} M = \frac{1}{0.4} 250 = 625
{% endkatex %}

The slip condition will be met when this applied force is equal to friction's opposition to motion, which is a function of the frictional coefficient and the normal force resulting from the truck's weight:

{% katex %}
F_f = \mu m g = \mu \frac{1}{4} 5000 9.81 = 12300 \mu
{% endkatex %}

At the slip condition, these expressions will be equal and we can therefore solve for the frictional coefficient `\mu`:

{% katex %}
F_f = F_m \Rightarrow 625 = 12300 \mu \Rightarrow \mu = 0.051
{% endkatex %}

### Problem 4

*Consider the area defined in an x/y plane by the x-axis boundary, y-axis boundary, and the curve `y=\cos(x)`. This area is continuous for the domain `0<=x<=\pi/2`. Compute the x and y coordinates of this area's centroid.*

Consider the following diagram.

![problem four](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kbmknzr6a2zigm7gst9p.png)

Recall the formulation for centroids of a two-dimensional area:

{% katex %}
c_x = \frac{1}{A} \int x dA
{% endkatex %}

{% katex %}
c_y = \frac{1}{A} \int y dA
{% endkatex %}

First, let us compute the area `A`. This is easily done by integrating along either axis--we will do so along the x-axis, which is considerably easier:

{% katex %}
A = \int_0^{\pi/2} \cos(x) dx = \sin(x) |_0^{\pi/2} = 1 - 0 = 1
{% endkatex %}

We can differentiate the function for area to obtain both `dx` and `dy` expressions. Let's start with the x-axis:

{% katex %}
A(x) = \int_0^x \cos(x) dx \Rightarrow \frac{dA}{dx} = \cos(x)
{% endkatex %}

Now we can solve for the x coordinate of the centroid:

{% katex %}
c_x = \int_0^{\pi/2} x dA = \int_0^{\pi/2} x \cos(x) dx = \left(\cos(x) + x \sin(x)\right)|_0^{\pi/2} = \frac{\pi}{2} - 1
{% endkatex %}

Now let's differentiate with respect to the y-axis, where we must invert the curve:

{% katex %}
A(y) = \int_0^y \cos^{-1}(y) dy \Rightarrow \frac{dA}{dy} = \frac{dA}{dy} = \cos^{-1}(y)
{% endkatex %}

We can now solve for the y coordinate of the centroid.

{% katex %}
c_y = \int_0^1 y dA = \int_0^1 y \cos^{-1}(y) dy
{% endkatex %}

This isn't straightforward, though, so I would encourage you to verify via [Wolfram Alpha](https://www.wolframalpha.com/) (or your nearest friendly mathematics major).

{% katex %}
c_y = \left(\frac{1}{4}\left[ -\sqrt{1-y^2}y + 2y^2\cos^{-1}(y) + \sin^{-1}(y)\right]\right)|_0^1
{% endkatex %}

{% katex %}
c_y = \left(\frac{1}{2}\cos^{-1}(1) + \frac{1}{4}\sin^{-1}(1)\right) - \left(\frac{1}{4}\sin^{-1}(0)\right) = \frac{\pi}{8}
{% endkatex %}

### Problem 5

*Consider the following truss configuration. If the weight of the truss is modeled by a force of 10,000 N down from the bottom-middle joint, and the length of each horizontal beam is 1 m (with an angle above the horizontal of 45 deg for all diagonal beams at each joint), determine the following:*

* *The restorative force at the joint R_1*

* *The restorative force at the joint R_2*

* *The force (tension or compression) borne by the beam AB*

* *The force (tension or compression) borne by the beam DE*

![our humble truss](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qzyf91pfudmr1cehjexc.png)

The restorative forces are most easily computed by calculation of the moment about point A. Note we assume a frame at each point--for example, right-handed moments about A are positive, and vice-versa. If the values turn out negative, we will know we need to flip the direction.

{% katex %}

M_{@A} = 0 = 2 * R_2 - 1 * W \Rightarrow R_2 = \frac{1}{2} W = 5,0000 N

{% endkatex %}

You can repeat this equation for joint C, but it should not surprise you to learn, for a symmetrical problem, that the restorative forces are identical at both ends.

Trusses are best solved by expressing x and y forces summed to zero (remember, we are in statics problems) at each point, beginning with points with external forces (like weight and restoratives) and "marching" towards the desired unknowns until the constraints let you back out values for each force.

As mentioned above, we will assume a frame at each point and correct when negative signs indicate we assumed incorrectly. Forces must be equal and opposite for each beam, so using a consistent frame and nomenclature will help keep us from messing up too badly. We'll assume positive x to the right, positive y up, and assume each force acts towards the point unless/until we know otherwise.

Let's start by summing forces at point A. Here is the free-body diagram to consider about that joint.

![free body diagram about joint A](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uws0li1gd3k1i9ex0p03.png)

First, let's sum forces to zero about the x axis:

{% katex %}

F_{x@A} = 0 = -F_{AB} - F_{AD} \cos(\frac{\pi}{4})

{% endkatex %}

Next, let's sum forces to zero about the y axis:

{% katex %}

F_{y@A} = 0 = R_1 - F_{AD} \sin(\frac{\pi}{2})

{% endkatex %}

Since we know `R_1`, we know `F_{AD}`. And since we know `F_{AD}`, we know `F_{AB}`. So far so good! In fact, `F_{AB}` was one of our unknowns to solve for, let's compute that now.

{% katex %}

F_{AD} = R_1 \frac{2}{\sqrt{2}} = \sqrt{2} 5000 = 7071 N

{% endkatex %}

{% katex %}

F_{AB} = -F_{AD} \cos(\frac{\pi}{4}) = -7071 \frac{\sqrt{2}}{2} = 5000 N

{% endkatex %}

Let's proceed to summing forces at point D. Here is the free-body diagram to consider about that joint.

![free body diagram about joint D](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a43vzjczdo1r4jxr9q99.png)

First, let's sum forces to zero about the x axis:

{% katex %}

F_{x@D} = 0 = F_{AD} \cos(\frac{\pi}{4}) - F_{BD} \cos(\frac{\pi}{4}) - F_{DE}

{% endkatex %}

Next, let's sum forces to zero about the y axis:

{% katex %}

F_{y@D} = 0 = F_{AD} \sin(\frac{\pi}{4}) + F_{BD} \sin(\frac{\pi}{4})

{% endkatex %}

Since we know `F_{AD}`, we can solve for `F_{BD}`. And since we can determine both of those, we can solve for `F_{DE}`, which was our final objective. Let's do it!

{% katex %}

F_{BD} = -F_{AD} = -7071 N

{% endkatex %}

Finally we can solve for `F_{DE}`.

$$F_{DE} = \frac{\sqrt{2}}{2} F_{AD} - \frac{\sqrt{2}}{2} F_{BD} \Rightarrow F_{DE} = \frac{\sqrt{2}}{2} (7071 - -7071) = 10,000 N$$

Cool beans!

## Summary

In statics, we use a combination of linear and rotational stasis (forces and moments summing to zero) to impose constraints on a system of variables. Combined with boundary conditions like weight, friction (a product of normal force and a frictional coefficient), and restorative forces, this lets us solve a system--that is, determine forces and moments throughout each element in a more complicated combination.

Other important topics include the concept of machines, in which forces and moments can be combined and transformed in specific ways. Simple machines like these can be aggregated in arrangements that let us manipulated the behavior of a system.

Lastly, we considered several implications of friction with respect to these machines and used trusses as an example of complicated systems.
