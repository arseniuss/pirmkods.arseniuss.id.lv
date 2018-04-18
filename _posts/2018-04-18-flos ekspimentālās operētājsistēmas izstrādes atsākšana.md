---
layout: post
title: "flos ekspimentālās operētājsistēmas izstrādes atsākšana"
category: flos
disqus: true
excerpt_separator: <!--more-->
---
<img align="right" src="https://raw.githubusercontent.com/arseniuss/flos/master/docs/logo.png" style="margin: 10px"> 

Kādā piektdienas vakarā, izpildot savas _beerware_ prasības, man sanāca sastāpt kādu zināmu puisi. Tas tiešām bija negaidīti! It kā puisi jau no tehnikuma laikiem zinu (arī programmētājs), bet nebiju gaidījis, ka viņš un arī citi būtu uzgājuši, manuprāt, muļķīgos mēģinājumus uztaisīt kaut ko tādu, ko varētu nosaukt par operētājsistēmu.

Protams, es neatturējos no sarunas. Alus bija, vakars - labs; kāpēc gan nē? Tika pieminēti knifiņi, kurus es izmantoju, lai man nevajadzētu čakarēties, kas viņam likās ģeniāli. Kā arī daudzas citas lietas, kas man atstāja labu iespaidu gan par savu "sīkumu", gan arī vispār par cilvēku ieinteresētību arī sarežģītās lietas.
<!--more-->

Un tā, lūk, ... viens vakars, trīs litri aliņa lika man atsākt visu no jauna.

Teikšu godīgi, man ir ieraža izmest vecos menrakstus (šeit: kodu) miskastē un sākt no sākuma. Man ik pa laikam rodas sajūta, ka es varu labāk, un šis ir mans eksperiments, zināšanu pārbaude, bet arī reizē lolojums. 

Jau kādu laiku esmu domājis, ka varētu izveidot savu programmēšanas valodu. Lūk, tas būtu interesanti! Bieži vien šķiet, ka visas pašreizējās nav īsti man - parāk šauras, pārāk neietekmējamas - ne man.

Es sāku rakstīt pseidokodu, kā, piemēram, noteiktas lietas es intuitīvi rakstītu:

    type List struct {
        head *
        tail *
    }

    type Node struct {
        data *
        next *
        prev *
    }
    
    func Node.Next() *Node { ... } 

Tā lēnām es nonācu pie tā, ka mans pseidokods izrādas vistuvāk atgādina `go` valodu. Tas gan man bija brīnums! Biju dzirdējis par tās valodas iespējām un radītājiem, bet nekad nebiju izmantojis. 

Pēc divām dienām - nē. Šis nav tas! Kaut sintakse, šķiet, ir līdzīga, tomēr domu gājiens man ir aizgājis tālāk. Pareizāk: citos ceļos.

## Jauns sākums

Neilgi pēc tā vakara es arhivēju veco repozitoriju ([flos.old@github](https://github.com/arseniuss/flos.old)) un augšupielādēju jaunās idejas ([flos@github](https://github.com/arseniuss/flos)). Man bija doma darīt lietas līdzīgi kā savulaik darīja _Plan9_ izstrādātāji: izveidot vienu mapi `source`, kur likt iekšā visu kodu, kas tiks izmantots.

Galvenā problēma vairāk ir ar bibliotēkām, kuras izmantot. Es jau sen biju domājis, ka vislabāk ir veidot lietas, kuras minimāli atkarīgas no C vai kādas citas valodas. Bet te atkal ir vistas un olas problēma: ja es gribu rakstīt savā programmēšanas valodā, man vispirms ir jāuzraksta kompilators, bet to vajag rakstīt citā valodā (visticamāk C).

Bet es varu labāk! Var rakstīt lietas C valodā, bet bināri simbolu nosaukumi bibliotēkā var būt tādi paši, ja tas būtu rakstīts no manas valodas!

Uzsāku rakstīt un drīz sapratu, ka nebūs labi. Man pietrūkst daudzas nepieciešamas lietas, lai varētu pat uzrakstīt mazāko, ko vajadzētu kodolam. Un es gribēju takš visu rakstīt savā programmēšanas valodā!

"Labi," es teicu sev. "Katrai bibliotēkai sākumā būs divas versijas: 1. C valodā rakstītā, kas atradīsies mapē `csource` un 2. (vēlāk) manā valodā rakstītā mapē `source`. ... OK, turpinam!"

Paķēru sen senos parsera, skenera un leksijas mēģinājumus un skatījos, kā viss ietu kopā. "Labi, nē, pagaidām! ... Vai nebūtu forši, ka man nevadzētu visu jau uzreiz uzrakstīt? Varētu kompilatoru uzrakstīt ar Linux kodolu apakšā un tad pielāgot! ... O, jā!"

Tikpat juceklīgi, cik es atklāju problēmas, tikpat arī es tās risināju. Vienu vakaru rakstīju UTF-8 apstrādes bibliotēku, vienu vakaru rakstīju kompilatora AST un tipu strukūras; citu - bibliotēku, kas runā ar kodolu. Tā ik pa laikam lēnām un mierīgi tuvojoties savam mērķim.

## Programmēšanas valodas projektēšana

Nav īsti svarīgi kā tu lietas nosauc. Tie ir tikai rīki, lai kaut ko panāktu. Bet tomēr ir jābūt kādai vienotai idejai. 

Es nu jau vairs neatceros, kāpēc savu eksperimentu nosaucu par _flos_. Kaut kā tas nosaukums aizgāja no fraktāļa bildes, kuru savulaik esmu saglabājis no kādas faktālu ģenerēšanas programmas. Likās piemēroti un labi.

Tātad, ja mana operētājsistēma tiek saukta par _flos_ (latv. _puķe_), tad valodu, kura tā ir rakstīta varētu nosaukt par _cell_ (latv. _šūna_). Kāpēc gan ne?

Kaut nosaukuma atrašana bija diezgan vienkārša, pati valodas projektēšana gan nē. Man ir lielas ambīcijas un vēlmes, ko es vēlos no šīs valodas. Idejas par interpretāciju, izmantošanu kodolā, lietoņu būvēšanu un arī izmantošanu čaulā un saskarnē. Vispār jau būtu forši, ja viss tiek rakstīts pēc viena principa, bet ar dažādiem valodas dialektiem. Tas, manuprāt, uztur zināmu kārtību un sapratni kas kur ir.
Un arī ļoti labi iet visā skatā: _puķes takš sastāv no dažādām šūnām, vai ne?_

## Plāni un problēmas

Šobrīd es esmu knapi izveidojis bibliotēku sistēmas izsaukumiem uz Linux kodolu. Tajā nav nekā no GNU koda - viss rakstīts no nulles pašam. Bet ar to nepietiek. Būtu jāuzraksta gan bibliotēka, kas spēj nolasīt failus (`libstd.file`), gan arī sistēmas funkcijām (`libos.linux` un `libos.flos`), gan teksta apstrādei (`libstd.utf`). Tās visas tiks izmantotas kompilatorā, gan ir jābūt bināri tā it kā būtu būvētas no paša kompilatora. 

C valodā nav vārdtelpas (angl. _namespaces_), bet es varu linkošanas laikā mainīt simbolu nosaukumus. Bet reizē būtu noderīgi spēt rakstīt lietas arī C valodā. Man tik un tā vajadzēs portēt lietas, lai paātrinātu paveikto pārizmantojot. Visvairāk - LLVM.

Domāju, ka tērēšu savus spēkus uz kompilatora izveidošanu ar visām nepieciešamībām, lai caur `chroot` varētu imitēt visu, ko man vajag!

Redzēs!
