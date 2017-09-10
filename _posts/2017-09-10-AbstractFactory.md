---
layout: post
title: Abstract factory
tags: [Design Pattern]
---

> 將零件的產生與產品的組裝都 interface 化，使用者不需要知道實作的細節。

中文名稱是抽象工廠，直接從例子來看。  
我們今天要來賣桌上型電腦，我們需要做哪些事情呢?

- 第一，我們需要請**工廠**生產各個零件，在這個例子中，我簡化為螢幕，鍵盤和主機三個大項目。
- 第二，我們需要將這些東西**組裝**成套餐賣給消費者。

<!-- more -->

# **零件**

首先我們先來定義零件，這邊為了簡化問題，我們的零件簡單分為三個，分別是螢幕，鍵盤和主機。我們希望工廠能幫我們生產出這個部位。

~~~ c++
typedef struct Screen // 螢幕
{
    int width; int height; // 解析度很重要吧?
}Screen;

typedef struct Keyboard // 鍵盤
{
    int color; // 現在流行酷炫的彩色鍵盤
}Keyboard;

typedef struct Computer // 主機
{
    int core; // 核心是整台電腦的重點囉
}Computer;
~~~

# **產品**

接下來我們定義我們的桌機:用螢幕，鍵盤和主機三個螢幕組裝起來。

~~~c++
typedef struct Desktop
{
    Screen *m_screen;       // 套組的螢幕
    Keyboard *m_keyboard;   // 套組的鍵盤
    Computer *m_computer;   // 套組的主機
    
    Desktop();
    Desktop(Screen *s, Keyboard *k, Computer *c);
    ~Desktop();
    
    void printInfo();  // 印出套組資訊
}Desktop;
~~~

# **抽象化**

首先是工廠，負責生產各零件的廠商擁有各種技術，大家所生產出來的產品好壞功能也都不相同，但是**各工廠都需要能夠產生出螢幕，鍵盤和主機這三樣零件。**這是工廠的最低限度，因此這邊我們開出一個規則，那就是每個廠商都要能產生出這三個東西，至於產生出來的螢幕是甚麼解析度, 鍵盤有幾種顏色, 主機是幾核心, 這些都不規範, 工廠只要能產生這三個東西就是一個合格的工廠。

接下來跟工廠下單的我們需要推出一個**桌上型電腦套組**，而怎樣算是一個完整的桌上型電腦套組呢?在我們的例子中是擁有螢幕，鍵盤和主機就可以算是一個完整套組了，我們也把這個抽象化成一個規範。我們身為一個有原則的組裝商，我們暫時要求自己**一個套裝電腦只能用同一個工廠生產出來的零件來組裝**。

工廠的最低規範用 interface 來實作。

~~~c++
class DesktopFactory
{
public:
    virtual Screen* GenerateScreen() const = 0;
    virtual Keyboard* GenerateKeyboard() const = 0;
    virtual Computer* GenerateComputer() const = 0;
};
~~~

套組的組裝資訊也用 interface 來實作。這 function 的意思是，給你一個工廠，請用他生產的零件來組裝出我要的桌上型電腦套組。

~~~c++
class DesktopProduct
{
public:
    virtual Desktop* GiveMeDesktop(DesktopFactory *factory) = 0;
};
~~~

# **Factory 的實現和 Product 的實現**

有了工廠規範之後，接下來就是實作幾個真正的工廠來用囉。這邊我們假設有兩個工廠，一個是鼎鼎大名的 Apple 工廠，另外一個是默默無名的 Soso 工廠。**雖然他們生產出來的零件性能完全不在一個檔次，但是他們都符合工廠規範，可以生產出螢幕，鍵盤和主機三個零件，因此他們都是合格的工廠。**

~~~c++
class AppleFactory : public DesktopFactory
{
public:
    Screen* GenerateScreen() const
    {
        return new Screen(4000, 4000);
    }

    Keyboard* GenerateKeyboard() const
    {
        return new Keyboard(256);
    }

    Computer* GenerateComputer() const
    {
        return new Computer(8);
    }
};

class SosoFactory : public DesktopFactory
{
public:
    Screen* GenerateScreen() const
    {
        return new Screen(2048, 2048);
    }
    
    Keyboard* GenerateKeyboard() const
    {
        return new Keyboard(7);
    }

    Computer* GenerateComputer() const
    {
        return new Computer(2);
    }
};
~~~

而在 DesktopProduct 方面，我們的例子推出兩種方案:正常方案和黑科技方案。正常方案的意思是說我們用這三個零件直接組裝成桌機給你，黑科技方案則是利用工廠給我們的零件，我們幫你偷偷改機，鍵盤顏色翻倍，核心數也翻倍，這是我們組裝商的小魔法。

~~~ c++
class DesktopProduct
{
public:
    virtual Desktop* GiveMeDesktop(DesktopFactory *factory) = 0;
};

class NormalProduct : public DesktopProduct
{
public:
    Desktop* GiveMeDesktop(DesktopFactory *factory)
    {
        Desktop *d = new Desktop(factory->GenerateScreen(),
                                 factory->GenerateKeyboard(),
                                 factory->GenerateComputer());
        return d;
    }
};

class SuperProduct : public DesktopProduct
{
public:
    Desktop* GiveMeDesktop(DesktopFactory *factory)
    {
        Desktop *d = new Desktop(factory->GenerateScreen(),
                                 factory->GenerateKeyboard(),
                                 factory->GenerateComputer());
        d->m_keyboard->color *= 2;
        d->m_computer->core *= 2; // black technology.
        return d;
    }
};
~~~

# **工廠與方案的組合**

最後對於 client 來說有四種組合可以選:

- Apple Factory + Normal Product
- Apple Factory + Super Product
- Soso Factory + Normal Product
- Soso Factory + Super Product

也就是兩間廠商與兩種方案的組合。我們可以利用 printInfo() 來看整個套組的訊息。而 client 端其實自始自終都不用知道零件有哪些，他們只要知道我使用產品方案 A 搭配某一個廠商可以讓我得到一組完整的桌上型電腦就可以了!

~~~c++
int main()
{
    AppleFactory apple_factory;
    SosoFactory soso_factory;

    NormalProduct normal_product;
    SuperProduct super_product;

    cout << "Apple factory + Apple product" << endl;
    Desktop* d1 = normal_product.GiveMeDesktop(&apple_factory);
    d1->printInfo();

    cout << "Apple factory + Super product" << endl;
    Desktop* d2 = super_product.GiveMeDesktop(&apple_factory);
    d2->printInfo();

    cout << "Soso factory + Normal product" << endl;
    Desktop* d3 = normal_product.GiveMeDesktop(&soso_factory);
    d3->printInfo();

    cout << "Soso factory + Super product" << endl;
    Desktop *d4 = super_product.GiveMeDesktop(&soso_factory);
    d4->printInfo();

    // release
    ...
    return 0;
}
~~~

# **對應名稱**

- **AbstractFactory**
  
  DesktopFactory 介面，他規範了工廠需要有能力產生哪些零件

- **ConcreteFactory**

  AppleFactory 和 SosoFactory，實作 DesktopFactory 而真正產生零件

- **AbstractProduct**

  DesktopProduct 介面，他說明不管是哪種套裝方案，給定一個工廠就會給使用者一組真正能使用的桌機套裝，不同方案的差別是組裝細節(在例子中便是有沒有使用黑科技)

- **ConcreteProudct**

  NormalProduct 和 SupterProduct，實作 AbstractProduct 而真正產生最後產品

- **Client**

  使用者，他不需要知道產品組裝零件與細節的任何資訊，他唯一需要知道的是對於某個方案，給予一個指定的工廠，他便會生產一個桌上型電腦給我。不同的工廠配上不同方案，給出來的桌機等級就不會相同。
  
# **實作細節**

完整的程式碼我放在 [GitHub](https://github.com/nk910216/DesignPattern/blob/master/abstract_factory/abstract_factory.cpp) 上，因為是簡單例子我就沒有特別分 class，但是可以簡單用 g++ 來觀看結果。

# **討論**

- 這個方法隱藏了許多實作，讓 client 端使用起來很方便，不用自己去維護許多東西。首先，它隱藏了零件的資訊，client 端需要維護的只有 Desktop本身，不需要去撰寫零件的程式碼。(在這個實作中我用 struct 代表桌機，讓使用者可以自由存取零件，這不是個好設計，只是為了簡單方便。較好的方式是將零件的細節都設定為 private，使用者無法從 Desktop class 直接存取零件，而是需要透過 method。)

- 如果今天我們想推出一個新的方案，那麼我只要再實作一個 AbstractProduct 就好。而如果今天有新的廠商進駐，那麼我們只要實作一個 DesktopFactory 就好，client 端不用去知道細節，他只要知道新的廠商與方案可以跟之前的廠商與方案做任意的搭配。

- 這個實作的另一個好處是，**對於任何一個方案來說，他只接受一個廠商參數，也就是強制任何方案都只能使用同一家廠商生產的零件。可以防止不同廠商零件之間不相容的問題。**

- 不過缺點也明顯，假設今天電腦的零件多了一個滑鼠才能稱為套組，那我們就得去更改介面 DesktopFactory。而每一間廠商就要去實作滑鼠這個零件，每一個套裝也要另外把滑鼠考慮進來。增加一個零件需要改動的程式碼幅度挺大的。

# **Reference**

- **Design Pattern聖經**
- [帥哥學長的網站](https://www.jyt0532.com/2017/05/03/abstract-factory/)
- [Graphic Design Pattern](http://design-patterns.readthedocs.io/zh_CN/latest/creational_patterns/abstract_factory.html)
- [sourcemaking](https://sourcemaking.com/design_patterns/abstract_factory)
