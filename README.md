# cypher
# https://asecuritysite.com/encryption/ecc_factor
import math
import random 
import sys

#y^2=x^3+ax+b mod n 

prime=[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271 ]


# ax+by=gcd(a,b). This function returns [gcd(a,b),x,y]. Source Wikipedia
def extended_gcd(a,b):   
	x,y,lastx,lasty=0,1,1,0
	while b!=0:
		q=a//b
		a,b=b,a%b
		x,lastx=(lastx-q*x,x)
		y,lasty=(lasty-q*y,y)
	if a<0: 
		return (-a,-lastx,-lasty)
	else:
		return (a,lastx,lasty)

def randomCurve(N):
  A,u,v=random.randrange(N),random.randrange(N),random.randrange(N)
  B=(v*v-u*u*u-A*u) % N
  return [(A,B,N),(u,v)]
def addPoint(E,p_1,p_2):
	if p_1=="Identity": return [p_2,1]
	if p_2=="Identity": return [p_1,1]
	a,b,n=E
	(x_1,y_1)=p_1
	(x_2,y_2)=p_2
	x_1%=n
	y_1%=n
	x_2%=n
	y_2%=n
	if x_1 != x_2 :
		d,u,v=extended_gcd(x_1-x_2,n)
		s=((y_1-y_2)*u)%n
		x_3=(s*s-x_1-x_2)%n
		y_3=(-y_1-s*(x_3-x_1))%n
	else:
		if (y_1+y_2)%n==0:return ["Identity",1]
		else:
			d,u,v=extended_gcd(2*y_1,n)
			s=((3*x_1*x_1+a)*u)%n
			x_3=(s*s-2*x_1)%n
			y_3=(-y_1-s*(x_3-x_1))%n

	return [(x_3,y_3),d]

	# http://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication
	#	Q=0 [Identity element]
	#	while m:
	#		if (m is odd) Q+=P
	#		P+=P
	#		m/=2
	#	return Q')

def mulPoint(E,P,m):
	Ret="Identity"
	d=1
	while m!=0:
		if m%2!=0: Ret,d=addPoint(E,Ret,P)
		if d!=1 : return [Ret,d]  # as soon as i got anything otherthan 1 return 
		P,d=addPoint(E,P,P)
		if d!=1 : return [Ret,d]
		m>>=1
	return [Ret,d]




def ellipticFactor(N,m,times=5):
	for i in range(times):
		E,P=randomCurve(N);
		Q,d=mulPoint(E,P,m)
		if d!=1 : return d
	return N

first=True
n=455832
if (len(sys.argv)>1):
        n=int(sys.argv[1])

print(n,'=', end=' ')

for p in prime:
	while n%p==0:
		if (first==True):
			print(p, end=' ')
			first=False
		else:
			print('x',p, end=' ')
		n/=p

m=int(math.factorial(2000))


while n!=1:
	k=int(ellipticFactor(n,m))
	n=int(n//k)
	if (first==True):
		print(k, end=' ')
		first=False
	else:
		print('x',k, end=' ')


Oldindan hisoblangan jadval algoritmi. 2 - algoritm

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class TrigonometricTable {
    // Sinus qiymatlarini saqlash uchun jadval
    private static Map<Integer, Double> sineTable = new HashMap<>();

    public static void main(String[] args) {
        // Sinus qiymatlarini oldindan hisoblash (0° dan 360° gacha)
        precomputeSineValues();

        Scanner scanner = new Scanner(System.in);
        System.out.print("Burchakni darajalarda kiriting (0-360): ");
        int degree = scanner.nextInt();

        // Burchakni tekshirish
        if (degree < 0 || degree > 360) {
            System.out.println("Xatolik: Burchak 0 va 360 orasida bo'lishi kerak.");
        } else {
            // Sinus qiymatini jadvaldan olish
            double sineValue = getSineValue(degree);
            System.out.printf("Sin(%d°) = %.6f\n", degree, sineValue);
        }

        scanner.close();
    }

    // Sinus qiymatlarini oldindan hisoblash
    private static void precomputeSineValues() {
        for (int degree = 0; degree <= 360; degree++) {
            // Radianlarga aylantirish va sinus qiymatini hisoblash
            double radians = Math.toRadians(degree);
            double sineValue = Math.sin(radians);
            sineTable.put(degree, sineValue);
        }
    }

    // Jadvaldan sinus qiymatini olish
    private static double getSineValue(int degree) {
        return sineTable.get(degree);
    }
}



