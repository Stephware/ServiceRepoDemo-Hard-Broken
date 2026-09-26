# 30 Bug Fix Summary

1. The product list used a static cache, so I removed it to make sure the list always shows the latest data.
2. The controller depended on `AppDbContext`, so I changed it to use only `IProductService` because the controller should not access the database directly.
3. `Index()` loaded products from `_context`, so I changed it to call the service to follow the proper app structure.
4. Duplicate-name checking was inside the controller, so I moved it to the service because it is business logic.
5. The duplicate check happened before trimming the name, so I trim first to avoid names with extra spaces being treated as different.
6. The controller was trimming the product name, so I moved that to the service because business rules should stay there.
7. The controller was setting `CreatedAt`, so I moved it to the service so creation rules are handled in one place.
8. Failed Create redirected back to the page, so I return the same view instead to keep the entered values and error message.
9. Successful Edit returned the Edit view again, so I changed it to redirect to `Index` after saving.
10. Delete used `AppDbContext` directly in the controller, so I moved it through the service to keep the proper controller-service-repository flow.
11. Delete used `Stock >= 0`, so I changed it to block only stock above `0` because products with zero stock should be deletable.
12. `ProductService` manually created `ProductRepository`, so I changed it to inject `IProductRepository` because DI should provide dependencies.
13. Create did not trim the product name in the service, so I added trimming to avoid extra spaces.
14. Create did not check duplicate names in the service, so I added the check to keep product names unique.
15. Create did not set `CreatedAt` in the service, so I added `DateTime.UtcNow` when creating a product.
16. Update did not trim the edited name, so I added trimming to keep the same rule as Create.
17. Update did not properly check duplicate names, so I exclude the current product ID while checking other products.
18. Update changed `CreatedAt`, so I stopped updating it because creation time should stay the same.
19. Update saved the posted `product` instead of the loaded `existing` product, so I update the existing entity instead.
20. `SaveChangesAsync()` was called without `await`, so I added `await` to make sure saving finishes properly.
21. Delete was missing from the service contract and logic, so I added `DeleteAsync` to the interface and service.
22. The repository sorted products by `Description`, so I changed it to `Name` because the list should be A-Z by name.
23. Repository Delete only detached the product, so I changed it to `Remove(product)` because detaching does not delete anything.
24. The repository cleared the change tracker before saving, so I removed that because it could throw away pending changes.
25. The Edit link sent `productId` instead of `id`, so I changed it to `asp-route-id` to match the controller parameter.
26. The Edit form posted to `Create`, so I changed it to post to `Edit` so the correct action runs.
27. The Delete form used a hardcoded URL, so I changed it to ASP.NET tag helpers to keep routing correct.
28. The Stock input used `name="Quantity"`, so I changed it to `asp-for="Stock"` so model binding gets the correct value.
29. The layout used `TempData.Peek()`, so I changed it to a normal TempData read so the message only shows once.
30. jQuery loaded after the validation scripts, so I moved jQuery before them because client-side validation depends on it.
