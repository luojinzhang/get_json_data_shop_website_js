# get_json_data_shop_website_js
Get json data from shop website

    var Ten_TV = document.getElementById("prod_title").textContent.trim();
    var Loai_TV = $("#prod_brand > div > a > span").text();
    var Don_gia = document.getElementById("special_price_box").textContent;
    var So_luong = document.getElementById("product-option-stock-number").textContent.trim() == "" ? "100" : "0";

    var a = $(".specification-table > tbody > tr")
    var Thong_so_ky_thuat = {
      Mau: "",
        Cong_ket_noi: "",
        Do_phan_giai: "",
        Kich_thuoc: "",
        Model: "",
        Trong_luong: "",
        Xuat_xu: "",
        Bao_hanh: ""};
    for (var i = 0; i < a.length; i++)
    {
      if (a[i].children[0].textContent.includes("Màu"))
        {
        Thong_so_ky_thuat.Mau = a[i].children[1].textContent;
        }

      if(a[i].children[0].textContent.includes("Cổng kết nối"))
      {
        Thong_so_ky_thuat.Cong_ket_noi = a[i].children[1].textContent;
      }

      if(a[i].children[0].textContent.includes("Độ phân giải màn hình"))
      {
        Thong_so_ky_thuat.Do_phan_giai = a[i].children[1].textContent;
      }

      if(a[i].children[0].textContent.includes("Kích thước màn hình"))
      {
        Thong_so_ky_thuat.Kich_thuoc = a[i].children[1].textContent;
      }

      if(a[i].children[0].textContent.includes("Mẫu mã"))
      {
        Thong_so_ky_thuat.Model = a[i].children[1].textContent;
      }

      if(a[i].children[0].textContent.includes("Trọng lượng"))
      {
        Thong_so_ky_thuat.Trong_luong = a[i].children[1].textContent;
      }

      if(a[i].children[0].textContent.includes("Sản xuất tại"))
      {
        Thong_so_ky_thuat.Xuat_xu = a[i].children[1].textContent;
      }

      if(a[i].children[0].textContent.includes("Thời gian bảo hành"))
      {
        Thong_so_ky_thuat.Bao_hanh = a[i].children[1].textContent;
      }
    }
    TV = {
      Ma_TV : "DARLING_01",
      Ten_TV: Ten_TV,
      Loai_TV: Loai_TV,
      Don_gia: Don_gia,
      So_luong: So_luong,
      Thong_so_ky_thuat: Thong_so_ky_thuat
    }


    var saveData = (function () {
        var a = document.createElement("a");
        document.body.appendChild(a);
        a.style = "display: none";
        return function (data, fileName) {
            var json = JSON.stringify(data, null, 1),
                blob = new Blob([json], {type: "octet/stream"}),
                url = window.URL.createObjectURL(blob);
            a.href = url;
            a.download = fileName;
            a.click();
            window.URL.revokeObjectURL(url);
        };
    }());

        fileName = TV.Ma_TV + ".json";

    saveData(TV, fileName);
