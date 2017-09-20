# <a name="adding-validation"></a><span data-ttu-id="b34b7-101">Adicionando uma validação</span><span class="sxs-lookup"><span data-stu-id="b34b7-101">Adding validation</span></span>

<span data-ttu-id="b34b7-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b34b7-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b34b7-103">Nesta seção, você adicionará a lógica de validação ao modelo `Movie` e garantirá que as regras de validação são impostas sempre que um usuário criar ou editar um filme.</span><span class="sxs-lookup"><span data-stu-id="b34b7-103">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="b34b7-104">Mantendo o processo DRY</span><span class="sxs-lookup"><span data-stu-id="b34b7-104">Keeping things DRY</span></span>

<span data-ttu-id="b34b7-105">Um dos princípios de design do MVC é o [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (“Don't Repeat Yourself”).</span><span class="sxs-lookup"><span data-stu-id="b34b7-105">One of the design tenets of MVC is [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ("Don't Repeat Yourself").</span></span> <span data-ttu-id="b34b7-106">O ASP.NET MVC incentiva você a especificar a funcionalidade ou o comportamento somente uma vez e, em seguida, refleti-lo em qualquer lugar de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b34b7-106">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an app.</span></span> <span data-ttu-id="b34b7-107">Isso reduz a quantidade de código que você precisa escrever e faz com que o código escrito seja menos propenso a erros e mais fácil de testar e manter.</span><span class="sxs-lookup"><span data-stu-id="b34b7-107">This reduces the amount of code you need to write and makes the code you do write less error prone, easier to test, and easier to maintain.</span></span>

<span data-ttu-id="b34b7-108">O suporte de validação fornecido pelo MVC e pelo Entity Framework Core Code First é um bom exemplo do princípio DRY em ação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-108">The validation support provided by MVC and Entity Framework Core Code First is a good example of the DRY principle in action.</span></span> <span data-ttu-id="b34b7-109">Especifique as regras de validação de forma declarativa em um único lugar (na classe de modelo) e as regras são impostas em qualquer lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b34b7-109">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the app.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="b34b7-110">Adicionando regras de validação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="b34b7-110">Adding validation rules to the movie model</span></span>

<span data-ttu-id="b34b7-111">Abra o arquivo *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="b34b7-111">Open the *Movie.cs* file.</span></span> <span data-ttu-id="b34b7-112">DataAnnotations fornece um conjunto interno de atributos de validação que são aplicados de forma declarativa a qualquer classe ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="b34b7-112">DataAnnotations provides a built-in set of validation attributes that you apply declaratively to any class or property.</span></span> <span data-ttu-id="b34b7-113">(Também contém atributos de formatação como `DataType`, que ajudam com a formatação e não fornecem nenhuma validação.)</span><span class="sxs-lookup"><span data-stu-id="b34b7-113">(It also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.)</span></span>

<span data-ttu-id="b34b7-114">Atualize a classe `Movie` para aproveitar os atributos de validação `Required`, `StringLength`, `RegularExpression` e `Range` internos.</span><span class="sxs-lookup"><span data-stu-id="b34b7-114">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

<span data-ttu-id="b34b7-115">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="b34b7-115">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span></span>

<span data-ttu-id="b34b7-116">Os atributos de validação especificam o comportamento que você deseja impor nas propriedades de modelo às quais eles são aplicados.</span><span class="sxs-lookup"><span data-stu-id="b34b7-116">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="b34b7-117">Os atributos `Required` e `MinimumLength` indicam que uma propriedade deve ter um valor; porém, nada impede que um usuário insira um espaço em branco para atender a essa validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-117">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span> <span data-ttu-id="b34b7-118">O atributo `RegularExpression` é usado para limitar quais caracteres podem ser inseridos.</span><span class="sxs-lookup"><span data-stu-id="b34b7-118">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="b34b7-119">No código acima, `Genre` e `Rating` devem usar apenas letras (espaço em branco, números e caracteres especiais não são permitidos).</span><span class="sxs-lookup"><span data-stu-id="b34b7-119">In the code above, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="b34b7-120">O atributo `Range` restringe um valor a um intervalo especificado.</span><span class="sxs-lookup"><span data-stu-id="b34b7-120">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="b34b7-121">O atributo `StringLength` permite definir o tamanho máximo de uma propriedade de cadeia de caracteres e, opcionalmente, seu tamanho mínimo.</span><span class="sxs-lookup"><span data-stu-id="b34b7-121">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="b34b7-122">Os tipos de valor (como `decimal`, `int`, `float`, `DateTime`) são inerentemente necessários e não precisam do atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-122">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="b34b7-123">Ter as regras de validação automaticamente impostas pelo ASP.NET ajuda a tornar o aplicativo mais robusto.</span><span class="sxs-lookup"><span data-stu-id="b34b7-123">Having validation rules automatically enforced by ASP.NET helps make your app more robust.</span></span> <span data-ttu-id="b34b7-124">Também garante que você não se esqueça de validar algo e inadvertidamente permita dados incorretos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b34b7-124">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

## <a name="validation-error-ui-in-mvc"></a><span data-ttu-id="b34b7-125">Interface do usuário do erro de validação no MVC</span><span class="sxs-lookup"><span data-stu-id="b34b7-125">Validation Error UI in MVC</span></span>

<span data-ttu-id="b34b7-126">Execute o aplicativo e navegue para o controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="b34b7-126">Run the app and navigate to the Movies controller.</span></span>

<span data-ttu-id="b34b7-127">Toque no link **Criar Novo** para adicionar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="b34b7-127">Tap the **Create New** link to add a new movie.</span></span> <span data-ttu-id="b34b7-128">Preencha o formulário com alguns valores inválidos.</span><span class="sxs-lookup"><span data-stu-id="b34b7-128">Fill out the form with some invalid values.</span></span> <span data-ttu-id="b34b7-129">Assim que a validação do lado do cliente do jQuery detecta o erro, ela exibe uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="b34b7-129">As soon as jQuery client side validation detects the error, it displays an error message.</span></span>

![Formulário da exibição de filmes com vários erros de validação do lado do cliente do jQuery](../../tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="b34b7-131">Talvez você não consiga inserir pontos decimais ou vírgulas no campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-131">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="b34b7-132">Para dar suporte à [validação do jQuery](http://jqueryvalidation.org/) para localidades de idiomas diferentes do inglês que usam uma vírgula (“,”) para um ponto decimal e formatos de data diferentes do inglês dos EUA, você deve tomar medidas para globalizar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b34b7-132">To support [jQuery validation](http://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="b34b7-133">Consulte [Recursos adicionais](#additional-resources) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b34b7-133">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="b34b7-134">Por enquanto, insira apenas números inteiros como 10.</span><span class="sxs-lookup"><span data-stu-id="b34b7-134">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="b34b7-135">Observe como o formulário renderizou automaticamente uma mensagem de erro de validação apropriada em cada campo que contém um valor inválido.</span><span class="sxs-lookup"><span data-stu-id="b34b7-135">Notice how the form has automatically rendered an appropriate validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="b34b7-136">Os erros são impostos no lado do cliente (usando o JavaScript e o jQuery) e no lado do servidor (caso um usuário tenha o JavaScript desabilitado).</span><span class="sxs-lookup"><span data-stu-id="b34b7-136">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="b34b7-137">Uma vantagem significativa é que você não precisa alterar uma única linha de código na classe `MoviesController` ou na exibição *Create.cshtml* para habilitar essa interface do usuário de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-137">A significant benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="b34b7-138">O controlador e as exibições criados anteriormente neste tutorial selecionaram automaticamente as regras de validação especificadas com atributos de validação nas propriedades da classe de modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-138">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span> <span data-ttu-id="b34b7-139">Teste a validação usando o método de ação `Edit` e a mesma validação é aplicada.</span><span class="sxs-lookup"><span data-stu-id="b34b7-139">Test validation using the `Edit` action method, and the same validation is applied.</span></span>

<span data-ttu-id="b34b7-140">Os dados de formulário não são enviados no servidor até que não haja erros de validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b34b7-140">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="b34b7-141">Verifique isso colocando um ponto de interrupção no método `HTTP Post` usando a [ferramenta Fiddler](http://www.telerik.com/fiddler) ou as [ferramentas do Desenvolvedor F12](https://dev.windows.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="b34b7-141">You can verify this by putting a break point in the `HTTP Post` method, by using the [Fiddler tool](http://www.telerik.com/fiddler) , or the [F12 Developer tools](https://dev.windows.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>

## <a name="how-validation-works"></a><span data-ttu-id="b34b7-142">Como funciona a validação</span><span class="sxs-lookup"><span data-stu-id="b34b7-142">How validation works</span></span>

<span data-ttu-id="b34b7-143">Talvez você esteja se perguntando como a interface do usuário de validação foi gerada sem atualizações do código no controlador ou nas exibições.</span><span class="sxs-lookup"><span data-stu-id="b34b7-143">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="b34b7-144">O código a seguir mostra os dois métodos `Create`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-144">The following code shows the two `Create` methods.</span></span>

<span data-ttu-id="b34b7-145">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]</span><span class="sxs-lookup"><span data-stu-id="b34b7-145">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]</span></span>

<span data-ttu-id="b34b7-146">O primeiro método de ação (HTTP GET) `Create` exibe o formulário Criar inicial.</span><span class="sxs-lookup"><span data-stu-id="b34b7-146">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="b34b7-147">A segunda versão (`[HttpPost]`) manipula a postagem de formulário.</span><span class="sxs-lookup"><span data-stu-id="b34b7-147">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="b34b7-148">O segundo método `Create` (a versão `[HttpPost]`) chama `ModelState.IsValid` para verificar se o filme tem erros de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-148">The second `Create` method (The `[HttpPost]` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="b34b7-149">A chamada a esse método avalia os atributos de validação que foram aplicados ao objeto.</span><span class="sxs-lookup"><span data-stu-id="b34b7-149">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="b34b7-150">Se o objeto tiver erros de validação, o método `Create` exibirá o formulário novamente.</span><span class="sxs-lookup"><span data-stu-id="b34b7-150">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="b34b7-151">Se não houver erros, o método salvará o novo filme no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b34b7-151">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="b34b7-152">Em nosso exemplo de filme, o formulário não é postado no servidor quando há erros de validação detectados no lado do cliente; o segundo método `Create` nunca é chamado quando há erros de validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b34b7-152">In our movie example, the form is not posted to the server when there are validation errors detected on the client side; the second `Create` method is never called when there are client side validation errors.</span></span> <span data-ttu-id="b34b7-153">Se você desabilitar o JavaScript no navegador, a validação do cliente será desabilitada e você poderá testar o método `Create` HTTP POST `ModelState.IsValid` detectando erros de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-153">If you disable JavaScript in your browser, client validation is disabled and you can test the HTTP POST `Create` method `ModelState.IsValid` detecting any validation errors.</span></span>

<span data-ttu-id="b34b7-154">Defina um ponto de interrupção no método `[HttpPost] Create` e verifique se o método nunca é chamado; a validação do lado do cliente não enviará os dados de formulário quando forem detectados erros de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-154">You can set a break point in the `[HttpPost] Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="b34b7-155">Se você desabilitar o JavaScript no navegador e, em seguida, enviar o formulário com erros, o ponto de interrupção será atingido.</span><span class="sxs-lookup"><span data-stu-id="b34b7-155">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="b34b7-156">Você ainda pode obter uma validação completa sem o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b34b7-156">You still get full validation without JavaScript.</span></span> 

<span data-ttu-id="b34b7-157">A imagem a seguir mostra como desabilitar o JavaScript no navegador FireFox.</span><span class="sxs-lookup"><span data-stu-id="b34b7-157">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![Firefox: Na guia Conteúdo de Opções, desmarque a caixa de seleção Habilitar JavaScript.](../../tutorials/first-mvc-app/validation/_static/ff.png)

<span data-ttu-id="b34b7-159">A imagem a seguir mostra como desabilitar o JavaScript no navegador Chrome.</span><span class="sxs-lookup"><span data-stu-id="b34b7-159">The following image shows how to disable JavaScript in the Chrome browser.</span></span>

![Google Chrome: Na seção JavaScript das configurações de Conteúdo, selecione Não permitir que nenhum site execute JavaScript.](../../tutorials/first-mvc-app/validation/_static/chrome.png)

<span data-ttu-id="b34b7-161">Depois de desabilitar o JavaScript, poste os dados inválidos e execute o depurador em etapas.</span><span class="sxs-lookup"><span data-stu-id="b34b7-161">After you disable JavaScript, post invalid data and step through the debugger.</span></span>

![Durante a depuração em uma postagem de dados inválidos, o IntelliSense em ModelState.IsValid mostra que o valor é falso.](../../tutorials/first-mvc-app/validation/_static/ms.png)

<span data-ttu-id="b34b7-163">Veja abaixo uma parte do modelo de exibição *Create.cshtml* gerada por scaffolding anteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="b34b7-163">Below is portion of the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="b34b7-164">Ela é usada pelos métodos de ação mostrados acima para exibir o formulário inicial e exibi-lo novamente em caso de erro.</span><span class="sxs-lookup"><span data-stu-id="b34b7-164">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

<span data-ttu-id="b34b7-165">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b34b7-165">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]</span></span>

<span data-ttu-id="b34b7-166">O [Auxiliar de Marcação de Entrada](xref:mvc/views/working-with-forms) usa os atributos de [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) e produz os atributos HTML necessários para a Validação do jQuery no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b34b7-166">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes and produces HTML attributes needed for jQuery Validation on the client side.</span></span> <span data-ttu-id="b34b7-167">O [Auxiliar de Marcação de Validação](xref:mvc/views/working-with-forms#the-validation-tag-helpers) exibe erros de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-167">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="b34b7-168">Consulte [Validação](xref:mvc/models/validation) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b34b7-168">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="b34b7-169">O que é realmente interessante nessa abordagem é que o controlador nem o modelo de exibição `Create` sabem nada sobre as regras de validação reais que estão sendo impostas ou as mensagens de erro específicas exibidas.</span><span class="sxs-lookup"><span data-stu-id="b34b7-169">What's really nice about this approach is that neither the controller nor the `Create` view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="b34b7-170">As regras de validação e as cadeias de caracteres de erro são especificadas somente na classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-170">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="b34b7-171">Essas mesmas regras de validação são aplicadas automaticamente à exibição `Edit` e a outros modelos de exibição que podem ser criados e que editam o modelo.</span><span class="sxs-lookup"><span data-stu-id="b34b7-171">These same validation rules are automatically applied to the `Edit` view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="b34b7-172">Quando você precisar alterar a lógica de validação, faça isso exatamente em um lugar, adicionando atributos de validação ao modelo (neste exemplo, a classe `Movie`).</span><span class="sxs-lookup"><span data-stu-id="b34b7-172">When you need to change validation logic, you can do so in exactly one place by adding validation attributes to the model (in this example, the `Movie` class).</span></span> <span data-ttu-id="b34b7-173">Você não precisa se preocupar se diferentes partes do aplicativo estão inconsistentes com a forma como as regras são impostas – toda a lógica de validação será definida em um lugar e usada em todos os lugares.</span><span class="sxs-lookup"><span data-stu-id="b34b7-173">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="b34b7-174">Isso mantém o código muito limpo e torna-o mais fácil de manter e desenvolver.</span><span class="sxs-lookup"><span data-stu-id="b34b7-174">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="b34b7-175">Além disso, isso significa que você respeitará totalmente o princípio DRY.</span><span class="sxs-lookup"><span data-stu-id="b34b7-175">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="b34b7-176">Usando atributos DataType</span><span class="sxs-lookup"><span data-stu-id="b34b7-176">Using DataType Attributes</span></span>

<span data-ttu-id="b34b7-177">Abra o arquivo *Movie.cs* e examine a classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-177">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="b34b7-178">O namespace `System.ComponentModel.DataAnnotations` fornece atributos de formatação, além do conjunto interno de atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-178">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="b34b7-179">Já aplicamos um valor de enumeração `DataType` à data de lançamento e aos campos de preço.</span><span class="sxs-lookup"><span data-stu-id="b34b7-179">We've already applied a `DataType` enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="b34b7-180">O código a seguir mostra as propriedades `ReleaseDate` e `Price` com o atributo `DataType` apropriado.</span><span class="sxs-lookup"><span data-stu-id="b34b7-180">The following code shows the `ReleaseDate` and `Price` properties with the appropriate `DataType` attribute.</span></span>

<span data-ttu-id="b34b7-181">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="b34b7-181">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span></span>

<span data-ttu-id="b34b7-182">Os atributos `DataType` fornecem apenas dicas para que o mecanismo de exibição formate os dados (e fornece atributos como `<a>` para as URLs e `<a href="mailto:EmailAddress.com">` para o email).</span><span class="sxs-lookup"><span data-stu-id="b34b7-182">The `DataType` attributes only provide hints for the view engine to format the data (and supply attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email.</span></span> <span data-ttu-id="b34b7-183">Use o atributo `RegularExpression` para validar o formato dos dados.</span><span class="sxs-lookup"><span data-stu-id="b34b7-183">You can use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="b34b7-184">O atributo `DataType` é usado para especificar um tipo de dados mais específico do que o tipo intrínseco de banco de dados; eles não são atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-184">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type, they are not validation attributes.</span></span> <span data-ttu-id="b34b7-185">Nesse caso, apenas desejamos acompanhar a data, não a hora.</span><span class="sxs-lookup"><span data-stu-id="b34b7-185">In this case we only want to keep track of the date, not the time.</span></span> <span data-ttu-id="b34b7-186">A Enumeração `DataType` fornece muitos tipos de dados, como Date, Time, PhoneNumber, Currency, EmailAddress e muito mais.</span><span class="sxs-lookup"><span data-stu-id="b34b7-186">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress and more.</span></span> <span data-ttu-id="b34b7-187">O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo.</span><span class="sxs-lookup"><span data-stu-id="b34b7-187">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="b34b7-188">Por exemplo, um link `mailto:` pode ser criado para `DataType.EmailAddress` e um seletor de data pode ser fornecido para `DataType.Date` em navegadores que dão suporte a HTML5.</span><span class="sxs-lookup"><span data-stu-id="b34b7-188">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="b34b7-189">Os atributos `DataType` emitem atributos `data-` HTML 5 (pronunciados “data dash”) que são reconhecidos pelos navegadores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="b34b7-189">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="b34b7-190">Os atributos `DataType` **não** fornecem nenhuma validação.</span><span class="sxs-lookup"><span data-stu-id="b34b7-190">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="b34b7-191">`DataType.Date` não especifica o formato da data exibida.</span><span class="sxs-lookup"><span data-stu-id="b34b7-191">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="b34b7-192">Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base nas `CultureInfo` do servidor.</span><span class="sxs-lookup"><span data-stu-id="b34b7-192">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="b34b7-193">O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:</span><span class="sxs-lookup"><span data-stu-id="b34b7-193">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="b34b7-194">A configuração `ApplyFormatInEditMode` especifica que a formatação também deve ser aplicada quando o valor é exibido em uma caixa de texto para edição.</span><span class="sxs-lookup"><span data-stu-id="b34b7-194">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="b34b7-195">(Talvez você não deseje isso em alguns campos – por exemplo, para valores de moeda, provavelmente, você não deseja exibir o símbolo de moeda na caixa de texto para edição.)</span><span class="sxs-lookup"><span data-stu-id="b34b7-195">(You might not want that for some fields — for example, for currency values, you probably do not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="b34b7-196">Você pode usar o atributo `DisplayFormat` por si só, mas geralmente é uma boa ideia usar o atributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-196">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="b34b7-197">O atributo `DataType` transmite a semântica dos dados, ao invés de apresentar como renderizá-lo em uma tela e oferece os seguintes benefícios que você não obtém com DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="b34b7-197">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="b34b7-198">O navegador pode habilitar os recursos do HTML5 (por exemplo, mostrar um controle de calendário, o símbolo de moeda apropriado à localidade, links de email, etc.)</span><span class="sxs-lookup"><span data-stu-id="b34b7-198">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>

* <span data-ttu-id="b34b7-199">Por padrão, o navegador renderizará os dados usando o formato correto de acordo com a localidade.</span><span class="sxs-lookup"><span data-stu-id="b34b7-199">By default, the browser will render data using the correct format based on your locale.</span></span>

* <span data-ttu-id="b34b7-200">O atributo `DataType` pode permitir que o MVC escolha o modelo de campo correto para renderizar os dados (o `DisplayFormat`, se usado por si só, usa o modelo de cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="b34b7-200">The `DataType` attribute can enable MVC to choose the right field template to render the data (the `DisplayFormat` if used by itself uses the string template).</span></span>

> [!NOTE]
> <span data-ttu-id="b34b7-201">A validação do jQuery não funciona com os atributos `Range` e `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-201">jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="b34b7-202">Por exemplo, o seguinte código sempre exibirá um erro de validação do lado do cliente, mesmo quando a data estiver no intervalo especificado:</span><span class="sxs-lookup"><span data-stu-id="b34b7-202">For example, the following code will always display a client side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="b34b7-203">Você precisará desabilitar a validação de data do jQuery para usar o atributo `Range` com `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="b34b7-203">You will need to disable jQuery date validation to use the `Range` attribute with `DateTime`.</span></span> <span data-ttu-id="b34b7-204">Geralmente, não é uma boa prática compilar datas rígidas nos modelos e, portanto, o uso do atributo `Range` e de `DateTime` não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="b34b7-204">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="b34b7-205">O seguinte código mostra como combinar atributos em uma linha:</span><span class="sxs-lookup"><span data-stu-id="b34b7-205">The following code shows combining attributes on one line:</span></span>

<span data-ttu-id="b34b7-206">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="b34b7-206">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span></span>

<span data-ttu-id="b34b7-207">Na próxima parte da série, examinaremos o aplicativo e faremos algumas melhorias nos métodos `Details` e `Delete` gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b34b7-207">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b34b7-208">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b34b7-208">Additional resources</span></span>

* [<span data-ttu-id="b34b7-209">Trabalhando com formulários</span><span class="sxs-lookup"><span data-stu-id="b34b7-209">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="b34b7-210">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="b34b7-210">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="b34b7-211">Introdução aos auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="b34b7-211">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b34b7-212">Criando auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="b34b7-212">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)