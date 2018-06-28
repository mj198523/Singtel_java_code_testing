# Singtel_java_code_testing
1. Can you implement the sing() method for the bird?

public interface Animal {

	public void walk();
	public void fly();
	public void sing();
	
}

public class Bird implements Animal {

	public void walk() {
		System.out.println("I am walking"); 
	}

	public void fly() {
		System.out.println("I am flying");
	}

	public void sing() {
		System.out.println("I am singing");
	}
}

public class Solution {

	public static void main(String[] args) { 
		Bird bird = new Bird();
		bird.walk();
		bird.fly();
		bird.sing();
	}
}

a. How did you unit test it ?
First of all. we should define a valid and common Bird object. A Bird should be able to sing, walk and fly.
Therefore when we create a Bird object and invoke the characters of it, we should get a Bird which is able to sing, walk and fly.

b. How did you optimize the code for maintainability?
Please refer to the code of Question 1. I think we should design the Animal as a interface which contains the common actions of an Animal.
Then for each different animal, we should specify the details of the actions.


2. Now, we have 2 special kinds of birds: the Duck and the Chicken... Can you implement them to make their own special sound ?
a. A duck says: “Quack, quack”

public class Duck extends Bird {

	public void sing() {
		System.out.println("Quack, quack");
	} 
}

b. A duck can swim

//Normally, the common actions of animal should be able to walk, fly, sing and swim
public interface Animal {

	public void walk();
	public void fly();
	public void sing();
	public void swim();
}

//Normally, bird should be able to walk, fly, sing, but not swim
public class Bird implements Animal{

	public void walk() {
		System.out.println("I am walking"); 
	}

	public void fly() {
		System.out.println("I am flying");
	}

	public void sing() {
		System.out.println("I am singing");
	}
	
	public void swim() {
		System.out.println("I can't swim");
	}
}

//For Duck, we specify the sing and swim action.
public class Duck extends Bird {

	public void sing() {
		System.out.println("Quack, quack");
	}
	
	public void swim() {
		System.out.println("I am swimming");
	}
}

c. A chicken says: “Cluck, cluck”

public class Chicken extends Bird {

	public void sing() {
		System.out.println("Cluck, cluck");
	}
}

d. A chicken cannot fly

//For Chicken, we specify the sing and fly action. The Bird class and Animal interface will use the same in question b.
public class Chicken extends Bird {

	public void sing() {
		System.out.println("Cluck, cluck");
	}
	
	public void fly() {
		System.out.println("I can't fly");
	}
}

3. Now how would you model a rooster?
a. A rooster says: “Cock-a-doodle-doo”

public class Rooster extends Chicken {

	public void sing() {
		System.out.println("Cock-a-doodle-doo");
	}
}

b. How is the rooster related to the chicken ?
Rooster is a kind of a chicken, then it should have the common behavior of chicken.

c. Can you think of other ways to model a rooster without using inheritance?
I think I will concider reflection.

import java.lang.reflect.Method;

public class Rooster {
	
	public void actions() {
		try {
			Class objectClass = Class.forName("Chicken");
			Method[] methods = objectClass.getMethods();
			Object object = objectClass.newInstance();
			
			for (int i = 0; i < methods.length; i++) {
				if (methods[i].getName().equals("sing")) {
					System.out.println("Cock-a-doodle-doo");
				} else if (methods[i].getName().equals("fly")) {
					System.out.println(methods[i].invoke(object));
				} else if (methods[i].getName().equals("walk")) {
					System.out.println(methods[i].invoke(object));
				} else if (methods[i].getName().equals("swim")) {
					System.out.println(methods[i].invoke(object));
				}
            }
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

4. Can you model a parrot? We are specifically interested in three parrots, one that lived in a house with dogs one in a house with cats, the other lived on a farm next to the rooster. 
a. A parrot living with dogs says: “Woof, woof”

public class Dog implements Animal {

	public void walk() {
		System.out.println("I am walking");
	}

	public void fly() {
		System.out.println("I can't fly"); 
	}

	public void sing() {
		System.out.println("Woof, woof"); 
	}

	public void swim() {
		System.out.println("I am swimming"); 
	}

}

public class Parrot extends Bird {

	public void sing() {
		Dog dog = new Dog();
		dog.sing();
	}
}

b. A parrot living with cats says: “Meow” 

public class Cat implements Animal {

	public void walk() {
		System.out.println("I am walking");
	}

	public void fly() {
		System.out.println("I can't fly"); 
	}

	public void sing() {
		System.out.println("Meow"); 
	}

	public void swim() {
		System.out.println("I am swimming"); 
	}
}

public class Parrot extends Bird {

	public void sing() {
		Cat cat = new Cat();
		cat.sing();
	}
}

c. A parrot living near the rooster says: “Cock-a-doodle-doo”

public class Parrot extends Bird {

	public void sing() {
		Rooster rooster = new Rooster();
		rooster.sing();
	}
}

d. How do you keep the parrot maintainable? What if we need another parrot lives near a Duck? Or near a phone that rings frequently?
I think the parrot should extends bird as it's a kind of bird and the sing() should return different voice from other animal object.

B. Model fish as well as other swimming animals.
1. In addition to the birds, can you model a fish? 
a. Fishes don’t sing 
b. Fishes don’t walk 
c. Fishes can swim

public class Fish implements Animal {

	public void walk() {
		System.out.println("I can't walk");
	}

	public void fly() {
		System.out.println("I can't fly"); 
	}

	public void sing() {
		System.out.println("I can't sing"); 
	}

	public void swim() {
		System.out.println("I am swimming"); 
	}
}

2. Can you specialize the fish as a Shark and as a Clownfish? 
a. Sharks are large and grey 
b. Clownfish are small and colourful (orange) 
c. Clownfish make jokes 
d. Sharks eat other fish

//Fish
public class Fish implements Animal {
	
	public String size;
	public String colour;
	public boolean isJoke;
	public boolean eatOthers;
	
	public Fish() {}
	
	public Fish(String size, String colour, boolean isJoke, boolean eatOthers) {
		this.size=size;
		this.colour=colour;
		this.isJoke=isJoke;
		this.eatOthers=eatOthers;
	}

	public void walk() {
		System.out.println("I can't walk");
	}

	public void fly() {
		System.out.println("I can't fly"); 
	}

	public void sing() {
		System.out.println("I can't sing"); 
	}

	public void swim() {
		System.out.println("I am swimming"); 
	}
	
	public String getSize() {
		return size;
	}

	public void setSize(String size) {
		this.size = size;
	}

	public String getColour() {
		return colour;
	}

	public void setColour(String colour) {
		this.colour = colour;
	}

	public boolean isJoke() {
		return isJoke;
	}

	public void setJoke(boolean isJoke) {
		this.isJoke = isJoke;
	}

	public boolean isEatOthers() {
		return eatOthers;
	}

	public void setEatOthers(boolean eatOthers) {
		this.eatOthers = eatOthers;
	}
}

//Shark
public class Shark extends Fish {

	public Shark(String size, String colour, boolean isJoke, boolean eatOthers) {
		super(size, colour, isJoke, eatOthers);
	}
}

//Clownfish
public class Clownfish extends Fish {

	public Clownfish(String size, String colour, boolean isJoke, boolean eatOthers) {
		super(size, colour, isJoke, eatOthers);
	}
	
}

public class Solution {
	public static void main(String[] args) { 
	
		Shark shark = new Shark("large","grey",false,true);
		System.out.println(shark.getSize());
		System.out.println(shark.getColour());
		System.out.println(shark.isJoke);
		System.out.println(shark.isEatOthers());
		
		Clownfish clownfish = new Clownfish("small","colourful",true,false);
		System.out.println(clownfish.getSize());
		System.out.println(clownfish.getColour());
		System.out.println(clownfish.isJoke);
		System.out.println(clownfish.isEatOthers());
	}
}

3. Dolphins are not exactly fish, yet, they are good swimmers 
a. Can you model a dolphin that swims without inheriting from a fish class?
I think i will still concider Java reflection, the code is similar like Question A-3-c.
b. How do you avoid duplicating code or introducing unneeded overhead?
I think there should be no duplicating code or introducing unneeded overhead if using my solution.


D. Model animals that change their behaviour over time 
1. Can you model a butterfly? 
a. A butterfly can fly
b. A butterfly does not make a sound 

//Butterfly
public class Butterfly implements Animal{

	public void walk() {
		System.out.println("I am walking");
	}

	public void fly() {
		System.out.println("I am flying"); 
	}

	public void sing() {
		System.out.println("I can't sing"); 
	}

	public void swim() {
		System.out.println("I can't swim"); 
	}
}

2. Can you optimize your model to account for the metamorphosis from caterpillar to butterfly? 
a. A caterpillar cannot fly 
b. A caterpillar can walk (crawl) 

//Caterpillar
public class Caterpillar extends Butterfly {

	public void walk() {
		System.out.println("I am walking");
	}

	public void fly() {
		System.out.println("I can't fly"); 
	}

	public void sing() {
		System.out.println("I can't sing"); 
	}

	public void swim() {
		System.out.println("I can't swim"); 
	}
	
	public void changeToButterfly() {
		super.fly();
		this.walk();
		this.sing();
		this.swim();
	}
}

E. Counting animals
1. Can you share the code to count: 
a. how many of these animals can fly? 
b. how many of these animals can walk? 
c. how many of these animals can sing? 
d. how many of these animals can swim? 

//Add method isWalk/isFly/isSing/isSwim to define if the animal is able to walk/fly/sing/swim
public interface Animal {

	public void walk();
	public void fly();
	public void sing();
	public void swim();
	public boolean isWalk();
	public boolean isFly();
	public boolean isSing();
	public boolean isSwim();
}

//Take bird as a sample, we implement the new method to know if Bird is able to walk/fly/sing/swim
public class Bird implements Animal{

	public void walk() {
		System.out.println("I am walking"); 
	}

	public void fly() {
		System.out.println("I am flying");
	}

	public void sing() {
		System.out.println("I am singing");
	}
	
	public void swim() {
		System.out.println("I can't swim");
	}

	public boolean isWalk() {
		return true;
	}

	public boolean isFly() {
		return true;
	}

	
	public boolean isSing() {
		return true;
	}

	public boolean isSwim() {
		return false;
	}
}

//The testing method
public class Solution {

	public static void main(String[] args) {
	
		int walkCount = 0;
		int flyCount = 0;
		int singCount = 0;
		int swimCount = 0;
		
		Animal[] animals = new Animal[]{
				new Bird(),new Duck(),new Chicken(),new Rooster(),
				new Parrot(),
				new Fish(),
				new Shark("large","grey",false,true),
				new Clownfish("small","colourful",true,false),
				new Dog(),new Butterfly(),new Cat() };
		for (int i=0; i < animals.length; i++) {
			if (animals[i].isWalk()) {
				walkCount++;
			}
			if (animals[i].isFly()) {
				flyCount++;
			}
			if (animals[i].isSing()) {
				singCount++;
			}
			if (animals[i].isSwim()) {
				swimCount++;
			}
		}
		System.out.println("Walk animal : " + walkCount + ", Fly animal : " + flyCount
				+ ", Sing animal : " + singCount + ", Swim animal : " + swimCount);
		
	}
}


BONUS
If you still have time left, please consider the following: 
1. Can you add a second language (if you know a language other than English) Use the rooster as a PoC for demonstrating this. For example, 
this is how the Rooster sounds differently depending on language. Please add the rooster sound in your native tongue.

//Rooster
public class Rooster extends Chicken {
	
	public enum Language {
		//Just take 3 languages as an example
		Danish("kykyliky", "Danish"), Dutch("kukeleku", "Dutch"), Finnish("kukko kiekuu", "Finnish");
        
        private String name;
        private String country;

        
        private Language(String name, String country) {
            this.name = name;
            this.country = country;
        }

        public static String getName(String country) {
            for (Language c : Language.values()) {
                if (c.getCountry().equals(country)) {
                    return c.name;
                }
            }
            return null;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getCountry() {
            return country;
        }

        public void setCountry(String country) {
            this.country = country;
        }
    }
	
	public void sing() {
		System.out.println("Cock-a-doodle-doo"); 
	}
	
	public String secondLanguage(String country) {
		return Language.getName(country);
	}
}

//Testing
public class Solution {

	public static void main(String[] args) {
	
		Rooster rooster = new Rooster();
		System.out.println(rooster.secondLanguage("Finnish"));
	}
}

2. Can you design a RESTful API for querying these animals ?

Request : http://localhost:8080/testing/searchAnimals/?animal=bird
Response : return one json oject.
			{
			 "walk":"I am working.",
			 "sing":"I am singing,",
			 "fly":"I am flying,",
			 "swim":"I can't swimming,",
			 "isAbleToWalk":true,
			 "isAbleToSing":true,
			 "isAbleToFly":true,
			 "isAbleToSwim":false,
			}

