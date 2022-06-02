---
typora-copy-images-to: ../assets/images/2022-05-31-Actividad-DIVA
typora-root-url: ../
title: Actividad DIVA
---

# Actividad DIVA

## Challenge 0:

![image-20220531165049069](/assets/images/2022-05-31-Actividad-DIVA/image-20220531165049069.png)

## Challenge 1: “Insecure logging”

![image-20220531172213832](/assets/images/2022-05-31-Actividad-DIVA/image-20220531172213832.png)

![image-20220531172506373](/assets/images/2022-05-31-Actividad-DIVA/image-20220531172506373.png)

## Challenge 2: “Hardcoding issues – part 1”

![image-20220531172723624](/assets/images/2022-05-31-Actividad-DIVA/image-20220531172723624.png)

![image-20220531172744973](/assets/images/2022-05-31-Actividad-DIVA/image-20220531172744973.png)

## Challenge 3: “INSECURE DATA STORAGE – PART 1”

![image-20220531173419351](/assets/images/2022-05-31-Actividad-DIVA/image-20220531173419351.png)

## Challenge 4: “INSECURE DATA STORAGE – PART 2”

Para sacar el archivo hay que reiniciar el servicio como root primero:

```
adb root
adb pull /data/data/jakhar.aseem.diva/databases/ids2
```

![image-20220531174126876](/assets/images/2022-05-31-Actividad-DIVA/image-20220531174126876.png)

## Challenge 5: “INSECURE DATA STORAGE – PART 3”

![image-20220531174403736](/assets/images/2022-05-31-Actividad-DIVA/image-20220531174403736.png)

## Challenge 6: “INSECURE DATA STORAGE – PART 4”

Me da un error al guardar el archivo, así que luego no está...

![image-20220531175029239](/assets/images/2022-05-31-Actividad-DIVA/image-20220531175029239.png)

![image-20220531175221639](/assets/images/2022-05-31-Actividad-DIVA/image-20220531175221639.png)

## Challenge 7: “INPUT VALIDATION ISSUES – PART 1”

![image-20220531175620098](/assets/images/2022-05-31-Actividad-DIVA/image-20220531175620098.png)

## Challenge 8: “INPUT VALIDATION ISSUES – PART 2”

Truco: para escribir el texto desde adb, donde podemos copiar y pegar:

```
adb shell input text "file:///data/data/jakhar.aseem.diva/shared_prefs/jakhar.aseem.diva_preferences.xml"
```

![image-20220531180401360](/assets/images/2022-05-31-Actividad-DIVA/image-20220531180401360.png)

## Challenge 9: “ACCESS CONTROL ISSUES – PART 1.”

![image-20220531180815217](/assets/images/2022-05-31-Actividad-DIVA/image-20220531180815217.png)

![image-20220531180840875](/assets/images/2022-05-31-Actividad-DIVA/image-20220531180840875.png)

No he tocado el botón, ¡lo juro!

## Challenge 10: “ACCESS CONTROL ISSUES – PART 2.”

No funciona, por lo visto...

![image-20220602162341267](/assets/images/2022-05-31-Actividad-DIVA/image-20220602162341267.png)

## Challenge 11: “ACCESS CONTROL ISSUES – PART 3.”

Este tampoco va...

## Challenge 12: “HARDCODING ISSUES – PART 2.”

![image-20220602164152537](/assets/images/2022-05-31-Actividad-DIVA/image-20220602164152537.png)

![image-20220602163044350](/assets/images/2022-05-31-Actividad-DIVA/image-20220602163044350.png)

## Challenge 13: “INPUT VALIDATION ISSUES – PART 3”

![image-20220602163500576](/assets/images/2022-05-31-Actividad-DIVA/image-20220602163500576.png)

![image-20220602163828539](/assets/images/2022-05-31-Actividad-DIVA/image-20220602163828539.png)

![image-20220602163908644](/assets/images/2022-05-31-Actividad-DIVA/image-20220602163908644.png)
