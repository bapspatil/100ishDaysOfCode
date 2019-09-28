
# Singleton

Read about how to build a Singleton pattern in Java here:

[How to make the perfect Singleton?](https://medium.com/@kevalpatel2106/how-to-make-the-perfect-singleton-de6b951dfdb0)

What the result code looks like:
```
public class SingletonClass implements Serializable {

    private static volatile SingletonClass sSoleInstance;

    //private constructor.
    private SingletonClass(){

        //Prevent form the reflection api.
        if (sSoleInstance != null){
            throw new RuntimeException("Use getInstance() method to get the single instance of this class.");
        }
    }

    public static SingletonClass getInstance() {
        if (sSoleInstance == null) { //if there is no instance available... create new one
            synchronized (SingletonClass.class) {
                if (sSoleInstance == null) sSoleInstance = new SingletonClass();
            }
        }

        return sSoleInstance;
    }

    //Make singleton from serialize and deserialize operation.
    protected SingletonClass readResolve() {
        return getInstance();
    }
}
```

Also read about Singleton in Flutter & Dart here:

[Singletons in Flutter / Dart](https://medium.com/@m_knabe/singletons-in-flutter-dart-f3fc94d9d9f7)
