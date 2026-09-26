# 30 Bug Fix Summary

Short notes on what was wrong, what I changed, and why.

| # | Error | Fix | Why it was wrong / why I fixed it |
|---|---|---|---|
| 1 | Product list used a static cache. | Removed the cache and always load fresh data. | The list could show old data after changes. |
| 2 | Controller depended on `AppDbContext`. | Controller now uses `IProductService` only. | The controller should not talk directly to the database. |
| 3 | `Index()` loaded products from `_context`. | `Index()` now calls the service. | It skipped the service layer. |
| 4 | Duplicate-name checking was in the controller. | Moved the check to the service. | Duplicate checking is business logic. |
| 5 | Duplicate check happened before trimming the name. | Trim first, then check. | `Mouse` and ` Mouse ` should be treated the same. |
| 6 | Product name was trimmed in the controller. | Moved trimming to the service. | The service should handle business rules. |
| 7 | `CreatedAt` was set in the controller. | Set it in the service. | The service should control creation rules. |
| 8 | Failed Create redirected back to Create. | Return the same view with the error. | Redirecting loses the entered values and error. |
| 9 | Successful Edit returned the Edit view again. | Redirect to `Index`. | A successful save should go back to the list. |
| 10 | Delete used `AppDbContext` directly in the controller. | Delete now goes through the service. | It skipped the service and repository layers. |
| 11 | Delete blocked `Stock == 0` because it used `>= 0`. | Changed the rule so only stock above `0` is blocked. | Products with exactly zero stock should be deletable. |
| 12 | `ProductService` manually created `ProductRepository`. | Inject `IProductRepository` in the constructor. | DI should provide the repository instead of using `new`. |
| 13 | Create service did not trim the name. | Added trimming before saving. | Extra spaces could cause bad or duplicate names. |
| 14 | Create service did not check duplicate names. | Added duplicate-name validation. | Product names are supposed to be unique. |
| 15 | Create service did not set `CreatedAt`. | Set `CreatedAt = DateTime.UtcNow`. | Creation time should be set once when the product is created. |
| 16 | Update service did not trim the name. | Trim the edited name. | Edited names should follow the same rule as Create. |
| 17 | Update did not properly check duplicates against other products. | Check duplicates while excluding the current product ID. | A product can keep its own name but cannot copy another product's name. |
| 18 | Update changed `CreatedAt`. | Stopped updating `CreatedAt`. | Creation time should never change during Edit. |
| 19 | Update saved the posted `product` instead of the loaded `existing` product. | Update the `existing` entity. | The loaded entity is the one being tracked and edited. |
| 20 | `SaveChangesAsync()` was called without `await`. | Added `await`. | The method could finish before saving was done. |
| 21 | Delete was missing from the service contract/logic. | Added `DeleteAsync` to the service and interface. | Delete business rules should stay in the service layer. |
| 22 | Repository sorted products by `Description`. | Sort by `Name`. | The requirement is Name A-Z. |
| 23 | Repository Delete detached the product. | Use `Remove(product)`. | Detaching does not delete anything from the database. |
| 24 | Repository cleared tracking before saving. | Removed `ChangeTracker.Clear()`. | Clearing first throws away pending changes. |
| 25 | Edit link sent `productId` instead of `id`. | Changed it to `asp-route-id`. | The controller expects a parameter named `id`. |
| 26 | Edit form posted to `Create`. | Changed the form action to `Edit`. | Saving an edit should call the Edit POST action. |
| 27 | Delete form used a hardcoded URL. | Used ASP.NET tag helpers for the Delete form. | Tag helpers keep routing correct and generate the proper form setup. |
| 28 | Stock input used `name="Quantity"`. | Changed it to `asp-for="Stock"`. | Model binding was not receiving the Stock value. |
| 29 | Layout used `TempData.Peek()`. | Read `TempData["Message"]` normally. | `Peek()` keeps the message, so it can appear again. |
| 30 | jQuery loaded after validation scripts. | Load jQuery before the page validation scripts. | jQuery validation needs jQuery to already be loaded. |
