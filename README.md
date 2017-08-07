   # Dependent Dropdown
   ## Example of Select Region After Selecting State
	   public function showFormFunction()
	    {
		$states = State::lists('display_name', 'id');
		$regions = ['Please select state first'];
		return view('dashboard', compact(['states']))
			    ->with('regions', $regions);
	    }
   
   ### HTML Form Code
	   <div class="form-group">
	      {!! Form::label('state_id', 'State : ', array('class' => 'col-md-2 control-label')) !!}
	      <div class="col-md-4">
		  {!! Form::select('state_id', $states ,null, array('class' => 'form-control', 'required', 'id' => 'state_id')) !!}
	      </div>
	   </div>

	   <div class="form-group">
	      {!! Form::label('region_id', 'Region : ', array('class' => 'col-md-2 control-label')) !!}
	      <div class="col-md-4">
		  {!! Form::select('region_id', $regions, null, array('class' => 'form-control', 'required', 'id' => 'region_id')) !!}
	      </div>
	   </div> 

   ### Ajax Javascript Code
    ==============================================================================
    
	     <script type="text/javascript">
		//depandable dropdown start
		$('#state_id').on('change', function(e){

		    var state_id = e.target.value; // get the value of state feild 
		    var apiUri =  "<?php echo URL::route('api.region'); ?>";
		    // AJAX GET Request
		    $.get(apiUri+'?state_id='+state_id, function(data){
			//success data
			$('#region_id').empty();
			$('#region_id').append('<option value=""> Please choose one</option>');
			// populate data
			$.each(data, function(key, value) {
			    var option = $("<option></option>")
				.attr("value", key)
				.text(value);
			    $('#region_id').append(option);
			});

		    });
		});
		//depandable dropdown end
	      </script>
   	==============================================================================
   ### Api route
      	Route::get('api/section-dropdown', array('as' => 'api.region', 'uses' => 'Api\ApiController@sectionDropDownData'));

   ### In ApiController 
	   public function sectionDropDownData()
	    {
		$state_id = \Input::get('state_id');

		if($state_id) {
		    $regions = Region::where('state_id', $state_id)->lists('region_code', 'id');
		    return \Response::json($regions);
		}
	    }
    
    ==============================================================================
    
  ### You are Good to Go. Thanks
    
    
