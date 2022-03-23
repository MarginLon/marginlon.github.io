---
title: Qt多语言
description: 
toc: true
authors:
tags:
categories:
series:
date: '2022-03-23'
lastmod: '2022-03-23'
draft: false
weight: 1
---

<!--more-->
## 1. 生成ts文件
- 将需要翻译的字符串加上 `tr( ) `。若没有在 `QObject `子类的成员函数中，那么可以直接使用`QCorerApplication::translate( ) `函数亦或` QT_TR_NOOP( ) `宏和` QT_TAANSLATE_NOOP( ) `宏。这两个宏仅对文本进行标记来方便 lupdate工具 提取。
- `TRANSLATIONS += English.ts \  
				   Chinese.ts`
- 工具->外部->QT语言家->更新翻译(lupdate)
- 至此，ts文件生成。
## 2. 生成qm文件
- 利用Qt Linguist打开生成的ts文件，手动添加对应的语言翻译
- 工具->外部->QT语言家->部署翻译(lrelease)
- 至此，qm文件生成。
## 3. 载入qm语言包
```cpp
    connect(ui.langChange, &QPushButton::clicked, this, [=]{
        m_bDisplayChinese = !m_bDisplayChinese;
        if(m_bDisplayChinese){
            m_qTranslator.load(":/popup_zh_CN.qm");
        } else {
            m_qTranslator.load(":/popup_en_CN.qm");
        }
        qApp->installTranslator(&m_qTranslator);

        ui.retranslateUi(this);
    });

```
## 4. 重新设置显示界面
```cpp
void LinguistWidget::retranslateUi()
{
    m_pBtnChangeLanguage->setText(tr("Change Language"));   //这里要用tr包起来，不然语言家无法识别
}
```