# MATH317_Assigment_1

import math

#Part 1
#1
Both functions attempt to approximate log(x), x inputed.
naivelog (Briggs):
- Takes the input x and turns it into y
- Iteratively takes the square root n times
-  Takes this square root and subtracts 1, to give l
- Multiplies l by 2^n
- Returns  l

 mylog (the way out of issue with Briggs):
 - Takes input x, creates z = x- 1
 - Iteratively divides z by (1+sqrt(1+z)) n times
 - l is set to the value of z after the for loop
 - Similarly, it multiplies l by 2^n
 - Returns l
 
 in both functions, n is the number of iterations each calculation
 goes through to calculate the log. The higher n, the closer the approximation
 should be to the true value.
    

'''

print(naivelog(5)) #1.6094774377779686
print(mylog(5)) #1.6094774377759418

print(mylog(5)) # 1.6094377040863037
print(mylog(5)) #1.6094379136402983


'''
#2
#As predicted, as n increases, the errors between the estimates
#decrease. Without being overly simple, the reason they ouput
#different results is because they employ different methods.
#naivelog just uses square root whilst mylog while mylog employs
#a square root and a division in its iterative sequence.


This just allows for us to see how the error changes (it shrinks as n increases!!)
```
for i in range(1,15):
     print(i)
     print(naivelog(i))
     print(mylog(I))
     print(math.log(i))
     print("Dif between naivelog and mylog = ", abs(naivelog(i)-mylog(i)))
     print("Dif between naivelog and math.log = ", abs(naivelog(i)-math.log(i)))
     print("Dif between mylog and math.log = ", abs(mylog(i)-math.log(i)))
     print("")
    
```
#3   
Here we used the known Taylor series as a suitable argument-halving method for all 3 functions
```
def myexp(x):
    #Important to set up e and term as floats, since they contain decimals
    #first term of exp is also always 1, thats why e = 1
    e = 1.0
    n = 15
    term = 1.0
    #Adding up additional terms until desired range of n
    for i in range (1, n+1): 
        term *= x/i
        e += term
    return e

print(myexp(2)) #7.389056095384136

print(myexp(5)) #148.4029173713987

print(myexp(10)) #20952.886968606548

print(myexp(-1) #0.3678794411713973

```

```
def mysin(x):
    #First term is x
    sin = 0
    n = 15
    term = x
    #Similar to above, but have to take a step now (only odd number exponents)
    for i in range (1, n+1, 2):
        #Simply the the series formula (multiplying each step by -x^2 and moving
        #the factorial in the denominator along 2 steps
        sin += term 
        term *= -(x * x) / ((i + 1) * (i + 2))
        
    return sin
```
```
def mycos(x):
    #First term is 1
    cos = 1.0
    n = 15
    term = 1.0
    #Again, must procede 2 steps at a time, starting at 2 since first term is constant = 1
    for i in range(2, n+1, 2):
    #Similarly, multiply the next term by -x^2, and moving the factorial along 2 steps
    #difference in code here is that cosx starts at 1 so factorial starts lower
        term *= -(x * x) / ((i - 1) * i)
        cos += term
    return cos
```  
```
print(mysin(0.1)) #0.09983341664682817
print(mycos(0.1)) #0.9950041652780258
print(mysin(0.2)) #0.19866933079506124
print(mycos(0.2)) #0.9800665778412416
print(mysin(0.5)) #0.479425538604203
print(mycos(0.5)) #0.8775825618903728
print(mysin(1))   #0.8414709848078937
print(mycos(1))   #0.540302305868092
```
```
def myarctan(x):
    n = 16
    
    arctan = 0
    term = x
    for i in range(1, n+1, 2):
        #Similar to last question
        #Multiplying by -x^2 and having the denominator skip by 2 each time
        arctan += term / i
        term *= -(x * x)
        
    return arctan

print(myarctan(0.5)) #0.46364724210793534
print(myarctan(0.6)) #0.5404119639916675
print(myarctan(0.8)) #0.6738973345593746


```
Part 2 Q1 
As explained in the comments, this method essentially takes the calculations of arctan from our previous function and utilizes it to get readjust x and y to get closer and closer to the line that gives us the desired angle. x, and y are based on the functions of cos and sin we saw in class. Angle difference, as it's name suggests, calculates how far off the new line's angle is away from the desired angle.
```

def cordic_tan(angle):
    n = 50
    x = 1
    y = 0
    
    #Accounting for inputs outside our range
    if angle > 2*math.pi:
        angle = angle - 2*math.pi
        
    angle_difference = 0
    
    for i in range(1, n+1):
        #This loop follows the formula of replacing y_n and x_n with x_n+1
        #and y_n+1 as seen in class, negative/positive when needed
        #Initially starting with + 45 degrees in order to base later
        #adjustments
        if (angle_difference == 0):
            new_x = x - (y / (2 ** i))
            new_y = y + (x / (2 ** i))
            angle_difference += myarctan(1 / 2 ** (i))
            
        elif (angle_difference - angle > 0):
            #Given each subsequent step is halved, its appropriate to
            #divide by 2^-i
            new_x = x + (y / (2 ** i))
            new_y = y - (x / (2 ** i))
            angle_difference -= myarctan(1 / 2 ** (i))
            
        elif (angle_difference - angle <0) :
            new_x = x - (y / (2 ** i))
            new_y = y + (x / (2 ** i))
            angle_difference += myarctan(1 / 2 ** (i))
        x = new_x
        y = new_y
    #Accounting for inputs outside our range 
    if (angle > math.pi/2 and angle < math.pi) or (angle > 3*math.pi/2 * math.pi):  
        tan = (math.pi-new_y/new_x)
    else:
        tan = new_y/new_x
    return tan      

  


 
print(cordic_tan(math.pi/5)) #0.7265430885728901

print(cordic_tan(math.pi/9)) #0.3639706497593441

print(cordic_tan(math.pi/6)) #0.5773507583845472
```
#2 Sine and Cosine are simply trigonometric identities based around tangent that we saw in Assignment 1.
```

def cordic_sine(x):
    #following the trigonometric identities in Assignment 1 4a
    tan = cordic_tan(x)
    sine = tan / ((1 + tan ** 2) ** (1/2))
    return sine

print(cordic_sine(math.pi/5)) #0.5877855491176244
print(cordic_sine(math.pi/9)) #0.34202048808912183
print(cordic_sine(math.pi/6)) #0.500000317741321
```

```    
def cordic_cosine(x):
    tan = cordic_tan(x)
    #Similarly to sine
    cosine = 1 / ((1 + tan ** 2) ** (1/2))
    return cosine
    
    
print(cordic_cosine(math.pi/5)) #0.8090167787187684
print(cordic_cosine(math.pi/9)) #0.9396924953022019
print(cordic_cosine(math.pi/6)) #0.8660252203363237

```
Similarly, arcsin is also a trigonometric identity we saw in class.
```
def cordic_arcsin(x):
    #Trig identity seen in Assignment 1, 4a
    return (cordic_arctan(x / (1 - x ** 2) ** (1/2)))
```
