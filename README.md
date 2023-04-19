
$(document).ready(function() {

    bpath = __basepath + "/";
    List_of_Leave_Type_table();
    load_tbldata_list_of_leave_type('');

});


var  _token = $('meta[name="csrf-token"]').attr('content');

var tbldata_list_of_leave_type;
var tbldata_employee_leave_Details;


function List_of_Leave_Type_table() {

    try{
        /***/
        tbldata_list_of_leave_type = $('#list_of_leave_type').DataTable({
            dom:
                "<'grid grid-cols-12 gap-6 mt-5'<'intro-y col-span-6 text_left_1'l><'intro-y col-span-6 text_left_1'f>>" +
                "<'grid grid-cols-12 gap-6 mt-5'<'intro-y col-span-12'tr>>" +
                "<'grid grid-cols-12 gap-6 mt-5'<'intro-y col-span-12'<'datatable_paging_1'p>>>",
            renderer: 'bootstrap',
            "info": false,
            "bInfo":true,
            "bJQueryUI": true,
            "bProcessing": true,
            "bPaginate" : true,
            "aLengthMenu": [[10,25,50,100,150,200,250,300,-1], [10,25,50,100,150,200,250,300,"All"]],
            "iDisplayLength": 10,
            "aaSorting": [],

            columnDefs:
                [
                    { className: "dt-head-center", targets: [ 5 ] },
                ],
        });



        tbldata_employee_leave_Details = $('#employee_leave_Details').DataTable({
            dom:
                "<'grid grid-cols-12 gap-6 mt-5'<'intro-y col-span-6 text_left_1'l><'intro-y col-span-6 text_left_1'f>>" +
                "<'grid grid-cols-12 gap-6 mt-5'<'intro-y col-span-12'tr>>" +
                "<'grid grid-cols-12 gap-6 mt-5'<'intro-y col-span-12'<'datatable_paging_1'p>>>",
            renderer: 'bootstrap',
            "info": false,
            "bInfo":true,
            "bJQueryUI": true,
            "bProcessing": true,
            "bPaginate" : true,
            "aLengthMenu": [[10,25,50,100,150,200,250,300,-1], [10,25,50,100,150,200,250,300,"All"]],
            "iDisplayLength": 10,
            "aaSorting": [],

            columnDefs:
                [
                    { className: "dt-head-center", targets: [ 5 ] },
                ],
        });


        /***/
    }catch(err){  }
}

//Start load list of leve type datatable

function load_tbldata_list_of_leave_type(id) {

    $.ajax({
        url: bpath + 'load_leave_type',
        type: "POST",
        data: {
            _token: _token,
            id: id,
        },
        success: function(data) {
            tbldata_list_of_leave_type.clear().draw();
            /***/
            var data = JSON.parse(data);
            //console.log( data );
            if(data.length > 0) {
                for(var i=0;i<data.length;i++) {

                    /***/
                    var id = data[i]['id'];
                    var typename = data[i]['typename'];
                    var category = data[i]['category'];
                    var qualifygender = data[i]['qualifygender'];
                    var numberofleaves = data[i]['numberofleaves'];
                    var leave_cat = data[i]['leave_cat'];
                
                    var cd = "";
                    /***/

                    cd = '' +
                        '<tr >' +

                        '<td style="display: none" class="id">' +
                        id+
                        '</td>' +

                        '<td>' +
                        typename+
                        '</td>' +

                        '<td>' +
                        category+
                        '</td>' +

                        '<td>' +
                        qualifygender+
                        '</td>' +

                        '<td>' +
                        numberofleaves+
                        '</td>' +

                        '<td>'+
                        leave_cat+
                        '</td>'+

                        '<td class="w-auto">' +
                        '<div class="flex justify-center items-center">'+
                        ' <div> <button id="'+id+'" class="edit_leave_type_btn" href="javascript:;" data-id="id"  data-tw-toggle="modal" data-tw-target="#edit_leave_type_modal"><i class="fa fa-edit text-warning"></i>&nbsp &nbsp &nbsp '+
                        ' <div> <button id="'+id+'" class="delete_leave_type_btn" href="javascript:;"><i class="fa fa-trash text-danger"></i> '+
                       
                        '</div>'+
                        '</td>' +

                        '</tr>' +
                        '';

                        tbldata_list_of_leave_type.row.add($(cd)).draw();


                    /***/

                }

            }
        }

    });
//End load list of leve type datatable


//Start add leave type 

    $("body").on("click", "#add__leave_type_btn", function () {
  
        var username = $('#username').val(); 
        var typename = $('#add_typename').val();
        var category = $('#add_category').val();
        var qualifygender = $('#add_qualifygender').val();
        var numberofleaves = $('#add_numberofleaves').val();
        var leave_cat = $('#add_leave_category').val();
        var long_name= $('#add_long_name').val();
       
    
        $.ajax({
            url: bpath + 'store_leave_type',
            type: "POST",
            data: {

                _token: _token,
                username:username,
                typename: typename,
                category: category,
                qualifygender:qualifygender,
                numberofleaves:numberofleaves,
                leave_cat:leave_cat,
                long_name:long_name
    
            },
            success: function(data) {
            
                var data = JSON.parse(data);
                
                __notif_load_data(__basepath + "/");
                load_tbldata_list_of_leave_type('');

                const mdl = tailwind.Modal.getOrCreateInstance(document.querySelector("#add_newleave_type_modal"));
                mdl.hide();
    
    
            }
    
            });  
    
    });
//End add leave type 

   
//Edit leave type

    $(document).on('click', '.edit_leave_type_btn', function(e) {
            e.preventDefault();
            let id = $(this).attr('id');
            $.ajax({
            url: 'edit_leave_type/' + id + '/edit',
            method: 'get',
            data: {
            id: id,
            _token: '{{ csrf_token() }}'
         
        },
        
        
          success: function(data) {

            $("#edit_username").val(data.username);
            $("#edi_typename").val(data.typename);
            $("#edit_category").val(data.category);
            $("#edit_qualifygender").val(data.qualifygender);
            $("#edit_numberofleaves").val(data.numberofleaves);
            $("#edit_leave_cat").val(data.leave_cat);
            $("#edit_long_name").val(data.long_name);
            $("#update_leave_type_btn").val("Update");
            
            $('#update_leave_type_Modal').attr('action', '/update_leave_type/' + data.id);

            

        }

        });

    
});
//End Edit leave type

// delete Leave Type 
 
    $(document).on('click', '.delete_leave_type_btn', function(e) {
    e.preventDefault();
    var id = $(this).attr('id');
    var csrf = '{{ csrf_token() }}';
    Swal.fire({
        
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        type: 'info',
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      confirmButtonText: 'Yes, delete it!',

    }).then((result) => {
     
      if (result.value==true) {
        $.ajax({
          url: 'delete_leave_type/' + id + '/delete',
          method: 'get',
          data: {
            id: id,
            _token: csrf
          },
          success: function(response) {
            console.log(response);
            Swal.fire(
              'Deleted!',
              'Your file has been deleted.',
              'error'
            )

            __notif_load_data(__basepath + "/");

            load_tbldata_list_of_leave_type('');

          }
        });
      }
    })
  });

 // End delete Leave Type 


 
//start load employee leave details

     function load_employee_leave_details(id) {

    $.ajax({
        url: bpath + 'load_leave_type',
        type: "POST",
        data: {
            _token: _token,
            id: id,
        },
        success: function(data) {
            tbldata_list_of_leave_type.clear().draw();
            /***/
            var data = JSON.parse(data);
            //console.log( data );
            if(data.length > 0) {
                for(var i=0;i<data.length;i++) {

                    /***/
                    var id = data[i]['id'];
                    var typename = data[i]['typename'];
                    var category = data[i]['category'];
                    var qualifygender = data[i]['qualifygender'];
                    var numberofleaves = data[i]['numberofleaves'];
                    var leave_cat = data[i]['leave_cat'];
                
                    var cd = "";
                    /***/

                    cd = '' +
                        '<tr >' +

                        '<td style="display: none" class="id">' +
                        id+
                        '</td>' +

                        '<td>' +
                        typename+
                        '</td>' +

                        '<td>' +
                        category+
                        '</td>' +

                        '<td>' +
                        qualifygender+
                        '</td>' +

                        '<td>' +
                        numberofleaves+
                        '</td>' +

                        '<td>'+
                        leave_cat+
                        '</td>'+

                        '<td class="w-auto">' +
                        '<div class="flex justify-center items-center">'+
                        ' <div> <button id="'+id+'" class="edit_leave_type_btn" href="javascript:;" data-id="id"  data-tw-toggle="modal" data-tw-target="#edit_leave_type_modal"><i class="fa fa-edit"></i> '+
                        '</div>'+
                        '</td>' +

                        '</tr>' +
                        '';

                        tbldata_list_of_leave_type.row.add($(cd)).draw();


                    /***/

                }

            }
        }

    });

    }   
//End load employee leave details


      
}

