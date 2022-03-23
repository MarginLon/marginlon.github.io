---
title: QSS加载
description: QSS加载
toc: true
authors:
tags:
categories:
series:
date: '2022-03-07'
lastmod: '2022-03-07'
draft: false
weight: 1
---

QSS加载

<!--more-->

## 加载QSS的三种方式

1. Widget 的对象调用```setStyleSheet(qss)``` 函数加载 QSS，QSS 的作用域是 widget 自己和它的所有子 widget 
2. QApplication 的对象```setStyleSheet(qss)``` 函数加载 QSS，QSS 的作用域是整个程序里的所有 widget
3. 在 Qt Designer 的```Change styleSheet...```打开的 QSS 编辑器中添加 QSS，在哪个 widget 上添加的，QSS 的作用域是那个 widget 自己和它的所有子 widget，其实和```1```是一样的，只不这里过是在 Qt Designer 里添加，不是我们自己手动写 C++ 代码添加而已。打开 ui 文件生成的代码(ui_xxxx.h)，可以看到里面也是自动生成代码调用```setStyleSheet(qss)```添加 QSS 的，和我们写代码添加没有区别，只是在 Qt Designer 里添加的话，有时候方便一些，也可以实时看到 QSS 的效果

## Widget 加载 QSS

QSS 的作用域是Widget及子Widegt

## QApplication 加载 QSS

QSS 的作用域是整个程序

## Qt Designer 里写 QSS

```
Change styleSheet...
```

## 从文件中加载CSS

1. 建立文本文件，写入样式表内容，更改文件后缀名为qss；  
2. 在工程中新建资源文件*.qrc，将qss文件加入资源文件qrc中，此处注意prefix最好为"/"，否则在调用qss文件时会找不到文件；  
3. 通过传入路径\文件名的方式创建一个QFile对象，以readonly的方式打开，然后readAll，最后qApp->setStyleSheet就可以使qss生效。  
```C++  //该段代码写在ui界面的后台cpp文件构造函数中。
MainWidget::MainWidget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::MainWidget)
{
　　//应用样式 apply the qss style
    QFile file(":/qss/main.qss");
    file.open(QFile::ReadOnly);
    QTextStream filetext(&file);
    QString stylesheet = filetext.readAll();
    this->setStyleSheet(stylesheet);
 　file.close();
}
```



## Tricks
项目中加载CSS的方式：  
1. Qt Designer里写QSS，有实时效果后保存至文件，删除Qt Designer的QSS。
2. 从文件读取QSS赋值给QString变量
3. qApp加载QSS

```C++
// uiUtil.h
#ifndef UIUTIL_H
#define UIUTIL_H
#include <QApplication>
#include <QTextStream>
#include <QFile>
#include <QDebug>
#include <QStyle>

class UiUtil
{
public:
    static void loadQss(const QString &style); // 为整个应用程序加载 QSS
    static void unLoadQss();
    UiUtil();
};

#endif // UIUTIL_H
```

```C++
#include "uiutil.h"

UiUtil::UiUtil()
{

}

void UiUtil::loadQss(const QString &style)
{
    QFile qss(style);
    if(qss.open(QFile::ReadOnly)) {
        qDebug( "open qss succeed!" );
        QTextStream filetext(&qss);
        qApp->setStyleSheet(qss.readAll());
        qss.close();
    } else {
        qDebug( "open qss failed!" );
    }
}

void UiUtil::unLoadQss()
{
    qApp->setStyleSheet("");
}

```

```C++
#include "widget.h"
#include "uiutil.h"
#include <QApplication>
#include <QShortcut>
#include <QObject>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    Widget w;
    QShortcut *shortcut = new QShortcut(QKeySequence(QObject::tr("Ctrl+L")), &w);
    QShortcut *shortcutshut = new QShortcut(QKeySequence(QObject::tr("Ctrl+K")), &w);

    QObject::connect(shortcut, &QShortcut::activated, [] {
        UiUtil::loadQss(":/style.qss");
    });
    QObject::connect(shortcutshut, &QShortcut::activated, [] {
         UiUtil::unLoadQss();
    });
    w.show();
    return a.exec();
}

```