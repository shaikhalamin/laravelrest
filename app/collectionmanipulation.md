#https://stackoverflow.com/questions/46388205/laravel-5-5-api-resources-for-collections-standalone-data


```javascript
class PageController extends Controller
{
    public function index()
    {
        return new PageResourceCollection(PageResource::collection(Page::all()));
    }

    public function show(Page $page)
    {
        return new PageResource($page);
    }
}

Resources:

class PageResource extends Resource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'slug' => $this->slug,
            'user' => [
                'id' => $this->user->id,
                'name' => $this->user->name,
                'email' => $this->user->email,
            ],
        ];
    }
}

class PageResourceCollection extends ResourceCollection
{
    public function toArray($request)
    {
        return [
            'data' => $this->collection->transform(function($page){
                return [
                    'id' => $page->id,
                    'title' => $page->title,
                    'slug' => $page->slug,
                ];
            }),
        ];
    }
}
```