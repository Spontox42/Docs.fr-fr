---
title: Mettre à jour les pages générées dans une application ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre à jour les pages générées dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477213"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Mettre à jour les pages générées dans une application ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale. Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être **Release Date** (en deux mots).

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Mettre à jour le code généré

Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

Cliquez avec le bouton droit sur une ligne ondulée rouge > **Actions rapides et refactorisations**.

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](da1/qa.png)

Sélectionnez `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations en haut de la liste](da1/da.png)

  Visual Studio ajoute `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Précédent : Utilisation de SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Suivant : Ajouter une fonction de recherche](xref:tutorials/razor-pages/search)
