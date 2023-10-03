<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Customer;
use Illuminate\Support\Str;
use Illuminate\Support\Facades\Storage;
use App\Models\Image1;
use Illuminate\Database\Eloquent\Model;


class CustomerController extends Controller
{
    public function index()
    {
        $customer = customer::latest()->paginate( 10 );
        return view( 'customer.index', compact( 'customer' ) )
        ->with( 'i', ( request()->input( 'page', 1 ) - 1 ) * 10 );
        // $customers = Customer::all();
        // return view('customer.create', ['customers' => $customers]);
    }

    public function create()
    {
        return view('customer.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'custname' => 'required',
            'phone' => 'required',
            'vechicleno' => 'required',
            'vechiclemake' => 'required',
            'Kms' => 'required',
            'Fuel' => 'required',
            'blankfield' => 'required',
            'newfield' => 'required',
            'logstaffname' => 'required',
            'asignstaffname' => 'required',
            'front_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'rh_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'lh_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'rear_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'dashboard_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'dicky_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
        ]);
    
        // Create a new Customer record
        $customer = Customer::create($request->all());
    
        // Handle image upload to front_image
        if ($request->hasFile('front_image')) {
            $imageName = time() . '.' . $request->file('front_image')->getClientOriginalExtension();
            $request->file('front_image')->storeAs('public/images', $imageName);
    
            // Create a new Image1 record and associate it with the Customer
            $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
            $customer->image1()->save($front_image);
        }

        // handle image upload to rh_image
        if ($request->hasFile('rh_image')) {
            $imageName = time() . '.' . $request->file('rh_image')->getClientOriginalExtension();
            $request->file('rh_image')->storeAs('public/images', $imageName);
    
            // Create a new Image1 record and associate it with the Customer
            $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
            $customer->image1()->save($rh_image);
        }

        // handle image upload to lh_image
        if ($request->hasFile('lh_image')) {
            $imageName = time() . '.' . $request->file('lh_image')->getClientOriginalExtension();
            $request->file('lh_image')->storeAs('public/images', $imageName);
    
            // Create a new Image1 record and associate it with the Customer
            $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
            $customer->image1()->save($lh_image);
        }

        // handle image upload to rear_image
        if ($request->hasFile('rear_image')) {
            $imageName = time() . '.' . $request->file('rear_image')->getClientOriginalExtension();
            $request->file('rear_image')->storeAs('public/images', $imageName);
    
            // Create a new Image1 record and associate it with the Customer
            $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
            $customer->image1()->save($rear_image);
        }

        // handle image upload to dashboard_image
        if ($request->hasFile('dashboard_image')) {
            $imageName = time() . '.' . $request->file('dashboard_image')->getClientOriginalExtension();
            $request->file('dashboard_image')->storeAs('public/images', $imageName);
    
            // Create a new Image1 record and associate it with the Customer
            $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
            $customer->image1()->save($dashboard_image);
        }

        // handle image upload to dicky_image
        if ($request->hasFile('dicky_image')) {
            $imageName = time() . '.' . $request->file('dicky_image')->getClientOriginalExtension();
            $request->file('dicky_image')->storeAs('public/images', $imageName);
    
            // Create a new Image1 record and associate it with the Customer
            $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
            $customer->image1()->save($dicky_image);
        }
    
        return redirect()->route('customer.index')->with('success', 'Customer created successfully');
    }

    public function show($id) {
        $customer = Customer::findOrFail($id);
        // $customer = Customer::with('image1')->find($id);
        return view( 'customer.show', compact( 'customer' ) );
    }

    public function edit($id)
    {
        $customer = Customer::find($id);
        return view('customer.edit', compact( 'customer' ) );
    }

    public function update(Request $request, $id)
    {
        $request->validate([
            'custname' => 'required',
            'phone' => 'required',
            'vechicleno' => 'required',
            'vechiclemake' => 'required',
            'Kms' => 'required',
            'Fuel' => 'required',
            'blankfield' => 'required',
            'newfield' => 'required',
            'logstaffname' => 'required',
            'asignstaffname' => 'required',
            'front_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'rh_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'lh_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'rear_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'dashboard_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
            'dicky_image' => 'image|mimes:jpeg,png,jpg,gif|max:2048',
        ]);
    
        $customer = Customer::findOrFail($id);
    
        // Handle image update and association
        if ($request->hasFile('front_image')) {
            $imageName = time() . '.' . $request->file('front_image')->getClientOriginalExtension();
            $request->file('front_image')->storeAs('public/images', $imageName);
    
            // Update or create the associated Image1 record
            if ($customer->image1) {
                // If an Image1 record exists, update its image path
                $customer->image1->update(['image_path' => 'storage/images/' . $imageName]);
            } else {
                // If no Image1 record exists, create a new one and associate it with the Customer
                $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
                $customer->image1()->save($front_image);
            }
        }

        if ($request->hasFile('rh_image')) {
            $imageName = time() . '.' . $request->file('rh_image')->getClientOriginalExtension();
            $request->file('rh_image')->storeAs('public/images', $imageName);
    
            // Update or create the associated Image1 record
            if ($customer->image1) {
                // If an Image1 record exists, update its image path
                $customer->image1->update(['image_path' => 'storage/images/' . $imageName]);
            } else {
                // If no Image1 record exists, create a new one and associate it with the Customer
                $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
                $customer->image1()->save($rh_image);
            }
        }

        if ($request->hasFile('lh_image')) {
            $imageName = time() . '.' . $request->file('lh_image')->getClientOriginalExtension();
            $request->file('lh_image')->storeAs('public/images', $imageName);
    
            // Update or create the associated Image1 record
            if ($customer->image1) {
                // If an Image1 record exists, update its image path
                $customer->image1->update(['image_path' => 'storage/images/' . $imageName]);
            } else {
                // If no Image1 record exists, create a new one and associate it with the Customer
                $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
                $customer->image1()->save($lh_image);
            }
        }

        if ($request->hasFile('rear_image')) {
            $imageName = time() . '.' . $request->file('rear_image')->getClientOriginalExtension();
            $request->file('rear_image')->storeAs('public/images', $imageName);
    
            // Update or create the associated Image1 record
            if ($customer->image1) {
                // If an Image1 record exists, update its image path
                $customer->image1->update(['image_path' => 'storage/images/' . $imageName]);
            } else {
                // If no Image1 record exists, create a new one and associate it with the Customer
                $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
                $customer->image1()->save($rear_image);
            }
        }

        if ($request->hasFile('dashboard_image')) {
            $imageName = time() . '.' . $request->file('dashboard_image')->getClientOriginalExtension();
            $request->file('dashboard_image')->storeAs('public/images', $imageName);
    
            // Update or create the associated Image1 record
            if ($customer->image1) {
                // If an Image1 record exists, update its image path
                $customer->image1->update(['image_path' => 'storage/images/' . $imageName]);
            } else {
                // If no Image1 record exists, create a new one and associate it with the Customer
                $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
                $customer->image1()->save($dashboard_image);
            }
        }

        if ($request->hasFile('dicky_image')) {
            $imageName = time() . '.' . $request->file('dicky_image')->getClientOriginalExtension();
            $request->file('dicky_image')->storeAs('public/images', $imageName);
    
            // Update or create the associated Image1 record
            if ($customer->image1) {
                // If an Image1 record exists, update its image path
                $customer->image1->update(['image_path' => 'storage/images/' . $imageName]);
            } else {
                // If no Image1 record exists, create a new one and associate it with the Customer
                $image1 = new Image1(['image_path' => 'storage/images/' . $imageName]);
                $customer->image1()->save($dicky_image);
            }
        }
    
        $customer->update($request->all());
    
        return redirect()->route('customer.index')->with('success', 'Customer updated successfully');
    }
    
  


    public function destroy(Customer $customer)
    {
        $customer->delete();
        return redirect()->route('customer.index')->with('success', 'Customer deleted successfully');
    }
}
