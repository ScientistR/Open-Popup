Step 1 Create Controller For Popup
[HttpGet]
 public async Task<IActionResult> GetPurchaserList()
 {
     List<ClientIndividualModel> purchaserList = new List<ClientIndividualModel>();
     int PFID = 0;
     if (!string.IsNullOrEmpty(HttpContext.Session.GetString(AppConstants.PFID)))
     {
         PFID = Convert.ToInt32(HttpContext.Session.GetString(AppConstants.PFID).ToString(), null);
     }
     using (var response = await Client.GetAsync(APIUrls.GetPurchaserList + AppConstants.QuestionMarrkPFIDEqual + PFID).ConfigureAwait(false))
     {
         string apiResponse = await response.Content.ReadAsStringAsync().ConfigureAwait(false);
         purchaserList = JsonConvert.DeserializeObject<List<ClientIndividualModel>>(apiResponse.ToString());
     }
     return PartialView("_Purchaserlist", purchaserList);
 }
Step 2 Create Partial Page(_Purchaserlist)
@model IEnumerable<ClearwaterNorth_LGLA.MODEL.ClientIndividualModel>
@{
    ViewData["Title"] = "FeeQuote";
}
<div class="modal-dialog modal-lg modal-dialog-centered">
    <!-- Modal content-->
    <div class="modal-content">
        <!-- <div class="modal-header"> -->
        <div class="modalHeaderFlex law-clerk-modal-header sticky-top">
            <div class="cstmHeader">
                <h5 class="modal-title dash-heading" id="exampleModalLabel1">Buyer List</h5>
            </div>
            <div class="closeModalBox" data-bs-dismiss="modal" aria-label="Close">
                <i class="fa-solid fa-xmark"></i>
            </div>
        </div>
        <!--modal closed-->
        <div class="modal-body p-0 modalwithDataTable">
            <div class="modal-Padding">
                <div class="datatable-main">
                    <div class="row">
                        <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12 col-12">
                            <div class="datatable-section">
                                <table id="Purchasertbl" class="CUSTOM-DATA table table-striped table-hover nowrap" style="width: 100%;">
                                    <thead>
                                        <tr>
                                            <th hidden>Gender</th>
                                            <th>First Name</th>
                                            <th>Middle Name</th>
                                            <th>Surname</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        @if (Model != null)
                                        {
                                            @foreach (var item in Model)
                                            {
                                                <tr style="cursor:pointer">
                                                    <td hidden>@item.Gender</td>
                                                    <td>@item.FirstName</td>
                                                    <td>@item.MiddleName</td>
                                                    <td>@item.Surname</td>
                                                </tr>
                                            }
                                        }
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!--data table end-->
        </div>
        <div class="d-flex flex-wrap justify-content-end pt-0 pb-2 px-2 modal-footer">
            <div class="select-btn pt-2">
                <button type="button" class="cancel me-2">
                    Close
                </button>
            </div>
        </div>
    </div>
</div>

<!--dashboard content end-->
<!--content wrapper end-->


Step 3 Create Jquery Function 
$(document).on("click", "#btnListPurchaserModal", function () {
    var url = "/Offeror/GetPurchaserList"
    $(".purchaserlistPopup").load(url, function () {
        $(".purchaserlistPopup").modal("show");      
        $('#Purchasertbl').DataTable();
    });
});  


Step 4 : Put code in Index Page
<div id="purchaserlist" class="modal purchaserlistPopup" role="dialog" tabindex="-1" aria-labelledby="gridSystemModalLabel" data-bs-backdrop="static" data-bs-keyboard="false">
</div>

