// app\Models\VendorDetail.php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;

class VendorDetail extends Model
{
    protected $table = 'vendor_details';
    protected $fillable = ['name', 'category', '...']; // Add other fillable columns
}
--Html
<form action="{{ route('vendor.store') }}" method="post">
    @csrf
    <label for="name">Name:</label>
    <input type="text" name="name" id="name">
    <label for="category">Category:</label>
    <select name="category" id="category">
        <option value="Category 1">Category 1</option>
        <option value="Category 2">Category 2</option>
        <!-- Add other options dynamically -->
    </select>
    <!-- Add other input fields -->
    <button type="submit">Submit</button>
</form>
use App\Http\Controllers\VendorDetailController;

Route::get('/vendor/create', [VendorDetailController::class, 'create']);
Route::post('/vendor', [VendorDetailController::class, 'store'])->name('vendor.store');
// app\Http\Controllers\VendorDetailController.php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use App\Models\VendorDetail;

class VendorDetailController extends Controller
{
    public function create()
    {
        return view('vendor.create');
    }

    public function store(Request $request)
    {
        $validatedData = $request->validate([
            'name' => 'required|string|max:255',
            'category' => 'required|string|max:255',
            // Add other validation rules
        ]);

        VendorDetail::create($validatedData);

        return redirect('/vendor/create')->with('success', 'Vendor details saved successfully!');
    }
}
// In VendorDetailController
public function show($id)
{
    $vendor = VendorDetail::findOrFail($id);
    return view('vendor.show', ['vendor' => $vendor]);
}
