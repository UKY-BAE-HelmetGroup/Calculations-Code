# Calculations-Code
The file below is a python script that is used to calculate the spring compression needed for the proper impact velocity at the given initial conditions of our testing systems.


'''
Projectile Motion Calculations for Equestrian Helmet Testing Group
Calculates the amount the spring must be stretched for the desired ground impact velocity.
Along with the final velocity calculations, it gives the time until the cart hits the release point.
The release point is currently set 12 inches away from the end of the track.
The inputs are based on the pinned locations of the fixture.


By: Eli Barrow
Initiated: 9/15/2024
Last Update: 10/23/2024

Message erba240@uky.edu for any concerns

'''

import math

# Given constants
m = 4.2041       # mass of headform in kg
g = 9.81      # gravity (m/s^2)
k = 8581      # given spring constant (N/m)

# Input the final velocity at the ground
FinalV = float(input("Enter the desired final velocity when the object hits the ground (m/s): "))

#spacing of the holes on the tube steel and the holes for the end of the track.
longholespace = 1
shortholespace = 2

# Calculations for h1 and h2
h2 = input("Which hole is the lower bracket inside(1 being the bottom hole)? (1,2,3): ")
if h2 == "1":
    h2 = 34.375-(shortholespace*2)
elif h2 == "2":
    h2 = 34.375 - shortholespace
elif h2 == "3":
    h2 = 34.375 - 0
else:
    h2 = 0

 #Calculations for h1 and h2, this is for the interesting choice of pin placement options on the taller end of the fixture.
verthole = input("Which hole is the pin placed inside on the main post(1 being the bottom hole)? (1,2,3): ")
if verthole == "1":
    verthole = 62.75
elif verthole == "2":
    verthole = 74.75
elif verthole == "3":
    verthole = 86.75
else:
    verthole = 0

# Setting the h1 based on which hole the track is pinned at. Then is used to find the angle of the track.
h1 = input("Which hole is the pin inside on the holed tube (1 being the bottom hole)? (1-16): ")
if h1 == "1":
    h1 = verthole + (longholespace * 7)
elif h1 == "2":
    h1 = verthole + (longholespace * 6)
elif h1 == "3":
    h1 = verthole + (longholespace * 5)
elif h1 == "4":
    h1 = verthole + (longholespace * 4)
elif h1 == "5":
    h1 = verthole + (longholespace * 3)
elif h1 == "6":
    h1 = verthole + (longholespace * 2)
elif h1 == "7":
    h1 = verthole + (longholespace * 1)
elif h1 == "8":
    h1 = verthole - 0
elif h1 == "9":
    h1 = verthole - (longholespace * 1)
elif h1 == "10":
    h1 = verthole - (longholespace * 2)
elif h1 == "11":
    h1 = verthole - (longholespace * 3)
elif h1 == "12":
    h1 = verthole - (longholespace * 4)
elif h1 == "13":
    h1 = verthole - (longholespace * 5)
elif h1 == "14":
    h1 = verthole - (longholespace * 6)
elif h1 == "15":
    h1 = verthole - (longholespace * 7)
elif h1 == "16":
    h1 = verthole - (longholespace * 8)
else:
    h1 = 0


print(f"h1 is {h1}")
print(f"h2 is {h2}")

# Calculate the height difference
href = h1 - h2
print(f"href is {href}")

#track angle calculation
width = 117.5 
theta = math.atan(href / width)
theta_angle = math.degrees(theta)
print(f"theta angle is {theta_angle}")

# Calculate h2_new
h2_new = math.sin(theta)  # calculates h2 that is 12 inches up the track from the pin. This determines the release point of the headform
h2_new = 12 * h2_new
print(f"h2_new is {h2_new}")
h2_new = h2 + h2_new
print(f"h2_new is {h2_new}")

# Convert heights to meters
h1_m = h1 * 0.0254
h2_new_m = h2_new * 0.0254

# Now I am working backwards:
# 1. Splitting the FinalV into horizontal and vertical components based on the angle (theta)

# Horizontal velocity component (does not change during the fall)
VeloH = FinalV * math.cos(theta)
print(f"Horizontal velocity at ground (VeloH) is {VeloH:.4f} m/s")

# 2. Find the vertical velocity at ground impact using energy conservation:
# FinalV^2 = V_vertical_ground^2 + V_horizontal^2
VeloV_ground = math.sqrt(FinalV**2 - VeloH**2)


#finding the Velocity of the cart when it's at the end of the track. This is the projectile motion.
V_endoftrack = ((((FinalV)**2)-(19.62*h2_new_m))/(((math.sin(theta))**2)+((math.cos(theta))**2)))**.5
print(f"Velocity end of track (V_endoftrack) is {V_endoftrack:.4f} m/s")


#calculating spring compression from end of track velo.
#finding the h1 for the energy equation.
y_value = h1-h2-(12*math.sin(theta))
print(f"y_value is {y_value:.4f} in")

#changing that value to meters
y_value_m = y_value * .0254
print(f"y_value in meters is {y_value_m:.4f} m")

#calculating the amount of spring compression to make the cart hit the ground at the desired speed.
springstretch = (((.5*m*(V_endoftrack)**2)-(m*g*y_value_m))/(.5*k))**.5
print(f"springstretch needed in meters is {springstretch:.4f} m")

#Time calculation
Force = k*springstretch
print(f"Force in Newtons is {Force:.4f} N")

#acceleration based on force and mass of the cart
acceleration = Force/m
print(f"Force in Newtons is {acceleration:.4f} m/s^2")

#finding distance traveled on track
distance = (href)/(math.sin(theta))*.0254
print(f"Distance in meters is {distance:.4f} m")

# Check if acceleration is a complex number
if isinstance(acceleration, complex):
    raise ValueError("Acceleration should be a real number, but a complex value was found.")

    # Proceed with the original comparison if it's a real number
    if acceleration > 0:
        t = math.sqrt(2 *  distance/ acceleration)
        print(f"The time for the cart to reach {distance:.4f} meters is: {t:.4f} seconds")
    else: 
        print("Error: The acceleration must be greater than zero.")

pass

    
