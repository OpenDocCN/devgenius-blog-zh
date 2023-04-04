# 从资助者数据库的 Elsevier 专家查询中筛选审查者

> 原文：<https://blog.devgenius.io/filtering-reviewers-from-expert-lookup-against-funder-datawarehouse-database-86df621191b5?source=collection_archive---------19----------------------->

资助者使用 Elsevier Expert Lookup 查找已提交研究提案的评审者列表。仍然需要花费大量时间来检查从专家查找中选择的评审者是否已经与资助者联系，或者他们是否已经评估了来自资助者的提议。

如果你喜欢，请在 https://www.buymeacoffee.com/rahulli 买咖啡给我

下面是一个脚本，资助者可以很容易地从资助者的数据库中的专家查找筛选审查者名单。

您必须有一个相关提案的审阅者列表，作为一个 csv 文件，从 Expert Lookup 下载。此外，您还可以使用资助方数据库中的提案信息、申请人建议和阻止的评审人以及评审人的 excel 文件。总共有五个输入文件。

下面的 github 存储库中给出了文件的空示例(申请人信息、专家查找的审核人、建议的审核人、被阻止的审核人、数据库)及其列名:

[](https://github.com/RaThorat/filtering_reviewers_from_Expert_Lookup) [## GitHub-RaThorat/filtering _ reviewers _ from _ Expert _ Lookup:资助者使用专家查找来获得一个列表…

### 资助者使用专家查找来查找提交的研究提案的评审者列表。仍然需要很多时间来…

github.com](https://github.com/RaThorat/filtering_reviewers_from_Expert_Lookup) 

# 韩德玲桂

如果您在 IDE 中运行以下代码，将出现一个图形用户界面(GUI ):单击“get proposal info ”(获取建议信息)以上传申请人信息文件。单击“获取建议的审核人”上传申请人建议的审核人文件。单击“获取数据仓库审核者”从资助方上传审核者数据库(这可能需要一两分钟。)单击“获取专家查找审阅者”从专家查找中上传 csv 文件。点击“获取非审核者”上传非审核者。然后关上窗户。

该脚本将自动运行，然后您将在运行该文件的同一文件夹中获得一个筛选出的审阅者的 excel 文件。

# 代码的准备:

导入各种模块进行分析:

```
import pandas._libs.tslibs.base
import numpy as np
import os
os.getcwd()
from tkinter import filedialog
import pandas as pd
from tkinter import*
import re
from nameparser import HumanName
```

构造函数:

```
#function to get proposals information
def getproposalinfo():
    global df_pv3
    import_file_path1 = filedialog.askopenfilename()
    df_pv3 = pd.read_excel (import_file_path1)

#function to get suggested reviewers    
def getsuggestedreviewers():
    global df_SR
    import_file_path2 = filedialog.askopenfilename()
    df_SR = pd.read_excel (import_file_path2)

#function to get reviewers information from YOUR datawarehouse    
def getdwhreviewers():
    global df_dwh
    import_file_path3 = filedialog.askopenfilename()
    df_dwh = pd.read_excel (import_file_path3)

#function to get Expert Lookup CSV
def getelcsv():
    global df_RL
    import_file_path4 = filedialog.askopenfilename()
    df_RL = pd.read_csv(import_file_path4)

#function to get list of non reviewers
def getnonreviewers():
    global df_nonref
    import_file_path5 = filedialog.askopenfilename()
    df_nonref = pd.read_excel(import_file_path5)
```

使用 Tkinter 开发用户界面:

```
#Tkinter buildup    
root=Tk()
topframe=Frame(root)
topframe.pack(side=TOP)
bottomframe=Frame(root)
bottomframe.pack(side=BOTTOM)
redbtn=Button(topframe, command=getproposalinfo, text="get proposal info", fg="Red")
redbtn.pack(side=LEFT)
bluebtn=Button(topframe, command=getsuggestedreviewers, text="get suggested reviewers", fg="Blue")
bluebtn.pack(side=LEFT)
greenbtn=Button(topframe, command=getdwhreviewers, text="get datawarehouse reviewers", fg="Green")
greenbtn.pack(side=LEFT)
blackbtn=Button(bottomframe, command=getelcsv, text="get Expert Lookup reviewers", fg="Black")
blackbtn.pack(side=LEFT)
yellowbtn=Button(bottomframe, command=getnonreviewers, text="get non reviewers", fg="Violet")
yellowbtn.pack(side=LEFT)

root.mainloop()
```

# 正在处理专家查找中的审阅者列表:

```
#replacing non existent values with nan
df_RL =df_RL.replace(r'^\s*$', np.nan, regex=True)
#resetting index
df_RL=df_RL.reset_index()
grantnumber=df_RL['GrantNumber'][0]
df_pv3=df_pv3[df_pv3['Aanvraag dossier']==grantnumber]
#resetting index
df_pv3=df_pv3.reset_index()
#For getting full name of the applicant from pivot table
df_RL['Hoofdaanvrager']=df_pv3['Hoofdaanvrager']

#Renaming columns
df_RL.rename(columns = {'GrantNumber':'Dossiernummer', 'Name':'Referent', 'Organization':'Inst','Country':'Land',
                        'Scopus Author Detail Page Link': 'ScopusLink', 'Web Search link':'WebSearchLink', 'Notes':'Opmerking'}, inplace=True)
#extracting details about the reviewers, such as title, first name, surname, gender
Title=[]
First_Name=[]
Middle_Name=[]
Last_Name=[]
for person in df_RL['Referent']:
    name = HumanName(person)
    Title.append(name.title)
    First_Name.append(name.first)
    Middle_Name.append(name.middle)
    Last_Name.append(name.last)
df_RL.loc[:, ('Rangorde')] = ""
df_RL.loc[:, ('Titel')]=Title
df_RL.loc[:, ('Voornaam')] = First_Name
df_RL.loc[:, ('Tuss')]=Middle_Name
df_RL.loc[:, ('Achternaam')] = Last_Name
df_RL.loc[:, ('URL')]=''
df_RL.loc[:, ('m/v')]=''
df_RL =df_RL.replace(r'^\s*$', np.nan, regex=True)
```

# 处理申请者或其他人建议的考核人

```
#Renaming columns of suggested reviewers
df_SR.rename(columns = {'Grant No.':'Dossiernummer', 'Last Name':'Achternaam', 'First Name':'Voornaam', 'Affiliation':'Inst','Country':'Land', 'Scopus Author ID': 'ScopusLink'}, inplace=True)

#filtering the suggested reviewers for the given proposal
df_SR=df_SR[df_SR['Dossiernummer']==grantnumber]
df_SR=df_SR.reset_index()
```

在主审阅者列表(RL)中插入的建议审阅者行的“opmerking”列中撰写注释

```
Bron=[]#Empty list
for i in range(len(df_RL)):
    for j in range(len(df_SR)):
        if df_RL['Achternaam'][i]==df_SR['Achternaam'][j]:
            item='Aanvrager, geen COI'# if the reviewer is selected by the applicant
        else: item='Expert Lookup'
        Bron.append(item)
#creation of 'Bron' column
df_RL['Bron'] = pd.DataFrame({'col':Bron})

#rearranging columns
df_RL=df_RL[['Rangorde','Bron', 'Dossiernummer', 'Referent','Titel','Voornaam','Tuss','Achternaam', 'Inst', 'Land', 'Email','m/v', 'URL', 'Opmerking', 'ProposalTitle', 'ProposalLink','Applicants','Hoofdaanvrager', 'ScopusLink', 'WebSearchLink', ]]
df_RL=df_RL.reset_index(drop=True)
#df_RL.head(2)
```

处理建议的审阅者列表并将其添加到审阅者主列表:

```
df_SR1=df_SR[['Dossiernummer', 'Achternaam', 'Voornaam',
       'ScopusLink', 'Email', 'Inst', 'Land']]# adding column with constant value
if len(df_SR1.index)>0:
    df_SR1.loc[:, ('Bron')] = 'Aanvrager, maar misschien COI'
    df_SR1.loc[:, ('Rangorde')] = ''
    df_SR1.loc[:, ('Referent')] = ''
    df_SR1.loc[:, ('Titel')]=' '
    df_SR1.loc[:, ('Tuss')]=''
    df_SR1.loc[:, ('URL')]=' '
    df_SR1.loc[:, ('m/v')]=''
    df_SR1.loc[:, ('Opmerking')] = ''
    df_SR1.loc[:, ('ProposalTitle')]=df_RL['ProposalTitle']
    df_SR1.loc[:, ('Referent')]= ''
    df_SR1.loc[:, ('ProposalLink')]=df_RL['ProposalLink']
    df_SR1.loc[:, ('Applicants')]=df_RL['Applicants']
    df_SR1.loc[:, ('Hoofdaanvrager')]=df_RL['Hoofdaanvrager']
    df_SR1.loc[:, ('WebSearchLink')]=''
    #rearranging columns
    df_SR1=df_SR1[['Rangorde','Bron', 'Dossiernummer', 'Referent','Titel','Voornaam','Tuss','Achternaam', 'Inst', 'Land', 'Email','m/v', 'URL', 'Opmerking', 'ProposalTitle', 'ProposalLink','Applicants', 'Hoofdaanvrager','ScopusLink', 'WebSearchLink']]

#adding suggested reviewers to reviewers master list    
df_RL_ = pd.concat([df_RL, df_SR1], axis=0)

#dropping duplicate records
df_RL_=df_RL_.drop_duplicates(subset=['Dossiernummer', 'Achternaam'],keep="first")

df_RL_=df_RL_.sort_values('Dossiernummer')

df_RL_1=df_RL_.reset_index(drop=True)
```

检查“数据框架的形状”审阅者列表是否保持不变。这意味着审阅者的来源列表中没有添加额外的列。

# 清理审阅者列表

执行以下操作

**i .栏目名称变更**

NeR:否定回答，NoR:未回答，PoR:肯定回答，

**二世。删除不必要的列**

NeR2021 '，' NeR0 '，' NoR2021 '，' NoR0 '，' PoR2021 '，' PoR0 '，

**三。过滤非审核者**

```
df_dwh.columns=['Referent','Achternaam', 'Onbeschikbaar', 'Email', 'Inst', 'Geslacht', 'Opmerking', 'Overleden', 'Voornaam', 'Land', 'Dossiernummer', 'Hoofdaanvrager','NeR2022','NeR2021','NeR2020', 'NeR2019','NeR2018', 'NeR2017',
                'NeR2016', 'NeR2015','NoR2022','NoR2021', 'NoR2020', 'NoR2019', 'NoR2018','NoR2017', 'NoR2016','NoR2015','PoR2022', 'PoR2021','PoR2020', 'PoR2019', 'PoR2018', 'PoR2017', 'PoR2016', 'PoR2015', 'Eindtotaal']

df_dwh1 = df_dwh.iloc[6:]
df_dwh1=df_dwh1.reset_index(drop=True)
```

# 查找评论者的称呼(博士或教授)

(假设:只选择审稿人的第一个称呼。)

```
df_dwh1.loc[:, ('ScopusLink')]=''
df_dwh1.loc[:, ('WebSearchLink')]=''
df_dwh1.loc[:, ('ProposalTitle')]=''
df_dwh1.loc[:, ('ProposalLink')]=''

#df_dwh1.head()
```

将审核人列表数据框与“电子邮件”上的数据库数据框合并:

```
#Merging reviewers list dataframe with database dataframe on 'email'df_RL2=df_RL_1.merge(df_dwh1, on='Email', how='left', indicator=True)

#replacing NaN values with zero for further calculation
df_RL2['Eindtotaal'].fillna(0, inplace=True)

#changing the type of column Eindtotaal for comparison
np.int64(df_RL2['Eindtotaal']) 
```

在“opmerking”栏中填写已经注册的评审者是在资助者数据库中还是新加入数据库的

```
 df_RL2.loc[df_RL2['Eindtotaal']!=0, 'Opmerking']='Info is in datawarehouse'

df_RL2.loc[df_RL2['Eindtotaal']==0, "Opmerking"]='New'
```

# 检查数据库中存在的审查者是否在 2021 年为资助者工作过

**替换 NaN 值，改变列“2021”的类型:**

```
df_RL2['PoR2021'].fillna(0, inplace=True)
df_RL2['NoR2021'].fillna(0, inplace=True)
df_RL2['NeR2021'].fillna(0, inplace=True)np.int64(df_RL2['PoR2021'])
np.int64(df_RL2['NoR2021'])
np.int64(df_RL2['NeR2021'])
```

**设置检查过滤器:**

```
df_RL2.loc[df_RL2['PoR2021']!=0, 'Opmerking']='Info is in datawarehouse, has assessed another application or round this year (2021)'

df_RL2.loc[df_RL2['NoR2021']!=0, 'Opmerking']='Info is in datawarehouse, did not respond to another request or round this year (2021)'

df_RL2.loc[df_RL2['NoR2021']!=0,'Opmerking']='Info is in datawarehouse, said no to review this year (2021) on another application or round'
```

**清理数据帧，删除不必要的列，重命名列，更改列的顺序以适应最终结果:**

```
df_RL2=df_RL2.drop(columns={'Referent_y', 'Titel_x',
       'Achternaam_y', 'Onbeschikbaar', 'Inst_y', 'm/v', 'Opmerking_y', 'Opmerking_x',
       'Overleden', 'Voornaam_y', 'Land_y', 'Dossiernummer_y',
       'ProposalTitle_y', 'ProposalLink_y', 'Hoofdaanvrager_y',
       'ScopusLink_y', 'WebSearchLink_y',  'NeR2022','NeR2021','NeR2020', 'NeR2019', 'NeR2018',
       'NeR2017', 'NeR2016', 'NeR2015', 'NoR2022','NoR2021', 'NoR2020','NoR2019',
       'NoR2018', 'NoR2017', 'NoR2016', 'NoR2015', 'PoR2022','PoR2021', 'PoR2020',
        'PoR2019', 'PoR2018', 'PoR2017', 'PoR2016', 'PoR2015', 'Eindtotaal',})

df_RL2=df_RL2.rename(columns={'Dossiernummer_x':'Dossiernummer', 'Referent_x':'Referent', 'Titel_y':'Titel',
       'Voornaam_x':'Voornaam', 'Achternaam_x':'Achternaam', 'Inst_x':'Inst', 'Land_x':'Land', 
'ProposalTitle_x':'ProposalTitle', 'ProposalLink_x':'ProposalLink', 'Hoofdaanvrager_x':'Hoofdaanvrager','ScopusLink_x': 'ScopusLink', 'WebSearchLink_x':'WebSearchLink','Geslacht':'m/v'})

df_RL2=df_RL2[['Dossiernummer', 'Bron', 'Referent', 'Titel','Voornaam','Achternaam', 'Inst', 'Land', 'Email','m/v','Opmerking','ProposalTitle', 'ProposalLink', 'Applicants', 'Hoofdaanvrager',
       'ScopusLink', 'WebSearchLink']]

df_RL2 = df_RL2.assign(Rangorde='')
df_RL2 = df_RL2.assign(Tuss='')
df_RL2 = df_RL2.assign(URL='')

df_RL2 =df_RL2.replace(r'^\s*$', np.nan, regex=True)
```

更改列的顺序

```
df_RL2 = df_RL2[['Rangorde','Bron','Dossiernummer', 'Referent', 'Titel', 'Voornaam', 'Tuss', 'Achternaam',
       'Inst', 'Land', 'Email', 'Opmerking', 'URL', 'm/v','ProposalTitle', 'ProposalLink', 'Applicants','Hoofdaanvrager',
       'ScopusLink', 'WebSearchLink']]
```

# 搜索之前在 2018 年、2019 年和 2020 年申请的申请人

如果当前一轮 2021 年的申请人已经申请了前几年的申请，则在审核当前申请时，可以考虑这些申请的审核人。审查者可能会被要求再次审查申请，因为他们熟悉该主题。或者可以避开这些评审员，以防止对申请产生偏见。

在资助者数据库中，我们搜索给定年份的申请号的公共字符串。例如，对于 2020 年，使用的申请号包含 2020 年和补贴名称。下面使用该公共字符串来分隔 2018 年、2019 年和 2020 年的申请。

```
df_dwh_her=df_dwh1[(df_dwh1['Dossiernummer'].str.contains('COMMON STRING of APPLICATION NUMBERS 2018')) |(df_dwh1['Dossiernummer'].str.contains('COMMON STRING of APPLICATION NUMBERS 2019'))|(df_dwh1['Dossiernummer'].str.contains('COMMON STRING of APPLICATION NUMBERS 2020'))]

df_dwh_her=df_dwh_her.reset_index()
```

检查数据仓库数据库和应用程序信息中的列

```
 df_dwh_her.columnsdf_pv3.columns
```

将数据仓库数据帧与提案信息数据帧合并，得到重复申请的数据帧

```
df_her=df_pv3.merge(df_dwh_her, on='Hoofdaanvrager', how='left', indicator=True)
```

检查复制列

```
df_her.columns
```

df_pv2 列是 Dossiernummer、Hoofdaanvrager、Hoofdaanvrager_Achternaam、Voornaam、Geslacht、Promotiedatum、C_Email、communicate Taal、HoofdOrganisatie、C_Organisatie、C_Postcode、C_Plaats、Titel、Samenvatting、Hoofddiscipline、Subdiscipline、Woord、SubsidieRonde_Naam、atl_Ingetrokken、atl _ Gehonoreerd

删除不必要的列并重命名剩余的列

```
df_her=df_her.drop(columns='Dossiernummer')df_her=df_her.rename(columns={'Aanvraag dossier':'Dossiernummer', 'Titel_y':'Titel', 'Geslacht':'m/v'})
```

检查申请人‘hoofdaanvrager’是否在 2018 年、2019 年、2020 年申请过:

```
df_her1=df_her[['Dossiernummer','Referent', 'Titel','Voornaam','Achternaam', 'Inst', 'Land', 'Email','m/v','Opmerking','Hoofdaanvrager', 'NeR2018','NoR2018','PoR2018', 'NeR2019','NoR2019','PoR2019', 'NeR2020','NoR2020','PoR2020']]

df_her1 = df_her1.assign(Rangorde='')
#df_her1 = df_her1.assign(Info_in_datawarehouse='Ja, herindiening')
df_her1 = df_her1.assign(Tuss='')
df_her1 = df_her1.assign(URL='')
df_her1 = df_her1.assign(ProposalTitle='')
df_her1 = df_her1.assign(ProposalLink='')
df_her1 = df_her1.assign(ScopusLink='')
df_her1 = df_her1.assign(WebSearchLink='')

i=0
k=df_her1.columns.get_loc('Hoofdaanvrager')
l=len(df_her1.index)
m=len(df_RL2.index)
n=df_RL2.columns.get_loc('Hoofdaanvrager')
for i in range(0,l):
    j=0
    for j in range(0,m):
        if df_her1.iloc[i,k]==df_RL2.iloc[j,n]:
            df_her1['ProposalTitle'][i]=df_RL2['ProposalTitle'][j]
            df_her1['ProposalLink'][i]=df_RL2['ProposalLink'][j]         
#df_her1.head(2)
```

# 检查资助者数据库中已知的审查者是否审查过 2018 年的申请

```
#replacing NaN values with zero for further calculation
df_her1['PoR2018'].fillna(0, inplace=True)
df_her1['NoR2018'].fillna(0, inplace=True)
df_her1['NeR2018'].fillna(0, inplace=True)

#changing the type of column 2021 for comparison
np.int64(df_her1['PoR2018'])
np.int64(df_her1['NoR2018'])
np.int64(df_her1['NeR2018'])

df_her1.loc[df_her1['PoR2018']==1, 'Opmerking']='Info is in datawarehouse, reviewed an application from the same applicant last time (2018)'

df_her1.loc[df_her1['NoR2018']==1, 'Opmerking']='Info is in datawarehouse, did not respond last time (2018) to review an application from the same applicant'

df_her1.loc[df_her1['NeR2018']==1, 'Opmerking']='Info is in datawarehouse, said no last time (2018) to review an application from the same applicant'
```

# 检查资助者数据库中已知的审查者是否审查过 2019 年的申请

```
#replacing NaN values with zero for further calculation
df_her1['PoR2019'].fillna(0, inplace=True)
df_her1['NoR2019'].fillna(0, inplace=True)
df_her1['NeR2019'].fillna(0, inplace=True)

#changing the type of column 2021 for comparison
np.int64(df_her1['PoR2019'])
np.int64(df_her1['NoR2019'])
np.int64(df_her1['NeR2019'])

df_her1.loc[df_her1['PoR2019']==1, 'Opmerking']='Info is in datawarehouse, reviewed an application from the same applicant last time (2019)'

df_her1.loc[df_her1['NoR2019']==1, 'Opmerking']='Info is in datawarehouse, did not respond last time (2019) to review an application from the same applicant'

df_her1.loc[df_her1['NeR2019']==1, 'Opmerking']='Info is in datawarehouse, said no last time (2019) to review an application from the same applicant'
```

# 检查资助者数据库中已知的审查者是否审查过 2020 年的申请

```
#replacing NaN values with zero for further calculation
df_her1['PoR2020'].fillna(0, inplace=True)
df_her1['NoR2020'].fillna(0, inplace=True)
df_her1['NeR2020'].fillna(0, inplace=True)

#changing the type of column 2020 for comparison
np.int64(df_her1['PoR2020'])
np.int64(df_her1['NoR2020'])
np.int64(df_her1['NeR2020']) df_her1.loc[df_her1['PoR2020']==1, 'Opmerking']='Info is in datawarehouse, reviewed an application from the same applicant last time (2020)'

df_her1.loc[df_her1['NoR2020']==1, 'Opmerking']='Info is in datawarehouse, did not respond last time (2020) to review an application from the same applicant'

df_her1.loc[df_her1['NeR2020']==1, 'Opmerking']='Info is in datawarehouse, said no last time (2020) to review an application from the same applicant' df_her1 =df_her1.replace(r'^\s*$', np.nan, regex=True)

df_her1=df_her1.drop(columns={'NeR2018',
       'NoR2018', 'PoR2018', 'NeR2019', 'NoR2019', 'PoR2019','NeR2020', 'NoR2020', 'PoR2020'})

df_her1.loc[:, ('Bron')]=''
df_her1.loc[:, ('Applicants')]=''

#changing the order of the columns  
df_her1 = df_her1[['Rangorde','Bron','Dossiernummer', 'Referent', 'Titel', 'Voornaam', 'Tuss', 'Achternaam',
       'Inst', 'Land', 'Email', 'Opmerking', 'URL', 'm/v','ProposalTitle', 'ProposalLink','Applicants',  'Hoofdaanvrager',
       'ScopusLink', 'WebSearchLink']]
```

# 筛选在过去三年中审核过申请人任何提案的审核人

```
df_dwh2=df_dwh1[(df_dwh1['PoR2018']==1) |(df_dwh1['PoR2019']==1)|(df_dwh1['PoR2020']==1)]
df_dwh2 = df_dwh2.reset_index(drop=True)df_her2=df_pv3.merge(df_dwh2, on='Hoofdaanvrager', how='left', indicator=True)

df_her2=df_her2.drop(columns='Dossiernummer')
df_her2=df_her2.rename(columns={'Aanvraag dossier':'Dossiernummer','Titel_y':'Titel', 'Geslacht':'m/v'})
#To check if in 2019 or 2018 there is a reapplication
df_her3=df_her2[['Dossiernummer','Referent', 'Titel','Voornaam','Achternaam', 'Inst', 'Land', 'Email','m/v','Opmerking','Hoofdaanvrager', 'PoR2018', 'PoR2019', 'PoR2020']]
df_her3 = df_her3.assign(Rangorde='')
#df_her1 = df_her1.assign(Info_in_datawarehouse='Ja, herindiening')
df_her3 = df_her3.assign(Tuss='')
df_her3 = df_her3.assign(URL='')
df_her3 = df_her3.assign(ProposalTitle='')
df_her3 = df_her3.assign(ProposalLink='')
df_her3 = df_her3.assign(ScopusLink='')
df_her3 = df_her3.assign(WebSearchLink='')

df_her3.loc[:, ('Opmerking')]='has assessed an application from the main applicant in the past three years'

df_her3.loc[:, ('Bron')]=''
df_her3.loc[:, ('Applicants')]=''

#changing the order of the columns  
df_her3 = df_her3[['Rangorde','Bron','Dossiernummer', 'Referent', 'Titel', 'Voornaam', 'Tuss', 'Achternaam',
       'Inst', 'Land', 'Email', 'Opmerking','URL', 'm/v','ProposalTitle', 'ProposalLink','Applicants',  'Hoofdaanvrager',
       'ScopusLink', 'WebSearchLink']]
```

# 过滤非审阅者

(如果申请人提交了不希望其申请被审查的非审查人员名单):

通常按申请号提供带有非审核者姓名的 excel 文件。通常，非评审人员的姓名与大学名称和电子邮件地址一起提供，用逗号分隔。

如果姓名和其他信息(如大学名称、电子邮件地址等)一起出现在 excel 单元格中，则将非读者姓名与其他文本分开。

```
df_nonref['referent naam']=df_nonref['Naam'].str.split(',').str[0]
df_nonref.head()
First_Name=[]
Last_Name=[]
for person in df_nonref['referent naam']:
    name = HumanName(person)
    First_Name.append(name.first)
    Last_Name.append(name.last)
df_nonref['First Name'] = First_Name
df_nonref['Last Name'] = Last_Name
#pd.DataFrame({'First_Name':First_Name,'Last_Name':Last_Name})
```

这些非评审者可能出现在专家查询和资助者数据库的评审者列表中。他们仍然可以留在列表中，但要有一个说明，明确表示不得联系他们来审查给定的应用程序。

提取电子邮件地址，以便我们可以使用这些地址从审阅者数据库中进行筛选:

```
email=[]
for referenten in df_nonref['Naam']:
    # \w matches any non-whitespace character
    # @ for as in the Email
    # + for Repeats a character one or more times
    FindEmail = re.findall("([\w.-]+@[\w.-]+)", referenten)
    # run for loop on the list variable
    for l in FindEmail:
    #set value in email variable
        emaill=l
    #declare local variables to store email addresses
    email.append(emaill)
df_nonref['Email']=email
```

提取大学名称:

```
df_uni = pd.DataFrame()
for referenten in df_nonref['Naam']:
    text=referenten
    matchlist = ['Hospital','University','Universitäts','Universität','Università','Hogeschool','Labs', 'Laboratory', 'Zoo','Institute','Institut','School','Ecole','Academy', 'Universite','College','Universitaet,' '* University'] 
    # define all keywords that you need look up
    p = re.compile('^(.*?),\s+(.*?),(.*?)\.')   # regex pattern to match
    # We use a list comprehension using 'any' function to check if any of the item in the matchlist can be found in either group1 or group2 of the pattern match results
    text_match = [m.group(1) if any(x in m.group(1) for x in matchlist) else m.group(2) for m in re.finditer(p,text)]
    df_uni = df_uni.append(text_match, ignore_index=True)
df_nonref['Inst']=df_uni.iloc[0:]
```

定义非审阅者数据框架

```
df_nRef=pd.DataFrame(columns=['Proposal Number','Grant No.','Last Name', 'First Name', 'Scopus Author ID', 'OrcId', 'Email', 'Affiliation', 'Country'])
```

如有必要，从授权号中提取最后一位数字以填入提案号:

```
#df_nRef['Proposal Number']=df_nonref['Project dossier'].str.split('.').str[-1]
```

为每列赋值

```
df_nRef['Bron']='Aanvrager'
df_nRef['Dossiernummer']=df_nonref['Dossiernr']
df_nRef['Rangorde']=''
df_nRef['Referent']=df_nonref['referent naam']
df_nRef['Titel']=''
df_nRef['Achternaam']=df_nonref['Last Name']
df_nRef['Tuss']=''
df_nRef['Voornaam']=df_nonref['First Name']   
df_nRef['Email']=df_nonref['Email']
df_nRef['Inst']=df_nonref['Inst']
df_nRef['Land']=''
df_nRef['m/v']=''
df_nRef['Hoofdaanvrager']=df_nonref['Aanvrager']
df_nRef['ProposalTitle']=''
df_nRef['ScopusLink']=''
df_nRef['WebSearchLink']=''
df_nRef['URL']=''
df_nRef['Applicants']=''
df_nRef['Opmerking']='Nonreferent'
df_nRef['ProposalLink']=''
#changing the order of the columns  
df_nRef = df_nRef[['Rangorde','Bron','Dossiernummer', 'Referent', 'Titel', 'Voornaam', 'Tuss', 'Achternaam',
       'Inst', 'Land', 'Email', 'Opmerking','URL', 'm/v','ProposalTitle', 'ProposalLink','Applicants',  'Hoofdaanvrager',
       'ScopusLink', 'WebSearchLink']]
```

从资助者数据仓库数据库，从去年的重新申请人，从非审查者，从合作者附加数据帧

```
df_RL3=pd.DataFrame().append([df_RL2,df_her1, df_her3, df_nRef])
```

# 正在处理审阅者的最终列表

```
df_RL3['Bron'] = df_RL3['Bron'].fillna('Expert Lookup')

df_RL3=df_RL3.reset_index(drop=True)

df_RL3 =df_RL3.replace(r'^\s*$', np.nan, regex=True)

df_RL3 = df_RL3.dropna(how='any',
                    subset=['Achternaam'])

df_RL3["Opmerking"].replace({"-": "New"}, inplace=True)
#https://www.kite.com/python/answers/how-to-replace-column-values-in-a-pandas-dataframe-in-python#:~:text=To%20replace%20values%20in%20the,old%20values%20to%20new%20values.

df_RL3=df_RL3.sort_values('Opmerking',ascending=True).drop_duplicates('Email',keep='last')
#Source:https://kanoki.org/2019/10/25/how-to-remove-duplicate-data-from-python-dataframe/

df_RL3=df_RL3.sort_values('Applicants')

df_RL4 = df_RL3.reset_index(drop=True)
```

# Step 导出每个建议的审阅者列表

设置 openpyxl 模块

```
from openpyxl.utils.dataframe import dataframe_to_rows
from openpyxl import Workbook
from openpyxl.styles import Color, PatternFill, Font, Border, Alignment
from openpyxl.styles.differential import DifferentialStyle
from openpyxl.formatting.rule import ColorScaleRule, CellIsRule, FormulaRule
from openpyxl.formatting import Rule
import openpyxl
from typing import NoReturn
from openpyxl.worksheet.dimensions import ColumnDimension
from openpyxl.utils import get_column_letter
```

设置 excel 的首页

```
nummer=df_RL_1['Dossiernummer'][0]
df1=df_RL4[df_RL4['Dossiernummer']==nummer]
df1=df1.reset_index(drop=True)
a_list=df1['Applicants'][0]
#Applicants = "|".join(a_list)
titles=df1['ProposalTitle'].unique()
titel=titles[0]

Expert_Lookup_links=df1['ProposalLink'].unique()
Expert_Lookup_link=Expert_Lookup_links[0]

wb = Workbook()

ws = wb.create_sheet("Instructions", 0)
ws['A1']='file no'
ws['A1'].font = Font(bold=True)
ws['B1']= nummer
ws['A2']='Applicants'
ws['A2'].font = Font(bold=True)  
ws['B2']=a_list
ws['A3']='Title'
ws['A3'].font = Font(bold=True)
ws['B3']=titel
#ws['A4']='Herindiening?'
ws['A4'].font = Font(bold=True)
#ws['B4']=herindiening
ws['A5']='Instructions'
ws['A5'].font = Font(bold=True)
ws['B5']='The referees have been filtered automatically on points below. Very occasionally something slips through, so we ask you to check the suggestions.'
ws['B6']='Sometimes the websearch link doesn not work or it grabs the wrong person. Please note that.'
ws['B7']='Sometimes the sponsor does not belong to the application at all in terms of expertise. Please note that.'
ws['B8']='If the first three or four referees do not match the application, it is better not to use the list.'
ws['B9']=''
ws['B10']='The automatically generated suggestions are based on a fingerprint that is generated in Expert Lookup based on the information in the application: the name of the applicant and any co-applicants, the summary and the keywords.'
ws['B11']=''
ws['B12']='The automatically generated suggestions are filtered in "Search and Select" by:'
ws['B13']='a.appropriate expertise for the proposal (based on the above-mentioned fingerprint)'
ws['B14']='b.no involvement in the proposal -> if the sponsor is mentioned in the proposal (for example as a collaboration partner)'
ws['B15']='c.no joint publications (in the last 3 years)'
ws['B16']='d.sufficient seniority (Referees are at least an associate professor or equivalent. If there is an excellent match in expertise, a more junior referee can review.)'
ws['B17']='e.resubmissions: we do not ask the same referees as for the previous submission'
ws['B18']='f.has not worked as a reviewer in the past 6 months for another round'
ws['B19']='g.has not worked as a reviewer in the past three years to assess an application from the same MAIN applicant'
ws['B20']=''
ws['B21']='For missing information (registration order, salutation, URL, sex), you can use the WebSearchLink or the scopus id link.'
ws['B22']=''
ws['B23']='To search for additional referees you can use the Expert lookup link below. Note: you must have an Expert Lookup account to open the link.'
ws['B24']=Expert_Lookup_link
ws['A24']='EL link'
ws['A24'].font = Font(bold=True)

ws['B26'] = 'Work overview referees on the next worksheet (enter the final selection yourself in the data warehouse)'
ws['B26'].font = Font(bold=True)
```

函数来适应列单元格中的审阅者信息

```
def columns_best_fit(ws: openpyxl.worksheet.worksheet.Worksheet) -> NoReturn:
    """
    Make all columns best fit
    """
    column_letters = tuple(openpyxl.utils.get_column_letter(col_number + 1) for col_number in range(ws.max_column))
    for column_letter in column_letters:
        ws.column_dimensions[column_letter].bestFit = True
```

将该功能应用于审阅者列表 excel 表

```
columns_best_fit(ws)
```

格式化审阅者列表 excel 表格

```
df2=df1.drop(['Dossiernummer','Referent','Hoofdaanvrager','Applicants','ProposalTitle','ProposalLink'], axis=1)    
df2=df2.reset_index(drop=True)
df2=df2.sort_values('Email')
ws1 = wb.create_sheet("Referentenlijst", 1) 
for r in dataframe_to_rows(df2, index=False, header=True):
    ws1.append(r)

for cell in ws1['1'] :#+ ws[1]
    cell.style = 'Pandas'
```

格式化整行

```
red_fill = PatternFill(bgColor="FFC7CE")
dxf = DifferentialStyle(fill=red_fill)
r = Rule(type="expression", dxf=dxf, stopIfTrue=True)
r.formula = [('$J1="Info is in data warehouse, has assessed another application or round this year (2021)"')]
ws1.conditional_formatting.add("A1:O100", r)
r1 = Rule(type="expression", dxf=dxf, stopIfTrue=True)
r1.formula = [('$J1="has assessed an application from the main applicant in the past three years"')]
ws1.conditional_formatting.add("A1:O100", r1)
r2 = Rule(type="expression", dxf=dxf, stopIfTrue=True)
r2.formula = [('$J1="Info is in data warehouse, reviewed application from same applicant last time (2018)"')]
ws1.conditional_formatting.add("A1:O100", r2)
r3 = Rule(type="expression", dxf=dxf, stopIfTrue=True)
r3.formula = [('$J1="Info is in data warehouse, reviewed application from same applicant last time (2019)"')]
ws1.conditional_formatting.add("A1:O100", r3)
r4 = Rule(type="expression", dxf=dxf, stopIfTrue=True)
r4.formula = [('$J1="Info is in data warehouse, reviewed application from same applicant last time (2020)"')]
ws1.conditional_formatting.add("A1:O100", r4)
r5 = Rule(type="expression", dxf=dxf, stopIfTrue=True)
r5.formula = [('$J1="Nonreviewer"')]
ws1.conditional_formatting.add("A1:O100", r5)
column_widths = []
for row in ws1:
    for i, cell in enumerate(row):
        if len(column_widths) > i:
            if len(str(cell.value)) > column_widths[i]:
                column_widths[i] = len(str(cell.value))
        else:
            column_widths += [len(str(cell.value))]

for i, column_width in enumerate(column_widths):
    ws1.column_dimensions[get_column_letter(i+1)].width = column_width
```

正在写入输出文件:

```
output_file_name="Reviewers " + str(nummer)+" Output file"+".xlsx"
#saving the excel workbook
wb.save(output_file_name)
```