### This is a fork from liquiidio's AtomicAssetsApiClient (https://github.com/liquiidio/AtomicAssetsApiClient)
I just added immutable properties filtering to the Api Factory, some more API URLs, and some documentation.

# AtomicAssetsApiClient

.NET and Unity3D-compatible (Desktop, Mobile, WebGL) ApiClient for AtomicAssets

 ## Usage

 The entry point to the APIs is in the AtomicAssetsApiFactory. You can initialise any supported API from there.
 You can then call any endpoint from the initialised API.
 Each endpoint has its own set of parameters that you may build up and pass in to the relevant function.
 
 ```csharp
 var assetsApi = AtomicAssetsApiFactory.MainNet1.AssetsApi;
  
 or
 
 var assetsApi = AtomicAssetsApiFactory.TestNet2.AssetsApi;
 
 NOTES:
 MainNet1 => AtomicAssetsIO
 MainNet2 => EOSAmsterdam
 MainNet3 => EOSAuthority
 TestNet1 => testnet3DK
 TestNet2 => testnetNefty
 ```

 ## Examples
 ### Getting assets available for trading on the exchange
 ```csharp

async Task GettingAllTheAssets()
{
    // Initialize the v1 assets API
    var assetsApi = AtomicAssetsApiFactory.MainNet1.AssetsApi;

    //Getting all the assets that are available for trading on the exchange.
    var assets = await assetsApi.Assets();

    // Print their IDs on the console.
    assets.Data.ToList().ForEach(a => Console.WriteLine(a.AssetId));
}

 ```
 
 ### Getting a filtered assets list that is available for trading
 ```csharp

async Task GettingFilteredAssetsList()
{
    // Initialize the v1 assets API
    var assetsApi = AtomicAssetsApiFactory.MainNet1.AssetsApi;

    // Build up the AssetsParameters with the AssetsUriParameterBuilder
    // This can be used to fine tune the kind of results we want
    var builder = new AssetsUriParameterBuilder()
        .WithLimit(5)
        .WithImmutableProperty("rarity","rare")
        .WithOwner("username")
        .WithCollectionName("mycollection")
        .WithSchemaName("myschema")
        .WithTemplateId(123456);

    // Getting all the assets that match the filters from the builder.
    var assets = await assetsApi.Assets(builder);

    // Print their IDs on the console.
    assets.Data.ToList().ForEach(a => Console.WriteLine(a.AssetId));

    // If you want to log your query for debugging the builder
    Debug.Log($"Query Parameters: {builder.Build()}");

}

 ```
 
 ### Getting a single asset directly by its atomicasset id
 ```csharp
var asset = assetsApi.Asset(1099553597937);

 ```
 
 ### Getting an Offer
 ```csharp

async Task GetOffer(string offerId)
{

    // Initialize the v1 offers API
    var api = AtomicAssetsApiFactory.Version1.OffersApi;

    // Call the offers endpoint passing the offerId as an input
    var sales = await api.Offer(offerId);

    // Access different informations about the offer using the Data property in the result
    Console.WriteLine(sales.Data.SenderName);
}
