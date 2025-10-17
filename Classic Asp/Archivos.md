---
title: Archivos
layout: home
parent: Classic Asp
nav_order: 1
---

## Archivos

### contar archivos

```
set folder = fs.GetFolder(ruta)
Set filez = folder.Files 
FileCount = folder.Files.Count
```

### Buscar archivos

```
Set fs=Server.CreateObject("Scripting.FileSystemObject")
ruta = ""
If (fs.FileExists(ruta))=true Then
      Response.Write("File c:\winnt\cursors\3dgarro.cur exists.")
	  
Else
      Response.Write("File c:\winnt\cursors\3dgarro.cur does not exist.")
End If
```

### buscar si carpeta existe

```
set fs = createobject("scripting.filesystemobject")
if fs.FolderExists("d:\carpeta\")=false then
'	response.write "no lo encontro"
response.write "	<tr><td align='center'>Sin Archivos Creados para este Periodo</td></tr>"
end if
```

### Crear Carpeta

```
set fs=Server.CreateObject("Scripting.FileSystemObject")
ruta = "d:\carpeta"
set fs = createobject("scripting.filesystemobject")
if fs.FolderExists(ruta)=false then		
    set nfolder = fs.createfolder(ruta)
end if
```

### eliminar archivo

```
fs.DeleteFile(ruta)
```

### leer el texto de los archivos

```
ruta = "d:\carpeta"
rutav = "/cotizaciones/"
set fs = createobject("scripting.filesystemobject")
set folder = fs.GetFolder(ruta)
encontrado = false
for each item in folder.Files

    set t=fs.OpenTextFile(ruta & "\" &item.Name ,1)
    do while t.AtEndOfStream = false
        ruts = t.ReadAll
        if instr(ruts, rst("rut")) > 0 then
            encontrado = true
        end if
    loop

next
t.Close
Set t=Nothing
Set fs=Nothing
```

### listar archivos

```
set fs = createobject("scripting.filesystemobject")
set folder = fs.GetFolder(ruta)
for each item in folder.Files
    response.Write "<br>" & item.Name
next 
```

