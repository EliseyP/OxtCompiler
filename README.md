# OxtCompiler
Oxt compiler from Bernard Marcelly fixed for LibreOffice ver > 6

ott-файл шаблона с заготовками для сборки расширения.    

После версии LibreOffice > 6 cборка расширения стала завершаться аварийно.   
Возможная причина:  

при запуске `Compile2LibO()` значение `FsetHelp` оказывалось по модулю верное, но **отрицательное**.

Соответственно в функции `M1a.beginDescription()` код:
   
`setAllowed(InstrFlags, FsetTooltip +FsetExtensionDescription +FsetLicense _`  
&nbsp;&nbsp;&nbsp;&nbsp;`+FsetHelp +FsetIcon +FsetPlatform _`  
&nbsp;&nbsp;&nbsp;&nbsp;`+FsetDisplayName +FsetPublisherName +FsetReleaseNotes)`

заменен на: 

`setAllowed(InstrFlags, FsetTooltip +FsetExtensionDescription +FsetLicense _`  
&nbsp;&nbsp;&nbsp;&nbsp;`+ABS(FsetHelp) +FsetIcon +FsetPlatform _`  
&nbsp;&nbsp;&nbsp;&nbsp;`+FsetDisplayName +FsetPublisherName +FsetReleaseNotes)`


#### Проблемы:  
Если Basic библиотека была экспортирована и используется при создании раширения, то если при установке эта же библиотека остается зарегистрированной в Office, расширение скорее всего не установится (для установки - удалить библиотеку).
