---
layout: post
title:  "Android系统中Parcelable和Serializable的区别"
date:   2016-03-14 11:26:05 +0800
categories: android
---


进行Android开发的时候，我们都知道，不能将对象的引用传递给activity或者fragment，我们需要将这些对象放到一个Intent或者Bundle里面，然后传递。通过Android的API文档，我们知道传递对象时需要对我们的对象进行`Parcelable`化或者`Serializable`化。作为java开发者，相信大家都对Serializable机制有一定的了解，那么为什么还需要Parcelable呢？

为了回答这个问题，让我们分别来看看这两者的差异。

## Serializable, 简单易用

```java
public class SerializableDeveloper implements Serializable{
  String name;
  int years;
  List<Skill> skillSet;
  float favorite;

  static class Skill implements Serializable{
    String name;
    boolean related;
  }
}
```

`Serializable`的迷人之处在于你只需要对某个类以及它的属性实现Serializable接口即可。Serializable接口是一种标识性接口（`marker interface`），这意味着无需实现方法，java便会对这个对象进行高效的序列化操作。

这种方法的缺点是使用了反射，序列化的过程比较慢。这种机制会在序列化的时候创建许多的临时对象，容易触发
垃圾回收。

## Parcelable, 速度至上

```java
class ParcelableDeveloper implements Parcelable{
  String name;
  int years;
  List<Skill> skillSet;
  float favorite;

  ParcelableDeveloper(Parcel in){
    this.name = in.readString();
    this.years = in.readInt();
    this.skillSet = new ArrayList<Skill>();
    in.readTypedList(skillSet, Skill.CREATOR);
    this.favorite = in.readFloat();
  }

  void writeToParcel(Parcel dest, int flags){
    dest.writeString(name);
    dest.writeInt(years);
    dest.writeTypedList(skillSet);
    dest.writeFloat(favorite);
  }

  int describeContents(){
    return 0;
  }

  static final Parcelable.Creator<ParcelableDeveloper> CREATOR
            = new Parcelable.Creator<ParcelableDeveloper>() {

        ParcelableDeveloper createFromParcel(Parcel in) {
            return new ParcelableDeveloper(in);
        }

        ParcelableDeveloper[] newArray(int size) {
            return new ParcelableDeveloper[size];
        }
    };

  static class Skill implements Parcelable {
      String name;
      boolean related;

      Skill(Parcel in) {
          this.name = in.readString();
          this.related = (in.readInt() == 1);
      }

      @Override
      void writeToParcel(Parcel dest, int flags) {
          dest.writeString(name);
          dest.writeInt(related ? 1 : 0);
      }

      static final Parcelable.Creator<Skill> CREATOR
          = new Parcelable.Creator<Skill>() {

          Skill createFromParcel(Parcel in) {
              return new Skill(in);
          }

          Skill[] newArray(int size) {
              return new Skill[size];
          }
      };

      @Override
      int describeContents() {
          return 0;
      }
  }
}
```

根据Google工程师的说法，这些代码会运行的特别快。原因之一就是我们已经很清楚的知道了序列化得过程，而不需要使用反射来推断。同时为了更快的进行序列化，对象的代码也需要高度的优化。

因此，很明显实现Parcelable并不容易。实现Parcelable接口需要写大量的模板代码，这使得对象代码变得难以阅读和维护。

## 速度测试

当然，我们还是想知道到底Parcelable相对于Serializable要快多少。

### 测试方法

> * 1、 通过将一个对象放到一个`Bundle`里面然后调用`writeToParcel(Parcel, int)`方法，来模拟传递对象给一个activity的过程，然后再把这个对象取出来。
> * 2、 在一个循环里面运行1000次
> * 3、 两种方法分别测试10次来减少内存整理、CPU被其他应用占用等情况的干扰。
> * 4、 参与测试的对象就是上面代码中的SerializableDeveloper和ParcelableDeveloper。
> * 5、 在多种Android手机上进行测试

### 测试结果

![](/image/img20160314000.png)


> || *测试机型*             || *Serializable* || *Parcelable* || *提升倍数* ||
> || LG Nexus 4 – Android 4.2.2       ||1.8539ms    ||0.1824ms   ||11.80倍||
> || Samsung Nexus 10 – Android 4.2.2 ||1.0004ms    ||0.0850ms   ||10.16倍||
> || HTC Desire Z – Android 2.3.3     ||5.1224ms    ||0.2938ms   ||17.36倍||

------


#### *由此可以得出: Parcelable 比 Serializable快了10多倍。有趣的是，即使在Nexus 10这样性能强悍的硬件上，一个相当简单的对象的序列化和反序列化的过程要花将近一毫秒。*

## 总结

如果你想成为一个优秀的软件工程师，你需要多花点时间来实现 Parcelable ，因为这将会为你对象的序列化过程快10多倍，而且占用较少的资源。

但是大多数情况下， Serializable 的龟速不会太引人注目。你想偷点懒就用它吧，不过要记得serialization是一个比较耗资源的操作，尽量少使用。

如果你想要传递一个包含许多对象的列表，那么整个序列化的过程的时间开销可能会超过一秒，这会让屏幕转向的时候变得很卡顿。
